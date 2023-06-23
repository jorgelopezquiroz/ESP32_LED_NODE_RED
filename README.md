# Encender led con node-red
En este repositorio muestra como podemos programar una ESP32 con un relay module y conexión a internet mediente Node-red para encender un led.

## Introducción

### Descripción
La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un sensor (```Relay module```) que nos servira para encender el led de forma remota; Cabe aclarar que esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/)
 y Node-red para la conexción a Internet.

## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/) como simulador.
- Tarjeta ```ESP 32```.
- Sensor ```Relay module```.
- Node-red


## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/) y tener instalado en tu computadora Node-red y comprobar que funcione de manera correcta.


### Instrucciones de preparación de entorno 

1. Entramos al simulador [WOKWI](https://https://wokwi.com/) y selecionamos la tarjeta ```ESP 32``` como se muestra en la siguiente imagen.

![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/ESP32.png?raw=true)


2. Posteriormente se abrirá la terminal de programación y se coloca la siguente programación:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "44.195.202.69";
const int mqttPort = 1883;
const char* mqttUser = "Jorge1304-led";
const char* mqttPassword = "1234";
const char* mqttTopic = "Jorge1304-led";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}
```
3. Se necesita instalar la libreria  **PubSubClient** en el simulador como se muestra en la siguiente imagen.
![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/Libreria.png?raw=true)
 
4. Hacer la conexion de **Relay module** y con la **ESP32** como se muestra en la siguente imagen.

![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/Diagrama%20Wokwi.png?raw=true)
## Diagrama de flujo en Node-red
5. Empezamos con nuestro mmqtt, elegimos un ***mmqtt out*** como se muestra en la imagen. 
![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/MMQTT%20OUT.jpeg?raw=true)

6. Elegimos nuestro servidor como se muestra en la imagen. 
![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/Servidor.jpeg?raw=true)

7. Realizamos nuestro diagrama de flujo como se muestra en la siguiente imagen.
![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/DIAGRAMA%20Node-red.jpeg?raw=true)

8. Es de gran importancia que nuestro **switch** este configurado  configurado como se muestra en la siguiente imagen.
![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/Configuracion%20switch.jpeg?raw=true)

9. En nuestro **Dashboard** Agregamos un tab en nuestro caso fue **Tab 4** pero puede ser el nombre que gusten y un groupo de nombre **boton** como se muestra en la siguiete imagen.
![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/Dashboard.jpeg?raw=true)
### Instrucciónes de operación

1. Iniciar la simulación.
2. Visualizar el monitor serial que la conexión entre [WOKWI](https://https://wokwi.com/) y Node-red sea establecidad correctamente .
4. Ir a **Node-red** y abrir nuestro dashboard y accionar nuestro switch.

## Resultados

Cuando haya compilado, verás los valores dentro del  en el  monitor serial y en nuestro **Dashboard** podras activar y desactivar el switch  como muestra en las siguente imagenes de evidencias.

## Monitor serial
![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/resultados%20Wokwi.png?raw=true
)



## Evidencias
**ON**

![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/on.png?raw=true)


**OFF**


![](https://github.com/jorgelopezquiroz/ESP32_LED_NODE_RED/blob/main/off.png?raw=true)



# Créditos

Desarrollado por Jorge Esteban Lopez Quiroz

- [GitHub](https://github.com/jorgelopezquiroz)