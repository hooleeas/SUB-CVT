# SUB-CVT 中文文档

[English](./README.md)

SUB-CVT 是一个订阅转换工具，基于 [subconverter](https://github.com/tindy2013/subconverter)，并更新了与当前 Sing-box 版本兼容的输出格式。

## Docker 部署

镜像发布后，可使用以下命令启动：

```bash
docker run -d --name sub-cvt --restart=always \
  -p 25500:25500 \
  ghcr.io/hooleeas/sub-cvt:latest
```

镜像支持 `linux/amd64`、`linux/arm64` 和 `linux/arm/v7`。Docker 会根据主机架构自动拉取对应镜像版本。Windows 主机请使用 Docker Desktop 的 Linux 容器模式。

检查服务是否启动：

```bash
curl http://localhost:25500/version
```

正常时会返回：

```text
SUB-CVT v0.1.0
```

也可以使用 Docker Compose：

```yaml
services:
  sub-cvt:
    image: ghcr.io/hooleeas/sub-cvt:latest
    container_name: sub-cvt
    ports:
      - "25500:25500"
    restart: always
```

启动或更新容器：

```bash
docker compose up -d
```

## 转换接口

接口地址：

```text
http://127.0.0.1:25500/sub?target=%TARGET%&url=%URL%&config=%CONFIG%
```

参数说明：

- `target`：目标格式，例如 `clash`、`surge` 或 `singbox`。
- `url`：订阅链接或源内容；作为查询参数传入前需要进行 URL 编码。
- `config`：可选，外部配置链接或本地配置路径；作为查询参数传入前需要进行 URL 编码。

多个订阅可以先用 `|` 连接，再对整个值进行 URL 编码。

## 支持的格式

| 格式 | 读取 | 输出 |
|---|:---:|:---:|
| Clash / ClashR | 是 | 是 |
| Quantumult / Quantumult X | 是 | 是 |
| Loon | 是 | 是 |
| Shadowsocks / ShadowsocksR | 是 | 是 |
| SSD | 是 | 是 |
| Surfboard | 是 | 是 |
| Surge 2–5 | 是 | 是 |
| V2Ray | 是 | 是 |
| Sing-box | 是 | 是 |

Sing-box 输出使用新版配置格式。WireGuard 会生成为 `endpoints`，不再作为普通节点加入 selector/urltest 节点组；`REJECT` 规则会转换为路由 `reject` action，selector 组中的旧 `REJECT` 项会被移除并记录警告。

## 配置文件

默认配置文件位于 `base/`。偏好设置示例：

- `base/pref.example.ini`
- `base/pref.example.yml`
- `base/pref.example.toml`

需要自定义偏好设置、规则、片段或配置模板时，可以基于项目镜像构建自定义镜像，并将替换文件复制到 `/base/`。例如：

```dockerfile
FROM ghcr.io/hooleeas/sub-cvt:latest
COPY replacements/ /base/
EXPOSE 25500
```

如果服务启用了 API Token，更新配置时请替换下面示例中的 `password`，不要使用示例值作为实际密码：

```bash
curl -F "data=@newpref.ini" \
  "http://localhost:25500/updateconf?type=form&token=password"
```

## 许可证

详见 [LICENSE](./LICENSE)。
