# WireGuard Easy


您已经找到了在任何 Linux 主机上安装和管理 WireGuard 的最简单方法！

## 功能

* 一体化：WireGuard + Web UI。
* 安装简便，使用简单。
* 列出、创建、编辑、删除、启用和禁用客户端。
* 显示客户端的二维码。
* 下载客户端的配置文件。
* 连接了哪些客户端的统计信息。
* 每个连接客户端的 Tx/Rx 图表。
* Gravatar 支持。
* 自动亮/暗模式
* 多语言支持
* 流量统计（默认关闭）
* 一次性链接（默认关闭）
* 客户端到期（默认关闭）
* Prometheus 指标支持

## 要求

* 具有支持 WireGuard 内核的主机（所有现代内核）。
* 安装了 Docker 的主机。

## 版本

> 💡 对于 **稳定** 版本，请阅读
> [**production** 分支](https://github.com/wg-easy/wg-easy/tree/production) 上的说明！

我们提供多个 docker 镜像标签，以下内容将帮助您决定
哪一个最适合您。

| 标签 | 分支 | 示例 | 说明 |
| - | - | - | - |
| `latest` | [`production`](https://github.com/wg-easy/wg-easy/tree/production) | `ghcr.io/wg-easy/wg-easy:latest` 或 `ghcr.io/wg-easy/wg-easy` | 尽可能稳定，在需要时快速修复错误，针对 [`production`](https://github.com/wg-easy/wg-easy/tree/production) 部署。 |
| `14` | [`production`](https://github.com/wg-easy/wg-easy/tree/production) | `ghcr.io/wg-easy/wg-easy:14` | 与最新版本相同，坚持使用版本标签。 |
| `nightly` | [`master`](https://github.com/wg-easy/wg-easy/tree/master) | `ghcr.io/wg-easy/wg-easy:nightly` | 大部分不稳定版本会频繁获得软件包和代码更新，针对 [`master`](https://github.com/wg-easy/wg-easy/tree/master) 部署。 |
| `development` | 拉取请求 | `ghcr.io/wg-easy/wg-easy:development` | 用于开发，在进入 [`master`](https://github.com/wg-easy/wg-easy/tree/master) 之前测试来自 PR 的代码。 |

## 安装

### 1. 安装 Docker

如果您尚未安装 Docker，请通过运行以下命令进行安装：

```shell
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker $(whoami)
exit
```

然后再次登录。

### 2. 运行 WireGuard Easy

要自动安装并运行 wg-easy，只需运行：

```shell
docker run --detach \
--name wg-easy \
--env LANG=de \
--env WG_HOST=<🚨YOUR_SERVER_IP> \
--env PASSWORD_HASH='<🚨YOUR_ADMIN_PASSWORD_HASH>' \
--env PORT=51821 \
--env WG_PORT=51820 \
--volume ~/.wg-easy:/etc/wireguard \
--publish 51820:51820/udp \
--publish 51821:51821/tcp \
--cap-add NET_ADMIN \
--cap-add SYS_MODULE \
--sysctl 'net.ipv4.conf.all.src_valid_mark=1' \
--sysctl 'net.ipv4.ip_forward=1' \
--restart except-stopped \
ghcr.io/wg-easy/wg-easy
```

> 💡 将 `<🚨YOUR_SERVER_IP>` 替换为您的 WAN IP 或动态 DNS 主机名。
>
> 💡 将 `<🚨YOUR_ADMIN_PASSWORD_HASH>` 替换为 bcrypt 密码哈希，以登录 Web UI。有关如何生成哈希密码的说明，请参阅 [How_to_generate_an_bcrypt_hash.md](./How_to_generate_an_bcrypt_hash.md)。

Web UI 现在将在 `http://0.0.0.0:51821` 上可用。

Prometheus 指标现在将在 `http://0.0.0.0:51821/metrics` 上可用。Grafana 仪表板 [21733](https://grafana.com/grafana/dashboards/21733-wireguard/)

> 💡 您的配置文件将保存在 `~/.wg-easy`

WireGuard Easy 也可以通过 Docker Compose 启动 - 只需下载
[`docker-compose.yml`](docker-compose.yml)，进行必要的调整并
执行`docker compose up --detach`。

### 3. 赞助商

您喜欢这个项目吗？ [给 Emile 买杯啤酒！](https://github.com/sponsors/WeeJeWel) 🍻 <br>
捐赠给核心组件：[WireGuard](https://www.wireguard.com/donations/)

## 选项

可以通过在 `docker run` 命令中使用 `-e KEY="VALUE"` 设置环境变量来配置这些选项。

| 环境 | 默认 | 示例 | 说明 |
| - | - | - |--------------------------------------------------------------------------------------------------------------------------------------------------|
| `PORT` | `51821` | `6789` | Web UI 的 TCP 端口。|
| `WEBUI_HOST` | `0.0.0.0` | `localhost` | Web UI 绑定到的 IP 地址。|
| `PASSWORD_HASH` | - | `$2y$05$Ci...` | 设置后，登录 Web UI 时需要密码。有关如何生成哈希的信息，请参阅 [如何生成 bcrypt 哈希.md]("https://github.com/wg-easy/wg-easy/blob/master/How_to_generate_an_bcrypt_hash.md")。|
| `WG_HOST` | - | `vpn.myserver.com` | 您的 VPN 服务器的公共主机名。|
| `WG_DEVICE` | `eth0` | `ens6f0` | 应通过该以太网设备转发 wireguard 流量。 |
| `WG_PORT` | `51820` | `12345` | VPN 服务器的公共 UDP 端口。WireGuard 将在 Docker 容器内监听该端口（否则为默认端口）。 |
| `WG_CONFIG_PORT`| `51820` | `12345` | [Home Assistant Plugin](https://github.com/adriy-be/homeassistant-addons-jdeath/tree/main/wgeasy) 上使用的 UDP 端口
| `WG_MTU` | `null` | `1420` | 客户端将使用的 MTU。服务器使用默认 WG MTU。 |
| `WG_PERSISTENT_KEEPALIVE` | `0` | `25` | 保持“连接”打开的秒数。如果此值为 0，则连接将不会保持活动状态。 |
| `WG_DEFAULT_ADDRESS` | `10.8.0.x` | `10.6.0.x` | 客户端 IP 地址范围。|
| `WG_DEFAULT_DNS` | `1.1.1.1` | `8.8.8.8, 8.8.4.4` | 客户端将使用的 DNS 服务器。如果设置为空值，客户端将不会使用任何 DNS。|
| `WG_ALLOWED_IPS` | `0.0.0.0/0, ::/0` | `192.168.15.0/24, 10.0.1.0/24` | 客户端将使用的允许 IP。|
| `WG_PRE_UP` | `...` | - | 请参阅 [config.js](https://github.com/wg-easy/wg-easy/blob/master/src/config.js#L19) 了解默认值。|
| `WG_POST_UP` | `...` | `iptables ...` | 请参阅[config.js](https://github.com/wg-easy/wg-easy/blob/master/src/config.js#L20)了解默认值。 |
| `WG_PRE_DOWN` | `...` | - | 请参阅[config.js](https://github.com/wg-easy/wg-easy/blob/master/src/config.js#L27)了解默认值。 |
| `WG_POST_DOWN` | `...` | `iptables ...` | 请参阅[config.js](https://github.com/wg-easy/wg-easy/blob/master/src/config.js#L28)了解默认值。 |
| `WG_ENABLE_EXPIRES_TIME` | `false` | `true` | 为客户端启用过期时间 |
| `LANG` | `en` | `de` | Web UI 语言（支持：en、ua、ru、tr、no、pl、fr、de、ca、es、ko、vi、nl、is、pt、chs、cht、it、th、hi、ja、si）。|
| `UI_TRAFFIC_STATS` | `false` | `true` | 在 Web UI 中启用详细的 RX / TX 客户端统计信息 |
| `UI_CHART_TYPE` | `0` | `1` | UI_CHART_TYPE=0 # 图表已禁用，UI_CHART_TYPE=1 # 折线图，UI_CHART_TYPE=2 # 面积图，UI_CHART_TYPE=3 # 条形图 |
| `WG_ENABLE_ONE_TIME_LINKS` | `false` | `true` | 启用显示和生成短的一次性下载链接（5 分钟后过期）|
| `MAX_AGE` | `0` | `1440` | Web UI 会话的最大使用期限（分钟）。`0` 表示会话将一直存在，直到浏览器关闭。|
| `UI_ENABLE_SORT_CLIENTS` | `false` | `true` | 启用按名称对 UI 客户端进行排序 |
| `ENABLE_PROMETHEUS_METRICS` | `false` | `true` | 启用 Prometheus 指标 `http://0.0.0.0:51821/metrics` 和 `http://0.0.0.0:51821/metrics/json`|
| `PROMETHEUS_METRICS_PASSWORD` | - | `$2y$05$Ci...` | 如果设置，则请求指标时需要基本身份验证。请参阅 [如何生成 bcrypt 哈希.md]("https://github.com/wg-easy/wg-easy/blob/master/How_to_generate_an_bcrypt_hash.md") 了解如何生成哈希。|

> 如果更改 `WG_PORT`，请确保也更改公开的端口。

## 更新

要更新到最新版本，只需运行：

```shell
docker stop wg-easy
docker rm wg-easy
docker pull ghcr.io/wg-easy/wg-easy
```

然后再次运行上面的 `docker run -d \ ...` 命令。

使用 Docker Compose，WireGuard Easy 可以通过单个命令更新：
`docker compose up --detach --pull always`（如果在 Compose 文件中指定了图像标签并且它不是 `latest`，请确保将其更改为所需的

；默认情况下，它会被省略并且
[默认为 `latest`](https://docs.docker.com/engine/reference/run/#image-references))。\
如果拉取了较新的图像，WireGuared Easy 容器将自动重新创建。

## 常见用例

* [将 WireGuard-Easy 与 Pi-Hole 结合使用](https://github.com/wg-easy/wg-easy/wiki/Using-WireGuard-Easy-with-Pi-Hole)
* [将 WireGuard-Easy 与 nginx/SSL 结合使用](https://github.com/wg-easy/wg-easy/wiki/Using-WireGuard-Easy-with-nginx-SSL)

对于不太常见或特定的边缘情况，请参阅 [Wiki](https://github.com/wg-easy/wg-easy/wiki) 中提供的详细信息。