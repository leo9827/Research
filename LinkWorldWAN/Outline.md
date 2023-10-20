Google [Outline VPN](https://getoutline.org/en/home) 是由Google开发的一种开源工具，用于搭建和管理私人虚拟专用网络（Virtual Private Network，VPN）。它旨在帮助用户轻松地搭建自己的VPN服务器，以加密和保护其网络通信。

Outline VPN 的设计目标是提供简单易用、安全可靠的VPN解决方案。用户可以使用Outline VPN 创建自己的VPN服务器，并将其用于保护网络连接的隐私和安全性。Outline VPN 支持多个平台，包括Windows、macOS、Linux和Android等，提供了简单的用户界面和配置工具，使用户可以快速设置和管理VPN服务器。

[Server](https://github.com/Jigsaw-Code/outline-server) 端使用 Shadowsocks，MacOS, Windows, iOS, Android 均有官方客户端。使用 Outline Manager 可以一键配置 DigitalOcean。其他平台例如 AWS, Google Cloud 也提供相应脚本。主要优点就是使用简单并且整个软件栈全部[开源](https://github.com/Jigsaw-Code/?q=outline)，有专业团队长期维护。
> Outline Manager，由 Jigsaw 开发。Outline Manager 应用程序创建并管理由 Shadowsocks 提供支持的 Outline 服务器。它使用 Electron 框架来提供对 Windows、macOS 和 Linux 的支持。

Outline VPN 使用了实时传输协议（Real-Time Streaming Protocol，RTSP）和Shadowsocks 加密协议来实现网络通信的安全性和保密性。它还提供了流量混淆和伪装功能，以减少VPN流量被识别和封锁的可能性。

需要注意的是，Google Outline VPN 是一个开源项目（[Github Jigsaw-Code](https://github.com/Jigsaw-Code/?q=outline)），用户可以自由使用、修改和分发。用户可以在GitHub上找到Outline VPN 的源代码和相关文档，以了解更多详细信息并参与其开发和改进。

以下来自官方:
## [Shadowsocks 抵抗检测和阻止](https://github.com/Jigsaw-Code/outline-server#shadowsocks-resistance-against-detection-and-blocking)
Shadowsocks 曾经在一些国家/地区被屏蔽，并且由于 Outline 使用 Shadowsocks，因此人们对 Outline 在这些国家/地区的运行持怀疑态度。事实上，人们过去曾尝试过 Outline，但他们的服务器被封锁了。

然而，自2020年下半年以来，情况发生了变化。Outline 团队和 Sh​​adowsocks 社区进行了许多改进，增强了 Shadowsocks 的能力，使其超出了审查者当前的能力。

正如研究[《中国如何检测和阻止 Shadowsocks》](https://gfw.report/talks/imc20/en/)所示，审查员使用主动探测来检测 Shadowsocks 服务器。探测可能是由数据包嗅探触发的，但这不是检测服务器的方式。

尽管 Shadowsocks 是一个标准，但它在如何实现和部署方面留下了很大的选择空间。

首先，您**必须使用 AEAD 密码**。旧的流密码很容易被破解和操纵，使您面临简单的检测和解密攻击。Outline 已禁止所有流密码，因为人们复制旧的示例来设置他们的服务器。Outline Manager 更进一步，为您选择密码，因为用户通常不知道如何选择密码，并且它会生成一个很长的随机秘密，因此您不容易受到基于字典的攻击。

其次，你需要**探测阻力**。Shadowsocks-libev 和 Outline 都添加了这一点。研究[检测抗探测代理](https://www.ndss-symposium.org/ndss-paper/detecting-probe-resistant-proxies/)表明，在过去，一个无效字节无论是插入到流的第 49、50 还是 51 位置，都会触发不同的行为，这一点很能说明问题。这种行为现在已经消失，审查员不能再依赖这种行为。

第三，您需要**防止数据重放**。Shadowsocks-libev 和 Outline 都添加了此类保护，您可能需要在 ss-libev 上显式启用该保护，但这是 Outline 上的默认设置。

第四，Outline 和使用 Shadowsocks-libev 的客户端现在**将 SOCKS 地址和初始数据合并**到同一个初始加密帧中，从而使第一个数据包的大小可变。之前第一个数据包只有 SOCKS 地址，并且大小固定，这是一个赠品。

审查机构曾经封锁 Shadowsocks，但 Shadowsocks 已经进化，在 2021 年，它在猫捉老鼠的游戏中再次领先。

2022 年，中国开始封锁看似随机的流量（[报告](https://www.opentech.fund/news/exposing-the-great-firewalls-dynamic-blocking-of-fully-encrypted-traffic/)）。虽然没有证据表明他们可以检测到 Shadowsocks，但该协议最终被阻止。

作为回应，我们[在 Outline Client 中添加了一项功能](https://github.com/Jigsaw-Code/outline-client/pull/1454)，允许服务管理员在访问密钥中指定在 Shadowsocks 初始化时使用的前缀，这可以用来绕过中国的封锁。

Shadowsocks 仍然是我们选择的协议，因为它简单、易于理解且性能非常好。此外，它背后还有一个由非常聪明的人组成的热情社区。

## Others
related page: [[Built Owner Network]].