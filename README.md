## Intro

This docker contains pre-packaged Docker Images that provides you a wonderful development environment without requiring you to install PHP, NGINX, MySQL, REDIS, and any other software on your local machine.


**Usage Overview:** Run `NGINX`, `MySQL` and `Redis`.

```shell
docker-compose up  nginx mysql redis
```
### Features

- Easy switch between PHP versions: 7.0 - 5.6 - 5.5 ...
- Choose your favorite database engine: MySQL - Postgres - Redis ...
- Run your own combination of software's: Memcached - MariaDB ...
- Every software runs on a separate container: PHP-FPM - NGINX ...
- Easy to customize any container, with simple edit to the `dockerfile`.
- All Images extends from an official base Image. (Trusted base Images).
- Pre-configured Nginx for Laravel.
- Data container, to keep Data safe and accessible.
- Easy to apply configurations inside containers.
- Clean and well structured Dockerfiles (`dockerfile`).
- Latest version of the Docker Compose file (`docker-compose`).
- Everything is visible and editable.

### Supported Containers

- PHP-FPM (7.0 - 5.6 - 5.5)
- NGINX
- MySQL
- PostgreSQL
- MariaDB
- Neo4j
- MongoDB
- Redis
- Memcached
- Beanstalkd
- Beanstalkd Console
- Workspace (contains: Composer, PHP7-CLI, Laravel Installer, Git, Node, Gulp, Bower, SQLite,  Vim, Nano and cURL)
- Data *(Databases Data Container)*
- Application *(Application Code Container)*

## Requirements

| Linux                                                                                   | Windows & MAC                                           |
|-----------------------------------------------------------------------------------------|---------------------------------------------------------|
|                 [Laravel](https://laravel.com/docs/master/installation)                 | [Laravel](https://laravel.com/docs/master/installation) |
|                           [Git](https://git-scm.com/downloads)                          |           [Git](https://git-scm.com/downloads)          |
| [Docker Engine](https://docs.docker.com/engine/installation/linux/ubuntulinux) |     [Docker Toolbox](https://www.docker.com/toolbox)    |
|                [Docker Compose](https://docs.docker.com/compose/install)                |                                                         |

## Installation

1 - Clone the `Docker` repository.

**A)** If you already have a Laravel project, clone this repository on your `Laravel` root direcotry:

```bash
git submodule add https://github.com/LaraDock/laradock.git
```
>If you are not already using Git for your Laravel project, you can use `git clone` instead of `git submodule`.

**B)** If you don't have a Laravel project, and you want to install Laravel from Docker, clone this repo anywhere on your machine:

```bash
git clone https://github.com/LaraDock/laradock.git
```

## Usage


1 - For **Windows & MAC** users only: If you are not using the native Docker-Engine `Beta`, make sure you have a running Docker Virtual Host on your machine. 
[How to run a Docker Virtual Host?](#Run-Docker-Virtual-Host)
(**Linux** users don't need a Virtual Host, so skip this step).

**Example:** Running NGINX and MySQL:

```bash
docker-compose up -d  nginx mysql
```

You can select your own combination of container form this list:

`nginx`, `mysql`, `redis`, `postgres`, `mariadb`, `neo4j`, `mongo`, `memcached`, `beanstalkd`, `beanstalkd-console`, `workspace`, `data`, `php-fpm`, `application`.


**Note**: `workspace`, `data`, `php-fpm` and `application` will run automatically in most of the cases.


<br>
3 - Enter the Workspace container, to execute commands like (Artisan, Composer, PHPUnit, Gulp, ...).

```bash
docker exec -it {Workspace-Container-Name} bash
```
Replace `{Workspace-Container-Name}` with your Workspace container name. 
<br>
To find the containers names type `docker-compose ps`.



<br>
4 - Edit the Laravel configurations.

If you don't have a Laravel project installed yet, see [How to Install Laravel in a Docker Container](#Install-Laravel).

Open your Laravel's `.env` file and set the `DB_HOST` to your `{Docker-IP}`:

```env
DB_HOST=xxx.xxx.xxx.xxx
```
[How to find my Docker IP Address?](#Find-Docker-IP-Address)




<br>
5 - Open your browser and visit your `{Docker-IP}` address (`http://xxx.xxx.xxx.xxx`).


### List current running Containers
```bash
docker ps
```
You can also use the this command if you want to see only this project containers:

```bash
docker-compose ps
```


<br>
### Close all running Containers
```bash
docker-compose stop
```

To stop single container do:

```bash
docker-compose stop {container-name}
```

<br>
### Delete all existing Containers
```bash
docker-compose down
```

*Note: Careful with this command as it will delete your Data Volume Container as well. (if you want to keep your Database data than you should stop each container by itself as follow):* 


<br>
### Enter a Container (SSH into a running Container)

1 - first list the current running containers with `docker ps`

2 - enter any container using:

```bash
docker exec -it {container-name} bash
```
3 - to exit a container, type `exit`.







<br>
### Edit default container configuration
Open the `docker-compose.yml` and change anything you want.

Examples: 

Change MySQL Database Name:

```yml
  environment:
    MYSQL_DATABASE: laradock
```

Change Redis defaut port to 1111:

```yml
  ports:
    - "1111:6379"
```


<br>
### Edit a Docker Image

1 - Find the `dockerfile` of the image you want to edit, 
<br>
example for `mysql` it will be `mysql/Dockerfile`.

2 - Edit the file the way you want.

3 - Re-build the container:

```bash
docker-compose build mysql
```

*If you find any bug or you have and suggestion that can improve the performance of any image, please consider contributing. Thanks in advance.*



<br>
### Build/Re-build Containers

If you do any change to any `dockerfile` make sure you run this command, for the changes to take effect:

```bash
docker-compose build
```
Optionally you can specify which container to rebuild (instead of rebuilding all the containers):

```bash
docker-compose build {container-name}
```

<br>
### Add more Software's (Docker Images)

To add an image (software), just edit the `docker-compose.yml` and add your container details, to do so you need to be familiar with the [docker compose file syntax](https://docs.docker.com/compose/compose-file/).



<br>
### View the Log files 
The Nginx Log file is stored in the `logs/nginx` directory.

However to view the logs of all the other containers (MySQL, PHP-FPM,...) you can run this:

```bash
docker logs {container-name}
```


### Install Laravel from a Docker Container

1 - First you need to enter the Workspace Container.

2 - Install Laravel.

Example using Composer

```bash
composer create-project laravel/laravel my-cool-app "5.2.*"
```

> We recommand using `composer create-project` instead of the Laravel installer, to install Laravel.

For more about the Laravel installation click [here](https://laravel.com/docs/master#installing-laravel).


3 - Edit `docker-compose.yml` to Map the new application path:

By default LaraDock assumes the Laravel application is living in the parent directory of the laradock folder. 

Since the new Laravel application is in the `my-cool-app` folder, we need to replace `../:/var/www/laravel` with `../my-cool-app/:/var/www/laravel`, as follow:

```yaml
    application:
        build: ./application
        volumes:
            - ../my-cool-app/:/var/www/laravel
```
4 - Go to that folder and start working..

```bash
cd my-cool-app
```



<br>
### Run Artisan Commands

You can run artisan commands and many other Terminal commands from the Workspace container.

1 - Make sure you have the workspace container running.

```bash
docker-compose up -d workspace // ..and all your other containers
```

2 - Find the Workspace container name:

```bash
docker-compose ps
```

3 - Enter the Workspace container:

```bash
docker exec -it {workspace-container-name} bash
```

4 - Run anything you want :)

```bash
php artisan
```
```bash
Composer update
```
```bash
phpunit
```

<br>
### Use Redis

1 - First make sure you run the Redis Container (`redis`) with the `docker-compose up` command.

```bash
docker-compose up -d redis
```

2 - Open your Laravel's `.env` file and set the `REDIS_HOST` to your `Docker-IP` instead of the default `127.0.0.1` IP.

```env
REDIS_HOST=xxx.xxx.xxx.xxx
```

If you don't find the `REDIS_HOST` variable in your `.env` file. Go to the database config file `config/database.php` and replace the default `127.0.0.1` IP with your `Docker-IP` for Redis like this:

```php
'redis' => [
    'cluster' => false,
    'default' => [
        'host'     => 'xxx.xxx.xxx.xxx',
        'port'     => 6379,
        'database' => 0,
    ],
],
```

3 - To enable Redis Caching and/or for Sessions Management. Also from the `.env` file set `CACHE_DRIVER` and `SESSION_DRIVER` to `redis` instead of the default `file`.

```env
CACHE_DRIVER=redis
SESSION_DRIVER=redis
```

4 - Finally make sure you have the `predis/predis` package `(~1.0)` installed via Composer:

```bash
composer require predis/predis:^1.0
```

5 - You can manually test it from Laravel with this code:

```php
\Cache::store('redis')->put('LaraDock', 'Awesome', 10);
```





<br>
### Use Mongo

1 - First make sure you run the MongoDB Container (`mongo`) with the `docker-compose up` command.

```bash
docker-compose up -d mongo
```


2 - Add the MongoDB configurations to the `config/database.php` config file:

```php
'connections' => [

    'mongodb' => [
        'driver'   => 'mongodb',
        'host'     => env('DB_HOST', 'localhost'),
        'port'     => env('DB_PORT', 27017),
        'database' => env('DB_DATABASE', 'database'),
        'username' => '',
        'password' => '',
        'options'  => [
            'database' => '',
        ]
    ],

	// ...

],
```

3 - Open your Laravel's `.env` file and update the following variables:

- set the `DB_HOST` to your `Docker-IP`.
- set the `DB_PORT` to `27017`.
- set the `DB_DATABASE` to `database`.


4 - Finally make sure you have the `jenssegers/mongodb` package installed via Composer and its Service Provider is added.

```bash
composer require jenssegers/mongodb
```
More details about this [here](https://github.com/jenssegers/laravel-mongodb#installation).

5 - Test it:

- First let your Models extend from the Mongo Eloquent Model. Check the [documentation](https://github.com/jenssegers/laravel-mongodb#eloquent).
- Enter the Workspace Continer `docker exec -it laradock_workspace_1 bash`.
- Migrate the Database `php artisan migrate`.






<br>
### [PHP]



### Install PHP Extensions

Before installing PHP extensions, you have to decide whether you need for the `FPM` or `CLI` because each lives on a different container, if you need it for both you have to edit both containers.

The PHP-FPM extensions should be installed in `php-fpm/Dockerfile-XX`. *(replace XX with your default PHP version number)*.
<br>
The PHP-CLI extensions should be installed in `workspace/Dockerfile`.









<br>
### Change the PHP-FPM Version
By default **PHP-FPM 7.0** is running.

>The PHP-FPM is responsible of serving your application code, you don't have to change the PHP-CLI version if you are planing to run your application on different PHP-FPM version.

1 - Open the `docker-compose.yml`.

2 - Search for `Dockerfile-70` in the PHP container section.

3 - Change the version number. 
<br>
Example to select version 5.6 instead of 7.0 you have to replace `Dockerfile-70` with `Dockerfile-56`.

Sample: 

```txt
php-fpm:
    build:
        context: ./php-fpm
        dockerfile: Dockerfile-70
```

Supported Versions:

- For (PHP 7.0.*) use `Dockerfile-70`
- For (PHP 5.6.*) use `Dockerfile-56`
- For (PHP 5.5.*) use `Dockerfile-55`


4 - Finally rebuild the container

```bash
docker-compose build php
```


<br>
### Change the PHP-CLI Version
By default **PHP-CLI 7.0** is running.

>Note: it's not very essential to edit the PHP-CLI verion. The PHP-CLI is only used for the Artisan Commands & Composer. It doesn't serve your Application code, this is the PHP-FPM job.

The PHP-CLI is installed in the Workspace container. To change the PHP-CLI version you need to edit the `workspace/Dockerfile`.

Right now you have to manually edit the `Dockerfile` or create a new one like it's done for the PHP-FPM. (consider contributing).


### Run a Docker Virtual Host

These steps are only for **Windows & MAC** users *(Linux users don't need a virtual host)*:

1 - Run the default Host:

```bash
docker-machine start default
```

* If the host "default" does not exist, create one using the command below, else skip it:

* ```bash
  docker-machine create -d virtualbox default
  ```

2 - Run this command to configure your shell:

```bash
eval $(docker-machine env)
```


<br>
### Find your Docker IP Address 

**On Windows & MAC:** 

Run this command in your terminal:

```bash
docker-machine ip default
```
If your Host name is different then `default`, you have to specify it (`docker-machine ip my-host`).

*(The default IP is 192.168.99.100)*

<br>

> **boot2docker** users: run `boot2docker ip` *(when boot2docker is up)*.

<br>
**On Linux:** 

Run this command in your terminal:

```shell
ifconfig docker0 | grep 'inet' | cut -d: -f2 | awk '{ print $1}' | head -n1
```

*(The default IP is 172.17.0.1)*



<br>
### Use custom Domain (instead of the Docker IP)

Assuming your custom domain is `laravel.dev` and your current `Docker-IP` is `xxx.xxx.xxx.xxx`.

1 - Open your `/etc/hosts` file and map your `Docker IP` to the `laravel.dev` domain, by adding the following:

```bash
xxx.xxx.xxx.xxx    laravel.dev
```

2 - Open your Laravel's `.env` file and replace the `127.0.0.1` default values with your `{Docker-IP}`.
<br>
Example:

```env
DB_HOST=xxx.xxx.xxx.xxx
```

3 - Open your browser and visit `{http://laravel.dev}`



Optionally you can define the server name in the nginx config file, like this:

```conf
server_name laravel.dev;
```


<br>
### Debugging

*Here's a list of the common problems you might face, and the possible solutions.*

#### + I see a blank (white) page instead of the Laravel 'Welcome' page!

run this command from the Laravel root directory:

```bash
sudo chmod -R 777 storage bootstrap/cache
```

#### + I see "Welcome to nginx" instead of the Laravel App!

use `http://127.0.0.1` instead of `http://localhost` in your browser.