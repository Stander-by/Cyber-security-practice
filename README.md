# Cyber-security-practice
ALL cyber-security-practice, include exploit loophole , web attack ,cryptographic, system explotation, developing tools and so on
## Web
### TCP协议漏洞利用
- `sysctl -w net.ipv4.tcp_syncookies=0/1`
- netwox `sudo netwox 76 -i IP -p PORT`
- scapy `python syn_flood.py`
- `syn_flood.c`  \
比较syn-cookies启用和不启用的情况下，攻击的效果（攻击前后，用telnet或者nc连接目标主机端口进行测试）
### DNS攻击
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
> - 在user 中`dig www.example.com`
> - `udp.c`
> - 观察dns缓存
### CSRF攻击
> Copyright © Wenliang Du, Syracuse University

[Ubuntu16.04-Seed](https://pan.baidu.com/share/init?surl=u1XG9LxCAEt-hjvrmo6yGw)。
- Firefox Web 浏览器
- Apache Web服务器
- Elgg Web 应用程序
- HTTPHeaderLive 插件
#### 用户名和口令
| User | UserName | Password |
| :---:   | :-----: | :------:|
| Admin   | admin  | seedelgg|
| Alice     | alice  | seedalice|
|Boby  |   boby  |   seedboby |
|Charlie | charlie |  seedcharlie |
|Samy     | samy   |  seedsamy |
#### Apache-Web对应源码

| URL |Description | Directory |
| :---: | :---------: | :-------: |
|http://www.csrflabattacker.com  | Attacker web site | /var/www/CSRF/Attacker/ |
|http://www.csrflabelgg.com | Elgg web site |/var/www/CSRF/Elgg/ |
#### 环境配置
- ` sudo service apache2 start` 
-  修改/etc/hosts `127.0.0.1   www.example.com` 
- 修改` /etc/apache2/ sites-available/000-default.conf `
#### Attack
- 基于GET请求的CSRF攻击
- 使用POST请求的CSRF攻击
- 实现login CSRF攻击
- 防御策略 
### XSS攻击
#### Apache-Web对应源码
| URL |Description | Directory |
| :---: | :---------: | :-------: |
|http://www.xsslabelgg.com | Elgg  | /var/www/XSS/Elgg/  |

其他和CSRF相同
#### Attack
- 从受害者的机器上盗取Cookie 
- 使用Ajax脚本自动发起会话劫持 
- 构造XSS蠕虫
- 防御策略





