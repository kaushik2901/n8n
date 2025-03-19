# n8n Docker Deployment

This project contains configuration files for deploying [n8n](https://n8n.io/) - a workflow automation tool - using Docker Compose with PostgreSQL as the database.

## Overview

n8n is a fair-code licensed workflow automation tool that allows you to connect different services and applications to automate tasks without coding. This setup provides:

- n8n workflow engine
- PostgreSQL database for persistent storage
- Docker Compose configuration for easy deployment
- Environment variable configuration

## Prerequisites

- Docker and Docker Compose installed
- A server or hosting environment where you can expose ports
- (Optional) A domain name if you want to use the webhook functionality

## Files

The project consists of two main files:

1. `docker-compose.yml` - Contains the Docker Compose configuration for n8n and PostgreSQL
2. `.env` - Environment variables for customizing your deployment (copy from `.env.example`)

## Setup Instructions

### 1. Clone the repository

```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. Configure environment variables

```bash
cp .env.example .env
```

Edit the `.env` file to set your custom values:

- `NAMESPACE`: Namespace for your containers (default: main)
- `N8N_HOST` and `N8N_DOMAIN`: Your domain name for n8n
- `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`: Database credentials
- `JWT_SECRET` and `ENCRYPTION_KEY`: Security keys (should be changed for production)

### 3. Create required network

```bash
docker network create web
```

### 4. Start the containers

```bash
docker-compose up -d
```

### 5. Access n8n

Once everything is up and running, you can access n8n at:

- `https://your-domain.com` (if you configured a domain)
- `http://localhost:5678` (for local access)

## Data Persistence

The setup uses Docker volumes to ensure your data persists between container restarts:

- n8n workflows and settings: `./data/n8n`
- PostgreSQL database: `./data/postgres`

## Health Checks

Both services include health checks to ensure they're running correctly:

- n8n checks its health endpoint every 30 seconds
- PostgreSQL verifies database connection every 10 seconds

## Environment Variables

See the `.env.example` file for all configurable options. Key settings include:

| Variable | Description | Default |
|----------|-------------|---------|
| NAMESPACE | Container name prefix | main |
| N8N_HOST | Host where n8n is running | localhost |
| N8N_DOMAIN | Domain for webhooks | n8n.example.com |
| POSTGRES_DB | Database name | n8n |
| POSTGRES_USER | Database username | n8n |
| POSTGRES_PASSWORD | Database password | password |
| JWT_SECRET | Secret for JWT tokens | (example in .env.example) |
| ENCRYPTION_KEY | Key for encrypting credentials | (example in .env.example) |

## Security Notes

For production deployments:
- Change all default passwords and security keys
- Consider setting up a reverse proxy with SSL termination
- Review n8n's security documentation for additional measures

## Updating n8n

To update to a newer version of n8n:

```bash
docker-compose pull
docker-compose up -d
```

## Resources

- [n8n Documentation](https://docs.n8n.io/)
- [Docker Documentation](https://docs.docker.com/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)