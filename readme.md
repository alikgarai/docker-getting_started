# [Docker Getting Started](https://docs.docker.com/get-started/)

docker build -t getting-started .

docker run -dp 3000:3000 getting-started
docker run -dp 3000:3000 --name todos getting-started

docker ps
docker ps -a

## Update the Application - stop the container, remove it and rebuild it

    docker start todos
    docker stop todos

    docker rm todos
    docker rm -f todos

    docker build -t getting-started .

## Share the Application - docker hub

    docker tag getting-started alikgarai/getting-started
    docker push alikgarai/getting-started

    docker run -dp 3000:3000 alikgarai/getting-started

## Persist the DB

    docker run -d ubuntu bash -c "shuf -i 1-10000 -n 1 -o /data.txt && tail -f /dev/null"
    docker exec <container-id> cat /data.txt
    docker run -it ubuntu ls /

    docker volume create todo-db
    docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started

## Use bind mounts

    docker run -dp 3000:3000 \
    -w /app -v C:\Users\I332308\getting-started\app:/app \
    node:12-alpine \
    sh -c "yarn install && yarn run dev"

    docker logs -f <container-id>

## Multi-container app

    docker network create todo-app

    docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:5.7

    docker exec -it b30b5e863749 mysql -u root -p

    docker run -it --network todo-app nicolaka/netshoot
    dig mysql

    docker run -dp 3000:3000 \
    -w /app -v C:\Users\I332308\getting-started\app:/app \
    --network todo-app \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=secret \
    -e MYSQL_DB=todos \
    node:12-alpine \
    sh -c "yarn install && yarn run dev"

    docker logs 4fedbb1a7a15
    docker exec -it b30b5e863749 mysql -p

## Using Docker Compose

    docker-compose up -d
    docker-compose down
    docker-compose down --volumes
