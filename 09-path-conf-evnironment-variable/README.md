# Environment Variables

- Environment variables are key-value pairs that are used to store configuration for applications and the operating system.
- They are dynamic and affect how processes, shell, and applications behave.
- They store iformation such as paths, user preferences, system settings, and application configurations.

## Types of Environment Variables

1. **System Environment Variables**: These are set at the system level and are available to all users and processes. `/etc/environment`, `/etc/profile`, `/etc/profile.d/`
2. **User Environment Variables**: These are set at the user level and are available only to the user who set them. `~/.bashrc`, `~/.bash_profile`, `~/.profile`
3. **Shell Environment Variables**: These are set within a shell session and are available only to processes started within that shell.

## Common Environment Variables

- `$HOME`: Current user's home directory. `/home/username`
- `$USER`: Current user's username.
- `$PATH`: Contains list of directories seperated by colon. Directories that the shell searches for executable files.
- `$SHELL`: Path to the current user's default shell. `/bin/bash`
- `$LANG`: System's default language.
- `$PWD`: Current working directory.
- `$EDITOR`: Default text editor.

## Viewing All Variables

```sh
printenv
env
```

## Viewing a Specific Variable

```sh
echo $VARIABLE_NAME
printenv VARIABLE_NAME
```

> Names are case-sensitive.

## Setting a Variable

### Setting a temporary variable

```sh

# VARIABLE_NAME=value
MY_VARIABLE="Hello World"

# export VARIABLE_NAME=value, Available for subprocesses as well
export MY_VARIABLE="Hello World"
```

### Setting a permanent variable

Add export command to a startup file:

- `~/.bashrc`: for interactive bash shells after login
- `~/.bash_profile` or `~/.profile`: for login shells
- `/etc/environment`: system-wide, loaded at boot and for all users

```sh
# Add the following line to ~/.bashrc or ~/.bash_profile
export MY_VARIABLE="Hello World"
```

> Whenever you make changes to these files, you need to reload the shell configuration using `source ~/.bashrc` or `source ~/.bash_profile` or `source ~/.profile` or `source /etc/environment`.

## Removing a Variable

```sh
unset VARIABLE_NAME
```

## $PATH

- The `$PATH` variable, contains colon-separated list of directories that the shell searches for executable files.
- Linux searches left to right for executable commands.
- The order of directories in the `$PATH` variable matters.
- The shell searches for executable files in the order of directories listed in `$PATH`. The first match is used.

### Modifying the $PATH variable

#### Temporarily adding a directory to `$PATH`

```sh
export PATH=$PATH:/new/directory
```

#### Add a directory to `$PATH` at the beginning

```sh
export PATH=/new/directory:$PATH
```

#### Permanently adding a directory to `$PATH`

```sh
echo 'export PATH=$PATH:/new/directory' >> ~/.bashrc
source ~/.bashrc
```

### Removing a Directory from `$PATH`

```sh
export PATH="${PATH/:/unwanted/dir/}"
```