# File Management

- File Management refers to creating, viewing, editing and managing files and directories.
- Everything in Linux treated as files, including regular files, directories, hardware devices and even processes.
- Any file or directory whose name begins with a dot (.) is hidden (e.g., .bashrc).

## Types of files

- Regular files (`-`): contain text, binary data, or program code.
- Directory files (`d`): contain other files and directories.
- Special files: 
    - Character devices (`c`): Keyboard, Mouse.
    - Block devices (`b`): Hard drives.
    - Symbolic links (`l`): Shortcut/reference to another file.
    - Sockets (`s`): For inter process communication.
    - Names piper (`p`): allow data transfer between processes.

## File Management Commands

### Navigation and Listing

- `pwd` (print working directory): Shows the current location in the filesystem
- `ls` (List): Lists the contents of a directory. It can print files of present working directory and it can take arguments to print files from another directory.
    -  `ls /etc`
    - `ls /home/user/Documents`
    - `ls ../`: parent directory
    - `ls -l`: Long listing format (shows permissions, owner, size, etc.)
    - `ls -a`: Lists all files, including hidden ones.
    - `ls -lh`: Shows file sizes in a human-readable format (e.g., KB, MB).
- `cd` (Change Directory):
    - `cd /path/to/directory`: Go to an absolute path.
    - `cd directory_name`: Go to a subdirectory.
    - `cd ..`: Move up or come back on directory level.
    - `cd` or `cd ~`: Go to your home directory.
    - `cd -`: Move to your previous directory.


### Creation and Deletion of File and Directory

- `touch filename`: Creates an empty file.
- `mkdir directory_name`: Creates a new empty directory.
    - `mkdir -p path/to/nested/folder`: Creates parent directories if they don't exist. `-p` stands for parent. It tells the command to create any missing parent directories along the way.
- `rm filename`: Remove(delete) a file.
    - `rm -f filename`: Forcefully deletes a file without confirmation.
- `rmdir directory_name`: Remove(delete) an empty directory.
    - `rmdir -r directory_name`: Recursively remove(delete) a directory and all its contents (use with caution).

### Viewing Files

- `cat filename`: Display the entire file content to the screen.
- `less filename`/`more filename`: Views file content page by page (scroll with arrows, use `q` to quit).
- `head filename`: Shows the first 10 lines of a file.
    - `head -n 5 filename`: `-n` let's you choose the number of lines.
- `tail filename`: Shows the last 10 lines of a file.
    - `tail -n 5 filename`: `-n` let's you choose the number of lines.


### Copying, Moving and Renaming Files

- `cp` (copy): Copies files or directories.
    - `cp source-filename destinatio-filename`
    - `copy file1.txt /path/to/destination`: Copies the file to a destination directory
    - `cp -r source_dir/ destination_dir/`: Recursively copies a directory and all of its contents.
- `mv` (move): Moves or renames files and directories.
    - `mv file.txt /path/to/destination`: Moves the file.
    - `mv oldname.text newname.txt`: Renames a file.

### Searching and Finding Files

- `find path options expression`: 
    `find /home -name "notes.txt"`: Search for a file named notes.txt inside the home directory and sub directories.
    - `find /home -iname "notes.txt"`: Case-insensitivity. Seaches Notes.txt, NOTES.TXT, etc
    - `find /etc -type d -name "conf*"`: Finds directories under /etc whose names start with “conf”.
    - `find /var/log -size +10M`: Finds files in /var/log larger than 10 MB.
- `locate`: Fast file search
    - `locate notes.txt`: Instantly lists all files on the system containing the name notes.txt.
    - `locate bashrc`: Lists all files that contain the word “bashrc” in their name or path.
- `grep`: Search inside file
    - `grep "root" /etc/passwd`: Finds all lines in /etc/passwd that contain the word “root”.
    - `grep "error" *.log`: Searches for “error” in all .log files in the current directory.
    - `grep -i "error" server.log`: Finds “error”, “Error”, or “ERROR”.
- ``:
