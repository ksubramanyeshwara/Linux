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

## Navigation and Listing

- `pwd` (print working directory): Shows the current location in the filesystem
- `ls` (List): Lists the contents of a directory. It can print files of present working directory and it can take arguments to print files from another directory.
  - `ls /etc`
  - `ls /home/user/Documents`
  - `ls ../`: parent directory
  - `ls -l`: Long format with details (shows permissions, owner, size, etc.)
  - `ls -a`: Lists all files, including hidden ones.
  - `ls -lh`: Shows file sizes in a human-readable format (e.g., KB, MB).
  - `ls -lt`: Sorts files by modification time, newest first.

```bash
drwxr-xr-x 16 user user 4096 Feb 9 15:05 Documents

# file type
# permissions
# link count (number of files in the directory)
# owner
# group
# size
# last modified date
# file name
```

![Long Listing Format](./longlistformat.png)

- `cd` (Change Directory):
  - `cd /path/to/directory`: Go to an absolute path.
  - `cd directory_name`: Go to a subdirectory.
  - `cd ..`: Move up or come back on directory level.
  - `cd` or `cd ~`: Go to your home directory.
  - `cd -`: Move to your previous directory.

## Creation of File and Directory

- `touch filename`: Creates an empty file.
- `mkdir directory_name`: Creates a new empty directory.
  - `mkdir -p path/to/nested/folder`: Creates parent directories if they don't exist. `-p` stands for parent. It tells the command to create any missing parent directories along the way.

## Deletion of File and Directory

- `rm filename`: Remove(delete) a file.
  - `rm -f filename`: Forcefully deletes a file without confirmation.
- `rmdir directory_name`: Remove(delete) an empty directory.
  - `rmdir -r directory_name`: Recursively remove(delete) a directory and all its contents (use with caution).

## Viewing Files

- `cat filename`: Display the entire file content to the screen.
- `less filename`/`more filename`: Views file content page by page (scroll with arrows, use `q` to quit).
- `head filename`: Shows the first 10 lines of a file.
  - `head -n 5 filename`: `-n` let's you choose the number of lines.
- `tail filename`: Shows the last 10 lines of a file.
  - `tail -n 5 filename`: `-n` let's you choose the number of lines.

### Copying Files

- `cp` (copy): Copies files or directories.
  - `cp source-filename destinatio-filename`
  - `copy file1.txt /path/to/destination`: Copies the file to a destination directory.
  - `cp -r source_dir/ destination_dir/`: Copies all the files and folder recursively.

## Moving and Renaming Files

- `mv` (move): Moves or renames files and directories.
  - `mv file.txt /path/to/destination`: Moves the file.
  - `mv oldname.text newname.txt`: Renames a file.

### Searching Files

Find searches based on name, size, file type, date, permissions, ownership, and then perform actions on the results.

```bash
$find [path] [options/expression] [action]

# path: Where to start searching.
# options/expression: Search criteria.
# action: What to do with the found files.
```

#### Searching by Name and Type

1. Find a specic file by name inside /home directory
   - `$find /home -name "notes.txt"`
2. Find all JPG files in current directory
   - `$find . -name "*.jpg"`
3. Find all directories name start with "conf" under /etc
   - `$find /etc -type d -name "conf*"`
4. Find only files whose names ends with .log inside /var
   - `$find /var -type f -name "*.log"`

#### Searching by Size, Date, and Permissions

1. Find all files larger than 10 KB in the current directory
   - `$find . -SIZE +10k`
2. Find all files modified in the last 7 days
   - `$find /log -mtime -7`
3. Modified in the last 24 hours:
   - `$find /var/www -mtime -1`
4. Accessed more than 30 days ago:
   - `$find . -atime +30`
     > -atime File was read or accessed. -ctime File metadata (like permissions) was changed.

#### Searching by Permissions and Ownership

1. Find all files owned by user "john" in /home
   - `$find /home -user john`
2. Find all files owned by group "developers" in /projects
   - `$find /projects -group developers`
3. Find all files with permission 774 in the current directory
   - `$find . -perm 774`
4.

#### Combining Logic

- -and / -a: (Default) Both conditions must be true.
- -or / -o: Either condition can be true.
- -not / !: Finds files that do not match the criteria.

## Searching inside files

`grep`: Global Regular Expression Print. Searches for text patterns inside files.

```bash
grep [options] [pattern] [file...]

# A file
# Multiple files
# A directory (with -r)
# Standard input (pipe)

# [options]: modify the behavior
# pattern: text or regex you want to search for.
# file: one or more files to search. If no file is given, grep reads from standard input (e.g., piped data).
```

**When grep searches for something, it prints the entire line that contains the match**

**Why does it show the whole line?**

- Because grep works in a line-by-line manner:
- Read one line
- Check if pattern exists
- If yes → print that line
- Move to next line
- It does NOT print partial content by default.

### grep Options

| Option | Description                                             |
| ------ | ------------------------------------------------------- |
| -i     | Ignore case (case-insensitive search)                   |
| -r     | Recursively search directories                          |
| -n     | Show line numbers                                       |
| -c     | Count the number of matching lines                      |
| -l     | Only list filenames with matches                        |
| -w     | Match whole words only                                  |
| -v     | Invert match (show lines that do not match the pattern) |
| -A NUM | Show NUM lines after the match                          |
| -B NUM | Show NUM lines before the match                         |
| -C NUM | Show NUM lines before and after the match               |

### Examples

- `grep "root" /etc/passwd`: Searches for the word **root** inside `/etc/passwd`.
- `grep "error" *.log`: Searches for “error” in all .log files in the current directory.
- `grep -i "error" server.log`: Finds error in server.log in the current directory irrespective of case.
- `grep -n "error" app.log`: It shows the line number of the match.
- `grep -c "error" app.log`: Counts the number of lines that contain the word “error” in app.log.
- `grep -v "error" app.log`: Shows lines that do not contain the word “error”.
- `grep -r "error" /var/log`: Recursively searches for “error” in all files under the /var/log directory.
- `grep -w "error" app.log`: Matches the whole word “error” (not substrings like “terror”).
- `grep -A 3 "error" app.log`: Shows 3 lines after each match.
- `grep -B 2 "error" app.log`: Shows 2 lines before each match.
- `grep -C 1 "error" app.log`: Shows 1 line before and after each match.

#### Real-world Examples

- Check container logs
  - `$docker logs container_name | grep ERRORCheck container logs`
- Lists all processes and filters for those containing “nginx”.
  - `ps aux | grep "nginx"`
- Displays all lines containing “error” (case-insensitive) from syslog.
  - `cat /var/log/syslog | grep -i "error"`

### grep with Regular Expressions

- `grep "^root" /etc/passwd`: Finds lines starting with “root” in `/etc/passwd`.
- `grep "root$" /etc/passwd`: Finds lines ending with “root” in `/etc/passwd`.
- `grep "r[ao]t" /etc/passwd`: Finds lines containing “rat” or “rot” in `/etc/passwd`.
- `grep "r.t" /etc/passwd`: Finds lines containing “rat”, “rot”, “rut”, etc. (any single character in place of the dot).
- `grep "r.*t" /etc/passwd`: Finds lines containing “rat”, “root”, “rtt”, etc. (any sequence of characters between “r” and “t”).

## Achive and Compression

- Combining multiple files and directories and their metadata (permission, ownership, timestamps, etc) into a single file.
- Reducing the size of a file or archive to save storage space or bandwidth.

> Linux, these are traditionally separate steps.
> Classic tool used to create and extract archives is `tar`.

### tar

- tar stands for Tape ARchive.
- It is standard for backups and software distribution.

- Creation: `tar [options] [destinatoin-locarion]/[archive-file] [input-location/file...]`
- Extraction: `tar [options] [input-location/archive-file] -C [destinatoin-locarion]`

> Always put f at the end of your options string (e.g., -cvf), because tar expects the filename to follow immediately after the f.

| Option | Meaning | What it does                                 |
| ------ | ------- | -------------------------------------------- |
| -c     | Create  | Starts a new archive.                        |
| -x     | Extract | Pulls files out of an archive.               |
| -t     | Tist    | Shows you what's inside without extracting.  |
| -v     | Verbose | Shows progress in the terminal (handy!).     |
| -f     | File    | Tells tar the next argument is the filename. |

### Adding compression to the tar

| Tool  | Flag | Speed  | Characterestics |
| ----- | ---- | ------ | --------------- |
| gzip  | -z   | Fast   | Moderate        |
| bzip2 | -j   | Medium | Good            |
| xz    | -J   | Slow   | Best            |

> Speed + compatibility: gzip (.tar.gz).
> Smaller size for text/source: xz or bzip2.

### Example

1. Create an archive

- `tar -cvf log.tar /etc`

2. Extract an archive

- `tar -xvf log.tar`

3. List contents without extracting

- `tar -tvf log.tar`

4. Create a compressed archive

- `tar -cvzf archive.tar.gz /etc`
- `tar -cvjf archive.tar.bz2 /etc`
- `tar -cvJf archive.tar.xz /etc`

5. Extract a compressed archive

- `tar -xvzf archive.tar.gz`
- `tar -xvjf archive.tar.bz2`
- `tar -xvJf archive.tar.xz`

## zip and unzip

- `zip` both archives and compresses.
- `unzip` extracts and decompresses.

> zip is not native to Linux, but it is available in most distributions.

```bash
$zip [options] destination/zipfile source-files
$unzip [options] source-zipfile -d output-dir
```

### Example

1. Create a zip archive of a file

- `zip /backup/file.zip /etc/file`

2. Create a zip archive of a directory

- `zip -r /backup/etc.zip /etc` -r includes everything inside the directory

3. Zip multiple files into a single archive

- `zip -r /backup/config.zip /etc/file /var/logfile`

4. Extract a zip to current directory

- `unzip /backup/etc.zip`

5. Extract a zip to a specific directory

- `unzip /backup/etc/zip -d /restore`

#### When to use what?

- Sharing with Windows users: Use `.zip`.
- Distributing Linux Source Code: Use `.tar.gz` or `.tar.xz`.
- Daily backups of your own files: Use `.tar.gz` (it's the fastest balance of size and speed).
- Archiving massive data for long-term storage: Use `.tar.xz` to save the most disk space.
