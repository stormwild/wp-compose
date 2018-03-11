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

