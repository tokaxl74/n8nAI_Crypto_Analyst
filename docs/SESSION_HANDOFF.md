# SESSION_HANDOFF

## Текущее состояние (сжато)
- n8n (regular) за Caddy: https://n8n.alcryptan.ru
- /webhook/signals/enrich → Binance 1m x120 → RSI14/ATR% → Postgres `signals_enriched`
- /webhook/signals/export — JSON из БД
- reports_send_now — SQL → Telegram Bot API (ручной запуск работал)
- БД: индексы; вьюхи `v_recent_signals`, `v_memory_pack`; таблицы `intel_notes`, `agent_activity`
- Док-хранилище: /opt/alcryptan_docs ↔ GitHub tokaxl74/n8nAI_Crypto_Analyst (main)

## Последние шаги (прокси / внешние API)
- Из-за ограничений исходящего трафика нужен прокси для Telegram/OpenAI.
- Поднят tinyproxy на отдельном VPS (90.156.220.12:8888). Проверка извне ОК (HEAD https://www.google.com через прокси → 200).
- Внутри контейнера n8n `curl` отсутствует → прямой тест из контейнера не прошёл; в ноде HTTP Request к Telegram без прокси было `EAI_AGAIN`/`404`.
- Начата интеграция прокси:
  1) Включать Proxy в HTTP Request-нодах (host/port/учётка).
  2) Или добавить глобально в docker-compose для сервиса n8n: HTTP_PROXY / HTTPS_PROXY и NO_PROXY для внутр. сервисов (postgres, redis, localhost).
- OpenAI от n8n ещё не проверяли; план — тест нодой HTTP Request + Proxy (list models), затем вынести ключи в Credentials.

## Где остановились
- tinyproxy работает, внешние тесты через него успешны.
- В reports_send_now осталось включить Proxy в ноде Telegram и повторить отправку.
- (Опционально) добавить переменные прокси в compose и перезапустить n8n.
- «Память» (intel_notes + v_memory_pack) готова; в отчёте нужно подмешать краткий контекст.

## Примечание по секретам
- Данные прокси и ключи храним в n8n Credentials/ENV, в репозиторий не коммитим.

### 2025-08-21T08:42:33+00:00

продолжить: включить Proxy в Telegram-узле/или ENV; протестировать OpenAI; включить cron; развести free/premium; ingest экспорта
