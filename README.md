# DataCafé|Américas

App instalable (PWA) para comprar café en 13 orígenes de América: calculadoras con moneda local, mercado, calendario de cosechas, catación, noticias y clima por país. Un producto de Sancocho Glorioso.

Publicada en `https://savilanbr-byte.github.io/datacafe_americas/` (GitHub Pages, rama `main`, carpeta raíz).

## Archivos

- `index.html`: toda la app (HTML, CSS y JavaScript en un solo archivo, sin compilación).
- `sw.js`: service worker; guarda la app para usarla sin conexión. Usa cachés `datacafe-americas-*`.
- `manifest.webmanifest` e `icons/`: instalación en el teléfono.
- `data/mercado.json`, `data/noticias.json`, `data/clima.json`, `data/cosechas.json`: los datos que muestra la app. Los actualiza una tarea programada; la app guarda la última copia para cuando no hay señal.

## Reglas

- Todo lo que la app guarda en el teléfono usa el prefijo `mca_`, porque comparte el dominio `savilanbr-byte.github.io` con DataCafé|Compras y DataCafé|Laboratorio.
- Si cambias `index.html` o `sw.js`, sube `VERSION` en `sw.js` (por ejemplo, `datacafe-americas-v7`) para que los teléfonos reciban la versión nueva.
- La tarea de datos solo debe sobrescribir `data/*.json`, nunca el código.

## Diseño adaptable

Un solo `index.html` para celular y computador:

- Menos de 1024 px de ancho (celular, tableta): formato de app, con menú inferior y menú lateral que se abre con ☰.
- Desde 1024 px (computador): formato web, con barra superior, menú lateral fijo (secciones, QR para compartir y contacto) y contenido en columnas. Los estilos están en el bloque `@media (min-width:1024px)` al final del CSS.

## Calculadora

El factor de rendimiento se expresa en kg de pergamino seco por saco de café verde, y el tamaño del saco lo elige el usuario (60, 69, 70 kg u otro). Con saco S y factor F:

- Kg café verde = kg pergamino × S / F
- Rendimiento (%) = 100 × S / F
- Precio café verde por kg = precio pergamino por kg × F / S
