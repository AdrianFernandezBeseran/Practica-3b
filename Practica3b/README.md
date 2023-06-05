# Practica 3B

En esta parte de la practica utilizaremos la multitarea para encender leds y apagarlos con tareas sincronizadas 
# Código 

```cpp
#include <Arduino.h>

void LED_ON (void * pvParameters);
void LED_OFF(void * pvParameters);

int LED = 2; 
int LED2 = 19; 

void setup(){

Serial.begin(115200);
pinMode(LED, OUTPUT);
pinMode(LED2, OUTPUT);
xTaskCreate(LED_ON, "ON", 10000, NULL, 1, NULL);
xTaskCreate(LED_OFF, "OFF", 10000, NULL, 1, NULL);
}

void loop(){}

void LED_ON (void * pvParameters){
    for(;;){
        digitalWrite(LED, HIGH);
        Serial.println("a");
        vTaskDelay(1000);
        digitalWrite(LED, LOW);
        vTaskDelay(1000);
    }
}

void LED_OFF (void * pvParameters){
    for(;;){
        digitalWrite(LED2, HIGH);
        Serial.println("b");
        vTaskDelay(500);
        digitalWrite(LED2, LOW);
        vTaskDelay(500);
    }
}
```

**Declaraciones**
```cpp
#include <Arduino.h>

void LED_ON (void * pvParameters);
void LED_OFF(void * pvParameters);

int LED = 2; 
int LED2 = 19;
```

Aqui declararemos las tareas de LED_ON y LED_OFF y los pines utilizados para los respectivos LED (2,19).

**SETUP**


```cpp
void setup(){

Serial.begin(115200);
pinMode(LED, OUTPUT);
pinMode(LED2, OUTPUT);
xTaskCreate(LED_ON, "ON", 10000, NULL, 1, NULL);
xTaskCreate(LED_OFF, "OFF", 10000, NULL, 1, NULL);
}
```
En el Setup inicializamos el Serial, configuramos los pines de los Led en modo OUTPUT para poder encenderlos y apagarlos.
Y finalmente iniciamos las tareas de OFF y ON con sus respectivos parametros 

**LOOP**
```cpp
void loop(){}
```
En este programa como hacemos uso de dos tareas independientes de la tarea principal no ejecutamos nada

**TAREA 1**
```cpp
void LED_ON (void * pvParameters){
    for(;;){
        digitalWrite(LED, HIGH);
        Serial.println("a");
        vTaskDelay(1000);
        digitalWrite(LED, LOW);
        vTaskDelay(1000);
    }
}
```
En esta tarea ultilizamos un for vacio para que se quede infinitamente ejecutando las acciones en este caso encendiendo el led y enviando una "a" por el monitor serial a modo de control de errores y atraves de la funcion vTaskDelay pausaremos la tarea durante 1s para despues volver Apagar el led


**Tarea 2**

```cpp
void LED_OFF (void * pvParameters){
    for(;;){
        digitalWrite(LED2, HIGH);
        Serial.println("b");
        vTaskDelay(500);
        digitalWrite(LED2, LOW);
        vTaskDelay(500);
    }}
```



 Esta tarea hace lo mismo que la tarea 1 pero ahora con otro LED(LED2) tambien enviaremos un mensaje de control de error ("b") en este caso la tarea tiene un delay de 500ms por lo tanto el led cambiara de estado el doble de rapido que en la tarea 1 mientras se ejecutan a la vez.

 ![ab](ab.png)

 VIDEO: https://drive.google.com/file/d/15JgKDRWshNJyNnsOy-cnfe21hp883ZA0/view?usp=sharing

 