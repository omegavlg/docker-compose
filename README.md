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

Соберем образ, для этого в директории, где находятся Dockerfile и index.html, выполним команду:
```
docker build -t custom-nginx:1.0.0 .
```
<img src = "img/03.png" width = 100%>

Авторизуемся в Docker Hub, чтобы загрузить образ в созданный репозиторий командой:
```
docker login
```
<img src = "img/04.png" width = 100%>

Тегируем образ и отправляем в репозиторий:
```
docker tag custom-nginx:1.0.0 omegavlg/custom-nginx:1.0.0
```
```
docker push omegavlg/custom-nginx:1.0.0
```
<img src = "img/05.png" width = 100%>

Ссылка на образ в Docker Hub:

https://hub.docker.com/repository/docker/omegavlg/custom-nginx/general

---
## Задание 2

1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080

2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"

3. Выполните команду date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html

4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

### Ответ:

