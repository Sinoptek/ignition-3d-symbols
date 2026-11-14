# SINOPTEK · Símbolos 3D · Paquete de inicio

Tres símbolos de equipos renderizados en 3D para pantallas SCADA / HMI / BMS, de uso libre en tus proyectos.
Hecho por SINOPTEK · https://sinoptek.com

> Qué está probado: Ignition 8.3.10, en una sesión de Perspective (Chrome). Las imágenes PNG se ven bien, el PNG
> animado (APNG) se reproduce dentro del componente Image de Perspective y las dos vistas de `perspective-views/`
> (`Symbol` y `Demo`, incluidos los tres botones de `Demo`) funcionan tal como se entregan.
> Sin probar: la importación por el Designer (subida con Image Management, pegado del JSON de la vista), Vision y
> cualquier versión de Ignition anterior a la 8.3.10 (incluida la 8.1).

## Qué incluye

| Símbolo | Vista | Ficheros (prefijo) | Estados |
|---|---|---|---|
| Bomba centrífuga | 3D exterior | `centrifugal-pump-` | stopped (paro), running (marcha), alarm (alarma) |
| Válvula todo/nada con actuador | 3D exterior | `on-off-valve-` | closed (cerrada), open (abierta), alarm |
| Caldera de gas de condensación | 3D en sección (se ve el quemador y el serpentín) | `gas-boiler-` | stopped, running, alarm + 3 fotogramas de marcha |

- `images/exchange/sinoptek-3d-symbols/` · 16 PNG en una sola carpeta, de 256 × 256 px y fondo transparente:
  - 12 imágenes fijas, con nombre `<símbolo>-<estado>.png`. Todos los estados de un símbolo comparten encuadre:
    al cambiar la imagen el símbolo no salta.
  - 4 PNG animados (APNG): `-alarm-blink` para cada símbolo y `gas-boiler-running-animated.png` (se mueve la
    llama). Si prefieres no usar APNG, recorre los fotogramas (ver abajo).
- `perspective-views/` · dos vistas de Perspective en `view.json`: `Symbol` (un símbolo para incrustar) y `Demo`
  (un pequeño circuito de calor interactivo). Ver «La vista Demo» más abajo y la nota de la carpeta.
- `manifest.json` · lista de símbolos, estados y ficheros.
- `LICENSE` · licencia MIT.

No incluye: animación de marcha de la bomba ni de la válvula (su «marcha»/«abierta» es una sola imagen), la vista
exterior de la caldera, versiones vectoriales SVG, otros tamaños ni los alzados 2D.

Convención de color del estado: luz apagada = paro/cerrada, verde = marcha/abierta, roja = alarma. En la caldera la
llama solo se ve en marcha. En los tres fotogramas de marcha de la caldera la llama está acentuada (cambia de
intensidad y de altura de un fotograma a otro) para que la animación se aprecie a 256 px.

Marca: la única marca de las imágenes es la pequeña chapa SINOPTEK que forma parte de cada equipo. No llevan
ningún rótulo, firma ni marca de agua añadidos.

## Uso en Perspective

1. Designer > Tools > Image Management. Crea las carpetas `exchange/sinoptek-3d-symbols` y sube todos los PNG de
   `images/exchange/sinoptek-3d-symbols/`.
2. Pon un componente Image en una vista. En `props.source` escribe la ruta de la imagen precedida de
   `/system/images`, por ejemplo `/system/images/exchange/sinoptek-3d-symbols/centrifugal-pump-stopped.png`.
3. Para que siga a un tag, pon un enlace de tipo Expression en `props.source`. Ejemplo con dos tags booleanos
   (cambia las rutas de los tags por las tuyas):

   ```
   "/system/images/exchange/sinoptek-3d-symbols/centrifugal-pump-"
   + if({[default]Pump1/Alarm}, "alarm", if({[default]Pump1/Running}, "running", "stopped"))
   + ".png"
   ```

   Con un tag entero de estado (0 = paro, 1 = marcha, 2 = alarma):

   ```
   "/system/images/exchange/sinoptek-3d-symbols/gas-boiler-"
   + case({[default]Boiler1/Status}, 1, "running", 2, "alarm", "stopped")
   + ".png"
   ```

4. Marcha animada de la caldera: usa `gas-boiler-running-animated.png` o recorre los tres fotogramas, por ejemplo

   ```
   "/system/images/exchange/sinoptek-3d-symbols/gas-boiler-running-frame-"
   + ((getSecond(now(1000)) % 3) + 1) + ".png"
   ```

La vista `Symbol` (ruta de imagen montada con sus parámetros `symbol` y `state` mediante una expresión) está
probada. Las expresiones con tags de arriba siguen el lenguaje de expresiones de Ignition, pero no se han ejecutado
contra tags reales.

## La vista Demo

`Exchange/Sinoptek 3D Symbols/Demo` es una pantalla ya montada de 1120 × 610 px que enseña los tres símbolos
trabajando juntos en un pequeño circuito de calor:

- cabecera con SINOPTEK, el nombre del paquete y sinoptek.com;
- la caldera en sección, la bomba y la válvula, a escala entre sí (300, 180 y 100 px). La imagen de la
  bomba tiene la aspiración a la izquierda, mirando a la caldera;
- tuberías de impulsión (roja) y retorno (azul) con codos curvados, bridas y flechas de sentido: la impulsión sale
  de la caldera, entra por la aspiración de la bomba, sale por su impulsión (arriba), cruza y baja hasta la válvula,
  y sigue hasta un emisor de calor; el retorno es una U azul de vuelta a la caldera. Tuberías y emisor están
  dibujados con componentes normales de Perspective; el emisor no es un símbolo del paquete;
- tres lecturas (temperatura de impulsión, temperatura de retorno y caudal). Son valores fijos de demostración,
  un juego por modo, no datos reales;
- un rótulo de estado bajo cada equipo;
- tres botones: STOPPED, RUNNING, ALARM.

Todo depende de una propiedad propia de la vista, `view.custom.mode`, que toma los textos `stopped`, `running` o
`alarm` (la vista se abre en `running`). Cada botón la escribe con un script de una línea
(`self.view.custom.mode = 'running'`); imágenes, rótulos, colores y lecturas la leen con enlaces de expresión.
El modo se aplica a los tres equipos a la vez:

| modo | caldera | bomba | válvula |
|---|---|---|---|
| `stopped` | `gas-boiler-stopped.png` | `centrifugal-pump-stopped.png` | `on-off-valve-closed.png` |
| `running` | `gas-boiler-running-animated.png` | `centrifugal-pump-running.png` (imagen única) | `on-off-valve-open.png` |
| `alarm` | `gas-boiler-alarm-blink.png` | `centrifugal-pump-alarm-blink.png` | `on-off-valve-alarm-blink.png` |

Requisito: las 16 imágenes del paquete deben estar en Image Management, en `exchange/sinoptek-3d-symbols`
(la vista usa imágenes fijas y animadas de esa carpeta).

Para gobernarla desde tu instalación en lugar de con los botones, enlaza `view.custom.mode` a un tag propio o a una
expresión que devuelva `"stopped"`, `"running"` o `"alarm"`, quita los botones (o haz el enlace bidireccional) y
sustituye las tres lecturas por tus tags. Esta variante con tags no la hemos probado; lo probado es la vista tal
como se entrega, con los tres botones cambiando los tres símbolos, sus rótulos, las lecturas y el botón activo.

## Uso en Vision

1. Sube los mismos PNG con Tools > Image Management.
2. Pon un componente Image y enlaza su propiedad Image Path con una expresión del mismo tipo. En Vision la ruta es
   la de Image Management sin el prefijo `/system/images/`, por ejemplo
   `"exchange/sinoptek-3d-symbols/centrifugal-pump-" + if({[default]Pump1/Running}, "running", "stopped") + ".png"`.

Sin probar en Vision. No es de esperar que Vision reproduzca el APNG; usa los fotogramas.

## Licencia

Licencia MIT (ver `LICENSE`). Puedes usar, copiar, modificar y redistribuir estos ficheros, también en proyectos
comerciales, siempre que conserves el aviso de copyright y el texto de la licencia. Se entregan «tal cual», sin
garantía. La licencia cubre solo los ficheros de este paquete.

## Más símbolos

Este paquete es una selección pequeña y completa en sí misma: con los tres símbolos se monta un pequeño circuito
de calor (caldera + bomba + válvula). Las bibliotecas SINOPTEK (calderas, producción de frío,
climatizadoras modulares, equipos de proceso industrial; vistas 3D, en corte y 2D; varios tamaños; símbolos a medida)
están en https://sinoptek.com · contacto: comercial@sinoptek.com
