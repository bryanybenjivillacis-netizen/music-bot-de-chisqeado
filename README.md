# Bot de música

Bot de Discord de música. Los comandos principales son **slash** (`/`). Solo el comando
compartido para varios bots a la vez usa prefijo de texto: `#`.

## Comandos

### Slash (`/`)
- `/play <link o nombre>` — todos. Reproduce YouTube/SoundCloud, playlists (hasta 200), o busca por nombre.
- `/queue` — todos. Muestra qué está sonando y la cola.
- `/join` — todos. Solo conecta el bot a tu canal, sin reproducir nada.
- `/farm [link o nombre]` — dueño del servidor, owner del bot, o persona asignada con `/assign`. Reproduce en bucle infinito para quedarte fijo en el canal.
- `/lock` — dueño del servidor, owner del bot, o persona asignada. Fija el bot al canal actual; si lo desconectan/mueven, expulsa a quien fue (si el audit log lo confirma) y se reconecta solo.
- `/assign [@persona]` — solo owner del bot. Da o quita acceso a `/lock` y `/farm`. Se pierde si el bot se reinicia.
- `/help` — lista todo esto dentro de Discord.

### Prefijo de texto (`#`)
- `#join [link o nombre]` / `#join @bot1 @bot2` — para que varios bots (con el mismo código) se unan y empiecen a farmear al mismo tiempo. Si mencionas bots específicos, solo esos responden.
- `#sts [@bot1 @bot2]` — cada bot dice a qué servicio de Railway/repo de GitHub está conectado, su canal y su modo.

Usa `/help` dentro de Discord para verlo todo con ejemplos.

## Variables de entorno (ver `_env.example`)
- `DISCORD_TOKEN` — token del bot.
- `BOT_OWNER_ID` — tu ID de Discord.
- `REPORT_OWNER_ID` — a quién se le manda el DM de reportes (por defecto, el mismo `BOT_OWNER_ID`).
- `FARM_DEFAULT_URL` — link que se usa si no pasas ninguno en farm/join.
- `YTDLP_COOKIES` — cookies de YouTube (opcional, mejora la tasa de éxito en algunos videos). **Nunca subas cookies reales a GitHub**, solo ponlas directo en Railway.
- `ICON_PREVIOUS_ID`, `ICON_PAUSE_ID`, `ICON_PLAY_ID`, `ICON_NEXT_ID`, `ICON_STOP_ID` — IDs de los emoji de aplicación para los botones del reproductor (opcional; sin esto, los botones solo muestran texto).

## Despliegue en Railway
1. Sube estos archivos a un repo en GitHub.
2. En Railway, crea un proyecto conectado a ese repo.
3. Configura las variables de entorno de arriba.
4. En el Developer Portal de Discord → tu aplicación → **Bot** → activa **MESSAGE CONTENT INTENT** (lo sigue necesitando para `#join`/`#sts`).
5. Invita el bot con los scopes **bot** y **applications.commands** (este último es obligatorio para que aparezcan los comandos `/`).
6. Dale permisos de **Connect**, **Speak**, **Move Members** (para el kick por audit log) y **View Audit Log**.
7. Revisa el log de build: debe instalar `ffmpeg` sin errores (viene de `nixpacks.toml`).
8. Los slash commands se sincronizan solos al arrancar el bot (`on_ready`); la primera vez puede tardar hasta ~1 hora en aparecer en todos los servidores.

## Iconos de los botones (opcional)
Los 5 PNG (`previous`, `pause`, `play`, `next`, `stop`) se suben en el Developer Portal → **Emoji**.
Copia el ID de cada uno y ponlo en las variables `ICON_*_ID` de arriba.
