# Incident 2: Disk Usage Alert Caused by Large Log File Growth

## Incident Summary

A disk usage alert was simulated on an Ubuntu Linux server by creating a large test log file under `/var/log/monitoring-lab`.

The purpose of this incident was to practice a typical L1/NOC troubleshooting workflow:

* Check current disk usage
* Identify which directory is consuming disk space
* Locate the exact large file
* Remove the unnecessary large file
* Verify disk usage returned to normal

## Environment

* Operating System: Ubuntu Linux
* Server Type: Local VirtualBox VM
* Filesystem Checked: `/`
* Incident Type: Disk usage alert
* Simulated Cause: Large test log file growth

## Initial Disk Usage Check

### Command

```bash
df -h /
```

### Output

```bash
/dev/sda2        20G   13G  6.0G  68% /
```

### Finding

The root filesystem `/` was using 68% of available disk space before the incident simulation.

This was used as the baseline disk usage before creating the large test log file.

## Simulated Alert Condition

A large test log file was created under `/var/log/monitoring-lab` to simulate log file growth.

### Command

```bash
sudo mkdir -p /var/log/monitoring-lab
sudo fallocate -l 2G /var/log/monitoring-lab/application-test.log
```

### Finding

A 2GB test log file was created to simulate a disk usage increase caused by log growth.

## Disk Usage After Simulated Log Growth

### Command

```bash
df -h /
```

### Output

```bash
/dev/sda2        20G   15G  4.0G  79% /
```

### Finding

Disk usage increased from 68% to 79% after the large test log file was created.

This confirmed that the simulated log file growth caused higher disk usage on the root filesystem.

## Directory Investigation

### Command

```bash
sudo du -xh --max-depth=2 /var/log 2>/dev/null | sort -h | tail -10
```

### Output

```bash
52K     /var/log/installer/block
60K     /var/log/unattended-upgrades
140K    /var/log/sysstat
200K    /var/log/apt
472K    /var/log/atop
1.2M    /var/log/installer
449M    /var/log/journal
449M    /var/log/journal/27b4a3ceaf584ff6b6dfcbbd6ff258d5
2.1G    /var/log/monitoring-lab
2.5G    /var/log
```

### Finding

The `/var/log/monitoring-lab` directory was identified as one of the largest directories under `/var/log`.

This indicated that the disk usage increase was related to files inside `/var/log/monitoring-lab`.

## Exact Large File Identification

### Command

```bash
sudo du -h /var/log/monitoring-lab/*
```

### Output

```bash
2.1G    /var/log/monitoring-lab/application-test.log
```

### Finding

The exact large file was identified as:

```bash
/var/log/monitoring-lab/application-test.log
```

The file was consuming approximately 2.1GB of disk space.

## File Detail Verification

### Command

```bash
sudo ls -lh /var/log/monitoring-lab/application-test.log
```

### Output

```bash
-rw-r--r-- 1 root root 2.0G Jun 25 15:50 /var/log/monitoring-lab/application-test.log
```

### Finding

The large file was confirmed to be a 2.0GB file owned by `root`.

This confirmed the root cause of the disk usage increase.

## Remediation Action

### Command

```bash
sudo rm /var/log/monitoring-lab/application-test.log
```

### Finding

The unnecessary large test log file was removed after it was confirmed as the cause of the disk usage increase.

## Post-Remediation Verification

### Command

```bash
df -h /
```

### Output

```bash
/dev/sda2        20G   13G  6.0G  68% /
```

### Finding

Disk usage returned from 79% back to 68% after removing the large test log file.

This confirmed that the remediation was successful.

## File Removal Verification

### Command

```bash
sudo ls -lh /var/log/monitoring-lab/
```

### Output

```bash
total 0
```

### Finding

The `/var/log/monitoring-lab` directory was empty after remediation.

The large test log file was successfully removed.

## Root Cause

The disk usage increase was caused by a large test log file created under:

```bash
/var/log/monitoring-lab/application-test.log
```

The file consumed approximately 2GB of disk space and caused the root filesystem usage to increase from 68% to 79%.

## Resolution

The large test log file was removed using `rm`.

After the file was removed, disk usage returned to the original baseline of 68%.

## Lessons Learned

* Disk usage alerts should be verified first using `df -h` to identify which filesystem is affected.
* `du` is useful to narrow down which directory or file is consuming the most disk space.
* Large log files can cause disk usage to increase and may affect application logging, services, or system stability.
* The exact file path and size should be confirmed before removing any file.
* After remediation, disk usage should be checked again to confirm that the issue has been resolved.
* In a real production environment, repeated log growth should be escalated for log rotation, application log review, or storage planning.
