# Домашнее задание к занятию "`Оркестрация группой Docker контейнеров на примере Docker Compose`" - `Дедюрин Денис`

---
## Задание 1

Сценарий выполнения задачи:

- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: {"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}
- Зарегистрируйтесь и создайте публичный репозиторий с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
- Скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:

```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП).
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general.

### Ответ:

Установим docker и docker compose plugin командой:
```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
```
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
<img src = "img/01.png" width = 100%>

Стартуем docker командой:
```
systemctl enable --now docker
```
Скачиваем образ nginx:1.21.1 командой:
```
docker pull nginx:1.21.1
```
<img src = "img/02.png" width = 100%>

Создадим файл Dockerfile с содержимым:
```
FROM nginx:1.21.1
COPY index.html /usr/share/nginx/html/index.html
```

Создадим файл index.html с вашим содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```

В директории, где находятся Dockerfile и index.html, выполним команду:
```
docker build -t custom-nginx:1.0.0 .
```
<img src = "img/03.png" width = 100%>
```
[root@netology-docker-compose netology]# docker build -t custom-nginx:1.0.0 .
[+] Building 0.9s (7/7) FINISHED                                                                                                                                                                                       docker:default
 => [internal] load build definition from Dockerfile                                                                                                                                                                             0.1s
 => => transferring dockerfile: 157B                                                                                                                                                                                             0.0s
 => [internal] load metadata for docker.io/library/nginx:1.21.1                                                                                                                                                                  0.0s
 => [internal] load .dockerignore                                                                                                                                                                                                0.0s
 => => transferring context: 2B                                                                                                                                                                                                  0.0s
 => [internal] load build context                                                                                                                                                                                                0.2s
 => => transferring context: 186B                                                                                                                                                                                                0.0s
 => [1/2] FROM docker.io/library/nginx:1.21.1                                                                                                                                                                                    0.4s
 => [2/2] COPY index.html /usr/share/nginx/html/index.html                                                                                                                                                                       0.1s
 => exporting to image                                                                                                                                                                                                           0.1s
 => => exporting layers                                                                                                                                                                                                          0.0s
 => => writing image sha256:1ca9446cd5b74b88bc19771d76775a1180dca00c7e109fe4bcc6062a926370c0                                                                                                                                     0.0s
 => => naming to docker.io/library/custom-nginx:1.0.0                                                                                                                                                                            0.0s
[root@netology-docker-compose netology]#
```




