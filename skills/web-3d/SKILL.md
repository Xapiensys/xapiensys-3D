---
name: web-3d
description: >-
  Proceso de Xapiensys para páginas web 3D, interactivas y cinematográficas (línea premium).
  Usar cuando el usuario diga "este proyecto va en 3D", "línea 3D", "usa web-3d", o pida un hero 3D,
  un modelo que gira con el scroll o el mouse, un hero cinematográfico con video, un "mundo" que se
  recorre con scroll, o una web "que no parezca genérica" con efectos 3D. Orquesta Higgsfield (assets),
  taste-skill (dirección de arte), scroll-world (mundos con scroll) y web-design-guidelines (revisión).
---

# web-3d — Línea 3D de Xapiensys

Esta skill es el **director** del proyecto. No reemplaza a las otras skills del plugin: decide
cuál usar, en qué orden y con qué límites. Objetivo de la línea: webs que se ven premium **y cargan
rápido en un celular de gama media**. Un 3D que tarda 8 s en cargar es peor que no tener 3D.

Skills del plugin que vas a llamar:

| Skill | Para qué |
|---|---|
| `design-taste-frontend` (taste-skill) | Dirección de arte: concepto, tipografía, paleta, composición. Siempre antes de escribir UI. |
| `scroll-world` | Solo para el Nivel 3 (mundo que se recorre con scroll). |
| `web-design-guidelines` | Revisión final de accesibilidad y UX del código. |
| MCP `higgsfield` | Generar imágenes, video y modelos 3D (GLB). |

---

## Paso 0 — Preparación (2 minutos, no te lo saltes)

1. Verifica que las herramientas MCP de Higgsfield estén disponibles (`generate_image`,
   `generate_video`, `generate_3d`, `balance`). Si no aparecen, dile al usuario que ejecute `/mcp`
   y se autentique en Higgsfield; no sigas con assets inventados.
2. Consulta `balance` y anótalo. Todo gasto de créditos se presupuesta antes (Paso 3).
3. Lee si existe `XAPIENSYS.md` o `ASSETS.md` en la raíz del proyecto (decisiones previas). Si no
   existen, los crearás.

## Paso 1 — Brief (pregunta solo lo que falta)

- Cliente y rubro, y la **idea del 3D**: ¿qué objeto o escena representa al negocio? (un producto,
  el local, un edificio, un símbolo de la marca). El 3D debe salir del negocio, no ser decoración.
- Marca: logo, colores (hex), tono. Si no hay, propón y que el usuario apruebe.
- Stack: por defecto **HTML/CSS/JS vanilla + Vite** o el que ya tenga el proyecto (Next.js, Astro).
  No cambies de framework por el 3D: Three.js funciona en todos.
- Presupuesto de créditos de Higgsfield que el cliente cubre.

## Paso 2 — Elige el nivel (díselo al usuario con su costo)

| Nivel | Qué ve el visitante | Técnica | Costo relativo |
|---|---|---|---|
| **1. Hero 3D interactivo** | Un modelo 3D real que gira con el scroll y sigue al mouse | GLB de Higgsfield + Three.js en tiempo real | Bajo (1–3 imágenes + 1 modelo 3D) |
| **2. Hero cinematográfico** | Video de calidad cine de fondo, o un video que avanza/retrocede con el scroll | Video de Higgsfield + `<video>` o scroll-scrub | Medio (imagen + 1–3 videos) |
| **3. Mundo con scroll** | Toda la página es un vuelo de cámara continuo entre escenas | skill `scroll-world` | Alto (N imágenes + 2N−1 videos, ×2 si hay versión móvil) |

Regla de recomendación:
- Producto físico protagonista (zapatilla, botella, gadget, joya) → **Nivel 1**.
- Experiencia, lugar o servicio (inmobiliaria, hotel, restaurante, evento) → **Nivel 2**.
- Marca que cuenta un recorrido (de la chacra a la taza, del plano al edificio) y cliente con
  presupuesto alto → **Nivel 3**.
- Se pueden combinar: Nivel 2 en el hero + un Nivel 1 pequeño en una sección de producto.
- Si el cliente es de servicios sin nada visual (contadores, abogados), dilo: el 3D no aporta y sube
  el costo. Ofrece la versión premium sin 3D (solo taste-skill).

## Paso 3 — Presupuesto de créditos ANTES de generar

Para cada generación planificada llama a la herramienta con `get_cost: true` (no genera nada) y suma.
Muestra al usuario: `créditos estimados = X (+15% por re-intentos) / saldo actual = Y`. Espera su OK.
Nunca pases `use_unlim` ni `use_free_gens` si el usuario no lo pidió.

## Paso 4 — Dirección de arte

Invoca `design-taste-frontend` (taste-skill) con el brief. De ahí salen: concepto, tipografía,
paleta y composición de secciones. El estilo visual de los assets 3D/video **debe** usar esa misma
paleta y luz: escribe un **preámbulo de estilo** (una frase fija) y repítelo idéntico en todos los
prompts de Higgsfield del proyecto. Guárdalo en `ASSETS.md`.

## Paso 5 — Generar assets con Higgsfield (MCP)

Usa `models_explore` para confirmar modelos, roles de `medias` y parámetros vigentes; los IDs cambian.
Defaults al momento de escribir esto (verifícalos):

### Nivel 1 — modelo 3D (GLB)
1. `generate_image` con el objeto **solo, centrado, fondo liso neutro, iluminación de estudio
   uniforme, sin texto, vista 3/4**. Es la imagen que se convierte en 3D: el modelo 3D solo reproduce
   lo que está en la imagen, así que corrige la imagen antes, no después.
2. Si se puede, genera 2–4 vistas del mismo objeto (frente, lado, atrás) → `multi_image_to_3d`
   (mejor geometría). Con una sola vista → `image_to_3d`.
3. `generate_3d` con el `job_id` de la imagen en `medias` (rol según `models_explore`). Pide textura
   PBR si el modelo lo ofrece.
4. Descarga el `.glb` a `public/assets/3d/<nombre>.glb`.

Para escenas 3D completas (un ambiente, varios objetos, ángulos de cámara de un mismo lugar) en vez
de un solo objeto, el MCP tiene el flujo `/3d-jutsu`: llama primero a `get_preset_instructions` con
`{ preset: "/3d-jutsu" }` y sigue esas instrucciones; el GLB final se baja con
`scene_builder_3d_get_glb`.

### Nivel 2 — video
- Fondo en loop: `generate_video` con imagen inicial (rol de start/first frame) y prompt de movimiento
  lento y continuo. Pide que el último frame sea parecido al primero para que el loop no salte.
- Scroll-scrub: un solo plano de cámara de 5–8 s (dolly-in, órbita lenta, grúa) sin cortes.
- Formato horizontal 16:9 para escritorio; si el presupuesto alcanza, una versión 9:16 para móvil
  (no recortes el horizontal si el sujeto queda fuera).

### Nivel 3 — mundo con scroll
Invoca `scroll-world` y sigue su proceso. Ojo: esa skill está escrita para el **CLI** `higgsfield`
y Monid. Si en la máquina solo está el MCP:
- stills → `generate_image` (mismo prompt y preámbulo de esa skill),
- clips → `generate_video` con roles de imagen inicial/final (confirma con `models_explore` que el
  modelo acepta ambos; sin imagen final no se pueden hacer los conectores),
- descarga cada resultado con su URL y continúa el pipeline de ffmpeg de la skill igual.

### Registro obligatorio
Anota en `ASSETS.md`: archivo, modelo, prompt, job_id, créditos gastados y fecha. Así se puede
regenerar o variar un asset meses después sin adivinar.

## Paso 6 — Optimizar (aquí se gana o se pierde el proyecto)

Presupuestos máximos por asset (el hero completo debe pesar < 5 MB en móvil):

| Asset | Escritorio | Móvil |
|---|---|---|
| GLB | ≤ 3 MB (ideal < 1.5 MB) | mismo archivo o versión `-m.glb` ≤ 1 MB |
| Video hero | ≤ 4 MB | ≤ 2 MB (720p) |
| Poster (imagen fallback) | ≤ 200 KB `.webp` | ≤ 120 KB |

Comandos:

```bash
# GLB: comprimir geometría (meshopt) y texturas a webp de máx 1024 px
npx @gltf-transform/cli optimize in.glb out.glb --compress meshopt --texture-compress webp --texture-size 1024

# Video para scroll-scrub: keyframe en cada frame para que el seek sea fluido, sin audio
ffmpeg -i in.mp4 -an -vf "scale=1920:-2,fps=30" -c:v libx264 -preset veryfast -crf 26 -g 1 -pix_fmt yuv420p -movflags +faststart hero.mp4
ffmpeg -i in.mp4 -an -vf "scale=720:-2,fps=30"  -c:v libx264 -preset veryfast -crf 28 -g 1 -pix_fmt yuv420p -movflags +faststart hero-m.mp4

# Video en loop normal (no scrub): keyframes normales, más liviano
ffmpeg -i in.mp4 -an -vf "scale=1920:-2" -c:v libx264 -preset veryfast -crf 25 -pix_fmt yuv420p -movflags +faststart loop.mp4

# Poster
ffmpeg -i in.mp4 -frames:v 1 -vf "scale=1600:-2" -c:v libwebp -quality 78 poster.webp
```

## Paso 7 — Construir

Parte de las plantillas probadas de `references/` y adáptalas al diseño de taste-skill:

- `references/hero-3d.html` — Nivel 1: Three.js, GLB que gira con el scroll y se inclina con el
  mouse, carga diferida, poster mientras carga, fallback si no hay WebGL, respeta
  `prefers-reduced-motion`, pausa el render cuando el hero no está en pantalla.
- `references/hero-video-scrub.html` — Nivel 2: video que avanza con el scroll, sin librerías,
  con versión móvil y poster.

Reglas que no se negocian:
1. **El texto principal (H1, propuesta de valor, CTA) es HTML real**, nunca parte del canvas o del
   video. SEO y accesibilidad dependen de eso.
2. El 3D/video **nunca bloquea** el primer render: poster primero, Three.js y el GLB se cargan
   después (import dinámico / `IntersectionObserver`).
3. `prefers-reduced-motion: reduce` → poster estático o modelo quieto.
4. Sin WebGL o en dispositivos muy lentos → poster. La página tiene que funcionar igual.
5. Fija versiones de librerías (no `@latest` en producción).

## Paso 8 — Revisar antes de entregar

1. Invoca `web-design-guidelines` sobre los archivos de la página.
2. Corre el inspector de taste-skill (sus scripts) para el pase visual.
3. Mide en móvil: Lighthouse (modo mobile) → objetivo **Performance ≥ 80, LCP < 2.5 s, CLS < 0.1**.
4. Checklist de `references/checklist.md`.
5. Actualiza `XAPIENSYS.md` del proyecto con: nivel elegido, preámbulo de estilo, decisiones y
   pendientes.

Al terminar, reporta al usuario: nivel entregado, peso total del hero (escritorio/móvil), puntaje
Lighthouse móvil y créditos gastados vs. presupuestados.
