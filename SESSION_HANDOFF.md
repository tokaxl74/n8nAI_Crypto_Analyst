
[2025-08-24] E2E: reports_build_and_send
- Webhook (prod): POST /webhook/reports/send
- Params: mode=free|premium; topN (free)
- Flow: Webhook → params → build_sql → select_data → build_message → send_to_tg → Respond
- SQL: 7d окно; join intel_notes; free=LIMIT topN; premium=full
- TG: OK (chat_id=1215909770 временно; TODO: env)
- Cron: system 00:00 UTC (08:00 SGT) → /usr/local/bin/reports-daily-utc8
- Логи: agent_activity + AGENT_LOG.txt
