# PRACTICA-10-ENCENDER-LED-NodeRed

Este repositorio se muestra el flow 3 del diplomado automatizaion industrial y mecatronica , en la cual se utilizo NodeRed y WOKWI

## Introducción

El flow 3 representa el segundo ejercicio a realizar con NodeRed. Este ejercicio consiste únicamente en hacer conexion a un servidor publico y encendidio de un led.

### Descripción

La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica solo ocuparemos un led; Esta practica se usara un simulador llamado [WOKWI](https://wokwi.com/), junto con NodeRed.


## Material Necesario

Para realizar este flow necesitas lo siguiente

- [Node.js](https://nodejs.org/en)
- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- Led
- Resistencia
  


## Instrucciones

### Requisitos previos

Para que este flow funcione, debes cumplir con los siguientes requisitos previos
1. Para realizar la practica de este repositorio se necesita entrar a la plataforma [WOKWI](https://https://wokwi.com/).
2. Tener instalado Node.js (version 20.11.0 LTS).

### Instrucciones de preparación del entorno en wokwi

1. Abrir la terminal de programación y colocar la siguente codigo:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "18.193.219.109";
const int mqttPort = 1883;
const char* mqttUser = "diegojm";
const char* mqttPassword = "1234";
const char* mqttTopic = "yasminzagall";

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

2. Instalar la libreria de **PubSubClient**. 
   - Seleccionar pestaña de Librery Manager --> Add a New library --> Colocamos el nombre de libreria
 ![](https://github.com/YasminZagal/PRACTICA-10-ENCENDER-LED-NodeRed/blob/main/libreria%20led.png)
  
3. Realizar la conexion del **LED** con la **ESP32** de la siguiente manera.
 ![](https://github.com/YasminZagal/PRACTICA-10-ENCENDER-LED-NodeRed/blob/main/conexión%20led.png)
     

### Instrucciones de preparación del entorno en NodeRed

Para ejecutar este flow, es necesario lo siguiente
1. Arrancar el contenedor de NodeRed en **cmd** con el comando
        
        node-red

2. Dirigirse a [localhost:1880](localhost:1880)

3. En la parte izquierda de la pantalla seleccionaremos
   - mqtt out
   - switch

 ![](https://github.com/YasminZagal/PRACTICA-10-ENCENDER-LED-NodeRed/blob/main/nodos.png)
  
4. Para la configuracion del mqtt out necesitaremos saber nuestra ip, que se saca de la siguiente manera:
   cmd --> nslookup broker.hivemq.com --> copiamos los numeros de la parte que dice addresses --> nos dirigimos a nodered --> seleccionamos mqtt in --> damos click en el icono del lapiz --> en la parte de server se pegara la direccion ip que copiamos --> update --> done

![](https://github.com/YasminZagal/PRACTICA-N-8-NodeRed-CON-DHT22/blob/main/direccion%20ip.png)   

![](https://github.com/YasminZagal/PRACTICA-N-8-NodeRed-CON-DHT22/blob/main/conf1.png)  

5. En la configuracion del **SWITCH** lo seleccionamos con doble click 

![](hhttps://github.com/YasminZagal/PRACTICA-10-ENCENDER-LED-NodeRed/blob/main/configuracion%20switch.png)


   
### Instrucciones de operación
1. Nos vamos a nuestro wokwi en donde nuestro simulador ya esta corriendo
![](hhttps://github.com/YasminZagal/PRACTICA-10-ENCENDER-LED-NodeRed/blob/main/resultadoo.png)

## Resultados

A continuación se puede observar una vista previa del resultado del flow 3 y la interaccion con el wokwi .

![](https://github.com/YasminZagal/PRACTICA-10-ENCENDER-LED-NodeRed/blob/main/resultaddo.png)



