# xapiensys-3d — Línea 3D de Xapiensys

Plugin de Claude Code para hacer webs premium con 3D: héroes con modelos 3D que giran con el scroll
y el mouse, video cinematográfico que avanza con el scroll, y "mundos" que se recorren con scroll.

## Qué trae

| Componente | Qué hace |
|---|---|
| `web-3d` (skill de Xapiensys) | El director: elige el nivel (1, 2 o 3), presupuesta créditos, genera assets, construye, optimiza y revisa. Incluye plantillas probadas en `skills/web-3d/references/`. |
| `taste-skill` | Dirección de arte para que la web no se vea genérica. |
| `scroll-world` | Mundo completo que se recorre con scroll (nivel 3, el más caro). |
| `web-design-guidelines` | Revisión de accesibilidad y UX antes de entregar. |
| MCP `higgsfield` | Genera imágenes, video y modelos 3D (`.glb`). Se conecta solo al instalar el plugin. |

## Instalar (una sola vez por computadora)

Dentro de Claude Code:

```
/plugin marketplace add TU-USUARIO/xapiensys-3d
/plugin install xapiensys-3d@xapiensys
```

Si todavía no está en GitHub, puedes instalarlo desde la carpeta local:

```
/plugin marketplace add C:\ruta\a\xapiensys-3d
/plugin install xapiensys-3d@xapiensys
```

Después reinicia Claude Code, ejecuta `/mcp` y autentícate en Higgsfield (se abre el navegador, sin API keys).

> Si ya habías agregado Higgsfield antes con `claude mcp add`, quedará duplicado. Quita uno con
> `claude mcp remove higgsfield` o desactiva el del plugin; con uno basta.

## Usar en un proyecto de cliente

Abre Claude Code en la carpeta del proyecto y escribe algo como:

> Este cliente va con la línea 3D. Usa web-3d. Es una tienda de zapatillas, quiere que la zapatilla
> gire en el hero.

O llama a la skill directamente: `/xapiensys-3d:web-3d`.

Claude va a: pedirte lo que falte del brief → proponer el nivel con su costo → pedirte OK de los
créditos → dirección de arte → generar assets → construir → optimizar → revisar → reportarte peso,
puntaje Lighthouse móvil y créditos gastados.

## Los 3 niveles (para cotizar)

| Nivel | Ideal para | Costo en créditos |
|---|---|---|
| 1. Hero 3D interactivo | Productos físicos | Bajo |
| 2. Hero cinematográfico | Lugares, experiencias, servicios con algo visual | Medio |
| 3. Mundo con scroll | Marcas con presupuesto alto que cuentan un recorrido | Alto |

## Actualizar

Edita las skills en este repo, sube el cambio y en cada PC ejecuta
`/plugin marketplace update xapiensys`.

Versiones incluidas de las skills de terceros (para saber de dónde vienen al actualizar):

| Skill | Repo | Commit |
|---|---|---|
| taste-skill | github.com/Leonxlnx/taste-skill | 12af471 |
| web-design-guidelines | github.com/vercel-labs/agent-skills | 1a2c79d |
| scroll-world | github.com/oso95/scroll-world | 71cc36d |

## Licencias

Las tres skills de terceros son MIT; sus licencias están en `licenses/`. La skill `web-3d` y las
plantillas son de Xapiensys.

## Privacidad

El MCP de Higgsfield envía prompts e imágenes a Higgsfield para generar los assets. Política de
privacidad: en el pie de página de higgsfield.ai.
