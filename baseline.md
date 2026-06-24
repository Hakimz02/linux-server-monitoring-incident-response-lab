# Server Baseline Check

## Objective
Capture the normal server condition before simulating any incident.

## Date
24 june 2026

## Commands Used

### System uptime

Command:

```bash
uptime
```

Output:

```text
16:45:40 up  1:04,  1 user,  load average: 0.05, 0.11, 0.08
```

Finding:
The server had been running for 1 hour and 4 minutes with low load average, indicating normal server condition during the baseline check.


### CPU and memory usage

Command:
```bash
free -h
```

Output:
```text
               total        used        free      shared  buff/cache   available
Mem:           3.8Gi       1.3Gi       931Mi        36Mi       1.9Gi       2.5Gi
Swap:          3.6Gi          0B       3.6Gi
```

Finding:
The server had 2.5Gi of available memory and no swap usage, indicating normal memory condition during the baseline check.


### Disk usage

Command:
```bash
df -h
```

Output:
```text
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           392M  1.5M  390M   1% /run
/dev/sda2        20G   13G  6.3G  66% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           5.0M  8.0K  5.0M   1% /run/lock
tmpfs           392M  128K  392M   1% /run/user/1000
/dev/sr0         51M   51M     0 100% /media/hakimi/VBox_GAs_7.2.6
```

Finding:
The root filesystem `/` was 66% used with 6.3G available. Disk usage was not critical, but should be monitored because the VM has limited storage capacity. The `/dev/sr0` entry is a mounted VirtualBox Guest Additions ISO and is not part of the main server disk.

### Nginx service status

Command:
```bash
systemctl status nginx
```

Output:
```text
nginx.service - A high performance web server and a reverse proxy server
Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
Active: active (running) since Wed 2026-06-24 15:58:52 +08; 32min ago
Main PID: 1393 (nginx)
Tasks: 4
Memory: 4.3M
CPU: 67ms
```

Finding:
Nginx was active and running. The service was enabled, which means it can start automatically during system boot. Memory and CPU usage were low, indicating normal web server condition during the baseline check.



### HTTP response check

Command:
```bash
curl -I http://localhost
```

Output:
```text
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Wed, 24 Jun 2026 08:33:57 GMT
Content-Type: text/html
Content-Length: 47
Last-Modified: Sun, 07 Jun 2026 06:48:32 GMT
Connection: keep-alive
ETag: "6a251440-2f"
Accept-Ranges: bytes
```

Finding:
The web server returned `HTTP/1.1 200 OK`, confirming that Nginx was running and able to respond successfully to local HTTP requests.


### Listening port check

Command:
```bash
ss -tuln | grep ':80'
```

Output:
```text
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*          
tcp   LISTEN 0      511             [::]:80            [::]:*
```

Finding:
Nginx was listening on port 80 for both IPv4 and IPv6, confirming that the web server was ready to accept HTTP connections.
