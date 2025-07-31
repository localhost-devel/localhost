
# 🐧 Linux File Commands & System Utilities for DevOps Engineers

This README provides essential Linux commands with practical use cases tailored for DevOps engineers and system administrators. Use these to effectively manage files, analyze logs, retrieve system info, monitor processes, and perform troubleshooting.

---

## 📁 1. Find Log Files

```bash
pwd                          # Current directory
ls -ltr                      # List sorted by modified time
find . -type f -name "*.log" # All log files
find . -type f -name "*.log" -mtime -7  # Modified in last 7 days
history                      # Command history
```

> **Use Case:** Monitor applications, debug issues from log files.

---

## 💾 2. Find Large Files

```bash
find . -type f -size +100M
find . -type f -size +100M -exec ls -lh {} \;
find . -type f -size +100M -exec du -h {} \;
```

> **Use Case:** Diagnose disk usage issues or heavy resource consumption.

---

## 🕒 3. Find Recently Modified Files

```bash
find . -type f -mtime -30
find . -type f -mtime -30 -exec ls -lh {} \;
find . -type f -mtime -30 -exec du -h {} \;
```

> **Use Case:** Auditing and tracking recent changes.

---

## 🔍 4. Find Files Containing a Specific String

```bash
find . -type f -exec grep -l "ERROR" {} \;
find . -type f -exec grep -l "ERROR" {} \; | xargs ls -lh
find . -type f -exec grep -l "ERROR" {} \; | xargs du -h
```

> **Use Case:** Quickly identify files containing error logs.

---

## 🚨 5. Find "ERROR" Lines in Logs

```bash
sudo grep -rn 'ERROR' / --include="*.log" 2>/dev/null
```

> **Use Case:** Critical for debugging and monitoring system logs.

---

## 🖥️ 6. System Info Commands

### OS & Kernel Info

```bash
cat /etc/os-release
uname -a
```

### Disk Usage

```bash
df -h
du -sh /
```

### Open Ports & Listeners

```bash
netstat -tuln
ss -tuln
lsof -i -P -n | grep LISTEN
```

> **Use Case:** Determine services listening on specific ports or resource bottlenecks.

---

## ⚙️ 7. Process Management

```bash
ps -ef
ps -ef | grep nginx
kill -9 <PID>
```

> **Use Case:** Stop unresponsive services or find related processes.

---

## 📨 8. Automate Script with Cron (Everyday 5AM)

```bash
crontab -e

# Cron entry:
0 5 * * * /path/to/script.sh
```

Sample `script.sh`:
```bash
#!/bin/bash
echo "Daily health check at $(date)" >> /tmp/health.log
```

---

## 🌐 9. Network Troubleshooting

```bash
traceroute google.com
ping example.com
curl -I https://example.com
```

---

## 📝 10. Edit File Without Opening

```bash
echo -e "line1
line2
line3" > test.txt
sed -i '3s/.*/This is a new line 3/' test.txt
cat test.txt
```

---

## ✅ 11. Verify Previous Command Status

```bash
ls /tmp
echo $?
ls /nonexistent
echo $?
```

> `0 = Success`, `1 = Error`

---

## 🔧 12. Service Management (e.g., nginx)

```bash
systemctl status nginx
systemctl restart nginx
systemctl stop nginx
```

---

## 🔗 13. Hard vs Soft Links

### Soft Link

```bash
ln -s file.txt link.txt
rm file.txt
cat link.txt # Broken link
```

### Hard Link

```bash
ln file.txt hardlink.txt
rm file.txt
cat hardlink.txt # Still works
```

> **Use Case:** Soft links for navigation, hard links for redundancy.

---

## 🔐 14. Secure File Transfer

```bash
scp file.txt user@remote:/path/
```

---

## 🌍 15. Domain IP Resolution

```bash
nslookup example.com
dig example.com
host example.com
```

---

## 📄 16. View First & Last Lines of a File

```bash
head -n 5 filename
tail -n 5 filename
```

---

## 👥 17. Group and User Management

```bash
groupadd devs
useradd -m alice
usermod -aG devs alice
groups alice
```

---

## 🧾 18. Change File Ownership

```bash
chown newuser:newgroup file.txt
```

Without sudo (if owned by you):

```bash
chown $USER:$USER file.txt
```

---

## ➕ Additional Must-Know DevOps Commands

```bash
top              # Real-time system monitoring
htop             # Interactive process viewer
free -h          # Memory usage
uptime           # System uptime
vmstat           # System performance
iostat           # I/O stats
who              # Currently logged in users
last             # Login history
dmesg            # Kernel logs
journalctl       # Systemd logs
```

---
## 📘 Tip For Beginners
- **Recommendation:** Bookmark this file or convert into a bash cheat sheet.
- **Practice:** Regularly use these commands to become proficient.
- **Explore:** Use `man <command>` to learn more about each command.
- **Experiment:** Try modifying commands to suit your needs.
- **Learn:** Understand the output of each command to troubleshoot effectively.
- **Documentation:** Refer to the official Linux documentation for deeper insights.

---
## 📚 Useful Resources

- 🔗 [Jenkins Beginner Docs](https://www.jenkins.io/doc/tutorials/build-a-java-app-with-maven/)
- 🔗 [Freestyle Projects in Jenkins](https://www.jenkins.io/doc/book/pipeline/freestyle/)

---

### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999