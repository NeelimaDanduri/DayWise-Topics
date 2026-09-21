# Production Log Warden & Forensic Archive

**Environment:** Ubuntu 26.04 LTS  
**Project:** Linux / DevOps Hands-on Assignment

## 1. Objective

The objective of this assignment was to build a resilient Linux log-monitoring daemon that continuously monitors an application traffic log, detects HTTP 5xx errors and unauthorized access patterns, and automatically creates a forensic incident archive when the configured error threshold is exceeded.

The solution was implemented using **Bash, Linux utilities, systemd, file permissions/ACLs, FIFOs, and `flock`**.

---

## 2. Requirements Implemented

- Continuous monitoring of `/var/log/app_traffic.log`
- Detection of HTTP 5xx responses
- Rolling **60-second** detection window
- Incident trigger when **more than 20 5xx events** occur
- Detection of unauthorized `401/403` requests from a specified user agent
- Forensic snapshot containing:
  - Top memory-consuming processes
  - Top CPU-consuming processes
  - Listening sockets
  - Active network connections
  - Last 500 log lines
- Compressed `.tar.gz` incident archive
- `flock`-based concurrency protection
- Graceful shutdown using signal traps
- Dedicated non-root `warden` service account
- Least-privilege access using ACLs
- systemd service management
- Automatic service restart on failure
- Log rotation/truncation resilience using `tail -F`

---

## 3. Architecture

```text
                    +-----------------------------+
                    | /var/log/app_traffic.log    |
                    +--------------+--------------+
                                   |
                                   v
                    +-----------------------------+
                    |     Log Warden Daemon       |
                    | /usr/local/bin/log_warden.sh|
                    +--------------+--------------+
                                   |
                    +--------------+--------------+
                    |                             |
                    v                             v
             5xx Detection                 401/403 Detection
          Rolling 60-second window       Unauthorized User-Agent
                    |
                    | > 20 events
                    v
             +----------------+
             | Incident Lock  |
             |     flock      |
             +-------+--------+
                     |
                     v
          +-----------------------+
          | Forensic Snapshot     |
          |-----------------------|
          | CPU processes         |
          | Memory processes      |
          | Listening sockets     |
          | Active connections    |
          | Last 500 log lines    |
          +-----------+-----------+
                      |
                      v
          /var/backups/incidents/
                      |
                      v
        incident_YYYYMMDD_HHMMSS.tar.gz
```

---

## 4. Security and Least Privilege

A dedicated system user named `warden` was created:

```text
sudo useradd --system --no-create-home --shell /usr/sbin/nologin warden
```

The daemon runs through systemd as:

```text
User=warden
Group=warden
```

The `warden` user was granted read-only access to the application log using ACL:

```text
sudo setfacl -m u:warden:r /var/log/app_traffic.log
```

The incident backup directory is owned by `warden`:

```text
sudo chown warden:warden /var/backups/incidents
```

The daemon script itself is executable by `warden` without making it writable:

```text
sudo setfacl -m u:warden:rx /usr/local/bin/log_warden.sh
```

This follows the principle of **least privilege** by avoiding execution of the monitoring daemon as root.

---

## 5. Log Monitoring

The daemon uses:

```text
tail -n 0 -F /var/log/app_traffic.log
```

`tail -F` was selected because it continues monitoring the log even when the file is rotated or replaced.

The script extracts the HTTP status code from the log entry and identifies 5xx responses.

Example:

```text
awk '{print $9}'
```

For HTTP 5xx detection:

```text
if [[ "$status" =~ ^5[0-9][0-9]$ ]]; then
```

---

## 6. Rolling 60-Second Detection

Every detected 5xx event is stored with the current Unix timestamp.

Old timestamps are removed when they are more than 60 seconds old.

The active event count is then evaluated:

```text
if (( count > THRESHOLD )); then
    take_snapshot
fi
```

Configuration:

```text
WINDOW=60
THRESHOLD=20
```

Therefore, an incident is generated when the number of detected 5xx events exceeds **20 within the rolling 60-second window**.

---

## 7. Unauthorized Access Detection

The daemon also checks for HTTP `401` and `403` responses.

The configured test user agent is:

```text
BAD_USER_AGENT="Badbot"
```

When a matching unauthorized request is detected, the daemon records a warning containing the HTTP status and user agent.

---

## 8. Incident Forensic Snapshot

When the threshold is exceeded, the daemon creates a temporary incident directory and collects:

### Top Memory Processes

```text
ps aux --sort=-%mem | head -n 6
```

### Top CPU Processes

```text
ps aux --sort=-%cpu | head -n 6
```

### Listening Sockets

```text
ss -tuln
```

### Active Connections

```text
ss -tun
```

### Recent Application Logs

```text
tail -n 500 /var/log/app_traffic.log
```

The collected evidence is compressed into:

```text
/var/backups/incidents/incident_YYYYMMDD_HHMMSS.tar.gz
```

---

## 9. Concurrency Protection

`flock` is used with file descriptor `200`:

```text
exec 200>"$LOCK_FILE"
```

Before creating an incident snapshot:

```text
if ! flock -n 200; then
    echo "Incident snapshot already running."
    echo "Trigger discarded."
    return
fi
```

This ensures that only one incident snapshot is generated at a time.

If another trigger occurs while a snapshot is already running, that trigger is discarded instead of creating multiple simultaneous archives.

---

## 10. Graceful Shutdown

The daemon handles:

```text
SIGTERM
SIGHUP
SIGINT
```

using:

```text
trap cleanup SIGTERM SIGHUP SIGINT
```

The cleanup process:

- Stops the `tail -F` child process
- Waits for the child process
- Removes the FIFO
- Closes the lock file descriptor
- Removes the PID file
- Cleans up runtime artifacts

This allows the daemon to shut down cleanly when stopped by systemd or interrupted manually.

---

## 11. systemd Service

The service is defined at:

```text
/etc/systemd/system/log-warden.service
```

Key configuration:

```text
[Unit]
Description=Production Log Warden
After=network.target

[Service]
Type=simple
User=warden
Group=warden
ExecStart=/usr/local/bin/log_warden.sh
Restart=on-failure
RestartSec=5
RuntimeDirectory=log-warden
RuntimeDirectoryMode=0750
NoNewPrivileges=true
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

The service was enabled to start automatically at boot:

```text
sudo systemctl enable log-warden
```

---

## 12. Testing and Verification

### Service Status

The service successfully reported:

```text
Active: active (running)
```

The service process tree showed:

```text
log_warden.sh
└── tail -n 0 -F /var/log/app_traffic.log
```

### 5xx Flood Test

A test was performed by generating **30 HTTP 502 responses**.

The configured threshold was 20 events within 60 seconds.

The daemon successfully detected the threshold and generated an incident archive.

### Incident Archive Generated

Example:

```text
/var/backups/incidents/incident_20260920_121753.tar.gz
```

The archive was created under the `warden` account:

```text
-rw-r--r-- 1 warden warden ... incident_20260920_121753.tar.gz
```

### Archive Contents

Verified using:

```text
sudo tar -tzf /var/backups/incidents/incident_20260920_121753.tar.gz
```

The archive contained:

```text
incident_20260920_121753/
incident_20260920_121753/app_traffic_tail.txt
incident_20260920_121753/active_connections.txt
incident_20260920_121753/listening_sockets.txt
incident_20260920_121753/top_cpu.txt
incident_20260920_121753/top_memory.txt
```

### Service Restart Test

The service was restarted successfully:

```text
sudo systemctl restart log-warden
```

Status after restart:

```text
Active: active (running)
```

### Process Verification

The running processes were checked using:

```text
pgrep -af log_warden
pgrep -af "tail.*app_traffic"
```

The expected daemon and its monitoring `tail` process were present, with no additional orphan processes observed.

---

## 13. Useful Management Commands

### Check service

```text
systemctl status log-warden
```

### Start service

```text
sudo systemctl start log-warden
```

### Stop service

```text
sudo systemctl stop log-warden
```

### Restart service

```text
sudo systemctl restart log-warden
```

### Enable at boot

```text
sudo systemctl enable log-warden
```

### View service logs

```text
journalctl -u log-warden -n 50 --no-pager
```

### List incident archives

```text
sudo ls -lh /var/backups/incidents/
```

### Inspect an archive

```text
sudo tar -tzf /var/backups/incidents/<archive-name>.tar.gz
```

---

## 14. Final Outcome

The **Production Log Warden & Forensic Archive** assignment was implemented and tested successfully.

The completed solution demonstrates practical experience with:

- Linux process and service management
- Bash scripting
- File permissions and ACLs
- Real-time log monitoring
- Rolling-window event detection
- Network inspection using `ss`
- Process inspection using `ps`
- File locking with `flock`
- Signal handling and cleanup
- systemd service configuration
- Least-privilege security
- Automated forensic evidence collection
- Incident archive creation and verification

The service is configured to run as a dedicated non-root user and automatically restart on failure, providing a foundation for resilient Linux-based log monitoring and incident collection.
