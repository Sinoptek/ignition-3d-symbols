VISTAS DE PERSPECTIVE / PERSPECTIVE VIEWS
=========================================
ES
Dos vistas en formato view.json, bajo Exchange/Sinoptek 3D Symbols/:

  Symbol  vista para incrustar con dos parámetros: symbol ("centrifugal-pump", "on-off-valve", "gas-boiler") y
          state ("stopped", "running", "alarm"; en la válvula "closed", "open", "alarm"). La imagen sale de
          /system/images/exchange/sinoptek-3d-symbols/<symbol>-<state>.png

  Demo    mini circuito de calor interactivo, 1120 x 610 px: cabecera SINOPTEK, caldera en sección, bomba y
          válvula a escala entre sí (300, 180 y 100 px; la bomba en espejo con scaleX(-1) en el estilo del
          componente, sin tocar la imagen), tuberías de impulsión (roja) y retorno (azul) con codos curvados,
          dibujadas con componentes, un emisor de calor también dibujado (no es un símbolo del paquete), tres lecturas de demostración (valores fijos, no datos
          reales), un rótulo de estado por equipo y tres botones STOPPED / RUNNING / ALARM.
          Todo depende de view.custom.mode ("stopped" | "running" | "alarm"; se abre en "running"). Los botones
          lo escriben con un script (self.view.custom.mode = '...'); imágenes, rótulos, colores y lecturas lo leen
          con enlaces de expresión. El modo cambia los tres equipos a la vez. En marcha usa
          gas-boiler-running-animated.png, centrifugal-pump-running.png y on-off-valve-open.png; en alarma, los
          tres ...-alarm-blink.png; en paro, gas-boiler-stopped, centrifugal-pump-stopped y on-off-valve-closed.
          Para gobernarla con datos propios: enlazar view.custom.mode a un tag o expresión que devuelva uno de los
          tres textos y quitar los botones (o hacer el enlace bidireccional). Esa variante no está probada.

Requisitos
  Las 16 imágenes de images/exchange/sinoptek-3d-symbols/ (12 fijas y 4 animadas) subidas a Image Management,
  en la carpeta exchange/sinoptek-3d-symbols. Symbol usa las fijas; Demo usa fijas y animadas.

Marca: las imágenes solo llevan la chapa SINOPTEK del propio equipo, sin rótulo añadido. En Demo la marca aparece
además en la cabecera y en el texto del pie.

Probado: Ignition 8.3.10, sesión de Perspective en Chrome. Imágenes, APNG y las dos vistas funcionan sin retocar;
Symbol, incrustada con un componente Embedded View (ia.display.view) y sus params, pinta el estado correcto; en
Demo los tres botones cambian los tres equipos, sus rótulos, las lecturas y el botón activo.
Sin probar: el camino por el Designer (crear la vista vacía y pegar el JSON con Mayús + clic derecho, o importar un
proyecto exportado), Vision y versiones anteriores a 8.3.10 (incluida 8.1). No son un proyecto exportado.

Dato observado en 8.3.10: las imágenes de Image Management se guardan en el disco del gateway, en
data/config/resources/core/ignition/images/<ruta>/<archivo>.png/ (una carpeta por imagen, con el PNG y un
resource.json), y se sirven en /system/images/<ruta>/<archivo>.png.

Los nombres siguen la guía de estilo de Exchange (carpeta Exchange/<recurso>, vistas en Title Case, propiedades
en camelCase, parámetros públicos en params).

EN
Two Perspective views as view.json under Exchange/Sinoptek 3D Symbols/.
Both need the 16 images of images/exchange/sinoptek-3d-symbols/ uploaded to Image Management under
exchange/sinoptek-3d-symbols.
Symbol: embeddable view with params "symbol" and "state".
Demo: small interactive heating loop (1120 x 610 px) with the cutaway boiler, the pump (shown mirrored with a
scaleX(-1) style transform; the image file is unchanged) and the valve, supply and return pipes, a heat emitter drawn with components (not a symbol of the pack), three demonstration readings
(fixed values), a status label per item and three buttons STOPPED / RUNNING / ALARM. Everything follows
view.custom.mode ("stopped" | "running" | "alarm"), written by the buttons with a script and read by expression
bindings; bind it to your own tag to drive the screen from your plant (that variant is not tested).
Tested: Ignition 8.3.10, Perspective session in Chrome: images, APNG, both views unmodified, Symbol embedded with
an Embedded View component, and the three Demo buttons.
Not tested: the Designer route (pasting the JSON into an empty view, or importing a project export), Vision, and
Ignition versions earlier than 8.3.10 (including 8.1). These files are not a project export.
