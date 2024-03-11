# WireShark Analyse
## EndPoint to EndPoint Statistics
`lotsofarp.pcapng`
<details>
<summary><strong>查看端点统计</strong></summary>
<ul>
<li><strong>协议</strong>:可以添加额外的协议过滤标签</li>
<li><strong>解析名称</strong>:可以开启名称解析功能来查看端点名称</li>
<li><strong>显示过滤器的限制</strong>:让端点窗口只显示与显示过滤器相匹配的端点</li>
<li>可以使用端点窗口将特定的数据包过滤出来，使其显示在数据包列表（Packet List）面板中，使用这种方法可以快速定位与某个特定端点相关联的数据包。具体的方法是右键单击一个特定端点，然后再菜单中选择相应的选项。</li>
</ul>
</details>

<details>
<summary><strong>查看网络会话</strong></summary>
<ul>
</ul>
</details>

<details>
<summary><strong>使用端点和网络会话窗口定位最高用量者</strong></summary>
<ul>
<li>通过端点窗口找到发送/接受数据包多，数据量最大的端点们；例如，找到通信量最大的两个端点，我们可以假设他们正在各自与很多其它的设备进行大量通信，或者，这两个IP之间正在进行彼此的通信。实际上，最大通信量之间的端点通信是比较常见的。</li>
<li>打开会话窗口就可以验证多个最大通信量端点之间是否存在通信，通信量达到什么程度。同时，将该会话作为过滤器应用，就可以在数据包面板中过滤出该会话的所有通信数据包，以进行进一步的分析。</li>

</ul>
</details>

<details>
<summary><strong>协议分层结构的统计</strong></summary>
<ul>
<li>统计->协议分级</li>
<li>TCP、IP、DHCP等所占百分比的多少</li>
</ul>
</details>

## Name Resolution
` lotsofarp.pcapng`
<details>
<summary><strong>名称解析</strong></summary>
<ul>
<li>编辑->首选项->Name Resolution</li>
<li> 解析MAC地址（resolve MAC address）：这种类型的名称解析使用ARP协议，试图将第2层的物理地址转换为网络层的IP地址。如果这种尝试失败，那么Wireshark会使用程序目录中的ethers文件尝试进行换。</li>
<li> 解析传输名称（resolve transport name）：这种类型的名称解析尝试将一个端口号转换成一个与其相关的名字。例如：将端口号80显示为http。当你碰到一个不常见的端口号而你又不知道这代表着什么协议的时候，这个功能就非常有用了。 </li>
<li> 解析网络名称（resolve network/IP name）：这种类型的名称解析试图将第三层的IP地址转换成为更容易记忆的域名，如果这个域名具有高描述性的话，这种转换对我们理解系统的目的或者所有者，将是非常有帮的。</li>
<li> Use captured DNS packet data for address resolve：从已经捕获的 DNS 数据包中解析出IP地址和域名的映射关系。</li>
<li>  Use an external network name resolve：允许 Wireshark 使用你当前设备配置的 DNS服务器来解析捕获数据包中的IP地址对应的名称。这个功能在捕获的数据包中没有DNS数据，但是仍然需要名称帮助分析的时候，非常有用。 </li>
</ul>
</details>

<details>
<summary><strong>使用自定义hosts文件</strong></summary>
<ul>
<li>在Wireshark 的首选项窗口中，选择Only use the profile “hosts” file。 </li>
<li> 使用 windows 记事本或者类似的文件编辑器创建一个新文件。该文件应该至少包含一条IP和对应名称的记录，Wireshark会根据这个映射来把相应的IP地址替换为hosts文件里对应的名称并最终显示到数据包列表面板里。 </li>
<li>把文件以文本格式命名为hosts，并保存到<USERPROFILE>\Application Data\Wireshark\hosts 目录下。</li>

</ul>
</details>

## Decode Protocol

## Trace flow

## Packet Lengrh statistics