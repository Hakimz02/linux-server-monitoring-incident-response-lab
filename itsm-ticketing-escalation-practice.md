# ITSM Ticketing and Escalation Practice

## Purpose

This document contains sample L1/NOC-style incident ticket updates based on the Linux Server Monitoring and Incident Response Lab.

The purpose is to practice writing clear incident notes, troubleshooting updates, resolution summaries, and escalation handoff information.

## Ticket 1: Nginx Service Down

### Ticket Summary

Nginx web service was unreachable from HTTP testing.

### Category

Infrastructure / Web Service

### Impact

Web page was unavailable to users during the simulated outage.

### Priority

Medium

### Symptoms Observed

* HTTP request failed.
* Nginx service was not running.
* Port 80 was not listening.

### Troubleshooting Notes

* Verified web service availability using `curl`.
* Checked Nginx service status using `systemctl status nginx`.
* Verified listening ports using `ss`.
* Identified that the Nginx service was stopped.

### Resolution Notes

Nginx service was started again using `systemctl start nginx`.

Service recovery was verified by confirming HTTP response returned successfully.

### Escalation Criteria

Escalation would be required if:

* Nginx failed to start.
* The same issue occurred repeatedly.
* Error logs showed application or configuration-related failures.
* The issue affected production users beyond L1 support scope.

---

## Ticket 2: Disk Usage Alert

### Ticket Summary

Disk usage increased due to large test log file growth.

### Category

Infrastructure / Storage

### Impact

High disk usage could affect logging, application stability, or service availability.

### Priority

Medium

### Symptoms Observed

* Root filesystem usage increased from baseline.
* `/var/log` showed higher storage usage.
* Large file was found under `/var/log/monitoring-lab`.

### Troubleshooting Notes

* Checked filesystem usage using `df -h`.
* Investigated large directories using `du`.
* Identified the exact large file using file size checks.
* Confirmed the large file was a test log file.

### Resolution Notes

The unnecessary test log file was removed after confirmation.

Disk usage was checked again and returned to the original baseline.

### Escalation Criteria

Escalation would be required if:

* Disk usage remained high after cleanup.
* Large log growth continued repeatedly.
* The file was production-related and could not be removed without approval.
* Log rotation or storage expansion was required.

---

## Ticket 3: High CPU Usage Alert

### Ticket Summary

High CPU usage was detected due to a runaway background process.

### Category

Infrastructure / Performance

### Impact

High CPU usage could cause slow server response or degraded service performance.

### Priority

Medium

### Symptoms Observed

* Server load average increased compared to baseline.
* One process was consuming nearly 100% CPU.
* Process name was identified as `yes`.

### Troubleshooting Notes

* Checked server load using `uptime`.
* Identified top CPU-consuming process using `ps aux --sort=-%cpu`.
* Confirmed the process ID before taking action.
* Verified that the process was not required for normal service operation in this lab.

### Resolution Notes

The runaway process was stopped using `kill`.

Server load was checked again and decreased after remediation.

### Escalation Criteria

Escalation would be required if:

* The high CPU process was unknown or business-critical.
* CPU usage remained high after stopping the process.
* The process restarted automatically.
* Further application, system, or capacity investigation was required.

---

## General L1/NOC Ticketing Workflow

1. Acknowledge the alert or incident.
2. Verify the affected system or service.
3. Record symptoms and impact.
4. Perform basic checks according to SOP.
5. Identify possible root cause.
6. Apply approved remediation if within L1 scope.
7. Verify recovery after action.
8. Escalate with clear handoff notes if unresolved.
9. Document final resolution and closure notes.

## Example Escalation Handoff Format

### Issue

Briefly describe the issue and affected system.

### Impact

Describe who or what is affected.

### Checks Completed

List troubleshooting steps already performed.

### Findings

Summarize what was discovered.

### Action Taken

Mention any remediation already attempted.

### Current Status

State whether the issue is resolved, ongoing, or requires further investigation.

### Request to Next Level Support

Clearly state what help or decision is needed from L2/L3 support.
