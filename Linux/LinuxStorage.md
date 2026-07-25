## Block Device
A **block device** is a type of hardware or virtual device in Linux/Unix that allows **data to be read and written in fixed-size blocks**, typically 512 bytes or 4 KiB, rather than as a continuous stream.
### Types of block devices 
| Block Device    Type         | Description                              | Linux Device File Examples               | Key Characteristics                                    |
| ---------------------------- | ---------------------------------------- | ---------------------------------------- | ------------------------------------------------------ |
| **HDD (Hard Disk Drive)**    | Traditional spinning disks               | `/dev/sda`, `/dev/sdb`                   | Persistent storage, random access in blocks            |
| **SSD (Solid-State Drive)**  | Flash-based storage                      | `/dev/nvme0n1` (NVMe), `/dev/sdX` (SATA) | Faster than HDD, no moving parts, block-addressable    |
| **Removable Storage**        | USB drives, external disks               | `/dev/sdX`, `/dev/sr1`                   | Hot-pluggable, treated as block devices                |
| **Optical Drives**           | CD/DVD/Blu-ray drives                    | `/dev/sr0`, `/dev/cdrom`                 | Read/write in blocks, often read-only                  |
| **RAID Devices**             | Virtual devices combining multiple disks | `/dev/md0`, `/dev/md1`                   | Redundancy or striping, block-level abstraction        |
| **Loop Devices**             | Treats a file as a block device          | `/dev/loop0`, `/dev/loop1`               | Mount ISO/images, encrypted containers                 |
| **Network Block Devices**    | Block storage over network               | `/dev/nbd0`, `/dev/sdX` (mapped)         | Behaves like local disk, supports iSCSI/NBD            |
| **Virtual / Memory Devices** | RAM disks                                | `/dev/ram0`, `/dev/ram1`                 | Volatile storage, very fast, behaves like block device |
##### Related Commands
```bash
# df is the utility to get info
df [OPTIONS/FLAGS] [ARGUMENTS] [DIR/FILE/BLOCK]
  -a # flag to see all the mounted files system including association with block
  -l # flag to limit only local file system 
  -h # flag to print in human readable format 
  -v # flag to invert results 
  -type #FILE-SYSTEM-TYPE option to filter out results on equivalence of file        system
```

```bash
# du command is used to diplay disk usage statics (quick commands) 
du -sh $DIRPATH/FIL # display the output summary in human readbale format
```

```bash
# Get the UUID of the block device (quick command) 
sudo blkid $PATH-TO-BLOCK
```
## Partition Scheme 
The data is actually stored onto the data blocks / sectors within the disk, a new disk is just set of physical data blocks. When the disk is initialised with a partition scheme a partition table is created and saved in the first reserved sector of the hard disk, this table contains meta data definition like how many partitions can exist how their sectors are organised accordingly.
### Partition scheme types 
#### MBR 
In an **MBR disk**, a **primary partition** is a directly defined partition in the MBR table that can store data or an OS and is bootable. An **extended partition** is a special type of primary partition that acts as a container for **logical partitions**, allowing a disk to have more than four partitions. **Logical partitions** reside inside the extended partition, function like primary partitions for storing data, but generally cannot be booted from directly. This hierarchy ensures that MBR’s four-partition limit is extended while maintaining OS compatibility.
#### GPT
In a **GPT disk**, all partitions are treated as **equal primary partitions**, eliminating the need for extended or logical partitions. Each partition is identified by a **unique GUID** and can store data, an OS, or special system files such as EFI or recovery partitions. GPT supports a large number of partitions (default 128, configurable up to thousands) and includes **redundant partition tables** and **checksums** for integrity, making it more robust and scalable than MBR.
##### References 
| Feature                | **MBR (Master Boot Record)**                                  | GPT (GUID Partition Table)                                                                                |
| ---------------------- | ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Metadata Location**  | Sector 0 (first 512 bytes): boot loader + 4 partition entries | LBA0: Protective MBR<br>LBA1: Primary GPT Header<br>LBAs 2–33: Partition Entries<br>Last LBAs: Backup GPT |
| **Max Disk Size**      | 2 TiB (32-bit LBA addressing)                                 | 8 ZiB (64-bit LBA addressing)                                                                             |
| **Max Partitions**     | 3 primary + 1 extended within upto 128 logicals inside        | Typically 128 by default                                                                                  |
| **Partition Type IDs** | 1-byte type code (e.g., 0x83 = Linux, 0x07 = NTFS)            | 16-byte GUID per partition type                                                                           |
| **Redundancy**         | None (single MBR sector → single point of failure)            | Yes: backup GPT header + CRC checksums for integrity                                                      |
| **Boot Compatibility** | BIOS (legacy boot loaders in MBR bootstrap code)              | UEFI (requires EFI System Partition); Protective MBR for legacy tools                                     |
| **Recovery**           | Fragile: corruption of sector 0 breaks scheme                 | Resilient: backup GPT at disk end can restore metadata                                                    |
##### Related Commands 
```bash
# fdisk is the utility to work with disk (with bitchass cli)

# To initilize a disk with GPT partition scheme 
fdisk /dev/sdx
# PRESS g for initilizing disk with GPT and creating a partition table 
# PRESS n for new partition followed with prompts 
# PRESS w for write the changes 
```
## Partitons 
After the partition scheme is defined to disk  partitions are created.  A partition is a logical division of the disk into parts when created these are defined in the partition table which keeps track of which partition starts and ends at which sector. 
##### Related Commands 
```bash
# fdisk to create partitions 
parted /dev/DEVICE # Start parted on a disk device (interactive shell)
parted -l # List all partitions on all recognized devices
parted /dev/DEVICE print # Show partition table for the device
parted /dev/DEVICE mklabel gpt # Create new GPT partition table
parted /dev/DEVICE mklabel msdos # Create new MBR partition table
parted /dev/DEVICE mkpart NAME TYPE START END # Create partition (TYPE=primary, logical, extended)
parted /dev/DEVICE rm N # Remove partition N
parted /dev/DEVICE resizepart N END # Resize partition N to END sector/size
parted /dev/DEVICE move N START END # Move partition N to new location
parted /dev/DEVICE name N NAME # Assign name to partition N
parted /dev/DEVICE set N FLAG STATE # Set flag (boot, lvm, raid, esp, hidden, etc.)
parted /dev/DEVICE align-check TYPE N # Check alignment of partition N (TYPE=min|opt)

```
## File systems
After the partitions are made, the partition is formatted with the file system, which handles all the things related to blocks like allocation , tracking, mapping etc.
### File system types 
| File System        | Best Use Case                                                | Key Features                                                   | Limitations                                                      |
| ------------------ | ------------------------------------------------------------ | -------------------------------------------------------------- | ---------------------------------------------------------------- |
| **ext4**           | General purpose (default in most distros, desktops, servers) | Journaling, stable, supports large files/volumes, low overhead | No built-in snapshots or checksumming                            |
| **XFS**            | Large files, high-performance servers, parallel I/O          | Scales well, journaling, good throughput                       | Poor performance with many small files, resizing is limited      |
| **Btrfs**          | Systems needing snapshots, rollback, data integrity          | Copy-on-write, snapshots, subvolumes, checksumming             | Still less stable at huge scale compared to ext4/XFS             |
| **F2FS**           | Flash/SSD storage (phones, embedded, laptops with SSD)       | Optimized for NAND flash, wear leveling                        | Not ideal for spinning HDDs, less common on servers              |
| **ZFS (on Linux)** | Enterprise storage, NAS, data integrity critical systems     | Copy-on-write, snapshots, self-healing, built-in RAID          | High RAM usage, not native in Linux kernel (CDDL license issues) |
##### Related Commands 
```bash
# Commands related to creation of file system 

mkfs.ext4 /dev/DEVICE # Create EXT4 filesystem (most common Linux FS)
mkfs.xfs /dev/DEVICE # Create XFS filesystem (high-performance, journaling FS)
mkfs.btrfs /dev/DEVICE # Create Btrfs filesystem (copy-on-write, snapshots, CS)
mkfs.fat /dev/DEVICE # Create FAT32 filesystem (compatibility, small devices)
mkfs.vfat /dev/DEVICE # Alias for mkfs.fat
mkfs.ntfs /dev/DEVICE # Create NTFS filesystem (used in Windows systems)
mkfs.exfat /dev/DEVICE # Create exFAT filesystem (large files, external drives)
mkfs.minix /dev/DEVICE # Create Minix filesystem (legacy, simple FS)
```

```bash
fsck /dev/DEVICE # Check and repair a filesystem
e2fsck /dev/DEVICE # Check/repair EXT2/3/4 filesystem
xfs_repair /dev/DEVICE # Repair XFS filesystem
btrfs check /dev/DEVICE # Check Btrfs filesystem
```
## Mounting 
Mounting is just mapping of directory to a block device, therefore all the data of the disk will be stored onto the block device. 
##### Related Commands 
```bash
# To find all the mountpoints (quick command) 
findmnt [DIRPATH/BLOCKDEVICE] --list
```
### Mount manually
```bash
# To handle mounting mount & unmount commands are used (quick commands)

# to mount a block device
mount -t FILE-SYS-TYPE -O options BLOCKDEVICE DIRPATH

# to unmount a block device
umount BLOCKDEVICE DIRPATH
```
### Mount automaticialy
The mounting is manual but can be permanent using /etc/fstab file. which is a table of 6 columns where
- **Device**: usually the given name or UUID of the mounted device (sda1/sda2/etc).
- **Mount Point**: designates the directory where the device is/will be mounted.
- **File System Type**: nothing trick here, shows the type of filesystem in use.
- **Options**: lists any active mount options. If using multiple options they must be separated by commas.
- **Backup Operation**: (the first digit) this is a binary system where `1` = dump utility backup of a partition. `0` = no backup. This is an outdated backup method and should NOT be used.
- **File System Check Order**: (second digit) Here we can see three possible outcomes. `0` means that fsck will not check the filesystem. Numbers higher than this represent the check order. The root filesystem should be set to `1` and other partitions set to `2`

| **Mount option** | **Description**                                                                                                          |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `defaults`       | Option that includes all of `auto`, `rw`, `nouser`, `exec`, and `suid`                                                   |
| `auto`           | Auto-mounting upon booting                                                                                               |
| `noauto`         | Does not mount automatically upon booting                                                                                |
| `rw`             | Mount so that reading and writing are both available                                                                     |
| `ro`             | Read only                                                                                                                |
| `nouser`         | Only the root account can mount                                                                                          |
| `user`           | Regular accounts can also mount                                                                                          |
| `exec`           | Permits files to run                                                                                                     |
| `suid`           | Permits SetUID and SetGID                                                                                                |
| `nofail`         | Allows booting even if an error occurs. If mounting fails due to a typo, it boots by excluding the volume with the error |
```bash
# Sample entry

/dev/sdb1   /mnt/data   ext4   defaults,auto,rw,user,exec,suid,nofail   0   2
```

## Logical Volume Management 
Logical volume management can be defined as a mechanism in which multiple block device with different storage capacity can contribute to storage pool, out of which needful storage can be casted out to use accordingly. \
### LVM components 
#### Physical Volume
Physical volume is just a block device initialised with physical volume of lvm. This can be used to contribute to volume group.
##### Related Commands
```bash
# physical volume commands
# LVM recognises physical volumes by applying label on them 

pvs # Command to list all the volumes
  -a # Flag tolList all devices including inactive PVs
  -o # Flag to specify columns to display
  -v # Flag fot Verbose output
```

```bash 
pvscan # Command to scan all disks for physical volumes and display details
  -v # Flag for verbose, show detailed info
  --cache # Flag to update cache
```

```bash
pvck $BLOCK # Check metadata and consistency of a Physical Volume
  -v # Flag for verbose, show detailed metadata
  -f # Flag to force check even if PV appears valid
```

```bash
pvcreate BLOCK # Initialize a block device or partition as an LVM Physical Volume
  -ff # Flag to force creation even if existing signatures or data exist
  -y  # Flag to answer yes to all prompts
  --dataalignment $ALIGNMENT_BYTE # Option to align data to specified boundary 
  --metadatasize $SIZE # Option to set metadata size for the PV (default auto)
  --uuid $UUID # Option to assign a specific UUID to the PV
```

```bash
pvchange [OPTIONS] BLOCK # Change attributes of an existing physical volume
  -x n|y $BLOCK # Flag to enable (y) or disable (n) allocation of LVs on this PV
  --uuid $UUID BLOCK # Flag to show or change the UUID of the PV
  --alloc $POLICY BLOCK # Option to set allocation policy (contiguous, cling, normal)
```

```bash
pvresize BLOCK # Resize a Physical Volume to match the size of the underlying block device
  --setphysicalvolumesize $SIZE $BLOCK # Option to resize PV to a specific size
  -v $BLOCK # Flag for verbose output
```

```bash
pvremove $BLOCK # Remove a Physical Volume from LVM
  -f $BLOCK # Flag to force removal without confirmation
  -y $BLOCK # Flag to answer yes to all prompts
```
#### Volume Group
A volume group represent storage pool, out of which logical storage can be casted out. 
##### Related Commands 
```bash
vgs # To list all the volume groups
  -a # Flag to include inactive volume groups
  -o $COLUMNS # Flag to specify columns to display
  -v # Flag for verbose output
```

```bash
vgscan # To get storage details of volume groups
  -v # Flag for verbose, show detailed info
  --cache # Flag to update cache
```

```bash
vgdisplay $VGNAME # To get details of a volume group with metadata
  -v # Flag for verbose, show all internal VG metadata
```

```bash
vgcreate VGNAME BLOCKS # To create a volume group from one or more physical volumes
  -s $SIZE # Option to set Physical Extent (PE) size
  -f # Flag to force creation even if VG with same name exists
  -v # Flag for verbose output
```

```bash
vgextend VGNAME BLOCKS # To extend a volume group by adding physical volumes
  -v # Flag for verbose output
  --alloc $POLICY # Option to set allocation policy (contiguous, cling, normal)
```

```bash
vgmerge $VGNAME1 $VGNAME2 # To combine two or more volume groups
  -v # Flag for verbose output
```

```bash
vgrename $NEW-VGNAME $OLD-VGNAME # To rename a volume group
  -v # Flag for verbose output
```

```bash
vgreduce $VGNAME $BLOCKS # To remove a physical volume from a volume group
  -v # Flag for verbose output
  --removemissing # Flag to remove missing PVs
```

```bash
vgremove $VGNAME # To delete a volume group
  -f # Flag to force removal without confirmation
```
#### Logical Volume
Logical volume is casted storage out of volume group this storage can be formatted with file system, and mounted too.
##### Related Commands
```bash
lvs # To list all logical volumes
  -a # Flag to include inactive logical volumes
  -o COLUMNS # Flag to specify columns to display
  -v # Flag for verbose output
```

```bash
lvscan # To scan all volume groups and logical volumes
  -v # Flag for verbose, show detailed info
```

```bash
lvdisplay LVNAME # To get detailed information about a logical volume
  -v # Flag for verbose, show all internal LV metadata
```

```bash
lvcreate -n LVNAME -L SIZE VGNAME # To create a new logical volume from a volume group
  -i NUMBER # Flag to create a striped LV across NUMBER of PVs
  -I STRIPESIZE # Option to set stripe size
  -s # Flag to create a snapshot of an existing LV
  -v # Flag for verbose output
```

```bash
lvextend -L SIZE LVNAME # To increase the size of a logical volume
  -r # Flag to resize the filesystem along with LV
  -v # Flag for verbose output
```

```bash
lvreduce -L SIZE LVNAME # To decrease the size of a logical volume
  -r # Flag to resize the filesystem along with LV
  -v # Flag for verbose output
  -f # Flag to force reduction without confirmation
```

```bash
lvremove LVNAME # To delete a logical volume
  -f # Flag to force removal without confirmation
  -v # Flag for verbose output
```

```bash
lvrename OLD-LVNAME NEW-LVNAME # To rename a logical volume
  -v # Flag for verbose output
```

```bash
lvconvert # To convert logical volumes to mirrored or RAID type
  -m NUMBER # Number of mirrors
  -v # Flag for verbose output
```
