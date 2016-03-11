# Congrats on your Hacker Development Kit!

Esta es una traducción de "Getting Started", y como tal va a estar desactualizada, así que es probable que algunos datos sean inválidos.

## Identificando tu dispositivo
Las principales variedades de HDK en el mercado son HDK 1.2 y HDK 1.3 . Ambas vienen dentro de una caja de cartón con manija de transporte con logotipos de OSVR y generalmente incluye un kit de cámara IR.

> Nota: Si obtuviste tu HDK antes de mediados del 2015, particularmente si no trajo una lujosa caja ensamblada en una fábrica con una cámara IR, y/o posée una pantalla LCD en vez de una OLED, es posible que sea un prototipo anterior. Generalmente puedes tratarlo como un HDK 1.2, excepto que deberìas evitar actualizar el firmware sin contactar al soporte : No quisiéramos que instales un firmware para OLED en una unidad con LCD, y dado que el Hacker Development Kit está diseñado para ser hackeado, incluso hasta la pantalla, hacer eso es posible.
The main exterior difference between the 1.3 and preceding versions is the adjustments for the optics, so it's the easiest way to identify what you have if you are unsure:

- El HDK 1.2 y anteriores poseen ajustes a tornillo por debajo de cada ojo, con ajuste de distancia entre pupilas (IPD) y foco. Esta imagen es de un HDK 1.1, pero el mecanismo en el HDK 1.2 es efectivamente el mismo. ![HDK pre-1.3 ajuste ocular](HDK11.jpg)
- El HDK 1.3 posee ajustes deslizables debajo de cada ojo para el ajuste de foco ( óptica modificada para brindar un "eyebox" más grande ( zona tridimensional en la cual el ojo puede ver la imagen correctamente ) eliminó la necesidad para el ajuste de distancia entre pupilas (IPD) ). ![HDK 1.3 ajuste deslizante](HDK13-ID.jpg)

Existen diferencias generalmente encontradas entre los chipsets de las "belt boxes" del HDK 1.2  y 1.3, así que cuando tengas el pack de drivers instalados ( usando Windows ), eso puede ayudarte a recordarlo, pero las diferencias mecánicas son las más confiables.

## Advertencias/Limitaciones
- El archivo de configuración por defecto en Windows asume un HDK 1.3, renderizado en modo directo ( direct mode ), y seguimiento por fusión de IMU y video. En HDK 1.2 o si no puedes/quieres usar modo directo, es necesario cambiar el archivo de configuración. Hay archivos de configuración con nombres auto explicativos incluidos.
- El servidor OSVR actualmente muestra una consola que puede ser minimizada, pero no debe ser cerrada mientras usas una aplicación OSVR. Una interfaz más agradable o invisible está planeada.

## Configuración

### Conectando el HDK y la Cámara IR.
Hay un par de conecciones que hacer: HDK al beltbox, electricidad de la pared al beltbox, HDMI y USB entre el beltbox y tu PC.
El orden particular no importa. Mantén la precaución de no colocar el beltbox en una posición que estrangule o estire los cables.
