# AppArmor
## chroot
### 为 touchstone 程序添加 setuid root 权限，并启动执行
- 关闭地址随机化：`sudo sysctl -w kernel.randomize_va_space=0`

- Make进行编译得到touchstone，并添加`setuid root`权限`sudo chown root touchstone` `sudo chown +s touchstone`，执行
 
- 然后打开 Firefox 浏览器，地址框内输入 `127.0.0.1:80` 登录 web server，点击register 注册，注册后点击 login 登录
 
- 再在/tmp 文件夹下创建 `test.txt` 文件，内容填充 test，将 owner 设为 root `sudo chown root test.txt`

- 下面来修改1.py的内容，`ldd banksy`查询到libc_base的地址为`0xf7d87000`
![](./pic/libc.png)
 
- 通过readelf指令查询到`system:0x0004c7b0`,`exit:0x0003bc40`,`unlink:0x0010cce0`

- 通过`stringx-tx`找到/bin/sh字符串的偏移为`0x001b5faa`
 
 
- Ebp地址根据touchstone运行返回的frame pointer确定，为`0xffffd1f8`
 
- 修改`1.py`
![](./pic/changepy1.png)
- 重新运行 touchstone，注意 sudo 运行，登录，然后注意此时 ebp 变了，需要更改代码里的 ebp 地址为新的 framepoint，然后运行` 1-1.py`，发现`test.txt`被删除。
![](./pic/res1.png) 
### 修改 server.c，增加 chroot 支持，并重新 make
- 按照要求需要修改 server.c，增加chroot支持，并重新make。
![](./pic/chroot.png)
- 运行脚本./chroot-setup.sh
 
- 在jail里执行touchstone
![](./pic/jail.png)

- 注意此时链接库变了，因此地址也要及时更改，这里我们不用 `ldd` 查看，开启另一个终端，执行如下命令
```
    ps -ef | grep banksv
    sudo gdb
    attach pid
    info proc mappings #查看libc.so的加载地址
```
- 可以得到libc.so.6的地址为`0xf7d99000`
 
- 去修改攻击脚本中的 base_addr，然后重复子任务一。发现删除不掉 `/tmp/test.txt`
![](./pic/res2.png)
- 重复子任务一。发现删除 `/jail/tmp/test.txt`
![](./pic/res3.png)
 
## Change euid
### 修改源代码，使用 setuid (或 setresuid 等)，及时减少 privilege，使得 banksv 进程没有（不必要的）root 权限，并重新 make
- `server.c` 和 `2.py` 都用最初的代码进行更改，打开` server.c` ，找到插入 setuid代码的位置，插入代码 `setresuid(1000,1000,1000)`(一共三处)
 
- Make进行编译，修改`2.py`部分地址进行攻击，发现不可以继续删除`root privilege`的文件`/tmp/test.txt`
![](./pic/euid.png)

## Seccomp
### 默认允许，显式拒绝（Fail Securely）
- 首先安装相应库
```
sudo apt install libseccomp-dev libseccomp2 seccomp
sudo apt-get install libseccomp-dev:i386
```
- 修改banksv.c
```c
ctx = seccomp_init(SCMP_ACT_ALLOW);
if(ctx ==NULL)
    exit(-1);
```
- 修改makefile
```
	gcc -m32 -no-pie -fno-stack-protector -g -o banksv . sql_lite3/sqlite3.o -l pthread -l dl ./sql_lite3sqlhelper.c banksv.c  token.c parse.c http-tree.c handle.c -l seccomp 
```
- 修改完成后进行make，然后在3-1.py中修改相关地址，详见默认拒绝
- 开启服务器，开始运行脚本，test.txt没有删除。
![](./pic/res4.png)
### 默认拒绝，显示允许

- 修改banksv.c
```c
ctx = seccomp_init(SCMP_ACT_KILL);
if(ctx ==NULL)
    exit(-1);
```
- 修改makefile
```
	gcc -m32 -no-pie -fno-stack-protector -g -o banksv . sql_lite3/sqlite3.o -l pthread -l dl ./sql_lite3sqlhelper.c banksv.c  token.c parse.c http-tree.c handle.c -l seccomp 
```
- 修改完成后进行make，然后修改相关地址
- 开启服务器，开始运行脚本，注意我们这里的脚本也是原始代码。默认拒绝，显式允许 unlink，删除成功
![](./pic/res5.png)

## AppArmor
- 开启APPARMOR服务，并安装相关包
```
sudo apt install apparmor-utils
sudo apt install apparmor-profiles
sudo systemctl start apparmor
```
- Make编译程序，一个终端执行`sudo aa-genprof banksv`，另一个运行程序
![](./pic/genprof.png)

- 添加访问控制策略。按"F"生成配置文件：文件位置为`/etc/apparmor.d/home.stander-by.pwn2.code.banksv`，打开该文件，编写配置如下
![](./pic/config.png)
- 用`dmesg`查看，可以看到被 apparmor 拒绝执行
![](./pic/res6.png)