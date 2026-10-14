# Linux 10-Activity Lab Workflow

Use your own lab VM. Run commands one at a time and document actual output.

## Activity 1 — Navigation and File Management
```bash
mkdir -p ~/linux_lab/docs ~/linux_lab/scripts
cd ~/linux_lab
touch readme.txt notes.txt script.sh
echo "Welcome to Linux Practice" > readme.txt
cp readme.txt docs/
mv notes.txt docs/
rm script.sh
tree ~/linux_lab
# If tree is unavailable:
ls -R ~/linux_lab
```

## Activity 2 — File Viewing and Editing
```bash
nano ~/linux_lab/readme.txt
```
Add your name and definitions of Kernel, FHS (Filesystem Hierarchy Standard), and Shell. Save with Ctrl+O, Enter, then Ctrl+X.
```bash
head -n 3 ~/linux_lab/readme.txt
tail -n 2 ~/linux_lab/readme.txt
wc -w ~/linux_lab/readme.txt
grep -n "Linux" ~/linux_lab/readme.txt
```

## Activity 3 — Permissions and Ownership
```bash
cd ~/linux_lab
touch secure.txt
chmod 640 secure.txt
ls -l secure.txt
chmod 777 secure.txt
ls -l secure.txt
chmod 640 secure.txt
getent passwd student2
```
If `student2` does not exist and your instructor permits account creation:
```bash
sudo useradd -m -s /bin/bash student2
sudo chown student2 ~/linux_lab/secure.txt
ls -l ~/linux_lab/secure.txt
```
`640` means owner read/write, group read, others none. `777` is intentionally insecure; restore `640`. Do not create accounts without authorization.

## Activity 4 — Archiving and Compression
```bash
mkdir -p ~/backup_test ~/restore_test
echo "First file" > ~/backup_test/file1.txt
echo "Second file" > ~/backup_test/file2.txt
echo "Third file" > ~/backup_test/file3.txt
tar -cvf ~/backup.tar -C ~ backup_test
gzip -f ~/backup.tar
ls -lh ~/backup.tar.gz
tar -xzvf ~/backup.tar.gz -C ~/restore_test
find ~/restore_test -type f -print
cat ~/restore_test/backup_test/file1.txt
cat ~/restore_test/backup_test/file2.txt
cat ~/restore_test/backup_test/file3.txt
```
The destination `~/restore_test` must exist before extraction. Expected extracted path: `~/restore_test/backup_test/`.

## Activity 5 — Process Management
```bash
sleep 300 &
jobs -l
fg
```
Press Ctrl+C. To demonstrate killing by PID:
```bash
sleep 300 &
jobs -l
ps aux | grep '[s]leep 300'
kill PID_NUMBER
ps aux | grep '[s]leep 300'
top
```
Replace `PID_NUMBER` with the real numeric PID. Press `q` to exit `top`.

## Activity 6 — Networking Basics
```bash
ip a
ping google.com -c 4
sudo ss -tulnp
hostname
sudo cp /etc/hosts /etc/hosts.backup-lab
echo "127.0.0.1 testsite.local" | sudo tee -a /etc/hosts
getent hosts testsite.local
ping testsite.local -c 4
```
Back up `/etc/hosts` first. Internet ping may fail due to DNS, network settings, or ICMP filtering. To undo the lab change, remove only the line you added and preserve other entries.

## Activity 7 — Searching and Finding
```bash
find / -name readme.txt 2>/dev/null
grep -n "Linux" ~/linux_lab/docs/*.txt
find ~ -type f -mmin -1440 -print
find ~ -type f -name "*.txt" | wc -l
```

## Activity 8 — Package Management (APT systems)
```bash
sudo apt update
sudo apt install tree
tree --version
sudo apt remove tree
dpkg -l > ~/packages.txt
ls -lh ~/packages.txt
head ~/packages.txt
```
Review proposed package changes before confirming.

## Activity 9 — Scripting Basics
```bash
cat > ~/linux_lab/hello.sh <<'EOF'
#!/bin/bash
echo "Hello, Linux World!"
date
pwd
EOF
chmod +x ~/linux_lab/hello.sh
~/linux_lab/hello.sh
```

## Activity 10 — System Monitoring and Logs
```bash
df -h
free -h
sudo tail -n 10 /var/log/syslog
```
If `/var/log/syslog` is missing, try `sudo tail -n 10 /var/log/messages`, or use `sudo journalctl -n 10 --no-pager`.
```bash
uptime
who
lscpu
```
Redact sensitive details from logs before sharing evidence.

Use the evidence checklist and results template to record real outcomes, including errors or permission limitations.
