# Local WordPress Development Using Docker

## Pre-requisites

- Docker

## Usage

Modify the `docker-compose.yml` to specify the absolute path to your local folder that will be used to mount into the container's `wp-content` folder

```yml
volumes:
       - /Users/stormwild/dev/wp/wp-content:/var/www/html/wp-content
       - /Users/stormwild/dev/wp/uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
```

Start services

```
cd wp
docker-compose up -d
```

Visit [http://localhost:8000](http://localhost:8000)

