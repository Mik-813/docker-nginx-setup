# Docker Nginx with Let's Encrypt SSL

This setup provides a Nginx web server with automatic SSL certificate management using Let's Encrypt certbot in Docker containers.

## Prerequisites

- Docker
- Docker Compose
- A domain name pointing to your server
- Available ports on your host (configurable in .env)

## Setup Instructions

1. Clone this repository to your server

2. Configure your environment:
   - Copy the `.env.example` file to `.env` (if not already done)
   - Edit the `.env` file and set:
     - `DOMAIN`: Your main domain (e.g., example.com)
     - `DOMAIN_WWW`: Your www domain (e.g., www.example.com)
     - `HTTP_PORT`: Your HTTP port (default: 20516)
     - `HTTPS_PORT`: Your HTTPS port (default: 30516)
     - `CERTBOT_EMAIL`: Your email for SSL certificate notifications
     - `STAGING`: Set to 1 for testing, 0 for production

3. Initialize SSL certificates:
   ```bash
   ./init-letsencrypt.sh
   ```
 
4. Start the services:
   ```bash
   docker compose up -d
   ```

## How it works

- Nginx container serves your website and handles SSL termination
- Certbot container automatically manages SSL certificates
- Certificates are renewed automatically every 12 hours if needed
- Nginx configuration is reloaded automatically when certificates are renewed

## Directory Structure

- `nginx/` - Contains Nginx configuration files
- `data/certbot/` - Contains Let's Encrypt certificates and renewal information
- `docker-compose.yml` - Defines the services and their configuration
- `init-letsencrypt.sh` - Script to initialize SSL certificates

## Customization

To customize the Nginx configuration, edit the files in the `nginx/` directory. After making changes, reload the Nginx configuration:

```bash
docker compose exec nginx nginx -s reload
```

## Troubleshooting

- If you see certificate errors, make sure your domain is properly pointing to your server
- Check Certbot logs: `docker-compose logs certbot`
- Check Nginx logs: `docker-compose logs nginx`

## Security Notes

- The staging mode is disabled by default. To test your setup, set `staging=1` in `init-letsencrypt.sh`
- Keep your email address updated in `init-letsencrypt.sh` for important notifications
- Regularly update your Docker images for security patches