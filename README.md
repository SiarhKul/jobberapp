# Запуск Jobber App

Этот репозиторий содержит общую инфраструктуру Jobber App и его микросервисы. Инфраструктура запускается из корня репозитория через Docker Compose.

## Что нужно установить

- Docker Engine и Docker Compose v2;
- Node.js и npm — для запуска сервисов локально.

Проверьте установку Docker:

```bash
docker --version
docker compose version
```

## Запуск инфраструктуры

Из корня проекта выполните:

```bash
cd ~/IT/microservices/jobberapp
docker compose up -d
```

Команда запускает Redis, MongoDB, MySQL, PostgreSQL, RabbitMQ, Elasticsearch и Kibana в фоне.

Проверьте состояние контейнеров:

```bash
docker compose ps
```

Посмотреть логи конкретного сервиса:

```bash
docker compose logs -f elasticsearch
docker compose logs -f kibana
docker compose logs -f rabbitmq
```

## Адреса сервисов

| Сервис | Адрес или порт | Учётные данные |
| --- | --- | --- |
| Redis | `localhost:6379` | — |
| MongoDB | `mongodb://localhost:27017` | — |
| MySQL | `localhost:3306` | пользователь `jobber`, пароль `api`, БД `jobber_auth` |
| PostgreSQL | `localhost:5432` | пользователь `jobber`, пароль `api`, БД `jobber_reviews` |
| RabbitMQ AMQP | `amqp://jobber:jobberpass@localhost:5672` | — |
| RabbitMQ UI | [http://localhost:15672](http://localhost:15672) | `jobber` / `jobberpass` |
| Elasticsearch API | [http://localhost:9200](http://localhost:9200) | безопасность отключена |
| Kibana UI | [http://localhost:5601](http://localhost:5601) | — |

## Запуск notification-сервиса

Сначала убедитесь, что инфраструктура уже запущена. Затем в новом терминале:

```bash
cd ~/IT/microservices/jobberapp/server/2-notificatin-server
npm install
npm run dev
```

Настройки окружения notification-сервиса находятся в `server/2-notificatin-server/.env`. Для локального Elasticsearch используется:

```env
ELASTIC_SEARCH_URL=http://localhost:9200
```

Сервис слушает порт `4001`.

## Остановка инфраструктуры

Остановить и удалить контейнеры, сохранив данные в `docker-volumes`:

```bash
cd ~/IT/microservices/jobberapp
docker compose down
```
