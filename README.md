### Laravel Project with structure based on Laravel Beyond CRUD

#### In local environment

Clone the project.

Run:
```shell
git clone git@github.com:vasymus/docker.git ./docker

cp ./docker/docker-compose.example.yml ./docker-compose.yml

cp ./.env.example .env

cp ./docker/mariadb/docker-entrypoint-initdb.d/createdb.sql.example ./docker/mariadb/docker-entrypoint-initdb.d/createdb.sql 
```

Set environment variables.

Amend ./docker-compose.yml file accordingly. For example, change network name in .env and in docker-compose.yml from 'mynetwork' to 'myproject'.

Set databases in './docker/mariadb/docker-entrypoint-initdb.d/createddb.sql'. Default is 'laravel'.

Run:
```shell
docker-compose up -d --build
```

Enter docker 'app' container:
```shell
docker-compose exec app bash
```

And run according commands:
```shell
composer install

php artisan storage:link

php artisan key:generate

exit
```

By this time database (default is 'laravel') should be created. To make sure, you can open adminer in browser -- DOCKER_LOCALHOST_SERVERNAME (default is 'lara.localhost') with port :8080

For example, `lara.locahost:8080`. Server `db`, Username default is 'default', Password default is 'secret' (see DOCKER_MARIADB_USER and DOCKER_MARIADB_PASSWORD).

If you do not see according databases, that you put to './docker/mariadb/docker-entrypoint-initdb.d/createddb.sql', run following script:

```shell
docker-compose exec db bash

mysql -u root -p < ./docker-entrypoint-initdb.d/createdb.sql
```
Password is 'root'.

Exit db container:
```shell
exit
```

Continue. Reenter app container
```shell
docker-compose exec app bash
```

Then run:
```shell
php artisan migrate

php artisan db:seed
```

Exit docker 'app' container:
```shell
exit
```

Run (use default 'lara.localhost' or set your local environment domain -- the one you put as 'DOCKER_LOCALHOST_SERVERNAME' .env variable):
```shell
sudo bash -c "echo '127.0.0.1 lara.localhost' >> /etc/hosts"
```

Now you can see locally in your browser application: http://lara.localhost

To stop docker run:
```shell
docker-compose down
```
