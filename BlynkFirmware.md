#Logiciel Blynk
## Configuration

### Blynk.begin()

La manière la plus simple de configurer Blynk est d'appeler `Blynk.begin()` :

```cpp
Blynk.begin(auth, ...);
```
Cette méthode prend de nombreux paramètres pour différents hardwares, en fonction du type de connexion que vous utilisez. Suivez les croquis d'exemple pour votre carte.

`begin()` suit basiquement ces étapes :

1. Se connecte à un réseau (WiFi, Ethernet, ...)
2. Appelle `Blynk.config(...)` - défini le jeton d'authentification, l'adresse de l'hôte
3. Essaie de se connecter au serveur une fois (peut bloquer pour plus de 30s)

Si votre type de connexion/shield n'est pas encore supporté, vous pouvez aisément en créer un vous-même !
[Voici quelques exemples](https://github.com/blynkkk/blynk-library/tree/master/examples/More/ArduinoClient).

### Blynk.config()

`config()` vous permet de gérer la connexion au réseau par vous-même.
Vous pouvez configurer votre shield (WiFi, Ethernet, ...) manuellement, puis appeler :

```cpp
Blynk.config(auth, server, port);
```
ou simplement
```cpp
Blynk.config(auth);
```

**Note :** Juste après `Blynk.config(...)`, Blynk n'est pas encore connecté au serveur.
Il essaiera de se connecter quand il atteindra le premier appel à `Blynk.run()` ou `Blynk.connect()`.
Si vous souhaitez éviter de vous connecter au serveur, appelez simplement `Blynk.disconnect()` juste après la configuration.

Pour paramétrer une connexion WiFi, vous pouvez utiliser `connectWiFi` (juste pour le confort) :

```cpp
Blynk.connectWiFi(ssid, pass);
```
Pour se connecter à des réseaux WiFi ouverts, définissez le mot de passe comme étant une chaîne de caractères vide (`""`).

## Gestion des connexions

Il existe plusieurs fonctions aidant à gérer les connexions :

### Blynk.connect()

```cpp
# Cette fonction essaiera de se connecter au serveur Blynk.
# Retourne true quand connecté, false si le délai maximum de connexion est atteint.
# Le délai maximum est par défaut de 30 secondes.
bool result = Blynk.connect();
bool result = Blynk.connect(timeout);
```

### Blynk.disconnect()

Pour se déconnecter du serveur Blynk, utilisez :

```cpp
Blynk.disconnect();
```

### Blynk.connected()
Pour obtenir l'état de la connexion au serveur Blynk, utilisez :

```cpp
bool result = Blynk.connected();
```

### Blynk.run()
Cette fonction doit être appelée fréquemment pour traiter les commandes entrantes et entretenir la connexion Blynk.
Elle est habituellement appelée dans le `void loop() {}`.

Vopus pouvez l'initier à d'autres endroits, à moins que vous excédiez le tas de la mémoire (heap memory - dans les fonctions imbriquées avec la mémoire locale).
Par exemple, il n'est pas recommendé d'appeler `Blynk.run()` à l'intérieur des fonctions `BLYNK_READ` et `BLYNK_WRITE` sur des périphériques ayant peu de mémoire vive (RAM).

## Contrôle des broches Analogiques et Digitales
La bibliothèque peut exécuter nativement des opérations basiques sur des broches d'entrée et sortie :
* digitalRead
* digitalWrite
* analogRead
* analogWrite (Signal Analogique ou PWM en fonction de la plateforme)

Pas la peine d'écrire du code pour de simples choses comme les LEDs, les relais et les capteurs analogiques.

## Contrôle des broches Virtuelles
Les broches Virtuelles sont conçues pour envoyer des données de votre micro-contrôleur à l'Application Blynk et inversement.
Pensez aux broches Virtuelles comme à des canaux permettant d'envoyer des données. Assurez-vous de bien différencier les broches Virtuelles des broches physiques de votre hardware. Les broches Virtuelles n'ont pas de représentation physique.

Les broches Virtuelles peuvent être utilisées comme interface avec les bibliothèques (Servo, LCD, et autres) et implémentent des fonctionnalités personnalisées.
Le périphérique peut envoyer des données à l'Application en utilisant `Blynk.virtualWrite(pin, value)` et recevoir des données de l'Application en utilisant `BLYNK_WRITE(vPIN)`.

#### Types de données des broches Virtuelles
Les valeurs actuelles sont envoyées en tant que chaînes de caractères, don il n'y a pas de limite pratique sur les données qui peuvent être envoyées.
Néanmoins, souvenez-vous des limitations de la plateforme quand vous traitez des nombres. Par exemple les nombres entiers sur l'Arduino font 16 bits, permettant des nombres entre -32768 et 32767.
Vous pouvez interpréter les données entrantes comme étant des nombres entiers (Integer), décimaux (Float, Double) et des chaînes de caractères (String) :
```cpp
param.asInt();
param.asFloat();
param.asDouble();
param.asStr();
```

Vous pouvez aussi récupérer les données brutes à partir du paramètre tampon.

```cpp
param.getBuffer()
param.getLength()
```

### Blynk.virtualWrite(vPin, value)

Vous pouvez envoyer tous les formats de données aux broches Virtuelles

```cpp
// Envoie une chaîne de caractères
Blynk.virtualWrite(pin, "abc");

// Envoie un nombre entier
Blynk.virtualWrite(pin, 123);

// Envoie un nombre décimal
Blynk.virtualWrite(pin, 12.34);

// Envoie plusieurs valeurs en tant que tableau (array)
Blynk.virtualWrite(pin, "hello", 123, 12.34);

// Envoie les données brutes
Blynk.virtualWriteBinary(pin, buffer, length);
```

**Note :** Appeler `virtualWrite` provoque une tentative d'envoyer la valeur au réseau immédiatement.

### Blynk.setProperty(vPin, "property", value)

Cela permet de [changer les propriétés d'un widget](#blynk-main-operations-change-widget-properties)

### BLYNK_WRITE(vPIN)

`BLYNK_WRITE` défini une fonction qui est appelée lorsque le périphérique reçoit une mise à jour de la valeur de la broche Virtuelle à partir du serveur :

```cpp
BLYNK_WRITE(V0)
{   
  int value = param.asInt(); // Obtient la valeur comme étant un nombre entier

  // Le paramètre peut contenir plusieurs valeurs, auquel cas :
  int x = param[0].asInt();
  int y = param[1].asInt();
}
```

### BLYNK_READ(vPIN)

`BLYNK_READ` défini une fonction qui est appelée quand il est demandé au périphérique d'envoyer la valeur actuelle d'une broche Virtuelle au serveur. Normalement, cette fonction doit contenir quelques appels à `Blynk.virtualWrite`.

```cpp
BLYNK_READ(V0)
{
  Blynk.virtualWrite(V0, newValue);
}
```

### BLYNK_WRITE_DEFAULT()

Redéfini la commande pour chaque broche qui ne sont pas couvertes par des fonctions `BLYNK_WRITE` personnalisées.

```cpp
BLYNK_WRITE_DEFAULT()
{
  int pin = request.pin;      // Quelle broche est gérée, exactement ?
  int value = param.asInt();  // Utilise le paramètre comme d'habitude.
}
```

### BLYNK_READ_DEFAULT()


Redéfini la commande pour chaque broche qui ne sont pas couvertes par des fonctions `BLYNK_READ` personnalisées.

```cpp
BLYNK_READ_DEFAULT()
{
  int pin = request.pin;    // Quelle broche est gérée, exactement ?
  Blynk.virtualWrite(pin, newValue);
}
```

### BLYNK_CONNECTED()

Cette fonction est appelée à chaque fois que Blynk se connecte au serveur. Il est convenu d'utiliser des fonctions de synchronisation ici.

```cpp
BLYNK_CONNECTED() {
// Votre code ici
}
```

### Blynk.syncAll()

Demande au serveur d'envoyer les valeurs les plus récentes pour chaque widget. En d'autres mots, les états de toutes les broches analogiques/digitales seront restaurés et chaque broche virtuelle génèrera un évennement `BLYNK_WRITE`.

```cpp
BLYNK_CONNECTED() {
  if (isFirstConnect) {
    Blynk.syncAll();
  }
}
```

### Blynk.syncVirtual(vPin)

Demande une mise à jour de la valeur des broches Virtuelles. La commande `BLYNK_WRITE` est appelée comme résultat.

```cpp
Blynk.syncVirtual(V0);
# Envoyer une requête à plusieurs broches est aussi possible :
Blynk.syncVirtual(V0, V1, V6, V9, V16);
```

## BlynkTimer

`BlynkTimer` vous permet d'exécuter des actions périodiquement dans le contexte `loop()` principal.
C'est le même que le communément utilisé SimpleTimer, mais corrige différents problèmes.
`BlynkTimer` est inclus dans la bibliothèque Blynk par défaut, donc il n'y a pas besoin d'installer SimpleTimer séparément ou d'inclure SimpleTimer.h.
**Notez qu'un seul objet BlynkTimer vous autorise de planifier jusqu'à 16 chronomètres.**
Pour plus d'informations sur l'utilisation du chronomètre, vous pouvez vous référer ici : http://playground.arduino.cc/Code/SimpleTimer  
Et voici un [croquis d'exemple pour BlynkTimer](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/PushData/PushData.ino#L30).

## Déboguage

### #define BLYNK_PRINT
### #define BLYNK_DEBUG

Pour activer l'affichage des messages de déboguage sur le port Série par défaut, ajoutez en haut de votre croquis **(doit être la première ligne)** :
To enable debug prints on the default Serial, add on the top of your sketch **(should be the first line)**:

```cpp
#define BLYNK_PRINT Serial // Défini l'objet utilisé pour l'affichage
#define BLYNK_DEBUG        // Optionnel, active un affichage plus détaillé
```

Et activez la sortie Série dans le `setup()` :

```cpp
Serial.begin(9600);
```
Ouvrez le Moniteur Série et vous verrez l'affichage des messages de déboguage.

Vous pouvez aussi utiliser d'autres ports hardwares de série ou SoftwareSerial pour l'affichage du débogage (Vous aurez besoin d'un adaptateur pour le connecter à l'ordinateur).

***Note :*** activer le mode débogage rendra la puissance de traitement de votre hardware jusqu'à dix fois plus lente.

### BLYNK_LOG()

Lorsque `BLYNK_PRINT` est défini, vous pouvez utiliser `BLYNK_LOG` pour afficher vos journaux. Son utilisation est similaire à `printf` :

```cpp
BLYNK_LOG("Voici ma valeur : %d", 10);
```

Sur d'autres plateformes (comme l'Arduino 101) `BLYNK_LOG` peut être indisponible, ou peut utiliser trop de ressources.
Dans ce cas vous pouvez utiliser un groupe de fonctions de journalisation simples :

```cpp
BLYNK_LOG1("Heeey"); // Affiche une chaîne de caractères
BLYNK_LOG1(10);      // Affiche un nombre
BLYNK_LOG2("Voici ma valeur : ", 10); // Affiche 2 valeurs
BLYNK_LOG4("Température: ", 24, " Humidité: ", 55); // Affiche 4 valeurs
// ...
```

## Réduire l'empreinte

Pour minimiser la mémoire vive/flash de votre programme, vous pouvez désactiver certaines fonctionnalités natives :

1. Commentez `#define BLYNK_PRINT` pour supprimer les affichages
2. Ajoutez en haut de votre croquis :
```cpp
#define BLYNK_NO_BUILTIN   // Désactive les opérations natives des broches digitales et analogiques
#define BLYNK_NO_FLOAT     // Désactive les opérations des décimales
```

Souvenez-vous aussi qu'un simple `BlynkTimer` peut planifier plusieurs chronomètres, vous n'aurez donc probablement besoin que d'une seule instance de BlynkTimer dans votre croquis.

## Porter, Bidouiller

Si vous voulez plonger dans la fabrication/modification/portage d'une implémentation de la bibliothèque Blynk, veuillez vous référer à [cette documentation](https://github.com/blynkkk/blynk-library/tree/master/extras/docs).
