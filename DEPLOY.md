# STAY Ecuador — Guía de despliegue en Cloudflare Pages

Este es un sitio estático (HTML + CSS + JS, sin backend ni paso de build), así que se despliega directo en **Cloudflare Pages**, gratis, sin necesidad de GoHighLevel ni ningún otro CMS.

## Tu situación en GoDaddy (confirmado)

Revisamos tu panel y esto es lo que tienes hoy:

- **Dominio `stay-ecuador.com`** — activo, registrado en GoDaddy. Esto no cambia, se queda ahí.
- **"Páginas Web + Marketing Gratis"** — activo sobre ese dominio. Es el Website Builder propio de GoDaddy (editor visual). **No tiene File Manager ni FTP, así que no acepta subir estos archivos HTML/CSS.**
- **WordPress administrado** — solo en "prueba gratis", no contratado.

No hay ningún hosting en GoDaddy donde cargar esta carpeta directamente. La ruta más simple y sin costo extra: dejar el dominio registrado en GoDaddy pero apuntarlo a **Cloudflare Pages** (gratis) siguiendo los pasos de abajo — no necesitas contratar hosting de pago en GoDaddy ni tocar el registro del dominio en sí.

## Antes de publicar — checklist de contenido

Hay 2 cosas marcadas intencionalmente como pendientes en el sitio, porque no pude generarlas ni confirmarlas desde aquí:

1. **Más fotos de las propiedades.** Cada card ya usa la foto de portada real de cada listing de Airbnb (enlazada directo desde el CDN de Airbnb, `a0.muscache.com`). Si prefieres alojar tus propias copias en vez de depender del hotlink a Airbnb, descarga esas fotos y guárdalas en:
   - `img/properties/casa-pb-1.jpg` (y opcionalmente `-2`, `-3`… para más fotos)
   - `img/properties/1201-1.jpg`
   - `img/properties/1202-1.jpg`
   - Ya tienes fotos reales de Casa PB (29 fotos, resolución 1440px) del kit de marketing — son el mejor punto de partida para ampliar la galería de cada propiedad.
2. **Formulario de contacto.** El formulario de `contacto.html` no envía correos todavía — un sitio 100% estático no puede procesar formularios por sí solo. Opciones simples y gratuitas para conectar el envío sin backend propio:
   - **Cloudflare Pages Functions** (gratis, se queda dentro del mismo proyecto) — requiere un poco de código.
   - **Formspree** o **Web3Forms** (gratis hasta cierto volumen) — solo agregas el `action` del `<form>` apuntando a su endpoint.
   - Mientras tanto, WhatsApp y el correo directo funcionan sin configuración adicional.
3. **Foto de portada (hero) del inicio.** No pude extraer el archivo de tu foto de atardecer ni el logo grande que compartiste en el chat — cuando pegas una imagen directo en la conversación, no queda guardada como archivo al que yo pueda acceder. Por ahora usé un degradado de atardecer hecho en CSS (mismo tono cálido, sin depender de un archivo). Si me envías esa foto como **archivo adjunto** (botón de adjuntar, no pegar la imagen en el mensaje), la reemplazo por la real en `img/hero-sunset.jpg`.

## Opción 1 — Subida directa (recomendada, sin código)

1. Entra a **dash.cloudflare.com** con la cuenta donde ya está el dominio `stay-ecuador.com`.
2. En el menú lateral, ve a **Workers y Pages** → pestaña **Pages** → **Crear una aplicación** → **Cargar activos** (Direct Upload).
3. Ponle un nombre al proyecto, por ejemplo `stay-ecuador`.
4. Arrastra la carpeta `stay-ecuador-site` completa (o comprime su contenido en `.zip` y súbelo) — no incluyas este archivo `DEPLOY.md` si prefieres, es opcional.
5. Cloudflare la publica en una URL temporal tipo `stay-ecuador.pages.dev`. Verifica ahí que todo se vea bien antes de conectar el dominio.

## Opción 2 — Conectado a GitHub (para poder actualizar con git push)

1. Sube la carpeta `stay-ecuador-site` a un repositorio nuevo en GitHub.
2. En Cloudflare: **Workers y Pages** → **Pages** → **Crear una aplicación** → **Conectar a Git**.
3. Selecciona el repositorio. En **Build settings**, deja el comando de build vacío y el directorio de salida como `/` (raíz) — no hay paso de build porque es HTML plano.
4. Cada vez que hagas `git push`, Cloudflare vuelve a publicar automáticamente.

## Conectar tu dominio stay-ecuador.com (hoy vive en GoDaddy, no en Cloudflare)

Tu dominio todavía usa los servidores DNS de GoDaddy, así que hay un paso adicional antes de lo que dice la Opción 1/2: mover la gestión DNS a Cloudflare. El registro del dominio se queda en GoDaddy (sigues pagando la renovación ahí, sin problema) — solo cambia quién resuelve el DNS.

1. Crea una cuenta gratis en **dash.cloudflare.com** si no tienes una.
2. Ahí, **Agregar un dominio** → escribe `stay-ecuador.com` → elige el plan **Free**. Cloudflare escaneará tus registros DNS actuales y te dará **2 nameservers** (algo como `ana.ns.cloudflare.com` / `bob.ns.cloudflare.com`).
3. Entra a GoDaddy → **Dominios** → `stay-ecuador.com` → **DNS** (el botón que aparece junto a "Administrar" en tu panel) → busca la sección de **Nameservers** → cámbialos de "GoDaddy" a **"Personalizados"** y pega los 2 que te dio Cloudflare.
4. El cambio tarda entre unos minutos y ~24 horas en propagarse. Cloudflare te avisa por correo cuando el dominio queda activo en su cuenta.
5. Con el dominio ya en Cloudflare, sigue la **Opción 1** o **Opción 2** de arriba para publicar el sitio en Pages.
6. Luego, dentro del proyecto de Pages, ve a **Custom domains** → agrega `stay-ecuador.com` (y `www.stay-ecuador.com` si lo usas). Como el DNS ya está en Cloudflare, el registro `CNAME` hacia tu proyecto se configura automáticamente.
7. **Si usas ese dominio para correo (por ejemplo con Google Workspace o el correo de GoDaddy), copia tus registros `MX` actuales antes del paso 3** y vuelve a crearlos manualmente en Cloudflare DNS una vez migres — si no, el correo de ese dominio deja de funcionar. Si no usas correo en `stay-ecuador.com`, ignora este punto.
8. El certificado SSL de Pages se emite automáticamente en unos minutos tras conectar el dominio.

## Después de publicar

- Verifica cada página y cada enlace (Alojamientos, Destinos, Property Manager, Bienes Raíces, WhatsApp, redes sociales).
- Activa **Cloudflare Web Analytics** (gratis, sin cookies) en el proyecto de Pages para medir visitas.
- El producto **"Páginas Web + Marketing Gratis"** de GoDaddy queda sin uso — puedes eliminarlo desde tu panel de GoDaddy (es gratis, no genera costo dejarlo, pero mejor borrarlo para no confundirte más adelante).
- Cancela o desconecta el sub-embudo de GoHighLevel una vez confirmes que el nuevo sitio funciona en el dominio — así evitas seguir pagando por él si ya no lo necesitas para nada más.
- Pide a un abogado que revise `terminos.html` y `privacidad.html` — están marcadas como plantillas de referencia, no como texto legal final.

## Estructura del sitio

```
stay-ecuador-site/
├── index.html              Inicio
├── alojamientos.html       Listado de las 3 propiedades activas
├── destinos.html           Punta Blanca y Playas Villamil
├── property-manager.html   Página de captación de propietarios
├── bienes-raices.html      Inversión / eXp Realty
├── nosotros.html           Historia y cifras
├── faq.html                Preguntas frecuentes
├── contacto.html           Formulario y datos de contacto
├── terminos.html           Plantilla legal (revisar con abogado)
├── privacidad.html         Plantilla legal (revisar con abogado)
├── css/style.css           Todo el diseño y la paleta de marca
├── js/main.js              Menú móvil + acordeón de FAQ
└── img/
    ├── logo.png                    Logo aprobado, versión con fondo blanco cuadrado (para usar sobre blanco)
    ├── logo-transparent.png        Logo aprobado, versión sin fondo — la que usa el sitio en el header/footer navy
    └── properties/                 Coloca aquí las fotos reales de cada propiedad
```
