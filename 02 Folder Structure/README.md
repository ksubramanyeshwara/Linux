# Folder Structure

- Linux uses a single rooted tree.
- Everything starts at `/`. No drive letters.
- The Filesystem Hierarchy Standard (FHS) defines what goes where so tools, packages, and admins stay consistent.

## `/`

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

- Contains essential commands available to all users and system at boot.
- It should be available even in single-user mode for system recovery.
- Critical for system boot and repair operations.

### `/sbin`

- Contains system administration commands.
- It requires root level access and regular users cannot execute these commands.
- Used for system maintenance, configuration, and recovery tasks.

### `/lib`, `/lib64`

- Contains essential shared libraries required by binaries in `/bin` and `/sbin` to boot the system and run the commands in the root filesystem.
- It also includes `/lib/modules/` for kernel drivers.
- It is used for making system calls to the hardware.

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

- A virtual directory (not real files) that provides runtime system information.
- Not stored on disk - exists only in memory.
- Provides structured information about devices, drivers, and kernel features.

```

/proc/cpuinfo: Information about the CPU.
/proc/meminfo: Information about memory usage.
/proc/loadavg: System load average.
/proc/[pid]/: A directory for every running process, containing details about its execution.

```

### `/sys` System Information

- Virtual filesystem for Kernel and Hardware information.

## System Configuration

### `/etc` Configuration Files

- Contains system wide configuration files.
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

- Home directory for root user.
- It is limited to normal user.
- Contains root user's personal files and configurations.

> If you are normal user say, `/home/kumar` then if you switch to root user using `sudo su -` then you will be in `/root`.

- `su` is used when you need tempory root access. It uses `/home/username` home directory.
- `su -` is used when you need to switch to root user and load root user's environment variables.

### `/home`

- Home directory for all regular users.
- Each user has their own subdirectory.
- `/home/ks`, `/home/ramya`

## Variable & Temporary Data

### `/var` Variable Files

- Contains files/data that change frequently. (logs, databases, caches, backups).
- Contains files that are expected to grow in size over time.

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

- Used by programs for temporary data.
- Data are typically deleted upon system reboot. Don't store anything important.

## Application and Software

### `/usr` User System Resources

- One of the largest directories.
- Contains the majority read-only user data and applications.
- Contains user programs, libraries, and documentation.

```

/usr/bin: User commands binaries for all users.
/usr/sbin: Non-essential system binaries.
/usr/lib: Shared libraries needed by the binaries.
/usr/share: Shared files like icons, documentation, man pages, timezone info, etc.
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

- Contains data for services provided by the system.
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

> /bin relies on /lib: The executable programs inside /bin (like ls, cp, bash) are rarely self-contained. They rely on shared code (libraries) stored in /lib.

> /usr relies on /etc: Almost every program installed in /usr/bin looks into /etc for its instructions.

> / relies on /boot: The /boot directory contains the kernel (the core of the OS). The computer loads this first.

> /var relies on /usr: The /var directory contains data that changes frequently. It relies on /usr for the programs that generate this data.

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
