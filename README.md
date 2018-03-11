# Local WordPress Development Using Docker

## Pre-requisites

- Docker

## Usage

The `wp-content` folder will contain the `plugins` and `themes` folders from the wordpress container.

Develop your themes and plugin from within these folders.

Custom volumes can be specified.

```yml
volumes:
       - ./wp-content:/var/www/html/wp-content
       - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
```

Start services

```
cd wp
docker-compose up -d
```

Visit [http://localhost](http://localhost)

## IDE Access to WordPress

Since the wordpress code running in the container is not accessible from your local host, IDEs like PHPStorm will not be able to navigate to the wordpress code.

Downloading and extracting wordpress in the current directory, (i.e. wp) will allow the IDEs to access the wordpress code.

