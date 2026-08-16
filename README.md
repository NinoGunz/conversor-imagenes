# Conversor de imágenes

Herramienta web para convertir, comprimir y redimensionar imágenes **sin subirlas a ningún servidor**.
Todo el procesamiento ocurre dentro del navegador usando la API de Canvas.

Funciona en tres modos: **convertir**, donde eliges formato y calidad; **comprimir**, donde indicas
un peso objetivo y la herramienta busca sola la mejor calidad que quepa dentro de él; y
**redimensionar**, que solo cambia el tamaño y conserva el formato de cada imagen.

Un solo archivo HTML, sin instalación ni dependencias.

## Sobre este proyecto

Lo hice para aprender y experimentar. Tengo pocos conocimientos de JavaScript, así que lo
construí con ayuda de Claude, en buena medida por curiosidad: quería ver hasta dónde podía
llegar iterando sobre la interfaz y la lógica sin que el código se rompiera. También me servía
de excusa para probar GitHub Pages, que siempre me había llamado la atención.

Soy consciente de que aquí no se siguen un montón de buenas prácticas. La más evidente es que
todo está en un mismo archivo, cuando lo correcto sería separar el HTML, el CSS y el JavaScript.
Fue una decisión deliberada: quería un archivo único que funcionara con doble clic y que se
pudiera publicar sin configurar nada.

La pensé para un caso muy concreto: descargo una imagen de internet, no me sirve en ese formato o
pesa demasiado, y necesito arreglarla en el momento. Por eso el escenario habitual es **una sola
imagen**, y como mucho unas cinco. No está pensada para procesar carpetas enteras, y con lotes
grandes conviene leer la advertencia sobre rendimiento más abajo.

Lo que sí hice fue probar cada función y ajustar la interfaz hasta que hiciera lo que necesitaba.
Y me resuelve un problema real: los conversores online suelen ponerle marca de agua al resultado,
limitar el tamaño, caerse en el peor momento, o pedirte que subas tus archivos a un servidor
desconocido.

El código es libre. Cualquiera con nociones de HTML y JavaScript puede replicarlo o mejorarlo.
O puede hacer lo mismo que yo: construirlo con Claude o la IA que prefiera, y aprender en el proceso.

## Funciones

- **Tres modos:** convertir con calidad manual, comprimir apuntando a un peso objetivo,
  o redimensionar conservando el formato original de cada archivo.
- **Formatos de salida:** WebP, AVIF, JPEG y PNG, con control de calidad.
- **Dimensiones:** tamaño original, límite por lado mayor, escala porcentual, o medidas exactas
  con opción de deformar o recortar desde el centro. El límite por lado mayor actúa como techo
  y no agranda las imágenes pequeñas, salvo que se active la ampliación explícitamente.
- **Ajustes por imagen:** configura el lote completo o selecciona imágenes concretas
  y dales una configuración propia.
- **Entrada y salida separadas:** los originales permanecen a la vista, así se distingue
  de un vistazo lo convertido de lo pendiente.
- **Entrada flexible:** arrastrar, explorador de archivos, pegar con `Ctrl+V` o traer por URL.
- **Avisos contextuales:** detecta pérdida de transparencia al pasar a JPEG, aumento de peso
  al pasar fotos a PNG y conversiones que no reducirían el tamaño.
- **Búsqueda de peso:** al comprimir, prueba distintas calidades hasta dar con la más alta que
  quepa bajo el límite pedido, y reduce dimensiones si aun así no alcanza.
- **Medidor de ahorro:** peso antes y después, por imagen y en total.
- **Guía integrada:** cambia según el modo activo. Comparativa de formatos al convertir,
  tabla para interpretar la calidad obtenida al comprimir, y explicación de los modos de
  tamaño al redimensionar.
- **Descargas:** individuales o todas en un `.zip`.
- **Historial:** archiva lo ya convertido para dejar la lista limpia sin perder los resultados.
- **Tema claro, oscuro o según el sistema**, cada uno con su propia paleta.

## Uso

**[Abrir la herramienta](https://ninogunz.github.io/conversor-imagenes/)**

También se puede descargar `index.html` y abrirlo directamente en el navegador. No hay servidor
ni instalación.

Está pensada para trabajar con **una imagen a la vez**, o unas pocas: el caso típico es arreglar
un archivo recién descargado que llegó en el formato equivocado o demasiado pesado. Admite lotes
más grandes, pero conviene revisar antes la sección de rendimiento.

### Sin conexión

Funciona sin internet, con dos diferencias respecto a la versión publicada:

- Las tipografías caen a las del sistema, así que el aspecto cambia un poco.
- La descarga conjunta en `.zip` deja de estar disponible, porque JSZip se carga desde un CDN.
  Las descargas individuales siguen funcionando con normalidad.

La opción de traer imágenes por URL, como es evidente, sí necesita conexión.

## Notas técnicas

**Orientación EXIF.** Las fotos de teléfono se enderezan automáticamente al decodificarlas,
algo que muchos conversores omiten.

**Transparencia y JPEG.** Como JPEG carece de canal alfa, las zonas transparentes se rellenan
en blanco en lugar del negro que Canvas usa por defecto.

**Soporte de AVIF.** Codificar AVIF depende del navegador y es distinto de poder mostrarlo.
Requiere Chrome o Edge 121+, Firefox 133+, o Safari 16.4+ en macOS. Cuando no está disponible,
la opción aparece deshabilitada con la explicación correspondiente.

**Imágenes por URL.** Sujeto a las restricciones CORS del sitio de origen: la mayoría de los
servidores no autoriza que otras páginas lean sus imágenes. Copiar la imagen y pegarla con
`Ctrl+V` evita esa limitación por completo.

**Todo el trabajo lo hace el dispositivo.** El servidor solo entrega el archivo HTML; a partir de
ahí, decodificar, redimensionar y volver a codificar ocurre en el equipo que abrió la página. El
rendimiento depende por completo de ese equipo, y los formatos disponibles también: el botón de
AVIF se habilita o no según el navegador de quien la abre.

**Rendimiento y tamaño de los lotes.** Las imágenes se procesan de a una en el hilo principal, sin
paralelizar, así que se usa un solo núcleo del procesador. Además cada imagen se decodifica
completa en memoria antes de procesarse. En un equipo de escritorio esto no supone ningún problema,
pero en un teléfono un lote de fotos grandes puede agotar la memoria y hacer que el navegador cierre
la pestaña. El modo comprimir es el más exigente, porque codifica cada imagen hasta siete veces, y
con AVIF la diferencia es notoria.

**Dónde quedan los resultados.** Las imágenes convertidas viven en memoria mientras la pestaña esté
abierta y se liberan al cerrarla o recargarla. No se usa `localStorage`, ni cookies, ni ninguna
forma de almacenamiento persistente. Si un resultado es muy pesado, el navegador puede volcarlo
temporalmente a su propia carpeta de perfil, pero ese archivo no es accesible desde otras páginas
y se elimina junto con la pestaña. Lo único que permanece es lo que se descarga de forma explícita.

**Peticiones externas.** La página carga las tipografías desde Google Fonts y JSZip desde un CDN.
Ninguno de los dos recibe imágenes, pero ambos servidores registran la visita. Se pueden incorporar
al repositorio para eliminar esa dependencia.

**Búsqueda de calidad.** El modo comprimir usa búsqueda binaria entre calidad 30 y 95, con siete
pasadas. La imagen se dibuja una sola vez en el lienzo y solo se vuelve a codificar, que es la
operación costosa. Si ni con la calidad mínima se alcanza el peso pedido, las dimensiones se
reducen en pasos del 20 % hasta lograrlo.

**El peso objetivo es un techo, no una meta.** Un resultado muy por debajo del límite significa
que a calidad máxima el archivo simplemente pesa eso. Aumentar el peso de una imagen exigiría
inventar información inexistente, así que no se intenta.

**Modo redimensionar.** Cada imagen se guarda en su formato de entrada, con calidad alta fija.
Los formatos que el navegador no puede escribir (GIF, BMP, y AVIF donde no hay soporte de
codificación) se guardan en PNG, que no pierde información ni transparencia.

**PNG y compresión.** PNG comprime sin pérdida y carece de parámetro de calidad, por lo que queda
deshabilitado en el modo comprimir. Para bajarle el peso hay que reducir sus dimensiones o
cambiar de formato.

**Ampliar imágenes.** Los píxeles añadidos se calculan interpolando los existentes, así que
ampliar no aporta detalle: la imagen se ve más borrosa y pesa más. Por eso está desactivado por
defecto y siempre va acompañado de una advertencia.

**Persistencia.** El historial vive en memoria y se pierde al recargar la página.

## Dependencias

[JSZip](https://stuk.github.io/jszip/) vía CDN, únicamente para empaquetar las descargas
múltiples. El resto es JavaScript sin librerías.

## Licencia

MIT