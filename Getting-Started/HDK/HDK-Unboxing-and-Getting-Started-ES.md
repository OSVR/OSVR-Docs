# Felicitaciones por tu Hacker Development Kit!

Esta es una traducción de "Getting Started", y como tal puede quedar desactualizada, así que es probable que algunos datos sean inválidos.

## Identificando tu dispositivo
Las principales variedades de HDK en el mercado son HDK 1.2 y HDK 1.3 . Ambas vienen dentro de una caja de cartón con manija de transporte con logotipos de OSVR y generalmente incluye un kit de cámara IR.

> Nota: Si obtuviste tu HDK antes de mediados del 2015, particularmente si no trajo una lujosa caja ensamblada en una fábrica con una cámara IR, y/o posee una pantalla LCD en vez de una OLED, es posible que sea un prototipo anterior. Generalmente puedes tratarlo como un HDK 1.2, excepto que deberìas evitar actualizar el firmware sin contactar al soporte : No quisiéramos que instales un firmware para OLED en una unidad con LCD, y dado que el Hacker Development Kit está diseñado para ser hackeado, incluso hasta la pantalla, hacer eso es posible.
The main exterior difference between the 1.3 and preceding versions is the adjustments for the optics, so it's the easiest way to identify what you have if you are unsure:

- El HDK 1.2 y anteriores poseen ajustes a tornillo por debajo de cada ojo, con ajuste de distancia entre pupilas (IPD) y foco. Esta imagen es de un HDK 1.1, pero el mecanismo en el HDK 1.2 es efectivamente el mismo. ![HDK pre-1.3 ajuste ocular](HDK11.jpg)
- El HDK 1.3 posee ajustes deslizables debajo de cada ojo para el ajuste de foco ( óptica modificada para brindar un "eyebox" más grande ( zona tridimensional en la cual el ojo puede ver la imagen correctamente ) eliminó la necesidad para el ajuste de distancia entre pupilas (IPD) ). ![HDK 1.3 ajuste deslizante](HDK13-ID.jpg)

Existen diferencias generalmente encontradas entre los chipsets de las "belt boxes" del HDK 1.2  y 1.3, así que cuando tengas el pack de drivers instalados ( usando Windows ), eso puede ayudarte a recordarlo, pero las diferencias mecánicas son las más confiables.

## Advertencias/Limitaciones
- El archivo de configuración por defecto en Windows asume un HDK 1.3, renderizado en modo directo ( direct mode ), y seguimiento por fusión de IMU y video. En HDK 1.2 o si no puedes/quieres usar modo directo, es necesario cambiar el archivo de configuración. Hay archivos de configuración con nombres autoexplicativos incluidos.
- El servidor OSVR actualmente muestra una consola que puede ser minimizada, pero no debe ser cerrada mientras usas una aplicación OSVR. Una interfaz más agradable o invisible está planeada.

## Configuración

### Conectando el HDK y la Cámara IR.
Hay un par de conexiones que hacer: HDK al beltbox, electricidad de la pared al beltbox, HDMI y USB entre el beltbox y tu PC.
El orden particular no importa. Mantén la precaución de no colocar el beltbox en una posición que estrangule o estire los cables.

Si usas seguimiento por video ( posicional ), asegurate de que el cable de sincronización esté conectado.

Una vez conectados el HDK, HDMI y cable de poder a la pared, el HDK debe ser reconocido como un display nuevo en la PC. Los usuarios de Windows pueden necesitar elegir "extender pantalla" en "Propiedades de Pantalla".
Usuarios de Linux: Pueden extender el display o ejecutar otra pantalla de X en el HDK, dependiendo del uso.

Probablemente se vea como un display de 1080x1920 "Retrato" por defecto. Este es el modo de mayor rendimiento. Sin embargo , actualmente la mayoría de las aplicaciones no funcionan en este modo, así que te conviene elegir la resolución de 1920x1080. ( Esto no significa que que debas cambiar la orientación , el HDK se encarga de eso internamente.)

En este punto debes tener la Cámara IR conectada, cuando menos para actualizar el firmware. Desafortunadamente al día de hoy el actualizador de firmware para la Cámara IR sólo funciona en Windows.

### Windows: Instalar el pack de drivers ( opcional pero recomendado )
Si te encuentras en Windows, hay un instalador del pack de drivers que puede mejorar tu experiencia. Si bien no es estrictamente necesario para un uso básico, provee mejores nombres para los dispositivos en el administrador de dispositivos, agrupa componentes de los dispositivos lógicamente con íconos correctos en "Dispositivos e Impresoras", y en Windows 8.1 ( y anteriores) es requerido para usar el software de control del OSVR HDK para actualizar el firmware, etc. ( Windows 10 ya posee el driver correcto, pero los otros beneficios aún se aplican).

Puedes encontrar la última versión en <https://github.com/OSVR/OSVR-HDK-Windows-Drivers/releases>. Descarga e instala antes de continuar.

### Verifica/Actualiza el firmware

Hay varias partes del sistema que tienen firmware que puede ser actualizado.
El **Firmware de la Cámara IR** actualizado es esencial, ya que las actualizaciones proveen mejoras sustanciales de rendimiento como compatibilidad con Linux, OS X, y otros paquetes de software además del plugin de seguimiento por video de OSVR. En Windows, con drivers 1.2.6 o superior, desde INICIO, escribe "Dispositivos e Impresoras" y presiona Enter, y deberías ver un número de dispositivos vinculados a OSVR con íconos correspondientes a lo que tengas conectado.

Si el ícono de tu Cámara IR se ve así:
![IR camara necesita actualizar](camera-needs-upgrade.png)
Click derecho, y elige "Get firmware upgrade". Alternativamente puedes ir a <http://osvr.github.io/using/>.
![IR menu contextual camara](camera-context-menu.png)
Una vez allí, debes buscar la descarga para el actualizador de firmware de la Cámara IR: va a estar marcado con este símbolo:
![IR camara simbolo actualizar](ircamera-updater.png)


> ¿No usas Windows?: Si bien no puedes actualizar el firmware en tu sistema, puedes revisar y ver la versión del firmware, ya que es parte del ID del hardware USB. El ID del fabricante es 0x0bda, ID de producto es 0x57e8, y la versión del firmware es cualquiera que sea listada en el campo `bcdDevice` ( que figura como `REV_` en Windows).
> En Linux, `lsusb -v -d 0bda:57e8` presenta mucha información sobre el dispositivo, y `lsusb -v -d 0bda:57e8 | grep bcdDevice` presenta sólo la revisión 0.07 para versión 7, la última al tiempo de autoría de este documento. Si es menor, el firmware necesita actualizaciones.

**El procesador del HDK** también posee firmware.

Si posees un HDK 1.2, el firmware más reciente que necesitas es la versión **1.84** - versiones más nuevas contienen código específico para la pantalla OLED del 1.3. Ve las instrucciones para un [HDK 1.2 upgrade procedure](HDK-1.2-Firmware-Update.md) que instalará automáticamente este software, así como también automatizar el proceso de actualización del de los VID/PID del procesador en caso de que tengas una unidad antigua donde estos estén fallados.

En ambos 1.2 y 1.3 el OSVR HDK Control utility en Windows es el mejor lugar para actualizar el firmware y para ajustar algunas características especiales en el HDK. Puedes obtenerlo en <http://osvr.github.io/using/>. También puede reportar la versión actual del firmware que tienes instalado, etc.
El firmware del HDK se puede actualizar en Linux o Mac OS X, pero el proceso no fue completamente documentado aún. Si necesitas instrucciones, contacta al soporte y te ayudaremos , actualizando la documentación en el proceso.

### Obtener el OSVR Server

OSVR Server es parte del framework OSVR y provee un sistema para acceder a los datos del dispositivo, configurar periféricos, etc. Los drivers del HDK están incluidos en el paquete OSVR Core ( que incluye al server ), y tu HDK puede ser auto detectado, así que no vas a necesitar editar ninguna configuración, salvo que desees conectar dispositivos de entrada adicionales.

Hay dos formas de obtener el OSVR Server: el instalador y las "build snapshots". Ya que actualmente el instalador no se actualiza automáticamente , la mejor manera es bajar un "snapshot" de OSVR Core, vinculado desde [Using OSVR][using]. Si usas una versión de 64 bits de Windows, tanto la de 32 o la de 64 bits funcionarán ( y serán compatibles con aplicaciones de 32 y 64 bits), así que sólo elige una. ( Usuarios de Linux: favor de ver [instrucciones de compilación](../Installing/Linux-Build-Instructions.md) en este repositorio.)

En cualquier caso, ejecutar `osvr_server` debe abrir una ventana de comandos con algunos mensajes. Si todo funciona bien, verás una linea que dice :

```
Added device: com_osvr_Multiserver/OSVRHackerDevKit0
```

Si te encuentras en Linux o Mac OS X y no observas esta línea, debes referirte a [known issues].

Puedes minimizar esta ventana, pero asegúrate de mantenerla corriendo mientras uses aplicaciones para OSVR.

[OSVR-Core]: https://github.com/OSVR/OSVR-Core/
[using]: http://osvr.github.io/using/
[known issues]: ../Installing/Linux-Build-Instructions.md#known-issues-temporary

### Ajuste de Óptica (HDK 1.0, 1.1, 1.2)
[[assets/HDK-optics-adjustment.png]]

Este diagrama del ajuste de óptica ([[printable full-page PDF here|assets/HDK-optics-adjustment.pdf]]) muestra cuales son los ajustes. Querrás hacer los ajustes mientras tengas puesto el visor con una imagen en pantalla, pero con un ojo a la vez ( cierra uno a la vez ). Puedes usar tus anteojos/gafas si los necesitas para estimar dónde quieres comenzar el control de foco, y luego ajustar el IPD hasta que las lentes se sientan centradas con tu ojo y toda la pantalla se ve igualmente nítida.

### Ajuste de Óptica (HDK 1.3)

Los deslizadores bajo las lentes del HDK 1.3 ajustan el foco en diópteras. Si normalmente usas anteojos/gafas, es posible que con el ajuste de foco no las necesites y puedas lograr una imágen nítida. De lo contrario, ajusta hasta que veas una imagen nítida con ambos ojos, seguramente cuando ambos ojos se encuentren cerca de la marca etiquetada 0.


## Software

### Visor de Seguimiento ( Tracker Viewer )
La primera aplicación que sugerimos probar no es glamorosa, pero es útil para chequear y asegurarse de que las cosas funcionen. En [Using OSVR][using] encontrarás un enlace de descarga para el OSVR Tracker Viewer. Descarga y ejecutalo ( ¡Con el OSVR Server corriendo! ) y verás una pequeña ventana con unas flechas 3D. Si eres conocedor de gráficos 3D o de VR, probablemente deduzcas lo que ves y lo que significa, pero la parte importante en general es verificar que las pequeñas flechas en el centro se mueven cuando rotas el HDK. ( Puedes hacer click derecho y arrastrar para hacer zoom)

Por supuesto puedes ignorar este paso, pero si tienes problemas, alguien te preguntará probablemente lo que ves en el Tracker Viewer.


### La Demo "Palace" ( Palacio )

La Demo Palace [OSVR Unity Palace Demo](https://github.com/OSVR/OSVR-Unity-Palace-Demo/releases) [(source repo)](https://github.com/OSVR/OSVR-Unity-Palace-Demo) es un entorno visualmente rico para observar y explorar usando dispositivos soportados por OSVR, incluyendo el HDK. EL primer vínculo contiene descargas para el binario para Windows: sólo descarga y ejecuta ( ¡Con el OSVR Server corriendo! ), y si lo deseas puedes usar un comando de juego o el teclado más el mouse. En la pantalla de inicio puedes decidir qué pantalla está configurada como tu HDK.

Recuerda que esta aplicación, como todas las aplicaciones Unity, si la ventana pierde el foco, la aplicación se congela, la mayoría de las veces se resuelve clickeando dentro de la ventana nuevamente.

## ¡Ayuda! ( Asume que el lenguaje del soporte es el Inglés )

Si experimentas problemas de hardware o software en general, abre un ticket de ayuda en [OSVR Support](http://support.osvr.com).

Si estás interesado en desarrollar software para OSVR o contribuir de alguna forma, favor de ver el [Developer Portal at osvr.github.io](http://osvr.github.io) para unirte al desarrollo/lista de correo, y ver la lista de proyectos y su información para desarrolladores.

Los tickets de soporte son monitoreados por múltiples personas así que serán atendidos con cuidado por la mejor persona disponible y preparada para responder tu pregunta. GitHub y la lista de correo son las principales formas para contactarse con los desarrolladores que contribuyen con OSVR.

- No es recomendado hacer preguntas sobre problemas o publicar preguntas en otros lugares ( foros/reddit, twitter, etc.) si deseas una respuesta "oficial" o de los desarrolladores, dado que no son los lugares que nosotros (desarrolladores/personas técnicas) necesariamente frecuentamos diariamente, o controlamos las cuentas "oficiales" en esos lugares.

- Como una regla general de comunidades de open-source, y por ende en OSVR también, es usualmente considerado descortés enviar un email a un desarrollador personalmente con una pregunta. Entre otras razones, sólo le permite a una persona atender tu pregunta ( cuando puede haber otras que podrían hacerlo, e incluso mejor) y no produciría un intercambio útil disponible en los archivos públicos. ( No te sorprendas si tu email es republicado como un ticket de soporte ).


