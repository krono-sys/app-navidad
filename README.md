# 🎄 Lista de Navidad

Listas de deseos de Navidad compartidas por grupos (familia, amigos, trabajo).

## Funciones
- **Grupos por invitación**: crea un grupo y comparte su enlace (`/invite/<código>`), con botones de compartir y WhatsApp. El creador puede renovar el enlace para invalidar el anterior.
- **Tu lista en cada grupo**: añade deseos con
  - buscador de Google integrado (Productos / Web / Imágenes): «Usar este» rellena nombre, enlace, precio y foto; «+ Referencia» lo guarda como referencia,
  - pegar el enlace de cualquier tienda (lee título, foto, precio y descripción de la página),
  - detalles libres (talla, color, alternativas…), precio, prioridad,
  - varias imágenes (subidas desde el móvil/PC o por URL) y varias referencias.
- **Listas públicas dentro del grupo**: cualquier miembro ve las listas de los demás.
- **«Lo regalo yo»**: un miembro reserva un regalo para que nadie lo repita. El dueño de la lista **no** lo ve.

## Puesta en marcha
```bash
npm install
cp .env.example .env.local   # opcional: claves del buscador
npm run dev                  # http://localhost:3000
```
Producción: `npm run build && npm start`. Datos en `data/navidad.db` (SQLite) e imágenes en `data/uploads/`. Haz copia de esa carpeta.

## Buscador de Google
Sin claves, la app funciona: el buscador ofrece abrir Google en otra pestaña y pegar el enlace.
Para resultados dentro de la app, configura **una** de estas opciones en `.env.local`:
- `SERPER_API_KEY` ([serper.dev](https://serper.dev)): resultados de Google, incluida Google Shopping con precios. Es la opción recomendada.
- `GOOGLE_API_KEY` + `GOOGLE_CSE_ID` (Google Programmable Search / Custom Search JSON API). Google ha anunciado el cierre de esta API para nuevos clientes, así que puede no estar disponible.

## Publicarla en internet (Railway)
1. Sube esta carpeta a un repositorio de GitHub.
2. En [railway.com](https://railway.com) → **New Project → Deploy from GitHub repo** → elige el repositorio. Detecta el `Dockerfile` solo.
3. En el servicio: **Settings → Volumes → Add Volume**, ruta de montaje `/data` (ahí se guardan la base de datos y las fotos).
4. **Settings → Networking → Generate Domain**. Ese es el enlace de la app (p. ej. `https://lista-navidad.up.railway.app`).
5. Opcional, en **Variables**: `SERPER_API_KEY` para el buscador integrado.

## Instalarla en el móvil
Comparte `https://TU-DOMINIO/instalar`. La página detecta el móvil y explica cómo instalarla:
- **iPhone**: Safari → Compartir → *Añadir a pantalla de inicio*.
- **Android**: botón *Instalar* o descargar el `.apk`.

### Generar el .apk de Android (gratis, ~5 min)
1. Ve a [pwabuilder.com](https://www.pwabuilder.com), pega `https://TU-DOMINIO` y pulsa **Start**.
2. **Package for stores → Android → Generate Package** (deja *Signing key: New*). Descarga el zip.
3. Del zip: copia el `.apk` a `public/descargas/lista-navidad.apk`, súbelo al repositorio y Railway lo publicará. La página `/instalar` mostrará el botón **Descargar la app (.apk)**.
4. Guarda el `signing.keystore` y la contraseña del zip (hacen falta para futuras versiones).
5. Para que se abra a pantalla completa sin barra del navegador, añade en Railway las variables `ANDROID_PACKAGE` y `ANDROID_SHA256` con los valores del archivo `assetlinks.json` del zip.

## Stack

Next.js 15 (App Router, Server Actions) · React 19 · Tailwind CSS 4 · SQLite (better-sqlite3) · sesiones con cookie httpOnly y contraseñas con bcrypt.

Despliegue: cualquier servidor con Node 20+ y disco persistente (VPS, Railway, Fly.io, Render con disco). No sirve en Vercel tal cual, porque SQLite y las imágenes necesitan disco.
