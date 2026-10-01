# Glofy · Talent Profiles

Landing interna para mostrar perfiles a clientes (slideshow + navegación por categorías, EN/ES con EN por defecto) y un admin para cargarlos.

## Archivos

| Archivo | Uso |
|---|---|
| `index.html` | Landing. Lee `profiles.json` y las fotos de `photos/`. |
| `admin.html` | Alta, edición, baja y orden de perfiles; categorías; textos del encabezado. Publica en el repo vía GitHub API. |
| `profiles.json` | Fuente única de datos (lo escribe el admin). |
| `photos/` | Fotos subidas desde el admin (JPEG optimizado, máx. 1100 px). |

## Puesta en marcha

1. Crear el repo `comms-glofy/talent-profiles` y subir `index.html`, `admin.html`, `profiles.json` y este README.
2. Settings → Pages → Deploy from branch → `main` / root.
3. Crear un token fine-grained: Repository access solo este repo, permiso **Contents: Read and write**.
4. Abrir `…/talent-profiles/admin.html` → pestaña **Conexión** → pegar el token → Guardar y conectar.

Si el repo tiene otro nombre, se cambia en la pestaña Conexión.

## Regla de nombres

Solo **nombre + inicial del apellido** (ej. "Juan Cruz D."). El admin no tiene campo de apellido: la inicial acepta una sola letra y descarta el resto.

## Flujo del admin

Los cambios quedan en borrador hasta tocar **Publicar cambios**: ahí se suben las fotos nuevas, se guarda `profiles.json` y se borran las fotos reemplazadas o de perfiles eliminados. La landing se actualiza en ~1 minuto (rebuild de GitHub Pages).

## Links útiles para comercial

- `?lang=es` abre en español
- `?view=browse` abre la vista por categorías
- `?cat=sdr` filtra una categoría (ids: `sdr`, `marketing-growth`, `sales-executive`, `account-manager`, `automation`)
- `?cat=sdr&p=santiago-h` abre directo un perfil

Atajos en el slideshow: ← → para navegar, espacio para autoplay, `F` para modo presentación (pantalla completa).
