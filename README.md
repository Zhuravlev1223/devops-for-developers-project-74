[![push](https://github.com/dmitritoe/devops-for-developers-project-74/actions/workflows/push.yml/badge.svg)](https://github.com/dmitritoe/devops-for-developers-project-74/actions/workflows/push.yml)

[DevOps for Developers](https://hexlet.io/courses/devops-for-developers)

## Требования
- Docker Engine >= 20.10
- Docker Compose >= 1.27
- Node.js 20.x (локально, если не через контейнер)

## Запуск проекта
1. `git clone ...`
2. `cd devops-for-developers-project-74`
3. `cp .env.example .env` (или `.env` создаётся автоматически через Makefile)
4. `docker-compose up -d`
5. http://localhost -> редирект на https://localhost
6. https://localhost -> приложение под Caddy -> Fastify

## Переменные окружения
- DATABASE_HOST=db
- DATABASE_NAME=postgres
- DATABASE_USERNAME=postgres
- DATABASE_PASSWORD=password
- DATABASE_PORT=5432

## Образ Docker Hub
- `dmitritoe/devops-for-developers-project-74:latest`

## Команды Makefile
- `make setup` (установить зависимости + миграции)
- `make dev` (локальный запуск как приложение с hot-reload)
- `make test` (запуск тестов)
- `make ci` (docker-compose тесты)
- `make test-docker` (docker-compose тесты через app)

## CI
- Файл: `.github/workflows/push.yml`
- Проверки запускаются в Docker:
   - `docker-compose -f docker-compose.yml up --abort-on-container-exit`
   - `docker-compose -f docker-compose.yml build app`
   - `docker push dmitritoe/devops-for-developers-project-74:latest`

## Архитектура сервисов
- `docker-compose.yml` — продакшен-строение, без проброса портов, `Dockerfile.production`.
- `docker-compose.override.yml` — dev-сборка, `Dockerfile` и сервисы `app`, `caddy`, `db`, порты 8080/80/443.

## Проверка
- `docker-compose -f docker-compose.yml up --abort-on-container-exit` - запускает тесты.
- `docker-compose up` - запускает `app`, `caddy`, `db`.

## Исключения
- `.env` добавлен в `.gitignore`
- `node_modules` добавлен в `.dockerignore`
