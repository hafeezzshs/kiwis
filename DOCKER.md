# Docker Setup for Kiwis

This setup provides Dockerfiles for both `kiwis-web` (Next.js) and `kiwis-worker` (Go) applications using their existing Makefiles.

## Prerequisites

- Docker installed
- Both submodules initialized: `git submodule update --init --recursive`

## kiwis-web (Next.js)

### Setup and Build:
```bash
cd kiwis-web

# Copy and configure environment file
cp .env.example .env
# Edit .env with your actual values

# Build the Docker image
docker build -t kiwis-web .

# Run the container
docker run -p 3000:3000 --env-file .env kiwis-web
```

**Access**: http://localhost:3000

### How it works:
- Uses `make install` to install dependencies
- Uses `make build` to build the production app  
- Uses `make start` to run the server

## kiwis-worker (Go)

### Setup and Build:
```bash
cd kiwis-worker

# Copy and configure environment file
cp .env.example .env
# Edit .env with your actual DATABASE_URL and other configs

# Build the Docker image
docker build -t kiwis-worker .

# Run the container
docker run -p 8080:8080 --env-file .env kiwis-worker
```

**Access**: http://localhost:8080

### How it works:
- Uses `make deps` to download Go dependencies
- Uses `make build` to compile the binary
- Runs the compiled binary directly

## Development vs Docker

### For Development (recommended):
Use the Makefiles directly on your host machine for hot reload:
```bash
# Web development server
cd kiwis-web && make dev

# Worker development (with auto-restart)  
cd kiwis-worker && make run
```

### For Production/Testing:
Use Docker for consistent deployment:
```bash
# Build and run web
cd kiwis-web
docker build -t kiwis-web .
docker run -p 3000:3000 --env-file .env kiwis-web

# Build and run worker
cd kiwis-worker
docker build -t kiwis-worker .
docker run -p 8080:8080 --env-file .env kiwis-worker
```

## Database

For the worker, you'll need a PostgreSQL database. You can:

1. **Use a local PostgreSQL installation**
2. **Use a cloud database** (AWS RDS, Supabase, etc.)
3. **Run PostgreSQL in Docker**:
   ```bash
   docker run -d \
     --name kiwis-postgres \
     -e POSTGRES_DB=kiwis_db \
     -e POSTGRES_USER=kiwis_user \
     -e POSTGRES_PASSWORD=kiwis_password \
     -p 5432:5432 \
     postgres:15-alpine
   ```
   Then set `DATABASE_URL=postgresql://kiwis_user:kiwis_password@localhost:5432/kiwis_db` in your worker's .env

## Database Migrations

To run migrations for the worker:
```bash
# If using local development
cd kiwis-worker && make migrate-up

# If worker is running in Docker, you can exec into it
docker exec -it <container-name> ./kiwis-worker
```

## Useful Commands

```bash
# View container logs
docker logs <container-name>

# View live logs
docker logs -f <container-name>

# Stop a running container
docker stop <container-name>

# Remove containers and images
docker rm <container-name>
docker rmi kiwis-web kiwis-worker

# Rebuild after code changes
docker build -t kiwis-web ./kiwis-web --no-cache
docker build -t kiwis-worker ./kiwis-worker --no-cache

# Run containers in background
docker run -d -p 3000:3000 --env-file .env --name kiwis-web-container kiwis-web
docker run -d -p 8080:8080 --env-file .env --name kiwis-worker-container kiwis-worker
```

## Environment Variables

### kiwis-web/.env
Configure your Next.js environment variables (API endpoints, auth keys, etc.)

### kiwis-worker/.env  
Configure your Go application variables including `DATABASE_URL` pointing to your PostgreSQL instance.

## Troubleshooting

- **Port conflicts**: Change the `-p` port mappings if 3000/8080 are already in use
- **Build failures**: Ensure your Makefiles work locally first: `cd kiwis-web && make install && make build`
- **Database connection issues**: Verify your `DATABASE_URL` in kiwis-worker/.env points to an accessible PostgreSQL instance
- **Container not starting**: Check logs with `docker logs <container-name>` to see error messages
