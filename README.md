# 🤖 RAIZEN BOT — Bot de WhatsApp con Baileys (200 comandos)

Bot de WhatsApp listo para usar, hecho con [Baileys](https://github.com/WhiskeySockets/Baileys).
Trae **200 comandos** organizados en 10 categorías, base de datos JSON integrada, economía, juegos,
herramientas de grupo y más. Sin APIs de pago ni claves: todo funciona de fábrica.

## 🚀 Instalación

Requisitos: **Node.js 20 o superior** y `git`.

```bash
git clone https://github.com/TU_USUARIO/raizen-bot.git
cd raizen-bot
npm install
```

1. Abre `config.js` y pon **tu número** en `owners` (con código de país, sin `+`).
2. Inicia el bot:

```bash
npm start
```

3. Vincula WhatsApp:
   - **QR:** escanéalo desde *WhatsApp > Dispositivos vinculados > Vincular un dispositivo*.
   - **Código (sin QR):** pon tu número en `pairingNumber` dentro de `config.js` y el bot imprimirá un
     código de 8 caracteres para *Vincular con el número de teléfono*.

La sesión queda guardada en `session/`. Si cierras sesión desde el celular, borra esa carpeta y vuelve a vincular.

> Para dejarlo 24/7 usa `pm2` (`npm i -g pm2 && pm2 start index.js --name raizen-bot`) o un VPS/panel con Node.
> `index.js` ya reinicia el bot solo si se cae o si usas `.restart`.

## 📂 Estructura

```
index.js          Lanzador con auto-reinicio
main.js           Conexión a WhatsApp (Baileys)
config.js         ⚙️ Toda la configuración
lib/handler.js    Procesa mensajes, permisos, antilink, bienvenidas
lib/loader.js     Carga automática de comandos
lib/db.js         Base de datos JSON (database.json)
cmds/*.js         Un archivo por categoría
```

## ➕ Cómo agregar un comando

Abre el archivo de la categoría (o crea uno nuevo en `cmds/`) y añade un objeto al array:

```js
{
  name: 'saludo', aliases: ['hola'], desc: 'Saluda al usuario', usage: '[nombre]',
  // group: true,  admin: true,  botAdmin: true,  owner: true   ← permisos opcionales
  async run(c) {
    c.reply(`¡Hola @${c.sender.split('@')[0]}!`, [c.sender])
  },
}
```

El loader lo detecta solo al reiniciar. Contexto `c`: `reply`, `send`, `react`, `args`, `text`, `sender`,
`from`, `isGroup`, `isAdmin`, `isOwner`, `getTarget()`, `getMedia()`, `user`, `group`, `db`, `sock`, `m`…

## 📋 Comandos (200)

Usa `.menu` en WhatsApp para verlos todos, o `.menu <categoría>` para ver descripciones.

| Categoría | Cant. | Ejemplos |
|---|---|---|
| **Info** | 14 | menu, ping, runtime, owner, infobot, stats, buscarcmd, reportar, sugerir |
| **Grupo** | 30 | kick, add, promote, demote, tagall, hidetag, link, abrir, cerrar, antilink, welcome, warn, encuesta, topactivos |
| **Utilidades** | 27 | calc, qr, password, b64enc, md5, sha256, uuid, hora, recordar, morse, caesar, color, edad |
| **Matemáticas** | 30 | suma, potencia, raiz, factorial, fibonacci, primo, mcd, porcentaje, c2f, km2mi, romano, imc |
| **Texto** | 20 | negrita, cursiva, mono, burbuja, voltear, zalgo, mayus, leet, mock, palindromo |
| **Diversión** | 30 | bola8, chiste, ppt, slot, ruleta, adivina, ship, horoscopo, trivia, ahorcado, abrazo |
| **Economía** | 22 | balance, daily, work, mine, crime, fish, hunt, deposit, transfer, rob, apostar, tienda, comprar |
| **Owner** | 13 | broadcast, ban, unban, setprefix, setnombre, restart, salir, unirse, modo, backup |
| **Internet** | 12 | clima, wiki, definir, pais, cripto, divisa, ip, github, acortar, perro, gato, npm |
| **Media** | 2 | sticker, toimg |

Además, la mayoría de comandos tienen alias (por ejemplo `.help`, `.ayuda` y `.comandos` abren el menú).

## ⚠️ Notas

- **Stickers animados** (`.sticker` con video) necesitan `ffmpeg` instalado en el servidor.
- `.toimg` convierte solo stickers estáticos.
- Los comandos de **Internet** usan APIs públicas gratuitas; si alguna cae o limita peticiones, ese comando avisa del error.
- `.recordar`, la trivia y los juegos viven en memoria: se pierden si el bot se reinicia.
- La economía, niveles, advertencias y ajustes de grupo se guardan en `database.json`.
- Baileys no es un producto oficial de WhatsApp. Úsalo con responsabilidad y evita el spam masivo
  (puede provocar el bloqueo del número). `.broadcast` ya espera 1.5 s entre grupos.

## 🔧 Prefijos y modo

- Prefijos por defecto: `.` `!` `/` `#` (cámbialos con `.setprefix` o en `config.js`).
- `.modo private` deja el bot solo para los dueños; `.modo public` lo abre a todos.
