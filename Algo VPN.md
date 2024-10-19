# Algo VPN

[![Twitter](https://img.shields.io/twitter/url/https/twitter.com/fold_left.svg?style=social&label=Follow%20%40AlgoVPN)](https://twitter.com/AlgoVPN)
[![](https://github.com/trailofbits/algo/workflows/Main/badge.svg?branch=master)](https://github.com/trailofbits/algo/actions)

Algo VPN 是一组 Ansible 脚本，可简化个人 WireGuard 和 IPsec VPN 的设置。它使用最安全的默认设置，并与常见的云提供商配合使用。有关更多信息，请参阅我们的 [发布公告](https://blog.trailofbits.com/2016/12/12/meet-algo-the-vpn-that-works/)。

## 功能

* 仅支持适用于 iOS、macOS 和 Linux 的强加密 IKEv2（AES-GCM、SHA2 和 P-256）
* 除 Android 和 Windows 10 外，还支持上述所有设备的 [WireGuard](https://www.wireguard.com/)
* 为 iOS、macOS、Android 和 Windows WireGuard 客户端生成 .conf 文件和二维码
* 生成 Apple 配置文件以自动配置 iOS 和 macOS 设备的 IPsec - 无需客户端软件
* 包含用于添加和删除用户的帮助脚本
* 使用本地 DNS 解析器屏蔽广告（可选）
* 设置有限的 SSH 用户以进行隧道传输流量（可选）
* 基于当前版本的 Ubuntu 和 strongSwan
* 安装到 DigitalOcean、Amazon Lightsail、Amazon EC2、Vultr、Microsoft Azure、Google Compute Engine、Scaleway、OpenStack、CloudStack、Hetzner Cloud、Linode 或 [您自己的 Ubuntu 服务器（用于更高级用户)](docs/deploy-to-ubuntu.md)

## 反功能

* 不支持旧式密码套件或协议，如 L2TP、IKEv1 或 RSA
* 不安装 Tor、OpenVPN 或其他有风险的服务器
* 不依赖于 [TLS](https://tools.ietf.org/html/rfc7457) 的安全性
* 不声称提供匿名性或避免审查
* 不声称保护您免受 [FSB](https://en.wikipedia.org/wiki/Federal_Security_Service)、[MSS](https://en.wikipedia.org/wiki/Ministry_of_State_Security_(China))、[DGSE](https://en.wikipedia.org/wiki/Directorate-General_for_External_Security) 或[FSM](https://en.wikipedia.org/wiki/Flying_Spaghetti_Monster)

## 部署 Algo 服务器

运行 Algo 服务器的最简单方法是在本地系统或 [Google Cloud Shell](docs/deploy-from-cloudshell.md) 上运行它，并让它在云中为您设置一个 _新_ 虚拟机。

1. **在云托管提供商上设置帐户。** Algo 支持 [DigitalOcean](https://m.do.co/c/4d7f4ff9cfe4)（最用户友好）、[Amazon Lightsail](https://aws.amazon.com/lightsail/)、[Amazon EC2](https://aws.amazon.com/)、[Vultr](https://www.vultr.com/)、[Microsoft Azure](https://azure.microsoft.com/)、[Google Compute Engine](https://cloud.google.com/compute/)、[Scaleway](https://www.scaleway.com/)、[DreamCompute](https://www.dreamhost.com/cloud/computing/)、[Linode](https://www.linode.com) 或其他基于 OpenStack 的云托管、[Exoscale](https://www.exoscale.com) 或其他基于 CloudStack 的云托管，或 [Hetzner Cloud](https://www.hetzner.com/)。

2. **获取 Algo 的副本。**Algo 脚本将安装在您的本地系统上。有两种方法可以获取副本：

- 下载 [ZIP 文件](https://github.com/trailofbits/algo/archive/master.zip)。解压文件以创建一个名为“algo-master”的目录，其中包含 Algo 脚本。

- 使用“git clone”创建一个名为“algo”的目录，其中包含 Algo 脚本：
```bash
git clone https://github.com/trailofbits/algo.git
```

3. **安装 Algo 的核心依赖项。**Algo 要求您的系统上安装 **Python 3.10 或更高版本**和至少一个支持包。

- **macOS：**Catalina (10.15) 及更高版本将 Python 3 作为可选命令行开发人员工具包的一部分。从终端运行：

```bash
python3 -m pip install --user --upgrade virtualenv
```

如果出现提示，请安装命令行开发人员工具并重新运行上述命令。

对于 Catalina 之前的 macOS 版本，请参阅 [从 macOS 部署](docs/deploy-from-macos.md) 以获取有关安装 Python 3 的信息。

- **Linux：** Ubuntu、Debian 和 Fedora 的最新版本已安装 Python 3。 如果您的 Python 版本不是 3.10，则需要使用 pyenv 安装 Python 3.10。确保您的系统是最新的，并安装支持包：
* Ubuntu 和 Debian：
```bash
sudo apt install -y --no-install-recommends python3-virtualenv 文件查找
```
在运行 Ubuntu 的 Raspberry Pi 上，还要安装`libffi-dev`和`libssl-dev`。

* Fedora：
```bash
sudo dnf install -y python3-virtualenv
```

-**Windows：**使用 Windows Subsystem for Linux (WSL) 创建在 Windows 下运行的 Ubuntu 副本，从中安装和运行 Algo。有关更多信息，请参阅 [Windows 文档](docs/deploy-from-windows.md)。

4. **安装 Algo 的其余依赖项。**您需要运行这些 c每次下载新的 Algo 副本时，都会从 Algo 目录中执行命令。在终端窗口中，使用 `cd` 进入 `algo-master`（ZIP 文件）或 `algo`（`git clone`）目录并运行：
```bash
python3 -m virtualenv --python="$(command -v python3)" .env &&
source .env/bin/activate &&
python3 -m pip install -U pip virtualenv &&
python3 -m pip install -r requirements.txt
```
在 Fedora 上，首先运行 `export TMPDIR=/var/tmp`，然后将选项 `--system-site-packages` 添加到上面的第一个命令（在 `python3 -m virtualenv` 之后）。在 macOS 上，如果出现提示，请安装 C 编译器。

5. **设置配置选项。** 在您最喜欢的文本编辑器中打开文件 `config.cfg`。在 `users` 列表中指定您想要创建的用户。为您计划连接到 VPN 的每台设备创建一个唯一用户。
> 注意：[仅限 IKEv2] 如果您想稍后添加或删除用户，则在服务器部署期间，**必须**在“是否要保留密钥 (PKI)？”提示中选择“是”。您还应该在部署之前查看其他选项，因为稍后改变主意[可能需要您部署全新的服务器](https://github.com/trailofbits/algo/blob/master/docs/faq.md#i-deployed-an-algo-server-can-you-update-it-with-new-features)。

6. **开始部署。**返回您的终端。在 Algo 目录中，运行 `./algo` 并按照说明进行操作。有几种可选功能可用，但对于功能齐全的 VPN 服务器来说，这些功能都不是必需的。这些可选功能在[此处](docs/deploy-from-ansible.md) 中有更详细的描述。

就是这样！服务器部署过程完成后，您将收到以下消息。记下 p12（用户证书）密码和 CA 密钥，以防您以后需要它们，**它们只会在这次显示**。

您现在可以设置客户端以连接到您的 VPN。继续下面的[配置 VPN 客户端](#configure-the-vpn-clients)。

```
"# 恭喜！#"
"# 您的 Algo 服务器正在运行。#"
"# 配置文件和证书位于 ./configs/ 目录中。#"
"# 连接后转到 https://whoer.net/#"
"# 并确保所有流量都通过 VPN。#"
"# 本地 DNS 解析器 172.16.0.1#"
"# 新用户的 p12 和 SSH 密钥密码为 XXXXXXXX#"
"# CA 密钥密码为 XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX#"
"# Shell 访问：ssh -F configs/<server_ip>/ssh_config <hostname>#"
```

## 配置 VPN 客户端

用户需要的证书和配置文件放在 `configs` 目录中。请确保这些文件的安全，因为许多文件包含私钥。所有文件都保存在以新 Algo VPN 服务器的 IP 地址命名的子目录下。

### Apple 设备

WireGuard 用于在 Apple 设备上提供 VPN 服务。Algo 为 `config.cfg` 中定义的每个用户生成一个 WireGuard 配置文件 `wireguard/<username>.conf` 和一个二维码 `wireguard/<username>.png`。

在 iOS 上，从 iOS App Store 安装 [WireGuard](https://itunes.apple.com/us/app/wireguard/id1441195209?mt=8) 应用程序。然后，使用 WireGuard 应用程序扫描二维码或将配置文件 AirDrop 到设备。

在 macOS Mojave 或更高版本上，从 Mac App Store 安装 [WireGuard](https://itunes.apple.com/us/app/wireguard/id1451685025?mt=12) 应用程序。运行该应用程序后，WireGuard 将出现在菜单栏中。单击 WireGuard 图标，选择 **从文件导入隧道...**，然后选择适当的 WireGuard 配置文件。

在 iOS 或 macOS 上，您可以通过编辑 WireGuard 应用中的隧道配置来启用“按需连接”和/或排除某些受信任的 Wi-Fi 网络（例如您的家庭或工作网络）。（Algo 无法自动为您执行此操作。）

在旧版本的 macOS 上安装 WireGuard 稍微复杂一些。请参阅 [使用 macOS 作为 WireGuard 的客户端](docs/client-macos-wireguard.md)。

如果您更喜欢在 Apple 设备上使用内置 IPSEC VPN，或者需要自动配置“按需连接”或排除的 Wi-Fi 网络，请参阅 [使用 Apple 设备作为 IPSEC 的客户端](docs/client-apple-ipsec.md)。

### Android 设备

WireGuard 用于在 Android 上提供 VPN 服务。安装 [WireGuard VPN 客户端](https://play.google.com/store/apps/details?id=com.wireguard.android)。将相应的 `wireguard/<name>.conf` 文件导入到您的设备，然后使用它建立新连接。有关更详细的操作步骤，请参阅 [Android 设置说明](/docs/client-android.md)。

### Windows

WireGuard 用于在 Windows 上提供 VPN 服务。Algo 为 `config.cfg` 中定义的每个用户生成一个 WireGuard 配置文件 `wireguard/<username>.conf`。

安装 [WireGuard VPN 客户端](https://www.wireguard.com/install/#window)s-7-8-81-10-2012-2016-2019）。将生成的 `wireguard/<username>.conf` 文件导入到您的设备，然后与其建立新连接。

### Linux WireGuard 客户端

WireGuard 与 Linux 客户端配合使用效果很好。请参阅 [此页面](docs/client-linux-wireguard.md) 以获取有关如何在 Ubuntu 上配置 WireGuard 的示例。

### Linux strongSwan IPsec 客户端（例如，OpenWRT、Ubuntu 服务器等）

请参阅 [此页面](docs/client-linux-ipsec.md)。

### OpenWrt Wireguard 客户端

请参阅 [此页面](docs/client-openwrt-router-wireguard.md)。

### 其他设备

根据平台的不同，您可能需要以下一个或多个文件。

* ipsec/manual/cacert.pem：CA 证书
* ipsec/manual/<user>.p12：用户证书和私钥（PKCS#12 格式）
* ipsec/manual/<user>.conf：strongSwan 客户端配置
* ipsec/manual/<user>.secrets：strongSwan 客户端配置
* ipsec/apple/<user>.mobileconfig：Apple 配置文件
* wireguard/<user>.conf：WireGuard 配置文件
* wireguard/<user>.png：WireGuard 配置二维码

## 设置 SSH 隧道

如果您打开了可选的 SSH 隧道角色，则将在 `config.cfg` 中为每个用户创建本地用户帐户，并且他们的 SSH authorized_key 文件将位于 `configs` 目录 (user.ssh.pem) 中。 SSH 用户帐户没有 shell 访问权限，无法使用密码进行身份验证，并且只有有限的隧道选项（例如，需要 `ssh -N`）。这可确保 SSH 用户拥有设置隧道所需的最低访问权限，并且无法在 Algo 服务器上执行任何其他操作。

使用下面的示例命令启动 SSH 隧道，方法是将 `<user>` 和 `<ip>` 替换为您自己的。隧道设置完成后，您可以配置浏览器或其他应用程序，以使用 127.0.0.1:1080 作为 SOCKS 代理，通过 Algo 服务器路由流量：

```bash
ssh -D 127.0.0.1:1080 -f -q -C -N <user>@algo -i configs/<ip>/ssh-tunnel/<user>.pem -F configs/<ip>/ssh_config
```

## 通过 SSH 进入 Algo 服务器

您的 Algo 服务器已配置为仅使用密钥的 SSH 访问，以用于管理目的。打开终端应用程序，`cd` 进入您最初下载 Algo 的 `algo-master` 目录，然后使用成功消息中列出的命令：

```
ssh -F configs/<ip>/ssh_config <hostname>
```

其中 `<ip>` 是您的 Algo 服务器的 IP 地址。如果您发现自己经常登录服务器，那么自动加载 Algo ssh 密钥将很有用。将以下代码片段添加到 `~/.bash_profile` 的底部，以将其永久添加到您的 shell 环境中：

```
ssh-add ~/.ssh/algo > /dev/null 2>&1
```

或者，您可以选择将为任何 Algo 服务器生成的配置包含在您的 SSH 配置中。编辑文件 `~/.ssh/config` 以在顶部包含此指令：

```
包括 <algodirectory>/configs/*/ssh_config
```

其中 `<algodirectory>` 是您克隆 Algo 的目录。

## 添加或删除用户

_如果您选择在部署过程中保存 CA 密钥_，那么 Algo 自己的脚本可以轻松地从 VPN 服务器中添加和删除用户。

1. 更新“config.cfg”中的“用户”列表
2. 打开终端，“cd”到 algo 目录，然后使用“source .env/bin/activate”激活虚拟环境
3. 运行命令：“./algo update-users”

此过程完成后，Algo VPN 服务器将仅包含“config.cfg”文件中列出的用户。

## 附加文档
* [常见问题](docs/faq.md)
* [故障排除](docs/troubleshooting.md)
* Algo 如何使用 [防火墙](docs/firewalls.md)

### 特定云提供商的设置说明
* 配置 [Amazon EC2](docs/cloud-amazon-ec2.md)
* 配置 [Azure](docs/cloud-azure.md)
* 配置 [DigitalOcean](docs/cloud-do.md)
* 配置 [Google Cloud Platform](docs/cloud-gce.md)
* 配置 [Vultr](docs/cloud-vultr.md)
* 配置 [CloudStack](docs/cloud-cloudstack.md)
* 配置 [Hetzner Cloud](docs/cloud-hetzner.md)

### 从常见平台安装和部署
* 从部署[macOS](docs/deploy-from-macos.md)
* 从 [Windows](docs/deploy-from-windows.md) 部署
* 从 [Google Cloud Shell](docs/deploy-from-cloudshell.md) 部署
* 从 [Docker 容器](docs/deploy-from-docker.md) 部署

### 设置 VPN 客户端以连接到服务器
* 设置 [Android](docs/client-android.md) 客户端
* 使用 Ansible 设置 [Linux](docs/client-linux.md) 客户端
* 设置 Ubuntu 客户端以使用 [WireGuard](docs/client-linux-wireguard.md)
* 设置 Linux 客户端以使用 [IPsec](docs/client-linux-ipsec.md)
* 设置 Apple 设备以使用 [IPsec](docs/client-apple-ipsec.md)
* 设置运行 macOS 10.13 或更早版本的 Mac 以使用[WireGuard](docs/client-macos-wireguard.md)

### 高级部署
* 部署到您自己的 [Ubuntu](docs/deploy-to-ubuntu.md) 服务器和 Road Warrior 设置
* 从 [Ansible](docs/deploy-from-ansible.md) 非交互部署
* 在创建时使用 shell 脚本或 cloud-init 部署到 [云服务器](docs/deploy-from-script-or-cloud-init-to-localhost.md)
* 部署到 [不受支持的云服务器ovider](docs/deploy-to-unsupported-cloud.md)
* 部署到您自己的 [FreeBSD](docs/deploy-to-freebsd.md) 服务器

如果您已阅读所有文档并有其他问题，请[创建新讨论](https://github.com/trailofbits/algo/discussions)。

## 认可

> 我一直在抱怨 VPN 服务的糟糕状况，可能是时候就这个问题进行适当的讨论。TL;DR：使用 Algo。

-- [Kenn White](https://twitter.com/kennwhite/status/814166603587788800)

> 在选择 VPN 提供商/应用之前，请务必进行一些研究
> https://research.csiro.au/ng/wp-content/uploads/sites/106/2016/08/paper-1.pdf ... – 或考虑 Algo

-- [The Register](https://twitter.com/TheRegister/status/825076303657177088)

> Algo 确实简单且安全。

-- [the grugq](https://twitter.com/thegrugq/status/786249040228786176)

> 我试用了 Algo VPN，这是一组脚本，即使您对开发不太了解，也可以在很短的时间内设置云中的 VPN。我不得不说，我对 Trail of Bits 的方法印象深刻。

-- [Romain Dillet](https://twitter.com/romaindillet/status/851037243728965632) 为 [TechCrunch](https://techcrunch.com/2017/04/09/how-i-made-my-own-vpn-server-in-15-minutes/) 撰写

> 如果您不愿意向匿名随机 VPN 提供商支付现金，这是最好的解决方案。

-- [Thorin Klosowski](https://twitter.com/kingthor) 为 [Lifehacker](http://lifehacker.com/how-to-set-up-your-own-completely-free-vpn-in-the-cloud-1794302432)

## 支持 Algo VPN
[![Flattr](https://button.flattr.com/flattr-badge-large.png)](https://flattr.com/submit/auto?fid=kxw60j&url=https%3A%2F%2Fgithub.com%2Ftrailofbits%2Falgo)
[![PayPal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s -xclick&hosted_button_id=CYZZD39GXUJ3E)
[![Patreon](https://img.shields.io/badge/back_on-patreon-red.svg)](https://www.patreon.com/algovpn)
[![Bountysource](https://img.shields.io/bountysource/team/trailofbits/activity.svg)](https://www.bountysource.com/teams/trailofbits)

所有捐款均支持持续开发。谢谢！

* 我们通过 [PayPal](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=CYZZD39GXUJ3E)、[Patreon](https://www.patreon.com/algovpn) 和 [Flattr](https://flattr.com/submit/auto?fid=kxw60j&url=https%3A%2F%2Fgithub.com%2Ftrailofbits%2Falgo) 接受捐赠。
* 当您注册 Digital Ocean 时，请使用我们的 [推荐代码](https://m.do.co/c/4d7f4ff9cfe4) 获得 10 美元信用额度。
* 我们还接受并赞赏通过 Github Pull 请求提供新代码和错误修复。

Algo 已获得 AGPLv3 许可和分发。如果您想分发基于 Algo 的闭源修改或服务，请考虑<a href="mailto:opensource@trailofbits.com">购买例外</a>。与上述方法一样，这将有助于支持持续开发。