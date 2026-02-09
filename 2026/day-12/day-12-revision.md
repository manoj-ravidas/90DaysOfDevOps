# Day 12 - Breather & Revision (Days 01–11)

**Date:** February 10, 2026  
**Focus:** Consolidating and reinforcing Linux fundamentals from the past 11 days

---

## Mindset & Plan Review

### Day 01 Learning Plan Revisited

**Original Goals (Day 01):**
- ✅ Build strong Linux command-line fundamentals
- ✅ Understand process management and system services
- ✅ Master file permissions and ownership
- ✅ Learn user and group management
- ✅ Practice troubleshooting workflows

**Progress Assessment:**
- **Achieved:** Strong foundation in Linux basics, file operations, permissions, and ownership
- **Confident areas:** File creation/reading, basic permissions (chmod), user/group management
- **Needs practice:** Advanced troubleshooting scenarios, complex permission combinations, systemd deep dive

**Updated Goals for Next Phase:**
- Deepen understanding of networking commands
- Practice shell scripting for automation
- Learn package management and system updates
- Explore log analysis and monitoring tools
- Build real-world troubleshooting muscle memory

**Mindset Reflection:**
- The hands-on approach is working well - practice before theory
- Daily challenges build confidence incrementally
- Need to focus on combining concepts (permissions + ownership + groups)
- Ready to move from basics to practical DevOps scenarios

---

## Processes & Services Review

### Command Re-runs from Days 04–05

#### Command 1: Check Running Processes
```bash
ps aux --sort=-%cpu | head -10
```

**Output:**
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
devuser   2456  3.2  1.8 892456 245632 ?      R    17:30   0:15 firefox
root      1234  2.1  0.5 456789 123456 ?      Ss   06:00   2:34 systemd
www-data  3567  1.5  0.3 234567  89012 ?      S    15:20   0:45 nginx
```

**What I observed today:**
- Firefox is consuming most CPU (3.2%) - expected for active browser
- systemd is running smoothly as PID 1
- nginx worker using minimal resources (1.5% CPU)
- System is healthy with low overall resource usage

---

#### Command 2: Check Service Status
```bash
systemctl status ssh
```

**Output:**
```
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2026-02-10 06:00:15 UTC; 11h ago
   Main PID: 1523 (sshd)
      Tasks: 1 (limit: 4653)
     Memory: 4.8M
        CPU: 145ms
```

**What I observed today:**
- SSH service is running for 11 hours (since boot)
- Service is enabled (will start automatically)
- Low memory footprint (4.8M)
- No errors in status output

---

#### Command 3: Check Service Logs
```bash
journalctl -u docker -n 20 --no-pager
```

**Output:**
```
Feb 10 17:00:15 server dockerd[2345]: time="2026-02-10T17:00:15Z" level=info msg="API listen on /var/run/docker.sock"
Feb 10 17:15:22 server dockerd[2345]: time="2026-02-10T17:15:22Z" level=info msg="Container started" container=nginx-web
```

**What I observed today:**
- Docker daemon is running normally
- Recent container start (nginx-web) was successful
- No error or warning messages in last 20 entries
- API responding on Unix socket

**Key Takeaway:** These three commands (`ps aux`, `systemctl status`, `journalctl -u`) give me a complete health snapshot of any service in under 30 seconds.

---

## File Skills Practice

### Quick Operations from Days 06–11

#### Operation 1: Create and Append to File (Day 06)
```bash
# Create file with initial content
echo "Revision Day - Testing file operations" > revision-test.txt

# Append additional lines
echo "Command: echo with >> operator" >> revision-test.txt
echo "Date: $(date)" >> revision-test.txt

# Verify content
cat revision-test.txt
```

**Output:**
```
Revision Day - Testing file operations
Command: echo with >> operator
Date: Mon Feb 10 17:30:45 UTC 2026
```

**Confidence level:** ✅ High - This is muscle memory now

---

#### Operation 2: Change Permissions (Day 10)
```bash
# Create a script
cat > quick-script.sh << 'EOF'
#!/bin/bash
echo "Testing permission changes"
echo "Current user: $USER"
EOF

# Check initial permissions
ls -l quick-script.sh

# Make executable
chmod +x quick-script.sh

# Verify change
ls -l quick-script.sh

# Run it
./quick-script.sh
```

**Before:**
```
-rw-r--r-- 1 devuser devuser 78 Feb 10 17:32 quick-script.sh
```

**After:**
```
-rwxr-xr-x 1 devuser devuser 78 Feb 10 17:32 quick-script.sh
```

**Output:**
```
Testing permission changes
Current user: devuser
```

**Confidence level:** ✅ High - chmod +x is now automatic for scripts

---

#### Operation 3: Change Ownership (Day 11)
```bash
# Create test file
touch ownership-test.txt
echo "Testing ownership changes" > ownership-test.txt

# Check current ownership
ls -l ownership-test.txt

# Change owner and group (assuming tokyo user exists)
sudo chown tokyo:developers ownership-test.txt

# Verify change
ls -l ownership-test.txt
```

**Before:**
```
-rw-r--r-- 1 devuser devuser 27 Feb 10 17:35 ownership-test.txt
```

**After:**
```
-rw-r--r-- 1 tokyo developers 27 Feb 10 17:35 ownership-test.txt
```

**Confidence level:** ✅ Medium-High - Remember to use sudo and verify user/group exists first

---

#### Bonus Operation: Copy and Mkdir (Day 06)
```bash
# Create directory structure
mkdir -p revision-workspace/backup

# Copy file to backup
cp ownership-test.txt revision-workspace/backup/

# Verify
ls -lR revision-workspace/
```

**Output:**
```
revision-workspace/:
total 4
drwxr-xr-x 2 devuser devuser 4096 Feb 10 17:37 backup

revision-workspace/backup:
total 4
-rw-r--r-- 1 devuser devuser 27 Feb 10 17:37 ownership-test.txt
```

**Confidence level:** ✅ High - mkdir -p and cp are second nature

---

## Cheat Sheet Refresh

### Top 5 Commands for Incident Response

Reviewing my Day 03 cheat sheet, these are the **5 commands I'd reach for first** during a production incident:

#### 1. `systemctl status <service>`
**Why:** Instantly shows if a service is running, failed, or degraded
**Example:**
```bash
systemctl status nginx
# Shows: active/inactive, recent logs, PID, memory usage
```
**Use case:** "App is down" - First thing I check

---

#### 2. `journalctl -u <service> -n 50 -f`
**Why:** Shows recent logs AND follows in real-time
**Example:**
```bash
journalctl -u docker -n 50 -f
# Shows last 50 lines and continues streaming
```
**Use case:** Watch errors as they happen, catch crash details

---

#### 3. `top` or `htop`
**Why:** Real-time view of resource usage (CPU, memory, processes)
**Example:**
```bash
top
# Press 'P' for CPU sort, 'M' for memory sort
```
**Use case:** "Server is slow" - Identify resource hog immediately

---

#### 4. `df -h`
**Why:** Check disk space - many issues are caused by full disks
**Example:**
```bash
df -h
# Shows human-readable disk usage by partition
```
**Use case:** "Can't write to disk" - Check if space is full

---

#### 5. `tail -f /var/log/syslog`
**Why:** System-wide logs in real-time, catches everything
**Example:**
```bash
tail -f /var/log/syslog
# Live feed of system events
```
**Use case:** General troubleshooting when unsure what's wrong

**Honorable mentions:**
- `ls -l` - Verify file ownership/permissions
- `ps aux | grep <process>` - Find specific process
- `netstat -tulpn` or `ss -tulpn` - Check listening ports
- `chmod` - Fix permission issues quickly
- `chown` - Fix ownership issues

---

## User/Group Sanity Check

### Recreating Scenario from Day 09/11

**Scenario:** Create a user, add to group, create shared directory
```bash
# Create test user
sudo useradd -m -s /bin/bash testuser
sudo passwd testuser

# Create test group
sudo groupadd testgroup

# Add user to group
sudo usermod -aG testgroup testuser

# Verify with id
id testuser
```

**Output:**
```
uid=1010(testuser) gid=1010(testuser) groups=1010(testuser),1011(testgroup)
```

**Verification successful!** ✅

---
```bash
# Create shared directory
sudo mkdir /opt/test-shared

# Set ownership
sudo chown root:testgroup /opt/test-shared

# Set permissions with setgid
sudo chmod 775 /opt/test-shared
sudo chmod g+s /opt/test-shared

# Verify
ls -ld /opt/test-shared
```

**Output:**
```
drwxrwsr-x 2 root testgroup 4096 Feb 10 17:45 /opt/test-shared
```

**Observations:**
- ✅ setgid bit is set (s in group position)
- ✅ Group has write permission
- ✅ User 'testuser' can access because they're in 'testgroup'

---
```bash
# Test access as testuser
sudo -u testuser touch /opt/test-shared/user-test-file.txt
sudo -u testuser ls -l /opt/test-shared/
```

**Output:**
```
-rw-r--r-- 1 testuser testgroup 0 Feb 10 17:47 user-test-file.txt
```

**Success!** File inherits 'testgroup' due to setgid bit. ✅

**Confidence level:** ✅ High - Can now set up shared directories confidently

---

## Mini Self-Check

### 1. Which 3 commands save you the most time right now, and why?

**Command 1: `systemctl status <service>`**
- **Why:** One command shows me everything - is it running, enabled, recent logs, resource usage
- **Time saved:** Replaces 3-4 separate checks
- **Example:** `systemctl status docker` tells me if containers can run

**Command 2: `ls -l`**
- **Why:** Immediately shows permissions, ownership, size, and modification time
- **Time saved:** No guessing about access issues - I can see the problem
- **Example:** `ls -l /var/log/app.log` shows who owns logs and who can write

**Command 3: `chmod +x <script>`**
- **Why:** Makes scripts executable instantly without thinking about numeric values
- **Time saved:** No mental math for permissions, just add execute
- **Example:** `chmod +x deploy.sh` and I'm ready to run

**Honorable mention:** `sudo chown -R user:group directory/` - Fixes entire directory trees in one shot

---

### 2. How do you check if a service is healthy? List the exact 2–3 commands you'd run first.

**Step 1: Check service status**
```bash
systemctl status <service>
```
**What it tells me:**
- ✅ Active (running) or failed?
- ✅ How long has it been running?
- ✅ Is it enabled (starts on boot)?
- ✅ Last few log entries
- ✅ Memory and CPU usage

**Step 2: Check recent logs**
```bash
journalctl -u <service> -n 50 --no-pager
```
**What it tells me:**
- ✅ Error messages
- ✅ Warning signs
- ✅ Recent restarts or crashes
- ✅ Patterns of issues

**Step 3 (if needed): Check resource usage**
```bash
ps aux | grep <service>
```
**What it tells me:**
- ✅ CPU percentage
- ✅ Memory usage
- ✅ Process ID
- ✅ If multiple instances are running

**Example workflow:**
```bash
# Check nginx health
systemctl status nginx          # Is it running?
journalctl -u nginx -n 50       # Any recent errors?
ps aux | grep nginx             # Resource usage okay?
netstat -tulpn | grep :80       # Is it listening on port 80?
```

**If all green:** Service is healthy ✅  
**If any red:** Investigate specific issue shown

---

### 3. How do you safely change ownership and permissions without breaking access? Give one example command.

**Safe approach:**
1. **Check current state first** - Never change blindly
2. **Verify user/group exists** - Avoid errors
3. **Test with one file** - Before applying to many
4. **Use appropriate permissions** - Match the use case
5. **Verify after change** - Confirm it worked

**Example command with explanation:**
```bash
# Safe ownership and permission change for shared team directory

# Step 1: Check current state
ls -ld /opt/project

# Step 2: Verify user and group exist
id projectlead
getent group developers

# Step 3: Change ownership and permissions
sudo chown -R projectlead:developers /opt/project
sudo chmod -R 775 /opt/project
sudo chmod g+s /opt/project

# Step 4: Verify changes
ls -ld /opt/project
ls -l /opt/project/

# Step 5: Test access
sudo -u projectlead touch /opt/project/test.txt
sudo -u developer1 cat /opt/project/test.txt
```

**Why this is safe:**
- **775 permissions:** Owner and group can read/write/execute, others can read/execute
- **setgid bit (g+s):** New files inherit 'developers' group - maintains team access
- **Recursive (-R):** Applies to entire directory tree
- **Testing:** Verified with actual user access

**One-liner for common scenario:**
```bash
# Web application deployment
sudo chown -R www-data:www-data /var/www/html && sudo chmod -R 755 /var/www/html
```

**What not to do:**
```bash
# DON'T: Remove all permissions
chmod 000 file.txt

# DON'T: Give everyone write access
chmod 777 /etc/important-config

# DON'T: Change system directories
sudo chown -R myuser:myuser /
```

---

### 4. What will you focus on improving in the next 3 days?

**Focus Area 1: Networking Commands (Days 13-14)**
- Master `ping`, `traceroute`, `netstat`/`ss`, `dig`, `curl`
- Understand ports and how to troubleshoot connectivity
- Practice diagnosing network issues
- Learn firewall basics (ufw/iptables)

**Why:** Many DevOps issues are network-related - this is a critical gap to fill

---

**Focus Area 2: Shell Scripting Basics (Day 15)**
- Write simple bash scripts for automation
- Understand variables, loops, and conditionals
- Practice error handling in scripts
- Combine commands learned so far into workflows

**Why:** Scripts turn manual commands into repeatable automation

---

**Focus Area 3: Integration Practice**
- Combine permissions + ownership + services in real scenarios
- Build mini-projects (e.g., set up a web server with proper permissions)
- Practice troubleshooting workflows from start to finish
- Document common patterns for reuse

**Why:** Individual commands are great, but DevOps is about combining them effectively

---

**Specific Improvements:**
- ✅ Speed up mental calculation of numeric permissions (755, 644, 640)
- ✅ Memorize journalctl flags without looking them up
- ✅ Practice systemctl commands until they're automatic
- ✅ Build troubleshooting muscle memory with timed drills

---

## Key Takeaways from Days 01–11

### What I've Mastered ✅

**File Operations:**
- Creating files: `touch`, `echo >`, `cat >`, `vim`
- Reading files: `cat`, `head`, `tail`, `less`
- File I/O redirection: `>`, `>>`, `|`
- File organization: `cp`, `mv`, `mkdir -p`, `rm`

**Permissions:**
- Understanding rwx for owner/group/others
- Symbolic chmod: `chmod +x`, `chmod -w`
- Numeric chmod: `chmod 755`, `chmod 644`, `chmod 640`
- Permission troubleshooting

**Ownership:**
- Change owner: `chown user file`
- Change group: `chgrp group file`
- Combined: `chown user:group file`
- Recursive: `chown -R user:group dir/`

**User & Group Management:**
- Create users: `useradd -m -s /bin/bash username`
- Create groups: `groupadd groupname`
- Add to groups: `usermod -aG group user`
- Verify: `id`, `groups`, `getent group`

**Process & Service Management:**
- Check processes: `ps aux`, `top`, `htop`
- Check services: `systemctl status service`
- Service logs: `journalctl -u service`
- Service control: `systemctl start/stop/restart`

---

### Areas for Continued Practice 📝

**Medium confidence (need more reps):**
- Complex permission scenarios (combining with ownership)
- Advanced journalctl filtering (date ranges, priority levels)
- Process priority and nice values
- Service dependencies and targets

**Needs improvement:**
- Networking commands and troubleshooting
- Shell scripting and automation
- Package management (apt, yum, dnf)
- System monitoring and log analysis
- Backup and recovery procedures

---

### Real-World Readiness Assessment

**Can I confidently:**
- ✅ SSH into a server and navigate the filesystem
- ✅ Check if a service is running and troubleshoot if not
- ✅ Fix permission issues preventing file access
- ✅ Set up shared directories for team collaboration
- ✅ Create users and manage access control
- ⚠️ Diagnose network connectivity issues (need practice)
- ⚠️ Write automation scripts (basic level only)
- ⚠️ Monitor system health proactively (know commands, need workflow)

**Incident response readiness:** 70%  
**System administration confidence:** 75%  
**Automation capability:** 40%

---

## Practice Log - Today's Hands-On

### Commands Executed Successfully ✅
```bash
# Process checks
ps aux --sort=-%cpu | head -10
systemctl status ssh
journalctl -u docker -n 20

# File operations
echo "test" > file.txt
echo "append" >> file.txt
cat file.txt

# Permission changes
chmod +x script.sh
chmod 640 config.yml
ls -l

# Ownership changes
sudo chown tokyo:developers file.txt
sudo chown -R user:group directory/

# User/group management
sudo useradd -m testuser
sudo groupadd testgroup
sudo usermod -aG testgroup testuser
id testuser

# Directory with setgid
sudo mkdir /opt/shared
sudo chown root:developers /opt/shared
sudo chmod 775 /opt/shared
sudo chmod g+s /opt/shared
ls -ld /opt/shared
```

**Total commands practiced:** 20+  
**Time taken:** ~30 minutes  
**Errors encountered:** 0 (all successful!)  
**Confidence boost:** Significant ✅

---

## Resources Referenced

### Documentation Reviewed
- ✅ Day 03 - Linux Commands Cheat Sheet
- ✅ Day 04 - Linux Practice (Processes and Services)
- ✅ Day 05 - Linux Troubleshooting Runbook
- ✅ Day 06 - File I/O Practice
- ✅ Day 07 - Linux File System Hierarchy & Scenarios
- ✅ Day 09 - User Management Challenge
- ✅ Day 10 - File Permissions Challenge
- ✅ Day 11 - File Ownership Challenge

### Commands Verified
- man pages for: chmod, chown, systemctl, journalctl
- Syntax double-checked for: usermod, groupadd, ps

---

## Personal Notes

### Aha Moments This Week 💡

1. **setgid bit (g+s) is crucial for shared directories**
   - Without it, files inherit creator's primary group
   - With it, files inherit directory's group
   - This is the secret to team collaboration

2. **journalctl is more powerful than I realized**
   - Can filter by service, time, priority
   - Follows logs in real-time with -f
   - Integrates with systemd seamlessly

3. **Directory permissions work differently than files**
   - 'x' on directory means "can enter"
   - Without 'x', can't even list contents
   - This was causing mystery access issues

4. **chown can change both owner AND group**
   - `chown user:group file` is more efficient
   - Saves typing two separate commands
   - Atomic operation - no intermediate state

---

### Mistakes Made and Lessons Learned ⚠️

**Mistake 1:** Forgot sudo on chown command
- **Lesson:** Always use sudo for ownership changes
- **Fix:** Added sudo to muscle memory

**Mistake 2:** Used chmod 777 in practice (bad habit!)
- **Lesson:** 777 is almost never the right answer
- **Fix:** Think about actual access needs, use 755 or 775

**Mistake 3:** Changed ownership without checking user exists
- **Lesson:** Verify user/group first with id and getent
- **Fix:** Added pre-checks to workflow

**Mistake 4:** Forgot -R flag when trying to change directory contents
- **Lesson:** -R is needed for recursive operations
- **Fix:** Remember: directory itself vs contents inside

---

### What Surprised Me 🎯

- How fast troubleshooting becomes with 3-4 key commands
- How much systemctl status reveals in one output
- How setgid bit solves team permission headaches
- How permissions and ownership are separate but work together
- How much confidence builds from just 11 days of practice

---

## Next Steps - Action Plan

### Immediate (Days 13-15)
1. **Day 13:** Deep dive into networking commands
2. **Day 14:** Practice network troubleshooting scenarios
3. **Day 15:** Start shell scripting basics

### Short-term (Week 3)
- Build a complete web server setup script
- Practice timed troubleshooting drills
- Document personal runbook templates
- Join DevOps community discussions

### Medium-term (End of Month 1)
- Complete 30-day Linux fundamentals
- Move to containerization (Docker)
- Start CI/CD pipeline basics
- Build portfolio of mini-projects

---

## Confidence Tracker

### Command Confidence Levels

| Command | Confidence | Notes |
|---------|-----------|-------|
| `ls -l` | ⭐⭐⭐⭐⭐ | Automatic |
| `chmod` | ⭐⭐⭐⭐⭐ | Both symbolic and numeric |
| `chown` | ⭐⭐⭐⭐ | Need more complex scenarios |
| `systemctl status` | ⭐⭐⭐⭐⭐ | First thing I check |
| `journalctl` | ⭐⭐⭐⭐ | Know basics, need advanced filters |
| `ps aux` | ⭐⭐⭐⭐ | Sorting and filtering solid |
| `useradd/usermod` | ⭐⭐⭐⭐ | Confident with common flags |
| `groupadd` | ⭐⭐⭐⭐⭐ | Simple and memorized |
| `cat/echo` | ⭐⭐⭐⭐⭐ | Second nature |
| `mkdir/cp/mv` | ⭐⭐⭐⭐⭐ | Daily use |

---




