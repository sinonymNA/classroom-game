# SUSPECT: Classroom Edition

**Trust nobody. Finish the mission.** SUSPECT is a lightweight, server-authoritative classroom social-deduction game for 4–12 players. It uses only Node.js, WebSockets, HTML, CSS, JavaScript, and Canvas.

## Start locally

```powershell
npm install
npm start
```

Open `http://localhost:8080/teacher.html` on the teacher device. Students on that computer use `http://localhost:8080`; devices on the same network use `http://TEACHER-LAN-IP:8080`. Permit Node through the local firewall if prompted.

## Commands

```powershell
npm run dev
npm test
npm run test:acceptance
npm run build
npm start
```

## Play

The teacher creates a room and keeps the private host token stored in that browser. Students enter the six-character code and a nickname, select a color, and ready up. Movement uses WASD/arrows; E interacts; R reports; M opens the map. Meetings intentionally have no chat so discussion happens in the classroom.

The server owns roles, answers, movement, collisions, kills, cooldowns, votes, sabotage, and wins. Reconnect tokens restore disconnected players. Rooms are memory-only and disappear on server restart; no names, accounts, analytics, grades, or tracking data are persisted.

## Deploy

This repository includes `render.yaml` for a free Render Web Service. Use Render's New Blueprint flow and select this repository.

## Question banks

Upload JSON in Mission Control. Examples are in `examples/`. Supported shapes are `multiple-choice`, `systems`, `chronology`, `matchmaker`, and `classification`; every item needs a unique `id`. Answers stay server-side.

## Standalone client

Run `npm run build`, then distribute `dist/suspect.html`. Configure it with `suspect.html?server=wss://YOUR-SERVER.example/ws`.

## Architecture

- `server/game.js`: authoritative room state machine
- `server/index.js`: HTTP/WebSocket transport, rate and payload limits
- `public/`: student and teacher clients
- `scripts/build.js`: deployable and standalone packaging
- `tests/`: rules, network, and full-match acceptance tests

## Known limitations

Rooms are ephemeral, the free host may cold-start, and school filters may require the final hostname to be allowlisted. Touch interaction buttons exist, but there is no virtual movement joystick. CSV import, audio effects, hats, and persistent match reports are not included.
