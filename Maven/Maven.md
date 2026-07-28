A build automation and project management tool for Java based applications. 
## Architecture
package directory structure. It contains `META-INF/MANIFEST.MF`, which can define attributes like `Main-Class`. The `jar` tool packages `javac` output into this archive. At runtime, `java -jar` reads the manifest, builds a classloader over the archive, and loads `.class` files directly from the ZIP without extracting the whole archive to disk. A JAR is a packaging format for bytecode and resources, not an executable format itself.

Manually installing correct versions of dependent JARs and resolving packaging conflicts is tedious and error-prone. Maven solves this declaratively through `pom.xml`. It behaves like a package manager: if a dependency isn't present locally, it fetches it from a remote repository, and if a new package is built, it can be pushed to a remote registry, with plugins handling automation.

At local all pulled or built packages, along with settings and configs, are stored in `~/.m2`. For example, `~/.m2/repository/org/slf4j/slf4j-api/1.5.6/` contains the actual JAR, its POM, and checksums.

Remote repositories (e.g., Maven Central, Nexus, Artifactory) mirror this exact same `groupId/artifactId/version` directory layout, just served over HTTP instead of sitting on local disk. So `org.slf4j:slf4j-api:1.5.6` resolves to a path like `org/slf4j/slf4j-api/1.5.6/` on the remote server, containing the same JAR, POM, and checksum files. Maven constructs the URL from the coordinates in `pom.xml`, downloads the artifact over HTTP(S), verifies it against the checksum, and caches it into the identical path under `~/.m2/repository` so local and remote share one addressing scheme, just different hosts.
## POM 
[Official pom docs ](https://maven.apache.org/ref/3.9.16/maven-model/maven.html) 
### Usuals
##### Minimal pom.xml
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- groupId+artifactId+version = the "GAV" coordinates that
         uniquely identify this project everywhere (local cache,
         remote repo, and as a dependency in other poms) -->
    <groupId>com.example</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>My Project</name>
    <description> A simple Maven project demonstrating basic POM metadata. </description> 

    <!-- what this project builds into: jar, war, pom, etc. -->
    <packaging>jar</packaging>
</project>
````
##### Compiling
```xml
    <!-- controls which javac source/target language level is used,
         and the encoding used when reading source files -->
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
```
##### Publishing
```xml
<!-- tells `mvn deploy` where to upload this project's built artifact -->
<distributionManagement>
    <repository>
        <id>YOUR_REPOSITORY_ID</id>
        <name>YOUR_REPOSITORY_NAME</name>
        <url>https://your-registry-url</url>
    </repository>
</distributionManagement>
```

```xml
<!-- ~/.m2/settings.xml -->
<!-- credentials for the repository above, matched by <id>.
     kept out of pom.xml so secrets aren't committed to source control -->
<settings>
    <servers>
        <server>
            <id>YOUR_REPOSITORY_ID</id>
            <username>YOUR_USERNAME</username>
            <password>YOUR_PASSWORD</password>
        </server>
    </servers>
</settings>
```
##### Dependencies 
```xml
<dependencies>
	<dependency>
	    <groupId>...</groupId>
	    <artifactId>...</artifactId>
	    <version>...</version>

	    <!-- artifact format Maven should look for; defaults to jar -->
	    <type>jar | war | pom | ejb | test-jar</type>

	    <!-- disambiguates variants of the same GAV, e.g. "sources" or "javadoc" -->
	    <classifier>...</classifier>

	    <!-- controls which classpaths (compile/test/runtime) this dependency
	         is added to, and whether it's packaged into the final artifact -->
	    <scope>compile | provided | runtime | test | system | import</scope>

	    <!-- only used with scope=system: a hardcoded local jar path instead
	         of resolving from a repository (generally avoid this) -->
	    <systemPath>absolute/path/to/file</systemPath>

	    <!-- if true, this dependency isn't pulled in transitively by
	         projects that depend on this one -->
	    <optional>true | false</optional>

	    <!-- blocks specific transitive dependencies from being pulled in,
	         useful for avoiding version conflicts or unwanted libs -->
	    <exclusions>
	        <exclusion>
	            <groupId>...</groupId>
	            <artifactId>...</artifactId>
	        </exclusion>
	    </exclusions>
	</dependency>
</dependencies>
```
##### Developers
```xml
<!-- metadata only — has no effect on the build, just documents who
     maintains the project (shows up in generated site docs, etc.) -->
<developers>
    <developer>
        <id>developer-id</id>
        <name>Developer Name</name>
        <email>developer@example.com</email>
        <url>https://example.com</url>
        <organization>Organization Name</organization>
        <organizationUrl>https://organization.example.com</organizationUrl>
        <roles>
            <role>developer</role>
            <role>maintainer</role>
        </roles>
        <timezone>UTC+0</timezone>
        <properties>
            <propertyName>propertyValue</propertyName>
        </properties>
    </developer>
</developers>
```
##### Plugins
```xml
<!-- plugins are where the actual build logic lives — compiling, testing,
     packaging, deploying are all plugin goals bound to lifecycle phases -->
<build>
    <plugins>
        <plugin>
            <groupId>...</groupId>
            <artifactId>...</artifactId>
            <version>...</version>
            <configuration>
                <!-- Plugin-specific configuration -->
                <option>...</option>
                <option>...</option>
            </configuration>
        </plugin>
    </plugins>
</build>
```
#### Parent pom with children
##### Parent pom.xml
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>my-parent</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!-- packaging=pom: this project produces no jar of its own,
         it only exists to hold shared config and list child modules -->
    <packaging>pom</packaging>

    <!-- lists subdirectories Maven should build. Maven also resolves
         inter-module dependencies and topologically sorts the build order
         (this ordered multi-module build is called "the reactor") -->
    <modules>
        <module>module-a</module>
        <module>module-b</module>
    </modules>

    <!-- inherited as-is by every child, no need to redeclare -->
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <!-- dependencyManagement only PINS a version, it does NOT add the
         dependency to any child's classpath. A child must still declare
         the dependency itself (without a version) to actually use it -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-api</artifactId>
                <version>1.5.6</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <!-- same idea as dependencyManagement, but for plugins: pin the
             version once here, children opt in under <plugins> without
             repeating <version> -->
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.13.0</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```
##### Child pom.xml (e.g. module-a/pom.xml
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- points back up to the parent. relativePath defaults to ../pom.xml
         if omitted, but it's shown explicitly here. Inherits the parent's
         groupId, version, properties, dependencyManagement and
         pluginManagement -->
    <parent>
        <groupId>com.example</groupId>
        <artifactId>my-parent</artifactId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>

    <!-- groupId/version inherited from parent, only artifactId is unique here -->
    <artifactId>module-a</artifactId>
    <packaging>jar</packaging>

    <dependencies>
        <!-- no <version> needed here — it's inherited from the parent's
             dependencyManagement block above -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
        </dependency>

        <!-- depending on a sibling module works like any other Maven
             coordinate. Maven's reactor figures out module-a needs
             module-b built first -->
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>module-b</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
</project>
```
##### Directory layout
```
my-parent/
├── pom.xml          (packaging=pom, lists <modules>)
├── module-a/
│   ├── pom.xml       (packaging=jar, <parent> points up)
│   └── src/
└── module-b/
    ├── pom.xml
    └── src/
```
## MVN
```bash
# Core lifecycle phases: each phase runs all preceding phases in order

mvn validate    # phase to check project structure and pom.xml is correct
mvn compile     # phase to compile source code (src/main/java -> target/classes)
mvn test        # phase to run unit tests via a test framework (e.g. Surefire)
mvn package     # phase to bundle compiled code into distributable format (jar/war)
mvn verify      # phase to run integration tests and checks on the package
mvn install     # phase to copy the package into local repo (~/.m2), makes it usable by other local projects
mvn deploy      # phase to copy the package to a remote repository (see distributionManagement)
```

```bash

mvn [OPTIONS/FLAGS] [PHASE/GOAL...]
  -f FILE          # flag to specify an alternate pom.xml path
  -pl MODULE       # flag to build only the specified module(s) (comma separated)
  -am              # flag to also build modules that the -pl module(s) depend on
  -amd             # flag to also build modules that depend on the -pl module(s)
  -N               # flag to build only the parent (non-recursive, skip modules)
  -P PROFILE       # flag to activate a build profile defined in pom.xml
  -D PROP=VALUE    # flag to set/override a property or plugin parameter
  -T N             # flag to build with N threads in parallel (e.g. -T 4, -T 1C)
  -o               # flag for offline mode, skip remote repository checks
  -U               # flag to force-update snapshots/releases from remote repo
  -q               # flag for quiet output, only show errors
  -X               # flag for debug/verbose output
  -e               # flag to show full stack traces on error
  -s FILE          # flag to use an alternate settings.xml
  -v               # flag to print Maven version and exit
```

```bash
# Cleaning and inspecting
mvn clean            # deletes the target/ directory (build output)
mvn clean install    # common combo: wipe old build output, then rebuild + install fresh
mvn dependency:tree  # prints the full resolved dependency tree, including transitive deps
mvn dependency:analyze  # reports unused declared deps and used undeclared deps
mvn help:effective-pom  # prints the fully resolved pom, after parent inheritance + profiles are applied
mvn versions:display-dependency-updates  # lists newer available versions for current dependencies
```

```bash
# Running specific tests / skipping steps

mvn test -Dtest=ClassName          # runs only the specified test class
mvn test -Dtest=ClassName#method   # runs only the specified test method
mvn install -DskipTests            # skips running tests, but still compiles them
mvn install -Dmaven.test.skip=true # skips both compiling and running tests
```

```bash
# Multi-module (reactor) specific usage

mvn install                         # builds all modules listed in parent's <modules>, in dependency order
mvn install -pl module-a            # builds only module-a
mvn install -pl module-a -am        # builds module-a and anything it depends on first
mvn install -pl module-a -amd       # builds module-a and anything that depends on it
mvn install -rf module-b            # resumes a multi-module build starting from module-b (useful after a failure)
```