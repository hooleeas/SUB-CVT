# SUB-CVT

[中文文档](./README-zh.md)

A subscription converter based on [subconverter](https://github.com/tindy2013/subconverter) by [tindy2013](https://github.com/tindy2013), with updated Sing-box output compatible with current Sing-box releases.

## Docker

Run the current image:

```bash
docker run -d --name sub-cvt --restart=always \
  -p 25500:25500 \
  ghcr.io/hooleeas/sub-cvt:latest
```

Check the service:

```bash
curl http://localhost:25500/version
```

Expected output:

```text
SUB-CVT v0.1.0
```

Docker Compose:

```yaml
services:
  sub-cvt:
    image: ghcr.io/hooleeas/sub-cvt:latest
    container_name: sub-cvt
    ports:
      - "25500:25500"
    restart: always
```

## Supported formats

| Format | Source | Target |
|---|:---:|:---:|
| Clash / ClashR | Yes | Yes |
| Quantumult / Quantumult X | Yes | Yes |
| Loon | Yes | Yes |
| Shadowsocks / ShadowsocksR | Yes | Yes |
| SSD | Yes | Yes |
| Surfboard | Yes | Yes |
| Surge 2–5 | Yes | Yes |
| V2Ray | Yes | Yes |
| Sing-box | Yes | Yes |

## Conversion endpoint

```text
http://127.0.0.1:25500/sub?target=%TARGET%&url=%URL%&config=%CONFIG%
```

Required parameters:

- `target`: output format, for example `clash`, `surge`, or `singbox`.
- `url`: subscription URL or source content. URL-encode this value first.

Optional parameters:

- `config`: external configuration URL or local configuration path. URL-encode this value first.

Multiple subscriptions can be joined with `|` before URL encoding.

## Configuration

The default configuration files are under `base/`. The example preference files are:

- `base/pref.example.ini`
- `base/pref.example.yml`
- `base/pref.example.toml`

To replace preferences, rules, snippets, or profiles in a container, build a small image on top of this project and copy the replacement files into `/base/`.

## License

See [LICENSE](./LICENSE).
