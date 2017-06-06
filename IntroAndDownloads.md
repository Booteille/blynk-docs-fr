#Introduction
Ce guide vous aidera à comprendre comment commencer à utiliser Blynk et vous donnera un aperçu compréhensible de toutes ses fonctionnalités.

Si vous souhaitez commencer directement à jouer avec Blynk, regardez le guide de démarrage.
<br>

[Guide de démarrage >](http://docs.blynk.cc/#getting-started)

##Comment fonctionne Blynk
Blynk a été conçu pour l'Internet des Objets. Il peut contrôler un matériel à distance, il peut afficher des données de capteur, il peut stocker des données, les visualiser et faire beaucoup d'autres trucs cools.

Il y a trois composants majeurs dans la plateforme:

- **Application Blynk** - vous permet de créer de fantastiques interfaces pour vos projets en utilisant différents widgets que nous fournissons.

- **Serveur Blynk** - responsable de toute les communications entre le smartphone et le matériel.
Vous pouvez utiliser notre Cloude Blynk ou faire tourner votre [Serveur privé Blynk](http://docs.blynk.cc/#blynk-server) localement.
C'est open-source, ça peut facilement gérer des milliers de périphériques et peut même être démarré sur une Raspberry Pi.

- **Bibliothèque Blynk** - pour toutes les plateformes matériel populaires - active la communication avec le serveur et traite toutes les commandes entrantes et sortantes.

Maintenant imaginez: chaque fois que vous pressez un Bouton sur l'application Blynk, le message voyage vers ~~l'espace~~ le Cloud Blynk, où il trouve magicalement un chemin vers votre matériel. Ça marche de la même manière dans l'autre sens et tout se déroule en un clignotement.

<img src="images/architecture.png" style="width: 640px; height:478px"/>

##Fonctionnalités
* API et UI similaire pour tous les matériels et périphériques supportés
* Connexion au cloud via:
  * Ethernet
  * WiFi
  * Bluetooth et BLE
  * USB (Serial)
  * ...
* Collection de widgets faciles à utiliser
* Manipulation des broches directes sans code à écrire
* Facilité d'intégrer et ajouter de nouvelles fonctionnalités en utilisant les broches virtuelles
* Surveillance de l'historique des données via le widget **History Graph**
* Communication Périphérique-à-Périphérique en utilisant le widget **Bridge**
* Envoi d'emails, de tweets, de notifications push, etc.
* ... de nouvelles fonctionnalités sont constamment ajoutées !

Vous pouvez trouver des [croquis d'example](https://github.com/blynkkk/blynk-library/tree/master/examples) couvrant les fonctionnalités basiques de Blynk.
Ils sont inclus dans la bibliothèque. Tous les croquis sont conçus pour être facilement combiné entre eux.

##Comment commencer à faire du Blynk ?
À ce niveau vous devez certainement penser : **"Okay, je veux ça. Que faire pour commencer ?"** – Juste quelques trucs, vraiment :

####**1. Matériel**.
Un Arduino, Raspberry Pi, ou un kit de développement similaire.

**Blynk fonctionne à travers Internet.**
Ce signifie que le matériel que vous choisissez doit être capable de connecter avec internet. Certaines cartes, comme l'Arduino Uno, vont nécesser un Shield Ethernet ou Wi-Fi pour communiquer, tandis que d'autres sont déjà capables de communiquer avec internet : comme l'ESP8266, le Raspberry Pi avec le donglet Wi-Fi, le Particle Photon ou le SparkFun Blynk Boarf. Néanmoins, même si vous n'avez pas de Shield, vous pouvez le connecter à travers le port USB de votre ordinateur (c'est un poil compliqué pour les débutants, mais nous vous couvrons).
Ce qui est cool, c'est que la [liste des matériels](http://docs.blynk.cc/#supported-hardware) fonctionnant avec Blynk est immense et continue de grandir.

####**2. Un Smartphone**.
L'application Blynk est une bâtisseuse d'interface bien conçue. Elle marche à la fois sur iOS et Android, donc aucune guerre sainte ici, ok ?

#Téléchargements
##**Application Blynk pour iOS ou Android** <br>
[<img src="http://linkmaker.itunes.apple.com/images/badges/en-us/badge_appstore-lrg.svg" alt="Drawing" style=" width: 170px; height:60px"/>](https://itunes.apple.com/us/app/blynk-control-arduino-raspberry/id808760481?ls=1&mt=8)  &nbsp; &nbsp; &nbsp; &nbsp;[<img src="https://play.google.com/intl/en_us/badges/images/apps/en-play-badge.png" alt="Drawing" style=" width: 158px; height:42px"/>](https://play.google.com/store/apps/details?id=cc.blynk)

##**Bibliothèque Blynk** <br>
[Téléchargez la Bibliothèque Blynk >](https://github.com/blynkkk/blynk-library/releases/latest)

Au cas où vous aurez oublié ou e savez pas comment installer une bibliothèque Arduino, [cliquez-ici](http://www.arduino.cc/en/guide/libraries).
