# Linux Command Practical Journey

This README documents the Linux command-line practice completed in the terminal.  
The examples cover **file viewing, searching, text processing, file permissions/ownership, system information, disk usage, process monitoring, DNS lookup, and process termination**.

> **Environment:** Ubuntu/Linux terminal  
> **Working directory used in the practice:** `~/Downloads/devops-30-day-practical-journey/linux/test-folder`

---

## 1. `cat` — Display File Contents

### Command

```bash
cat test
```

Displays the complete contents of the `test` file.

### Add line numbers with `cat -b`

```bash
cat -b test
```

`-b` numbers only the non-empty lines.

### Example output

```text
1  file1
2  file2
3  file3
4  file4
5  file5
6  file6
7  file7
```

### When to use

- Quickly read a small file.
- Check configuration or text files.
- Combine multiple files:

```bash
cat file1 file2
```

### Screenshot

![cat and cat -b](screenshots/01-cat-and-cat-b.png)

---

## 2. `grep` — Search for Text

### Command

```bash
grep file3 test
```

Searches the `test` file for lines containing `file3`.

### Example output

```text
file3
```

### Useful variations

```bash
grep -i "error" test
grep -n "file3" test
grep -r "file3" .
```

- `-i` → case-insensitive search
- `-n` → show line numbers
- `-r` → search recursively in directories

### DevOps use

`grep` is frequently used to search logs, configuration files, command output, and deployment/debugging information.

### Screenshot

![grep](screenshots/02-grep.png)

---

## 3. `sort` — Sort Lines

### Command

```bash
sort -f test
```

Sorts the lines in the file. The `-f` option makes sorting case-insensitive.

### Important

`sort` does **not** modify the original file unless its output is redirected:

```bash
sort -f test > sorted-test
```

### Useful examples

```bash
sort test
sort -r test
sort -n numbers.txt
sort -f test
```

- `-r` → reverse order
- `-n` → numeric sorting
- `-f` → ignore case

### Screenshot

![sort](screenshots/03-sort.png)

---

## 4. `tail` — View the End of a File

### Command

```bash
tail test
```

By default, `tail` displays the last **10 lines** of a file.

### View the last 10 lines explicitly

```bash
tail -n 10 test
```

### Follow a growing log file

```bash
tail -f application.log
```

This is especially useful for monitoring application logs in real time.

### Note from the practice

The command:

```bash
tail 10 test
```

caused an error because `10` was interpreted as a filename.

Use:

```bash
tail -n 10 test
```

instead.

### Screenshot

![tail](screenshots/04-tail.png)

---

## 5. `chmod` — Change File Permissions

### Command used

```bash
chmod u+rwx test
```

This gives the **file owner**:

- `r` → read
- `w` → write
- `x` → execute

### Verify permissions

```bash
ls -la
```

The practice changed the file from:

```text
-rw-rw-r--
```

to:

```text
-rwxrw-r--
```

### Common permission examples

```bash
chmod u+x script.sh
chmod 755 script.sh
chmod 644 file.txt
```

### DevOps use

Understanding permissions is essential when working with:

- Shell scripts
- SSH keys
- Docker files
- Application configuration
- CI/CD agents
- Linux servers

### Screenshot

![chmod](screenshots/05-chmod.png)

---

## 6. `chown` — Change File Ownership

### Command

```bash
sudo chown root test
```

Changes the owner of `test` to `root`.

### Verify

```bash
ls -la
```

The ownership changed from the current user to:

```text
root
```

### Common examples

```bash
sudo chown user file.txt
sudo chown user:group file.txt
sudo chown -R user:group directory/
```

### DevOps use

`chown` is commonly required when fixing ownership of:

- Application directories
- Web-server files
- Docker-mounted volumes
- Deployment artifacts
- Log directories

> Use `sudo` carefully because changing ownership of system files can affect system services.

### Screenshot

![chown](screenshots/06-chown.png)

---

## 7. `id` — Display User and Group Information

### Command

```bash
id
```

Displays the current user's:

- User ID (UID)
- Primary group ID (GID)
- Group memberships

### Example

```text
uid=1001(virenlahamage)
gid=1001(virenlahamage)
groups=1001(virenlahamage),27(sudo),998(docker)
```

### Why this matters in DevOps

`id` is useful when troubleshooting:

- Linux permissions
- Docker access
- Sudo permissions
- File ownership
- Service users

### Screenshot

![id](screenshots/07-id.png)

---

## 8. `sed` — Stream Editor

### Command used

```bash
sed 's/PA100/VA100/' test
```

This replaces the first occurrence of `PA100` with `VA100` on each matching line and prints the modified result to the terminal.

### Important

This command **does not modify the original file**.

To save the change to another file:

```bash
sed 's/PA100/VA100/' test > test2
```

To modify the original file directly:

```bash
sed -i 's/PA100/VA100/' test
```

### Breakdown

```text
s/PA100/VA100/
│ │     │
│ │     └── replacement
│ └──────── search text
└────────── substitute operation
```

### Example

```text
PA10001,Test Customer 01,...
```

becomes:

```text
VA10001,Test Customer 01,...
```

### Screenshot

![tail and sed](screenshots/08-tail-and-sed.png)

---

## 9. `diff` — Compare Two Files

### Command

```bash
diff test test2
```

`diff` compares two files line by line and reports their differences.

### Typical output

```text
1,51c1,6
< old content
---
> new content
```

Symbols commonly seen:

- `<` → content from the first file
- `>` → content from the second file
- `c` → changed
- `a` → added
- `d` → deleted

### DevOps use

`diff` is useful for comparing:

- Configuration files
- Deployment manifests
- Generated files
- Script versions
- Before/after changes

### Screenshot

![diff](screenshots/09-diff.png)

---

## 10. `history` — View Previously Executed Commands

### Command

```bash
history 10
```

Displays the last 10 commands executed in the shell.

### Example

```text
2070  sed 's/PA100/VA100/' test
2071  tail 10 test
2072  sed 's/PA100/VA100/' test
2073  ls -la
...
2079  history 10
```

### Useful commands

```bash
history
history 20
history | grep docker
```

You can also use:

```text
Up Arrow
```

to navigate through previously executed commands.

### DevOps use

History is useful for reviewing troubleshooting steps and reproducing commands.

### Screenshot

![history](screenshots/10-history.png)

---

## 11. `free` — Check Memory Usage

### Command

```bash
free
```

A more readable version is:

```bash
free -h
```

It shows memory and swap information.

### Important columns

| Column | Meaning |
|---|---|
| `total` | Total memory |
| `used` | Memory currently used |
| `free` | Completely unused memory |
| `shared` | Shared memory |
| `buff/cache` | Buffers and filesystem cache |
| `available` | Memory estimated to be available for new applications |

### DevOps use

Useful when diagnosing:

- High memory usage
- Application crashes
- Kubernetes node pressure
- Docker workloads
- Swap usage

### Screenshot

![free](screenshots/11-free.png)

---

## 12. `nslookup` — DNS Lookup

### Command

```bash
nslookup google.com
```

It queries DNS and returns IP address information for the domain.

### Example

The practice returned IPv4 addresses such as:

```text
192.178.211.139
192.178.211.101
```

and an IPv6 address.

> DNS responses can change, so the exact IP addresses shown in the screenshot should not be treated as permanent values.

### Useful alternative

```bash
nslookup example.com
dig example.com
```

### DevOps use

DNS troubleshooting is important for:

- Kubernetes services
- Load balancers
- Application domains
- API connectivity
- Cloud networking
- Website outages

### Screenshot

![nslookup](screenshots/12-nslookup.png)

---

## 13. `df` and `du` — Disk Usage

### `df -h`

```bash
df -h
```

Shows filesystem-level disk usage.

The `-h` option makes sizes human-readable.

### Important columns

| Column | Meaning |
|---|---|
| `Filesystem` | Disk/filesystem |
| `Size` | Total capacity |
| `Used` | Used space |
| `Avail` | Available space |
| `Use%` | Percentage used |
| `Mounted on` | Mount point |

### `du -h`

```bash
du -h
```

Shows disk usage of files/directories.

### `du -sh`

```bash
du -sh
```

Shows a summarized total.

### Difference

```text
df → filesystem/disk capacity
du → file and directory usage
```

### DevOps use

These commands are essential for diagnosing:

- Full disks
- Large log files
- Docker storage problems
- Build-server storage
- Kubernetes node disk pressure

### Screenshot

![df and du](screenshots/13-df-and-du.png)

---

## 14. `top` — Real-Time Process Monitoring

### Command

```bash
top
```

`top` provides a real-time view of running processes and system resource usage.

### Important information shown

- Load average
- CPU usage
- Memory usage
- Swap usage
- Process IDs
- CPU percentage
- Memory percentage
- Running commands

### Useful keys inside `top`

| Key | Action |
|---|---|
| `q` | Quit |
| `P` | Sort by CPU |
| `M` | Sort by memory |
| `k` | Send a signal to a process |
| `r` | Change process priority |

### DevOps use

`top` is useful when a server or container host is experiencing high CPU or memory usage.

### Screenshot

![top](screenshots/14-top.png)

---

## 15. `ps aux` — List Running Processes

### Command

```bash
ps aux
```

Displays a snapshot of running processes.

### Important columns

| Column | Meaning |
|---|---|
| `USER` | Process owner |
| `PID` | Process ID |
| `%CPU` | CPU usage |
| `%MEM` | Memory usage |
| `VSZ` | Virtual memory |
| `RSS` | Resident memory |
| `TTY` | Terminal |
| `STAT` | Process state |
| `START` | Start time |
| `TIME` | CPU time |
| `COMMAND` | Command/process |

### Useful combinations

Find a process:

```bash
ps aux | grep nginx
```

Find processes for a user:

```bash
ps -u username
```

### `top` vs `ps`

```text
top     → continuously updated process view
ps aux  → one-time process snapshot
```

### Screenshot

![ps aux](screenshots/15-ps-aux.png)

---

## 16. `kill` — Terminate a Process

### Command

```bash
kill PID
```

By default, `kill` sends the `SIGTERM` signal, requesting that the process terminate gracefully.

Example:

```bash
kill 2080661
```

### Force kill

If a process does not terminate gracefully:

```bash
kill -9 2080661
```

`SIGKILL` forces termination and cannot be caught by the process.

### Check the process first

```bash
ps aux | grep 2080661
```

or:

```bash
ps -p 2080661
```

### Best practice

Prefer:

```bash
kill PID
```

before:

```bash
kill -9 PID
```

because applications may need the opportunity to clean up files, connections, or other resources.

### Screenshot

![kill](screenshots/16-kill.png)

---

# Quick Command Reference

| Command | Purpose | Example |
|---|---|---|
| `cat` | Display file contents | `cat test` |
| `grep` | Search text | `grep file3 test` |
| `sort` | Sort lines | `sort -f test` |
| `tail` | Show end of file | `tail -n 10 test` |
| `chmod` | Change permissions | `chmod u+rwx test` |
| `chown` | Change ownership | `sudo chown root test` |
| `id` | Show user/group IDs | `id` |
| `sed` | Transform/replace text | `sed 's/PA100/VA100/' test` |
| `diff` | Compare files | `diff test test2` |
| `history` | Show command history | `history 10` |
| `free` | Show memory usage | `free -h` |
| `nslookup` | DNS lookup | `nslookup google.com` |
| `df` | Filesystem disk usage | `df -h` |
| `du` | File/directory usage | `du -sh` |
| `top` | Live process monitoring | `top` |
| `ps` | Process snapshot | `ps aux` |
| `kill` | Send signal to process | `kill PID` |

---

# Key DevOps Takeaways

These commands form an important Linux foundation for DevOps work.

### File & text operations

```text
cat → grep → sort → tail → sed → diff
```

These are especially useful for inspecting and processing application logs and configuration files.

### System administration

```text
chmod → chown → id
```

These help troubleshoot Linux users, groups, permissions, and ownership.

### System troubleshooting

```text
free → df → du → top → ps → kill
```

These commands help investigate memory, disk, CPU, and process-related issues.

### Networking

```text
nslookup
```

This provides a basic way to troubleshoot DNS resolution.

---

# Practice Directory

The commands were practiced from:

```bash
cd ~/Downloads/devops-30-day-practical-journey/linux/test-folder
```

Check the current directory:

```bash
pwd
```

List files with permissions and ownership:

```bash
ls -la
```

---

## Next Recommended Linux Practice

After these commands, continue with:

```text
find
xargs
awk
cut
tr
head
wc
uniq
tee
curl
wget
tar
gzip
ssh
scp
rsync
systemctl
journalctl
crontab
```

For DevOps, give special attention to **`find`, `awk`, `cut`, `sed`, `grep`, `xargs`, `curl`, `ssh`, `systemctl`, and `journalctl`**, because they are frequently used in automation, troubleshooting, CI/CD, and server administration.
