---
layout: post
title: Utiliser un Raspberry Pi pour programmer un AVR
---

![]({{ site.baseurl }}/images/piATmega-500.jpg)

## Installation de avrdude

Une fois votre AVR connecté on va pouvoir installer avrdude : 

### Prérequis 

```
pi@raspberrypi ~ $ sudo apt-get update
pi@raspberrypi ~ $ sudo apt-get install bison flex -y
```

### Installation 
On installe avrdude depuis les sources :

```
pi@raspberrypi ~ $ wget http://download.savannah.gnu.org/releases/avrdude/avrdude-6.2.tar.gz
pi@raspberrypi ~ $ tar xfv avrdude-6.2.tar.gz
pi@raspberrypi ~ $ cd avrdude-6.2/
```

On active `linuxgpio` dans les sources de avrdude puis on l'installe :

```
pi@raspberrypi avrdude-6.2/~ $ ./configure – -enable-linuxgpio
pi@raspberrypi avrdude-6.2/~ $ make
pi@raspberrypi avrdude-6.2/~ $ sudo make install
```

On dois spécifier a avrdude quel GPIO utiliser :

```
pi@raspberrypi avrdude-6.2/~ $ sudo nano /usr/local/etc/avrdude.conf
```

On va chercher `linuxgpio` : 

```
#programmer
#  id    = "linuxgpio";
#  desc  = "Use the Linux sysfs interface to bitbang GPIO lines";
#  type  = "linuxgpio";
#  reset = ?;
#  sck   = ?;
#  mosi  = ?;
#  miso  = ?;
#;
```

Et on le remplace par :

```
programmer
  id    = "linuxgpio";
  desc  = "Use the Linux sysfs interface to bitbang GPIO lines";
  type  = "linuxgpio";
  reset = 4;
  sck   = 11;
  mosi  = 10;
  miso  = 9;
;
```

### Test 
On va maintenant tester la communication : 

```
sudo avrdude -c linuxgpio -p atmega8 -v 
```

**-c** Type de programmateur
**-p** Modèle de puce 
**-v** Mode verbeux 

## Premier programme 

Il est tout à fait possible de programmer un AVR ou même un Arduino sans l'IDE grâce à Arduino-MK :

```
pi@raspberrypi ~ $ sudo apt install arduino-mk
```

On va ainsi crée un programme de base : 

```
pi@raspberrypi ~ $ mkdir blink
pi@raspberrypi ~ $ cd blink
pi@raspberrypi blink/~ $ vi blink.ino
```

On va ajouter ce programme :
```
#include <Arduino.h>

void setup() {
  pinMode(13, OUTPUT);     
}

void loop() {
  digitalWrite(13, HIGH);
  delay(500);
  digitalWrite(13, LOW);
  delay(100);
}
```

On va maintenant crée un Makefile :
```
include /usr/share/arduino/Arduino.mk
MCU = atmega48
F_CPU = 1000000L
ARDUINO_DIR = /usr/share/arduino
ARDUINO_LIBS =
```

On compile :

```
pi@raspberrypi blink/~ $ make
```

Un dossier `build-uno` est créé à la compilation et contient un fichier `.hex`

```
pi@raspberrypi blink/~ $ sudo avrdude -c linuxgpio -p atmega8 -v -U flash:w:build-uno/blink.hex:i
```

## Ajout de librairie : 

Ajouter ceci dans le Makefile pour ajouter des librairie :
```
USER_LIB_PATH += /home/my_username/my_libraries_directory
ARDUINO_LIBS += Wire \
                SPI \
                my_custom_lib
```

# Troubleshoot :

## Erreur GPIO
```
Can't export GPIO 4, already exported/busy?: Device or resource busy
```
### Fix : 
```
echo 4 > /sys/class/gpio/unexport 
```
## Exemple de fuse 

### Pour un atmega 48 en internal crystal 8mhz
sudo avrdude -c linuxgpio -p atmega48 -U lfuse:w:0xe2:m -U hfuse:w:0xdf:m -U efuse:w:0xff:m

### Pour un atmega 8 en internal crystal 8mhz 
sudo avrdude -c linuxgpio -p m8 -U lfuse:w:0xe4:m -U hfuse:w:0xd9:m 