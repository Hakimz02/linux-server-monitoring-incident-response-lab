# Incident 3: High CPU Usage Alert Caused by Runaway Process

## Incident Summary

A high CPU usage alert was simulated on an Ubuntu Linux server by running a background process that consumed high CPU resources.

The purpose of this incident was to practice a typical L1/NOC troubleshooting workflow:

* Check baseline server load
* Simulate high CPU usage
* Identify the top CPU-consuming process
* Confirm the root cause
* Stop the runaway process
* Verify that server load returned closer to normal

## Environment

* Operating System: Ubuntu Linux
* Server Type: Local VirtualBox VM
* Incident Type: High CPU usage alert
* Simulated Cause: Runaway `yes` process

## Baseline CPU / Load Check

### Command

```bash
uptime
```

### Output

```bash
23:37:51 up 23 min,  1 user,  load average: 0.00, 0.02, 0.06
```

### Finding

The server was idle before the incident simulation.

The baseline load average was low, which indicated normal system condition before the high CPU process was started.

## Simulated Alert Condition

A background process was started to simulate a runaway process consuming CPU resources.

### Command

```bash
yes > /dev/null & echo $! > /tmp/high-cpu-test.pid && cat /tmp/high-cpu-test.pid
```

### Output

```bash
[1] 3542
3542
```

### Finding

A background `yes` process was started with PID `3542`.

This process was used to simulate high CPU usage on the server.

## Top CPU Process Investigation

### Command

```bash
ps aux --sort=-%cpu | head -5
```

### Output

```bash
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
hakimi      3542 99.9  0.0  16964  2096 pts/0    R    23:38   0:34 yes
hakimi      2307  1.7  7.4 4168624 298740 ?      Ssl  23:14   0:26 /usr/bin/gnome-shell
hakimi      3176  0.2  1.4 633788 57536 ?        Ssl  23:14   0:04 /usr/libexec/gnome-terminal-server
hakimi      2559  0.2  0.7 429816 29856 ?        Sl   23:14   0:03 /usr/libexec/ibus-extension-gtk3
```

### Finding

The process `yes` with PID `3542` was consuming approximately 99.9% CPU.

This confirmed the suspected high CPU process.

## Server Load During Incident

### Command

```bash
uptime
```

### Output

```bash
23:39:27 up 25 min,  1 user,  load average: 0.66, 0.21, 0.12
```

### Finding

The load average increased compared to the baseline.

This confirmed that the simulated high CPU process affected server load.

## Root Cause

The high CPU usage was caused by the background `yes` process:

```bash
yes > /dev/null
```

The process was running with PID `3542` and consuming nearly 100% CPU.

## Remediation Action

### Command

```bash
kill $(cat /tmp/high-cpu-test.pid)
```

### Finding

The high CPU process was stopped using the saved PID.

## Process Termination Verification

### Command

```bash
ps -p $(cat /tmp/high-cpu-test.pid) -o pid,%cpu,comm
```

### Output

```bash
    PID %CPU COMMAND
[1]+  Terminated              yes > /dev/null
```

### Finding

The process was no longer running after the remediation action.

This confirmed that the runaway CPU process had been stopped successfully.

## Post-Remediation Verification

### Command

```bash
uptime
```

### Output

```bash
23:41:38 up 27 min,  1 user,  load average: 0.13, 0.19, 0.13
```

### Finding

The load average decreased after the high CPU process was stopped.

This confirmed that the remediation was successful.

## Cleanup

### Command

```bash
rm /tmp/high-cpu-test.pid
```

### Finding

The temporary PID file was removed after the incident was resolved.

## Resolution

The runaway `yes` process was identified using `ps`, stopped using `kill`, and the server load was verified again using `uptime`.

## Lessons Learned

* High CPU alerts should be verified by checking server load and running processes.
* `uptime` is useful for checking server load average before and after an incident.
* `ps aux --sort=-%cpu` helps identify processes consuming the most CPU.
* The exact PID should be confirmed before stopping a process.
* After remediation, CPU/load should be checked again to confirm recovery.
* In a real production environment, suspicious or unknown high CPU processes should be escalated before termination unless approved by SOP.
