# DataCafé|Américas

App instalable (PWA) para comprar café en 13 orígenes de América: calculadoras con moneda local, mercado, calendario de cosechas, catación, noticias y clima por país. Un producto de Sancocho Glorioso.

Publicada en `https://savilanbr-byte.github.io/datacafe_americas/` (GitHub Pages, rama `main`, carpeta raíz).

## Vistas (zoom por país)

La app tiene una vista general, **Américas**, y vistas por país. El nombre de la vista es el selector: se toca "Américas" en *DataCafé|Américas* (o se usa "Vista" en el menú lateral) y el nombre cambia, por ejemplo a *DataCafé|Colombia*.

- **Américas**: los 13 países a nivel de país (lo de `data/*.json`).
- **Colombia** (fase 1, 26 oct. 2026): lo mismo que DataCafé|Colombia, por departamento: calculadora en pesos por carga con TRM oficial, precio FNC, diferencial implícito, históricos de 6 meses, "En vivo" (TradingView), 18 regiones con mitaca, clima por departamento con IDEAM y noticias filtradas por departamento.
- **CostaRica** (27 oct. 2026): las 7 regiones oficiales del ICAFE (reforma de 2025: Valle Central, Valle Occidental, Los Santos, Turrialba, Pérez Zeledón, Coto Brus y Zona Norte); calculadora de la cereza al oro (cajuelas → fanegas → kg de cereza → kg de oro → quintales → sacos, con precios en colones y FOB); tipo de cambio de compra del BCCR; liquidación del ICAFE y "diferencial implícito ICAFE"; clasificación por altura (SHB, GHB, MHB); clima con avisos del IMN y noticias por región. Bolsa, robusta y fondos se toman de la vista Américas.
- **Honduras, Guatemala y ElSalvador** (27 oct. 2026): misma estructura que Costa Rica, con las unidades de compra de cada país.
  - Honduras: 6 regiones del IHCAFE; calculadora de quintales de uva → pergamino mojado → pergamino seco (precio clave) → oro, en lempiras con el tipo de cambio de referencia del BCH; grados SHG, HG y CS.
  - Guatemala: 8 regiones de Anacafé; quintales de cereza → pergamino (precio clave) → oro, en quetzales con el tipo de cambio del Banguat; grados SHB, HB, SH, EP y P.
  - El Salvador: 6 cordilleras; quintales de uva (precio clave) → pergamino → oro, en dólares; grados SHG, HG y CS.
  - Diferencial implícito: precio promedio de exportación registrado (IHCAFE, Anacafé, ISC) menos el promedio de la bolsa de NY (OIC, 2.ª y 3.ª posición) en los mismos meses.
- **Brasil** (27 oct. 2026): 11 regiones (8 de arábica y 3 de conilon, cosecha por región según la Conab); calculadora por saca de 60 kg de café beneficiado (bica corrida) en reales, con pestañas Arábica (contra KC) y Conilon (contra Londres), aprovechamiento en la preparación y costos hasta FOB; indicadores CEPEA/ESALQ de arábica y robusta con diferencial implícito diario; Clasificación Oficial Brasileña (bebida, tipo y criba); producción por región (Conab) y exportaciones (Cecafé).
- **Perú** (27 oct. 2026): 10 regiones; calculadora por quintal de 46 kg de pergamino en soles, con descuento de humedad sobre 12 % y rendimiento exportable; tipo de cambio interbancario del BCRP; diferencial implícito con el precio promedio de exportación del semestre frente a NY; exportación por región.
- Enlace directo a una vista: `?pais=colombia`, `?pais=costarica`, `?pais=honduras`, `?pais=guatemala`, `?pais=elsalvador`, `?pais=brasil` o `?pais=peru`. La app recuerda la última vista usada.
- Cada vista guarda su propia calculadora (`mca_mc_calc_*` en Américas, `mca_co_calc_*` en Colombia, `mca_cr_calc_*`, `mca_hn_calc_*`, `mca_gt_calc_*`, `mca_sv_calc_*`, `mca_br_calc_*` y `mca_pe_calc_*` en los demás). La catación es una sola para todas las vistas.

### Datos de la vista Colombia

Por ahora la vista Colombia lee los datos que ya publica DataCafé|Colombia en el mismo dominio (`/datacafe_compras/data/*.json`, incluido `historico.json`), así no hubo que cambiar su tarea programada. Si esa ruta no responde, usa la copia de respaldo `data/co/*.json` (foto del 26 oct. 2026; no se actualiza sola) y nunca reemplaza datos más nuevos por la copia.

### Lo que se trae de DataCafé|Colombia

Las dos apps comparten el almacenamiento del navegador (mismo dominio). En cada apertura, Américas trae de DataCafé|Colombia lo que falte: sesiones de catación nuevas, el diferencial que escribiste y tus descriptores propios. La primera vez en la vista Colombia, la calculadora parte de los valores que tenías allá. No se borra nada de DataCafé|Colombia, y una sesión traída que borres aquí no vuelve.

## Archivos

- `index.html`: toda la app (HTML, CSS y JavaScript en un solo archivo, sin compilación).
- `sw.js`: service worker; guarda la app para usarla sin conexión. Usa cachés `datacafe-americas-*`.
- `manifest.webmanifest` e `icons/`: instalación en el teléfono.
- `data/mercado.json`, `data/noticias.json`, `data/clima.json`, `data/cosechas.json`: los datos de la vista Américas. Los actualiza una tarea programada; la app guarda la última copia para cuando no hay señal.
- `data/co/`: copia de respaldo de los datos de Colombia (ver arriba).
- `data/cr/`, `data/hn/`, `data/gt/`, `data/sv/`, `data/br/`, `data/pe/`: datos de Costa Rica, Honduras, Guatemala, El Salvador, Brasil y Perú (`mercado`, `noticias`, `clima`, `cosechas`, `historico`). Cada tarea programada debe tocar solo la carpeta de su país. En `mercado.json`, el bloque `ref` lleva el precio de exportación, el peso del quintal y los meses de NY con que se calcula el diferencial implícito; en `clima.json`, cada punto lleva sus coordenadas.

## Reglas

- Todo lo que la app guarda en el teléfono usa el prefijo `mca_`, porque comparte el dominio `savilanbr-byte.github.io` con DataCafé|Compras y DataCafé|Laboratorio. Las únicas lecturas sin prefijo son las de DataCafé|Colombia (`mc_sessions`, `mc_calc_*`, `mc_ajustes`, `mc_desc_propios`), y solo se leen.
- Si cambias `index.html` o `sw.js`, sube `VERSION` en `sw.js` (por ejemplo, `datacafe-americas-v12`) para que los teléfonos reciban la versión nueva.
- La tarea de datos solo debe sobrescribir `data/*.json`, nunca el código.

## Diseño adaptable

Un solo `index.html` para celular y computador:

- Menos de 1024 px de ancho (celular, tableta): formato de app, con menú inferior y menú lateral que se abre con ☰.
- Desde 1024 px (computador): formato web, con barra superior, menú lateral fijo (vista, secciones, QR para compartir y contacto) y contenido en columnas. Los estilos están en el bloque `@media (min-width:1024px)`.

## Calculadora

El factor de rendimiento se expresa en kg de pergamino seco por saco de café verde, y el tamaño del saco lo elige el usuario (60, 69, 70 kg u otro). Con saco S y factor F:

- Kg café verde = kg pergamino × S / F
- Rendimiento (%) = 100 × S / F
- Precio café verde por kg = precio pergamino por kg × F / S

En la vista Colombia la calculadora queda en pesos, por carga de 125 kg, saco de 70 kg y TRM oficial.

En la vista Costa Rica la calculadora encadena la cereza y el oro. Con C cajuelas, c cajuelas por fanega (20), K kg de cereza por fanega (≈ 253), O kg de oro por fanega (≈ 46) y P precio por fanega:

- Fanegas = C / c; kg de cereza = fanegas × K; kg de oro = fanegas × O; quintales = kg de oro / 46
- Precio por cajuela = P / c; por kg de cereza = P / K; por kg de oro = P / O; por quintal oro = P / O × 46
- Costo del café oro (USD/lb) = (P / O) / 2,2046 / tipo de cambio; FOB = costo + beneficio, exportación y margen

En Honduras, Guatemala y El Salvador la calculadora es una cadena de etapas medidas en quintales (Q, peso del quintal en kg). Con r₁, r₂… el rendimiento de cada paso (%), f = producto de los rendimientos hasta cada etapa y P el precio por quintal en la etapa de compra k:

- Quintales en la etapa i = quintales de uva o cereza × fᵢ; kg de oro = quintales de oro × Q
- Precio por quintal en la etapa i = P × fₖ / fᵢ; precio por kg de oro = precio por quintal de oro / Q
- Factor uva/oro = 1 / f(oro); costo USD/lb = precio por kg de oro / 2,2046 / tipo de cambio; FOB = costo + beneficio, exportación y margen

En Brasil, con N sacas de 60 kg, aprovechamiento A (%), precio P por saca y tipo de cambio T: costo USD/lb = P / (A/100) / T / (60 × 2,2046); FOB = costo + costos hasta FOB. Diferencial implícito CEPEA del arábica = indicador / dólar / 132,28 lb × 100 − KC (¢/lb); del conilon = indicador / dólar / 60 × 1.000 − Londres (USD/t).

En Perú, con humedad H (%) se corrige el peso: si H > 12, kg al 12 % = kg × (100 − H) / 88. Kg exportables = kg al 12 % × rendimiento exportable; costo por kg exportable = precio por kg de pergamino / (factor de humedad × rendimiento).
