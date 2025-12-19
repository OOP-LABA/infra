# Infra: backend + frontend

Минимальный compose для локального/простого деплоя.

## Как запустить
1. Клонировать и подтянуть сабмодули:
   ```bash
   git clone <infra-repo-url>
   cd infra
   git submodule update --init --recursive
   ```
2. Собрать и поднять:
   ```bash
   docker compose up --build
   ```
3. URLы: фронт http://localhost:5173, бэк http://localhost:8080/api/v1, Postgres на 5432.

## Переопределить API URL фронта
`VITE_API_BASE_URL` подставляется на этапе сборки образа. Меняй так:
```bash
docker compose build --build-arg VITE_API_BASE_URL=http://localhost:8080/api/v1 frontend
```
или пропиши значение в `docker-compose.yml` в `frontend.build.args`.

## Что внутри
- `backend/Dockerfile` — multi-stage: сам собирает Spring Boot jar, запускает на 8080.
- `client/Dockerfile` — билдит Vite, отдаёт через nginx с fallback на `index.html`.
- Postgres 15 с томом `db_data`.

При изменениях в сабмодулях не забудь закоммитить их отдельно и обновить указатели в этом репо.
