# Uttara Jyotish — статус задач

Живая страница статуса задач: **https://andreyastrolog-gif.github.io/status/**

- `index.html` — вся страница (HTML/CSS/JS без сборки). Интерфейс на русском, время — Тбилиси (UTC+4).
- `status.json` — источник данных. Страница перечитывает его каждые 30 секунд без перезагрузки:
  сначала через GitHub API (`/repos/andreyastrolog-gif/status/contents/status.json?ref=main`, минуя кэш CDN Pages),
  при ошибке или лимите запросов — `status.json?t=<timestamp>`.
- Страница закрыта от индексации (`noindex,nofollow`). Не добавляйте сюда личные данные — репозиторий публичный.

## Формат status.json

```json
{
  "updated": "2026-09-25T17:36:00+04:00",
  "tasks": [
    {
      "id": "status-page",
      "title": "Страница статуса задач",
      "description": "Короткое описание",
      "status": "running",            // running | queued | waiting_user | done
      "progress": null,               // 0–100 или null (для running с started+eta считается по времени)
      "started": "2026-09-25T17:36:00+04:00",
      "eta": "2026-09-25T17:55:00+04:00",
      "finished": null,
      "link": null                    // ссылка на результат (для done)
    }
  ]
}
```

## Обновление — helper `/workspace/tools/status_update.py`

```bash
status_update.py list
status_update.py add  --id foo --title "Новая задача" --desc "..." --status running --eta +30m
status_update.py set  foo --progress 60 --eta 18:40        # любые поля; "null" очищает поле
status_update.py done foo --link https://example.com
status_update.py remove foo
status_update.py push -m "message"                           # закоммитить и запушить
```

`add`, `set`, `done`, `remove` сами делают commit + push (отключить: `--no-push`); поле `updated` обновляется автоматически.
Время: `now`, `HH:MM` (сегодня, Тбилиси), `+25m`/`+2h` или полный ISO.
