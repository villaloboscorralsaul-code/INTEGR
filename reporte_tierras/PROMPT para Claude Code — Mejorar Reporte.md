# PROMPT para Claude Code — Mejorar el Reporte de Tierras Físicas con fotos

**Cómo usarlo:** Abre Claude Code dentro de la carpeta del proyecto donde vive `reporte_tierras/index.html`. Asegúrate de que la carpeta de fotos de la inspección (las imágenes de la revisión del casino) esté dentro o accesible desde ahí. Luego copia **todo lo que está debajo de la línea** y pégalo como un solo mensaje.

> Notas que ya descubrí por ti y que el prompt aprovecha:
> - Las 35 imágenes son **HEIC de iPhone con extensión `.PNG` falsa** → hay que convertirlas o el navegador no las muestra.
> - Los **nombres de archivo mienten** (`ETIQUETADO.PNG` es un transformador, `REFRIGERACIONCUARTO.PNG` es un minisplit) → hay que mirar cada foto, no confiar en el nombre.
> - Las fotos traen **valores reales** que tus tablas tienen en blanco (0.42 Ω, 0.00 Ω, 29.5 A, ~125 V, sello CFE C25V936651, transformador 225 kVA).

---

Actúa como **ingeniero eléctrico senior + desarrollador front-end**. Vas a transformar un reporte técnico en plantilla (campos vacíos) en un **informe profesional terminado, con registro fotográfico completo y listo para imprimir a PDF**. Trabaja sobre el proyecto que contiene `reporte_tierras/index.html`.

## Contexto del proyecto
- El reporte es un **Estudio de Tierras Físicas** bajo **NOM-022-STPS-2015**, **NOM-001-SEDE-2012**, **NMX-J-549-ANCE-2005** e **IEEE Std 80-2013**, elaborado por **INTEGR (Ingeniería e Integración de Sistemas Eléctricos)**.
- Sitio: **Av. Las Torres 2111, Col. Lote Bravo, Local Subancla 4, C.P. 32575, Chihuahua, Chih.** Folio **INTG-TF-001**.
- Personal: Ing. Sixto Gallegos (responsable), Saúl Villalobos (elaboró), Ojilbert Rodríguez (técnico de campo).
- El HTML ya tiene 12 secciones (01 Datos generales … 12 Conclusiones) más un **Registro Fotográfico** con marcadores vacíos ("Toca para agregar foto"). La estructura es buena; falta llenarla, insertar fotos y pulir el diseño.

## ⚠️ Tres reglas críticas (no las ignores)
1. **Las imágenes son HEIC disfrazadas de `.PNG`.** Antes de usarlas, conviértelas a un formato web (JPG o WebP) con un script. NO confíes en la extensión: verifica el tipo real (`file <archivo>` o cabecera `ftypheic`). Conserva los originales intactos; escribe las versiones convertidas en una carpeta nueva, p. ej. `reporte_tierras/img/`. Corrige la orientación según EXIF y comprime a un ancho máx. ~1600 px para que el HTML cargue ligero.
2. **Los nombres de archivo NO son confiables.** Debes **abrir y mirar cada imagen** para identificar qué muestra (tablero, varilla de tierra, telurómetro con lectura, medidor CFE, no conformidad, etc.). Clasifica por contenido visual, no por el nombre.
3. **No inventes datos de medición.** Yo elegí el modo "fotos + datos marcados": cuando una foto muestre un valor legible (Ω, A, V), colócalo en la tabla correspondiente **resaltado y etiquetado como `valor sugerido — confirmar`** (por ejemplo, fondo amarillo y una nota). Nunca presentes un número como definitivo si proviene solo de la lectura de una foto. Los campos sin evidencia se quedan como marcadores editables, claramente resaltados como pendientes.

## Tareas (en orden)

### 1. Inventario y conversión de imágenes
- Recorre la carpeta de fotos, detecta los HEIC, conviértelos a JPG/WebP en `reporte_tierras/img/` con nombres limpios y consistentes (sin espacios), corrigiendo orientación EXIF.
- Genera un archivo `reporte_tierras/manifiesto_fotos.md` (o `.json`) con una tabla: **archivo original → archivo convertido → qué muestra (descripción por visión) → valor medido visible (si hay) → sección del reporte donde irá**.

### 2. Clasificación por visión
Mira cada foto y agrúpalas por tipo. Categorías esperadas (úsalas como guía, ajusta según lo que veas):
- **Medidor CFE / acometida**: medidor Elster, sello de seguridad CFE, número de serie/sello.
- **Tableros**: tableros principales y derivados, interior, etiquetado, puertas.
- **Sistema de tierra**: varillas, barras/busbar de tierra, conexiones, conductor de cobre.
- **Mediciones con instrumento (lectura visible)**: telurómetro de gancho (resistencia de tierra), multímetro de gancho (corriente/voltaje).
- **No conformidades**: cableado expuesto, conducto abierto/quemado, multicontactos, conexiones inseguras.
- **Equipos/instalación**: transformador, minisplit, cuarto eléctrico.

### 3. Pistas ya confirmadas por análisis previo (úsalas y verifica)
Estas lecturas y elementos ya los identifiqué revisando varias fotos. **Confírmalas contra las imágenes** y márcalas como "valor sugerido — confirmar" donde apliquen:
- **Resistencia de electrodo (telurómetro de gancho ETCR2100+):** una lectura de **0.42 Ω** (pinza sobre barra de tierra) y otra de **0.00 Ω** (pinza sobre cable de cobre). → Sección 08.
- **Corriente (Fluke 376 FC):** ~**29.5 A** sobre conductores en tablero. → dato de operación / observaciones.
- **Voltaje (Fluke 376 FC):** ~**125 V** con puntas. → dato de operación / observaciones.
- **Medidor CFE Elster A3RAL** con **sello de seguridad CFE núm. C25V936651** (clase 480 V, 3F 4H). → Sección 01 Datos generales (voltaje de suministro/medición).
- **Transformador seco Square D de 225 kVA, 480-220/127 V.** → Sección 01 (capacidad/voltaje a equipos del cliente).
- **Minisplit York** y cuarto eléctrico (contexto del sitio).
- **No conformidades visibles:** caja de conexiones con **cableado expuesto y empalmes con cinta**; **conducto/registro abierto con residuo quemado (carbonizado)**.

> Nota de coherencia: el reporte lista los instrumentos EM-01 (MC Miller), EM-02 (Fluke 1630) y EM-03 (Extech CR2100+), pero las fotos muestran un **Fluke 376 FC** y un **ETCR2100+**. Señala esta discrepancia en un comentario y propón reconciliar la lista de equipos con lo realmente usado en campo (sin borrar nada: solo deja una nota visible para que el responsable lo valide).

### 4. Insertar el registro fotográfico
- Reemplaza los marcadores "Toca para agregar foto" por las fotos reales, con `<img>` de rutas **relativas** a `img/`, `loading="lazy"`, `alt` descriptivo y **pie de foto** (qué muestra + ubicación + lectura si aplica).
- Llena los 6 espacios guía del reporte (vista general, posición de electrodos, equipo en operación, lectura del instrumento ×2, condición del suelo) con las fotos más representativas, y agrega una **galería ampliada** para el resto, organizada por categoría (Tableros, Tierra, Mediciones, Equipos).
- Mantén el diseño en cuadrícula, responsivo, con buen espaciado.

### 5. Sección nueva: **No Conformidades / Hallazgos**
Crea una sección nueva (p. ej. después de la 11 Observaciones) titulada **"HALLAZGOS Y NO CONFORMIDADES"** con una tabla y fotos. Para cada falla detectada incluye:
- **Foto** del hallazgo.
- **Descripción** técnica de la condición insegura.
- **Severidad** (Alta / Media / Baja) con color (rojo / ámbar / amarillo).
- **Norma incumplida** (NOM-001-SEDE-2012, NOM-022-STPS-2015, etc.).
- **Recomendación / acción correctiva**.
Como mínimo documenta: cableado expuesto en caja de conexiones, conducto abierto con evidencia de calentamiento/quemado, y multicontactos/conexiones improvisadas si aparecen en otras fotos. Usa "severidad sugerida — confirmar".

### 6. Llenado de tablas (modo marcado)
- En **Sección 07 (Resistividad – Wenner)** y **08 (Resistencia de electrodo)**, coloca los valores que provengan de fotos **resaltados** con la etiqueta `valor sugerido — confirmar` (estilo: fondo amarillo claro, ícono o nota al pie). Deja el resto de celdas como campos editables marcados como "pendiente".
- En **Sección 06 (Condiciones del sitio)**, deja fecha/hora/temperatura/humedad como campos editables resaltados (no los inventes); si alguna foto tiene fecha/hora EXIF, ofrécela como dato sugerido.
- En **Sección 01 (Datos generales)**, precarga como "sugerido — confirmar": voltaje de suministro (480 V según medidor CFE), capacidad de subestación/transformador (225 kVA), a partir de las fotos.

### 7. Diseño y "listo para imprimir a PDF"
- Pule el CSS: tipografía profesional, jerarquía clara, branding INTEGR, encabezado con folio en cada sección.
- Agrega **`@media print`**: saltos de página correctos (`page-break-inside: avoid` en tablas y tarjetas de foto), márgenes, encabezado/pie con folio y paginación, ocultar botones/controles al imprimir, imágenes que no se corten.
- Agrega un **botón "Imprimir / Guardar PDF"** que dispare `window.print()` (oculto en la impresión).
- Verifica que el reporte se vea bien tanto en pantalla como en vista previa de impresión (tamaño carta).

### 8. Calidad y entrega
- Rutas relativas siempre (que funcione abriendo el `index.html` directo).
- No borres contenido existente; si reemplazas, conserva el texto técnico y el folio.
- Todo en **español**.
- Al terminar, entrega un **resumen de cambios** y una **lista de los campos que requieren mi confirmación** (los "valor sugerido"), para que yo los valide rápido.

## Checklist de verificación final (hazlo antes de decir que terminaste)
- [ ] Todas las imágenes convertidas se muestran en el navegador (ninguna rota).
- [ ] Cada foto está en la sección correcta según su **contenido**, no su nombre.
- [ ] Ningún valor inventado: todo dato de foto está marcado "confirmar".
- [ ] Sección de No Conformidades creada con severidad y norma.
- [ ] Vista previa de impresión (PDF carta) sin cortes feos ni botones visibles.
- [ ] Discrepancia de instrumentos (Fluke 376/ETCR2100+ vs. lista EM-0x) señalada.
- [ ] Resumen de cambios + lista de campos a confirmar entregados.
