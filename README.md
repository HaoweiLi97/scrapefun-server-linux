# ScrapeFun Linux Server

ScrapeFun 的 Linux AMD64 服务端发行版。下载发布包后即可在 Linux
服务器上运行网页界面，并由 systemd 持续管理服务。

最新安装包请到 [Releases](https://github.com/HaoweiLi97/scrapefun-server-linux/releases) 下载。

## 系统要求

- 64 位 x86_64 / AMD64 Linux（面向 glibc 发行版构建）
- Node.js 20、22 或 24
- systemd（使用安装脚本时需要）
- 建议安装 FFmpeg；播放、转码和部分媒体处理功能依赖它

## 安装

1. 从某个 Release 下载以下四个文件到同一目录：发布包、`.sha256`、
   `SHA256SUMS` 和安装脚本。
2. 在下载目录校验并安装：

```bash
sha256sum -c SHA256SUMS
sudo bash install-scrapefun-server-linux-amd64.sh \
  scrapefun-server-linux-amd64-<version>-stable.tar.gz
```

安装程序会创建并启动 `scrapefun` 服务。首次安装会自动创建认证密钥。
完成后在浏览器打开：

```text
http://<服务器地址>:8096
```

## 服务管理

```bash
sudo systemctl status scrapefun
sudo systemctl restart scrapefun
sudo journalctl -u scrapefun -f
```

默认位置：

| 内容 | 位置 |
| --- | --- |
| 程序 | `/opt/scrapefun` |
| 数据库、图片与缓存 | `/var/lib/scrapefun` |
| 配置 | `/etc/scrapefun/server.env` |

可在 `/etc/scrapefun/server.env` 中修改端口或监听地址，例如：

```bash
PORT=8096
HOST=0.0.0.0
```

修改后重启服务使配置生效。

## 更新

下载新版 Release 的四个文件，先校验 `SHA256SUMS`，再用同一条安装命令
安装新归档。服务会自动重启；数据和配置会保留在上述目录中。

## WSL

可在 WSL2 中直接解压后试运行，不需要 systemd：

```bash
tar -xzf scrapefun-server-linux-amd64-<version>-stable.tar.gz
cd scrapefun-server-linux-amd64-<version>-stable
export APP_AUTH_SECRET="$(node -e "process.stdout.write(require('node:crypto').randomBytes(48).toString('base64url'))")"
export SCRAPEFUN_DATA_DIR="$PWD/data"
./start.sh
```

在 Windows 浏览器中访问 `http://localhost:8096`。如果希望 WSL 在后台自动
运行，请启用 WSL 的 systemd 后使用上面的安装方式。

## 常见问题

- 服务无法启动：先运行 `sudo journalctl -u scrapefun -n 100 --no-pager` 查看日志。
- 端口被占用：修改 `server.env` 中的 `PORT`，然后重启服务。
- 媒体播放或转码不可用：确认服务器已安装 `ffmpeg` 和 `ffprobe`，且它们在 `PATH` 中。
- GPU 图像增强可用性取决于主机 GPU、Vulkan 驱动与权限；没有可用 GPU 时，普通服务功能仍可使用。
