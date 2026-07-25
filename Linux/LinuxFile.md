## Linux file
Everything in linux is a file cause think, a task is completed by a process, a process is a program in execution, and where do we store the program ?? ofc on the file. The data itself is stored in binary format within the allocated data blocks on the disk. A inode is a data structure which represent a file but under the hood it maps all the data of the file to data blocks in the disk. 

The filesystem manages how these bytes are organized into blocks and how they are linked back to their corresponding inodes. Suppose in a operation of file opening the inode mapped to filename is checkout out, from that inode pointers to all data blocks are gone through to fetch data later representing it with specific encoding. (the editing and representing is done via buffer)
There are whole lotta operations one can perform on file including manipulation ownership / permission / access control , archival , compression , encryption , manipulating data inside of file handling storage etc.
### Linux file types 
#### Regular files
The regular files are nothing but simple media files , program files or executables.
#### Directory
A directory is essentially a file that contains entries. Each entry consists of a filename and a pointer (or reference) to the corresponding inode of the file it points to. The inode contains metadata about the file, including its location on disk, permissions, and other attributes.
#### Special files
##### `Character files`
Character files serve as a buffer between linux and character devices (mouse)
##### `Block files` 
Block files are the files which act as a data storage like disks or external storage devices
##### `Links` 
Links are the pointers to the inode they share same block location on the data storage device. These pointers can exist across multiple locations and can be created they can be synced (hard links) or not (soft links).0
##### `Sockets`
Sockets are mechanism of inter process communication, in socket the data flows both direction this can be across different devices too they can also use packeting via tcp.
##### `Pipes`
Pipes are mechanism of inter process communication, in pipes the data flow is unidirectional, they are typically used for processes that share the same lineage.
##### Related Commands
```bash
# Get file info 
file $FILE

# Get File type 
stat $FILE 

# Create file 
touch $FILE 

# Remove file 
rm -r $FILE 

# Create Directory 
mkdir -p $DIRECTORY 

# Remove directory 
rm -r $DIRECTORY 

# Create soft link 
ln $SOURC_EDIR $TARGET_DIR 
  -s # flag for softlink 

# To remove link 
unlink $LINK 
```

### Linux file system and structure

linux file are distributed throughout the system. 
##### `/bin` 
Bin contains all the binary executables which are accessible throughout the system. 
##### `/boot`
Boot contains all the boot files like boot loader.
##### `/dev`
Dev contains all the block devices.
##### `/etc`
Etc contains all the system configurations.
##### `/home`
Home contains all the home directory of the user.
##### `/lib`
Lib contains system libraries and frameworks.
##### `/media`
Media contain all the media files specially those of mounted.
##### `/mnt`
Mnt contain all the mounted items such as removable ssds.
##### `/opt`
Opt contains all the add-on application software.
##### `/tmp`
Tmp contains all the temporary files.
##### `/usr`
Contains unix system resources like binaries , utils etc.
##### `/var` 
Var contains data that changes frequently and usually system generated.  
##### `/proc`
Proc contains all the system generated process information 

## Linux file Ownership and Control 
### Linux file ownership 
When a file is created in linux, the user who creates the file own it along with permissions, at the same time the default group usr is part of gets the same privileges. User is the owner, so he can modify the ownership and file permissions and file controls too. 
##### Related Commands
```bash
# Change and manipulate file ownership

chown [OPTIONS/FLAGS] [USER:GROUP, USER, :GROUP] $FILE
  -R # flag for for recursive change  
```
### Linux file permissions 
A file in linux can be used only if accessible this include operations like `read` `write` `execute` . 
These permissions are represented via octane bits 001 = 1 = execute , 010 = 2 = write , 100 = read. Or initials r = read , w = write , x = execute too. These can be used combined in set of any like +rwx, -rw etc.

Special permissions are also available by the owner this include. 
- `Sticky Bit` Run as the user who owns it SUID or run as the group who owns it SGID.  `s`
- RUN as both sticky bit. `t`
##### Related Commands 
```bash
# Change and manipulate user ownership 

chmod [+/-][$PERMISSION_BITS] [USER, GROUP, USER:GROUP] $FILE 
  -R # flag for recursively apply   
```
### Linux files access control 
For refined access control like multiple user of same group with specific permissions access control lists are used. 
##### Related Commands 
```bash
# Refine access control with access control list 

# To get access control list permissions 
getfacl $FILE
  -R # flag for changing permissions recursively
  -P # flag to avoid symbolic links
  -n # flag for printing numeric identifiers

# Set permissions for the files 
setfacl $FILE 
# Accessor identifier can be u for user g for group as
# Accessor can be any username or group-name or account-name
# Permission can be r for read w for write x for execute s for SxID t for SBIT
  -m [ACCESSOR-IDENTIFIER]:[ACCESSOR]:[PERMISSIONS] # Option for modifying           applying
  -b # Flag to clear all permissions
  -x [ACCESSOR-IDENTIFIER]:[ACCESSOR] # Option to remove permissions for accessor
  -R # Flag changing permissions recursively
```
### Linux file attributes 
File operations such as deletion, append etc can be managed with the help of file attributes. 
##### Related Commands 
```bash
# To set file attributes 

# List list all the file access control info 
lstattr [ATTRIBUTE] $FILE 

# Set the file access control 
chattr [OPTIONS/FLAGS] [ACTION][ATTRIBUTE] FILE
# Action can be + for adding attributes, - for dropping them
a # For appemd mode 
i # For preventing changes 
s # For source deletion 
  -R # for changing recursively
  
```

## Linux File Operations
### Linux File Archival 
A tarball is a single file with metadata which encapsulates set of directory and files init. 
##### Related Commands
```bash
# tar to archive files and directories 

tar -f TARBALL [OPTIONS/FLGAS]
  -f $TARBALL # required option 
    -c FILE1 FILEN # option for creation of archival
      -a # sub-flag for auto compress @ archival (--auto-compress)
      -z # sub-flag for gzip compression @ archival (--gzip)
      -j # sub-flag for bzip2 compression @ archival (--bzip2)
      -J # sub-flag for xz compression @ archival (--xz)
      -Z # sub-flag for compression @ archival (--compress)
    -x # flag for extraction
  -t # flag for listing contents
  -p # sub-flag can be used for preserving permissions (--preserve-permissions)
  -v # sub-flag can be used for verification
  -C $PATH #flag can be used for extracting at specific location
 
# General command format for archival: tar -f TARBALL.tar -c FILE1 FILE2 FILEN
# General command format for unarchival: tar -f TARBALL.tar -xC DESIRED-DIR-PATH/
# Sample command: tar -f testdir/archived123.tar.gz -cz 1.txt 2.txt 3.txt
```
### Linux File Compression
File data when repetitive can be represented with an algorithm. 
##### Related Commands
```bash
# bzip, gzip, xz, 7z are the most use command line utils for compressions \n
# Extensions are necessary to identify the compression formal without hassale \n

# To compress and decompress original file using bzip \n 
bzip [OPTIONS/FLGAS] [ARGUMENTS] $FILES
# simply providing file result in compression of orignal file  
  -c # flag to write output into stdout (IO redirection) 
  -d # flag for decompression 
# General command format for compression: bzip -c FILES > FILE.bz2
# General command format for decompression: bzip -dc FILE.bz2 > DESIRED-DIR-PATH/

# To compress and decompress original file using gzip 
gzip [OPTIONS/FLGAS] [ARGUMENTS] $FILE
# simply providing file result in compression of orignal file  
  -c # flag to write output into stdout (IO redirection) 
  -d # flag for decompression 
# General command format for compression: gzip -c FILES > FILE.gz
# General command format for decompression: gzip -dc FILE.gz > DESIRED-DIR-PATH/

# To compress and decompress original file using xz 
xz [OPTIONS/FLGAS] [ARGUMENTS] $FILE
# simply providing file result in compression of orignal file  
  -c # flag to write output into stdout (IO redirection) 
  -d # flag for decompression 
# General command format for compression: xz -c FILES > FILE.xz
# General command format for decompression: xz -dc FILE.xz > DESIRED-DIR-PATH/

# To compress and decompress original file using zip for compression and unzip for decompress 
zip/unzip [OPTIONS/FLGAS] [ARGUMENTS] $FILE
  -d # flag for decompression
# General command format for compression: zip FILE.zip FILE
# General command format for decompression: unzip FILE.zip -d DESIRED-DIR-PATH/
```
### Linux file encryption 
Encryption is converting data into unusable form i.e by readability, content, decryption not likewise. In a normal situation where send wants to send data via encryption here how its follows the sender gets the receiver public key, sender decrypt the data using it, and send. Moving on the receiver receives it and decrypt with the help of private key. more security stuff follows like signatures etc. In the following the chances of getting nuked is extremely low as as a middle men to get data its useless as decryption is done at the user end, However mimicking as one can lead to disasters.
#### Encryption options 
##### `RSA and RSA (default)` 
Description:This option generates a key pair where both the public and private keys use the RSA algorithm for both encryption and signing.
Use Cases: It allows you to encrypt messages and files as well as sign them for authenticity. This is the most commonly used option and is suitable for general purposes.
##### `DSA and Elgamal`
Description: This option creates a Digital Signature Algorithm (DSA) key for signing and an Elgamal key for encryption.
Use Cases: DSA is used for creating digital signatures, while Elgamal is used for encrypting data. This combination is less common than RSA but can be useful in specific scenarios where you want to separate signing from encryption.
##### `DSA (sign only)`
Description: This option generates a key pair that uses DSA solely for signing purposes.
Use Cases: It is used when you only need to create digital signatures without the need for encryption. This might be suitable for verifying the integrity of messages or documents.
##### `RSA (sign only)`
Description: This option creates an RSA key that can only be used for signing. 
Use Cases: Similar to the DSA (sign only) option, this type is used when you need to sign messages or files but do not require encryption. It ensures that the signed content can be verified by others using your public key.
##### Related Commands
```bash

# gpg from GnuPG is the most trusted utility for encrypting the files 

gpg [OPTIONS/FLGAS] [ARGUMENTS]
  --full-generate-key # flag for generating keys with multiple options for the keys 
  --list-keys # flag for listing public keys 
  --list-secret-keys # flag for listing private keys 
  --import $PUBLIC-KEY-FILE # option for importing public keys
    --allow-secret-key-import # sub-flag to import secret key @ import
  --export $KEY-ID # option for stdout the key  
  --export-secret-key $KEY-ID # option for stdout the secret key 
  --delete-key $KEY-ID # option for deleting public keys 
  --delete-secret-key $KEY-ID # option for deleting public keys 
  --output $FILE # option for redirecting the output  
  --armour # flag for converting the keys binary content to ASCII 

# General command to export public key to a file 
gpg --armor --export KEY-ID --output FILE.asc 

# General command to export private key to a file 
gpg --armor --export-secret-keys KEY-ID --output FILE 

# General command to encrypt a file 
gpg --encrypt FILE --recipient KEY-ID --output FILE 

# General command to decrypt a file (assume private key is present thy which the decrypt was done)
gpg --decrypt FILE --output FILE 

```
### Linux file search
With the help of file name and file properties they can be searched across the system with the help of `find` utility. 

```bash
# find is the command line utility to search for file across system

find [PATH] [FILE] [OPTIONS/FLGAS] [ARGUMENTS] 
  -o # flag to make an OR expression 
  -not # flag to make a NOT expression \!
  -name FILE/REGEXP # option to search the file with the help of regex 
  -iname FILE/REGEXP # option to search the file case sensitive
  -i # flag to search files ignnoring case  
  -type [d/f/l] # option to search file by its type 
  # Use d for directory f for regular file l for links 
  -size [+/-]N[c/k/M/G] # option for searching file by their size 
  # Use c for bytes, k for kilobytes, M for megabytes, and G for gigabytes.
  -mmin [+/-]N #option to search  file on the basics of last modified min
  # - sign is used to check files that are modified under last n minutes 
  # + sign can be used to check files that are modified before n minutes 
  -mtime [+/-]N # option to search file on the basics of days 
  # 1 represent 24 hours 
  -perm [+/-][PERM] # option to search file on the  basics of permissions 
  # Permission can be of octal or non octal like chmod format 
  # + sign can be used for max permissions 
  # - sign can be used for least permissions 
  # / sign can be used for any of the permissions provided
  
# Sample command 
find /BikiniBottom/ -iname *formula.txt -size +69420k -type f 
```
