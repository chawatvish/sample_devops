# sample_devops

A minimal Go HTTP API with automated CI/CD to a DigitalOcean droplet via Docker.

## API Endpoints

| Method | Path      | Description         |
|--------|-----------|---------------------|
| GET    | /         | Hello World (JSON)  |
| GET    | /health   | Health check (JSON) |

## Local Development

```bash
# Run locally
PORT=8080 go run ./main.go

# Run tests
go test -v -race ./...

# Build Docker image
docker build -t go-api:local .

# Run container
docker run --rm -p 8080:8080 go-api:local
```

## CI/CD Pipeline

Push to `main` triggers three sequential GitHub Actions jobs:

1. **Test** — `go test -v -race ./...`
2. **Build & Push** — Docker image pushed to `ghcr.io/chawatvish/sample_devops:<git-sha>`
3. **Deploy** — SSH into droplet, pull new image, restart container

## Required GitHub Secrets

| Secret            | Description                              |
|-------------------|------------------------------------------|
| `DROPLET_HOST`    | Public IP of the DigitalOcean droplet    |
| `DROPLET_USER`    | SSH username (e.g. `root`)               |
| `DROPLET_SSH_KEY` | Full PEM-encoded private key content     |

`GITHUB_TOKEN` is provided automatically — no manual setup needed.

## Container Image

```
ghcr.io/chawatvish/sample_devops:sha-<git-sha>
ghcr.io/chawatvish/sample_devops:latest
```
