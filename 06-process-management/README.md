# Process Management

- A process is a running instance of a program.

## Each process includes

- PID (process id): Unique number
- PPID (parent process id)
- Own Memory space.
- File descriptors (open files, sockets, pipes).
- Security credentials (UID, GID, capabilities).
- Execution context (CPU registers, program counter, stack).
- A kernel-managed `task_struct` that holds all its state.

## Process States

- Running (R): Currently executing or on the queue to execute.
- Sleeping (S): Waiting for an event (user input, network data, a timer to expire).
- Disk Sleep (D): An uninterruptible sleep. The process is waiting for I/O to complete. cannot be interrupted, if interrupted it could leave the hardware in an inconsistent state.
- Stopped (T): Process has been stopped by signal (SIGSTOP, SIGTSTP). It can be resumed later.
- Zombie (Z): Process has been terminated but its parent don't know yet. The process entry remains so that parent can read its exit status.
- Dead (X): Process that has been completely terminated.

## Process Lifecycle

1. PID1 (`systemd`)
   - When Linux boots: BIOS/UEFI → Bootloader → Kernel loads
   - The kernel starts the first process i.e. PID1
   - In modern Linux, PID 1 = systemd
   - PID1 Starts system services (SSH, network, database, etc.).
2. **Creation fork()**
   - `fork()` creates a child process that is identical to parent process.
   - Child gets its own unique **PID** and inherits a copy of parent's memory, file descriptors, and credentials.
   - `fork()` return is used to tell parent and child apart.
   - In the parent, `fork()` returns the PID of the new child to manage child process.
   - In the child, `fork()` returns `0` so that child can identify itself as new process.
3. **Execution**
   - `exec()` replaces the current process's memory space with a new program.
   - PID does not change. Only the program changes.
     > `fork()` is almost always followed by an `exec()` in the child process to run a different program.
4. **Running**
   - Scheduled by the CPU scheduler to use CPU time.
5. **Waiting (Sleeping)**
   - Process waits for some event (I/O completion, child process exit, etc.).
6. **Termination**
   - Normal Termination: `exit()` system call (`0` for success, non-zero for an error)
   - Abnormal Termination: The process is killed by a signal (e.g., `SIGKILL`, `SIGTERM`)
   - Parent retrieves exit status using `wait()`.

## Types of Process

- Foreground Process
  - Attached to the current terminal.
  - Takes input and displays output directly.
  - When you type `vim` or `ls`, you start a foreground process.
  - Control of the shell is taken. User input is blocked until foreground process completes or is stopped..
- Background Process
  - Run independently of the current terminal.
  - Doesn’t block user input.
  - Start backgroud process by adding `&` to your command. Ex: `sleep 300 &`
- Deamon Process
  - These are system-level background processes that provide services.
  - Started at the boot and doesn't attached to any login session.
  - Example: `sshd` (SSH server), `httpd` (web server), and `cron` (job scheduler).
- Zombie Process
  - Process that has finished execution but still has an entry in the process table (waiting for parent to read its exit status).

## Process Identifiers (IDs)

- `PID (Process ID)`: Unique number assigned to each process.
- `PPID (Parent Process ID)`: The PID of the process that started this process.
- `UID (User ID)`: The user who owns the process.
- `GID (Group ID)`: Group associated with the process.

## Important Process Attributes

- `State`: Running, Sleeping, Stopped, Zombie.
- `Priority (PRI)`: Determines when the process will be scheduled.
- `Nice Value (NI)`: User-controllable value that affects priority (-20 = high, +19 = low).
- `CPU Usage`: % of CPU time consumed.
- `Memory Usage`: Physical + Virtual memory.

## Viewing Processes

### `ps` - Process Status - Shows processes running in current shell

- `ps -e` or `ps -A` - Lists all/every processes.
- `ps -ef`: Shows every process on the system in full format. Also shows PPID, making it easier to see who started what.
- `ps aux`: A popular BSD-style variant. `a` (all users), `u` (user-oriented format), `x` (processes that are not attached to a terminal).
- `ps -u` - Displays processes for current user with detailed information.
- `ps -u username` – Show user-specific processes.
- `ps -C processname` – Show a process by name
- `pgrep processname`– Find a process by name and return its PID
- `pidof processname`– Find the PID of a running program

- `uptime`: Shows how long the system has been running, along with the number of users and system load averages.
- `vmstat`: Virtual memory statistics.
- `iostat`: CPU and I/O statistics.

> Difference between ps aux and ps -ef is you can see Memory utilization aux but not with -ef

> Key columns are USER, PID, %CPU, %MEM, COMMAND

| Scenario                         | Best Command |
| -------------------------------- | ------------ |
| Check if process exists          | `ps -e`      |
| Understand parent-child relation | `ps -ef`     |
| Monitor CPU / memory             | `ps aux`     |
| Debug your login/session         | `ps -u`      |

| Command  | Style | All Processes | PPID | CPU/MEM | Daemons |
| -------- | ----- | ------------- | ---- | ------- | ------- |
| `ps -e`  | UNIX  | ✅            | ❌   | ❌      | ✅      |
| `ps -ef` | UNIX  | ✅            | ✅   | ❌      | ✅      |
| `ps aux` | BSD   | ✅            | ❌   | ✅      | ✅      |
| `ps -u`  | UNIX  | ❌            | ❌   | ✅      | ❌      |

## Managing Processes

### Kill the Process

- `kill PID` - Used to kill the process first check the process id and then use the command.
- `kill -9 PID` - Used to kill the process forcefully.
- `pkill processname` - Used to kill a process by pattern matching.
- `killall processname` - Used to kill all processes by name.

### Signals and Process Communication

- `kill -l` - List all signals.

| Signal    | Number | Meaning                           |
| --------- | ------ | --------------------------------- |
| `SIGHUP`  | 1      | Reload configuration or terminate |
| `SIGINT`  | 2      | Interrupt (Ctrl + C)              |
| `SIGQUIT` | 3      | Quit with core dump               |
| `SIGKILL` | 9      | Forceful kill                     |
| `SIGTERM` | 15     | Graceful termination              |
| `SIGCONT` | 18     | Continue                          |
| `SIGSTOP` | 19     | Pause                             |

## Resource Monitoring

- `free -h`: Displays free and used memory in the system.
- `df -h`: Displays disk space usage.
- `du -sh directory`: Displays disk usage of a specific directory.

### Lower and Increase the priority

In linux, process priority is controlled by nice value. Nice values from -20 to +19.

- Lower nice value - Higher priority - `-20` means higher priority.
- `0` - Default priority.
- Higher nice value - Lower priority - `19` means lower priority.

- You cannot set nice below -20 or above 19.
- Increasing priority (negative nice) requires root privileges because it can impact system performance.
- Decreasing priority (positive nice) is allowed for your own processes without sudo.
- Use priority adjustments wisely:
  - High-priority processes can starve others and make the system unresponsive.
  - Low-priority is useful for background tasks (e.g., backups, compilations).

**Start process with High priority**

`nice -n -5 ./script.sh`

**Start process with Low priority**

`nice -n 15 ./script.sh`

**Increase priority of a running process**

`renice -n -18 -p PID`

**Decrease priority of a running process**

`renice -n 10 -p PID`

## Real World Monitoring Scenarios

1. High CPU Usage
   - `top` or `htop` to identify the process consuming CPU.
   - `ps aux --sort=-%cpu | head` to list top CPU-consuming processes.
   - `kill -15 PID` or `kill -9 PID` to terminate a process if needed (use with caution).

2. High Memory Usage
   - `free -h` to check memory usage.
   - `top` or `htop` to identify memory-hungry processes.
   - `kill -15 PID` or `kill -9 PID` to terminate a process if needed (use with caution).

3. Disk Space Issues
   - `df -h` to check disk space.
   - `du -sh /path/to/directory` to check directory sizes.
   - `rm -rf /path/to/unnecessary/files` to remove unnecessary files (use with caution).

4. Application not responding
   - `ps aux | grep application-name` to check if the application is running.
   - `systemctl restart application-name` to restart the application.

5. Monitoring Logs
   - `tail -f /var/log/syslog` to monitor system logs.
   - `journalctl -u service-name` to check logs for a specific service.

6. Network Issues
   - `ping google.com` to check connectivity.
   - `netstat -tuln` or `ss -tuln` to check open ports.
   - `ifconfig` or `ip a` to check network interfaces.

7. Service Failures
   - `systemctl status service-name` to check service status.
   - `journalctl -u service
