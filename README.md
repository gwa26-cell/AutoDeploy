# AutoDeploy — Flask + Docker + GitHub Actions

Учебный проект: Flask API в Docker, Nginx frontend и CI/CD через GitHub Actions с публикацией образа в **GitHub Container Registry (GHCR)**.

## Структура

```
AutoDeploy/
├── app.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── frontend/
│   ├── index.html
│   └── nginx.conf
├── .github/workflows/deploy.yml
└── README.md
```

## Быстрый старт (локально)

```bash
docker compose up --build -d
```

- Веб-интерфейс: http://localhost  
- API напрямую: http://localhost:5000  
- Health через Nginx: http://localhost/api/health  

Остановка:

```bash
docker compose down
```

## GitHub Actions

Workflow `.github/workflows/deploy.yml`:

1. Запускается при **push в `main`** (и вручную через `workflow_dispatch`)
2. Собирает Docker-образ
3. Пушит в GHCR: `ghcr.io/gwa26-cell/autodeploy:latest`

После успешного запуска образ появится во вкладке **Packages** репозитория.

### Опциональный SSH-деплой

Job `deploy` включается переменной репозитория:

- `ENABLE_SSH_DEPLOY=true` (Settings → Variables)

Секреты (Settings → Secrets):

| Secret | Описание |
|--------|----------|
| `SSH_HOST` | IP или домен сервера |
| `SSH_USER` | Пользователь SSH |
| `SSH_PRIVATE_KEY` | Приватный ключ |
| `SSH_PORT` | Порт (по умолчанию 22) |
| `DEPLOY_PATH` | Путь к проекту на сервере |

Без этих настроек job `deploy` пропускается, а сборка и push в GHCR работают.

## Тесты API

```bash
curl http://localhost/api/health
curl http://localhost/api/info
curl http://localhost/api/multiply/10/5
curl http://localhost/api/divide/20/4
```
