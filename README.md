# PPC-Infra

Infrastructure for PPC

Docker Compose Configuration for Traefik and WordPress Deployment

This repository contains a Docker Compose configuration file for setting up a Traefik reverse proxy along with a WordPress instance. The setup includes automatic SSL certificate provisioning through Let's Encrypt using Traefik's built-in features.

## Prerequisites

- Docker
- Docker Compose

## Getting Started

1. Clone this repository:

   ```bash
   git clone https://github.com/kernelguardian/PPC-Infra.git
   cd PPC-Infra
   ```

2. Create a `.env` file in the same directory as the `docker-compose.yml` file. Check `example.env` for more info. Fill in the environment variables:

   ```env
   TRAEFIK_SSL_EMAIL=your-email@example.com
   WORDPRESS_DB_HOST=db
   WORDPRESS_DB_USER=wordpressuser
   WORDPRESS_DB_PASSWORD=wordpresspassword
   WORDPRESS_DB_NAME=wordpressdb
   MYSQL_ROOT_PASSWORD=rootpassword
   ```

3. Run the Docker Compose command to start the services:
   ```bash
   docker-compose up -d
   ```

## Services

### Traefik

- **Image**: traefik:latest
- **Ports**: 80 (HTTP), 443 (HTTPS), 8080 (Traefik Dashboard)
- **Configuration**:
  - Automatic SSL certificates using Let's Encrypt
  - HTTP to HTTPS redirection
- **Volumes**: `/var/run/docker.sock:/var/run/docker.sock:ro`

### WordPress

- **Image**: wordpress:latest
- **Environment Variables**:
  - WORDPRESS_DB_HOST: Database host (set to `db`)
  - WORDPRESS_DB_USER: Database user
  - WORDPRESS_DB_PASSWORD: Database password
  - WORDPRESS_DB_NAME: Database name
- **Labels**: Traefik labels for routing and SSL certificate resolution
- **Volumes**: `wordpress_data:/var/www/html`
- **Dependencies**: db service

### MySQL Database (db)

- **Image**: mysql:5.7
- **Environment Variables**:
  - MYSQL_ROOT_PASSWORD: Root user password
  - MYSQL_DATABASE: WordPress database name (same as WORDPRESS_DB_NAME)
  - MYSQL_USER: WordPress database user (same as WORDPRESS_DB_USER)
  - MYSQL_PASSWORD: WordPress database user password
- **Volumes**: `db_data:/var/lib/mysql`

## Notes

- Adjust the domain names and routing rules in the WordPress service labels as needed.
- The commented-out `certbot` service can be used to manually obtain SSL certificates if desired.
- Use portainer above all to go YOLO

## License

This project is distributed under the [MIT License](LICENSE).

For questions or assistance, please contact me at hello@kernelguardian.com or visit my website at [www.kernelguardian.com](http://www.kernelguardian.com).
