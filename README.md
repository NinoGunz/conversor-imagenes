# Conversor de imágenes por lotes

Herramienta web para convertir, comprimir y redimensionar imágenes **sin subirlas a ningún servidor**.
Todo el procesamiento ocurre dentro del navegador usando la API de Canvas.

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

Lo que sí hice fue probar cada función y ajustar la interfaz hasta que hiciera lo que necesitaba.
Y me resuelve un problema real: los conversores online suelen ponerle marca de agua al resultado,
limitar el tamaño, caerse en el peor momento, o pedirte que subas tus archivos a un servidor
desconocido.

El código es libre. Cualquiera con nociones de HTML y JavaScript puede replicarlo o mejorarlo.
O puede hacer lo mismo que yo: construirlo con Claude o la IA que prefiera, y aprender en el proceso.

## Funciones

- **Formatos de salida:** WebP, AVIF, JPEG y PNG, con control de calidad.
- **Dimensiones:** tamaño original, límite por lado mayor (proporcional) o medidas exactas
  con opción de deformar o recortar desde el centro.
- **Ajustes por imagen:** configura el lote completo o selecciona imágenes concretas
  y dales una configuración propia.
- **Entrada y salida separadas:** los originales permanecen a la vista, así se distingue
  de un vistazo lo convertido de lo pendiente.
- **Entrada flexible:** arrastrar, explorador de archivos, pegar con `Ctrl+V` o traer por URL.
- **Avisos contextuales:** detecta pérdida de transparencia al pasar a JPEG, aumento de peso
  al pasar fotos a PNG y conversiones que no reducirían el tamaño.
- **Medidor de ahorro:** peso antes y después, por imagen y en total.
- **Descargas:** individuales o todas en un `.zip`.
- **Historial:** archiva lo ya convertido para dejar la lista limpia sin perder los resultados.
- **Tema claro, oscuro o según el sistema.**

## Uso

Abrir `index.html` en el navegador. No requiere servidor ni conexión a internet,
salvo para la opción de traer imágenes por URL.

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

**Persistencia.** El historial vive en memoria y se pierde al recargar la página.

## Dependencias

[JSZip](https://stuk.github.io/jszip/) vía CDN, únicamente para empaquetar las descargas
múltiples. El resto es JavaScript sin librerías.

## Licencia

MIT
