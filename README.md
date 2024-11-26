# TU AMIGA TITI
##### Proyecto LAB I 
### Por Bianconi Clara y Ogas Avril:
En este repositorio vamos a almacenar toda la informacion sobre el final de la materia LAB I, donde presentaremos todos los archivos necesarios para nuestro proyecto "Tu amiga Titi"

## Descripción

Este proyecto consiste en desarrollar una planta interactiva que funcione como una mascota, integrando funciones automáticas mediante la tecnología Arduino. Sus principales características incluyen:

**Sistema de riego inteligente** : Controlable de forma automática o manual a través de una aplicación, con un sensor de humedad que activa una bomba de agua conectada a un tanque. La planta emite sonidos para avisar cuándo necesita agua y cuándo ha sido regada, mientras que un rostro en la pantalla LCD cambia de expresión según su estado.

**Detección de iluminación**: Un sensor de luz evalúa si la planta recibe la luz adecuada. Según la cantidad de luz, el rostro en la pantalla LCD se ajusta, y el sistema notifica al usuario en la aplicación si la planta necesita ser reubicada en un lugar mejor iluminado.

**Interacción por voz**: La planta responde a comandos básicos como "Hola", "Chau" y "titi", lo que permite una interacción sencilla y amigable. Aunque no es un sistema conversacional avanzado, esta función añade un toque de cercanía y conexión con el usuario.

El objetivo principal de este proyecto es automatizar el cuidado de la planta, ofreciendo una experiencia divertida y accesible para quienes buscan compañía en el hogar sin asumir grandes responsabilidades, facilitando el proceso de cuidado de una manera entretenida y práctica.

## Tecnologías utilizadas
* Placa Arduino MEGA
* Arduino IDE
* APP Inventor

## Contenido del repositorio
**1. Documentos**

    * Anteproyecto 
    * Informe del proyecto
    * Presentacion PowerPoint 
    * Video Tutorial
    
**2. APP**

    * archivo .aia de "APP Inventor"
    * codigo de la APP 

**3. Arduino**

    * codigo en c++

**4. Diseño 3D**

    * Diseño 3D Titi
        + obj.mtl
        + tinker.obj

**5. Diseño Electronico**

    * Diseño Electronico
        + diseño del circuito electronico-AutoDesk
        + diseño electronico titi.brd
        + diseño electronico-editado
        + diseño electronico-tinkercard

## Guía de instalación 
Para la correcta instalación del prototipo "Tu Amiga Titi", es necesario seguir los pasos detallados a continuación para asegurar la correcta conexión y configuración de los componentes, apoyese de los recursos provistos en la documentacion (Video tutorial) y en el diseño electronico (esquematico del diseño electronico-editado). 

### Conexiones: 

Primero, conecte la placa Arduino Mega a un espacio seguro, preferiblemente utilizando una protoboard para simplificar las conexiones y evitar posibles cortocircuitos. A continuación, conecte los cables necesarios de la mini bomba de agua en la protoboard e coloque la mini bomba de agua sumergible dentro de un recipiente con agua, conectando una manguera de cristal a su salida y dirigiéndola hacia la maceta que contiene la planta. Asegúrese de que la bomba esté completamente sumergida en el agua para un funcionamiento óptimo. 

La pantalla LCD Display 20x4 debe conectarse a la placa Arduino, siguiendo el esquema de conexión provisto, prestando especial atención a los pines de alimentación y comunicación. El sensor de humedad debe insertarse en el sustrato de la planta, conectando sus pines de alimentación y señal a la placa Arduino Mega. Asegúrese de que el sensor esté colocado en la tierra de su planta. El sensor capacitivo TTP223, que permite la interacción táctil, debe ubicarse en un lugar de fácil acceso y conectarse adecuadamente a la placa, siguiendo el esquema de conexión proporcionado. 

El sensor de luz debe instalarse en un lugar donde pueda medir de manera precisa la intensidad de la luz ambiental. Asegúrese de que sus pines de alimentación y señal estén conectados correctamente a la placa Arduino. El módulo DFPlayer Mini debe conectarse al serial 1 de la placa Arduino Mega, asegurándose de que el altavoz esté correctamente conectado para la emisión de sonidos. Finalmente, el módulo Bluetooth HC-06 debe conectarse al puerto serial 2 en la placa Arduino, verificando las conexiones de alimentación, RX y TX. 

Código: 

Una vez realizados todos los cableados, verifique que todos los componentes estén correctamente instalados: la bomba de agua debe estar funcionando dentro del recipiente con agua, la manguera debe estar correctamente orientada hacia la maceta, y los sensores deben estar correctamente ubicados para medir la humedad, la luz y recibir la interacción táctil. Posteriormente, cargue el código proporcionado en el Arduino IDE, seleccionando correctamente el puerto y modelo de la placa antes de compilar y cargar el programa. 

Aplicación: 

 Una vez validado el sistema, descargue e instale la aplicación móvil desde el enlace proporcionado. Asegúrese de que el dispositivo Bluetooth del prototipo se conecte correctamente con su teléfono móvil. Con todos estos pasos completados, el prototipo estará listo para su uso.

