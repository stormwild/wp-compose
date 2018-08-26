# WordPress Development Using Docker

## Pre-requisites

- Docker
- Git

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

```sh
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

## MySQL

Update Site Url in the database:

```sql
UPDATE wp_options SET option_value = 'http://example.local' WHERE option_name = 'siteurl';
```

## Bash

Access bash in the running container

```sh
docker exec -it wpdb bash
```

## Notes

- Import a different database into container
- Switch between databases by changing the database reference
- Restart containers

## WordPress Admin

admin:pass

Extract a tar.gz file

```sh
tar -zxvf yourfile.tar.gz
```

Extracts the file to the current directory.

You can specify a different directory to extract to using -C parameter and a path to the directory as follows:

```sh
tar -C /folder -zxvf yourfile.tar.gz
```
