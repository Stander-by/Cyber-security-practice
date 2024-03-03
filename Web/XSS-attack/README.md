# 跨站脚本攻击（XSS）实验

## 从受害者的机器上盗取 Cookie

首先以 Samy 的身份登录并打开个人简介进行编辑，在 Brief description 中添加如下脚本：

![](media/9e3830119b1634051ef6291c451f400f.png)

随后使用 Alice 账号登录，点击 Samy 主页并使用 nc -l 5555 -v 进行监听，观察到成功得到 Alice 的 cookie 信息，如下图

![](media/c40f4f14deffc8c4dc0ea6fa9ac8d11f.png)

## 使用 Ajax 脚本自动发起会话劫持

在 Samy 的 about me 中添加攻击脚本如下

![](media/e7678d36d847a9d1a0a7614ad189dd64.png)选择edit html模式

![](media/05d9822312b189fb44ad6e4d397f7878.png)

![](media/19b2718ee0c18b4afc1046e78c6ec4f6.png)

以 Alice 身份登录，点击 Samy 主页查看后使用 HTTPHeaderLive 抓取信息，观察到发起了修改个人简介的请求，如下图

![](media/86fd29dcc615b86d715b1ecdcac343a9.png)

![](media/793173f59a305f24b1b837b69f4bcc4a.png)

## 构造 XSS 蠕虫

修改任务 3 中的脚本使其产生复制作用，脚本如下

![](media/5769197fa1ae252f9418fbf6967d9b47.png)

![](media/80c8765370c7c45f0740bfb56faf1e34.png)

以 Alice 身份登录后点击 Samy 的主页进行查看，观察到成功发送 POST，如下图

![](media/21a043f944a7acfba093518874644b33.png)

![](media/9a282809f6e7b4a194b9f29efa92ae26.png)

同时观察到Alice的about me也变为了蠕虫脚本，蠕虫病毒攻击成功。

![](media/8483bd4db3fea1201fdc55293ca034a8.png)

## 防御策略

以 admin 账号登录 Elgg 随后点击右上角的 Account 、administration 、plugins将该插件激活，点击 Activate，让其变为 Deactivate 如下

![](media/6acd6b53b031671c4f311adb95e5fc90.png)

测试蠕虫病毒是否有效，观察到此时 JavaScript 脚本被解析为文本而非以脚本方式执行，如下图

![](media/34ce216fc997d0c267c9b46029006969.png)

然后开启 htmlspecialchars

取消掉/var/www/XSS/Elgg/vendor/elgg/elgg/views/default/output目录下text.php 、url.php 、dropdown.php 、email.php 对 htmlspecialchars() 函数调用的注释

![](media/50b911dc07224a34e50ff15c7268942d.png)

email.php

![](media/4fd670a96232aab9c7e6b23abef6d3bb.png)

text.php

![](media/1db8fb8bd334046bdd291a29f21c6c01.png)

url.php

![](media/2e471d85391a0e232cf59dace4d7365c.png)

dropdown.php

![](media/ff96f9ecc36a331097a3fed36f430456.png)

随后测试蠕虫病毒，登录Alice账号，然后编辑Alice简介，在about me处edit html,重新输入脚本,然后保存，再点开Alice的编辑简介，点edit html查看，观察到\>等字符被转义为了&lt等内容，如下

![](media/c15e00b387ab619690914e497b876ba7.png)
