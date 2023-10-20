## Shadowsocks
Shadowsocks 是一种**加密代理协议**，用于在网络上进行安全和私密的通信。它通过使用 [[SOCKS5]] 代理协议和加密技术来实现网络流量的传输和保护。

[Github Main Page](https://github.com/shadowsocks) | [Github shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev).

Shadowsocks 的设计目标是绕过网络审查和限制，使用户能够自由访问被封锁的网站和服务。
它最初是由中国开发者在2012年创建的，用于应对中国的网络审查墙（Great Firewall）限制。

**工作原理**
Shadowsocks 原理是基于代理服务器（Proxy Server）和客户端（Client）之间的通信。
- 当用户使用 Shadowsocks 客户端连接到代理服务器（Proxy）时，客户端会将用户的网络流量加密并发送到代理服务器。代理服务器解密流量，并转发请求到目标服务器。
- 目标服务器将响应发送回代理服务器，再由代理服务器加密后发送到客户端，最后客户端解密并展示给用户。
示例
```
Client(加密) ---> ProxyServer(解密) ---> RealServer

Client(解密) <--- (加密)ProxyServer <--- RealServer
```

特点：
1. 加密保护：Shadowsocks 使用加密算法对网络流量进行加密，确保数据的安全性和隐私保护。
2. 网络代理：Shadowsocks 作为代理软件，可以将用户的网络流量通过代理服务器转发，实现绕过网络限制和审查的目的。
3. 灵活性：Shadowsocks 支持多种加密算法和代理协议，用户可以根据自己的需求和偏好进行配置和定制。
4. 跨平台支持：Shadowsocks 客户端和服务器支持多种操作系统和设备，包括 Windows、Mac、Linux、Android 和 iOS 等
### ShadowsocksR
[[ShadowsocksR]]（简称SSR）是在Shadowsocks（SS）基础上的一个分支版本，它在原有SS的功能基础上增加了一些**新特性**。

ShadowsocksR的主要特点包括：

1. [[协议混淆]]：SSR引入了协议混淆功能，通过对网络流量进行混淆和伪装，使其更难被识别和阻止。这样可以提高网络流量的安全性和隐匿性。
2. 更强的加密支持：SSR支持更多的加密算法，使用户能够选择更强的加密方式来保护数据的安全性。
3. 扩展的协议支持：SSR支持原有SS所使用的协议，如 [[SOCKS5]] 和HTTP代理，同时还引入了一些新的协议，如auth_aes128_md5、auth_aes128_sha1等。
4. 混合模式：SSR可以同时使用多种协议和混淆方式，通过混合模式的组合，提供更高级别的流量混淆和安全性。
5. 高级参数配置：SSR提供了更多的参数配置选项，使用户能够根据自己的需求进行更精细的调整和优化。

## [Transparent proxy 透明代理](https://github.com/shadowsocks/shadowsocks-libev#transparent-proxy)
最新的shadowsocks-libev已经提供了_redir_模式。您可以将基于 Linux 的盒子或路由器配置为透明地代理所有 TCP 流量，如果您使用 OpenWRT 支持的路由器，这会很方便。
>[note] redir 模式
>"redir" 模式是 Shadowsocks（SS）代理服务器的一种工作模式。在 [[redir mode]]下，Shadowsocks 服务器会将流量重定向到本地的 Shadowsocks 客户端进行处理和转发。
>
>redir 模式主要用于实现透明代理，即将所有流量通过 Shadowsocks 代理服务器转发，而无需在客户端上进行额外的配置。当用户设备上的网络流量经过 Shadowsocks 服务器时，服务器会将流量重定向到本地的 Shadowsocks 客户端，然后由客户端进行加密和转发。
>
>使用 redir 模式可以实现以下功能：
>1.透明代理：无需在客户端上进行额外的配置，所有流量都会被自动转发到 Shadowsocks 服务器。
>2.流量转发：Shadowsocks 客户端会对流量进行加密，并将其转发到目标服务器，实现对目标服务器的访问。
>3.绕过封锁：通过使用 Shadowsocks 服务器代理网络流量，可以绕过网络审查和限制，访问被封锁的内容。
## [透明代理（纯tproxy）](https://github.com/shadowsocks/shadowsocks-libev#transparent-proxy-pure-tproxy)
在linux主机上执行此脚本可以代理本机的所有出站流量（发送到保留地址的流量除外）。同一局域网下的其他主机也可以将其默认网关更改为该linux主机的ip（同时将dns服务器更改为1.1.1.1或8.8.8.8等）以代理其传出流量。

> 当然ipv6代理也类似，只是改成`iptables`、`ip6tables`to 、to等细节即可`ip`。`ip -6``127.0.0.1``::1`


### [Open WRT](https://github.com/shadowsocks/shadowsocks-libev#openwrt)
OpenWRT 项目维护在这里： [openwrt-shadowsocks](https://github.com/shadowsocks/openwrt-shadowsocks)。

## Others
其他Protocol
- [[Shadowsocks]]（SS）
- [[ShadowsocksR]]（SSR）
- [[VMess]]（Virtual Machine Sockets）
- [[Hysteria Protocol]]

Refrence
1.[[Built Owner Network]].