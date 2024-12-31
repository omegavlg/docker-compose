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

Выполняем команды задания:
```
docker run -d --name dedyurindn-custom-nginx-t2 -p 127.0.0.1:8080:80 custom-nginx:1.0.0
```
```
docker rename dedyurindn-custom-nginx-t2 custom-nginx-t2
```
```
date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
```
```
curl http://127.0.0.1:8080
```
<img src = "img/06.png" width = 100%>

---
## Задание 3

1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните docker ps -a и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду nginx -s reload, а затем внутри контейнера curl http://127.0.0.1:80 ; curl http://127.0.0.1:81.
9. Выйдите из контейнера, набрав в консоли exit или Ctrl-D.
10. Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8080 , docker port custom-nginx-t2, curl http://127.0.0.1:8080. Кратко объясните суть возникшей проблемы.
11. Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно.
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

### Ответ:

Чтобы подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2" выполним команду:
```
docker attach custom-nginx-t2
```
<img src = "img/07.png" width = 100%>

Нажимаем Ctrl-C

<img src = "img/08.png" width = 100%>

Контейнер остановился, потому что при нажатии Ctrl-C был завершён основной процесс контейнера (nginx), который и поддерживал работу контейнера. В Docker контейнер прекращает свою работу, когда завершается его основной процесс.

Перезапускаем контейнер командой:
```
docker start custom-nginx-t2
```
<img src = "img/09.png" width = 100%>

Заходим в интерактивный терминал контейнера с оболочкой bash:
```
docker exec -it custom-nginx-t2 bash
```
Устанавливаем редактор nano
```
apt-get update && apt-get install -y nano
```
<img src = "img/10.png" width = 100%>

Открываем на редактирование конфигурационный файл nginx:
```
nano /etc/nginx/conf.d/default.conf
```
Находим строку с "listen 80" и заменяем ее на "listen 81"

<img src = "img/11.png" width = 100%>

Выполняем перезагрузку nginx и проверяем доступность серверов:
```
nginx -s reload
```
```
curl http://127.0.0.1:80
```
```
curl http://127.0.0.1:81
```
<img src = "img/12.png" width = 100%>

Выходим из контейнера и выполняем команды:
```
ss -tlpn | grep 127.0.0.1:8080
```
```
docker port custom-nginx-t2
```
```
curl http://127.0.0.1:8080
```
<img src = "img/13.png" width = 100%>

Проблема возникает потому, что мы изменили порт внутри контейнера, но не изменили маппинг портов на хосте. Теперь nginx слушает на порту 81 внутри контейнера, но хост всё ещё пытается направлять трафик на порт 80 контейнера.

Удаляем контейнер "custom-nginx-t2", не останавливая его, командой:
```
docker rm -f custom-nginx-t2
```
<img src = "img/14.png" width = 100%>

---
## Задание 4

- Запустите первый контейнер из образа centos c любым тегом в фоновом режиме, подключив папку текущий рабочий каталог $(pwd) на хостовой машине в /data контейнера, используя ключ -v.
- Запустите второй контейнер из образа debian в фоновом режиме, подключив текущий рабочий каталог $(pwd) в /data контейнера.
- Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
- Добавьте ещё один файл в текущий каталог $(pwd) на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

### Ответ:

Использованные команды в процессе выполнения задания:

```
docker run -d -v $(pwd):/data --name centos-container -it centos:latest
```
```
docker run -d -v $(pwd):/data --name debian-container -it debian:latest
```
<img src = "img/15.png" width = 100%>

```
docker exec -it centos-container bash
```
```
echo "File1" > /data/file1.txt
exit
```
```
echo "File2" > file2.txt
```
<img src = "img/16.png" width = 100%>

```
docker exec -it debian-container bash
```
```
ls /data
cat /data/file1.txt
cat /data/file2.txt
exit
```
<img src = "img/17.png" width = 100%>

---
## Задание 5

1. Создайте отдельную директорию (например /tmp/netology/docker/task5) и 2 файла внутри него. "compose.yaml" с содержимым:

```
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

"docker-compose.yaml" с содержимым:

```
version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/

4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer. (логин и пароль адмнистратора)

5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```

6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

### Ответ:

Создадим в директории /opt файлы "compose.yaml" и "docker-compose.yaml" с содержимым, указанным в задании.

<img src = "img/18.png" width = 100%>

Выполним команду:
```
docker compose up -d
```

<img src = "img/19.png" width = 100%>

Был запущен файл compose.yaml, так как Docker по умолчанию ищет файл с именем docker-compose.yaml и compose.yaml, но compose.yaml имеет приоритет.

Редактируем compose.yaml, чтобы запустились оба файла:
```
version: "3"

include:
  - docker-compose.yaml
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

И снова выполняем команду "docker compose up -d"

<img src = "img/20.png" width = 100%>

Выполним серию команд, для того чтобы залить образ custom-nginx, изменить ему тег на latest и залить его в локальный репозиторий.
```
docker pull omegavlg/custom-nginx:1.0.0
```
```
docker tag omegavlg/custom-nginx:1.0.0 127.0.0.1:5000/custom-nginx:latest
```
```
docker images
```
```
docker push 127.0.0.1:5000/custom-nginx:latest
```

<img src = "img/21.png" width = 100%>

Заходим в интерфейс, выполняем первоначальную настройку, и деплоим компоуз:

<img src = "img/22.png" width = 100%>

<img src = "img/23.png" width = 100%>

Переходим в containers, выбираем контейнер с nginx. 
<img src = "img/24.png" width = 100%>

Удаляем compose.yaml и выполняем команду "docker compose up -d":
<img src = "img/25.png" width = 100%>

Получаем предупреждение, о том что были обнаружены осиротевшие контейнеры (orphan containers), которые относятся к проекту, но не указаны в текущем Compose-файле. И предлагается выполнить очистку выполнив команду: 
```
docker compose up -d --remove-orphans
```

<img src = "img/26.png" width = 100%>


Далее мы можем погасить весь проект целиком:
```
docker compose down
```

<img src = "img/27.png" width = 100%>


