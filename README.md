# The Docker Nginx(Pagespeed)-MySQL-PHP-Redis-Elastic Setup
* Change settings under `.env` in root folder  
* Change PHP Versions 7.0, 7.1, 7.2, 7.3 all based on php:alpine docker image

## Description
This Setup installs the basic docker containers for Magento 2. 

## Get Source
#### Use Git

    git clone git@github.com:aliuosio/mage2.docker.git

## Start docker
    
    # Linux
    docker-compose build;
    docker-compose up -d;
    
    # OSX (if docker-sync is missing install using: gem install docker-sync)
    docker-sync start;
    docker-compose -f docker-compose.osx.yml build;
    docker-compose -f docker-compose.osx.yml up -d;
     

## Magento 2 Configuration
    
    # Install Magento 2
     docker exec -it -u <USERNAME> <NAMESPACE>_php chmod +x ./install.sh
     docker exec -it -u <USERNAME> <NAMESPACE>_php ./install.sh
     
> \<USERNAME\> \<NAMESPACE\> set in .env
    
Call: `https://mage2.localhost` in your browser.  
> Overwrite env.php in app/etc **after Magento 2 configuration** to use Redis for sessions and caching and MySQL over Sockets


## PHP Container Usage
    
    docker exec -it -u <USERNAME> <NAMESPACE>_php sh
    
## SSL Certificate Registration
    
    # register certificate
    docker-compose run --rm letsencrypt \
        letsencrypt certonly --webroot \
        --email <your_email-address> --agree-tos \
        -w /var/www/letsencrypt -d <subdomian or domain only: my.example.com>
        
    # restart webserver
    docker-compose kill -s SIGHUP nginx  
    
>**Renewal** (Quote: https://devsidestory.com/lets-encrypt-with-docker/)  
Let’s Encrypt certificates are valid for 3 months,  
they’d have to be renewed periodically with the following command:  
    
    # renew certificates which are expiring in less than 30 days,
    docker-compose run --rm letsencrypt letsencrypt renew 
    
    # restart webserver
    docker-compose kill -s SIGHUP nginx

#### PHP Container Usage
    
    docker exec -it -u <USERNAME> <NAMESPACE>_php sh
    
#### Composer Usage
    
    docker exec -it -u <USERNAME> <NAMESPACE>_php composer <command>

#### Magerun2 Usage
    
    docker exec -it -u <USERNAME> <NAMESPACE>_php n98-magerun2 shell
    
#### Mailhog Usage
    
    http://<your_domain>:8025

#### Elasticsearch Usage:
In Magento 2 Backend `stores` -> `Configuration` -> `Catalog` -> `Catalog` -> `Tab: Catalog Search`
    
    Search Engine: Elasticsearch 5.0+
    Elasticsearch Server Hostname: elasticsearch
    Elasticsearch Server Port: 9200
> You **MUST** set `sysctl -w vm.max_map_count=262144` on the docker host system or the elasticsearch container goes down
> On OSX see link: https://stackoverflow.com/questions/41192680/update-max-map-count-for-elasticsearch-docker-container-mac-host?rq=1

#### Todos
* handle magento 2 cronjobs per docker container or add job to php container
* fix sockets for redis with magento 2
* ~~nginx with pagespeed module~~
* ~~create seperat containers for redis session and cache~~
* ~~create seperat containers for cronjob and image optimization~~
* ~~fix file permissions and ownership between containers and docker host~~
* ~~move Magento 2 specific tools and config to post-build.sh called in docker-compose.yml~~
* ~~move xdebug install & config to magento-install.sh band install after magento 2 install and sampledata~~
* test with mounts instead of volumes
* ~~setup script for PHP Container to set IP for xdebug or Domain~~
* clean up alpine packages after build
* PWA Studio as variable option
* set authentification for elasticsearch
* ~~add composer package [magenerds/smtp](https://github.com/magenerds/smtp)~~
* reduce the number of volumes
* optimize pagespeed caching
* ~~use pagespeed with redis cache~~
* increase vm max count for elasticsearch without system reboot
* Nginx Header Config passes at https://securityheaders.com/
* set timezone in containers
* secure socket connection between containers
* add varnish container and configure with magento 2

#### Bugs
* ~~fix OSX version~~
* ~~check that all commands function in post-build.sh~~
* ~~sampledata deploy error on docker-compose build~~

#### Contribute
Please Contribute by creating a fork of this repository.  
Follow the instructions here: https://help.github.com/articles/fork-a-repo/

#### License
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)



## Features
* alternative **OSX docker-compose** file using docker-sync **for better perfomance**
* set project directory to where ever you want (as configurable option)
* set PHP-FPM minor Versions under 7 (7.0, 7.1, 7.2, 7.3) as configurable option  
(24.02.2019: Magento 2.3 at this point does not work with PHP 7.3)
* setup valid **SSL certificates** with letsmcrypt container
* Nginx uses **Pagespeed** Module
* both **PHP GD and PHP Imagick** are installed
* nodejs any yarn for [mage2tv/magento-cache-clean](https://github.com/mage2tv/magento-cache-clean) 
* **PHP Xdebug** as configurable option
* **PHP Opcache** enabled
* **PHP redis** enabled
* [magenerds/smtp](https://github.com/magenerds/smtp) install
* ~~Alpine **Image Libraries** in PHP Docker Container: jpegoptim, optipng, pngquant, gifsicle~~
* **install magento 2** as configurable option
* **install magento 2 sample data** as configurable option
* permissions are set after magento 2 install  
following [Magento 2 Install Guide](https://devdocs.magento.com/guides/v2.3/config-guide/prod/prod_file-sys-perms.html)  as configurable option
* **http basic authentication** 
* **use mysql, php over sockets** instead of ports for faster data container exchange
* **Extra Composer Packages**
    * [hirak/prestissimo](https://github.com/hirak/prestissimo) composer Package
* **Extra Composer Packages with Magento 2 Installer **
    * [msp/devtools](https://github.com/magespecialist/m2-MSP_DevTools) DevTools for Magento2  
    * [mage2tv/magento-cache-clean](https://github.com/mage2tv/magento-cache-clean) 
    
> features can be enabled in .env

## Docker Container Overview
* ~~Magento Cronjobs~~
* Elasticsearch
* letsencrypt
* mailhog
* nginx
* mysql
* php
* redis

