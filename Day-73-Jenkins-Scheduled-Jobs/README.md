# Day 73: Jenkins Scheduled Jobs 🔄

## Task Overview
Created a Jenkins job to automatically copy Apache logs from App Server to Storage Server every 5 minutes for centralized log management.

## What I Built
**Job Name:** `copy-logs`  
**Schedule:** Every 5 minutes (`*/5 * * * *`)  
**Source:** Apache logs from App Server 1 (stapp01)  
**Destination:** Storage Server (ststor01) at `/usr/src/data/`

## Implementation

### 1. Setup
- Installed **Publish Over SSH** plugin
- Configured SSH credentials for both servers

### 2. Build Configuration
- **Trigger:** Build periodically with cron expression `*/5 * * * *`
- **Build Step:** Single Execute Shell script

### 3. Final Script

```bash
#!/bin/bash

# Setup SSH
mkdir -p ~/.ssh
chmod 700 ~/.ssh

# Accept host keys
ssh-keyscan -H stapp01 >> ~/.ssh/known_hosts 2>/dev/null || true
ssh-keyscan -H ststor01 >> ~/.ssh/known_hosts 2>/dev/null || true

# Credentials
APP_PASS="Ir0nM@n"
STORAGE_PASS="Bl@kW"

# Copy logs from App Server
echo "Copying logs from App Server..."
sshpass -p "$APP_PASS" ssh -o StrictHostKeyChecking=no tony@stapp01 \
  "echo $APP_PASS | sudo -S cat /var/log/httpd/access_log" > /tmp/access_log

sshpass -p "$APP_PASS" ssh -o StrictHostKeyChecking=no tony@stapp01 \
  "echo $APP_PASS | sudo -S cat /var/log/httpd/error_log" > /tmp/error_log

# Transfer to Storage Server
echo "Transferring to storage server..."
sshpass -p "$STORAGE_PASS" scp -o StrictHostKeyChecking=no \
  /tmp/access_log natasha@ststor01:/usr/src/data/

sshpass -p "$STORAGE_PASS" scp -o StrictHostKeyChecking=no \
  /tmp/error_log natasha@ststor01:/usr/src/data/

echo "SUCCESS: Logs copied!"
```

## Key Learnings

✅ Jenkins cron expressions for scheduled builds  
✅ SSH automation with sshpass  
✅ Handling sudo passwords in remote commands  
✅ Proper SSH host key management  
✅ Debugging Jenkins console output

## Result
Job runs successfully every 5 minutes, copying both access_log and error_log to the storage server.

## Tools Used
- Jenkins (Freestyle Job)
- Bash Scripting
- SSH/SCP
- sshpass

---
# Day 73: Jenkins Scheduled Jobs 🔄

## Task Overview
Created a Jenkins job to automatically copy Apache logs from App Server to Storage Server every 5 minutes for centralized log management.

## What I Built
**Job Name:** `copy-logs`  
**Schedule:** Every 5 minutes (`*/5 * * * *`)  
**Source:** Apache logs from App Server 1 (stapp01)  
**Destination:** Storage Server (ststor01) at `/usr/src/data/`

## Implementation

### 1. Setup
- Installed **Publish Over SSH** plugin
- Configured SSH credentials for both servers

### 2. Build Configuration
- **Trigger:** Build periodically with cron expression `*/5 * * * *`
- **Build Step:** Single Execute Shell script

### 3. Final Script

```bash
#!/bin/bash

# Setup SSH
mkdir -p ~/.ssh
chmod 700 ~/.ssh

# Accept host keys
ssh-keyscan -H stapp01 >> ~/.ssh/known_hosts 2>/dev/null || true
ssh-keyscan -H ststor01 >> ~/.ssh/known_hosts 2>/dev/null || true

# Credentials
APP_PASS="Ir0nM@n"
STORAGE_PASS="Bl@kW"

# Copy logs from App Server
echo "Copying logs from App Server..."
sshpass -p "$APP_PASS" ssh -o StrictHostKeyChecking=no tony@stapp01 \
  "echo $APP_PASS | sudo -S cat /var/log/httpd/access_log" > /tmp/access_log

sshpass -p "$APP_PASS" ssh -o StrictHostKeyChecking=no tony@stapp01 \
  "echo $APP_PASS | sudo -S cat /var/log/httpd/error_log" > /tmp/error_log

# Transfer to Storage Server
echo "Transferring to storage server..."
sshpass -p "$STORAGE_PASS" scp -o StrictHostKeyChecking=no \
  /tmp/access_log natasha@ststor01:/usr/src/data/

sshpass -p "$STORAGE_PASS" scp -o StrictHostKeyChecking=no \
  /tmp/error_log natasha@ststor01:/usr/src/data/

echo "SUCCESS: Logs copied!"
```

## Key Learnings

✅ Jenkins cron expressions for scheduled builds  
✅ SSH automation with sshpass  
✅ Handling sudo passwords in remote commands  
✅ Proper SSH host key management  
✅ Debugging Jenkins console output

## Result
Job runs successfully every 5 minutes, copying both access_log and error_log to the storage server.

## Tools Used
- Jenkins (Freestyle Job)
- Bash Scripting
- SSH/SCP
- sshpass

---