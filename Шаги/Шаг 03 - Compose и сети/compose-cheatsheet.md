🧩 Универсальный шаблон `docker-compose.yml`

```yaml
version: "3.8"  # ✅ Можно опустить в новых версиях Docker Compose

services:
  <имя_сервиса>:
    image: <имя_образа>  # Например: nginx, postgres:15, your/image:tag
    container_name: <контейнер_имя>  # ❗️Необязательно. Укажи для удобства
    ports:
      - "80:80"  # ❗️Необязательно. Пробрасывай порты, если нужен доступ снаружи
    volumes:
      - ./data:/data  # ❗️Необязательно. Монтируй директории, если нужно
    environment:
      - ENV_VAR=value  # ❗️Необязательно. Переменные окружения
    depends_on:
      - <другой_сервис>  # ❗️Необязательно. Используй, если есть зависимости
    restart: unless-stopped  # ❗️Необязательно. Полезно для автозапуска
    networks:
      - default  # ❗️Необязательно, если не создаёшь кастомные сети

# ❗️Необязательно: можно определить пользовательские сети
networks:
  default:
    driver: bridge

```

## 🔹 Что менять под свой проект


| Что                    | Что вписать                                                         |
| ------------------------- | ----------------------------------------------------------------------------- |
| `<имя_сервиса>` | Например:`nginx`,`db`,`app`                                           |
| `<имя_образа>`   | Docker Hub-образ (например,`nginx:alpine`,`mysql:8`)             |
| `container_name`          | Удобное имя для отладки (`nginx`,`postgres`)              |
| `ports`                   | `хост:контейнер`, например`8080:80`, если нужно |
| `volumes`                 | Для сохранения данных или конфигов              |
| `environment`             | Переменные:`POSTGRES_USER=admin`,`TZ=UTC`                           |
| `depends_on`              | Если`app`зависит от`db`— добавь это                    |
| `restart`                 | Например:`always`,`on-failure`,`unless-stopped`                       |

💡 Пример с двумя сервисами: `nginx + whoami`

```yaml
version: "3.8"

services:
  nginx:
    image: nginx:alpine
    container_name: nginx
    ports:
      - "8080:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - whoami
    restart: unless-stopped

  whoami:
    image: traefik/whoami
    container_name: whoami
    restart: unless-stopped
```

## 🛠 **Основные команды Docker Compose**


| Команда                    | Объяснение                                                 |
| --------------------------------- | -------------------------------------------------------------------- |
| `docker compose up`               | Запустить все сервисы                             |
| `docker compose up -d`            | Запустить в фоне (detached mode)                       |
| `docker compose down`             | Остановить и удалить всё, что создано |
| `docker compose build`            | Собрать образы из Dockerfile                          |
| `docker compose restart`          | Перезапустить сервисы                            |
| `docker compose ps`               | Показать список запущенных сервисов  |
| `docker compose logs`             | Показать логи                                            |
| `docker compose logs -f`          | Логи в реальном времени                          |
| `docker compose exec имя bash` | Зайти внутрь работающего сервиса        |
| `docker compose config`           | Проверить валидность docker-compose.yml           |

⚠️ Начиная с Docker v20.10+ используется `docker compose` (с пробелом), **а не**`docker-compose` (через дефис).

## 🧹 **Полезные приёмы**

```bash
# Убить всё и очистить
docker compose down --volumes

# Пересобрать образы и перезапустить
docker compose up -d --build

# Проверить конфиг на ошибки
docker compose config

```



## 📦 **Файлы .env (для env\_file)**

Пример .env:

```ini
MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=appdb
TZ=Europe/Moscow
```
