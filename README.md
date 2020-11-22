# NGINX Latest - PHP FPM 7.2 & Phalcon 4.1
Docker for start your project PHP, with pre-installed phalcon framework v4.1 and All requirements.

### Run

`docker-compose up -d` to run, and docker will start build image.

alternative, you can pull the latest build from Hub, just run this before run `docker-compose up -d` to cut build time in local.

- `docker pull kuyaninja/nginx`
- `docker pull kuyaninja/php-phalcon`

### Edit

to edit the container, just edit the Dockerfile in docker folder to meet your needs.

### example

after run, to check installation,

- `docker-compose exec app /bin/sh` and then run 
- `/var/www/app$ phalcon i`

if no error, then open in the browser http://localhost/

### path

- `source/app` docker share to `/var/www/app`
- `source/app/public` nginx default. 
