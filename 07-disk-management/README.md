# Disk Management

Disk Management means

- Identifying disks and partitions
- Creating / Deleting partitions
- Formatitng disks with filesystem
- Mounting and Unmounting storage
- Monitoring disk usage and health

Disks and Partitions are represented as special files under `/dev/` called as block devices.

## why `/dev`?

- `/dev` = device files
- Linux treats everything as a files, including hardware.
- Disks, USBs, CDs, NVMe, cloud volumes → all appear here

## Traditional Disk Naming

### Format

`/dev/sdx`

| name | Meaning                            |
| ---- | ---------------------------------- |
| sd   | SCSI disk (used for SATA, USB too) |
| x    | Letter: a, b, c, ...(disk order)   |

**Example**

| Device     | Meaning     |
| ---------- | ----------- |
| `/dev/sda` | First disk  |
| `/dev/sdb` | Second disk |
| `/dev/sdc` | Third disk  |

### Partitions

`/dev/sda1`

| Component | Meaning          |
| --------- | ---------------- |
| `sda`     | Disk             |
| `1`, `2`  | Partition number |

## NVMe Disk Naming (Modern Systems)

Controller + Namespace

`/dev/nvmeXnX`

| Part    | Meaning          |
| ------- | ---------------- |
| `nvme0` | NVMe controller  |
| `n1`    | Namespace (disk) |
| `p1`    | Partition        |

**Example**

| Device           | Meaning                   |
| ---------------- | ------------------------- |
| `/dev/nvme0n1`   | Controller 0, Namespace 1 |
| `/dev/nvme0n1p1` | Partition 1               |

## Virtual Disk

Common in cloud environments or virtual machines (KVM/QEMU).

`vdX`

- `vda`
- `vdb`

## Persistent Naming (The "UUID" Method)

UUID - Universally Unique Identifier

Traditional disk naming are dynamic. If you unplug or rebot or change a BIOS setting, `sda` might become `sdb`. To prevent system crashes, Linux uses Persistent Naming found in `/dev/disk/`.

- by-label: Human-readable labels (e.g., "Data").
- by-uuid: Filesystem UUID (focus of this explanation).
- by-id: Hardware-based (e.g., WWID or serial number).
- by-path: PCI/path-based (less reliable).
- by-partuuid / by-partlabel (for GPT partitions)

## Identifying Disks and Partitions

- `lsblk` - List block devices
- `fdisk -l` - List all disk partition and provides detailed information about disk size and sector counts.
- `blkid` – Show UUIDs of devices
- `df -h` – Check disk space usage
- `du -sh /path` – Show size of a directory
