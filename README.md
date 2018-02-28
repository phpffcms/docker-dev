# FFCMS docker-dev environment
Docker-dev provide ffcms developer environment features via [docker](https://www.docker.com/) container. To use docker-dev box you should install docker.

## Installation
Clone this repository
```bash
git clone https://github.com/phpffcms/docker-dev.git
```
and run docker-compose:
```bash
docker-compose up -d
```
Thats done! Now you can access to your php-fpm/nginx/mysql stack at http://localhost:8000. You can manage your database with phpmyadmin at http://localhost:8080 

## Project struct
Main compose settings located in ```./docker-compose.yml```. There you can change port routing features, directory naming and software versions. 

Application directory root is: ```/www```. This directory is shared for nginx and php-fpm containers.

All mysql database files stored in ```/db/mysql``` folder. 

## Author & license
This project distribute under MIT open source license. You can use it, modify or redistribute anyway you like it. 

Author: Pyatinskiy M.M, Russia, 2018.
