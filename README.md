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

### Subscription formats and common clients

| Format | Common compatible clients |
|---|---|
| Base64 | v2rayN (Windows), v2rayNG (Android), NekoRay (Windows/Linux), NekoBox (Android), Hiddify (multi-platform), Shadowrocket (iOS/macOS), Streisand (iOS), FoXray (iOS/macOS) |
| Clash | Clash Verge Rev, Mihomo Party, FlClash, Clash Nyanpasu, Nextin, Clash for Windows (discontinued), Clash for Android (discontinued), Clash Meta for Android (legacy project), ClashX / ClashX Pro, Stash (iOS), OpenClash (OpenWrt) |
| Sing-box | sing-box for Android, sing-box for Apple platforms (iOS/macOS), NekoBox (Android) |
| Surge | Surge (iOS/macOS) |
| Loon | Loon (iOS) |

These are common examples, not an exhaustive list. Import and proxy compatibility depend on the client version, core, and protocols included in the subscription. “Clash Meta” usually refers to the Mihomo core; the name of the client application may differ.

Sing-box output uses the current configuration format. WireGuard is emitted as an `endpoint` rather than a regular outbound, so it is not added to selector/urltest groups. `REJECT` rules use the route `reject` action; legacy `REJECT` items in selector groups are removed with a warning.

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
