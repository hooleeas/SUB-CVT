# SUB-CVT Docker deployment

## Run

```bash
docker run -d --name subconverter-new --restart=always \
  -p 25500:25500 \
  ghcr.io/hooleeas/subconverter-new:latest
```

Verify the container:

```bash
curl http://localhost:25500/version
```

The expected response is:

```text
subconverter-new v0.1.0
```

## Docker Compose

```yaml
services:
  subconverter-new:
    image: ghcr.io/hooleeas/subconverter-new:latest
    container_name: subconverter-new
    ports:
      - "25500:25500"
    restart: always
```

To migrate from the previous `sub-cvt` container:

```bash
docker rm -f sub-cvt
docker pull ghcr.io/hooleeas/subconverter-new:latest
docker run -d --name subconverter-new --restart=always \
  -p 25500:25500 \
  ghcr.io/hooleeas/subconverter-new:latest
```

## Updating preferences

Upload a preference file with the configured API token:

```bash
curl -F "data=@newpref.ini" \
  "http://localhost:25500/updateconf?type=form&token=password"
```

## Custom image

Copy replacement preferences, rules, snippets, or profiles into `/base/`:

```dockerfile
FROM ghcr.io/hooleeas/subconverter-new:latest
COPY replacements/ /base/
EXPOSE 25500
```

Build and run it:

```bash
docker build -t subconverter-new-custom:latest .
docker run -d --name subconverter-new --restart=always \
  -p 25500:25500 subconverter-new-custom:latest
```
