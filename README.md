# The Docker Nginx-MySQL-PHP-Redis-Elastic Setup
* Change settings under ```.docker/.env```
* Change PHP Versions 7.0, 7.1, 7.2 all based on php:alpine docker image in ```.docker/.env``` file

## Get Source (Use git clone or composer require)
``` git clone https://github.com/aliuosio/mage2.docker.git ```
    or
``` composer require aliuosio/mage2.docker ```

> Notes: MANDATORY
if you want to run th magento 2 installer you need to copy the file ```auth.json.template``` to ```auth.json``` and set # - your credentials there
You must set project absolute folder path ```WORKDIR``` in ```.docker/.env``` 

## start docker (!! on OSX !!)
    cd .docker;
    docker-sync start; 
    docker-compose -f docker-compose.mac.yml build;
    docker-compose up -d;

> Notes: OSX Users:
if ```docker-sync``` is missing on your OSX then 
visit the http://docker-sync.io/ website to get it

## start docker
    cd .docker;
    docker-compose build;
    docker-compose up -d;
    
> Notes  ```https:/<SHOP_URI>``` in Browser if you set the ```INSTALL_MAGENTO=true``` to configure magento 2
    Database Host Name is: mysql 
just like the docker container is named under services in the docker-compose.yml

### Login to PHP container (values set in .env)
    docker exec -it -u <USER> <NAMESPACE>_php bash
    
### Use Composer (values set in .env)
    docker exec -it -u <USER> <NAMESPACE>_php composer <command>

### Use Magerun (values set in .env)
    docker exec -it -u <USER> <NAMESPACE>_php n98-magerun2 shell
    
### All outgoing mails caught by MailHog (values set in .env)
    https://<SHOP_URI>:8025

> *** you have to configure the mageplaza smtp extension in Magento 2 Backend ```stores``` -> ```configuration```
    
    *SMTP* 
    Host: mailhog
    port: 1025

### Todos
* add let's encrypt/ssl key generator container to generate certificates for valid domains
