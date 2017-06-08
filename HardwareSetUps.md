#Paramètres du Hardware
## Arduino à travers l'USB (sans Shield)
Si vous n'avez pas de Shield et que votre hardware n'a aucune connectivité, vous pouvez tout de même utiliser Blynk – directement par l'USB :

1. Ouvrez [l'exemple Arduino Serial USB](https://github.com/blynkkk/blynk-library/blob/master/examples/Boards_USB_Serial/Arduino_Serial_USB/Arduino_Serial_USB.ino)
et changez le [Jeton d'Authentification](https://booteille.github.io/blynk-docs-fr/#guide-de-demarrage-guide-de-demarrage-de-lapplication-blynk-4-jeton-dauthentification)

	```cpp
	// Vous pourriez utiliser un Serial Hardware supplémentaire sur les cartes qui en ont (comme le Mega)
	#include <SoftwareSerial.h>
	SoftwareSerial DebugSerial(2, 3); // RX, TX

	#define BLYNK_PRINT DebugSerial
	#include <BlynkSimpleStream.h>

	// Vous devez récupérer le Jeton d'Authentification dans l'application Blynk.
	// Allez dans les Paramètres du Projet (Icône d'écrou).
	char auth[] = "YourAuthToken";

	void setup()
	{
	  // Console de débuggage
	  DebugSerial.begin(9600);

	  // Blynk fonctionnera à travers le Serial
	  Serial.begin(9600);
	  Blynk.begin(auth, Serial);
	}

	void loop()
	{
	  Blynk.run();
	}
	```
2. Lancez le script qui est habituellement situé dans le dossier ```/scripts``` :

 - Windows : ```Mes Documents\Arduino\libraries\Blynk\scripts```
 - Mac	```User$/Documents/Arduino/libraries/Blynk/scripts```


**Sur Windows :**

Ouvrez cmd.exe

Écrivez votre chemin vers le dossier de blynk-ser.bat. Par exemple :

```
cd C:\blynk-library-0.3.1\blynk-library-0.3.1\scripts
```

Lancez le fichier ```blynk-ser.bat```. Par exemple : ```blynk-ser.bat -c COM4``` (où COM4 le port où est connecté votre Arduino)

Et pressez "Entrée", pressez "Entrée" puis pressez "Entrée".

**Sur Linux et Mac**:

Naviguez vers le dossier /scripts. Par exemple :

```
cd User$/Documents/Arduino/libraries/Blynk/scripts
```

  Une fois à l'intérieur du dossier, lancez :

```
user:scripts User$ ./blynk-ser.sh
```

**Attention :** Ne fermez pas une fenêtre de terminal avec un script qui tourne.

Dans certains cas, vous aurez aussi besoin d'exécuter :

```
user:scripts User$ chmod +x blynk-ser.sh
```

Vous pourriez aussi avoir besoin de l'exécuter avec ```sudo```

```
user:scripts User$ sudo ./blynk-ser.sh
```

C'est ce que vous verrez dans l'application Terminal sur Mac (l'adresse d'usbmodem peut être différente) :

```
[ Press Ctrl+C to exit ]
/dev/tty.usbmodem not found.
Select serial port [ /dev/tty.usbmodem1451 ]:
```

Copiez l'adresse du port série : ```/dev/tty.usbmodem1451``` et collez-le à côté:

```
Select serial port [ /dev/tty.usbmodem1451 ]: /dev/tty.usbmodem1451
```

Après avoir pressé "Entrée", vous devriez voir quelque chose de similaire à ceci :

```
Resetting device /dev/tty.usbmodem1451...
Connecting: GOPEN:/dev/tty.usbmodem1451,raw,echo=0,clocal=1,cs8,nonblock=1,ixoff=0,ixon=0,ispeed=9600,ospeed=9600,crtscts=0 <-> openssl-connect:blynk-cloud.com:8441,cafile=/Users/.../server.crt,nodelay
2015/10/03 00:29:45 socat[30438.2046857984] N opening character device "/dev/tty.usbmodem1451" for reading and writing
2015/10/03 00:29:45 socat[30438.2046857984] N opening connection to LEN=16 AF=2 45.55.195.102:8441
2015/10/03 00:29:45 socat[30438.2046857984] N successfully connected from local address LEN=16 AF=2 192.168.0.2:56821
2015/10/03 00:29:45 socat[30438.2046857984] N SSL connection using AES128-SHA
2015/10/03 00:29:45 socat[30438.2046857984] N starting data transfer loop with FDs [3,3] and [4,4]
```

<span style="color:#D3435C;">**NOTE :** Arduino IDE peut se plaindre avec "programmer is not responding" (Le programmeur ne répond pas, en anglais). Vous devez terminer vos scripts avant de téléverser de nouveaux croquis.</span>

Contenus additionnels :

- [Tutoriel: Contrôler Arduino via l'USB avec l'Application Blynk. Aucun Shield Nécessaire. (Mac OS) - En Anglais](https://www.youtube.com/watch?v=fgzvoan_3_w)
- [Comment contrôler Arduino (Sans fil) avec Blynk via l'USB. Windows - En Anglais](https://www.youtube.com/watch?v=I_hgIj2FdPI)
- [Instructables : Contrôlez Arduino avec Blynk à travers l'USB - En Anglais](http://www.instructables.com/id/Control-arduino-using-Blynk-over-usb/)


## Raspberry Pi
1. Connectez votre Rapsberry Pi à Internet et ouvrez sa console.
2. Lancez cette commande (elle met à jour les dépôts de paquets de votre système d'exploitation pour y inclure les paquets requis) :

	```
	curl -sL "https://deb.nodesource.com/setup_6.x" | sudo -E bash -
	```

3. Téléchargez et créez la bibliothèque Blynk JS en utilisant npm :

	```
	sudo apt-get update && sudo apt-get upgrade
	sudo apt-get install build-essential
	sudo npm install -g npm
	sudo npm install -g onoff
	sudo npm install -g blynk-library
	```

4. Lancez le script de test de Blynk (Mettez votre Jeton d'Authentification):

	```
	blynk-client 715f8cafe95f4a91bae319d0376caa8c
	```

5. Vous pouvez écrire votre propre script basé sur les [exemples](https://github.com/vshymanskyy/blynk-library-js/tree/master/examples)

6. Pour activer l'auto-redémarrage de Blynk pour Pi, trouvez le fichier ```/etc/rc.local``` et ajoutez avant ```exit 0``` :

	```
	node chemin_complet_vers_votre_script.js <Auth Token>
	```

Contenus Supplémentaires:

- [Instructables: Blynk avec Javascript pour Raspberry Pi, Intel Edison et autres - En Anglais](http://www.instructables.com/id/Blynk-JavaScript-in-20-minutes-Raspberry-Pi-Edison)
- [Instructables: Utiliser les capteurs DHT11/DHT12 avec Raspberry Pi et Blynk - En Anglais](http://www.instructables.com/id/Raspberry-Pi-Nodejs-Blynk-App-DHT11DHT22AM2302/?ALLSTEPS)

**Note :** Plutôt que d'utiliser Node.js, vous pouvez aussi créer une installation de la version C++ de la bibliothèque (La même que Arduino, basé sur WiringPI) :
- [LISEZ-MOI de la Bibliothèque pour Linux - En Anglais](https://github.com/blynkkk/blynk-library/blob/master/linux/README.md)
- [Sujet de la Communauté Blynk : Comment faire avec Raspberry-Pi - En Anglais](http://community.blynk.cc/t/howto-for-raspberry-pi/332)
- [Tutoriel Vidéo - Configurer Blynk et Raspberry Pi - En Anglais](https://www.youtube.com/watch?v=iSG_8g6KyGE)
<iframe width="200" height="110" src="https://www.youtube.com/embed/iSG_8g6KyGE" frameborder="0" allowfullscreen></iframe>

## ESP8266 Standalone

Vous pouvez faire tourner Blynk directement sur l'ESP8266 !

Installez la dernière version de la bibliothèque ESP8266 pour Arduino en utilisant [ce guide](https://github.com/esp8266/Arduino#installing-with-boards-manager).

**Croquis d'exemple : Sketch** [ESP8266_Standalone](https://github.com/blynkkk/blynk-library/blob/master/examples/Boards_WiFi/ESP8266_Standalone/ESP8266_Standalone.ino)

Contenus supplémentaires:

- [Instructables: ESP8266 ESP-12(Standalone)+ Blynk - En Anglais](http://www.instructables.com/id/ESP8266-ESP-12Standalone-Blynk-101)
- [Instructables: ESP8266-12 standalone Blynk lm35 capteur de température - En Anglais](http://www.instructables.com/id/ESP8266-12-blynk-wireless-temperature-LM35-sensor/?ALLSTEPS)

[Tutoriel étape-par-étape - En Russe](http://esp8266.ru/esp8266-blynk)

## NodeMCU

Suivez [ces instructions détaillées](https://github.com/blynkkk/blynk-library/tree/master/examples/Boards_WiFi/NodeMCU#instruction-for-nodemcu-setup).
Ou visionnez [ce tutoriel vidéo - En Anglais](https://www.youtube.com/watch?v=FhS44hGk1Lc).

## Particle
Blynk fonctionne avec la famille entière de produits Particle : Core, Photon et Electron

1. Ouvrez l'[IDE Particle Web](https://build.particle.io/build).
2. Allez dans les bibliothèques.
3. Recherchez **Blynk** dans les bibliothèques de la Communauté et cliquez dessus
4. Ouvrez l'exemple ```01_PARTICLE.INO```
5. Cliquez sur "utiliser cet exemple"
6. Mettez votre Jeton d'Authentification ici : ``` char auth[] = "YourAuthToken";``` et flashez le Particle !

Vous pouvez scanner ce code QR à partir de l'application Blynk et vous obtiendrez un projet prêt-à-tester pour **Particle Photon**. Mettez simplement votre Jeton d'Authentification dans l'exemple ```01_PARTICLE.INO```.
<img src="images/Particle Demo1530733075.png" style="width: 300px; height:300px"/>

Contenus supplémentaires:

- [Particle core + DHT22 - En Anglais](https://www.hackster.io/gusgonnet/temperature-humidity-monitor-with-blynk-7faa51)
