# File Permission

- Every file and directory in linux has set of permissions.
- These permissions controls who can read, write, or execute it.
- Permissions are stored in the **inode** of the file system and managed by kernel.

## Types of User Categories

### u (User)

- The user who created(own) the file.

### g (Group)

- The group of users associated with the file. This allows multiple, specific users to share access.

### o (Others)

- Everyone else. Any user who is not the owner and is not in the group.

## Types of Access Permissions

### r (Read)

- **On a file**: View file content.
- **On a directory**: List directory contents.

### w (Write)

- **On a file**: Modify or Truncate file content.
- **On a directory**: Add, delete, or rename files inside directory.

### x (Execute)

- **On a file**: Run file as program/script
- **On a directory**: Enter (or "changing into") the directory with `cd`
  - You cannot access files inside a directory without `x` permission on it, even if you have `r` permission.

## Numeric Representation of Permissions

| Permission  | Symbol | Numeric Value |
| ----------- | ------ | ------------- |
| **Read**    | `r`    | `4`           |
| **Write**   | `w`    | `2`           |
| **Execute** | `x`    | `1`           |

## Viewing Permission

```sh
# ls -l
-rwxr-xr--  1 root  devs  1234  Nov 4  script.sh
```

- `-` : regular file (`d` = directory, `l` = symlink)
- `rwx` : owner permissions
- `r-x` : group permissions
- `r--` : others permissions
- `1` : hard link count
- `root` : owner
- `devs` : group
- `1234` : file size
- `Nov 4` : modification date
- `script.sh` : file name

### Combining the Numeric Permission Values

| Example | Meaning     | Symbolic       |
| ------- | ----------- | -------------- |
| `7`     | 4+2+1 = rwx | full access    |
| `6`     | 4+2 = rw-   | read & write   |
| `5`     | 4+1 = r-x   | read & execute |
| `4`     | read only   | r--            |

## Changing Permission of File and Directory

- `chmod` (change mode): Changes permissions (read/write/execute)

### Symbolic Method (letters)

- Syntax: `chmod who operator permission file/folder`
- Who: `u` (owner), `g` (group), `o` (others), `a` (all)
- Operators: `+` (add), `-` (remove), `=` (set exactly)

```sh
chmod u+x script.sh # add execute for user

chmod g-w,o+r file.txt # remove write from group, set others to read-only

chmod a=rw config.ini # set everyone to read-write(no execute)
```

### Numeric Method

- Who: `u` (owner), `g` (group), `o` (others), `a` (all)
- Values: `r`=4 (Read), `w`=2 (Write), `x`=1 (Execute)

```sh
chmod 744 file.txt # rwxr--r-- (owner read/write/execute, group/others read-only)

chmod 700 private_key # rwx------ (owner only)
```

### Recusrive method

- Everything inside the directory will have the same permission.

```sh
$chmod -R 755 /path/to/directory
```

## chown(change owner)

- It changes the owner or group of a file or directory.
- Syntax: `chown [OPTIONS] NEW-USER:NEW-GROUP FILE/FOLDER`
  - USER: New user name
  - GROUP: Optional.
  - `:`: Seperator

> You must have root (superuser) privileges to change ownership
> When you change the ownership the location of the file or directory doesn't change

### Change only Ownership

```sh
chown developer file.txt

chown developer folder1
```

### Change both Owner and Group

```sh
chown developer:developer file.txt

chown developer:developer folder1
```

### Change only group

```sh
chown :developer file1.txt

chown :developer folder
```

### Change Ownership Recursively (for all subdirectories/files):

```sh
chown -R developer:developer /home/qe
```

## Symlink (Symbolic Link)

- Symlink stores a path pointing to the original file or directory.
- If the original file or directory is deleted, the symlink becomes broken.
- `ln -s /path/to/original /path/to/symlink`

### Options

- `s`: Create Symbolic Link
- `f`: Force. Overwrite if the symlink already exists.
- `v`: Verbose. Show what link is created.
- `n`: Treat link name as a normal file.

### Using Absolute Path

```sh
ln -s /home/user/documents/file.txt /home/user/desktop/file_link
```

### Using Relative Path

```sh
# if you are in the directory where the file exists
ln -s ../documents/file.txt ./desktop/file_link

# if you are in the directory where the symlink will be created
ln -s ../documents/file.txt file_link
```

### When symlink break?

- If the original file or directory is deleted.
- If the symlink is moved to a different location (relative path only)
- If the directory structure changes.
- If the file or directory is renamed.

> Symlink works fine if the original file or directory is modified.
> Using absolute path is recommended.

### When to use Symlink?

- When you need a shortcut to another location or file
- When you want safe behavour(If target is removed, symlink breaks.)
- Deployment structure
  - /app/releases/v1
  - /app/releases/v2
  - /app/current → /app/releases/v2

## Hard Link

- Another name for the same file.
- Every file on Linux/Mac has two parts:
  - The filename: human-readable name.
  - The inode: a unique ID that holds metadata (size, permissions) and points to the actual data.
- Hard link adds a second filename to the same inode.

`ln /path/to/original /path/to/hardlink`

```sh
echo "hello" > file.txt
ln file.txt hardlink.txt        # create hard link
```

```
file.txt     ──┐
               ├──►  inode 42  ──►  [data: "hello"]
hardlink.txt ──┘
```

### Characteristics of Hard Link

- Edit one file, the other changes too.
- Delete one file, the other still exists.
- Data only deleted when ALL names are removed
- Hard links point to an inode, not a path. So relative or absolute makes zero difference.
- Hard link cannot be created for directories.(Root user can but it's not recommended)

### Options

- `f`: Force. Overwrite if the hardlink already exists.
- `v`: Verbose. Show what link is created.
- `i`: Ask before overwriting.

> Hard links almost NEVER break like symlinks. Because they do not depend on file name.

### When to use Hard Link?

- When you want true duplicate name for same file.
- When you want file to survive deletion of original name.
- When you want to create a backup of a file.(It doesn't copy the data)
