# Local WordPress Development Using Docker LEMP Stack

## Pre-requisites

- Docker

## Architecture

This setup uses a LEMP stack (Linux, Nginx, MySQL, PHP-FPM) with the following components:

- WordPress with PHP-FPM (Alpine-based for smaller footprint)
- Nginx as the web server
- MySQL 8.0 as the database
- phpMyAdmin for database management

## Usage

The `wp-content` folder will contain the `plugins` and `themes` folders from the wordpress container.
The `wordpress` folder contains the core WordPress files.

Develop your themes and plugins from within these folders.

### Environment Variables

The stack uses environment variables for configuration. Copy `.env.sample` to `.env` to customize:

```bash
cp .env.sample .env
```

### Custom Volumes

Several volumes are mounted for development:

```yml
volumes:
       - ./wordpress:/var/www/html
       - ./wp-content:/var/www/html/wp-content
       - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
       - ./nginx.conf:/etc/nginx/nginx.conf
       - ./ssl_certs:/etc/ssl/private
```

### SSL Certificates

Before starting the services, ensure you have SSL certificates in the `ssl_certs` directory:

- `cert.pem`: SSL certificate
- `key.pem`: Private key

### Starting Services

```bash
docker-compose up -d
```

The following services will be available:

- WordPress:
  - HTTP: [http://localhost](http://localhost) (redirects to HTTPS)
  - HTTPS: [https://localhost](https://localhost)
- phpMyAdmin: [http://localhost:8080](http://localhost:8080)

## IDE Access to WordPress

### PHPStorm

Since WordPress core files are now mounted in the `./wordpress` directory, PHPStorm can directly access the WordPress code for proper code navigation and autocompletion.

In PHPStorm you can also use include paths to include the path to your WordPress install.

### Visual Studio Code

Visual Studio Code allows for multi-root workspaces. The WordPress core files are now available in the `./wordpress` directory, so you can:

1. Add the `wordpress` folder to your workspace for core WordPress code navigation
2. Add your theme/plugin folders from `wp-content` to the same workspace
3. Install PHP extensions to get proper code intelligence

## Database (MySQL 8.0)

MySQL 8.0 is now fully supported and is the default version. No special configuration is needed.

Check running containers:

```bash
docker ps
```

Example output:

```sh
CONTAINER ID   IMAGE                          COMMAND                  CREATED         STATUS                         PORTS                                 NAMES        
f4af3e7c8369   nginx:latest                   "/docker-entrypoint.…"   2 minutes ago   Restarting (1) 8 seconds ago                                         nginx_proxy  
b3b47872d476   phpmyadmin/phpmyadmin:latest   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes (healthy)         127.0.0.1:8080->80/tcp                phpmyadmin   
49eaded9adc9   wordpress:latest               "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes                   80/tcp                                wordpress_dev
9b389a67fed1   mysql:8.0                      "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes (healthy)         127.0.0.1:3306->3306/tcp, 33060/tcp   mysql_db    
```

## Performance Features

The LEMP stack configuration includes several performance optimizations:

- PHP-FPM for efficient PHP processing
- Nginx FastCGI caching for dynamic content
- Static file caching with appropriate headers
- Gzip compression for text-based content
- HTTP/2 support for improved performance
- Alpine-based images for smaller footprint

## Security Features

- All services are bound to 127.0.0.1 by default, preventing external access
- HTTPS support with modern SSL configuration
- HTTP/2 support
- Security headers configured in Nginx
- Protection against sensitive file access
- WordPress-specific security rules
- Container health checks implemented for all services
- Environment variable support for secure configuration

## Database Management

### Using phpMyAdmin

Access phpMyAdmin through [http://localhost:8080](http://localhost:8080)

### Direct MySQL Access

```bash
docker exec -it mysql_db bash
mysql -u wordpress_user -p
```

When prompted, enter your MySQL password (default: wordpress_password)

## Cleanup

Stop and remove containers:

```bash
docker-compose down
```

Verify:

```bash
docker ps -a
```

Remove volumes:

```bash
docker volume ls
docker volume rm wp-compose_db_data
```

## Resources

- [How To Install WordPress With Docker Compose](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose)
- [Nginx WordPress FastCGI Caching](https://www.nginx.com/blog/wordpress-nginx-fastcgi-cache/)
- [PHP-FPM Configuration](https://www.php.net/manual/en/install.fpm.configuration.php)
