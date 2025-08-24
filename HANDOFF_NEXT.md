
[2025-08-24] Next
- Убрать хардкод chat_id → $env FREE_CHAT_ID/PREMIUM_CHAT_ID в контейнере.
- Дедуп по (symbol,t0); сортировать score desc, t0 desc.
- Метрики: winrate/avg PnL/score в тексте.
- Формат HTML: группировка по символам, цитаты, ссылки.
- Перенести крон в n8n Schedule Trigger (08:00 SGT), удалить system cron.
- Respond: вернуть {ok, rows, mode, topN}.
- Ретраи TG (429/5xx), лог ошибок в agent_activity.
