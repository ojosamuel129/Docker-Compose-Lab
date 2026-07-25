# WordPress Docker Compose Setup

This project starts a local WordPress site with a MySQL database using Docker Compose.

## Files

- `compose.yml` - Docker Compose configuration for WordPress and MySQL

## How to run

1. Open a terminal in this folder:
   ```bash
   cd /c/Users/PSALM/Projects/WordPress/my-wordpress-site
   ```
2. Start the containers:
   ```bash
   docker compose up -d
   ```
3. Open WordPress in your browser:
   ```
   http://localhost:8080
   ```

## Stop and remove containers

```bash
docker compose down
```

## Notes

- The WordPress site persists data using Docker volumes.
- If you change `compose.yml`, restart the services with `docker compose down` and `docker compose up -d`.
