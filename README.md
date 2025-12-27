# CodeDript Infrastructure

Docker-based deployment configuration for the CodeDript platform. This setup includes Nginx reverse proxy, the backend API server, and frontend client application.

## Overview

This infrastructure configuration provides:

- **Nginx**: Reverse proxy with SSL/TLS support
- **Backend Server**: Node.js API application
- **Frontend Client**: React application
- **Docker Compose**: Service orchestration

## Prerequisites

- Docker Engine 20.10 or higher
- Docker Compose 2.0 or higher
- Domain name (optional, for SSL)

## Quick Start

1. Configure environment variables:

   ```bash
   cp .env.example .env
   ```

2. Edit `.env` file with your configuration

3. Start all services:

   ```bash
   docker compose up -d
   ```

4. Verify deployment:
   ```bash
   docker compose ps
   ```

Access the application at `http://localhost`

## Services

### Nginx

- Ports: 80 (HTTP), 443 (HTTPS)
- Reverse proxy with security headers
- Configuration: `nginx/nginx.conf`

### Backend API

- Internal port: 5000
- Health check endpoint: `/health`
- Image: Node.js application

### Frontend Client

- Internal port: 80
- Built with React and Vite
- Served through Nginx

## Configuration

### Environment Variables

See `.env.example` for all required variables.

Key variables:

```
PORT=5000
SUPABASE_URL=your_supabase_url
JWT_SECRET=your_jwt_secret
PINATA_JWT=your_pinata_jwt
ETHEREUM_RPC_URL=your_rpc_url
```

### SSL/HTTPS Setup

For production with SSL:

1. Update domain in `nginx/ssl.conf`
2. Place SSL certificates in `ssl/` directory:
   - `ssl/cert.pem`
   - `ssl/key.pem`
3. Restart services:
   ```bash
   docker compose restart nginx
   ```

## Management Commands

View logs:

```bash
docker compose logs -f
```

Restart a service:

```bash
docker compose restart [service-name]
```

Stop all services:

```bash
docker compose down
```

Update to latest:

```bash
docker compose pull
docker compose up -d
```

## Project Structure

```
codedript-infra/
├── nginx/
│   ├── nginx.conf       # Main nginx config
│   └── ssl.conf         # SSL configuration
├── compose.yaml         # Docker compose file
└── .env.example         # Environment template
```

## Troubleshooting

Check service status:

```bash
docker compose ps
```

View service logs:

```bash
docker compose logs [service-name]
```

Test nginx configuration:

```bash
docker compose exec nginx nginx -t
```

## License

MIT
