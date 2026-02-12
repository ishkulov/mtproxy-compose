# Telegram MTProxy via `telemt-docker` + SOCKS5

## Files
- `compose.yml` - single `telemt` service.
- `.env.example` - runtime variables template.
- Image: `whn0thacked/telemt-docker:latest` (as in upstream README).

## Init and run
1. Create local env file:
   ```bash
   cp .env.example .env
   ```
2. Edit `.env` and set values:
   - `SECRET` - 32-byte hex value without `ee` prefix.
   - `UPSTREAMS` - one or more upstreams, comma-separated.
3. Start proxy:
   ```bash
   docker compose up -d
   ```
4. Watch logs:
   ```bash
   docker compose logs -f telemt
   ```

## `UPSTREAMS` format
- Single SOCKS5 upstream:
  ```text
  socks5://user:pass@host:port
  ```
- Multiple upstreams:
  ```text
  socks5://user:pass@host1:1080,socks5://user:pass@host2:1080
  ```

## Validation and troubleshooting
```bash
docker compose config
docker compose ps
docker compose logs --tail=100 telemt
```

## Telegram client URL
Use the proxy URL/secret emitted by `telemt` logs as the primary source.

Template (if constructing manually):
```text
tg://proxy?server=<IP>&port=${PORT}&secret=ee${SECRET}${HEX_DOMAIN}
```

Example:
```bash
IP="$(curl -s ifconfig.me)"
source .env
HEX_DOMAIN="$(printf '%s' "${FAKE_TLS_DOMAIN}" | xxd -p -c 256)"
echo "tg://proxy?server=${IP}&port=${PORT}&secret=ee${SECRET}${HEX_DOMAIN}"
```
