# Postgresql DB
## Run postgresql DB container
Define the following environment variable in .env.
* PG_IMAGE: Docker image to use.
* PG_HOST_PORT: Port for postgres is forwarded to this port in host pc.
* PG_NAME: Database name to use.
* PG_USER: User name.
* PG_PASSWORD: Password.
* PG_MOUNTED_VOLUME: Directory mounted to store data of pg container parmanetly.

Here's an example of .env file. Note that .env file can be prepared mode-wise, such as .env.production, .env.development, and .env.testing, etc., also can be stored in .env.d dir.
```
(.env.d/pg.env.development)
PG_IMAGE='postgres:12.4'
PG_HOST_PORT=5432
PG_DB_NAME=pg_db_dev
PG_USER=pg_user_dev
PG_PASSWORD=pgsecrepassword
PG_MOUNTED_VOLUME=./data/pg
```
You can run and stop pg docker container as below.
```
(Launch postgres service)
$ docker-compose --env-file <path-to-envfile> up -d <service-name>
(Example)
$ docker-compose --env-file ./.env.d/pg.env.development up -d postgres
(Stop postgres service)
$ docker stop <container name>
(Example)
$ docker stop pg_server
```

You may like to avoid storing password even in .env file.<br>
In this case, execute docker-compose as follow.
```
$ PG_PASSWORD=<password> docker-compose --env-file <path-to-envfile> up -d <service-name>
```

## Login by postgresql client
Install postgresql client.
```
$ sudo apt install postgresql-client
```
Login.
```
$ psql -U <user> -h <host> -p <port> -d <db-name>
(Example)
$ psql -U user_dev -h 127.0.0.1 -p 5432 -d postgres_dev
```

## Trouble shooting
### Password authentication failed
When you try to login by psql, you may encounter authentication error.<br>
Try again as super user.
```
$ sudo su -
# psql -U <user> -h <host> -p <port> -d <db-name>
```

### Environment variable not set
Make sure that you're specify .env file correctly when you execute docker-compose up.<br>
Also, do not use "env_file" parameter in docker-compose.yml since it's not working as you expect.
