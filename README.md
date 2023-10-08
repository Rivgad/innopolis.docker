# Docker get started guide

Прохождение 8-ми частей практического урока https://docs.docker.com/get-started/

### Part 3 - Share the application

Loaded image to https://hub.docker.com/repository/docker/rivgar/getting-started/general

### Part 4 - Persist the DB

docker volume create todo-db


docker volume inspect todo-db

```
[
    {
        "CreatedAt": "2023-10-08T21:54:57Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": null,
        "Scope": "local"
    }
]
```

### Part 5 - Use bind mounts

```
docker logs -f 49dd321a75de98bf9b342667ce9ee2dc03d404480e2715d14624db9c9da75dca

yarn install v1.22.19
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
Done in 41.83s.
yarn run v1.22.19
$ nodemon src/index.js
[nodemon] 2.0.20
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node src/index.js`
Using sqlite database at /etc/todos/todo.db
Listening on port 3000
```

### Part 6 - Multi container apps

```
> docker network create todo-app

> docker run -d `
    --network todo-app --network-alias mysql `
    -v todo-mysql-data:/var/lib/mysql `
    -e MYSQL_ROOT_PASSWORD=secret `
    -e MYSQL_DATABASE=todos `
    mysql:8.0

> docker exec -it 1f531134c2191b43c87ca84ce0536f5301a2c7cb97186d2f4074b06cce263668 mysql -u root -p

Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.34 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.


mysql> SHOW DATABASES;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| todos              |
+--------------------+
5 rows in set (0.00 sec)


> docker run -it --network todo-app nicolaka/netshoot

dig mysql

; <<>> DiG 9.18.13 <<>> mysql
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 28451
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;mysql.                         IN      A

;; ANSWER SECTION:
mysql.                  600     IN      A       172.19.0.2

;; Query time: 0 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Sun Oct 08 22:16:50 UTC 2023
;; MSG SIZE  rcvd: 44


> docker run -dp 127.0.0.1:3000:3000 `
>>   -w /app -v "$(pwd):/app" `
>>   --network todo-app `
>>   -e MYSQL_HOST=mysql `
>>   -e MYSQL_USER=root `
>>   -e MYSQL_PASSWORD=secret `
>>   -e MYSQL_DB=todos `
>>   node:18-alpine `
>>   sh -c "yarn install && yarn run dev"


> docker logs -f 342c30173f68409d3db594a685af78483f00d0e9d6f6741738a10d4a8aa07db4

yarn install v1.22.19
[1/4] Resolving packages...
success Already up-to-date.
Done in 0.40s.
yarn run v1.22.19
$ nodemon src/index.js
[nodemon] 2.0.20
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node src/index.js`
Waiting for mysql:3306.
Connected!
Connected to mysql db at host mysql
Listening on port 3000


mysql> select * from todo_items;
+--------------------------------------+------------+-----------+
| id                                   | name       | completed |
+--------------------------------------+------------+-----------+
| 3d2650ce-db0e-440a-a628-11d9c509acf2 | New task   |         0 |
| ee4cf0db-a136-44f4-b58f-e195d39236cb | New task 2 |         0 |
+--------------------------------------+------------+-----------+
2 rows in set (0.00 sec)

```