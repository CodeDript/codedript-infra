# CodeDript Infrastructure

Infrastructure setup for the CodeDript API server using Docker Compose with Nginx reverse proxy and MongoDB database.

## Overview

This repository contains the production infrastructure configuration for CodeDript, including:

- **Nginx**: Reverse proxy server with SSL/TLS support
- **API Server**: Node.js backend application
- **MongoDB**: Database for data persistence

## Prerequisites

- Docker Engine 20.10+
- Docker Compose 2.0+
- (Optional) Domain name for SSL setup

## Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/CodeDript/codedript-infra.git
   cd codedript-infra
   ```

2. **Configure environment variables**
   
   Create a `.env` file in the root directory with the following variables:
   ```env
   PORT=5000
   MONGODB_URI=mongodb://mongodb:27017
   DB_NAME=codedript
   JWT_SECRET=your_jwt_secret_here
   JWT_EXPIRES_IN=1h
   JWT_REFRESH_EXPIRES_IN=7d
   CORS_ORIGINS=http://localhost:3000
   EMAIL_SERVICE=gmail
   EMAIL_USER=your_email@gmail.com
   EMAIL_PASSWORD=your_app_password
   SUPABASE_URL=your_supabase_url
   SUPABASE_ANON_KEY=your_supabase_anon_key
   SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
   SUPABASE_BUCKET_NAME=your_bucket_name
   SEPOLIA_RPC_URL=your_sepolia_rpc_url
   PINATA_JWT=your_pinata_jwt
   PINATA_GATEWAY=your_pinata_gateway
   ```

3. **Start the services**
   ```bash
   docker compose up -d
   ```

4. **Verify the deployment**
   ```bash
   docker compose ps
   ```
   
   Access the API at `http://localhost:80`

## Services

### Nginx
- **Ports**: 80 (HTTP), 443 (HTTPS)
- **Purpose**: Reverse proxy with security headers and rate limiting
- **Configuration**: `nginx/nginx.conf`
- **SSL Config**: `nginx/ssl.conf` (for production with SSL certificates)

### API Server
- **Image**: `yujiro2hanma/codedript:latest`
- **Port**: 5000 (internal)
- **Health Check**: `/health` endpoint

### MongoDB
- **Image**: `mongo:7-jammy`
- **Volumes**: Persistent data storage
- **Database**: codedript

## SSL/HTTPS Setup

To enable HTTPS in production:

1. **Update domain names** in `nginx/ssl.conf`:
   ```nginx
   server_name api.codedript.com;  # Replace with your domain
   ```

2. **Obtain SSL certificates** using Certbot:
   ```bash
   # Install certbot
   # Run certification process for your domain
   ```

3. **Enable SSL configuration**:
   - Rename or symlink `ssl.conf` to be loaded by Nginx
   - Update the compose.yaml to use the SSL configuration

4. **Restart services**:
   ```bash
   docker compose down
   docker compose up -d
   ```

## Health Checks

All services include health checks:
- **Nginx**: Checks `/health` endpoint on port 80
- **API Server**: Node.js health endpoint on port 5000
- **MongoDB**: Mongosh ping command

## Management Commands

**View logs**:
```bash
docker compose logs -f [service-name]
```

**Restart a service**:
```bash
docker compose restart [service-name]
```

**Stop all services**:
```bash
docker compose down
```

**Stop and remove volumes**:
```bash
docker compose down -v
```

**Update to latest version**:
```bash
docker compose pull
docker compose up -d
```

## Network

All services communicate through the `codedript-network` bridge network for isolation and security.

## Volumes

- `mongodb-data`: MongoDB database files
- `mongodb-config`: MongoDB configuration

## Security Features

- Security headers (X-Frame-Options, X-Content-Type-Options, etc.)
- Client body size limits (50MB)
- Connection timeouts
- SSL/TLS support (when configured)
- Rate limiting on API endpoints

## Troubleshooting

**Container won't start**:
```bash
docker compose logs [service-name]
```

**Database connection issues**:
- Verify MongoDB is healthy: `docker compose ps`
- Check MongoDB logs: `docker compose logs mongodb`

**Nginx configuration errors**:
```bash
docker compose exec nginx nginx -t
```

## License

This infrastructure configuration is part of the CodeDript project.
