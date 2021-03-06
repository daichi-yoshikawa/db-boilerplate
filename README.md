# Postgresql DB
## Run postgresql DB container
Define the following environment variable in .env file.
* POSTGRES_CONTAINER_NAME: Name of container where postgres will run.
* POSTGRES_DB: Database name
* POSTGRES_HOST_PORT: Port for postgres is forwarded to this port in host.
* POSTGRES_USER: User name.
* POSTGRES_PASSWORD: Password.
* POSTGRES_MOUNTED_VOLUME: Directory mounted to store data of pg container parmanetly.

Here's an example of .env file. Note that .env file can be prepared mode-wise, such as .env.production, .env.development, and .env.testing, etc., also can be stored in .env.d dir.
```
(.env.d/.env.development)
POSTGRES_CONTAINER_NAME=postgres_server
POSTGRES_DB=postgres_dev
POSTGRES_HOST_PORT=5432
POSTGRES_USER=user_dev
POSTGRES_PASSWORD=secret_dev
POSTGRES_MOUNTED_VOLUME=./data/postgres
```

You can run postgres docker container as below.
```
$ docker-compose --env-file <path-to-envfile> up -d <service-name>
(Example)
$ docker-compose --env-file ./.env.d/pg.env.development up -d postgres
```

You may like to avoid storing password even in .env file.<br>
In this case, execute docker-compose as follow.
```
$ POSTGRES_PASSWORD=<password> docker-compose --env-file <path-to-envfile> up -d <service-name>
```

## Stop container
```
$ docker stop <container name>
(Example)
$ docker stop postgres_server
```

## Login postgres from host
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

## Trouble shooting(Postgres)
### Repeatedly container is restarted
Perhaps caused by failure of mounting volume. If possible, clear the mounted volume and redo docker-compose.

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

### Would like to start over but "volume is in use"
Do not blindly execute the follows...
```
$ docker-compose stop
$ docker-compose down
$ docker container prune
$ docker volume prune
$ sudo rm -r <path-to-mounted-dir>
```

# Redis
## Run redis container
Define the following environment variable in .env file.
* REDIS_CONTAINER_NAME: Name of container where redis server will run.
* REDIS_HOST_PORT: Port for redis is forwarded to this port in host.
* REDIS_PASSWORD: Password.
* REDIS_MOUNTED_VOLUME: Directory mounted to store data of redis container parmanetly.

```
$ docker-compose --env-file <path-to-envfile> up -d redis
(Example)
$ docker-compose --env-file .env.d/dot.env.default up -d redis
```

## Stop redis container
```
$ docker stop redis_server
```

## Login redis from host
```
$ sudo apt install redis-tools
$ redis-cli -n <dbnum> -h <host> -p <port> -a <password>
(Example)
$ redis-cli -n 0 -h 127.0.0.1 -p 6379 -a secret_pass
```
Instead of using '-a' option, you can set REDISCLI_AUTH environment variable.

## Ping from Host
```
$ redis-cli -n <dbnum> -h <host> -p <port> -a <password> ping
(Example)
$ redis-cli -h 127.0.0.1 -p 6379 ping -a secret_pass ping
```

# Tips
## Stop and down by docker-compose.
Do not forget specify .env file.
```
$ docker-compose --env-file <envfile> stop/down
(Example 1)
$ docker-compose --env-file .env.d/.env.development stop
(Example 2)
$ docker-compose --env-file .env.d/.env.development down
```
