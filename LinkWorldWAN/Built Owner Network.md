## Basic
### Protocol
- [[Shadowsocks]]（SS）
- [[ShadowsocksR]]（SSR）
- [[VMess]]（Virtual Machine Sockets）
- [[Hysteria Protocol]]
- HTTP(s)
- Snell
- Trojan

### Client
- SS
- [[V2Ray]]
- Clash
- Netch
- Surge

### Rule
- [[chnroutes]]：
- [[Gfwlist]]：5953

### Tunnel
- [[gost]]：GO Simple Tunnel - 用 golang 编写的简单隧道

Other

- `net.ipv4.ip_forward` 是内核参数，主要是用来把Linux当成路由器来用的参数。一般来说，一个路由器至少要有两个网络接口，一个是WAN，的一个是LAN的，为了让LAN和WAN的流量相通，需要进行内核上路由。
- `iptables -A FORWARD -i eth0 -j ACCEPT` 通行所有需要转发的包，只有机器成为一个路由器时，需要在两个网卡间进行网络包转发时，才需要配置这条规则。
- `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE` 关键字 `MASQUERADE` 意思是“伪装“，NAT的工作原理是就像是一个宿舍收发室对学生宿舍一样，学生宿舍的地址外部不可见，邮递员只看得见整栋宿舍收发室的地址，邮递员把快递交给收发室，收发室再把快递转给学习宿舍（反之，如果学生要对外寄邮件，也是先到收发室，收发室传给邮局）。现在的问题是，所有的学生宿舍如何才能参与到任何快递的通信中，如果把学生宿舍地址发到外部，则没人能把信送回来。如果这个收发室是个自动化的机器人，他要干的事就是，把学生宿舍的地址换成收发室地址。这就是 `MASQUERADE` 的意思——**来自具有接收方 IP 地址的本地网络到达 Internet 某处的数据包必须进行修改，也就是让发送方的地址等于路由器的地**址。
### Cloudflare Warp 原生 IP
>这一段来自： [haoel.github.io#科学上网#10.4 Cloudflare Warp 原生 IP](https://github.com/haoel/haoel.github.io#104-cloudflare-warp-%E5%8E%9F%E7%94%9F-ip)。

很多我们需要访问的网站都需要使用“原生 IP”，比如：[Disney+](https://www.disneyplus.com/)， [ChatGPT](https://chat.openai.com/)，[New Bing](https://www.bing.com/) 等。

所谓“原生 IP”就是指该网站的 IP 地址和其机房的 IP 地址是一致的，但是，很多 IDC 提供商的 IP 都是从其它国家调配来的，这导致我们就算是翻墙了，也是使用了美国的 VPS，但是还是访问不了相关的服务。所以，我们需要使用 Cloudflare Warp 来访问这些网站。

有几种安装方式：
- 全局模式。这种模式下，所有流量都会通过 Cloudflare 的网络，相当于VPN。
- 代理模式。通过在服务器本机启动一个 SOCKS5 代理，然后把需要的流量转发到这个代理上。
#### WARP 模式
WARP 模式是 Cloudflare 提供的一种全局代理模式，就是一个客户端的 VPN，它会将所有流量都通过 Cloudflare 的网络，这样就能访问到原生 IP 了。

可以使用这个一键安装的脚本来快速完成安装 [https://github.com/P3TERX/warp.sh](https://github.com/P3TERX/warp.sh)

下载脚本
```shell
wget git.io/warp.sh
chmod +x warp.sh
```

运行脚本 中文菜单
```shell
./warp.sh menu
```

先后执行如下安装：

- 1 - 安装 Cloudflare WARP 官方客户端
- 4 - 安装 WireGuard 相关组件
- 7 - 自动配置 WARP WireGuard 双栈全局网络

#### 代理模式
如果 WARP 模式安装不能成功，那么可以使用如下的代理模式。代理模式也就是通过一个代理来访问 Cloudflare WARP 的服务，这样就可以访问到原生 IP 了。

>下面的步骤主要使用 Cloudflare 的官方客户端，也许会随时间流逝导致过时，你以参考 [Cloudflare WARP 的官方文档](https://developers.cloudflare.com/warp-client/get-started/linux/)

**1） 安装软件包**

如果是 Ubuntu，那么可以使用以下命令在添加安装源：
```shell
sudo apt install curl
curl https://pkg.cloudflareclient.com/pubkey.gpg | gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflare-client.list
```

添加完源后，安装 Cloudflare WARP 客户端：
```shell
sudo apt update
sudo apt install cloudflare-warp
```
**2） 配置 Cloudflare WARP**

如果是第一次安装，你需要先注册一个帐号。其注册信息会在这里`/var/lib/cloudflare-warp/reg.json`
```shell
warp-cli register
```

然后设置代理模式，这点非常重要，因为默认是 WARP 模式，这个会把你的整个 VPS 带到 Cloudflare 的 VPN 网络中，那么就会出现无法连接的情况。
```shell
warp-cli set-mode proxy
```

然后，设置永久连接模式。
```shell
warp-cli enable-always-on
```

配置完后，你可以使用 `warp-cli settings` 来查看配置。你也可以通过查看配置文件来看是否配置成功，配置文件在 `/var/lib/cloudflare-warp/settings.json`。

**3) 连接 Cloudflare WARP**

使用如下命令来连接 Cloudflare WARP：
```shell
warp-cli connect
```

你可以使用 `warp-cli status` 来查看连接状态。如：
```shell
> warp-cli status
Status update: Connected
Success
```

连接成功后，你可以会在本地有一个 Socks5 代理， `127.0.0.1:40000`，你可以使用如下命令来查看：

**4）验证 Cloudflare WARP**

你可以使用如下命令来验证是否成功：

```shell
curl -x "socks5://127.0.0.1:40000" ipinfo.io
```

如果输出现如下的信息，那么恭喜你，你已经成功了
```json
  "ip": "104.28.247.70",
  "org": "AS13335 Cloudflare, Inc."
```

**6）其它事宜**

**查看运行状态**。 `warp-cli` 是 `warp-svc` 的客户端，真正的程序是 `warp-svc`，你可以使用如下命令来查看：
    
```shell
systemctl status warp-svc
```
    
**内存泄漏问题**。 目前，`warp-svc` 这个程序有内存泄漏的问题（参看 [#124](https://github.com/haoel/haoel.github.io/issues/124)），所以，你需要定期重启服务。你可以使用cronjob来重启服务。运行 `crontab -e`，然后添加如下内容：
```
0 * * * * /bin/systemctl resetart warp-svc
```
    
**文件描述符不足**。另外，如果你的程序出现文件描述不足的情况：
```shell
warp-cli connect
Status update: Unable to connect. Reason: Insufficient system resource: file descriptor
```
需要修改文件描述符的限制，这类的文章比较多，你自行 Google，这里就不再赘述了。如果你想配置 Cloudflare WARP 文件描述符的限制，你可以编辑 `/lib/systemd/system/warp-svc.service`，在 `[Service]` 下面添加如下内容：
```ini
LimitNOFILE=65535
LimitNOFILESoft=65535
```
 然后，重启服务：
```shell
sudo systemctl daemon-reload
sudo systemctl restart warp-svc.service
```

#### Docker proxy
用 Docker 可以更方便地部署起一个 Cloudflare WARP Proxy，只需要一行命令:

```shell
docker run -v $HOME/.warp:/var/lib/cloudflare-warp:rw \
  --restart=always --name=cloudflare-warp e7h4n/cloudflare-warp
```

这条命令会在容器上的 40001 开启一个 socks5 代理，接下来查看这个容器的 ip:

```shell
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' cloudflare-warp
```

然后可以通过 curl 镜像来测试，例如，如果容器的 ip 是 `172.17.0.2`，则可以运行:

```shell
docker run --rm curlimages/curl --connect-timeout 2 -x "socks5://172.17.0.2:40001" ipinfo.io
```

返回的结果中 `org` 字段应该能看到 Cloudflare 相关的信息。

接下来我们给 Gost 增加一个条件转发规则，转发我们希望的域名、地址到 cloudflare 的 warp 网络:

```
-F=socks5://172.17.0.2:40001?bypass=~*.openai.com,openai.com&notls=true
```

### 其它的方式
如下还有一些其它的方式（注：网友提供，没有验证过）

[[Outline]] 是由 Google 旗下 [Jigsaw](https://jigsaw.google.com/) 团队开发的整套翻墙解决方案。Server 端使用 Shadowsocks，MacOS, Windows, iOS, Android 均有官方客户端。使用 Outline Manager 可以一键配置 DigitalOcean。其他平台例如 AWS, Google Cloud 也提供相应脚本。主要优点就是使用简单并且整个软件栈全部[开源](https://github.com/Jigsaw-Code/?q=outline)，有专业团队长期维护。
## 参考资料：
1.[Haoel 科学上网](https://github.com/haoel/haoel.github.io#-%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91-)

