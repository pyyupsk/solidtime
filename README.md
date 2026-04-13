# solidtime (local)

Personal local setup for [solidtime](https://github.com/solidtime-io/solidtime) — a modern open source time tracker, great for freelancers. Runs on `localhost` only, not intended for public access.

## Requirements

- Docker

## Setup

### 1. Configure environment files

Copy the example files and fill in the values:

```bash
cp .env.example .env
cp laravel.env.example laravel.env
```

### 2. Generate security keys

```bash
docker compose run --rm scheduler php artisan self-host:generate-keys
```

Copy the output values into `laravel.env`:

```env
APP_KEY=
PASSPORT_PRIVATE_KEY=
PASSPORT_PUBLIC_KEY=
```

### 3. Start containers

```bash
docker compose up -d
```

Database migrations run automatically on first start.

### 4. Create your account

```bash
docker compose exec scheduler php artisan admin:user:create "Your Name" "your@email.com" --verify-email
```

### 5. Open the app

Navigate to [http://localhost:8000](http://localhost:8000) and log in with the email and password from the previous step.

## Updating

```bash
docker compose pull
docker compose up -d
docker compose exec scheduler php artisan migrate --force
```

## Useful commands

```bash
# View logs
docker compose logs -f

# Stop
docker compose down

# Stop and remove all data (destructive)
docker compose down -v
```

## Notes

- This setup is for **local personal use only** — no HTTPS, no reverse proxy, no public domain
- Emails use the `log` driver — no SMTP needed, all mail output goes to logs
- PDF export is handled by [Gotenberg](https://gotenberg.dev) included in the compose setup
