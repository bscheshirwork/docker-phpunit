# Docker composition for run phpunit tests

based on latest php docker image.
>note: PHPUnit is only supposed to be used with the CLI SAPI.

### How to work

- Clone to local.  
Work outside you project dir:  
for example your project dir is `phpunit` and current dir (workdir) is `projects/phpunit`

```sh
git clone bscheshirwork/docker-phpunit ../docker-phpunit
```

- Change `volumes` in `prjects/docker-phpunit/docker-compose.yml` if neccessary. For example mapped `phpunit`
```yml
    volumes:
      - ../phpunit:/var/www/html #src and tests shared to container
```

- Run `composer install`/`composer update` for your project if neccessary.
```sh
docker-compose -f ../docker-phpunit/docker-compose.yml run --rm --entrypoint composer php update -vvv 
```

- Run your tests  
all tests from directory root
```sh
docker-compose -f ../docker-phpunit/docker-compose.yml run --rm php
```
single file
```sh
docker-compose -f ../docker-phpunit/docker-compose.yml run --rm php tests/Util/ConfigurationTest.php
```

single test
```sh
docker-compose -f ../docker-phpunit/docker-compose.yml run --rm php tests/Util/ConfigurationTest.php --filter testHandlePHPConfigurationDoesOverwriteVariablesFromPutEnvWhenForced
```
same
```sh
docker-compose -f ../docker-phpunit/docker-compose.yml run --rm --entrypoint /repo/vendor/bin/phpunit php tests/Util/ConfigurationTest.php --filter testHandlePHPConfigurationDoesOverwriteVariablesFromPutEnvWhenForced
```

### update
after changes in Dockerfile or update parent image run
```sh
docker-compose -f ../docker-phpunit/docker-compose.yml build --pull --no-cache
```
