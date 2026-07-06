# Cotizador Simple — Continuidad de Proyecto

**Repo:** github.com/fcohubto/cotizador-simple
**Live:** https://fcohubto.github.io/cotizador-simple/
**Origen:** DM de validación en Instagram — cotizador gratis a cambio de feedback real de uso.

## Estado actual (2026-07-06)

MVP funcional, con identidad visual propia (Material Design 3), accesible, instalable como app desde el teléfono. Francisco probó la versión desplegada y encontró un gap real: faltaba unidad de medida en ingredientes/recetas. Ya está resuelto en local, **pendiente de commit + deploy**.

## Completado

- **Unidad de medida en Ingrediente** — nuevo campo `unidad` (Unidad/kg/g/L/ml/Docena). El "valor unitario" se reinterpreta como "precio por esa unidad" (label dinámico en el form). En Recetas, cada fila de ingrediente muestra la unidad al lado del campo cantidad (ej. `2 kg`), sin ambigüedad sobre qué representa el número. Sin conversión automática entre unidades — decisión explícita de Francisco para mantener el alcance simple (cantidad se ingresa en la misma unidad que el precio del ingrediente). Migración automática: ingredientes ya guardados sin `unidad` quedan con `unidad: 'unidad'` por defecto. Probado en navegador (servidor local): migración, edición de unidad, cálculo de subtotal, todo correcto.
- **Modelo de datos** — 3 entidades: Ingrediente (catálogo, valor unitario compartido), Receta (reusable, referencia ingredientes), Producto (receta + valor adicional variable → precio final). Migración automática de datos del modelo viejo (plano) sin pérdida.
- **Navegación** — mobile list-first: Productos (home) / Recetas / Ingredientes, bottom nav bar.
- **UI** — Material Design 3 Expressive (ref. `m3.material.io/blog/whats-new-at-io26`): color roles con seed ámbar propio (no el azul/morado default de Google), shape scale, tipografía Roboto + Material Symbols, FAB, text fields filled, badges tonales, emojis por ingrediente (identidad + reconocimiento rápido).
- **Accesibilidad** — mínimo 14px en toda UI de pantalla (regla propia, labels usan mayúsculas/tracking en vez de tamaño para jerarquía), botones de lista navegables por teclado (antes eran `<div onclick>`), landmark `<main>`, heading semántico, contraste AA/AAA verificado en todos los pares de color.
- **App instalable** — `manifest.json` + `icon.svg` (emoji 🧮 sobre fondo ámbar), meta tags iOS/Android para "Agregar a pantalla de inicio".
- **Deploy** — commit `c77df18` en `master`, GitHub Pages (legacy build, deploy automático desde `master`/root) reconstruido y verificado (200 en index/manifest/icon).

## Pendiente

1. **Commit + deploy** del cambio de unidad de medida — bloqueante, ahora mismo solo existe en local (`index.html` modificado, sin commitear).
2. Seguir con feedback de Francisco tras probar la versión con unidad de medida en su teléfono.
3. Reestructurar a estándar de proyecto OLIVA: separar `/css` y `/js` (hoy todo vive en `index.html`), sumar `README.md`.
4. Definir con la usuaria real el resto del flujo pendiente (ver punto 6 nunca cerrado: ¿el valor adicional varía o es casi fijo? — confirmado que varía, no requiere default).
5. Sin backend — persistencia solo en `localStorage` del navegador. Si la usuaria cambia de teléfono o borra datos del sitio, pierde todo. No es un problema mientras sea validación, pero es una limitación a tener presente si el uso se vuelve regular.

## Decisiones estructurales tomadas

- **Material Design 3 en vez de Carbon dark** — Carbon es el sistema propietario de OLIVA para herramientas internas; este producto es de consumo externo validado con una usuaria real, así que Material Design es la dirección correcta para este caso específico (no aplica a otros productos de Olivacraft).
- **3 entidades separadas, no 1** — la usuaria necesita que actualizar el precio de un ingrediente se refleje en cascada en todas las recetas/productos que lo usan. Un modelo plano (ingrediente = texto por producto) no lo permite.
- **Sin monetización todavía** — objetivo actual es validar uso real, no vender. No priorizar features de cobro/paywall sin instrucción directa de Francisco.
