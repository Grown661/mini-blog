# mini-blog

Minimalistische, selbst-gehostete Blog-Engine: Posts in **Markdown**, serverseitig
gerendert mit einem **eigenen Markdown-Renderer** — komplett ohne npm-Dependencies.
Inklusive Admin-Bereich (Login per signiertem Session-Cookie) und generiertem RSS-Feed.

## Problem

Für einen kleinen Blog braucht es kein WordPress und keinen Static-Site-Build-Prozess.
Diese Engine ist eine einzige Datei, startet in einer Sekunde und speichert Posts als JSON —
schreiben, speichern, fertig.

## Features

- Eigener Mini-Markdown-Renderer (Überschriften, Bold/Italic, Inline-Code, Codeblöcke, Links, Listen)
- Admin-Bereich: Posts anlegen, bearbeiten, löschen — Login mit Passwort aus `ADMIN_PASSWORD`
- HMAC-signierte Session-Cookies (`node:crypto`, timing-safe Vergleiche)
- Generierter **RSS-2.0-Feed** unter `/feed.xml`
- XSS-sicher: alles wird escaped, nur `http(s)`-/relative Links erlaubt
- Lesefreundliches Serifen-Theme, keine Client-JS-Abhängigkeit

## Stack

- Node.js (nur Builtins: `node:http`, `node:crypto`, `node:fs/promises`) — **kein `npm install`**
- JSON-File-Store unter `data/`

## Setup & Start

```bash
ADMIN_PASSWORD=geheim node server.js   # Standard-Port 8221
PORT=9000 ADMIN_PASSWORD=geheim node server.js
```

`ADMIN_PASSWORD` ist **Pflicht** — ohne gesetzte Variable bricht der Server den Start
mit einer Fehlermeldung ab (kein Default-Login). Für lokales Testen darf das Passwort
kurz sein, es muss aber explizit gesetzt werden. Login-Fehlversuche sind pro IP auf
5 in 15 Minuten begrenzt (danach `429`).

- Blog: `http://localhost:8221/`
- Admin: `http://localhost:8221/admin`
- RSS: `http://localhost:8221/feed.xml`

## API / Routen

| Methode | Pfad | Beschreibung |
|---|---|---|
| `GET` | `/` | Post-Liste |
| `GET` | `/post/:slug` | Gerenderter Post |
| `GET` | `/feed.xml` | RSS-2.0-Feed |
| `GET` | `/admin` | Admin-Dashboard (oder Login) |
| `POST` | `/admin/login` | Login (`password`) |
| `POST` | `/admin/posts` | Post anlegen/aktualisieren |
| `POST` | `/admin/delete` | Post löschen |

## Screenshot

_(Screenshot folgt)_

## Lizenz

MIT
