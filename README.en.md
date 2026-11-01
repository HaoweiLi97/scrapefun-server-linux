# ScrapeFun Linux Server

[简体中文](./README.md) · **English**

Native Linux amd64 Server with a web interface and systemd service management. The current stable release is [0.5.0](https://github.com/HaoweiLi97/scrapefun-server-linux/releases/tag/v0.5.0).

## Install and update

Requires glibc Linux x86_64 / amd64, running systemd, Node.js 22.13+ or 24, Python 3, curl, FFmpeg and FFprobe.

```bash
curl -fsSL https://raw.githubusercontent.com/HaoweiLi97/ScrapeFun/main/scripts/one-click-native-deploy.sh | sudo bash
```

Existing installations must first rerun this command or use `sudo scrapefun update` once to install Server 0.5.0 and the components required for manual web updates. Retain `--config-dir` for custom instances. Updates restart the service and briefly interrupt work.

Download the native archive, its `.sha256` and `SHA256SUMS` from the Release. Save the public installer alongside them and run:

```bash
curl -fsSL https://raw.githubusercontent.com/HaoweiLi97/ScrapeFun/main/scripts/one-click-native-deploy.sh -o one-click-native-deploy.sh
sha256sum --check --ignore-missing SHA256SUMS
sudo bash one-click-native-deploy.sh stable --archive ./scrapefun-server-linux-amd64-0.5.0-stable.tar.gz
```

The archive includes installation, update and startup tools. The installer checks the port, dependencies and archive, backs up data before updating, and restores the previous installation and data on failure. The default address is `http://<server-address>:8096`; select another port during installation if needed.

## Management and data

```bash
sudo scrapefun status
sudo scrapefun health
sudo scrapefun logs --follow
sudo scrapefun restart
sudo scrapefun backup
```

Defaults: program `/opt/scrapefun`, data `/var/lib/scrapefun`, configuration `/etc/scrapefun/server.env`. Updates preserve ports, authentication settings and data. Backups contain secrets; store them securely.

See [installation and management](https://github.com/HaoweiLi97/ScrapeFun/blob/main/docs/linux-server-management.md) for custom paths, multiple instances, WSL and troubleshooting. GPU enhancement depends on host drivers and permissions.
