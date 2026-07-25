## Linux Streams and pipes
Streams can be defined as the mechanism for inter process data share. Within streams data flow from source to target. Since data is stored on the file, files are used to represent streams. Processes can refer file with the help of file descriptor: A file descriptor is an positive integer used to refer a file.  

Pipes are data channel reserved data buffer in the memory space managed by the kernel which are used for passing output of data from one to other process. They do not have a name just a `|` The flow of data is linear. 
### Standard Streams Types
Standard streams mapped to reserved FD are stored in FD table managed by the kernel.  There are three standard streams, these streams are shared by all the processes in the system.
#### Stdin 
All the default input passes via stdin, represented by file descriptor `0` its file can be found at `/dev/stdin` Stream can be redirected from pipe, block device or file. The stream gets open and assigned to process on the time of creation, it remains open till process terminates or a signal is passed explicitly, at that time if a child process is spawned the file descriptor is shared.  Default target is keyboard / input devices.  
#### Stdout
All the default output passes via stdout, represented by file descriptor `1` file can be found at `/dev/stdout` which opens only in write only mode, just like `IS` it can be redirected and is shared by child processes. 
#### Stderr
All the default error messages and diagnostics pass via `stderr`, represented by file descriptor `2`. Its file can be found at `/dev/stderr`. The stream is opened in **write-only mode** at the time of process creation, remains open until the process terminates, or is explicitly closed by the program or due to a signal. Unlike `stdout`, it is **unbuffered** (or line-buffered in some environments), ensuring that errors are written immediately without waiting for buffer flush. The stream can also be redirected to files, pipes, or devices, and is inherited by child processes. Default target is the terminal display (screen), separate from `stdout`, so normal output and error output remain distinguishable unless explicitly redirected.
##### Related Commands 
```bash 
# IOE redirections 

COMMAND > FILPATH         # redirects the output to provided file 
COMMAND 2> FILPATH        # redirects the errors to provided file 
COMMAND < FILPATH         # provides files as an input 
```

```bash 
# Redirections inbetween streams 

COMMAND > FILPATH d1>&d2  # redirects d1 to d2   
```

```bash
# Assigninf and revoking file descriptor 

exec N> FILPATH          # assign file descriptor with value N to provided file 
COMMAND OTHER-FD&N       # using assigned file descriptor
exec N>&-                # revoking file descriptor 
```

## `Regex` 
Regex is a mechanism by with with the help of symbols a series of text patterns can be generalised. 
##### Character Expressions 
| Pattern       | Meaning                                                            |
| ------------- | ------------------------------------------------------------------ |
| `^CHARS`      | Matches the **start** of a string (anchored at beginning).         |
| `CHARS$`      | Matches the **end** of a string (anchored at end).                 |
| `.`           | Matches **any single character** (except newline by default).      |
| `*`           | Matches **0 or more** occurrences of the preceding element.        |
| `+`           | Matches **1 or more** occurrences of the preceding element.        |
| `?`           | Matches **0 or 1** occurrence of the preceding element (optional). |
| `{n}`         | Matches **exactly `n`** occurrences.                               |
| `{n,}`        | Matches **at least `n`** occurrences.                              |
| `{n,m}`       | Matches **between `n` and `m`** occurrences.                       |
| `[]`          | Matches **any one character** from the class inside brackets.      |
| `[0-9]`       | Matches any **digit** from `0` to `9`.                             |
| `[a-z]`       | Matches any **lowercase letter** from `a` to `z`.                  |
| `[a-zA-Z0-9]` | Matches any **alphanumeric character**.                            |
| `[abc]`       | Matches any **one of `a`, `b`, or `c`**.                           |
| `[^abc]`      | Matches any character **except** `a`, `b`, or `c`.                 |
## Linux Data Handling Utils 
```bash
# sed is the utility to edit/manipulate data across data stream

sed [OPTIONS/FLAGS] [ARGUMENTS] $DATA/FILE  
  -n # flag to suppress automatic printing 
  -e # flag to add inline script command  
  -f # flag to take sed script from file  
  -r / -E # flag to enable extended regex (`-r` GNU, `-E` BSD/macOS)  
  -i[SUFFIX] # flag for in-place editing (optional backup suffix)  
    s/pattern/replacement/ # command to substitute first match in line  
    s/pattern/replacement/g # command to substitute all matches in line  
    s/pattern/replacement/n # command to substitute only nth match in line  
    d # command to delete the line  
    p # command to print the line (used with `-n`)
```

```bash
# cut is the utility to extract sections of data stream

cut [OPTIONS/FLAGS] [ARGUMENTS] $DATA/FILE  
  -c LIST # flag to select characters by position (example: `-c1-5`)  
  -f LIST # flag to select fields (requires delimiter)  
  -d DELIM # flag to specify delimiter (default is TAB)  
  --complement # flag to invert selection (exclude specified chars/fields)  
  -s # flag to suppress lines without delimiters
```

```bash
# Grep is the utility to seach data across data stream 

grep [OPTIONS/FLAGS] [ARGUMENTS] $SEARCHING-DATA $DATA/FILE  
  -v # flag to invert results 
  -i # flag to ignore case
  -o # flag to search only resultant
  -w # flag to extract only words 
  -E # flag for extended regex 
  -BN # option to print the N number of lines before the results
  -AN # option to print the N number of lines after the results
```

```bash
# AWK is the utility to filter, transform, and extract data fields from text streams

awk [OPTIONS/FLAGS] 'pattern { action }' [FILE/STREAM]
  -F<sep> # option to set input field separator (default is whitespace)
  -v var=val # option to pass variable into awk program
  -f script # option to run awk program from file
  --posix   # flag to enforce POSIX compatibility

# Common built-in variables
  NR   # current record (line) number
  NF   # number of fields in current record
  FS   # input field separator
  OFS  # output field separator
  RS   # input record separator
  ORS  # output record separator
  $0   # entire current line
  $N   # Nth field in current line (e.g., $1 = first field)

# Common patterns/actions
  'pattern'  # print lines matching regex
  '{ print $1, $2 }' # print specific fields
  'NR==1' # first line only
  'NR>1' # skip header line
  'NF>0' # skip empty lines
  '{ sum+=$1 } END { print sum }'  # sum first column
  '{ count[$1]++ } END { for (i in count) print i, count[i] }' # frequency count

```

```bash
# To get file content 
cat $FILE   

# To get  file's top entry 
head [OPTIONS/FLAGS] FILES   
  -n $N # option to print n number of lines from top 

# To get  file's bottom entry 
tail [OPTIONS/FLAGS] FILES   
  -n $N # option to print n number of lines from bottom 

# To get ifile's  entry from bottom to top 
tac FILE   
```
