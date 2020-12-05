# DevBox
Docker-compose for start your PHP project, with pre-installed phalcon framework v4.1 and All requirements.

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

### Metabase Doc

Admin Config
-------------------------------------
after success install login as Admin.

1.1 Go to Admin Page
1.2 Go Database page.

Setup/Edit Database bellow to meet your expectation.

- 1.2.1 ***DB-SLAVE-NEW-QR***, Setup For **DB MYSQL MBIZ QR** 
- 1.2.2 ***DMP-ADMIN-SLAVE***, Setup For **Alun-alun Mbizmarket**
- 1.2.3 ***DMP-SLAVE***, Setup For **DB MYSQL MBIZMarket/DMP**
- 1.2.4 ***MBIZ-SLAVE***, Setup For **DB MYSQL MBIZ QR *OLD* **
- 1.2.5 ***odoo - OpenErp Prod***, Setup For **DB odoo**

Troubleshooting
---------------------------------------
if something happen in future, just try to reload the docker-compose, or update metabase image and reload docker-compose.

Or [you can find some issue in logs](https://meta.mbiz.co.id/admin/troubleshooting/logs)

