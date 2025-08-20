# HANDOFF_NEXT (промпт следующему агенту)

Контекст:
- Документация в /opt/alcryptan_docs
- БД n8n (Postgres) — таблица signals_enriched

Цели:
1) Автоматизировать ежедневные отчёты (free/premium).
2) Настроить ingestion из Telegram экспортов.
3) Расширить фичи (RSI/ATR → больше метрик).

Ограничения:
- Не ломать существующий пайплайн.
- Все шаги логировать через agent-log и обновлять SESSION_HANDOFF.md
