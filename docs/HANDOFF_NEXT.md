# Что делать дальше (Next)

1) Прокси для исходящих:
   - В workflow reports_send_now открыть ноду HTTP Request к Telegram → Options → Proxy → указать host/port/логин (пароль из Credentials) → тест.
   - (Опц.) В docker-compose.yml сервиса n8n добавить:
     HTTP_PROXY, HTTPS_PROXY, NO_PROXY="localhost,127.0.0.1,n8n,postgres,redis"
     Перезапуск: docker compose up -d

2) OpenAI проверка:
   - Нода HTTP Request → GET https://api.openai.com/v1/models
   - Headers: Authorization: Bearer <OPENAI_API_KEY>
   - Включить Proxy как в п.1 → убедиться, что ответ 200.

3) Отчёты по расписанию:
   - Сделать cron-воркфлоу (daily/hourly) на базе reports_send_now.
   - Развести free/premium (2 Chat ID, разные payload).

4) Ingest Telegram экспортов:
   - Workflow ingest_telegram_exports читает /home/node/.n8n/tg_exports/result.json|messages.json,
     извлекает тикеры и дергает /webhook/signals/enrich (rate-limit + idempotency по (symbol,t0)).

5) Улучшения:
   - В отчёты добавить из v_memory_pack 1-2 последних заметки/тега.
   - Мини-health workflow (пинг БД/Redis, состояние очередей, push в приватный чат).

6) Ритуал:
   - Каждый шаг: agent-log "actor" "что сделал"
   - В конце: agent-stop "где остановился/next"
   - Коммит и push в GitHub (main)
