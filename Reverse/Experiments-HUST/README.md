

# 逆向工程分析技术

## Experiment-1
通过对"Please Input"字符串的查找，在汇编代码中找到关键变量dword_5F3088。

其中cmp eax，41h可以猜测到是把第八个字符换成A。

![](./media/ef091366677de70c995425e2d97220ba.png)

跟踪相关函数，反汇编，理解对字符串进行怎么样的操作，这里对每个字符都与0x2E进行亦或。

![](./media/cda7c9a41a32c448b57fd065b6586feb.png)

通过对dword_5F3088进行xref查找，得到对字符串修改的函数。

![](./media/0ee8ebc41e3c2d0ed36b94bc92143673.png)

查找sub_49DC80 \<- sub_48DACA \<- sub_49CEB0()找到a1234isanexa。

![](./media/cd1960cffd1a3923c4aa2e79103045d0.png)

编写脚本处理，得到结果如下flag为2107\#jpAbm\#f{b654;

![](./media/347615d0a370c9c351b655c25d2bb7c8.png)

反编译失败，修改相关指令。

![](./media/162ce8b6e3e193dd503c6dd573f7e0be.png)

## Experiment-2

先进行Android环境的配置，下载AndroidSDK并安装相应的安卓版本，创建安卓虚拟机，其中通过修改host文件来得到dl.google.com的资源。

![](media/29cc131da02571478a12e4e55782a603.png)

将自毁程序在安卓模拟器中打开，注意Android要选用arm架构的，并且要安装相应的java环境，并打开moniter.bat进行进程的监听，打开自毁程序如图所示。

![](./media/a65b1d54251b7044fd968489b87b7c76.png)

另外用AndroidKiller加载apk文件，打开AndroidManifest.xml找到程序的入口类com.yaotong.crackme.MainActivity。

![](./media/421a95cd2e401d5c7c5370fa6eed46e7.png)

通过jadx-gui分析源代码可以得到会调用securityCheck函数，该函数定义在libcrackme.so中。

![](./media/b7eabd384e52f29a10c63112a224d271.png)

用IDA分析libcrackme.so文件，并用c语言展示重要的几个函数，例如securityCheck ，jolin和JNI_OnLoad。

![](./media/dbd597b885582b2f8def85dec2a8bc3c.png)

![](./media/50e3d721a5a25c00f8aae6d2bfd054fb.png)

![](./media/6987faf1ff2d92511ee3862c9459abba.png)

动态分析中，需要将android_server复制到对应目录，设置权限并运行，另开一个cmd进行端口映射，选择调试器，并attach相对应的进程，需要先在模拟器中运行apk。

![](./media/e102b53c65e001b70d8012dcf936725c.png)

![](./media/95d18df402e16fa9dbdd9b1acd7c6795.png)

观察到相应的进程，并加载进入到调试界面，发现程序加载停止到linker处，然后按照指导书设置断点进行调试，并设置相应的一些选项。

![](./media/e6b6fc508642074461ae4446501c22d7.png)

找到Modules模块中crackme的路径，点击进入具体位置。

![](./media/183f688e8af2a40d45ecf79854ed561c.png)

调试到停留在24行断点处，观察off_C10628C显示其内容为aWojiushidaan，得到正确的flag。

![](./media/1dff921d94b89b23fb1d33fe1da2f439.png)

![](./media/835f4ade088eff12d565ab47dab3efda.png)

![](./media/ae1b69985e981e5f36533f3b3990f268.png)

输入flag。

![](./media/273e57c868741ffd25e0715dc8236830.png)
## Experiment-3
首先利用ollydbg打开可执行文件Green.exe，右键选择查找中的“所有模块中的名称”，找到CreateFileA内置API

![](./media/ba16b6e5983ced622f530382cfc2769b.png)

发现此时FileName参数指示了将要打开system.ini文件

![](./media/e4904296e95c8f4450225bf110920075.png)

System.ini文件是乱码，可能是解密该乱码

![](./media/f305929b8b5f7bb3f0b481c559f308a0.png)

从断点处单步执行文件，并观察堆栈段情况，对比system.ini文件，发现首先将密文读入堆栈段

![](./media/c57d5c9344628d5cd3f953b2dd146e0a.png)

逐步执行语句，发现是逐步破解该密文

![](./media/d995f38b983471ba77d00012f903f8be.png)

对应的地址发生改变

接下来对循环部分用高级语言解密

将185c70到185d00部分的16进制码存入一个数组里。

![](./media/84101aac777551f4fc95b4e69ea12b05.png)

![](./media/15885b43c2fa41cdec5482843bcdc9f5.png)

再写解密的过程的那个循环：

![](./media/17d0451f9c4613a48ab081cf11ce1ab9.png)

![](./media/7982ff21c44ba342b5d95abbe72ad389.png)

最后打印出来的内容和实际过程一样：

![](./media/98762f742a60c63a9a5e5264915b1ba7.png)
