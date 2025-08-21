# Workflows inventory (обновлено вручную)

- signals_enrich — Active — /webhook/signals/enrich — Binance 1m x120 → RSI/ATR → Postgres
- export_signals_json — Active — /webhook/signals/export — JSON export из БД
- healthcheck_fixed — Active — служебный health-пинг
- agent_skeleton_fixed — Active — шаблон/скелет под мини-агентов
- reports_send_now — Inactive — ручной снепшот в Telegram (для Cron)
- ingest_telegram_exports — Inactive — парс Telegram экспортов из /home/node/.n8n/tg_exports/

План: reports_daily_utc8 (Cron, TZ Asia/Singapore, 08:00) с разводкой Free/Premium.
