# TU AMIGA TITI
##### Proyecto LAB I 
### Por Bianconi Clara y Ogas Avril:
En este repositorio vamos a almacenar toda la informacion sobre el final de la materia LAB I, donde presentaremos todos los archivos necesarios para nuestro proyecto "Tu amiga Titi"

## Descripción

Este proyecto consiste en desarrollar una planta interactiva que funcione como una mascota, integrando funciones automáticas y personalizables mediante tecnología Arduino. Sus principales características incluyen:

**Sistema de riego inteligente:** Un sensor de humedad evalúa el estado del suelo y activa automáticamente una bomba de agua conectada a un tanque cuando la planta necesita ser regada. Además, cuando se le consulta su estado, el sistema emitira sonidos para informar al usuario tanto cuando la planta requiere agua como cuando ha sido regada de más.

**Detección de iluminación:** Un sensor de luz analiza si la planta recibe la cantidad adecuada de iluminación. Si la luz es insuficiente o demasiada, el sistema, al ser consultado mediante la aplicación móvil, notifica al usuario y sugiere reubicar la planta en un lugar mejor iluminado para optimizar su cuidado.

**Interacción por voz:** La planta responde a comandos básicos como "Hola", "Chau" o "¿Como estas?", lo que permite una interacción sencilla y amigable. Adicionalmente, al consultar su estado, la planta mostrará una reacción visual en la pantalla LCD, reflejando su estado de ánimo o necesidades actuales, lo que agrega un toque de cercanía y personalidad al sistema.

**Configuración y personalización mediante aplicación móvil:** A través de una aplicación móvil, el usuario puede configurar el sistema de acuerdo con el tipo de planta que posee, ajustando parámetros como niveles óptimos de humedad y luz. También es posible interactuar directamente con la planta, consultando su estado, enviando comandos básicos o recibiendo notificaciones en tiempo real sobre su cuidado.

El objetivo principal de este proyecto es automatizar el cuidado de la planta, brindando una experiencia entretenida, accesible y práctica. Está diseñado para personas que buscan disfrutar de la compañía de una planta sin las preocupaciones asociadas a su mantenimiento diario, facilitando el proceso de cuidado mientras añaden un toque interactivo y divertido a su hogar.

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
    * codigo en bloques de la APP 

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

### 1. Recursos Adicionales
Aprovecha los recursos proporcionados para facilitar el proceso de instalación:

***Video tutorial:*** Consulta el video para una explicación visual de cada paso.

***Esquemático del diseño electrónico:*** Verifica las conexiones siguiendo el esquema editado.

### 2. Conexiones de Hardware

**Paso 1: Configuración Inicial**

- Coloca la placa Arduino Mega en un lugar seguro, utilizando una protoboard para las conexiones y evitar cortocircuitos.
- Conecta la placa a tu computadora mediante un cable USB o a una fuente de alimentación externa adecuada.

**Paso 2: Conexión de la Mini Bomba de Agua**
- Sitúa la mini bomba sumergible en un recipiente lleno de agua.
- Conecta una manguera de cristal a la salida de la bomba y dirige su extremo hacia la maceta.
- Asegúrate de que la bomba esté completamente sumergida en el agua.
- Realiza la conexión eléctrica:
    + VCC y GND al módulo de control (relé o transistor).
    + Conecta el módulo al Arduino Mega para controlar la bomba desde el software.

**Paso 3: Pantalla LCD Display 20x4**
- Conecta la pantalla LCD al módulo I2C para simplificar las conexiones.
- Realiza las conexiones según el esquema:
    + VCC: Pin de 5V en Arduino Mega.
    + GND: Pin GND en Arduino Mega.
    + SDA: Pin SDA (20) en Arduino Mega.
    + SCL: Pin SCL (21) en Arduino Mega.

**Paso 4: Sensor de Humedad**
- Inserta el sensor en la tierra de la planta para medir la humedad del sustrato.
- Conecta sus pines:
    + VCC: Pin de 5V en Arduino Mega.
    + GND: Pin GND en Arduino Mega.
    + Signal: Pin analógico A0 en Arduino Mega.

**Paso 5: Sensor Táctil Capacitivo TTP223**
- Coloca el sensor en un lugar accesible para la interacción del usuario.
- Conecta sus pines:
    + VCC: Pin de 5V en Arduino Mega.
    + GND: Pin GND en Arduino Mega.
    + Signal: Pin digital (D2, por ejemplo) en Arduino Mega.

**Paso 6: Sensor de Luz**
- Posiciona el sensor donde pueda medir la intensidad lumínica ambiental.
- Realiza las conexiones:
    + VCC: Pin de 5V en Arduino Mega.
    + GND: Pin GND en Arduino Mega.
    + Signal: Pin analógico (A1, por ejemplo) en Arduino Mega.

**Paso 7: Módulo DFPlayer Mini**
- Conecta el módulo al puerto Serial 1 de Arduino Mega:
    + VCC: Pin de 5V en Arduino Mega.
    + GND: Pin GND en Arduino Mega.
    + RX y TX: Pines TX1 y RX1 de Arduino Mega.
- Conecta el altavoz al módulo DFPlayer Mini, asegurando la polaridad correcta.

**Paso 8: Módulo Bluetooth HC-06**
- Conecta el módulo Bluetooth al puerto Serial 2 de Arduino Mega:
    + VCC: Pin de 5V en Arduino Mega.
    + GND: Pin GND en Arduino Mega.
    + RX: Pin TX2 en Arduino Mega.
    + TX: Pin RX2 en Arduino Mega.
- Verifica que las conexiones estén firmes para evitar interrupciones en la comunicación.

### 3. Configuración de Software

**Paso 1: Verificación de Conexiones**
Antes de cargar el código, verifica:

- La bomba de agua está correctamente instalada y orientada.
- Los sensores están posicionados adecuadamente y conectados firmemente.
- Los módulos LCD, DFPlayer y Bluetooth están conectados según el esquema.

**Paso 2: Carga del Código**
- Abre el Arduino IDE en tu computadora.
- Carga el código proporcionado:
    + Selecciona el modelo de placa (Arduino Mega 2560).
    + Configura el puerto correspondiente.
    + Compila y sube el programa al Arduino Mega.
    + Observa el funcionamiento inicial de los componentes.

### 4. Configuración de la Aplicación Móvil
***Descarga e instalación:*** Descarga la aplicación desde el enlace proporcionado.

***Emparejamiento Bluetooth:***
- Activa el Bluetooth en tu dispositivo móvil.
- Busca el módulo HC-06 y realiza el emparejamiento.
- Confirma la conexión en la aplicación.
- Configura las funciones de la planta interactiva directamente desde la aplicación.

### 5. Validación Final
- Pruebas funcionales:
- Verifica el funcionamiento de la bomba al detectar baja humedad.
- Observa las lecturas de los sensores en la pantalla LCD.
- Testea la interacción táctil y la respuesta del sistema.
- Comprueba la emisión de sonido mediante el DFPlayer Mini.
- Ajustes finales: Realiza correcciones necesarias en la instalación o en el código.

Siguiendo todos estos pasos correctamente, tendras instalado el prototipo "Tu amiga Titi"
