# Incident Report: Nginx Service Down

## Objective
Simulate a web server service outage and document the troubleshooting process.

## Incident Scenario
Nginx service was stopped intentionally to simulate a web server outage.

## Expected Impact
The web server should stop responding to HTTP requests.

## Troubleshooting Steps

### 1. Check HTTP response

Command:
```bash
curl -I http://localhost
```

Output:
```text
curl: (7) Failed to connect to localhost port 80 after 0 ms: Couldn't connect to server
```

Finding:
The web server failed to respond on port 80, indicating a possible Nginx service outage.

### 2. Check Nginx service status

Command:
```bash
systemctl status nginx | grep Active
```

Output:
```text
Active: inactive (dead) since Wed 2026-06-24 23:26:29 +08; 10min ago
```

Finding:
Nginx was inactive, confirming that the web server outage was caused by the Nginx service being stopped.

### 3. Check listening port

Command:
```bash
ss -tuln | grep ':80'
```

Output:
```text
No output
```

Finding:
No service was listening on port 80, confirming that the web server was not accepting HTTP connections.

## Root Cause
The incident was caused by the Nginx service being stopped. Because Nginx was inactive, the server was not listening on port 80 and HTTP requests to `http://localhost` failed.

## Resolution
Started the Nginx service using the following command:

```bash
sudo systemctl start nginx
```

After starting the service, Nginx was active and running again:

```text
Active: active (running) since Wed 2026-06-24 23:49:45 +08; 20s ago
```

## Verification
Verified the web server response after restarting Nginx.

Command:
```bash
curl -I http://localhost
```

Output:
```text
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Wed, 24 Jun 2026 15:53:57 GMT
Content-Type: text/html
Content-Length: 47
Last-Modified: Sun, 07 Jun 2026 06:48:32 GMT
Connection: keep-alive
ETag: "6a251440-2f"
Accept-Ranges: bytes
```

Finding:
The web server returned `HTTP/1.1 200 OK`, confirming that the Nginx service was restored successfully.

## Lesson Learned
When a web server is unreachable, checking only the service status is not enough. A proper troubleshooting flow should include HTTP response testing, service status verification, and listening port checks. In this incident, `curl`, `systemctl`, and `ss` helped confirm that the outage was caused by Nginx being stopped.
