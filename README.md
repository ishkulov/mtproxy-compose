# MTProxy (Fake TLS) via Docker Compose

## Files
- `compose.yml` - MTProxy service and one-shot secret generator.
- `.env.example` - runtime variables template.

## Init and run
1. Create local env file:
   ```bash
   cp .env.example .env
   ```
2. Generate Fake TLS secret:
   ```bash
   docker compose run --rm secretgen
   ```
3. Put generated `ee...` value into `.env`:
   ```bash
   SECRET=ee...
   ```
4. Start proxy:
   ```bash
   docker compose up -d mtproxy
   ```

## Validation and troubleshooting
```bash
docker compose config
docker compose logs --tail=50 mtproxy
docker ps
```

## Telegram client URL
Template:
```text
tg://proxy?server=<IP>&port=${PORT}&secret=${SECRET}
```

Example with shell values:
```bash
IP="$(curl -s ifconfig.me)"
source .env
echo "tg://proxy?server=${IP}&port=${PORT}&secret=${SECRET}"
```
