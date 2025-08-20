# Alcryptan — аналитический суперагент для крипто-сигналов
_Состояние на 2025-08-20_

## TL;DR (5 строк)
- Цель: быстрый прод «умного слоя» над сигналами: сбор → рынок (OHLCV) → фичи → БД → отчёты TG (free/premium).
- Инфра: n8n (regular), Caddy+TLS, Postgres, Telegram-бот.
- База: `signals_enriched` (фичи/ohlcv), свечи > 0 ок.
- Отчёт в TG доставляется (reports_send_now).
- Дальше: Cron, ingest TG-экспортов, free/premium-витрина, масштаб источников.

## Архитектура MVP
- Сервер 78.140.253.42, домен n8n.alcryptan.ru, Caddy TLS, Docker Compose.
- Воркфлоу enrich: Webhook → Code → HTTP(Binance klines) → Parse → Features → Postgres.
- reports_send_now: SQL → формат → Telegram.
- ingest_telegram_exports (план): Read file → Extract tickers → POST в /webhook/signals/enrich.

## Данные
signals_enriched: id, t0, symbol, exchange, features JSONB, ohlcv JSONB,
rsi14 NUMERIC, atrp14 NUMERIC, received_at, индекс (symbol,t0).

## MVP
- Приём сигналов, 120×1m OHLCV, RSI/ATR, запись в БД, отчёт в TG.
- Free: top-N сетапов; Premium: полный лист + доп.фичи.

## Таймлайн
D+0: стабилизация enrich/БД/отчёта, docs+handoff  
D+1: Cron (daily/часовой), free/premium разводка, UNIQUE(symbol,t0), мониторинг  
D+2-3: ingest из TG-экспортов + троттлинг Binance; фичи SMA/EMA/ATR-классы  
Неделя 2: топ-10 символов, формат отчётов, метрики конверсии, новые источники

## Риски
Rate limit Binance, дубли, сеть/DNS, валидность символов — меры: троттлинг/ретраи/UNIQUE/нормализация.
