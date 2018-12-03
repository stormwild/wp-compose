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

## Notes

It seems the wp-content volume needs to be empty prior to starting the container the first time, or else it will be unable to write to the directory.



