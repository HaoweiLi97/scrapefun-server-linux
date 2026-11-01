# ScrapeFun Linux Server

**简体中文** · [English](./README.en.md)

Linux amd64 原生服务端，提供网页界面并由 systemd 管理。当前稳定版为 [0.5.0](https://github.com/HaoweiLi97/scrapefun-server-linux/releases/tag/v0.5.0)。

## 安装与升级

需要 glibc Linux x86_64 / amd64、运行中的 systemd、Node.js 22.13+ 或 24、Python 3、curl、FFmpeg 和 FFprobe。

运行公开安装入口：

```bash
curl -fsSL https://raw.githubusercontent.com/HaoweiLi97/ScrapeFun/main/scripts/one-click-native-deploy.sh | sudo bash
```

已有安装需先通过上方命令，或执行一次 `sudo scrapefun update`，升级到 0.5.0 并安装网页更新所需组件。自定义实例请沿用 `--config-dir`。完成后可从网页手动安装更新；更新会重启服务并短暂中断任务。

也可下载 Release 中的原生归档、对应 `.sha256` 和 `SHA256SUMS`，将公开安装脚本保存到同一目录后执行：

```bash
curl -fsSL https://raw.githubusercontent.com/HaoweiLi97/ScrapeFun/main/scripts/one-click-native-deploy.sh -o one-click-native-deploy.sh
sha256sum --check --ignore-missing SHA256SUMS
sudo bash one-click-native-deploy.sh stable --archive ./scrapefun-server-linux-amd64-0.5.0-stable.tar.gz
```

安装、更新与启动工具已包含在归档中。安装器检查端口、依赖与包完整性，更新前备份数据，失败时恢复旧安装与数据。首次访问默认地址为 `http://<服务器地址>:8096`，端口可在安装时指定。

## 管理与数据

```bash
sudo scrapefun status
sudo scrapefun health
sudo scrapefun logs --follow
sudo scrapefun restart
sudo scrapefun backup
```

默认程序位于 `/opt/scrapefun`，数据位于 `/var/lib/scrapefun`，配置位于 `/etc/scrapefun/server.env`。更新保留端口、认证配置与数据；备份包含密钥，应妥善保存。

完整参数、多实例、自定义路径、WSL 与故障排查见 [Linux 安装与管理](https://github.com/HaoweiLi97/ScrapeFun/blob/main/docs/linux-server-management.md)。GPU 增强取决于主机驱动与权限。
