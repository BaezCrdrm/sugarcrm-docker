# Overview
This project is based on [sugarcrm-docker](https://github.com/intelestream/sugarcrm-docker). It was adapted to run SugarCRM 10.x.



# Installation
In order to create local development environment do next:

1. Install Docker
2. Clone this repo
3. Run `docker-compose up --build` from root folder created when the repo is cloned, and wait for images to be pulled and containers to be created.
4. Apache server should already listed on port 5002, so you can access your website via http://localhost:5002/test.php.
Changes you make in the source are immediately visible.
5. Copy sugarcrm code inside `www` folder. My suggestion is to create separate folder inside the www folder for each SugarCRM instance.
6. Open http://localhost:5002 in browser and start installation. 

For Mysql enter next credentials:
- host: mysql_crm (yes, host is mysql_crm, it is resolved by docker - check docker-compose.yml)
- db: sugarcrm
- user: sugarcrm
- pass:sugarcrm
- root user: root
- root password: sugarcrm

For Elastic search (_not working on current release_):
- host: elasticsearch_crm (yes, this is host, again check docker-compose.yml)
- port: 9200

7. Complete installation.
If you need to access mysql, you can connect to it using ip address from step 6.
At some point we can add phpmyadmin software, but it is not needed for now.

In case you need to access any containers:
docker ps - This will list you all containers with their ids

In command below replace container_id with container id you want to connect
to based on results of previous step
docker exec -i -t container_id /bin/bash


To set cronjob, add next line:
```bash
$ cd /app/www/crm; /usr/local/bin/php -f cron.php > /dev/null 2>&1
```
And start cron service:
```bash
$ service cron start
```

Note that if you shutdown and remove container, you will need to set cronjob again for other instances.

# To considerate
## PHP >= 7.2
### libzip-dev
Add to `DockerFile`.
```dockerfile
RUN apt-get install libzip-dev
```
_Note: Add before_:
```dockerfile
RUN apt-get install -y zlib1g zlib1g-dev
RUN docker-php-ext-install zip
```

### mcryipt
mbcrypt needs to be manually installed.
https://stackoverflow.com/a/53466129/5424025

## PHP >= 8.x 
https://github.com/docker-library/php/issues/931#issuecomment-568658449

### mcrypt
mcrypt won't work on newer versions. https://www.php.net/manual/es/book.mcrypt.php.

TODO: Search for a PHP 8.x installation.
