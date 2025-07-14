# Setup Guide

This guide will help you set up and run the WordPress development environment using Docker Compose.

## Prerequisites

- Docker Desktop installed and running
- Docker Compose (included with Docker Desktop)
- Git (for cloning the repository)

## Quick Start

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd wp-compose
   ```

2. **Set up environment variables**

   ```bash
   cp .env.sample .env
   ```

   Edit the `.env` file to customize your configuration if needed.

3. **Start the development environment**

   ```bash
   docker-compose up -d
   ```

4. **Access your WordPress site**
   - WordPress: <http://localhost> (or <http://localhost:80>)
   - phpMyAdmin: <http://localhost:8080>

## Environment Variables

The following environment variables can be customized in your `.env` file:

### MySQL Configuration

- `MYSQL_VERSION`: MySQL version (default: 8.4.4)
- `MYSQL_ROOT_PASSWORD`: Root password for MySQL
- `MYSQL_DATABASE`: WordPress database name
- `MYSQL_USER`: WordPress database user
- `MYSQL_PASSWORD`: WordPress database password
- `DB_PORT`: MySQL port (default: 3306)

### WordPress Configuration

- `WORDPRESS_VERSION`: WordPress image version (default: php8.4-fpm-alpine)
- `WORDPRESS_DB_HOST`: Database host (default: db)
- `WORDPRESS_PORT`: WordPress port (default: 80)

### phpMyAdmin Configuration

- `PHPMYADMIN_VERSION`: phpMyAdmin version (default: latest)
- `PHPMYADMIN_PORT`: phpMyAdmin port (default: 8080)

### Nginx Configuration

- `NGINX_VERSION`: Nginx version (default: alpine)

## Development Workflow

### Theme Development

1. Navigate to the `wp-content/themes/` directory
2. Create or modify your theme files
3. Changes are automatically reflected in the running WordPress instance

### Plugin Development

1. Navigate to the `wp-content/plugins/` directory
2. Create or modify your plugin files
3. Activate plugins through the WordPress admin interface

### Database Management

- Access phpMyAdmin at <http://localhost:8080>
- Use the credentials from your `.env` file
- Export/import databases as needed

## Useful Commands

### Start the environment

```bash
docker-compose up -d
```

### Stop the environment

```bash
docker-compose down
```

### View logs

```bash
docker-compose logs -f
```

### Restart a specific service

```bash
docker-compose restart wordpress
```

### Access container shells

```bash
# WordPress container
docker-compose exec wordpress sh

# MySQL container
docker-compose exec db mysql -u root -p

# Nginx container
docker-compose exec nginx sh
```

### Database Operations

```bash
# Create database backup
docker-compose exec db mysqldump -u root -p wordpress_db > backup.sql

# Restore database backup
docker-compose exec -T db mysql -u root -p wordpress_db < backup.sql
```

## Troubleshooting

### Common Issues

1. **Port conflicts**
   - Check if ports 80, 3306, or 8080 are already in use
   - Modify ports in `.env` file if needed

2. **Permission issues**
   - Ensure Docker has proper permissions
   - On Windows, make sure Docker Desktop is running as administrator if needed

3. **Database connection errors**
   - Wait for MySQL to fully initialize (check logs)
   - Verify database credentials in `.env` file

4. **WordPress installation issues**
   - Delete `wordpress/wp-config.php` and restart containers
   - Run WordPress installation wizard again

### Viewing Logs

```bash
# All services
docker-compose logs

# Specific service
docker-compose logs wordpress
docker-compose logs db
docker-compose logs nginx
```

## File Structure

```sh
wp-compose/
├── docs/                  # Documentation
├── wordpress/             # WordPress core files
├── wp-content/            # WordPress content (themes, plugins, uploads)
├── ssl_certs/             # SSL certificates (if using HTTPS)
├── docker-compose.yml     # Docker Compose configuration
├── nginx.conf             # Nginx configuration
├── my.cnf/                # MySQL configuration directory
├── uploads.ini            # PHP upload settings
└── .env                   # Environment variables
```

## Performance Tips

1. **Use volumes for persistent data**
   - Database data is stored in a Docker volume
   - WordPress files are mounted from host

2. **Optimize MySQL settings**
   - Adjust configuration files in `my.cnf/` directory for your development needs
   - Increase buffer pool size for better performance

3. **Enable opcache**
   - PHP opcache is enabled by default in the WordPress container
   - Adjust settings in `uploads.ini` if needed

## Security Considerations

- Change default passwords in `.env` file
- Keep WordPress and plugins updated
- Use strong passwords for database access
- Bind services to localhost (127.0.0.1) in production

## IDE Integration

### PHPStorm

Since WordPress core files are mounted in the `./wordpress` directory, PHPStorm can directly access the WordPress code for proper code navigation and autocompletion.

**Setup steps:**

1. Open PHPStorm and add the project folder
2. Add the `wordpress` folder to your project for core WordPress code navigation
3. Configure include paths to include the WordPress installation path
4. Install WordPress and PHP plugins for enhanced support

### Visual Studio Code

Visual Studio Code supports multi-root workspaces, which is perfect for WordPress development.

**Recommended setup:**

1. Add the `wordpress` folder to your workspace for core WordPress code navigation
2. Add your theme/plugin folders from `wp-content` to the same workspace
3. Install recommended extensions:
   - PHP Intelephense
   - WordPress Snippets
   - Docker extension
4. Configure PHP executable path to point to the container

## SSL/HTTPS Configuration

For HTTPS development, you can add SSL certificates to the `ssl_certs` directory:

**Required files:**

- `cert.pem`: SSL certificate
- `key.pem`: Private key

**Note:** The current nginx.conf is configured for HTTP development. For HTTPS, you'll need to modify the nginx configuration to include SSL settings.

## Additional Resources

### Helpful Links

- [How To Install WordPress With Docker Compose](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose)
- [Nginx WordPress FastCGI Caching](https://www.nginx.com/blog/wordpress-nginx-fastcgi-cache/)
- [PHP-FPM Configuration](https://www.php.net/manual/en/install.fpm.configuration.php)
- [WordPress Docker Official Documentation](https://hub.docker.com/_/wordpress)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

### WordPress Development Resources

- [WordPress Developer Handbook](https://developer.wordpress.org/)
- [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/)
- [WordPress Plugin Development](https://developer.wordpress.org/plugins/)
- [WordPress Theme Development](https://developer.wordpress.org/themes/)
