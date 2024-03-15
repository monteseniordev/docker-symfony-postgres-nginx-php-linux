# Minimal clean Docker configuration with PHP 8.2+, NGINX 1.25+, PostgreSQL 16.0 + and Symfony 7.0 on ubuntu22.04 in VM of Windows10 for development.
# Funny

- Based on Alpine Linux.
- Xdebug and PHPUnit.
- Doctrine
- Makefile
- Symfony
- VMware 17.*
- Linux(Ubuntu22.04)

## Prerequisites

Required requisites:

1. [Git](https://git-scm.com/book/en/Getting-Started-Installing-Git)

```
sudo apt install git-all
```
# or if you use other linux such as CentOS

```
sudo dnf install git-all
```
2. [Docker](https://docs.docker.com/engine/installation/)
   [Docker Compose](https://docs.docker.com/compose/install/)

# Docker and Docker Compose can be installed with
# [Docker Desktop](https://www.docker.com/products/docker-desktop/) app.
# For non-Gnome Desktop environments, gnome-terminal must be installed:
```
sudo apt install gnome-terminal
```

# Add Docker's official GPG key:
```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

# Add the repository to Apt sources:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
# Download latest DEB package.

# Install the package with apt as follows:
```
 sudo apt-get update
 sudo apt-get install ./docker-desktop-<version>-<arch>.deb
```
# ex: 
```
sudo apt-get install ./docker-desktop-4.28.0-amd64.deb
```

# Docker desktop sign up or sign in
# in case of Sign up, run this command on terminal.
```
gpg --generate-key
```

# get public key and run following cmd.
```
pass init <your_generated_gpg-id_public_key>
```
3. [Composer](https://getcomposer.org)
```
sudo apt-get update
```

# Install curl and PHP
```
sudo apt-get install curl
sudo apt-get install php php-curl
curl -sS https://getcomposer.org/installer -o composer-setup.php
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

### for update in the future
```
sudo composer self-update
```

# check composer
```
composer -v
``` 

## Initialization

1. Clone the project:

```
git clone https://github.com/monteseniordev/docker-symfony-postgres-nginx-php-linux.git
```

2. Go to the project's folder

```
cd /path/to/docker-symfony-postgres-nginx-php-linux
```

3. Update and install Composer packages

```
composer update
```

4. Build and up project with Docker Compose
```
docker compose up -d --build
```
# if you named composer file with docker-composer.yaml
```
docker-compose up -d --build
```

5. Open `http://localhost` in your browser, you should see the Symfony's welcome page.
# if you get Apache2 page in browser, check port 80 using for Apache or Ngnix.
```
sudo lsof -i -P -n | grep LISTEN
```

# if using for Apache2
kill process in xampp or others

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

All .sql files inside `docker/postgres/conf` will be executed on container build. Currently, there is `docker/postgres/conf/create_test_table.sql` file, creating `test_table` with 3 records for testing purposes. It can be safely deleted.

Postgres database name, user and password defined in `.env` file.

Connect to database with default login:

```
docker-compose exec postgres psql -U dbuser dbname
```

### Using PHP

# PHP config located at `docker/php/conf/php.ini`. You might want to change `date.timezone` value in `php.ini`. Default value is `Europe/Helsinki` (GMT+3).

# Execute command `php -v` in the `php` container:

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

### Using Makefile

To execute Makefile command use `make <command>` from project's folder

List of commands:

| Command | Description |
| ----------- | ----------- |
| up | Up containers |
| down | Down containers |
| build | Build/rebuild continers |
| test | Run PHPUnit tests |
| bash | Use bash in `php` container as `www-data` |
| bash_root | Use bash in `php` container as `root` |

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

## Contact me

You always welcome to mail me at monteseniordev@gmail.com
