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