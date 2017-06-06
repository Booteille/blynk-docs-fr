#Guide de démarrage  
Ce guide vous permettra de commencer en 5 minutes (lire ne compte pas !).
Nous allons activer une LED connectée à votre Arduino à l'aide de l'application Blynk sur votre smartphone.

Connectez une LED comme montré ici :

<img src="images/Arduino_LED.jpg" style="width: 250px; height:350px"/>

##Guide de démarrage de l'Application Blynk
###1. Créer un compte Blynk
Une fois l'application Blynk téléchargée, vous aurez besoin de créer un nouveau compte Blynk. Ce compte est séparé des comptes utilisés pour les Forums Blynk, au cas où vous en avez déjà un.

Nous recommandons d'utiliser une **réelle** adresse e-mail car ça vous simplifiera les choses plus tard.

<img src="images/register_account.png" style="width: 200px; height:360px"/>

####Pourquoi ai-je besoin de créer un compte ?

Un compte est nécessaire afin de sauvegarder vos projets et y avoir accès à partir de plusieurs périphériques et de n'importe où. C'est aussi une mesure de sécurité.

Vous pouvez toujours configurer votre propre [serveur privé Blynk](http://docs.blynk.cc/#blynk-server) et avoir le contrôle absolu.   

###2. Créer un nouveau Projet
Après vous être correctement identifié avec votre compte, commencez par créer un nouveau projet.

<img src="images/getting_started/create_project_button.png" style="width: 200px; height:360px"/>

Donnez-lui un nom.

<img src="images/getting_started/give_project_name.png" style="width: 200px; height:360px"/>

###3. Choisir son matériel
Sélectionnez le modèle de matériel que vous allez utiliser. Vous pouvez vérifier la [liste des matériels supportés](http://docs.blynk.cc/#supported-hardware)!

<img src="images/getting_started/select_hardware.png" style="width: 200px; height:360px"/>

###4. Jeton d'Authentification

Le **Jeton d'Authentification** est un identifiant unique nécessaire pour connecter votre matériel à votre smartphone.
Chaque nouveau projet que vous créérez a son propre Jeton d'Authentification. Vous obtiendrez un Jeton d'Authentification automatiquement sur votre e-mail après la création d'un projet. Vous pouvez aussi le copier manuellement. Cliquez sur la section périphériques :

<img src="images/getting_started/token_1.png" style="width: 200px; height:360px"/>

Cliquez sur le périphérque :

<img src="images/getting_started/list_of_devices.png" style="width: 200px; height:360px"/>

Et vous verrez le jeton :

<img src="images/getting_started/new_device.png" style="width: 200px; height:360px"/>

<span style="color:#D3435C;">**NOTE :** Ne partagez votre Jeton d'Authentification avec personne, à moins que vous ne souhaitiez que quelqu'un puisse avoir accès à votre matériel.</span>


C'est vraiment pratique de l'envoyer via l'e-mail. Pressez le bouton e-mail et le jeton vous sera envoyé à l'adresse e-mail utilisée lors de l'inscription.
Vous pouvez aussi appuyer sur la ligne du Jeton et il sera copié vers le presse-papier.

Pressez maintenant le bouton **"Create"**.

<img src="images/new_project.png" style="width: 200px; height:360px"/>

###5. Ajouter un Widget

Votre grille de projet est vide, ajoutons un bouton pour crontrôler notre LED.

Appuyez n'importe où sur la grille pour ouvrir la liste des Widgets. Tous les widgets disponibles sont situés ici. Maintenant, choisissez un boutton.

**Liste des widgets**

<img src="images/widgets_box.png" style="width: 200px; height:360px"/>

<img src="images/project_with_button.png" style="width: 200px; height:360px"/>

**Glisser-Déposer** - Tapotez et garder le Widget pour le déposer vers sa nouvelle position.

**Paramètres du Widget** - Chaque Widget a ses propres paramètres. Tapez sur le widget pour y accéder.

<img src="images/button_settings.png" style="width: 200px; height:360px"/>

Le paramètre le plus important est **PIN** (broche, en anglais). La liste des broches reflète les broches physiques définies par votre matériel. Si votre LED est connectée à la broche digitale 8 - alors sélectionnez **D8** (**D** - est pour **D**igital).

<img src="images/pin_selection.png" style="width: 200px; height:360px"/>

###6. Démarrer le Projet
Quand vous aurez terminé avec les Paramètres - appuyez sur le bouton **PLAY**. Vous basculerez ainsi du mode ÉDITION au mode JOUER où vous pouvez interragir avec votre matériel. Pendant le mode JOUER, vous n'êtes pas autorisé à déplacer ou configurer de nouveaux widgets, appuyez sur **STOP** et retournez au mode ÉDITION pour l'être de nouveau.

Vous obtiendrez un message indiquant "Arduino UNO is offline" (Arduino UNO est hors-ligne). Nous traiterons cela dans la prochaine section.

<img src="images/play_button.png" style="width: 200px; height:360px"/>

##Guide de démarrage du Matériel
###Comment utiliser un croquis d'example
Vous devriez à partir de maintenant avoir la bibliothèque Blynk installée sur votre ordinateur. Si ce n'est pas le cas - [cliquez-ici](http://docs.blynk.cc/#downloads-blynk-library).

Les croquis d'exemple vous aideront à connecter votre matériel rapidement et à mettre en place les fonctionnalités majeures de Blynk.

Ouvrez le croquis d'exemple en fonction du modèle de votre matériel et du shield que vous utilisez.

<img src="images/connection_type_sketch.png" style="width: 500px; height:217px"/>

Jetons un coup d'oeil au croquis d'exemple pour un [Arduino UNO + Shield Ethernet](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino)

```cpp
#define BLYNK_PRINT Serial
#include <SPI.h>
#include <Ethernet.h>
#include <BlynkSimpleEthernet.h>

char auth[] = "YourAuthToken";

void setup()
{
  Serial.begin(9600); // Affiche l'état de la connexion dans le moniteur Série
  Blynk.begin(auth);  // Votre Arduino se connecte ici au Cloud de Blynk
}

void loop()
{
  Blynk.run(); // Toute la magie de Blynk se passe ici...
}
```

###Jeton d'Authentification
Dans ce croquis d'exemple, trouvez cette ligne :

```cpp
char auth[] = "YourAuthToken";
```
C'est le [Jeton d'Authentification](http://docs.blynk.cc/#getting-started-getting-started-with-application-4-auth-token) que vous vous êtes envoyé par e-mail.
Vérifiez votre e-mail et copiez-le, puis collez-le entre les guillemets.

Ce devrait ressembler à ceci :

```cpp
char auth[] = "f45626c103a94983b469637978b0c78a";
```

Téléversez le croquis vers la carte et ouvrez le moniteur Série. Attendez jusqu'à ce que vous voyez quelque chose comme ceci :

```
Blynk v.X.X.X
Your IP is 192.168.0.11
Connecting...
Blynk connected!
```

<span style="color:#24C48C" >**Félicitations ! Vous l'avez fait ! Le matériel est désormais connecté au Cloud de Blynk !**</span>

##Jouer avec Blynk
Retournez sur l'application de Blynk, appuyez sur le bouton et activez puis désactivez la LED ! Elle devrait clignoter.

<img src="images/button_pressed.png" style="width: 200px; height:360px"/>

Regardez [d'autres croquis d'exemple](https://github.com/blynkkk/blynk-library/tree/master/examples).

Sentez vous libre d'expérimenter et de combiner différents exemples ensembles afin de créer vos propres projets.

Par exemple, pour attacher une LED à une broche [PWM](http://www.arduino.cc/en/Tutorial/Fading) sur votre Arduino, paramétrez le widget **slider** afin de contrôler la luminosité d'une LED. Utilisez les mêmes étapes décrites plus haut.