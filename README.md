# Telegram MTProxy via `telemt-docker` (upstream-aligned)

## Files
- `compose.yml` - `telemt` service in the same style as upstream README.
- `telemt.toml` - main Telemt config mounted to `/etc/telemt.toml`.
- `.env.example` - optional runtime variables (`RUST_LOG`).

## Init and run
1. Create local env file:
   ```bash
   cp .env.example .env
   ```
2. Generate secret (32 hex chars):
   ```bash
   openssl rand -hex 16
   ```
3. Edit `telemt.toml`:
   - set `[censorship].tls_domain`
   - paste generated secret into `[access.users].docker`
   - set SOCKS5 upstream in `[[upstreams]]` (`address`, `username`, `password`)
4. Start:
   ```bash
   docker compose up -d
   ```
5. Logs:
   ```bash
   docker compose logs -f telemt
   ```

## Secret placement
Put your generated value here:
```toml
[access.users]
docker = "0123456789abcdef0123456789abcdef"
```

## Validation and troubleshooting
```bash
docker compose config
docker compose ps
docker compose logs --tail=100 telemt
```

## SOCKS5 in `telemt.toml`
Without auth:
```toml
[[upstreams]]
type = "socks5"
address = "1.2.3.4:1234"
enabled = true
weight = 1
```

With auth:
```toml
[[upstreams]]
type = "socks5"
address = "1.2.3.4:1234"
username = "user"
password = "pass"
enabled = true
weight = 1
```

## Notes
- Image: `whn0thacked/telemt-docker:latest` (as in upstream README).
- Container starts as `telemt /etc/telemt.toml`; config file is mandatory.
