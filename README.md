# Docker get started guide

Прохождение 8-ми частей практического урока https://docs.docker.com/get-started/

### Part 3

Loaded image to https://hub.docker.com/repository/docker/rivgar/getting-started/general

### Part 4

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