# Package Management

- It is how linux system install, update, configure, and remove software packages.

## Key Concepts

- **Package**: A collection of software binaries with meta data. Meta data includes:
  - Name, version, description, dependencies, installation scripts, and other information.
- **Repository**: Centreal sever that stores and provides packages.
- **Dependency**: A software/package that is required by another package to function properly.
- **Package Manager**: A tool that handles installation, update, configuration, and removal of software packages.

## Package Manager Types

- **Low Level Package Manager**: Handles the basic operations of installing, updating, and removing packages. Do not resolve dependencies.
- **High Level Package Manager**: Work on top of low level package manager to resolve and install dependencies.

## Package Formats

1. **.deb**: Used by Debian, Ubuntu, Linux Mint, Pop!\_OS, and their derivatives.
   - `apt`: High level
   - `dpkg`: Low level
   ```
    package.deb
    ├── debian-binary      # Version of deb format
    ├── control.tar.gz     # Metadata (name, version, deps, scripts)
    ├── data.tar.gz        # Actual files to install
    └── metadata            # Checksums, etc.
   ```
2. **.rpm**: Used by Red Hat, Fedora, CentOS, OpenSUSE, and others.
   - `yum`: High level(older)
   - `dnf`: High level(Modern)
   - `rpm`: Low level
   ```
   package.rpm
    ├── Header             # Metadata (signatures, dependencies)
    ├── Signature          # GPG verification
    ├── Payload            # Cpio archive of files
    └── Scripts            %pre, %post, %preun, %postun
   ```
3. **.pkg.tar.zst**: Used by Arch Linux and its derivatives.
   - `pacman`: High level
   - `makepkg`: Low level

## Universal Package Formats

1. **.snap**: Used by Ubuntu, Fedora, and others.
   - `snap`: High level
2. **.flatpak**: Used by Ubuntu, Fedora, and others.
   - `flatpak`: High level

## Package Management Commands

### APT(Advanced Package Tool)

- `sudo apt update` - Update the package list
- `sudo apt upgrade` - Upgrade all packages
- `sudo apt install <package>` - Install a package
- `sudo apt remove <package>` - Remove a package
- `sudo apt search <package>` - Search package by name

### DPKG(Debian Package Manager)

- `sudo dpkg -i <package.deb>` - Install a package
- `sudo dpkg -r <package>` - Remove a package
- `sudo dpkg -l` - List installed packages

### DNF(Dandified YUM)

- `sudo dnf install <package>` - Install a package
- `sudo dnf update` - Update the package list
- `sudo dnf upgrade` - Upgrade all packages
- `sudo dnf remove <package>` - Remove a package
- `sudo dnf search <package>` - Search package by name
- `sudo dnf list installed` - List installed packages
- `sudo dnf repolist` - List enabled repositories
- `sudo dnf config-manager --add-repo <repo-url>` - Add a repository
- `sudo dnf config-manager --disable <repo-name>` - Disable a repository

### RPM(Red Hat Package Manager)

- `sudo rpm -ivh <package.rpm>` - Install a package
- `sudo rpm -Uvh <package.rpm>` - Update a package
- `sudo rpm -e <package>` - Remove a package
- `rpm -qa` - List installed packages

### Pacman

- `sudo pacman -Syu` - Update the package list and upgrade all packages
- `sudo pacman -S <package>` - Install a package
- `sudo pacman -R <package>` - Remove a package
- `pacman -Ss <package>` - Search package by name

### Snap

- `sudo snap install <package>` - Install a package
- `sudo snap refresh` - Update all packages
- `sudo snap remove <package>` - Remove a package
- `snap list` - List installed packages

### Flatpak

- `flatpak install <package>` - Install a package
- `flatpak update` - Update all packages
- `flatpak uninstall <package>` - Remove a package
- `flatpak list` - List installed packages

## Troubleshooting common issues

### Broken Dependencies

`E: Unable to correct problems, you have held broken packages`

**Why it happens:**

- Package installation was interrupted
- Dependency version conflicts
- Incomplete upgrades

**Fix:**

1. Debian/Ubuntu:
   - `sudo apt --fix-broken install`: It look for missing dependencies and install them, or remove the broken package that causing the problem
   - `sudo dpkg --configure -a`: It configure the packages that are not properly configured or interrupted during installation
   - `sudo apt clean`, `sudo apt update`, `sudo apt upgrade`: It clean the cache, update the package list and upgrade the packages
2. RHEL/CentOS/Fedora:
   - `sudo dnf check`: It check for broken dependencies and report back if any broken dependencies found
   - `sudo dnf distro-sync`: It synchronize the packages with the repositories and resolve the broken dependencies
   - `sudo dnf clean all`, `sudo dnf update`, `sudo dnf upgrade`: It clean the cache, update the package list and upgrade the packages.

### Locked Package Database

**Why it happens:**

- Another package manager process is running
- Previous installation/update was interrupted
- Corrupted package database

**Fix:**

1. Diagnosis:
   - Find what's locking the database: `sudo lsof /var/lib/dpkg/lock` or `sudo lsof /var/lib/dnf/lock`
   - Check if there's a process using the lock file: `ps aux | grep -i apt` or `ps aux | grep -i dnf`
2. Debian/Ubuntu:
   - `sudo systemctl status unattended-upgrades`: Check if unattended-upgrades service is running or not
   - `sudo kill <PID>`: If stuck, identify and kill the process safely
   - `sudo rm /var/lib/dpkg/lock-frontend`
   - `sudo rm /var/lib/dpkg/lock`
   - `sudo rm /var/cache/apt/archives/lock` : Remove stale locks (only if no processes are running)
   - `sudo dpkg --configure -a`: Reconfigure dpkg
   - `sudo apt update`
3. RHEL/CentOS/Fedora:
   - `sudo rm -f /var/lib/dnf/lock`: Remove the lock file

## Best Practices

- **Update before installing**: Always apt update / pacman -Sy first
- **Read the output**: Package managers show disk space and new dependencies
- **Clean cache regularly**: apt autoclean, pacman -Sc, dnf clean all
- **Use official repos**: Third-party repos can break systems
- **Check changelogs**: apt changelog package before major upgrades
- **Backup before dist-upgrades**: apt full-upgrade or pacman -Syu can be disruptive

<!-- 1. Dependency Resolution
2. Repository Management, Repository Security -->
