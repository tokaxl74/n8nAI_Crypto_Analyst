# Alcryptan — Docs & Handoff

## Где что лежит
- Док-папка: `/opt/alcryptan_docs/docs`
- Сырые файлы: `/opt/alcryptan_docs/raw`
- Логи агента: `/opt/alcryptan_docs/AGENT_LOG.txt`

## Handoff
- Текущий: `/opt/alcryptan_docs/docs/SESSION_HANDOFF.md`
- Следующему агенту: `/opt/alcryptan_docs/docs/HANDOFF_NEXT.md`

## Команды (памятка)
### Просмотр доков
ls -1 /opt/alcryptan_docs/docs

### Логи
tail -n 30 /opt/alcryptan_docs/AGENT_LOG.txt
docker compose -f /opt/alcryptan-n8n/docker-compose.yml exec -T postgres \
  psql -U n8n -d n8n -c \
"SELECT id,actor,message,at FROM agent_activity ORDER BY at DESC LIMIT 10;"

### Документы handoff
cat /opt/alcryptan_docs/docs/SESSION_HANDOFF.md
cat /opt/alcryptan_docs/docs/HANDOFF_NEXT.md

### GitHub sync
# (один раз) добавить remote:
# git -C /opt/alcryptan_docs remote add origin git@github.com:USER/alcryptan-docs.git
git -C /opt/alcryptan_docs add .
git -C /opt/alcryptan_docs commit -m "docs: update handoff"
git -C /opt/alcryptan_docs push

## Правила
- Каждый шаг фиксируй через `agent-log`.
- В конце сессии запускай `agent-stop "где остановился / что дальше"`.
- Не переписывай чужие доки — дополняй.
- Все изменения дублируем на сервере и в GitHub.

## Главные документы
- [PROJECT_OVERVIEW.md](docs/PROJECT_OVERVIEW.md)
