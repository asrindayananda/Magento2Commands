# Magento 2 / Adobe Commerce Commands

Useful commands used in Magento 2 development

## Code compilation
```
nano /bin/deployMagentoCode
chmod +x /bin/deployMagentoCode
deployMagentoCode
rm -rf generated/code/* && 
rm -rf pub/static/frontend/* && 
rm -rf pub/static/adminhtml/* && 
docker-compose run --rm deploy magento-command setup:upgrade && 
docker-compose run --rm deploy magento-command setup:di:compile && 
docker-compose run --rm deploy cloud-deploy && 
docker-compose run --rm deploy magento-command cache:clean && 
docker-compose run --rm deploy magento-command cache:flush
```

```
nano /bin/restartDockersDeploy
chmod +x /bin/restartDockersDeploy
restartDockersDeploy
docker-compose down && 
docker-compose up -d && 
docker-compose run --rm deploy cloud-deploy && 
docker-compose run --rm deploy magento-command cache:clean && 
docker-compose run --rm deploy magento-command cache:flush
```

```
rm -rf generated/metadata/*
rm -rf pub/static/_cache/*
rm -rf var/cache/*
rm -rf var/page_cache/*
rm -rf var/view_preprocessed/*
php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento setup:static-content:deploy en_US en_GB -f -j 2
php bin/magento cache:clean
php bin/magento cache:flush
```

### Enable Crons
```
./vendor/bin/ece-tools cron:enable 
docker-compose run --rm deploy magento-command cron:install
docker-compose run --rm deploy magento-command cron:run
```

### SSH into Docker containers
```
docker exec -it <container name> /bin/bash
```

### Script that creates new repo instance of magento
- Shut down any other magentos
- Create folder by hand
cd <FOLDER_NAME>
```
eval `ssh-agent -s` && 
ssh-add ~/.ssh/github-key && 
git clone git@github.com:asrindayananda/Magento-community-edition-2.4.3-docker.git . && 
composer install && 
docker volume create --name=magento-sync && 
docker volume create --name=mymagento-magento-sync && 
./vendor/bin/ece-docker build:compose --mode="developer" --with-cron --expose-db-port=3306 --no-tls --host=magento2.docker && 
docker-compose up -d && 
docker-compose run --rm deploy php ./vendor/bin/ece-patches apply && 
docker-compose run --rm deploy cloud-deploy && 
docker-compose run --rm deploy magento-command deploy:mode:set developer && 
docker-compose run --rm deploy magento-command config:set web/secure/use_in_adminhtml 0 && 
docker-compose run --rm deploy magento-command config:set web/secure/use_in_frontend 0 && 
docker-compose run --rm deploy magento-command config:set web/unsecure/base_url http://magento2.docker/ && 
docker-compose run --rm deploy magento-command config:set web/secure/base_url http://magento2.docker/ && 
docker-compose run --rm deploy cloud-deploy && 
docker-compose run --rm deploy magento-command deploy:mode:set developer && 
docker-compose run --rm deploy cloud-deploy && 
docker-compose run --rm deploy cloud-deploy && 
docker-compose run --rm deploy magento-command admin:user:create --admin-user=Admin --admin-password=password123! --admin-email=azcodez@gmail.com --admin-firstname=Az --admin-lastname=Codez
```
  
### Create Admin user
```
docker-compose run --rm deploy magento-command admin:user:create --admin-user=Admin --admin-password=password123! --admin-email=azcodez@gmail.com --admin-firstname=Az --admin-lastname=D
```
