# Opérations Principales de Blynk

## Broches Virtuelles
Blynk peut contrôler des broches Digitales et Analogiques d'entrée et sortie sur votre hardware directement. Vous n'avez même pas à écrire de code pour cela.
C'est génial pour faire clignoter des LEDs mais ce n'est souvent pas suffisant...

Nous avons conçu les broches Virtuelles pour envoyer **toute sorte** de données de votre micro-controlleur vers l'Application Blynk et inversement.

Tout ce que vous connecterez à votre hardware sera capable de communiquer avec Blynk.
Avec les broches Virtuelles vous pouvez envoyer quelque-choque à partir de l'Application, le traiter sur le micro-controlleur et le renvoyer vers le smartphone. Vous pouvez déclencher des fonctions, lire des périphériques I2C, convertir des valeurs, contrôler des servo et DC moteurs, etc.

Les broches Virtuelles peuvent être utiliser pour interfacer avec des bibliothèques externes (Servo, LCD, et autres) et implémenter des fonctionnalités personnalisées.

Le hardware peut envoyer des données aux Widgets à travers les broches Virtuels de cette manière :

```cpp
Blynk.virtualWrite(pin, "abc");
Blynk.virtualWrite(pin, 123);
Blynk.virtualWrite(pin, 12.34);
Blynk.virtualWrite(pin, "hello", 123, 12.34);
```

Pour plus d'informations sur les broches Virtuelles, [lisez ceci](http://docs.blynk.cc/#blynk-firmware-virtual-pins-control)

## Envoyer des données de l'application au hardware
Vous pouvez envoyer n'importe quelle donnée à partir des Widgets de votre application vers le hardware.

Tous les [Widgets Controller](http://docs.blynk.cc/#widgets-controllers) peuvent envoyer des données aux broches virtuelles de votre hardware.
Par exemple, le code ci-dessous montre comment obtenir des valeurs à partir du Widget Button de l'Application

```cpp
BLYNK_WRITE(V1) // Le Widget Button écrit à la broche virtuelle V1
{
  int pinData = param.asInt();
}
```
Quand vous appuyez sur un Button, l'Application Blynk envoie  ```1```. Au deuxième clic, elle envoie ```0```

Voici comment le Widget Button est configuré :

<img src="images/button_virtual_1.png" style="width: 200px; height:360px"/>


Croquis d'exemple complet: [Obtenir des données](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/GetData/GetData.ino#L24)

### Envoyer un tableau à partir d'un Widget
Certains Widgets (comme le Joystick ou zeRGBa) ont plus d'une sortie.

<img src="images/joystick_merge_mode.png" style="width: 200px; height:360px"/>

Cette sortie peut être écrite à une broche Virtuelle en tant que tableau de valeurs.
Du côté hardware - Vous pouvez obtenir n'importe quel élément du tableau [0, 1, 2...] en utilisant :

```cpp
BLYNK_WRITE(V1) // Le Widget ÉCRIT sur la broche Virtuelle V1
{   
  int x = param[0].asInt(); // Obtient la pemière valeur
  int y = param[1].asInt(); // Obtient la seconde valeur
  int z = param[N].asInt(); // Obtient la valeur N
}
```

 **Croquis :** [JoystickTwoAxis](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/JoystickTwoAxis/JoystickTwoAxis.ino#L24)

## Obtenir des données du hardware
Il y a deux manières d'envoyer des données de votre hardware vers les Widgets de votre application à travers les broches Virtuelles.

### Effectuer des requêtes par Widget
- En utilisant la fréquence de lecture intégrée pendant que l'Application est active en configurant le paramètre 'Reading Frequency' à un certain interval :

<img src="images/frequency_reading_pull.png" style="width: 200px; height:360px"/>

```cpp
BLYNK_READ(V5) // Le Widget dans l'application LIT la broche virtuelle V5 avec une certaine fréquence
{
  // Cette commande écrit le temps de disponibilité de l'Arduino en secondes à la broche Virtuelle V5
  Blynk.virtualWrite(5, millis() / 1000);
}
```

**Croquis :** [PushDataOnRequest](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/PushDataOnRequest/PushDataOnRequest.ino#L26)


### Envoyer des données à partir du hardware
Si vous avez besoin d'ENVOYER des données de capteur ou autre de votre hardware au Widget, vous pouvez écrire la logique que vous souhaitez.
Configurez simplement la fréquence en mode PUSH. Toute commande que le hardware envoie au Cloud de Blynk est automatiquement stoqué sur le serveur et vous obtenez cette information soit avec le widget [History Graph](http://docs.blynk.cc/#widgets-displays-history-graph) soit avec [HTTP API](http://docs.blynkapi.apiary.io/#reference/0/pin-history-data/get-all-history-data-for-specific-pin).

<img src="images/frequency_reading_push.png" style="width: 200px; height:360px"/>

Nous vous recommandons d'envoyer des données par interval et d'éviter une [erreur Flood](http://docs.blynk.cc/#troubleshooting-flood-error).  
Vous pouvez utiliser des chronomètres comme [BlynkTimer](http://docs.blynk.cc/#blynk-firmware-blynktimer).
Suivez les instructions de ce [croquis d'exemple](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/PushData/PushData.ino) pour plus de détails.

Voici comment cela peut marcher :

```cpp
#include <SPI.h>
#include <Ethernet.h>
#include <BlynkSimpleEthernet.h>

char auth[] = "YourAuthToken"; // Mettez votre jeton ici

BlynkTimer timer; // Créé un objet Chronomètre nommé timer

void setup()
{
  Serial.begin(9600);
  Blynk.begin(auth);

  timer.setInterval(1000L, sendUptime); // Ici vous configurez l'interval (1sec) et quelle fonction appeler
}

void sendUptime()
{
  // Cette fonction envoie la durée de diponibilité de l'Arduino toutes les secondes au port Virtuel (V5)
  // Dans l'application, la fréquence de lecture du Widget devrait être paramétré sur PUSH
  // Vous pouvez envoyer n'importe quoi avec n'importe quel interval en utilisant cette structure
  // N'envoyez pas plus de dix valeurs par seconde

  Blynk.virtualWrite(V5, millis() / 1000);
}

void loop()
{
  Blynk.run(); // Toute la magie de Blynk se passe ici
  timer.run(); // BlynkTimer fonctione...
}
```

**Croquis :** [PushData](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/PushData/PushData.ino#L30)

## État de la synchronisation

### Pour le hardware
Si votre hardware perd la connexion internet et redémarrage, vous pouvez restaurer toutes les valeurs à partir des Widgets dans l'application Blynk.

```cpp
BLYNK_CONNECTED() {
  if (isFirstConnect) {
    Blynk.syncAll();
  }
}

// Ici, les gestionnaires pour la commande de synchronisation
BLYNK_WRITE(V0) {
   ....
}

```

La commande ```Blynk.syncAll()``` restaure toues les valeurs du Widget basées sur les dernières valeurs sauvegardées sur le serveur.
Les états des toutes les broches analogiques et digitales seront restaurés. Chaque broche Virtuelle va effectuer un évènnement ```BLYNK_WRITE```.

[Synchroniser le hardware avec l'état de l'Application - En Anglais](https://github.com/blynkkk/blynk-library/blob/master/examples/More/Sync/HardwareSyncStateFromApp/HardwareSyncStateFromApp.ino)

Vous pouvez aussi ne mettre à jour qu'une seule broche Virtuelle en appelant ```Blynk.syncVirtual(V0)``` ou vous pouvez mettre à jour plusieurs broches avec ```Blynk.syncVirtual(V0, V1, V2, ...)```.

Vous pouvez aussi utiliser le serveur pour stocker n'importe quelle valeur sans Widget. Appelez simplement ```Blynk.virtualWrite(V0, value)```.

[Stocker une seule valeur sur le serveur - En Anglais](https://github.com/blynkkk/blynk-library/blob/master/examples/More/ServerAsDataStorage/ServerAsDataStorage_SingleValue/ServerAsDataStorage_SingleValue.ino)

[Stocker plusieurs valeurs sur le serveur - En Anglais](https://github.com/blynkkk/blynk-library/blob/master/examples/More/ServerAsDataStorage/ServerAsDataStorage_MultiValue/ServerAsDataStorage_MultiValue.ino)

### Pou l'application
Si vous avez besoin de synchroniser votre hardware avec l'état de votre Widget même si votre application est hors ligne utilisez  ```Blynk.virtualWrite```.

Imaginez vous avez un Widget LED connecté à la broche Virtuelle V1 dans l'application et un bouton physique attaché à votre hardware.
Quand vous appuyez sur le bouton physique, vous attendez que l'application affiche l'état du Widget LED mis à jour.
Pour y parvenir vous avez besoin d'envoyer ```Blynk.virtualWrite(V1, 255)``` quand un bouton physique est pressé.

[Représenter l'état d'un bouton physique via le widget LED avec des interrupteurs - En Anglais](https://github.com/blynkkk/blynk-library/blob/master/examples/More/Sync/ButtonInterrupt/ButtonInterrupt.ino)

[Représenter l'état d'un bouton physique via le widget LED avec Scrutation - En Anglais](https://github.com/blynkkk/blynk-library/blob/master/examples/More/Sync/ButtonPoll/ButtonPoll.ino)

[Représenter l'état d'un bouton physique via le widget Button avec Scrutation - En Anglais](https://github.com/blynkkk/blynk-library/blob/master/examples/More/Sync/SyncPhysicalButton/SyncPhysicalButton.ino)

## Contrôle de plusieurs périphériques
L'Application Blynk supporte plusieurs périphériques. Ce sinifie que vous pouvez assigner n'importe quel widget à un périphérique spécifique avec son propre jeton d'authentification.
Par exemple - vous pouvez avoir un boutton sur V1 qui contrôle le voyant Wi-Fi A et un autre bouton sur V1 qui contrôle le voyant Wi-Fi B. Afin de faire cela vous avez besoin de plus d'un périphérique dans votre projet. Pour y parvenir, allez dans les paramètres du projet et cliquez sur la section "Devices" :

<img src="images/new_project_settings.png" style="width: 200px; height:360px"/>

Vous obtiendrez une liste des périphériques :

<img src="images/list_of_devices.png" style="width: 200px; height:360px"/>

Et pourrez ajouter de nouveaux périphériques :

<img src="images/new_device.png" style="width: 200px; height:360px"/>

Après les étapes ci-dessus, chaque widget aura plus d'un champ "Target" :

<img src="images/widget_settings_devices.png" style="width: 200px; height:360px"/>

Désormais, vous avez besoin d'assigner le périphérique au widget et après cela le widget contrôlera seulement ce périphérique spécifique.

C'est tout ! Maintenant vous avez besoin de téléverser des croquis avec les bons Jetons d'Authentification dans votre hardware.

### Étiquettes

La fonctionnalité Tags (étiquettes, en anglais) vous permet de grouper de multiples périphériques entre eux. Les Tags sont très utiles lorsque vous voulez contrôler plusieurs périphériques avec un seul widget. Par exemple, imaginez un cas où vous avez 3 ampoules intelligentes et vous voulez allumer toutes ces ampoules avec un unique clic. Vous avez besoin d'assigner 3 périphériques à 1 tag et assigner le tag à un bouton. C'est tout.

Les widgets Tag supportent aussi la synchronisation de l'état. Vous pouvez donc obtenir l'état du widget à partir de votre hardware.  Néanmoins vous ne pouvez mettre à jour l'état de tels widgets à partir du hardware.

## État des périphériques connectés
L'application Blynk supporte l'état de connectivité de plusieurs périphériques.

<img src="images/online_status.png" style="width: 200px; height:360px"/>

Dans un monde idéal, quand le périphérique ferme la connexion TCP avec ```connection.close()``` - le serveur connecté reçoit une notification indiquant la fermeture de la connexion. Vous pouvez donc obtenir une mise à jour instantanée de l'état sur l'interface utilisateur. Néanmoins, dans le monde réel, c'est plutôt une situation exceptionnelle.
Dans la majorité des cas, il n'y a pas de manière facile et instantannée de découvrir que la connexion n'est plus active.

C'est pourquoi Blynk utilise le méchanisme ```HEARTBEAT```. Avec cette approche, le périphérique envoie périodiquement des commandes ```ping``` à un interval prédéfini (10 secondes par défault, [propriété](https://github.com/blynkkk/blynk-library/blob/master/src/Blynk/BlynkConfig.h) ```BLYNK_HEARTBEAT```).
Dans le cas où le hardware n'envoie rien dans les dix secondes, le serveur attend cinq secondes supplémentaires et après ça, la connexion est considérée comme interrompue et fermée par le serveur. Sur l'interface utilisateur, vous verrez donc la mise à jour de l'état 15 secondes après que ce soit réellement arrivé.

Vous pouvez aussi modifier l'interval de ```HEARTBEAT``` du côté hardware via ```Blynk.config```.
Dans ce cas, la formule ```newHeartbeatInterval * 2.3``` sera appliquée. Donc dans le cas où vous avez décider de mettre l'interval ```HEARTBEAT``` à 5 secondes, vous serez notifié à propos de la connexion avec un délai de 11 secondes dans le pire des cas.

## Changer les propriétés d'un Widget
Changer certaines propriétés d'un widget à partir du côté hardware est aussi supporté.
Par exemple, vous pouvez changer la couleur du widget LED basé sur une condition :

```cpp
// Change la couleur de la LED
Blynk.setProperty(V0, "color", "#D3435C");

// Change le label de la LED
Blynk.setProperty(V0, "label", "My New Widget Label");

// Change les labels du MENU
Blynk.setProperty(V0, "labels", "Menu Item 1", "Menu Item 2", "Menu Item 3");

```

[Modifier les propriétés pour un champ à valeur unique- En Anglais](https://github.com/blynkkk/blynk-library/blob/master/examples/More/SetProperty/SetProperty_SingleValue/SetProperty_SingleValue.ino)

[Modifier les propriétés pour un champ à valeur multiples - En Anglais](https://github.com/blynkkk/blynk-library/blob/master/examples/More/SetProperty/SetProperty_MultiValue/SetProperty_MultiValue.ino)

**NOTE : ** Changer ces paramètres fonctionne **SEULEMENT** pour les widgets attachés à des broches Virtuelles (les broches analogiques/digitales ne marcheront pas).

Quatre propriétés de widget sont supportées - ```color```, ```label```, ```min```, ```max``` pour tous les widgets :

```label``` est une chaîne de caractères pour le label de tous les widgets

```color``` est une chaîne de caractères au format [HEX](http://www.w3schools.com/html/html_colors.asp) (sous la forme: #RRGGBB,
où RR (rouge), GG (vert) et BB (bleu) sont des valeurs hexadécimales entre 00 et FF). Par exemple :
```
#define BLYNK_GREEN     "#23C48E"
#define BLYNK_BLUE      "#04C0F8"
#define BLYNK_YELLOW    "#ED9D00"
#define BLYNK_RED       "#D3435C"
#define BLYNK_DARK_BLUE "#5F7CD8"
```

Du côté logiciel, les objets widget supportent aussi les fonctions ```setLabel()``` et ```setColor()```.

Propriétés de Widget Spécifiques:

**Button**

```onLabel``` est une chaîne de caractères pour le label ON du bouton;

```offLabel``` est une chaîne de caractères pour le label OFF du bouton;

**Music Player**

```isOnPlay``` est un booléen acceptant true/false;
```
Blynk.setProperty(V0, "isOnPlay", "true");
```

**Menu**

```labels``` est une liste de chaînes de caractères pour la sélection du widget Menu;
```
Blynk.setProperty(V0, "labels", "label 1", "label 2", "label 3");
```

Vous pouvez aussi changer les propriétés d'un widget via l'[API HTTP](http://docs.blynkapi.apiary.io/#).

## Limitations et Recommendations
- Ne mettez pas ```Blynk.virtualWrite``` ou n'importe quelle commande ```Blynk.*``` à l'intérieur de ```void loop()```- ce provoquera une tonne de messages sortants vers notre serveur et votre connexion sera interrompue.

- Nous recommandons d'utiliser les fonctions avec des intervals. Par exemple, avec [BlynkTimer](http://docs.blynk.cc/#blynk-firmware-blynktimer)

- Évitez d'utiliser de longs délais avec ```delay()``` – ce peut causer des interruptions de la connexion;

- Si vous envoyez plus de cent valeurs par seconde - vous pouvez provoquer une [erreur Flood](http://docs.blynk.cc/#troubleshooting-flood-error) et votre hardware sera automatiquement déconnecté du serveur;

- Faites attention à ne pas envoyer trop de comamndes ```Blynk.virtualWrite``` puisque de nombreux périphériques ne sont pas très puissants (comme l'ESP8266) alors il ne pourra pas gérer beaucoup de requêtes.
