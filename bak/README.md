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

### PHPStorm

Since the wordpress code running in the container is not accessible from your local host, IDEs like PHPStorm, will not be able to navigate to the wordpress code.

Downloading and extracting wordpress in the current directory, (i.e. wp) will allow the IDEs to access the wordpress code.

In PHPStorm it may be possible to use include path to include a path to your local wordpress install.

### Visual Studio Code

Visual Studio Code allows for multi-root workspaces. You can add your local wordpress folder in the same workspace as your theme folder.

## MySQL 8

Check running containers

```
docker ps
```

Output

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
03feb7391f85        wordpress:latest    "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        0.0.0.0:80->80/tcp    wp
daba7a75e9ee        mysql:latest        "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        3306/tcp, 33060/tcp   wpdb
```

Just use 5.7

[WordPress can’t connect to the latest MySQL 8.x Server?](https://medium.com/@hkdb/wordpress-cant-connect-to-the-latest-mysql-8-x-server-33c4c43b05b7)

## Cleanup

```sh
docker-compose down
```

Verify

```sh
docker ps -a
```

Remove volumes

```sh
docker volume ls
docker volume rm daba7a75e9ee
```

## Access MySQL

```sh
docker exec -it wpdb bash
```

```sh
mysql -uwpadmin -ppassword
```

## Resources

- [How To Install WordPress With Docker Compose](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose)
