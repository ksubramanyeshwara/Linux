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

## Changing Permission of File and Folder

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

