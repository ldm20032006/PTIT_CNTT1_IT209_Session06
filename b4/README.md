# Session 06 — B4: Chạy script nền với `nohup`

## 1. SSH vào server

```bash
ssh devops@221.121.3.53
```

```
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-144-generic x86_64)
System information as of Mon 05 Oct 2026 07:57:10 PM +07
 System load:  0.28     Processes: 116
 Usage of /:   19.5% of 19.09GB   Users logged in: 0
 Memory usage: 25%      IPv4 address for eth0: 221.121.3.53
 Swap usage:   0%
```

## 2. Tạo script `loop-monitor.sh`

```bash
cat << 'EOF' > loop-monitor.sh
#!/bin/bash

while true; do
    echo "System time: $(date)" >> /tmp/monitor.log
    sleep 5
done
EOF
```

Nội dung file:

```bash
cat loop-monitor.sh
```

```bash
#!/bin/bash

while true; do
    echo "System time: $(date)" >> /tmp/monitor.log
    sleep 5
done
```

## 3. Cấp quyền thực thi

```bash
chmod +x loop-monitor.sh
ls -l loop-monitor.sh
```

```
-rwxrwxr-x 1 devops devops 97 Oct  5 19:57 loop-monitor.sh
```

## 4. Chạy script nền bằng `nohup`

```bash
nohup ./loop-monitor.sh > /dev/null 2>&1 &
[1] 49481
```

Kiểm tra process:

```bash
pgrep -f loop-monitor.sh
# 49481
```

## 5. Xem log

```bash
tail -n 10 /tmp/monitor.log
```

```
System time: Mon 05 Oct 2026 07:58:43 PM +07
System time: Mon 05 Oct 2026 07:58:48 PM +07
System time: Mon 05 Oct 2026 07:58:53 PM +07
System time: Mon 05 Oct 2026 07:58:58 PM +07
System time: Mon 05 Oct 2026 07:59:03 PM +07
```

## 6. Dừng script

```bash
kill -15 49481
ps aux | grep loop-monitor.sh
```

```
devops 49595  0.0  0.0  6440  720 pts/0  S+  20:01  0:00 grep --color=auto loop-monitor.sh
[1]+  Terminated  nohup ./loop-monitor.sh > /dev/null 2>&1
```
