## Linux Accounts
An account in linux can be defined as the linux resource accessor / user. This includes processes with account footprint , resources with account footprint etc. Account have permissions aka level of operations one can perform on the resources. These defaults are declared in `/etc/default/useradd`, the entries of the user can be found at `/etc/passwd` 
where the entry `user:x:501:501::/home/user:/bin/bash`
### Account properties
An account created have properties to provide controlled properties and apply functionalities. 
#### Username 
A username is juts a alphanumeric unique identifier for the account.
#### User id
A user id a unique numeric identifier to account, it is equivalently maps to username.
#### Password 
Password for user authentication and to cap specific user access control. The password entries can be found at `/etc/passwd` where entry looks like `user:!!:20317:0:99999:7:::` 
#### Shell 
A Shell is the command interpreter which executes user commands. The environment variables for the users can be found in `~/.profile` which is read by login shells. `~/.bashrc` which is read by interactive non login shells, `~/.bash_profile` read whenever user session starts.  All the available shell list can be found at `/etc/shells` 
#### Home directory
Home directory is the dir where user logs in, where the file and resources specific to user reside in.  When a user is created the files are copied form `/etc/skel` to user's home directory.
#### Resource Limits
Resource limits are cap on the user resource usage like memory, cpu cores disk etc.  This default configuration can be found at`/etc/security/limits.conf`
#### Primary and Secondary Groups 
A user can be the part of the group (A shared set of privileges shared by set of users).  LinuxGroup
### Account types 
The account can be divided into many categories but the thing is linux treat every account as a account with privileges. “an account is an account”. 
#### Root (root | sudo | su )
Root is the first account in the user space matter fact the first process in user space (init) was started by root. Root have all the privileges the user space has to offer an account can mimic root with the help of sudo utility.  Root account have a UID of `0` located at home dir: `/root` 
#### User 
A user account is an account with privileges on resources, this include `read` `write` `execute` `operation` `resource limits` etc.  Regular user account have a UID ranged in `1000+` and located at home dir: `/home/$USERNAME` User accounts have a expiration time in terms of password, password change, account lifeline. 
#### System 
System Accounts the ones that are tasked with carrying out system operations usually they have no login shell and group assigned they are created automatically by the system. Service Accounts that are created for the services, background processes. System account have a UID in range `1-199` `<1000` they typically do not have a login shell hence no home directory.
##### Related Commands 
```bash
# useradd is the command line utility for creating user/service accounts 

useradd [OPTIONS/FLAGS] [ARGUMENTS] $USERNAME 
  -u $UID # option for giving custom UID to user (--uid)
  -g $GROUP-ID/GROUP-NAME # option for setting up primary group (--gid)
  -d $DIRPATH # option for custom home directory (--base-dir)
  -s $SHELLPATH # option for providing the custom shell (--shell)
  -c $"STUFF" # option for custom comments (--comment)
  -e $YEAR-MONTH-DAY # option for expiration date (--expire-date)
  -G $GROUPNAMES/GIDS # option for creating a user with multiple secondary           groups (--groups)
  -k $DIRPATH # option for setting up the custom skel directory (--skel)
  -m # flag for creating user home directory onthego override defaults               (--create-home)
  -M # flag for avoid creating user home directory overriding defaults               (--no-create-home)
  -N # flag for not creating user default group yet use -g option to set one         (--no-user-group)
  -r # flag for creating system account (--system) 


userdel [OPTIONS/FLAGS] [ARGUMENTS] $USERNAME
  -r # flag for remooving the home directory too (--remove)
```

```bash
 # usermod is the command line utility for modifying user settings 
 
 usermod [OPTIONS/FLAGS] [ARGUMENTS] $USERNAME 
   -g $GROUP-ID/GROUP-NAME # option for setting/changing up primary group (--gid)
   -l $NEW-USER-NAME $OLD-USER-NAME # option for changing username (--login)
   -m $DIRPATH # option for giving moving user home dir (--move-home) @ --home
   -s $SHELL-PATH $USER-NAME # option for changing/setting user's login shell #(--shell)
   -L # flag for locking the account (--lock)
   -U # flag for unlocking the account (--unlock)
   -aG $GROUPNAMES/GIDS # option with add flag to add user to group 
   -e $YEAR-MONTH-DATE $USERNAME # option for setting up the expire date.             (--expire-date)
```

```bash 
# chage is the command line utility for setting up user pass

chage [OPTIONS/FLAGS] [ARGUMENTS] USERNAME 
  -l # flag to list user settings (--list)
  -d $YEAR-MONTH-DAY # option to set the last day pass expires (--lastday) 
  -M $NUMBER-0F-DAYS # option to set the maximum age of password (--maxdays) 
  -m $NUMBER-0F-DAYS # option to set minimum days interval between password          change (--mindays)
  -W $NUMBER-0F-DAYS # option to set the warning days (--warndays)  
```

## Groups
A Linux group can be defined as collection of user who share set of same privileges. Just like account group have a group ID and group name. List of groups can be found at `/etc/groups` 
##### Related Commands 
```bash
# To create and delete group

groupadd [OPTIONS/FLAGS] [ARGUMENTS] $GROUPNAME 
  -g $GROUP-ID # option for setting/changing up custom gid (--gid)
  -r # flag for creating system group (--system) 

groupdel $GROUPNAME # command to delete group
```

```bash
# gpasswd to manage group

gpasswd [OPTIONS/FLAGS] [ARGUMENTS] $GROUPNAME
  --add $USER # option to add user to group 
  --delete $USER # option to remove user from the group 
  --members # flag to list group members  
```

```bash
# groupmod to change group properties 

groupmod [OPTIONS/FLAGS] [ARGUMENTS] $GROUPNAME
  -g $GROUP-ID # option for setting up custom gid (--gid)
  -n $NEW-NAME # option for changing group name (--new-name)


```
