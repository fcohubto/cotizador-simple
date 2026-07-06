# Cotizador Simple — Continuidad de Proyecto

**Repo:** github.com/fcohubto/cotizador-simple
**Live:** https://fcohubto.github.io/cotizador-simple/
**Origen:** DM de validación en Instagram — cotizador gratis a cambio de feedback real de uso.

## Estado actual (2026-07-06)

MVP funcional, con identidad visual propia (Material Design 3), accesible, instalable como app desde el teléfono. Dos rondas de feedback de Francisco ya resueltas y desplegadas: (1) unidad de medida en ingredientes/recetas, (2) rediseño de navegación con Home dashboard, fix de zoom en mobile y jerarquía visual. Commit `a61784a` en `master`, pendiente que Francisco lo vuelva a probar en su teléfono.

## Completado

- **Unidad de medida en Ingrediente** — campo `unidad` (Unidad/kg/g/L/ml/Docena). El "valor unitario" se reinterpreta como "precio por esa unidad" (label dinámico). En Recetas, cada fila muestra la unidad junto a la cantidad (ej. `2 kg`). Sin conversión automática entre unidades — decisión explícita de Francisco para mantener el alcance simple. Migración automática para ingredientes ya guardados.
- **Fix de auto-zoom en mobile** — causa raíz: `font-size` de inputs/select por debajo de 16px dispara zoom automático de iOS/Android al enfocar un campo, lo que rompía el layout junto a los elementos `position: fixed`. Fix: inputs/select a 16px (`--text-md`), sin desactivar el zoom del usuario (decisión explícita de no romper accesibilidad WCAG 1.4.4).
- **Home (Inicio)** — nueva vista en el nav bar (ahora 4 tabs): resumen de conteos (Productos/Recetas/Ingredientes) en card con números protagonistas en ámbar, y "Últimos productos". Estado vacío total muestra guía de 3 pasos (Ingrediente → Receta → Producto) con jerarquía tipográfica real (número grande alineado arriba del texto) en vez de un sistema de onboarding separado — decisión explícita de Francisco de no sobre-construir para este alcance.
- **Botón de agregar movido al header** — de FAB flotante (`position: fixed`) a botón inline en cada vista (Productos/Recetas/Ingredientes), reduce superficie del bug de zoom. Inicio no tiene botón propio (ambigüedad detectada y corregida: no hay una única acción "agregar" obvia en un dashboard resumen).
- **Íconos por contenido** — sistema de emoji por palabra clave (antes solo en Ingredientes) extendido a Productos/Recetas (torta → 🎂, pan → 🍞, etc.), con fallback genérico.
- **Safe-area para PWA instalada** — `viewport-fit=cover` + `env(safe-area-inset-bottom)` en nav bar y padding del contenido, para no quedar pegado al borde en iPhone con gesture bar.
- **Modelo de datos** — 3 entidades: Ingrediente (catálogo, valor unitario compartido), Receta (reusable, referencia ingredientes), Producto (receta + valor adicional variable → precio final). Migración automática de datos del modelo viejo (plano) sin pérdida.
- **UI** — Material Design 3 Expressive, seed ámbar propio, shape scale, tipografía Roboto + Material Symbols, text fields filled, badges tonales.
- **Accesibilidad** — mínimo 14px en toda UI de pantalla, botones de lista navegables por teclado, landmark `<main>`, heading semántico, contraste AA/AAA verificado.
- **App instalable** — `manifest.json` + `icon.svg`, meta tags iOS/Android para "Agregar a pantalla de inicio".
- **Deploy** — commit `a61784a` en `master`, GitHub Pages reconstruido automáticamente.

## Pendiente

1. Feedback de Francisco tras probar esta versión (Home + fixes de zoom/jerarquía) en su teléfono real — bloqueante para el resto de los pasos.
2. Reestructurar a estándar de proyecto OLIVA: separar `/css` y `/js` (hoy todo vive en `index.html`), sumar `README.md`.
3. Definir con la usuaria real el resto del flujo pendiente (el valor adicional varía, no requiere default — ya confirmado).
4. Sin backend — persistencia solo en `localStorage` del navegador. Si la usuaria cambia de teléfono o borra datos del sitio, pierde todo. No es un problema mientras sea validación, pero es una limitación a tener presente si el uso se vuelve regular.

## Decisiones estructurales tomadas

- **Material Design 3 en vez de Carbon dark** — Carbon es el sistema propietario de OLIVA para herramientas internas; este producto es de consumo externo validado con una usuaria real, así que Material Design es la dirección correcta para este caso específico (no aplica a otros productos de Olivacraft). Confirmado de nuevo al revisar el wireframe de Organizapp: se mantiene la identidad visual actual (dark + ámbar), no se adopta el tema claro/morado del wireframe.
- **3 entidades separadas, no 1** — la usuaria necesita que actualizar el precio de un ingrediente se refleje en cascada en todas las recetas/productos que lo usan. Un modelo plano (ingrediente = texto por producto) no lo permite.
- **Sin monetización todavía** — objetivo actual es validar uso real, no vender. No priorizar features de cobro/paywall sin instrucción directa de Francisco. Se evaluó y descartó (por ahora) agregar un concepto de "venta/pedido" que apareció en un wireframe de referencia — fuera de alcance mientras el objetivo sea validación.
- **Sin onboarding separado** — en vez de un tutorial/wizard, la app resuelve la guía de flujo con estados vacíos contextuales justo en el punto de fricción (ej. "necesitas un ingrediente antes de crear una receta" + atajo directo), incluyendo ahora el estado vacío de Home. Decisión explícita de Francisco de no sobre-construir para el tamaño actual del proyecto.
