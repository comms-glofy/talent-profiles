# Glofy · Talent Profiles

Landing interna para mostrar perfiles a clientes (slideshow + navegación por categorías, EN/ES con EN por defecto) y un admin para que el equipo cargue y edite perfiles sin tocar código.

## Archivos

| Archivo | Dónde va | Uso |
|---|---|---|
| `index.html` | Repo | Landing. Lee `profiles.json` y las fotos de `photos/`. |
| `admin.html` | Repo | Admin: el equipo entra con su nombre + una clave compartida. |
| `profiles.json` | Repo | Fuente única de datos (la escribe el admin). |
| `worker.js` | Cloudflare | Guarda el token de GitHub y publica en nombre del equipo. **No se sube al repo.** |

## Cómo funciona

El equipo nunca ve ni maneja un token. Entra al admin con su nombre y la clave de acceso; al publicar, el Worker guarda todo en el repo en un solo commit (`Update profiles — Nombre`), así queda registrado quién hizo cada cambio. La landing se actualiza para todos en ~1 minuto.

## Puesta en marcha (una sola vez)

### 1. Repo y GitHub Pages
1. Crear `comms-glofy/talent-profiles` y subir `index.html`, `admin.html`, `profiles.json` y este README.
2. Settings → Pages → Deploy from branch → `main` / root.

### 2. Worker en Cloudflare
1. Cloudflare → **Workers & Pages** → **Create** → **Create Worker** → nombre `talent-profiles-api` → **Deploy**.
2. **Edit code** → borrar el contenido → pegar `worker.js` → **Deploy**.
3. En el Worker → **Settings → Variables and Secrets** → **Add**, tipo **Secret**:
   - `GITHUB_TOKEN`: el token fine-grained (Contents: Read and write sobre `talent-profiles`).
   - `ADMIN_PASSWORD`: la clave que va a usar el equipo.
4. Copiar la URL del Worker (ej. `https://talent-profiles-api.xxxx.workers.dev`).

### 3. Conectar el admin
En `admin.html`, al inicio del `<script>`, reemplazar:

```js
const API_URL = 'https://talent-profiles-api.REEMPLAZAR.workers.dev';
```

por la URL del paso anterior y subir el archivo al repo.

### 4. Compartir con el equipo
- Admin: `https://comms-glofy.github.io/talent-profiles/admin.html`
- Clave: la de `ADMIN_PASSWORD`

## Mantenimiento

- **Cambiar la clave** (alguien se va, la clave circuló): Cloudflare → Worker → Settings → editar `ADMIN_PASSWORD`. Quien esté logueado tiene que volver a ingresar.
- **Token vencido**: el admin muestra "El token de GitHub del servicio venció…". Generar uno nuevo y reemplazar `GITHUB_TOKEN` en el Worker. Nadie más tiene que hacer nada.
- **Repo con otro nombre**: agregar en el Worker las variables de texto `GITHUB_REPO` (y `GITHUB_OWNER` / `GITHUB_BRANCH` si hiciera falta).
- **Admin en otro dominio**: agregar la variable `ALLOWED_ORIGINS` con el dominio (por defecto acepta `https://comms-glofy.github.io`).

## Regla de nombres

Solo **nombre + inicial del apellido** (ej. "Juan Cruz D."). El admin no tiene campo de apellido y el Worker además recorta cualquier inicial a una sola letra antes de guardar.

## Links útiles para comercial

- `?lang=es` abre en español
- `?cat=sdr` filtra una categoría (ids: `sdr`, `marketing-growth`, `sales-executive`, `account-manager`, `automation`)
- `?cat=sdr&p=santiago-h` abre directo un perfil

Atajos: ← → para navegar, espacio para autoplay, `/` para buscar, `F` para modo presentación.

## Video de nivel de inglés

En el admin, cada perfil tiene un campo opcional "Video de YouTube". Acepta links `youtube.com/watch?v=…`, `youtu.be/…` o Shorts, y respeta el minuto de inicio (`?t=…`). El video tiene que estar como Público u Oculto (no Privado). Si el campo queda vacío, el botón de video no aparece en la landing.
