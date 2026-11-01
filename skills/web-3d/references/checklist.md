# Checklist de entrega — Línea 3D Xapiensys

Marca cada punto antes de decir "listo". Si algo no cumple, arréglalo o avísalo al usuario.

## Contenido y SEO
- [ ] H1, propuesta de valor y CTA son texto HTML (no dentro del canvas ni del video).
- [ ] `<title>`, meta description y Open Graph (imagen = el poster) completos.
- [ ] Imágenes decorativas con `alt=""`; las que informan, con `alt` descriptivo.

## Rendimiento (medido en Lighthouse modo móvil)
- [ ] Performance ≥ 80, LCP < 2.5 s, CLS < 0.1.
- [ ] El poster carga primero (`fetchpriority="high"`); Three.js/video no bloquean el primer render.
- [ ] GLB ≤ 3 MB (móvil ≤ 1 MB), comprimido con gltf-transform (meshopt + texturas webp).
- [ ] Video hero ≤ 4 MB escritorio / ≤ 2 MB móvil, sin audio, `+faststart`.
- [ ] Versiones de librerías fijadas (nada de `@latest`).
- [ ] El render 3D se pausa fuera de pantalla.

## Accesibilidad y robustez
- [ ] `prefers-reduced-motion: reduce` → sin animación (poster o modelo quieto).
- [ ] Sin WebGL / JS desactivado → la página se ve completa con el poster.
- [ ] Probado en: Chrome escritorio, Chrome Android (gama media o emulación con CPU 4x más lenta), Safari iOS.
- [ ] Contraste de texto sobre video/3D suficiente (usa degradado oscuro si hace falta).
- [ ] `web-design-guidelines` ejecutado sin hallazgos graves pendientes.

## Registro
- [ ] `ASSETS.md` con prompt, modelo, job_id y créditos de cada asset.
- [ ] `XAPIENSYS.md` con nivel, preámbulo de estilo, decisiones y pendientes.
- [ ] Reporte al usuario: nivel, peso del hero, puntaje Lighthouse móvil, créditos gastados vs. presupuesto.
