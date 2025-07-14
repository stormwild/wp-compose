# Changelog

All notable changes to this WordPress Docker development environment will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased] - 2025-07-14

### Added

- MySQL 8.4.4 LTS support for improved stability and performance
- MySQL configuration directory (`my.cnf/`) with WordPress-specific settings
- PHP 8.4 FPM Alpine support for latest PHP features and security updates
- Enhanced documentation structure in `docs/` folder
- Directory structure documentation (`docs/directory-structure.md`)

### Changed

- **BREAKING**: Updated MySQL version from 8.0 to 8.4.4 LTS
- **BREAKING**: Updated WordPress image from PHP 8.3 to PHP 8.4 FPM Alpine
- Organized MySQL configuration files in `my.cnf/` directory structure
- Reorganized documentation files into `docs/` folder
- Updated `.env` file with new version specifications

### Fixed

- MySQL configuration file structure (was incorrectly set as directory)
- Docker Compose volume mapping for MySQL configuration

### Technical Details

#### MySQL Configuration Improvements

- Added WordPress-optimized buffer pool settings
- Configured UTF8MB4 character set for full Unicode support
- Enhanced logging for development debugging
- Optimized connection and timeout settings
- Added performance tuning for WordPress workloads

#### WordPress Container Updates

- Upgraded to PHP 8.4 FPM Alpine for better performance
- Maintained existing volume mappings and environment variables
- Preserved security and upload configurations

#### Infrastructure Changes

- Maintained existing LEMP stack architecture
- Preserved Nginx configuration and optimization
- Kept phpMyAdmin integration for database management
- Maintained network isolation and security practices

### Migration Notes

If upgrading from previous versions:

1. **Backup your database** before upgrading MySQL versions
2. Update your `.env` file with new version numbers:

   ```
   MYSQL_VERSION=8.4.4
   WORDPRESS_VERSION=php8.4-fpm-alpine
   ```

3. Configuration files are organized in the `my.cnf/` directory
4. Test your WordPress installation after upgrade

### Security Considerations

- MySQL 8.4.4 includes latest security patches
- PHP 8.4 provides enhanced security features
- Maintained existing Nginx security headers
- Preserved container isolation and network security
