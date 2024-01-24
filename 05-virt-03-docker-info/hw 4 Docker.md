#Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

###Задача 1

Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
Зарегистрируйтесь и создайте публичный репозиторий с именем "custom-nginx" на https://hub.docker.com;
скачайте образ nginx:1.21.1;   
Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:

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

Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 .
Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .


####ответ 1
https://hub.docker.com/repository/docker/ubik84/custom-nginx/general
#####решение
Nano Dockerfile
    
        FROM nginx:1.21.1
        RUN rm -rf /usr/share/nginx/html/*
        COPY index.html /usr/share/nginx/html/
    

Создаем новый index.html
    
        Nano index.html
        <html>
        <head>
        Hey, Netology
        </head>
        <body>
        <h1>I will be DevOps Engineer!</h1>
        </body>
        </html>
    

Выполняем сборку
    
        Sudo docker build -t custom-nginx:1.0.0 .
        Sudo docker images
    

Запускаем конт
    
        docker run -p 8080:80 custom-nginx:1.0.0
        Проверяем http://localhost:8080
    
Отправляем образ в докер хаб
        
        Docker login (login,pass)
        Docker tag custom-nginx:1.0.0 <your-name>/<your-repository>:1.0.0
        Docker push <your-name>/<your-repository>:1.0.0


###Задача 2

Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:

имя контейнера "ФИО-custom-nginx-t2"
контейнер работает в фоне
контейнер опубликован на порту хост системы 127.0.0.1:8080

Переименуйте контейнер в "custom-nginx-t2"
Выполните команду date +"%d-%m-%Y %T.%N %Z" && sleep 0.150 && docker ps && ss -tlpn | grep 127.0.0.1:8080  && docker logs custom-nginx-t2 -n1 && docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

#### ответ 2
    скриншоты приложил

##### решение
    Sudo docker run —name demchenkoAM-custom-nginx-t2 –d –p 127.0.0.1:8080:80 custom-nginx:1.0.0
    Sudo docker rename demchenkoAM-custom-nginx-t2 custom-nginx-t2
    Curl http:\\127.0.0.1
для себя

        date +"%d-%m-%Y %T.%N %Z": Выводит текущую дату и время в формате "день-месяц-год час:минута:секунда.наносекунда часовой_пояс". Например, "01-01-2023 12:34:56.789012345 UTC".
        sleep 0.150: Приостанавливает выполнение команд на 0.15 секунды.
        docker ps: Выводит список активных контейнеров Docker.
        ss -tlpn | grep 127.0.0.1:8080: Проверяет, есть ли активное соединение на порту 8080. Это может использоваться для проверки того, что контейнер слушает порт 8080 на локальном хосте.
        docker logs custom-nginx-t2 -n1: Выводит последнюю строку логов контейнера с именем "custom-nginx-t2".
        docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html: Запускает команду внутри контейнера. В данном случае, она используется для кодирования в base64 содержимого файла "/usr/share/nginx/html/index.html".

###Задача 3
1.	Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2
2.	Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3.	Выполните docker ps -a и объясните своими словами почему контейнер остановился.
4.	Перезапустите контейнер

##### решение 3

        docker attach custom-nginx-t2
Ctrl-C остановило работу контейнера (Ctrl+D)
Ctrl-C остановило nginx c кодом code 0 , а так –как активных процессов в контейнере нет то он завершил работу.

        docker restart custom-nginx-t2

5.	Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.

        docker exec -it custom-nginx-t2 /bin/bash

6.	Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.

        #Apt-get update
        #Apt-get install nano

7.	Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".

        #Nano /etc/nginx/conf.d/default.con

8.	Запомните(!) и выполните команду nginx -s reload, а затем внутри контейнера curl http://127.0.0.1:80 && curl http://127.0.0.1:81.

        #nginx -s reload 

10.	Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8080 , docker port custom-nginx-t2, curl http://127.0.0.1:8080. Кратко объясните суть возникшей проблемы.

    ответ:
        -В контейнере настроена маршрутизация 80 порт проброшен на 8080 
        -А nginx вещает на 81 порту

* Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно.

        –	Установил в контейнер socat
        –	Пробросил с 81 на 80

        sudo docker exec -d custom-nginx-t2 socat TCP-LISTEN:80,fork TCP:localhost:81

        Без перезапусков и остановок

Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

        sudo docker rm -f custom-nginx-t2

###Задача 4
1 - Запустите первый контейнер из образа centos c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог \$(pwd) на хостовой машине в /data контейнера.
2 - Запустите второй контейнер из образа debian в фоновом режиме, подключив текущий рабочий каталог \$(pwd) в /data контейнера.
3 - Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
4 - Добавьте ещё один файл в каталог /data на хостовой машине.
5 - Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.

    1   sudo docker run -it -d -v $(pwd):/data --name centos-it centos
    3   sudo docker exec -it centos-it /bin/bash
    3       #touch test1.txt
    2   sudo docker run -it -d -v $(pwd):/data --name debian-it debian
    5   sudo docker exec -it debian-it /bin/bash
    5       #Cd data/
    5       #Ls
Получилась общая диретория хоста и контов

###Задача 5
1.	Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него. "compose.yaml" с содержимым:

        version: "3"
        services:
        portainer:
            image: portainer/portainer-ce:latest
            network_mode: host
            ports:
            - "9000:9000"
            volumes:
            - /var/run/docker.sock:/var/run/docker.sock
            
    "docker-compose.yaml" с содержимым:

        version: "3"
        services:
        registry:
            image: registry:2
            network_mode: host
            ports:
            - "5000:5000"

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-file/03-compose-file/)

        mkdir -p /tmp/netology/docker/task5
        cd /tmp/netology/docker/task5
        nano compose.yaml
        nano docker-compose.yaml
        docker compose up -d
Запустился  compose.yaml по умолчанию Docker Compose ищет файл с именем compose.yml или compose.yaml

5 - 1. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

        version: "3"
        services:
        portainer:
            image: portainer/portainer-ce:latest
            network_mode: host
            ports:
            - "9000:9000"
            volumes:
            - /var/run/docker.sock:/var/run/docker.sock
        registry:
            extends:
            file: docker-compose.yaml
            service: registry

5 - 2.	Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/

        docker build -t custom-nginx:latest /Dockerfile
        docker tag custom-nginx:latest localhost:5000/custom-nginx:latest
        docker push localhost:5000/custom-nginx:latest

5 - 3.	Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)

        да

5 - 4.	Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

        version: '3'
        services:
        nginx:
        image: 127.0.0.1:5000/custom-nginx
        ports:
        - "9090:80"

1.	Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

        принт в файлах

2.	Удалите любой из манифестов компоуза(например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите тестовый стенд ОДНОЙ(обязательно!!) командой.

    появилась ошибка

        Found orphan containers ([task5-portainer-1]) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up. 
    
    лечение 

        docker compose up -d --remove-orphans

        poweroff
