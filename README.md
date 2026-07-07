# Linux Server Monitoring and Incident Response Lab

## Overview

This project demonstrates basic Linux server monitoring and incident response using an Ubuntu virtual machine and Nginx web server.

The lab focuses on checking server health, identifying simulated infrastructure issues, finding the root cause, applying remediation, verifying recovery, and documenting the troubleshooting process.

## Lab Objectives

* Capture normal server baseline condition
* Monitor uptime, memory, disk usage, service status, HTTP response, and listening ports
* Simulate common infrastructure incidents
* Troubleshoot issues using Linux commands
* Restore service or system condition after remediation
* Document findings in incident reports

## Tools and Commands Used

* Ubuntu Linux
* Nginx
* `uptime`
* `free -h`
* `df -h`
* `du -h`
* `systemctl`
* `curl`
* `ss`
* `fallocate`
* `rm`
* `ps aux`
* `kill`

## Files Included

* `baseline.md` - Server baseline health check
* `incident-nginx-down.md` - Incident report for simulated Nginx service outage
* `incident-disk-usage-alert.md` - Incident report for simulated disk usage alert caused by large log file growth
* `incident-high-cpu-usage.md` - Incident report for simulated high CPU usage caused by runaway process
* `monitoring-command-cheatsheet.md` - Summary of monitoring and troubleshooting commands

## Incident Reports

### Incident 1: Nginx Service Down

The Nginx service was intentionally stopped to simulate a web server outage.

The issue was detected using `curl`, confirmed through `systemctl`, and verified by checking that port 80 was not listening.

Resolution: The Nginx service was started again and recovery was verified by confirming that the web server returned `HTTP/1.1 200 OK`.

### Incident 2: Disk Usage Alert

A disk usage alert was simulated by creating a large test log file under `/var/log/monitoring-lab`.

The issue was investigated using `df -h` to check filesystem usage and `du -h` to identify the large directory and exact file causing disk growth.

Resolution: The large test log file was removed after confirmation, and disk usage returned from 79% back to the original baseline of 68%.

### Incident 3: High CPU Usage Alert

A high CPU usage alert was simulated by running a background process that consumed nearly 100% CPU.

The issue was investigated using `uptime` to compare server load and `ps aux --sort=-%cpu` to identify the top CPU-consuming process.

Resolution: The runaway process was stopped using `kill`, and server load decreased after remediation.

## Key Learning Outcome

This lab helped practice basic NOC and infrastructure support troubleshooting flows:

```text
Check alert -> Verify system condition -> Identify root cause -> Apply remediation -> Verify recovery -> Document incident
```

Examples practiced:

* Web service outage investigation
* Linux service status checking
* HTTP response validation
* Listening port verification
* Disk usage investigation
* Large file identification and cleanup
* High CPU process investigation
* Runaway process termination
* Incident documentation
