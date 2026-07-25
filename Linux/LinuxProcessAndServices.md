## Linux Processes
A process is a program in execution, the program is stored onto the file. Every process have a parent process, identifiers like `PID` `PPID` through they can be allocated with resources such as memory, disk etc.  In linux the the process' program executables are just command file following properties such as ownership(user), privileges, limits to resources. Processes can be controlled and can be communicated with signals, system calls, pipes and sockets. Every process is invoked explicitly except init. A processes may have child processes which can share same data as their parent.  
##### Related Commands 
```bash
# process listing using ps 

ps [OPTIONS/FLAGS] [ARGUMENTS]
  -a # To list current process 
  -U $USERNAME # option to see user's initiated process
  -C $COMMAND # option to list process initiated with provided comand
  --sort=%X # option to sort the process with specifics
  -o $X # option to format the lists with specifics like pid,user etc
  -t $TTY_SESSION_ID # option to list process within the terminal session id
  -e # flag to list all the processes (usually outputs all daemons)
  -f # flag to list processes with info like date pid etc.
  --forest # flag to list processes with tree like structure
```

```bash
# process finding via pgrep 

pgrep [OPTIONS/FLAGS] [ARGUMENTS] PROCESSNAME 
  -u $USERNAME # option to see users initiated process 
  -c $COMMAND # option to list process initiated with provided comand 
  -P $PID # option to see sepecific process child processes 
  -l # flag to list with process id and name 
  -i # flag to pass case insensitiveness 
  -v # flag to invert results 
```
### `Signals` 
Signals are just events they are not process, a process detect the signal and act accordingly.
These signals are stored in the process control block, A signal is created the passed to dedicated process' control block queue all done by the kernel. 
#### Some signals include 
| Signal  | Number | Default Action                       | Purpose / Usage                                |
| ------- | ------ | ------------------------------------ | ---------------------------------------------- |
| SIGTERM | 15     | Terminate                            | Graceful termination of a process.             |
| SIGKILL | 9      | Terminate (cannot be caught/ignored) | Forcefully kill a process.                     |
| SIGSTOP | 19     | Stop (cannot be caught/ignored)      | Pause/suspend a process.                       |
| SIGTSTP | 20     | Stop                                 | Terminal stop (Ctrl+Z), can be caught/handled. |
| SIGCONT | 18     | Continue execution                   | Resume a stopped process.                      |
| SIGHUP  | 1      | Terminate                            | Reload configuration or restart daemon.        |
| SIGINT  | 2      | Terminate                            | Interrupt from keyboard (Ctrl+C).              |
| SIGQUIT | 3      | Core dump + Terminate                | Quit and dump core (optional for debugging).   |
##### Related Commands 
```bash
# Create the signal for processes via kill 

kill [OPTIONS/FLAGS] [ARGUMENTS] -[PID]
  -l # flag to list all the available signals 
  -s $SIGNAL/SIGNAL_NUMBER # option to pass a specific signal to process 
  -p $PID # option to avoid sending a signal to particular process 
  
killall $PROCESS # option to kill multiple command at once 

```
### `System calls` 
A **system call** is a mechanism by which a **user-space program requests a service from the operating system (OS) kernel**. User programs cannot directly access hardware or critical OS functions (like file I/O, process control, or memory management), so they must go through **system calls** to safely interact with the kernel.
#### Some system calls
| System Call | Work It Does                                                     |
| ----------- | ---------------------------------------------------------------- |
| fork()      | Creates a new process by duplicating the calling process.        |
| exec()      | Replaces the current process image with a new program.           |
| exit()      | Terminates the calling process and returns a status to parent.   |
| wait()      | Waits for a child process to terminate and retrieves its status. |
| getpid()    | Returns the process ID of the calling process.                   |
| open()      | Opens a file and returns a file descriptor.                      |
| read()      | Reads data from a file descriptor into a buffer.                 |
| write()     | Writes data from a buffer to a file descriptor.                  |
| close()     | Closes an open file descriptor.                                  |
| kill()      | Sends a signal to a process or group of processes.               |
| signal()    | Sets a handler for a specific signal.                            |
| mmap()      | Maps files or devices into memory.                               |
##### Related Commands
```bash
# Find system calls used with the help of strace 

strace [OPTIONS/FLAGS] [COMMAND/PID]
  -p $PID # option to attach to a running process
  -o $FILE # option to save output to a file
  -e trace=<SYSCALLS> # option to trace specific system calls
  -f # flag to follow child processes
  -c # flag to count calls and time per syscall
  -s <BYTES> # option to set max string length to display
  -tt # flag to show timestamps with microseconds
  -T # flag to show time spent in each syscall
  -r # flag to show time since previous syscall
  -e trace=network # option to trace only network syscalls
  -e fault=<SYSCALL> # option to simulate faults in specific syscalls
```

## Linux Services
A service is a background process which runs continuously under the hood to provide for other processes. Usually init process manages all the background services and can be used to interact with them. 
### Systemd 
Systemd is one of the init system found at most of the distributions, it have a dedicated cli `systemctl` to interact with background process `daemons` `service` to interact with.
Since service is a process its program files can be found at 
#### Service file locations 
- `/usr/lib/systemd/system/` (RHEL) or `/lib/systemd/system/` (Debian) services stored in this location have the highest priority and are provided by the operating system
- `/etc/systemd/system` these have the lower priority as compare to the lib ones these are mainly used for modifying certain behaviour of the services without changing the orignal files
- `/usr/lib/systemd/user/` (RHEL) or `/lib/systemd/user/` (Debian) service files stored on these location are specific to user environment
#### Service file structure 
A service file is divided into three major parts 
##### `[Unit]` 
A unit section can be defined as a set of meta data for the service and dependencies with meta behaviour. These include 
- Description=A short description of the service
- Documentation=url or path to service documentation
- After=units that should run before this service
- Before=Units that must run after this service
- Requires=List of unit files that this service cannot run without
- Wants=similar to Requires but failure of unit files will not result in failure of the Service
- Conflicts=List of unit files that must not start at the same time else it will cause clashes
##### `[Service]` 
Service section defines the workflow and working of the service like which command to execute continuously, how it behaved when the explicit instructions are passed etc. 
- Type=Tells how a process will start cause at the end of the day services are just processes
- ExecStartPre=command to run before the service start
- ExecStratPro=command to run after the start of the service
- ExecStart=command to start the service
- ExecStopPre=command to run before stopping the service
- ExecStop=command to stop the service
- ExecStopPro=command to run after the service stops
- WorkingDirectory=Directory where the command is executed
- Environment=Sets the environment variable for the service Restart=vale when to restart again possible values -> always, on-failure
- RestartSec=delay in seconds to restart the service again ExecReload=command to reload the values for the service
- StandardOutput=file path in which the output is to be generated StandardError=file path in which the errors are stored
##### `[Install]` 
Install deals with service enabling and loading instructions specific to run levels. 
- WantedBy=Target linked to enable the service
- RequiredBy=Service required target 
##### Related Commands
```bash
# Systemctl to interact with systemd

systemctl [OPTIONS/FLAGS] [ARGUMENTS] [SERVICENAME]
  status $SERVICE-NAME # option to check the status of the service 
  stop $SERVICE-NAME # option to stop service will (will start after reboot) 
  start $SERVICE-NAME # option to start the service (depends on reboot)
  restart $SERVICE-NAME # option to restart the service 
  enable $SERVICE-NAME # option to enable service to start at boot 
  disable $SERVICE-NAME # option to disable service  
  reload $SERVICE-NAME #option to reload service with updated configurations  
  daemon-reexec | daemon-reload # To reload 
  list-dependencies $SERVICE-NAME # option to list dependent services 
  cat $SERVICE-NAME # option to output current service configuration  
  edit $SERVICE-NAME # option to edit the main configuration file 
  --now # sub-flag to do it at moment 
```
