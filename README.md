# PRACTICA7

## Codi sencer
```cpp
#include <Arduino.h>
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup()
{
  Serial.begin(115200);

  audioLogger = &Serial;
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(10,9,1);
  aac->begin(in, out);
}


void loop()
{
  
  if (aac->isRunning()) {
    aac->loop();
  } else {
    Serial.printf("AAC done\n");
    delay(1000);
  }
}
```

## Explicació del codi per parts
### Biblioteques
```cpp
#include <Arduino.h>
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"
```
- #include <Arduino.h>: Inclou la llibreria bàsica d'Arduino.
- #include "AudioGeneratorAAC.h": Inclou la llibreria que permet generar àudio a partir d'un fitxer AAC.
- #include "AudioOutputI2S.h": Inclou la llibreria per a la sortida d'àudio mitjançant el protocol I2S.
- #include "AudioFileSourcePROGMEM.h": Inclou la llibreria per utilitzar fitxers d'àudio emmagatzemats a la memòria del programa (PROGMEM).
- #include "sampleaac.h": Inclou el fitxer d'àudio AAC emmagatzemat a PROGMEM.

### Declaració de variables globals
```cpp
AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;
```
- AudioFileSourcePROGMEM *in;: Declara un punter a un objecte AudioFileSourcePROGMEM, que s'utilitzarà per llegir el fitxer d'àudio des de la memòria del programa.
- AudioGeneratorAAC *aac;: Declara un punter a un objecte AudioGeneratorAAC, que s'utilitzarà per descomprimir i generar l'àudio AAC.
- AudioOutputI2S *out;: Declara un punter a un objecte AudioOutputI2S, que s'utilitzarà per enviar l'àudio a un dispositiu de sortida mitjançant I2S.

### Setup
```cpp
void setup()
{
  Serial.begin(115200);

  audioLogger = &Serial;
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out->SetGain(0.125);
  out->SetPinout(10, 9, 1);
  aac->begin(in, out);
}
```
- Serial.begin(115200);: Inicia la comunicació sèrie a 115200 bauds.
- audioLogger = &Serial;: Assigna el logger d'àudio al port sèrie per a la sortida de missatges de depuració.
- in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));: Crea un nou objecte AudioFileSourcePROGMEM amb el fitxer AAC emmagatzemat a sampleaac.
- aac = new AudioGeneratorAAC();: Crea un nou objecte AudioGeneratorAAC.
- out = new AudioOutputI2S();: Crea un nou objecte AudioOutputI2S.
- out->SetGain(0.125);: Estableix el guany de sortida a 0.125.
- out->SetPinout(10, 9, 1);: Estableix els pins de sortida I2S (10 per al bit clock, 9 per al word select i 1 per a les dades).
- aac->begin(in, out);: Comença la generació d'àudio AAC utilitzant el fitxer in i la sortida out.

### Loop
```cpp
void loop()
{
  if (aac->isRunning()) {
    aac->loop();
  } else {
    Serial.printf("AAC done\n");
    delay(1000);
  }
}
```
- if (aac->isRunning()) { aac->loop(); }: Si l'objecte aac està funcionant, crida la funció loop() de aac per continuar la reproducció de l'àudio.
- else { Serial.printf("AAC done\n"); delay(1000); }: Si aac ha acabat de reproduir l'àudio, imprimeix "AAC done" al monitor sèrie i espera un segon abans de continuar.
