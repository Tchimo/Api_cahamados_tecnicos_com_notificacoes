# Helpdesk API

API REST completa de chamados técnicos, desenvolvida em Python para portfólio de backend júnior.

## Stack

FastAPI, SQLAlchemy 2, PostgreSQL, JWT, Argon2, Docker, Pytest e OpenAPI.

## Funcionalidades

- Cadastro e login com JWT.
- Perfis `customer`, `agent` e `admin`.
- Criação, consulta, atualização e filtros de chamados.
- Prioridades com SLA automático: baixa 72h, média 24h, alta 8h e crítica 2h.
- Atribuição, transição de status, resolução e fechamento.
- Comentários públicos e internos.
- Histórico de alterações.
- Métricas de atendimento e chamados atrasados.
- Swagger em `/docs` e ReDoc em `/redoc`.
- CORS configurável e execução com Docker.

## Executar com Python

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
uvicorn app.main:app --reload
```

Acesse `http://localhost:8000/docs`.

## Executar com Docker

```bash
docker compose up --build
```

## Testes

```bash
pytest -q
```

## Rotas principais

| Método | Rota | Acesso |
|---|---|---|
| POST | `/api/v1/auth/register` | público |
| POST | `/api/v1/auth/login` | público |
| POST | `/api/v1/tickets` | autenticado |
| GET | `/api/v1/tickets` | autenticado |
| GET | `/api/v1/tickets/{id}` | autenticado |
| PATCH | `/api/v1/tickets/{id}` | autenticado |
| GET/POST | `/api/v1/tickets/{id}/comments` | autenticado |
| GET | `/api/v1/tickets/{id}/history` | autenticado |
| GET | `/api/v1/tickets/metrics/summary` | agent/admin |
| GET | `/health` | público |

## Exemplo rápido

```bash
curl -X POST http://localhost:8000/api/v1/auth/register \\
  -H 'Content-Type: application/json' \\
  -d '{"name":"Ana","email":"ana@example.com","password":"senha-segura-123"}'
```

Depois faça login, copie `access_token`, clique em **Authorize** no Swagger e use `Bearer TOKEN`.

## Decisões técnicas

A aplicação usa camadas simples de rotas, schemas, modelos, segurança e dependências. O banco é criado automaticamente para facilitar o primeiro uso; em um ambiente produtivo, substitua isso por migrations Alembic. O envio de e-mail e anexos pode ser conectado posteriormente a Celery/Redis e armazenamento S3.


