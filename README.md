# laravel-docker

Simple Docker setup with Laravel, MySQL, Nginx, Elasticsearch, PHP7.1, Redis

### Steps for Config

1. Copy ```.env.example``` to ```.env```
2. Copy ```docker-compose.yml.example``` to ```docker-compose.yml```
3. Copy ```php-worker/supervisord.conf.example``` to ```php-worker/supervisord.conf```
4. Copy ```workspace/crontab.example``` to ```workspace/crontab.example```
5. Edit these files as required

### Commands to run

1. Start: ```docker-compose up -d```
2. Stop: ```docker-compose stop```
3. Rebuild: ```docker-compose up -d --build``` this rebuild all containers. For specific container build use ```docker-compose up -d --build nginx``` etc
4. Remove: ```docker-compose rm``` this removes all



**All issues and PRs are hugely appreciated** :) 
