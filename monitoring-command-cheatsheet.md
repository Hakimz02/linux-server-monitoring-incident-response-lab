# Monitoring Command Cheat Sheet

## Purpose
This note summarizes the basic Linux monitoring and troubleshooting commands used in this lab.

## Commands

### uptime
Used to check how long the server has been running and view the system load average.

Example:
```bash
uptime
```

Use case:
Helps identify whether the server recently restarted and whether the system load is normal or high.

---

### free -h
Used to check memory and swap usage.

Example:
```bash
free -h
```

Use case:
Helps identify whether the server has enough available memory or if swap is being used.

---

### df -h
Used to check disk usage.

Example:
```bash
df -h
```

Use case:
Helps identify whether the server storage is almost full. Full disk usage can cause service issues, logging failures, or application instability.

---

### systemctl status nginx
Used to check the status of the Nginx service.

Example:
```bash
systemctl status nginx
```

Use case:
Helps confirm whether the web server service is active, inactive, stopped, or failed.

---

### curl -I http://localhost
Used to test whether the web server responds to HTTP requests.

Example:
```bash
curl -I http://localhost
```

Use case:
Helps verify whether the web server is responding successfully. A `200 OK` response means the web server is reachable.

---

### ss -tuln | grep ':80'
Used to check whether any service is listening on port 80.

Example:
```bash
ss -tuln | grep ':80'
```

Use case:
Helps confirm whether the server is accepting HTTP connections on port 80.

---

## Troubleshooting Flow

When a web server is unreachable:

1. Test HTTP response using `curl`
2. Check service status using `systemctl`
3. Check listening port using `ss`
4. Identify root cause
5. Apply resolution
6. Verify service recovery


## Disk Usage Investigation Commands

### df -h /

Used to check disk usage for the root filesystem.

Example:
```bash
df -h /
```

Use case:
Helps identify whether the main filesystem is running low on available disk space.

---

### du

Used to check which directories or files are consuming disk space.

Example:
```bash
sudo du -xh --max-depth=2 /var/log 2>/dev/null | sort -h | tail -10
```

Use case:
Helps narrow down large directories during disk usage alerts.

---

### ls -lh

Used to check file details such as size, owner, permissions, and modified date.

Example:
```bash
sudo ls -lh /var/log/monitoring-lab/application-test.log
```

Use case:
Helps confirm the exact large file before taking action.

---

### rm

Used to remove unnecessary files.

Example:
```bash
sudo rm /var/log/monitoring-lab/application-test.log
```

Use case:
Used only after confirming the file is safe to remove.
