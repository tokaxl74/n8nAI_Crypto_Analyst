# AI Crypto Analyst — Handoff Prompt (Next Agent)

Role: Tech Lead/DevOps. Одна команда → подтверждение → следующий шаг. Без воды.
Mission v0.1.x: держать E2E отчёты (free/premium), подготовка метрик и автоторговли.

Infra/Context:
- Docs: /opt/alcryptan_docs (git tokaxl74/n8nAI_Crypto_Analyst)
- n8n: https://n8n.alcryptan.ru ; WF: reports_build_and_send
- ENV (target): TELEGRAM_BOT_TOKEN, FREE_CHAT_ID, PREMIUM_CHAT_ID, FREE_TOP_N=5
- Proxy: NO_PROXY включает api.telegram.org (sendMessage OK)
- DB: signals_enriched, intel_notes, agent_activity (индексы есть)
- Logs: agent-log "<actor>" "<msg>" → AGENT_LOG.txt + public.agent_activity(actor,message,at)

State (2025-08-24):
- Webhook prod: POST /webhook/reports/send — OK
- Flow: Webhook → params → build_sql(7d, join intel_notes) → select_data → build_message(HTML) → send_to_tg → Respond
- Free/Premium: free=LIMIT topN, premium=full; TG отправка OK
- Cron: system 00:00 UTC (08:00 SGT) → /usr/local/bin/reports-daily-utc8 (free+premium)
- Exec data saving: включено; см. execution_entity/execution_data
- Временно: chat_id в send_to_tg зашит 1215909770 (заменить на ENV)

Plan (priority):
 D0:
  1) Убрать хардкод chat_id → $env.FREE_CHAT_ID/$env.PREMIUM_CHAT_ID в контейнере и ноде send_to_tg; тест free/premium
  2) Дедуп по (symbol,t0), сортировка t0 DESC (позже score DESC, t0 DESC)
  3) Respond body: вернуть { ok, rows, mode, topN }
 D1–D2:
  4) Метрики (7d): winrate, avg_pnl, простой score; вывести в сообщении
  5) HTML формат: заголовок с датой UTC, группировка по символам, note_snip в <blockquote>
  6) Надёжность: ретраи 429/5xx, таймауты, лог ошибок в agent_activity
  7) Перенести cron в n8n Schedule Trigger (08:00 Asia/Singapore), удалить system cron
 Later (v0.2+): онлайн-скоринг, фильтры, бэктесты, sandbox/paper trading, риск-лимиты, kill-switch

Checklists/Commands:
- Webhook free:  curl -sS -H "Content-Type: application/json" -d '{"mode":"free","topN":5}' https://n8n.alcryptan.ru/webhook/reports/send
- Webhook prem:  curl -sS -H "Content-Type: application/json" -d '{"mode":"premium"}'       https://n8n.alcryptan.ru/webhook/reports/send
- n8n logs:      docker logs --tail=200 alcryptan-n8n-n8n-1
- psql tables:   docker exec -e PGPASSWORD="<pgpass>" -i alcryptan-n8n-postgres-1 psql -U n8n -d n8n -c '\dt'

Ritual:
- После шага: agent-log "<actor>" "<что сделал>"
- Финал:       agent-stop "где остановились/что дальше"
