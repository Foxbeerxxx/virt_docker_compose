# Домашнее задание к занятию "`Оркестрация группой Docker контейнеров на примере Docker Compose" - `Татаринцев Алексей`


### Задание 1

1. `Docker и Docker Compose у меня уже установлены на хостовой машине на Ubuntu.Проверяю версии:`

```
alexey@dell:~$ docker compose version 
Docker Compose version v2.18.1
alexey@dell:~$ docker --version
Docker version 24.0.2, build cb74dfc
```
2. `Работа с Docker Hub, захожу и создаю публиный репозиторий custom-nginx`
3. `На хостовой машине создаю рабочую директорию`

```
mkdir custom-nginx && cd custom-nginx
```
4. `Создаю файл index.html с заданным содержимым:`
```
cat > index.html <<EOF
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
EOF
```
5. `Создаем Dockerfile`
```
cat > Dockerfile <<EOF
FROM nginx:1.21.1
COPY index.html /usr/share/nginx/html/index.html
EOF
```
6. `Скачиваем базовый образ nginx:1.21.1`

```
docker pull nginx:1.21.1
```
7. `Собираю  образ, логинюсь в DockerHub и ПУШ в репозиторий.`

![1](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img1.png)
![2](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img2.png)

8. `Запускаю контейнер из собранного образа и проверяю`
```
docker run -d -p 8081:80 foxbeer13/custom-nginx:1.0.0
```
![3](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img3.png)

```
curl http://localhost:8081
```

![4](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img4.png)

```
#Ссылка на репозиторий
https://hub.docker.com/repository/docker/foxbeer13/custom-nginx/general
```


---

### Задание 2

1. `Запуск контейнера с указанными параметрами`
```
docker run --name tatatyncevaa-custom-nginx-t2 -d -p 127.0.0.1:8081:80 foxbeer13/custom-nginx:1.0.0
```
![5](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img5.png)



2. `Переименовать по команде`

```
docker rename tatatincevaa-custom-nginx-t2 custom-nginx-t2
```
![6](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img6.png)

3. `Выполнение комплексной команды`
```
date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8081 ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html

```
 `Вывод`

```
alexey@dell:~/virt_docker_compose/custom-nginx$ date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8081 ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
18-05-2025 21:15:08.598334442 MSK
CONTAINER ID   IMAGE                          COMMAND                  CREATED         STATUS         PORTS                    NAMES
1654c7f7fe27   foxbeer13/custom-nginx:1.0.0   "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   127.0.0.1:8081->80/tcp   custom-nginx-t2
LISTEN  0       4096              127.0.0.1:8081         0.0.0.0:*              
172.17.0.1 - - [18/May/2025:18:14:10 +0000] "GET / HTTP/1.1" 200 95 "-" "curl/7.68.0" "-"
PGh0bWw+CjxoZWFkPgpIZXksIE5ldG9sb2d5CjwvaGVhZD4KPGJvZHk+CjxoMT5JIHdpbGwgYmUg
RGV2T3BzIEVuZ2luZWVyITwvaDE+CjwvYm9keT4KPC9odG1sPgo=

```

4. `Проверка доступности`
```
alexey@dell:~/virt_docker_compose/custom-nginx$ curl -v http://127.0.0.1:8081
*   Trying 127.0.0.1:8081...
* TCP_NODELAY set
* Connected to 127.0.0.1 (127.0.0.1) port 8081 (#0)
> GET / HTTP/1.1
> Host: 127.0.0.1:8081
> User-Agent: curl/7.68.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Server: nginx/1.21.1
< Date: Sun, 18 May 2025 18:17:05 GMT
< Content-Type: text/html
< Content-Length: 95
< Last-Modified: Sun, 18 May 2025 16:47:53 GMT
< Connection: keep-alive
< ETag: "682a0f39-5f"
< Accept-Ranges: bytes
< 
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
* Connection #0 to host 127.0.0.1 left intact
```
![7](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img7.png)
---

### Задание 3


1. `Подключение к стандартным потокам контейнера`

```
alexey@dell:~/virt_docker_compose/custom-nginx$ docker attach custom-nginx-t2
^C2025/05/18 18:31:07 [notice] 1#1: signal 2 (SIGINT) received, exiting
2025/05/18 18:31:07 [notice] 32#32: exiting
2025/05/18 18:31:07 [notice] 32#32: exit
2025/05/18 18:31:07 [notice] 31#31: exiting
2025/05/18 18:31:07 [notice] 31#31: exit
2025/05/18 18:31:07 [notice] 1#1: signal 17 (SIGCHLD) received from 31
2025/05/18 18:31:07 [notice] 1#1: worker process 31 exited with code 0
2025/05/18 18:31:07 [notice] 1#1: signal 29 (SIGIO) received
2025/05/18 18:31:07 [notice] 1#1: signal 17 (SIGCHLD) received from 32
2025/05/18 18:31:07 [notice] 1#1: worker process 32 exited with code 0
2025/05/18 18:31:07 [notice] 1#1: exit

После нажатия Ctrl-C контейнер остановится, потому что:
-Nginx запущен в foreground режиме (основной процесс контейнера)
-Ctrl-C отправляет SIGINT сигнал основному процессу
-При завершении основного процесса контейнер останавливается

```
![8](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img8.png)

2. `Перезапускаю контейнер и захожу в интерактивный режим`\
```
alexey@dell:~/virt_docker_compose/custom-nginx$ docker start custom-nginx-t2
custom-nginx-t2
alexey@dell:~/virt_docker_compose/custom-nginx$ docker exec -it custom-nginx-t2 bash
root@1654c7f7fe27:/# 
```

3. `Ставлю nano для редактирования conf файла`
```
apt-get update && apt-get install -y nano
nano /etc/nginx/conf.d/default.conf
```

4. `Через nano меняю порт с 80 на 81, сохраняю. Перезагружаю Nginx и проверяю`

```
root@1654c7f7fe27:/# nano /etc/nginx/conf.d/default.conf
root@1654c7f7fe27:/# nginx -s reload
2025/05/18 18:40:20 [notice] 310#310: signal process started
root@1654c7f7fe27:/# curl http://127.0.0.1:80
curl: (7) Failed to connect to 127.0.0.1 port 80: Connection refused
root@1654c7f7fe27:/# curl http://127.0.0.1:81
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
root@1654c7f7fe27:/# 
```


5. `Проверка состояния`

```
alexey@dell:~/virt_docker_compose/custom-nginx$ ss -tlpn | grep 127.0.0.1:8081
nginx-t2
curl -v http://127.0.0.1:8081LISTEN  0       4096              127.0.0.1:8081         0.0.0.0:*              
alexey@dell:~/virt_docker_compose/custom-nginx$ docker port custom-nginx-t2
80/tcp -> 127.0.0.1:8081
alexey@dell:~/virt_docker_compose/custom-nginx$ curl -v http://127.0.0.1:8081
*   Trying 127.0.0.1:8081...
* TCP_NODELAY set
* Connected to 127.0.0.1 (127.0.0.1) port 8081 (#0)
> GET / HTTP/1.1
> Host: 127.0.0.1:8081
> User-Agent: curl/7.68.0
> Accept: */*
> 
* Recv failure: Соединение разорвано другой стороной
* Closing connection 0
curl: (56) Recv failure: Соединение разорвано другой стороной
```

6. `Удаление без остановки`

```alexey@dell:~/virt_docker_compose/custom-nginx$ docker rm -f custom-nginx-t2
custom-nginx-t2
```
![9](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img9.png)


### Задание 4


1. `Запуск контейнера CentOS`
```
docker run -d -v $(pwd):/data --name centos-container centos:7 tail -f /dev/null

```
2. `Запуск контейнера Debian`

```
docker run -d -v $(pwd):/data --name debian-container debian:11 tail -f /dev/null
```
3. `Проверяем запуск docker ps`
```
docker ps
```
![10](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img10.png)

4. `Создание файла в контейнере CentO, подключаюсь к нему`

```
docker exec -it centos-container bash 
touch /data/centos_file.txt
```
5. `Такую же процедуру делаем на Debian`
```
docker exec -it debian-container bash 
touch /data/debian_file.txt

```
6. `На Хостовой машине создаем файл.`

```
touch host_file.txt
```
7. `Как итог на хостовой машине видны файлы которые создавались на Deb и CentOs контейнерах`

![11](https://github.com/Foxbeerxxx/virt_docker_compose/blob/main/img/img11.png)

8. `Для примера проверки еще подключусь на Deb контейнер`

```
alexey@dell:~/virt_docker_compose/custom-nginx$ docker exec -it debian-container bash -c "ls -l /data && cat /data/*"
total 8
-rw-rw-r-- 1 1000 1000 67 May 18 16:49 Dockerfile
-rw-r--r-- 1 root root  0 May 18 19:17 centos_file.txt
-rw-r--r-- 1 root root  0 May 18 19:20 debian_file.txt
-rw-rw-r-- 1 1000 1000  0 May 18 19:22 host_file.txt
-rw-rw-r-- 1 1000 1000 95 May 18 16:47 index.html
FROM nginx:1.21.1
COPY index.html /usr/share/nginx/html/index.html
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```

### Задание 5


1. `Создание директории и файлов`
```
mkdir -p /tmp/netology/docker/task5 && cd /tmp/netology/docker/task5

# compose.yaml

cat > compose.yaml <<EOF
 version: "3"
 services:
   portainer:
     network_mode: host
     image: portainer/portainer-ce:latest
     volumes:
       - /var/run/docker.sock:/var/run/docker.sock
 EOF


#docker-compose.yaml

cat > docker-compose.yaml <<EOF
version: "3"
services:
  registry:
    image: registry:2
    ports:
      - "5000:5000"
EOF

```
2. `Запуск docker compose up -d`

```

Был запущен compose.yaml, потому что Docker Compose v2+ по умолчанию ищет файлы в порядке приоритета.
1)compose.yaml (новый стандарт)
2)docker-compose.yaml (старый формат)
 
```
3. `Чтобы запустить оба сервиса (portainer и registry) в одном проекте, можно использовать include`

```
cat > compose.yaml <<EOF
version: "3.8"
include:
  - docker-compose.yaml
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
EOF
```