# HANDOFF_NEXT — что сделать следующему агенту

## 1) Починить прогон WF `reports_build_and_send` end-to-end
- Убедиться, что цепочка связана по Main-портам:
  `Webhook → params → build_sql → select_data → build_message → send_to_tg → Respond to Webhook`.
- В `Webhook`: Respond = Using 'Respond to Webhook' Node.
- В `params` использовать универсальный парсер:
  ```js
  const src = items[0].json || {};
  const body = (src && typeof src.body === 'object' && src.body !== null) ? src.body : src;
  const mode = String((body.mode ?? 'premium')).toLowerCase();
  const topN = Number(body.topN ?? ($env.FREE_TOP_N ?? 5));
  return [{ json: { mode, topN } }];

