# Service Management

- **Process**: Running instance of a program.
- **Service**: A process that runs in the background and performs a specific task. Often stated at boot.
- **Daemon**: A service that runs in the background. (e.g. `sshd`, `httpd`, `mysqld`, `nginx`)

## init system

- init system is a process that starts when the system boots and manages other processes.
- Modern system uses systemd as a defualt init stystem(PID1)

## systemd

- systemd is a init system that is used in most modern Linux distributions.
- systemd manages services, sockets, devices, mount points, and other system resources.
- systemd have parellel service startup, which leads to faster boot times.
- `systemctl` command is used to manage services.

## systemctl

- `systemctl start <service-name>`: Start a service in current session.
- `systemctl stop <service-name>`: Stop a service.
- `systemctl restart <service-name>`: Restart a service.
- `systemctl reload <service-name>`: Reload a service without stopping it.
- `systemctl status <service-name>`: Check the status of a service.
- `systemctl enable <service-name>`: Enable a service to start at boot.
- `systemctl disable <service-name>`: Disable a service from starting at boot.
- `systemctl is-enabled <service-name>`: Check if a service is enabled to start at boot.
- `systemctl is-active <service-name>`: Check if a service is active.
- `systemctl list-unit-files:` Shows all services and states.

## Service States

- active (running) — currently running
- active (exited) — ran successfully and exited (one-shot)
- failed — exited with an error
- inactive — not running
- enabled/disabled — whether it starts at boot

## Viewing Logs

- `journalctl -u <service-name>`: View logs for a specific service. `u = unit`
- `journalctl -u <service-name> -f`: Follow live logs for a specific service.
- `journalctl -u <service-name> --since today`: Shows todays logs for a specific service. You can also mention yesterday, this week, this month, this year, "2026-03-01", "1 hour ago", etc.
- `journalctl -u <service-name> -n 50`: Shows last 50 lines for a specific service. `n = number of lines`

> All logs are stored in /var/log/journal or /var/log/syslog

## Unit (Service) Files

- Services are defined by unit files.
- Service files are located in `/etc/systemd/system/` and `/lib/systemd/system/`.

```
[Unit]
Description=My App Service

[Service]
ExecStart=/usr/bin/node app.js
Restart=always

[Install]
WantedBy=multi-user.target
```

> systemctl daemon-reload: Reload systemd manager configuration.

