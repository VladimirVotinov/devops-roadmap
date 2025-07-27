## 🧩 Универсальный `Dockerfile`

```Dockerfile
# ✅ Базовый образ — выбирается под задачу: язык, фреймворк, система
FROM <base-image>:<tag>  
# Примеры:
# FROM ubuntu:22.04
# FROM node:20-alpine
# FROM python:3.12-slim

# ❗️Необязательно: установка зависимостей (если нужно)
# RUN apt-get update && apt-get install -y <packages>

# ❗️Необязательно: установка зависимостей через менеджер (npm, pip, poetry и др.)
# COPY requirements.txt .
# RUN pip install -r requirements.txt

# Копируем все файлы проекта внутрь контейнера
COPY . /app  

# Переходим в рабочую директорию внутри контейнера
WORKDIR /app  

# ❗️Необязательно: установка прав
# RUN chmod +x your_script.sh

# ❗️Необязательно: сборка (если нужно)
# RUN npm run build / RUN make и т.д.

# ❗️Необязательно: переменные окружения
# ENV ENV_VAR=value

# Порт, который будет открыт внутри контейнера
EXPOSE <порт>  
# Например: EXPOSE 80 или 3000

# Команда, которая запускает приложение
CMD ["<команда>", "<аргументы>"]
# Примеры:
# CMD ["nginx", "-g", "daemon off;"]
# CMD ["python", "app.py"]
# CMD ["node", "server.js"]

```

## 🔹 Что менять


| Строка | Описание                            | Пример                          |
| ------------ | ------------------------------------------- | ------------------------------------- |
| `FROM`       | Базовый образ                   | `python:3.12-slim`                    |
| `COPY`       | Копирование файлов         | `COPY . /app`                         |
| `WORKDIR`    | Рабочая директория         | `/app`,`/code`,`/usr/src/app`         |
| `RUN`        | Установка зависимостей | `RUN pip install -r requirements.txt` |
| `EXPOSE`     | Порт                                    | `80`,`5000`,`3000`                    |
| `CMD`        | Запуск                                | `["python", "main.py"]`               |

## 💡 Пример простого Dockerfile для Python API

```Dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]

```

## 🐳 Docker: Команды, советы и подсказки

## 🔧 Основные команды Docker

### 📜 Показать все команды и справку

```bash
docker
```

### 🔍 Информация о версии

```bash
docker version
```

### 🧠 Общая информация о системе Docker

```bash
docker info
```

## 📦 Работа с контейнерами

### 🚀 Запуск контейнера в фронтграунде

```bash
docker container run -it -p 80:80 nginx
```

### 🌙 Запуск контейнера в фоне

```bash
docker container run -d -p 80:80 nginx
```

### 🏷 Именование контейнера

```bash
docker container run -d -p 80:80 --name nginx-server nginx
```

### 📌 Что делает команда run:

- 🔍 Ищет образ nginx в кеше
- ⬇️ Если не найден — скачивает с DockerHub
- 📦 Создаёт новый контейнер
- 🔁 Пробрасывает порты (host:container → 80:80)
- 🎯 Можно указать другую версию: nginx:1.19

### 📋 Список запущенных контейнеров

```bash
docker container ls
```

или

```bash
docker ps
```

### 📋 Список всех контейнеров

```bash
docker container ls -a
```

### 🛑 Остановить контейнер

```bash
docker container stop [ID или NAME]
```

### 🛑 Остановить все контейнеры

```bash
docker stop $(docker ps -aq)
```

### 🧹 Удалить контейнер

```bash
docker container rm [ID]
```

### ❗ Удалить запущенный контейнер с флагом -f

```bash
docker container rm -f [ID]
```

### 🧹 Удалить все контейнеры

```bash
docker rm $(docker ps -aq)
```

### 📜 Просмотр логов

```bash
docker container logs [NAME]
```

### 🧬 Процессы в контейнере

```bash
docker container top [NAME]
```

## 🖼 Работа с образами

### 📋 Список образов

```bash
docker image ls
```

### ⬇️ Скачивание образа

```bash
docker pull [IMAGE]
```

### ❌ Удаление образа

```bash
docker image rm [IMAGE]
```

### ❌ Удаление всех образов

```bash
docker rmi $(docker images -a -q)
```

### ℹ️ Образ ≠ ОС

Образ — это бинарник приложения + зависимости. Ядро не включается — его предоставляет хост.

### 🔧 Примеры запуска:

**NGINX:**

```bash
docker container run -d -p 80:80 --name nginx nginx
```

**Apache (httpd):**

```bash
docker container run -d -p 8080:80 --name apache httpd
```

**MongoDB:**

```bash
docker container run -d -p 27017:27017 --name mongo mongo
```

**MySQL:**

```bash
docker container run -d -p 3306:3306 --name mysql --env MYSQL_ROOT_PASSWORD=123456 mysql
```

## 🔍 Информация о контейнере

### 🧾 Инспекция контейнера

```bash
docker container inspect [NAME]
```

### 📌 Получить IP контейнера

```bash
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' [NAME]
```

### 📊 Мониторинг ресурсов

```bash
docker container stats [NAME]
```

## 🔓 Доступ внутрь контейнеров

### 🛠 Создать и войти в контейнер NGINX

```bash
docker container run -it --name mynginx nginx bash
```

### 🐧 Ubuntu контейнер

```bash
docker container run -it --name ubuntu ubuntu
```

### ❎ Удаляется после выхода

```bash
docker container run --rm -it --name test ubuntu
```

### 🔁 Войти в уже созданный контейнер

```bash
docker container start -ai ubuntu
```

### 📁 Редактирование конфигов через bash

```bash
docker container exec -it mysql bash
```

### 🧬 Alpine (очень лёгкий дистрибутив)

```bash
docker container run -it alpine sh
```

## 🌐 Работа с сетями

### 🔍 Порты контейнера

```bash
docker container port [NAME]
```

### 📡 Список сетей

```bash
docker network ls
```

### 🔬 Информация о сети

```bash
docker network inspect [NETWORK_NAME]
```

### ➕ Создать сеть

```bash
docker network create [NETWORK_NAME]
```

### 📦 Запустить контейнер в сети

```bash
docker container run -d --name mynginx --network [NETWORK_NAME] nginx
```

### 🔗 Подключить контейнер к сети

```bash
docker network connect [NETWORK_NAME] [CONTAINER_NAME]
```

### 🔌 Отключить контейнер от сети

```bash
docker network disconnect [NETWORK_NAME] [CONTAINER_NAME]
```

## 🏷 Теги и DockerHub

### 🔄 Переименовать образ

```bash
docker image tag nginx yourname/nginx
```

### ⬆️ Загрузить образ в DockerHub

```bash
docker push yourname/nginx
```

### 🔐 Войти в DockerHub

```bash
docker login
```

### ➕ Добавить тег

```bash
docker image tag yourname/nginx yourname/nginx:latest
```

## 🏗 Dockerfile

### Основные директивы:

- **FROM** — базовый образ
- **RUN** — команды установки/настройки
- **COPY** — копирование файлов
- **WORKDIR** — рабочая директория
- **EXPOSE** — открытие порта
- **CMD** — команда запуска

### Сборка образа:

```bash
docker image build -t myapp .
```

## 💽 Volumes (Тома)

### 📋 Список томов

```bash
docker volume ls
```

### 🧹 Удаление неиспользуемых томов

```bash
docker volume prune
```

### Пример с именованным томом:

```bash
docker volume create mysql_data
docker container run -d --name mysql -v mysql_data:/var/lib/mysql mysql
```

### Инспекция тома

```bash
docker volume inspect mysql_data
```

## 🔗 Bind Mounts

```bash
docker run -v $(pwd):/app nginx
```

На Windows:

```bash
docker run -v //c/Users/you/app:/app nginx
```

## 📜 Docker Compose

```yaml
version: '3'
services:
  nginx:
    image: nginx
    ports:
      - "80:80"
    volumes:
      - .:/usr/share/nginx/html
```

### ▶️ Запуск:

```bash
docker-compose up
```

### 🧙‍♂️ В фоне:

```bash
docker-compose up -d
```

### 🧹 Остановка:

```bash
docker-compose down
```
