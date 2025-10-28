# Folder Structure

- Linux uses a single rooted tree. 
- Everything starts at `/`. No drive letters.
- The Filesystem Hierarchy Standard (FHS) defines what goes where so tools, packages, and admins stay consistent.

## `/`
 
- The root of all directories. 
- Starting point for entire filesystem.
- It acts as a mount point for partitions and filesystems.

## `/root` 

- Home directory for root user.
- It is limited normal user.
- Contains root user's personal files and configurations.

## `/bin`

- Contains essential user commands available to all users and system at boot.
- It should be available even in single-user mode for system recovery.
- Critical for system boot and repair operations.

## `/sbin`

- Contains system administration commands.
- It requires root level access and regular users cannot execute these commands.
- Used for system maintenance, configuration, and recovery tasks.

## `/lib`, `/lib64`

- Contains essential shared libraries required by binaries in `/bin` and `/sbin` to boot the system and run the commands in the root filesystem.
- It also includes `/lib/modules/` for kernel drivers.
- It is used for making system calls to the hardware.

##  `/boot`

- Contains all files required to boot the system.
    - Kernel image, actual Linux kernel (`vmlinuz`)
    - Initial RAM disk used during boot (`initrd` or `initramfs`)
    - Bootloader configuration files (`grub.cfg`)
    - 

## `/usr` User System Resources

- One of the largest directories.
- Contains the bulk of read-only user data and applications.
- Contains user programs, libraries, and documentation.

```

/usr/bin: User commands binaries for all users.
/usr/sbin: Non-essential system binaries.
/usr/lib: Shared libraries needed by the binaries.
/usr/share: Shared files like icons, documentation, man pages, timezone info, etc.
/usr/local: Locally installed software.

```

## `/srv` Service Data

- Contains data for services provided by the system.
- If you run a web server, your website files could go in `/srv/www/`. If you run an FTP server, files could go in `/srv/ftp/`.

## `/opt` Optional/Third-Party Software

- Used for installing third-party software that don't follow the standard FHS.
- Used for installing third-party softwares like, Google chrome, Oracle database.
- Each application installs in its own directory (e.g., /opt/google/chrome/).

## `/mnt` Mount

- Used by admins, It is a temporary mount point external storage.

## `/media` Media

- Mount point for removable media devices such as, USB drives, CD/DVD-ROMs, external hard drives.

## `/var` Variable Files

- Contains files that are expected to grow in size over time.
```

/var/log/: System log files.
/var/cache/: Application cache data.
/var/spool/: Data waiting to be processed (Print/mail queues, cron jobs.)
/var/tmp/: Temporary files preserved between reboots
/var/www/: Web server files

```

## `/tmp` Temporary Files

- Used by programs for temporary data.
- Data are typically deleted upon system reboot. Don't store anything important.

## `/home` User Home Directories

- Each normal user will have their own personal folder under `/home`
- `/home/ks`, `/home/ramya` 

## `/data` Data Folder

-  Shared data folder used for storing data (used in orgs for reporting, billing)

## `/etc` Configuration Files

- Contains system wide configuration files.
- Editing these often requires root privileges.

```

/etc/passwd: User account information.
/etc/hosts: Local hostname mappings.
/etc/group: Group information.
/etc/fstab: Filesystem mount information.
/etc/ssh/sshd_config: SSH server configuration.

```

## `/run` Runtime Data

- Contains runtime process information.
    - Process IDs (PID files)
    - Socket files
    - Lock files
    - Temporary runtime data
- Cleared on reboot

## `/sys` System Information

- Virtual filesystem for Kernel and Hardware information.

## `/proc` Virtual Filesystem for Processes

- A virtual directory (not real files) that provides runtime system information.
- Not stored on disk - exists only in memory.
- Provides structured information about devices, drivers, and kernel features.

```

/proc/cpuinfo: Information about the CPU.
/proc/meminfo: Information about memory usage.
/proc/loadavg: System load average.
/proc/[pid]/: A directory for every running process, containing details about its execution.

```


## `/dev` Device Files

- Every device (disks, USB, terminal) in Linux is treated as a file.
- Contains device files that represent hardware components and pseudo-devices.

```

/dev/sda, /dev/sdb: Represents your hard drives or SSDs.
/dev/tty1, /dev/pts/0: Represents your terminal sessions.
/dev/random, /dev/urandom: Sources of random data.

```

## Main Categories of Linux Filesystem

| Category                          | Description                                                         | Example Directories               |
| --------------------------------- | ------------------------------------------------------------------- | --------------------------------- |
| 1️⃣ Essential System Binaries     | Core commands and libraries needed for booting and single-user mode. Without these, the system cannot start or operate in rescue mode. | `/bin`, `/sbin`, `/lib`, `/lib64` |
| 2️⃣ System Configuration          | System-wide configuration files and service settings                | `/etc`                            |
| 3️⃣ User Data & Environment       | Stores user files, personal configurations, and home environments.                             | `/home`, `/root`                  |
| 4️⃣ Temporary & Variable Data     | Files that change frequently or are temporary                       | `/tmp`, `/var`, `/run`            |
| 5️⃣ System & Hardware Information | Virtual filesystems providing process and kernel info               | `/proc`, `/sys`, `/dev`           |
| 6️⃣ Application & Software        | Optional or third-party software, system programs                   | `/usr`, `/opt`, `/srv`            |
| 7️⃣ Mount Points                  | Used to attach external or temporary filesystems                    | `/mnt`, `/media`                  |


> How is the Linux filesystem structured?

> The Linux filesystem starts at / and is divided into logical areas — system binaries and libraries (/bin, /lib), configuration (/etc), user data (/home), variable and temporary data (/var, /tmp), virtual system information (/proc, /sys, /dev), applications (/usr, /opt), and mount points (/mnt, /media). Each serves a specific purpose to keep the OS modular and organized

## Understanding the linux terminal prompt

### Inside a Docker Container

```
root@ubuntu-dev:/#
```

- root: The currently logged in user name (in this case, the root user with full admin privileges).
    - Separate user accounts are usually created for each individual and access will be given to them based on role.
- ubuntu-dev: The hostname of the container.
- : Separator (no special meaning here).
- / The present working directory (here it's the root / of the file system).

> Every file and folder in Linux are created under /.

### In an EC2 Instance

In an AWS EC2 instance along with root user it creates a another user ubuntu

```
ubuntu@hostname:~$
```

- ubuntu: The user currently logged onto AWS EC2 instance. (By default, root user is not logged in for the security reasons).
- hostaname: Name of the instance
- ~: Represents the current working directory. `~` is shorthand for your home directory. ~ means /home/ks
- $: This is the prompt symbol.
    - `$` usually indicates a normal user.
    - If you were the root user, it would typically be `#`.

### In an Local Machine

```
ks@ks:~$
```

- ks(before @): Currently logged in user name
- ks(after @): Hostaname of the machine
- ~: Represents the current working directory. `~` is shorthand for your home directory.
- $: This is the prompt symbol.
    - `$` usually indicates a normal user.
    - If you were the root user, it would typically be `#`.




