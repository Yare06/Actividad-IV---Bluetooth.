# Actividad-IV---Bluetooth.

# README – Actividad IV: Gateway Bluetooth–WiFi con ESP32 y MQTT

## 1. Objetivo General    
Diseñar un gateway que integre **BLE AT-09**, **ESP32** y **MQTT** para comunicación **JSON** con indicadores **LED**.

## 2. Objetivos Específicos  
- Configurar la comunicación bidireccional entre un módulo Bluetooth BLE y un dispositivo móvil mediante ESP32.  
- Implementar una máquina de estados asíncrona para gestionar automáticamente los enlaces Bluetooth y WiFi.  
- Formatear mensajes en JSON utilizando la librería ArduinoJSON, asegurando la validación del campo `"id"`.  
- Publicar y suscribirse a un broker MQTT público, transmitiendo y recibiendo tramas JSON.  
- Implementar un sistema de alertas visuales mediante LEDs que indiquen estados de conexión y validez de mensajes.  
- Controlar un LED violeta/rosa en función de comandos `"accion"` recibidos en los mensajes JSON.  
- Registrar todos los eventos de envío y recepción en el broker para monitoreo.  

## 3. Competencias  
- Configuración de módulos BLE y ESP32.  
- Manejo de comunicación inalámbrica Bluetooth y WiFi.  
- Implementación de protocolos IoT mediante MQTT.  
- Programación estructurada y asíncrona con máquinas de estados.  
- Uso de la librería ArduinoJSON para serialización y deserialización.  
- Diseño de indicadores visuales con LEDs para monitoreo de eventos.  

## 4. Tabla de Contenidos  

1. [Objetivo General](#1-objetivo-general)  
2. [Objetivos Específicos](#2-objetivos-específicos)  
3. [Competencias](#3-competencias)  
4. [Tabla de Contenidos](#4-tabla-de-contenidos)  
5. [Descripción](#5-descripción)  
6. [Requisitos](#6-requisitos)  
   - [Hardware necesario](#61-hardware-necesario)  
   - [Software y bibliotecas requeridas](#62-software-y-bibliotecas-requeridas)  
   - [Conocimientos previos imprescindibles](#63-conocimientos-previos-imprescindibles)  
7. [Instalación y Configuración](#7-instalación-y-configuración)  
8. [Conexiones de Hardware](#8-conexiones-de-hardware)  
9. [Parámetros Técnicos del AT-09](#9-parámetros-técnicos-del-at-09)  
10. [Uso y ejemplos de Código](#10-uso-y-ejemplos-de-código)  
11. [Resultados de Prueba](#11-resultados-de-prueba)  
12. [Consideraciones Éticas y de Seguridad](#12-consideraciones-éticas-y-de-seguridad)  
13. [Formato de Salida (JSON)](#13-formato-de-salida-json)  
14. [Solución de Problemas](#14-solución-de-problemas)  
15. [Contribuciones](#15-contribuciones)  
16. [Referencias](#16-referencias)  
 

## 5. Descripción  
Esta práctica consiste en la creación de un entorno inteligente que conecta Bluetooth y WiFi mediante un ESP32 actuando como gateway IoT. El módulo BLE AT-09 permite la comunicación con un dispositivo móvil, mientras que el ESP32 transmite y recibe datos en formato JSON hacia un broker MQTT público. Los mensajes son procesados, filtrados y visualizados mediante indicadores LED que reflejan el estado del sistema, garantizando robustez, bajo consumo y monitoreo en tiempo real.  

## 6. Requisitos  

### 6.1 Hardware necesario  
- Laptop o equipo de cómputo.  
- ESP32 (shield de 30, 32, 36 o 38 pines con cable USB).  
- Módulo BLE AT-09 (o HM-10).  
- 7 LEDs (rojo, azul, verde, amarillo, blanco, naranja y violeta/rosa).  
- 7 resistencias según el color del LED.  
- Protoboard (400 u 830 puntos).  
- Cables puente (HxH, HxM o MxM).  

### 6.2 Software y bibliotecas requeridas  
- Arduino IDE (última versión).  
- Librería **ArduinoJSON**.  
- Cliente MQTT (ejemplo: **PubSubClient** para Arduino).  
- Aplicación móvil **Serial Bluetooth Terminal** o equivalente.  
- Broker MQTT público (ejemplo: `test.mosquitto.org`).  

### 6.3 Conocimientos previos imprescindibles  
- Fundamentos de comunicación serial y protocolos inalámbricos.  
- Programación en C/C++ para microcontroladores.  
- Uso básico de Arduino IDE.  
- Conceptos de IoT, JSON y MQTT.  

## 7. Instalación y Configuración  
1. Clonar este repositorio en tu equipo.  
2. Abrir el proyecto en **Arduino IDE**.  
3. Instalar las siguientes librerías desde el **Library Manager**:  
   - ArduinoJSON  
   - PubSubClient  
4. Conectar el ESP32 vía USB y seleccionar el puerto correspondiente en Arduino IDE.  
5. Configurar en el código:  
   - Credenciales WiFi (SSID y contraseña).  
   - Dirección del broker MQTT y el tópico de publicación/suscripción con las iniciales del equipo.  
   - Pines asociados a cada LED.  
6. Subir el código al ESP32.  
7. Emparejar el módulo AT-09 con el smartphone mediante la aplicación **Serial Bluetooth Terminal**.  
8. Probar la comunicación enviando mensajes JSON y verificar la correcta publicación en el broker MQTT.  

## 8. Conexiones de Hardware


| Señal del módulo | Pin de la placa | Función        |
|------------------|-----------------|----------------|
| VCC (AT-09)      | 3.3V / 5V       | Alimentación   |
| GND (AT-09)      | GND             | Tierra         |
| TX  (AT-09)      | GPIO16 (RX2)    | Transmisión    |
| RX  (AT-09)      | GPIO17 (TX2)    | Recepción      |
| LED_ROJO         | GPIO26          | Salida Digital |
| LED_AZUL         | GPIO27          | Salida Digital |
| LED_VERDE        | GPIO4           | Salida Digital |
| LED_AMARILLO     | GPIO5           | Salida Digital |
| LED_BLANCO       | GPIO18          | Salida Digital |
| LED_NARANJA      | GPIO19          | Salida Digital |
| LED_VIOLETA      | GPIO22          | Salida Digital |
| STATE            | GPIO23          | Estado BLE     |

## 9. Parámetros Técnicos del AT-09

| Componente           | Voltaje de operación | Corriente típica | Interfaz        | Frecuencia             |
|----------------------|----------------------|------------------|-----------------|------------------------|
| ESP32                | 3.3V                 | 160–500 mA       | UART, WiFi, BLE | CPU hasta 240 MHz      |
| Módulo BLE AT-09     | 3.3V – 5V            | ~40 mA (máx)     | UART            | 2.4 GHz (BLE 4.0)      |
| LED (cada color)     | 2V – 3.3V (aprox.)   | 10–20 mA         | GPIO            | —                      |
| Pin STATE (GPIO23)   | 3.3V                 | <10 mA           | Entrada digital | Estado conexión BLE    |

## 10. Uso y ejemplos de Código

Para este proyecto en Esp32 el archivo principal es con extension .ino, donde no se usaron mas librerias localespara la ejecucion del codigo.
Para realizar la ejecución del codigo simplemente es necesario tener un archivo "main.ino" (tener las librerias en arduino) y pegar el codigo del github
o en caso consecuente de hacer un git clone, es simplemente necesario tener las librerias en el arduino ide y verificar que el archivo se llame "main.ino".
Para realizar el copy paste del codigo tenemos que entender que de base en archivos .ino, deberemos poner 2 funciones: loop() y setup(), donde en setup() declararemos la configuracion inicial que se ejecuta una sola vez, y en loop() donde se repite constantemente.
Si queremos realizar comentarios cortos de una linea usaremos las barras diagonales: // [Comentario aqui]
pero si queremos que nuestro comentario sea extenso podemos usar : 
    /*
    [Comentario extenso aqui de varias lineas]
    */ 
    
## 11. Resultados de Prueba
Durante las pruebas y ejecución del proyecto es importante **registrar los resultados obtenidos**.  
Esto incluye:

- **Lecturas de sensores** (valores de temperatura, estado de pines, etc.).
- **Mensajes enviados** (comandos hacia el módulo BLE, confirmaciones, estados).
- **Eventos del sistema** (conexión/desconexión, errores, advertencias).

Los resultados deben documentarse indicando claramente **dónde se generan**:

| Medio de salida     | Ejemplo de uso                                  |
|---------------------|-------------------------------------------------|
| **Consola serial**  | `Serial.println("LED encendido");`              |
| **Broker MQTT**     | Publicación de estado: `topic/led/state -> ON`  |
| **Archivo de log**  | Guardar datos en SD, SPIFFS o en servidor web   |




## 12. Consideraciones Éticas y de Seguridad

***PRIVACIDAD DE DATOS***

* Exposición de información personal si los datos transmitidos no están cifrados.
* Riesgo de rastreo por direcciones MAC fijas (tracking de dispositivos).
* Posible acceso no autorizado a información sensible almacenada en el dispositivo.
* Problemas de confidencialidad si se comparten datos de usuarios en claro.
* Falta de consentimiento informado en algunas aplicaciones al recolectar información vía Bluetooth.

***POSIBLES VULNERABILIDADES***

* **BlueBorne Attack**: explotación de vulnerabilidades en el protocolo Bluetooth para ejecución remota de código.
* **Bluesnarfing:** Una persona no autorizada accede a la información de un dispositivo con Bluetooth sin el consentimiento del propietario.
* **Insecto azul:** Permite a los atacantes tomar el control del dispositivo de la víctima.
* **Ataques de intermediario (MITM):** Un atacante intercepta la comunicación entre dos dispositivos Bluetooth y escucha o altera la información que se intercambia.
* **Suplantación de dispositivo Bluetooth:** Un atacante se hace pasar por un dispositivo Bluetooth confiable para engañar a los usuarios y lograr que se conecten.
* **Protocolos Bluetooth obsoletos o inseguros:** Las versiones antiguas de Bluetooth tienen un cifrado más débil y medidas de seguridad obsoletas que los atacantes pueden explotar fácilmente. 
* **Error humano:** Se produce cuando los empleados dejan sus dispositivos en modo visible o se conectan a dispositivos inseguros sin percatarse de los riesgos.



***MEDIDAS DE MITIGACIÓN***

* Usar cifrado fuerte (AES, ECC) en la comunicación.
* Implementar Bluetooth Secure Simple Pairing (SSP) o métodos modernos de autenticación.
* Configurar rotación de direcciones MAC aleatorias para evitar rastreo.
* Aplicar actualizaciones regulares de firmware para cerrar vulnerabilidades conocidas.
* Limitar el alcance de transmisión del dispositivo para reducir la exposición.
* Restringir permisos de aplicaciones que usan Bluetooth (principio de mínimo privilegio).
* Monitorizar intentos de conexión no autorizados.
* Educar a los usuarios sobre la importancia de apagar Bluetooth cuando no se utilice.

#### 

## 13. Formato de Salida (JSON)



**Salida JSON**



{

&nbsp; "id": "SYCRMLAJLJ",

&nbsp; "origen": "Consola serial",

&nbsp; "accion": "ON",

&nbsp; "fecha": "05-09-2025",

&nbsp; "hora": "15:42:10"

}



**| Campo    | Tipo de dato         | Ejemplo                                       | Descripción                                                                                              |**

**| ------- | --------------------- | --------------------------------------------- | ---------------------------------------------------------------- |**

**| id      |**  string               **| `"**SYCRMLAJLJ**"`                                |** Identificador único del equipo/dispositivo. Permite validar que el mensaje pertenece al equipo correcto.

										                                                                        

**| origen  |**  string               **|** `"Consola serial"` / `"Smartphone"` / `"MQTT" **|** Medio desde el que se generó el mensaje.                                                                 **|**

**| accion  |**  string               **|** `"ON"` / `"OFF"`                              **|** Controla el encendido o apagado de elementos (ej. LED violeta).

**| fecha   |**  string (DD-MM-YYYY)` **|** `"05-09-2025"`                                **|** Fecha en formato \*\*día-mes-año\*\* obtenida desde NTP.                                                     **|**

**| hora    |** `string (HH:MM:SS)`   **|** `"15:42:10"`                                  **|** Hora en formato \*\*24h\*\* obtenida desde NTP.      





## 14. Solución de Problemas

                                                        

1. ***El ESP32 no detecta el módulo BLE***



* Conexiones TX/RX invertidas o mal cableadas.
* No hay GND común entre ESP32 y el módulo.
* Baudrate distinto al esperado (9600).



***2. El módulo no responde a los comandos AT***



* Uso de un firmware distinto (HM-10 en lugar de AT-09).
* Configuración incorrecta en el puerto serie.



***3. El celular no logra emparejarse con el módulo***



* Se intenta usar Bluetooth clásico en vez de BLE.
* La app no es compatible con AT-09.
* Nombre o visibilidad del módulo mal configurados.



***4. Los LEDs indicadores no muestran el estado correcto***



* Errores en la máquina de estados del código.
* Variables de conexión no actualizadas.
* LEDs cableados en pines distintos a los definidos.



***5. Mensajes JSON inválidos o con ID incorrecto***



* El campo "id" no coincide con el del equipo.
* El JSON llega mal formateado o incompleto.

***6. El LED violeta no responde a ON/OFF***

* El campo "accion" no llega en el JSON o está mal escrito.
* No se validó correctamente antes de ejecutar la acción.
  

***7. No se publican/reciben mensajes en el broker MQTT***

* Error en la conexión Wi-Fi (SSID/contraseña incorrectos).
* mqtt.connected() nunca llega a establecerse.
* Topics mal escritos o sin el sufijo correcto (\_TX y \_RX).


## 16. Referencias

- Arduino. (s. f.). *ArduinoJSON library*. Recuperado el 5 de septiembre de 2025, de [https://arduinojson.org](https://arduinojson.org)  

- Espressif Systems. (2023). *ESP32 Series Datasheet*. Espressif Systems. Recuperado de [https://www.espressif.com/en/products/socs/esp32](https://www.espressif.com/en/products/socs/esp32)  

- Eclipse Foundation. (s. f.). *MQTT: The Standard for IoT Messaging*. Recuperado de [https://mqtt.org](https://mqtt.org)  

- Jakar, B. (2020). *Getting started with Bluetooth Low Energy (BLE) and ESP32*. Instructables. Recuperado de [https://www.instructables.com](https://www.instructables.com)  

- PubSubClient. (s. f.). *MQTT client library for Arduino*. GitHub. Recuperado de [https://github.com/knolleary/pubsubclient](https://github.com/knolleary/pubsubclient)  

- Texas Instruments. (2013). *Bluetooth® Low Energy overview*. Recuperado de [https://www.ti.com/bluetooth-low-energy](https://www.ti.com/bluetooth-low-energy)  

- Universidad de Colima. (2025). *Guía de actividades de laboratorio IV.1.* Documento académico interno, Facultad de Ingeniería Mecánica y Eléctrica.  

- Villanueva, M. S. (2024, noviembre 7). 8 common Bluetooth risks and how to mitigate them. Itsasap.com. https://www.itsasap.com/blog/bluetooth-risks-prevention
  
- Armis Security. (2017). The BlueBorne Attack: The dangers of Bluetooth implementations. Disponible en: https://www.armis.com/blueborne

- Bluetooth SIG. (2023). Bluetooth Core Specification 5.4. Disponible en: https://www.bluetooth.com/specifications

- International Telecommunication Union (ITU). (2021). Security aspects of Bluetooth and similar technologies. ITU-T X.1121.

