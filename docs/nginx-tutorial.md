# Nginx Configuration Tutorial for WordPress LEMP Stack

## Main Context (Global Settings)

```nginx
user nginx;                    # Run nginx as 'nginx' user
worker_processes auto;         # Auto-detect CPU cores for worker processes
pid /var/run/nginx.pid;       # Process ID file location
```

## Events Context (Connection Processing)

```nginx
events {
    worker_connections 1024;   # Max connections per worker process
}
```

## HTTP Context (Web Server Settings)

```nginx
http {
    # MIME Types
    include /etc/nginx/mime.types;     # Include standard MIME type definitions
    default_type application/octet-stream;  # Default MIME type for unknown files
    
    # Custom MIME types for fonts
    types {
        font/ttf    ttf;
        font/woff   woff;
        font/woff2  woff2;
    }
    
    # Logging Configuration
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                   '$status $body_bytes_sent "$http_referer" '
                   '"$http_user_agent" "$http_x_forwarded_for"';
    
    access_log /var/log/nginx/access.log main;  # Access log location
    error_log /var/log/nginx/error.log warn;    # Error log location
    
    # Performance Settings
    sendfile on;               # Efficient file serving
    keepalive_timeout 65;      # Keep connections alive for 65 seconds
}
```

## Upstream Context (Backend PHP-FPM)

```nginx
upstream php {
    server wordpress:9000;     # PHP-FPM backend (Docker service name:port)
}
```

**Explanation**: This defines a backend pool named "php" that points to your WordPress container's PHP-FPM service on port 9000.

## Server Context (Virtual Host)

```nginx
server {
    listen 80;                 # Listen on port 80 (HTTP)
    listen [::]:80;           # Listen on IPv6
    server_name localhost;     # Server name (domain)
    
    root /var/www/html;       # Document root (WordPress files)
    index index.php;          # Default index file
    
    client_max_body_size 100M; # Max upload size (important for WordPress)
}
```

## Location Contexts (URL Routing)

### 1. Main Location Block

```nginx
location / {
    try_files $uri $uri/ /index.php?$args;
}
```

**Explanation**:

- First tries to serve the exact file (`$uri`)
- Then tries as a directory (`$uri/`)
- Finally passes to WordPress's index.php with query parameters
- This enables WordPress pretty permalinks

### 2. PHP Processing

```nginx
location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;  # Split PHP file and path info
    fastcgi_pass php;                          # Send to upstream 'php'
    fastcgi_index index.php;                   # Default PHP file
    include fastcgi_params;                    # Include FastCGI parameters
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param PATH_INFO $fastcgi_path_info;
    
    # Performance tuning
    fastcgi_buffers 16 16k;        # Buffer configuration
    fastcgi_buffer_size 32k;       # Buffer size
    fastcgi_read_timeout 600;      # Timeout for PHP execution
}
```

**Explanation**: This handles all `.php` files by sending them to PHP-FPM for processing.

### 3. Static File Caching

```nginx
# CSS Files
location ~* \.(css)$ {
    expires max;                           # Cache forever
    add_header Cache-Control "public, no-transform";
    log_not_found off;                     # Don't log missing CSS files
    charset utf-8;                         # Set character encoding
    add_header Content-Type "text/css; charset=utf-8";
}

# JavaScript Files
location ~* \.(js)$ {
    expires max;
    add_header Cache-Control "public, no-transform";
    log_not_found off;
}

# Image Files
location ~* \.(jpg|jpeg|png|gif|ico|webp)$ {
    expires max;
    add_header Cache-Control "public, no-transform";
    log_not_found off;
}

# Font Files
location ~* \.(ttf|woff|woff2)$ {
    expires max;
    add_header Cache-Control "public, no-transform";
    log_not_found off;
}
```

**Explanation**: These blocks optimize static file delivery with aggressive caching.

## Security Headers

```nginx
add_header X-Content-Type-Options "nosniff" always;     # Prevent MIME sniffing
add_header X-Frame-Options "SAMEORIGIN" always;         # Prevent clickjacking
add_header X-XSS-Protection "1; mode=block" always;     # XSS protection
```

## Content Security Policy (CSP) Explained

### What is CSP?

Content Security Policy is a security feature that helps prevent:

- Cross-Site Scripting (XSS) attacks
- Code injection attacks
- Data injection attacks

It works by defining which sources of content are allowed to load.

### Your Commented CSP Header

```nginx
# Currently commented out:
add_header Content-Security-Policy "default-src 'self' data: https: https://secure.gravatar.com *.gravatar.com; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https://secure.gravatar.com *.gravatar.com; font-src 'self' data:; connect-src 'self'; frame-ancestors 'self';" always;
```

### Should You Enable CSP for Local Development?

**Short Answer**: No, not recommended for local development.

**Detailed Explanation**:

**Cons for Local Development**:

1. **Breaks functionality**: Many WordPress plugins and themes use inline scripts/styles
2. **Development friction**: Constantly need to update CSP for new plugins/themes
3. **Debugging complexity**: CSP violations can be hard to trace
4. **Third-party integrations**: Many development tools inject inline scripts

**When to Use CSP**:

- **Staging environment**: Test CSP before production
- **Production environment**: Essential for security
- **Known stable setup**: When your plugin/theme stack is finalized

**Recommendation for Your Setup**:
Keep the CSP header commented out for now. When you're ready for production, gradually enable and test it:

```nginx
# Start with report-only mode to see violations without blocking
add_header Content-Security-Policy-Report-Only "default-src 'self'; report-uri /csp-report" always;
```

## WordPress-Specific Optimizations in Your Config

1. **Large Upload Support**: `client_max_body_size 100M` - Handles large media uploads
2. **PHP Timeout**: `fastcgi_read_timeout 600` - Allows long-running WordPress operations
3. **Buffer Optimization**: FastCGI buffers tuned for WordPress
4. **Static File Caching**: Aggressive caching for theme assets
5. **Security Headers**: Basic protection without breaking functionality

Your current configuration is well-optimized for WordPress development!
