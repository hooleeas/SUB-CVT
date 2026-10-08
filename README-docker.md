# SUB-CVT Docker deployment

## Run

```bash
docker run -d --name sub-cvt --restart=always \
  -p 25500:25500 \
  ghcr.io/hooleeas/sub-cvt:latest
```

Verify the container:

```bash
curl http://localhost:25500/version
```

The expected response is:

```text
SUB-CVT v0.1.0
```

## Docker Compose

```yaml
services:
  sub-cvt:
    image: ghcr.io/hooleeas/sub-cvt:latest
    container_name: sub-cvt
    ports:
      - "25500:25500"
    restart: always
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
FROM ghcr.io/hooleeas/sub-cvt:latest
COPY replacements/ /base/
EXPOSE 25500
```

Build and run it:

```bash
docker build -t sub-cvt-custom:latest .
docker run -d --name sub-cvt --restart=always \
  -p 25500:25500 sub-cvt-custom:latest
```
