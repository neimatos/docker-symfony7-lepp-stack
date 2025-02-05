# Docker configuration for LEPP stack + Symfony 7.0+ for development.

- NGINX 1.26+
- PostgreSQL 17.2+
- PHP 8.4+
- Doctrine
- Maker Bundle
- PHPUnit and Xdebug.

## Prerequisites

Required requisites:

1. [Git](https://git-scm.com/book/en/Getting-Started-Installing-Git)
2. [Docker](https://docs.docker.com/engine/installation/)
3. [Docker Compose](https://docs.docker.com/compose/install/)
4. [Composer](https://getcomposer.org)

Docker and Docker Compose can be installed with [Docker Desktop](https://www.docker.com/products/docker-desktop/) app.

## Enviroment Initialization

1. Clone the project:

```
git clone https://github.com/neimatos/docker-symfony7-lepp-stack.git
```

2. Go to the project's folder

```
cd docker-symfony7-lepp-stack
```

3. Update and install Composer packages

```
composer update
```

4. Build and up project with Docker Compose

```
docker-compose up -d --build
```

5. Open `http://localhost` in your browser, you should see the Symfony 7's welcome page.

![image](https://github.com/user-attachments/assets/d6158b9b-081e-4c64-8e75-a601b535c3a8)

## Using

### Using Docker Compose

Build and up:

```
docker-compose up -d --build
```

Up only:

```
docker-compose up -d
```

Down:

```
docker-compose down
```

Rebuild and up:

```
docker-compose down -v --remove-orphans
docker-compose rm -vsf
docker-compose up -d --build
```

### Using PostgreSQL

Init .sql file inside `docker/postgres/conf` will be executed on container build. Currently, there is `docker/postgres/conf/init_test_table.sql` file, creating `test_table` with 3 records for testing purposes. It can be safely deleted.

Postgres database name, user and password defined in `.env` file.

Connect to PostgreSQL psql terminal:

```
docker-compose exec postgres psql -U postgres
```

### Using PHP

PHP config located at `docker/php/conf/php.ini`. You might want to change `date.timezone` value in `php.ini`. Default value is `America/Fortaleza` (GMT-3).

Execute command `php -v` in the `php` container:

```
docker-compose exec php php -v
```

*There is OPCache commented out settings in `php.ini` as well as loading line in `Dockerfile` if you need it, by any means. Not recommended for development environment.*

### Using PHPUnit

There is two test for testing Docker environment:

1. PHPUnit self test
2. PostgreSQL connection test

Run the tests:

```
docker-compose exec php vendor/bin/phpunit ./tests
```

Successfully passed tests output:

```
OK (2 tests, 2 assertions)
```

### Using Xdebug

XDebug config located at `docker/php/conf/xdebug.ini`.

To enable Xdebug at the project build, set `INSTALL_XDEBUG` variable to `true` in `.env` file.

## Mapping

Folders mapped for default Symfony folder structure (assuming local `/` is project's folder):

| Local | Container | Description |
| - | - | - |
| / | /var/www | Project root |
| /public | /var/www/public | Web server document root |
| /logs/nginx | /var/logs/nginx | NGINX logs |

Ports mapped default:

| Local | Container | Description |
| - | - | - |
| 80 | 80 | NGINX port |
| 5432 | 5432 | PostgreSQL port |
