## Linux Package management 
Package refers to a compressed archive of files that make up a software application, along with metadata describing the package, like version, dependencies, installation instructions, and more. Packages are managed by **package managers**, which automate installation, upgrading, configuration, and removal.

The package is dependent of the aspects such as architecture of the system, distribution. 
### Low level package management 
At local level the package files require are present at the local this can be managed locally. 
##### Related Commands 
```bash 
# dpkg for managing the low level packages on the debian based systems 

dpkg [OPTIONS/FLAGS] [ARGUMENTS] [PACKAGE]
  -i # flag for installation 
  -r # flag for removal 
  -l # flag to list packages 
  -s # flag for checking details 
```

```bash 
# rpm for managing the low level packages on the rh based systems 

rpm [OPTIONS/FLAGS] [ARGUMENTS] [PACKAGE]
  -i # flag for installation 
  -e # flag for removal 
  -U # flag for upgrade
```
### High level package management
In high level package management a remote repository is set to store set of packages , files and related data. If the file does not exist on the local files are pulled from remote. Done by high level package manager. These repository have to be declared in the configuration. 
##### Related Commands 
```
# file configuration at /etc/yum.repos.d/FILE.repo entry 

[custom-repo]
name=Custom Repository
baseurl=http://example.com/path/to/repo/
enabled=1
gpgcheck=1
gpgkey=http://example.com/path/to/GPG-KEY
```

```bash
# yum a high level package manager for rh based systems 

yum [OPTIONS/FLAGS] [ARGUMENTS] package
  install UTILS/PACKAGES # option to install utilities and manage pkg files 
  remove UTILS/PACKAGES # option to install utilities and manage pkg files
  provides UTIL # option to check for the repo and packages associated with util
  repolist # flag to list all the repos
  update # flag to refresh metadata and apply local updates/changes if any 
  upgrade # flag to upgrade old versions with newer ones including dependencies 
```

```
# file configuration at /etc/apt/sources.list.d/custom.list

deb [arch=amd64] http://example.com/path/to/repo/ stable main
deb-src http://example.com/path/to/repo/ stable main
```

```bash
# apt 

apt [OPTIONS/FLAGS] [ARGUMENTS] package
  install UTILS/PACKAGES # option to install utilities with pkg files
  remove UTILS/PACKAGES # option to install utilities and with pkg files
  list # flag to list all the repos
  update # flag to refresh metadata and apply local updates/changes if any 
  upgrade # flag to upgrade old versions with newer ones including dependencies 
```
## Scheduling tasks

##### Related Commands
```bash
# cron a utility to schedule tasks cron.daily crom.hourly 

The first five value represent time  6 represent user 7 command 
  first  - minute (0-59)
  second  - hour(0-23)
  third - day of month (1-31) mon tue wed thu fri sat sun 
  fourth  - month (1-12) jan feb mar apr 
  fifth  - day of the week (0-6) where 0 is sunday 

 * represent every value 
 , can be used to add multipple time 6,9 
 - can be used to specify range 6-9 the job will run on 6 7 8 9 
 the basic step is one unit /NUMBER can be used to add custom steps ex 6-9/2 
```

```bash
# anacron 

/etc/anacrontab
  first - Period (@daily @weekly @monthly)
  delay - in minutes how much time the job will wait to execute after boot 
  identifier - any name given to task which would be used for logs 
  command  - full path of command 
```
