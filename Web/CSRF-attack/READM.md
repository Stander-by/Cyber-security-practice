

# 跨站请求伪造（CSRF）攻击实验

## 基于 GET 请求的 CSRF 攻击

首先启动apache服务器

![](media/33894e361be6320b8657ad40f4f449b7.png)

Elgg Web应用程序中的账户信息如下图所示

![](media/fb15175c52dc05fbe687444d4a7c0319.png)

http header live的使用

![](media/43d48b5cdc7f337afccc7594ba3b471a.png)

![](media/f43d348e806cb26b4bd9b98cc4b28004.png)

Apache服务器下的网站源文件均在/var/www/目录下。首先以Charlie账户登录，发送添加 Samy为好友的请求，并使用HTTPHeaderLive来获取添加好友请求的HTTP请求，如下图

登录<http://www.csrflabelgg.com>

![](media/637115d17c4813e6fe62b1b0cd86add8.png)

观察到发送了一条 POST 请求，在这条请求中浏览器需要发送当前想要添加的用户的GUID 信息，而对于 Samy 而言可知其 GUID 为 45。据此编写攻击网页内容如下，其中包含一个img 条目，以自动引发与 POST 类似的 GET 请求，网页内容如下

![](media/8dac7b40a9294d8cad77963849e3852e.png)

更改/etc/apache2/sites-available/000-default.conf 如下

![](media/3d464cacf2d3e899d7c84b8df78a7475.png)

去/var/www/CSRF/Attacker 下建一个 index.html，然后写入刚才的网页内容

![](media/be9aa7ed73d0215e253236cf46532661.png)

该恶意网站中包含 Samy 的 GUID，当点击后由于网页中包含一个 img 类型的文件，因此会向其指定的网站发起 GET 请求，而此时会携带登录的 cookie，因此最终会添加 Samy 为好友。

![](media/30931cab19eb8e7cbc0b622ea9262bca.png)

向 Alice 发送带有恶意网址的邮件，如下图：

![](media/12cd0f0996ca3bf446b2051cf9581ed0.png)

登录 Alice 账户，查看收到的邮件，如下：

![](media/9dab4401716e16edc919b0abb5e3e2e0.png)

点开链接，成功抓包执行 get 请求，也发现成功加了 Samy 好友

![](media/4a2fd6ca3b1d10b07a1d39f1e34970f5.png)

![](media/4bbc577a05cd6f571b77696925ded024.png)

## 使用 POST 请求的 CSRF 攻击

以Alice身份登录并尝试修改自身的个人资料，同时使用 HTTPHeaderLive抓取信息并分析，如下

![](media/96d019118906e332fd3d568bf3bafb69.png)

观察到 URL 值应为 /action/profile/edit ，提交的表单中需要包含需要修改信息的用户的GUID，获取 GUID 的方式为使用自己的账户登录并试图添加 Alice为好友，用 HTTPHeaderLive 抓取，可以观察到发送的 GUID 为 42。因此编写恶意网页信息如下

![](media/49d8dac1a4f9b1335e8c5bde231d91f0.png)

写进index.html, 仿照任务一，用 Charlie 发邮件，点击链接，得到下图

![](media/be9a44a878922dc69ef66aca59124e52.png)

![](media/234db99b6e0a15ac6fa9418d579fb584.png)

![](media/da1645743d2048956c0c749af45851b6.png)

### 实现 login CSRF 攻击

以 Alice 身份登录并抓取登录表单信息，观察到使用 URL 为 /action/login ，同时需要包含用户名和密码信息，如下图：

![](media/867269903689235c149f4da51da091c0.png)

编写恶意网站内容如下

![](media/7f44ddfcc4db9b21e5d5eed8180e5d61.png)

重复任务一的操作

编写诱导邮件并发送给 Alice，再用 Alice 登录，点击链接，发现立即登录Samy 账号

![](media/f4f7305c40957fa08a1392c8ebe47066.png)

##防御策略

首先进入目录/var/www/CSRF/Elgg/vendor/elgg/elgg/engine/classes/Elgg，在ActionService.php 文件中找到函数 gatekeeper()并注释掉 return true 语句，如下图

![](media/2a75ee897a2e51f7d0648561f5f6f03b.png)

随后对任务 2 和任务 3 进行测试如下：

任务3

点击链接后观察到不断发送 POST 请求，但并不能表单的自动提交，如下图：

![](media/923b8abfe45eea809ce47e0510943e47.png)

返回原始网页后观察到提示页面到期错误，如下图

![](media/78bb2b1713caf4bc75b3a84e121a7881.png)

防御成功。抓到的 token 如下

![](media/ecc953a47996e6af4a3e1bc65eefaf86.png)

任务二防御：

![](media/a3e0b1e2475e8926ab4db1bcc7a9a1dd.png)

返回原始网页后出现大量的令牌错误提示，如下图

![](media/0ae0c2b0bca3227c97561841f336a3a4.png)

防御成功。但是该过程无法获取到令牌信息，这是因为无法绕过浏览器的访问控制来使用攻击页面中的 JavaScript 访问任何内容。