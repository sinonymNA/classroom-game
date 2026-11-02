# Deployment guide

## Local classroom network (most predictable)

1. Install current Node.js LTS on the teacher computer.
2. Run `npm install`, then `npm start`.
3. Find the computer's LAN address with `ipconfig` (for example `192.168.1.25`).
4. Teacher: `http://localhost:8080/teacher.html`.
5. Students: `http://192.168.1.25:8080`.
6. Keep the terminal running. Confirm the school network permits device-to-device traffic and port 8080.

Set another port with `$env:PORT=3000; npm start`. Production should terminate TLS at the hosting platform so clients use HTTPS/WSS.

## Free managed possibilities

The server needs long-lived WebSockets and in-memory room affinity. Provider free tiers change frequently, so this repository does not claim a permanently free provider. At deployment time, check Render, Railway, Fly.io, Koyeb, or an education-provided Node host for current WebSocket support, sleep limits, and free quotas. PartyKit was not made a hard dependency because its service terms and free availability cannot be guaranteed, while the included standard Node server remains portable and self-hostable.

Generic deployment:

1. Upload the repository to the provider.
2. Build command: `npm install && npm run build`.
3. Start command: `npm start`.
4. Set `PORT` only if the provider does not inject it.
5. Verify `https://YOUR-HOST/teacher.html` and `wss://YOUR-HOST/ws`.

The same Node service hosts both frontend and WebSocket server, avoiding CORS and mixed-content problems. Static frontend hosting is optional: upload `dist/` to any HTTPS static host and pass the Node service URL to the standalone client as `?server=wss://HOST/ws`.

## Standalone LMS distribution

Run `npm run build` and upload `dist/suspect.html` to the LMS. Give students a link containing the URL-encoded server address, for example:

`suspect.html?server=wss%3A%2F%2Fsuspect-server.example%2Fws`

Some LMS systems sandbox downloaded HTML or rewrite query strings. If WebSockets fail, distribute the normal hosted student URL instead. Never use an insecure `ws://` server from an HTTPS page.

## Operations and troubleshooting

- **Room disappeared:** the server restarted; create a new room. This is explicit ephemeral behavior.
- **Students cannot connect:** confirm the exact host/port, firewall, network isolation, and WSS certificate.
- **Page works but socket fails:** HTTPS pages require `wss://`; inspect proxy WebSocket upgrade support.
- **Student duplicated:** use the same browser/profile so its reconnect token remains available.
- **Stalled game:** teacher can pause, end the round, remove a player, or return to lobby.
- **Before class:** run `npm test`, `npm run test:acceptance`, and test two real devices on the target network.

## Privacy and retention

Only nicknames, transient game actions, and reconnect tokens are held in server memory. No analytics, ads, emails, accounts, grades, or third-party assets are used. Restarting the process clears every room. The teacher can also end a round or replace the room from the dashboard.

