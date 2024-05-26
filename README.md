# Practica-4
## Sistema Operatiu en Temps Real (Multitasques en ESP32)
### Codi Complet
```cpp
#include <Arduino.h>

void anotherTask( void * parameter );

void setup() {
  Serial.begin(112500);
  /* we create a new task here */
  xTaskCreate(
    anotherTask, /* Task function. */
    "another Task", /* name of task. */
    10000, /* Stack size of task */
    NULL, /* parameter of the task */
    1, /* priority of the task */
    NULL); /* Task handle to keep track of created task */
}

void loop() {
  Serial.println("this is ESP32 Task");
  delay(1000);
}

void anotherTask( void * parameter ) {
  /* loop forever */
  for(;;)
  {
  Serial.println("this is another Task");
  delay(1000);
  }
/* delete a task when finish, this will never happen because this is infinity loop */
vTaskDelete( NULL );
}
```
### Funcionament per parts del codi

__1. Inclusió de Biblioteques__
```cpp
#include <Arduino.h>

```
- Arduino.h: Biblioteca estàndart d'Arduino
  
__2. Declaració de Funció anotherTask__
```cpp
void anotherTask( void * parameter );
```
Declara una funció que serà utilitzada com una tasca separada en FreeRTOS. Aquesta funció ha de tenir el tipus de retorn void i un únic paràmetre void *

__3. Definició de la Tasca anotherTasks__
```cpp
void anotherTask( void * parameter ) {
  /* loop forever */
  for(;;)
  {
  Serial.println("this is another Task");
  delay(1000);
  }
/* delete a task when finish, this will never happen because this is infinity loop */
vTaskDelete( NULL );
}
```
- for(;;): Bucle infinit.
- Serial.println("this is another Task");: Envia un missatge a través de la comunicació sèrie.
- delay(1000);: Pausa l'execució durant 1000 mil·lisegons (1 segon).
- vTaskDelete(NULL);: Aquesta línia de codi mai s'assolirà perquè el bucle és infinit. En cas que la tasca acabi, aquesta funció eliminaria la tasca actual.

  
__4. Setup__
```cpp
void setup() {
  Serial.begin(112500);
  /* we create a new task here */
  xTaskCreate(
    anotherTask, /* Task function. */
    "another Task", /* name of task. */
    10000, /* Stack size of task */
    NULL, /* parameter of the task */
    1, /* priority of the task */
    NULL); /* Task handle to keep track of created task */
}
```
S'estableix la configuració inicial del programa:
- Serial.begin(115200): Inicialitza la comunicació sèrie en 115200 bauds.
- xTaskCreate(): Crea una nova tasca en FreeRTOS
- anotherTask: Nom de la funció que defineix la tasca.
- "another Task": Nom descriptiu de la tasca (per depuració).
- 10000: Mida de la pila per a la tasca en paraules (32 bits)
- NULL: Paràmetre que es passa a la tasca (en aquest cas, cap).
- 1: Prioritat de la tasca
- NULL: Maneig de la tasca (pot ser utilitzat per controlar la tasca, encara que aquí no s'utilitza).
  
__5. Loop__
```cpp
void loop() {
  Serial.println("this is ESP32 Task");
  delay(1000);
}

```
- Serial.println("this is ESP32 Task");: Envia un missatge a través de la comunicació sèrie.
- delay(1000);: Pausa l'execució durant 1000 mil·lisegons (1 segon).


Resum del funcionament: 

El setup() inicialitza la comunicació sèrie i crea una nova tasca que s'executa en paral·lel amb el bucle principal loop().

La funció loop() imprimeix un missatge cada segon.

La tasca anotherTask també imprimeix un missatge diferent cada segon.

Ambdues funcions (loop() i anotherTask) s'executen de manera concurrent gràcies a FreeRTOS, que permet executar múltiples tasques en paral·lel utilitzant el sistema operatiu en temps real (RTOS).



