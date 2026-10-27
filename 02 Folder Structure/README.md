# Folder Structure

- Linux uses a single root directory.
- Everything starts at `/`. No drive letters.
- The Filesystem Hierarchy Standard (FHS) defines what goes where so tools, packages, and admins stay consistent.

## `/` Root Directory

- The root of all directories.
- Starting point for entire filesystem.
- It acts as a mount point for partitions and filesystems.

## Boot and Startup

### `/boot`

- Contains all files required to boot the system.
- Kernel image, actual Linux kernel (`vmlinuz`)
- Initial RAM disk used during boot (`initrd` or `initramfs`)
- Bootloader configuration files (`grub.cfg`)

## Essential Commands and Libraries

### `/bin`

- Contains essential commands needed for boot and single user mode.
- Commands like `ls`, `cp`, `cat`, `mv`, `rm`, `bash` reside here.

> **Note:** In modern Linux systems, `/bin` is typically a symlink to `/usr/bin`.

### `/sbin`

- Contains system administration and system management commands.
- Some commands require root or elevated privileges, while others may be executable by regular users.
- Used for system maintenance, configuration, and recovery tasks.
- Commands like `ifconfig`, `fdisk`, `shutdown`, `ip`, `route` reside here.

> **Note:** In modern Linux systems, `/sbin` is typically a symlink to `/usr/sbin`.

### `/lib`, `/lib64`

- Contains essential shared libraries required `/bin` and `/sbin`.

> **Note:** In modern Linux systems, `/lib` and `/lib64` are symlinks to `/usr/lib` and `/usr/lib64`.

## System and Hardware Information

### `/dev` Device Files

- Every device (disks, USB, terminal) in Linux is treated as a file.
- Contains device files that represent hardware components and pseudo-devices.

```
/dev/sda, /dev/sdb: Represents your hard drives or SSDs.
/dev/tty1, /dev/pts/0: Represents your terminal sessions.
/dev/random, /dev/urandom: Sources of random data.
```

### `/proc` Virtual Filesystem for Processes

- A virtual filesystem (not real files) that provides runtime system information.
- Provides structured information about processes and kernel/system information.
- Not stored on disk - exists only in memory.

```
/proc/cpuinfo: Information about the CPU.
/proc/meminfo: Information about memory usage.
/proc/loadavg: System load average.
/proc/[pid]/: A directory for every running process, containing details about its execution.
```

### `/sys` System Information

- Virtual filesystem for drivers and hardware information.

## System Configuration

### `/etc` Configuration Files

- System wide configuration files.
- Editing these often requires root privileges.
- DevOps engineers work with these files often.

```
/etc/passwd: User account information.
/etc/hosts: Local hostname mappings.
/etc/group: Group information.
/etc/fstab: Filesystem mount information.
/etc/ssh/sshd_config: SSH server configuration.
```

## User Data and Home Directories

### `/root`

- Home directory for the root user account.
- `/root` is normally accessible only to root.
- Contains root user's personal files and configurations.

> If you are normal user say, `/home/kumar` then if you switch to root user using `sudo su -` then your working directory will change to `/root`.

- `su` is used when you need tempory root access. Switches to root while keeping current user's working directory and existing environment variable.
- `su -` Switches to root user with full login shell. It loads root user's environment variables and sets the working directory to `/root`.

### `/home`

- Home directory for all regular users.
- Each user has their own subdirectory.
- `/home/ks`, `/home/ramya`

## Variable & Temporary Data

### `/var` Variable Files

- Files/data changes while system is running. (logs, databases, caches, backups).

```
/var/log/: System and application log files.
/var/cache/: Application cache data.
/var/spool/: Data waiting to be processed (Print/mail queues, cron jobs.)
/var/tmp/: Temporary files preserved between reboots
/var/www/: Web server files
```

### `/run` Runtime Data

- Contains runtime process information.
  - Process IDs (PID files)
  - Socket files
  - Lock files
  - Temporary runtime data
- Cleared on reboot

### `/tmp` Temporary Files

- Stores temporary files.
- Usually cleared on reboot so don't store anything important in `/tmp` folder.

## Application and Software

### `/usr` User System Resources

- Contains system wide applications, utilities, libraries, and documentation.

```
/usr/bin: contains most user commands and executable programs.
/usr/sbin: contains system administration commands.
/usr/lib: contains shared libraries needed by the binaries.
/usr/share: Architecture-independent shared data (man pages, icons, fonts, timezone data).
/usr/local: Locally installed software.
```

### `/opt` Optional/Third-Party Software

- Used for installing third-party software that don't follow the standard FHS.
- Used for installing third-party softwares like, Google chrome, Oracle database.
- Each application installs in its own directory (e.g., /opt/google/chrome/).

## Mount Points and Storage

### `/mnt` Mount

- Used by admins, It is a temporary mount point for external storage.

### `/media` Media

- Mount point for removable media devices such as, USB drives, CD/DVD-ROMs, external hard drives.

## Service-Specific

### `/srv` Service Data

- Data served by the system.
- If you run a web server, your website files could go in `/srv/www/`. If you run an FTP server, files could go in `/srv/ftp/`.

## Main Categories of Linux Filesystem

| Category      | Directories                      |
| ------------- | -------------------------------- |
| Boot & Core   | `/boot`, `/bin`, `/sbin`, `/lib` |
| Config        | `/etc`                           |
| Users         | `/home`, `/root`                 |
| Software      | `/usr`, `/opt`                   |
| Variable Data | `/var`                           |
| Temporary     | `/tmp`, `/run`                   |
| Devices       | `/dev`                           |
| Virtual       | `/proc`, `/sys`                  |
| Mount         | `/mnt`, `/media`                 |

> /bin, /lib, and /sbin are often just symbolic links (shortcuts) pointing to /usr/bin, /usr/lib, and /usr/sbin.

> Many executables in /bin /sbin depend on shared libraries located under /lib or /usr/lib.

> Many system and application programs use configuration files stored under /etc.

> /boot contains boot-related files such as the Linux kernel and initramfs. The bootloader uses these files to start the operating system.

> Programs installed under /usr may read or write runtime/application data stored under /var.

## Production Examples

- Logs: `/var/log/` → System and application logs.
- Configurations: `/etc/` → System-wide configuration files.
- User data: `/home/` → Users' personal files.
- Applications: `/usr/` or `/opt/` → Installed software.
- Temporary files: `/tmp/` → Temporary data.
- Backups: `/var/backups/` → System backups.
- Databases: `/var/lib/mysql/` → Common MySQL data location.
- Web servers: `/var/www/html/` → Common web document root.
- SSH: `/etc/ssh/` → SSH configuration files.

# Path

A path is an address for a file or folder. It tells the operating system where a file or folder is located. To access a file or a folder you can provide either Absolute Path or Relative Path.

## Absolute and Relative Paths

- Absolute Path: Starts from root directory.
- Relative Path: Starts from current directory.
- `.` means current directory
- `..` means parent directory
- `~` means home directory.
  - If you are in `/home/user/Documents` and if you run `$cd ~` will take you to `/home/user`.
  - `cd ~`, `cd $HOME`, and `cd` are same.

### Absolute Path

- Starts from root directory.
- Always starts with `/`.
- Works the same no matter where you currently are in the filesystem.
- It points to exactly one location.
- Doesn't depend on current working directory.
- Use case: Scripts, cron jobs, config files, systemd units
- Example: `/home/user/Documents/file.txt`

### Relative Path

- Starts from current working directory.
- Doesn't start with `/`.
- Depends on current working directory.
- Use case: Navigating within a directory.
- Example: `Documents/file.txt`
