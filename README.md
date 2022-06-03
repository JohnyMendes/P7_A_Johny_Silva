# Johny Silva Mendes
# PRACTICA 7 : Buses de comunicación III (I2S)
## Ejercicio Practico 1 reproducción desde memoria interna

### Código del programa

```cpp
#include "Arduino.h" 
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup(){
  Serial.begin(9600);

  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);
}

void loop(){
  if (aac->isRunning()) {
    aac->loop();
  } else {
    aac -> stop();
    Serial.printf("Sound Generator\n");
    delay(1000);
  }
}
```

### Funcionamiento 

Primeramente se añaden las librerías necesarias, se añade la librería ESP8266Audio de Earle F.Philhower. A continuación se declaran tres punteros el primero "AudioFileSourcePROGMEM"; su función es leer un archivo de una matriz PROGREM. El segundo puntero "AudioGeneratorAAC"; traga de reproducir un archivo AAC mono o estéreo.Por último el puntero "AudioOutputI2" que envía señales estéreo o mono a cualquier frecuencia prestablecida.

```cpp
#include "Arduino.h" 
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

```

**Setup:**se inicializa el puerto serie, se declara el puntero *in*, declarado previamente, como la variable que leerá el fichero de audio. El *aac* se encargará de la comunicación entre el codificador y la placa. Finalmente, el *out* se encargará de las diferentes configuraciones como la ganancia o pines de salida. 

```cpp
void setup(){
  Serial.begin(9600);

  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);
}
```
**Loop:** comprueba si hay algún error y si se ha leído exisotamente el fichero mediante la sentencia *isRunning*. 

```cpp
void loop(){
  if (aac->isRunning()) {
    aac->loop();
  } else {
    aac -> stop();
    Serial.printf("Sound Generator\n");
    delay(1000);
  }
}
```

### Salida por Terminal 

Cuando se reproduzca el fichero, se mostrará:

```cpp
Sound Generator
```
