#!/bin/bash

DOCKER_BE = symfony-api-platform-be
OS := $(shell uname)

ifeq ($(OS),Darwin)
	UID = $(shell id -u)
else ifeq ($(OS),Linux)
	UID = $(shell id -u)
else
	UID = 1000
endif

help: ##Show this help message
	@echo 'usage: make[target]'
	@echo 
	@echo 'targets:'
	@egrep '^(.+)\:\ ##\ (.+)' ${MAKEFILE_LIST}

run: ##Start the containers
	U_ID=${UID} docker-compose up -d

stop: ##Start the containers
	U_ID=${UID} docker-compose stop

restart: ##Restart the containers
	$(MAKE) stop && $(MAKE) run

build: ##Rebuilds all the containers
	U_ID=${UID} docker-compose build

prepare: ##Runs backend commands
	$(MAKE) composer-install

# Backend commands
composer-install: ##Install composer dependencies
	U_ID=${UID} docker exec --user ${UID} -it ${DOCKER_BE} composer install --no-scripts --no-interaction --optimize-autoloader

migrations: ##Runs the migrations
	U_ID=${UID} docker exec -it --user ${UID} ${DOCKER_BE} bin/console doctirne:migrations:migrate -n

be-logs: ##Tails the symfony dev log
	U_ID=${UID} docker exec -it --user ${UID} ${DOCKER_BE} tail -f /var/log/dev.log

ssh-be: ##ssh's into the be container
	U_ID=${UID} docker exec -it --user ${UID} ${DOCKER_BE} bash

code-style: ##Runs php-cs to fix code styling
	U_ID=${UID} docker exec -it --user ${UID} ${DOCKER_BE} php-cs-fixer fix src --rules=@symfony
