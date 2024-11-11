# Для начала нужно установить Linux Oracle на VirtualBox:

Установить образ Linux

Выделить 4 ядра

Выделить 4096 мб оперативной памяти



# Установка docker

Устанавливает утилиту wget на систему

`sudo yum install wget`

![image](https://github.com/user-attachments/assets/a34c05c1-19c8-4333-b327-0e6ddbc7b1df)

Скачиваем файл репозитория

`sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`

![image](https://github.com/user-attachments/assets/3f5bfcff-97c0-49af-b6d6-3d5a11b5a3d6)

Устанавливаем docker

`sudo yum install docker-ce docker-ce-cli containerd.io`

![image](https://github.com/user-attachments/assets/37e76d81-9ebd-4417-9955-c0484ce4d8a6)

Запускаем его и разрешаем автозапуск

`sudo systemctl enable docker --now`

![image](https://github.com/user-attachments/assets/9efa56ba-9f4d-426a-a400-cea2293f87dd)



# Установка compose

Для начала нужно убедиться в наличии пакета curl

`sudo yum install curl`

![image](https://github.com/user-attachments/assets/2620b22f-5a7c-42c7-9613-6f6dce4257f4)

Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней
версии Docker Compose

`COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

Теперь скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin

`sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`

![image](https://github.com/user-attachments/assets/7d9a1df5-eb78-43c2-b8e7-b6d5bf7b3b90)

Предоставление прав на выполнение файла docker-compose

`sudo chmod +x /usr/bin/docker-compose`

Проверка установленной версии Docker Compose

`sudo docker-compose --version`

![image](https://github.com/user-attachments/assets/656ec825-f6ff-468d-809e-872692753b91)



# Делаем grafana

Установка git

`sudo yum install git`

![image](https://github.com/user-attachments/assets/00fcf41a-e716-40ba-aa2e-2a1039c6dfc7)

Этот код скачивает содержимое репозитория skl256/grafana_stack_for_docker

`sudo git clone https://github.com/skl256/grafana_stack_for_docker.git`

Заходит в папку - cd

`cd grafana_stack_for_docker`

cd .. - возвращает в папку выше

![image](https://github.com/user-attachments/assets/d1b08726-15ef-4a93-85a1-839e7a71598f)

Cоздаем папки двумя разными способами

`sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

`sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data}`

Выдаем права

`sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`

Создаем файл

`sudo touch /mnt/common_volume/grafana/grafana-config/grafana.ini`

Копирование файлов

`sudo cp config/* /mnt/common_volume/swarm/grafana/config/`

Переименовывание файла

`sudo mv grafana.yaml docker-compose.yaml`

![image](https://github.com/user-attachments/assets/9a0c45bc-c9dd-481e-aea2-31202e07d7fa)

Собрать докер (нужно запускать из папки где docker-compose.yaml)

`sudo docker compose up -d`

Опустить докер - sudo docker compose stop

![image](https://github.com/user-attachments/assets/92a51c5c-8e75-435c-a8e0-7250acc3b61a)



# Начинаем чистку файлов

Команда открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя

`sudo vi docker-compose.yaml`

Что-бы что-то изменить в тесковом редакторе нужно нажать insert на клавиатуре

Затем в docker-compose нужно вставить node-exporter и удалить ненужные файлы (можно вставить готовый докер)

![image](https://github.com/user-attachments/assets/17111ed9-5e63-4973-8217-d18cbb1bec78)

выйти не сохраняясь из vim - `esc -> :q!`

выйти сохраняясь из vim - `esc -> :wq!`

Заходим в другую папку 

`cd /mnt/common_volume/swarm/grafana/config/`

Открываем файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя

`sudo vi prometheus.yaml`

![image](https://github.com/user-attachments/assets/6ef091ef-4843-4e46-8709-c26fb83ad51a)

Далее нужно исправить targets: на exporter:9100

![image](https://github.com/user-attachments/assets/571e3003-dda1-4fa3-867c-9439e31e06d8)



# Делаем grafana на сайте

- Заходим на сайт localhost:3000

- User и Password - admin, потом где он предлагает установить пароль, нажимаем - скип

- Dashboards - create dashboard - configure a new data source - prometheus

 - Connection: http://prometheus:9090

 - Authentication - basic authentication - admin admin

 - save test

- Dashboards - import dashboard

 - Find and import... - 1860 - load

- prometheus - prometheus

 - import


 
# Делаем VictoriaMetrics

Для начала зайдем в нужную папку

`cd grafana_stack_for_docker`

Открываем docker-compose

`sudo vi docker-compose.yaml`

После prometheus вставляем vmagent (но у нас уже вставлен готовый докер)

![image](https://github.com/user-attachments/assets/dafc71d3-040c-43d8-84d4-36b568ecbaf8)

Ангелина помоги
виктория метрик 8428
