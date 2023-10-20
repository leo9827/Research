[ss-redir man help page](https://manpages.org/ss-redir).

**Shadowsocks-libev**是一个轻量级且安全的socks5代理。它是 clowwindy 创建的原始 Shadowsocks 的端口。**Shadowsocks-libev**采用纯 C 语言编写，利用**libev**实现高性能和低资源消耗的同时实现。

## Intr
shadowsocks中的redir mode是一个**重定向模式**。

ss-redir 是 [[Shadowsocks]] 提供的一个全局系统代理模式。它通过iptables规则实现将本机所有流量重定向至shadowsocks tunnel,从而简化代理配置,且应用进行透明。

它的工作原理是:

1. 客户端和服务器建立一个shadowsocks tunnel。
2. 客户端配置iptables规则,将本机某个端口(如1080)的流量重定向到shadowsocks tunnel。
3. 现在,所有发送到1080端口的流量都会通过shadowsocks tunnel加密传输,实现代理功能。

使用redir模式的主要优点是:

- 不需要修改应用程序配置,实现系统全局代理。所有的流量都会自动通过代理隧道。
- 与本地应用程序完全分离,不影响正常使用。
- 部署简单,仅需要设置iptables规则。
- 兼容性强,支持Windows和Linux等多种系统。

## 网速影响
redir mode 会对网络速度产生一定影响:

- 额外开销：redir模式需要额外通过iptables将流量重定向到shadowsocks隧道,这会增加一定的软件开销。
- 加密解密：流量在通过shadowsocks隧道传输时需要进行加密和解密操作,这也会消耗一定计算资源。
- 单线程传输：redir模式下,所有流量都是通过一个shadowsocks tunnel串行传输的,无法实现并行加快传输。
- 隧道性能：流量速度依赖于shadowsocks服务器和隧道的带宽性能。如果服务器带宽不足,会影响最终速度。
- 系统负载：全局代理模式下,系统需要处理更多额外的网络流量,可能增加CPU和内存利用率。

总的来说,使用redir mode 相比直接网络连接, Speedtest 等测速结果一般会慢10%-30%。

## Difference
shadowsocks的服务端和客户端在使用redir模式和不使用redir模式时,有以下主要区别:

服务端方面:

- 不使用redir模式：服务端只需提供标准的shadowsocks服务,只负责通信隧道的建立和数据的加密/解密处理。
- 使用redir模式：    服务端和不使用redir模式时behaviour完全相同,无任何变化。

客户端方面:

- 不使用redir模式：客户端需要自行配置应用程序的代理设置,或使用系统代理等方式实现代理功能。
- 使用redir模式:
    - 需要额外安装iptables进行流量重定向。
    - 需要在启动脚本中添加iptables规则,将本地端口(如1080)的流量重定向到shadowsocks隧道。
    - 不再需要单独为每个应用程序配置代理,全局实现系统代理功能。
    - 对客户端来说,隧道端口和密码等配置与不使用redir模式相同。
        
总结来说:

- 服务端方面：redir模式与否完全无差异。
- 客户端方面：使用redir模式可以简化代理配置,无需单独为每个应用设置,但需要额外安装iptables进行流量转发。