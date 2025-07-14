# Architecture Overview

This document describes the architecture of the WordPress development environment using Docker Compose with a LEMP stack.

## Stack Components

### LEMP Stack

- **L**inux: Alpine Linux (container base)
- **E**nginx: Web server and reverse proxy
- **M**ySQL: Database server (8.4.4 LTS)
- **P**HP: WordPress with PHP-FPM (8.4)

## Container Architecture

```ascii
┌──────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│     Nginx      │    │   WordPress   │    │     MySQL     │
│   (Web Server) │────│   (PHP-FPM)   │──── │  (Database)   │
│   Port: 80     │    │   Port: 9000  │    │   Port: 3306  │
└──────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                     │
         │              ┌───────────────────┐            │
         │              │  phpMyAdmin     │            │
         │              │  (DB Management)│──────────────┘
         │              │  Port: 8080     │
         │              └───────────────────┘
         │
┌───────────────────┐
│   Host System   │
│   (Development) │
└───────────────────┘
```

## Service Details

### Nginx Container

- **Image**: `nginx:alpine`
- **Purpose**: Web server, static file serving, PHP-FPM proxy
- **Port**: 80 (bound to host)
- **Configuration**: Custom `nginx.conf` with WordPress optimizations

**Key Features**:

- FastCGI PHP processing
- Static file caching
- Security headers
- Gzip compression
- Large file upload support

### WordPress Container

- **Image**: `wordpress:php8.4-fpm-alpine`
- **Purpose**: WordPress application with PHP-FPM
- **Port**: 9000 (internal, FastCGI)
- **PHP Version**: 8.4 (latest stable)

**Key Features**:

- PHP-FPM for better performance
- Alpine Linux for smaller footprint
- WordPress core files
- Custom PHP configuration

### MySQL Container

- **Image**: `mysql:8.4.4`
- **Purpose**: Database storage
- **Port**: 3306 (bound to localhost only)
- **Version**: 8.4.4 LTS for stability

**Key Features**:

- LTS version for stability
- WordPress-optimized configuration
- UTF8MB4 character set
- Performance tuning
- Health checks

### phpMyAdmin Container

- **Image**: `phpmyadmin/phpmyadmin:latest`
- **Purpose**: Database administration interface
- **Port**: 8080 (bound to host)

**Key Features**:

- Web-based MySQL administration
- Import/export capabilities
- Query execution
- Database browsing

## Network Architecture

### Docker Network

- **Type**: Bridge network (`wp_network`)
- **Purpose**: Container-to-container communication
- **Security**: Isolated from host network

### Port Mapping

```
Host System          Docker Network
localhost:80    ────▶ nginx:80
localhost:3306  ────▶ mysql:3306
localhost:8080  ────▶ phpmyadmin:80
```

## Volume Management

### Data Persistence

```
Host Directory               Container Mount Point
./wordpress/            ────▶ /var/www/html
./wp-content/           ────▶ /var/www/html/wp-content
db_data (Docker volume) ────▶ /var/lib/mysql
```

### Configuration Files

```
Host File               Container Mount Point
./nginx.conf       ────▶ /etc/nginx/nginx.conf
./my.cnf/              ────▶ /etc/mysql/conf.d/
./uploads.ini      ────▶ /usr/local/etc/php/conf.d/uploads.ini
```

## Request Flow

1. **HTTP Request** → Nginx (Port 80)
2. **Static Files** → Served directly by Nginx
3. **PHP Files** → Proxied to WordPress container via FastCGI
4. **Database Queries** → WordPress → MySQL container
5. **Response** → WordPress → Nginx → Client

## Security Architecture

### Network Security

- Containers communicate via internal Docker network
- Only necessary ports exposed to host
- MySQL bound to localhost only

### Application Security

- WordPress security headers via Nginx
- PHP security configurations
- Database access controls

### Development Security

- Environment variables for sensitive data
- Separate configuration files
- No hardcoded credentials

## Performance Optimizations

### Nginx Optimizations

- FastCGI buffering
- Static file caching
- Gzip compression
- Keep-alive connections

### PHP-FPM Optimizations

- Process management
- Memory limits
- Execution timeouts
- OPcache enabled

### MySQL Optimizations

- InnoDB buffer pool sizing
- Query cache configuration
- Connection pooling
- Optimized for WordPress queries

## Development Features

### Hot Reloading

- Theme and plugin files are mounted as volumes
- Changes reflect immediately without container restart

### Database Management

- phpMyAdmin for visual database management
- Direct MySQL access for advanced operations
- Easy backup and restore procedures

### Logging and Debugging

- Container logs accessible via Docker Compose
- PHP error logging
- MySQL query logging (configurable)
- Nginx access and error logs

## Scalability Considerations

### Horizontal Scaling

- Nginx can be configured for load balancing
- Multiple WordPress containers possible
- Database clustering support

### Resource Management

- Memory limits configurable per container
- CPU limits can be set
- Storage volumes can be expanded

## Best Practices

### Development

- Use version-controlled theme/plugin development
- Regular database backups
- Environment-specific configurations

### Security

- Regular updates of base images
- Secure password management
- Network isolation
- Principle of least privilege

### Performance

- Monitor container resource usage
- Optimize database queries
- Use appropriate caching strategies
- Regular performance testing
