# SESSION_HANDOFF

## Текущее состояние
- n8n в regular; конвейер enrich пишет в Postgres.
- Telegram отчёт `reports_send_now` работает.

## Что дальше (черновик)
- [ ] Cron на daily snapshot (и часовой — по желанию)
- [ ] Развести free/premium вывод (2 Chat ID)
- [ ] Ingest из Telegram-экспортов
- [ ] Мини-monitor workflow + health-пинг
