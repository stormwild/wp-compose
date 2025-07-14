# WordPress Docker Development Environment

A modern WordPress development environment using Docker Compose with a LEMP stack (Linux, Nginx, MySQL 8.4.4 LTS, PHP 8.4 FPM).

## Quick Start

1. **Clone and setup**

   ```bash
   git clone <repository-url>
   cd wp-compose
   cp .env.sample .env
   ```

2. **Start the environment**

   ```bash
   docker-compose up -d
   ```

3. **Access your site**
   - WordPress: <http://localhost>
   - phpMyAdmin: <http://localhost:8080>

## Documentation

Comprehensive documentation is available in the [`docs/`](docs/) folder:

- 📋 **[Setup Guide](docs/setup-guide.md)** - Detailed installation and configuration instructions
- 🏗️ **[Architecture Overview](docs/architecture.md)** - Technical architecture and container details  
- 📁 **[Directory Structure](docs/directory-structure.md)** - File and folder organization
- 📝 **[Changelog](docs/CHANGELOG.md)** - Version history and changes

## Features

- **Modern Stack**: WordPress with PHP 8.4 FPM + Nginx + MySQL 8.4.4 LTS
- **Development Optimized**: Hot reloading, debugging tools, and performance tuning
- **Database Management**: phpMyAdmin included for easy database operations
- **Security**: Best practices with security headers and isolated networking
- **Customizable**: Environment variables for easy configuration

## Architecture

This setup uses a LEMP stack with the following components:

- **WordPress** with PHP 8.4 FPM (Alpine-based for smaller footprint)
- **Nginx** as the web server and reverse proxy
- **MySQL 8.4.4 LTS** as the database server
- **phpMyAdmin** for database management

## Development Workflow

The `wp-content` folder contains your themes and plugins, while the `wordpress` folder contains the core WordPress files.

### Theme Development

1. Create/edit themes in `wp-content/themes/`
2. Changes are reflected immediately
3. No container restart required

### Plugin Development  

1. Create/edit plugins in `wp-content/plugins/`
2. Activate through WordPress admin
3. Debug with container logs if needed

## Common Commands

```bash
# Start environment
docker-compose up -d

# Stop environment  
docker-compose down

# View logs
docker-compose logs -f

# Access WordPress container
docker-compose exec wordpress sh
```

For detailed setup instructions, troubleshooting, and advanced usage, see the [Setup Guide](docs/setup-guide.md).

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Update documentation if needed
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE.txt](LICENSE.txt) file for details.
