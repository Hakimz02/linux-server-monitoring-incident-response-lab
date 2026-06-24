# Linux Server Monitoring and Incident Response Lab

## Overview
This project demonstrates basic Linux server monitoring and incident response using an Ubuntu virtual machine and Nginx web server.

The lab focuses on checking server health, identifying a simulated web server outage, finding the root cause, restoring the service, and documenting the troubleshooting process.

## Lab Objectives
- Capture normal server baseline condition
- Monitor uptime, memory, disk usage, service status, HTTP response, and listening ports
- Simulate an Nginx service outage
- Troubleshoot the issue using Linux commands
- Restore the Nginx service
- Document findings in an incident report

## Tools and Commands Used
- Ubuntu Linux
- Nginx
- `uptime`
- `free -h`
- `df -h`
- `systemctl`
- `curl`
- `ss`

## Files Included
- `baseline.md` - Server baseline health check
- `incident-nginx-down.md` - Incident report for simulated Nginx service outage
- `monitoring-command-cheatsheet.md` - Summary of monitoring and troubleshooting commands

## Incident Summary
The Nginx service was intentionally stopped to simulate a web server outage. The issue was detected using `curl`, confirmed through `systemctl`, and verified by checking that port 80 was not listening. The issue was resolved by starting the Nginx service again and confirming that the web server returned `HTTP/1.1 200 OK`.

## Key Learning Outcome
This lab helped practice a basic NOC and infrastructure support troubleshooting flow:

```text
Check HTTP response -> Check service status -> Check listening port -> Identify root cause -> Restore service -> Verify recovery
```
