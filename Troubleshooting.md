# Résolution des Problèmes

## Connexion

Si vous expérimpentez des problèmes de connexion, suivez ces étapes :
1. Vérifiez que votre matériel, vos câbles et votre alimentation électrique sont de bonne qualité, non-endommagés, etc.
  Utilisez des câbles et ports USB à haute puissance.
2. Vérifiez vos câblages en utilisant les exemples (Client TCP/HTTP ou similaire) **fournis avec votre shield ou matériel**.
  * Une fois que vous comprenez comment gérer la connexion, il est bien plus simple d'utiliser Blynk.
3. Essayez de lancer la commande ```telnet blynk-cloud.com 8442``` à partir de votre PC, connecté au même réseau que votre matériel.
  Vous devriez voir quelque chose comme : ```Connected to blynk-cloud.com.```.
4. Essayez de lancer les exemples par défaut de Blynk pour votre plateforme **sans faire de modifications** afin de voir si ils fonctionnent.
  * Vérifiez par deux fois que vous avez sélectionné **le bon exemple** pour votre type de connexion et modèle de matériel.
  * Nos exemples sont fournis avec **des commentaires et des explications**. **Lisez-les attentivement.**
  * Vérifiez que votre Jeton d'Authentification est valide (copié à partir de l'Application et **ne contient pas d'espace, etc.**).
  * Si cela ne marche pas, jetez un coup d'oeil à [l'affichage du débogage série](https://booteille.github.io/blynk-docs-fr/#resolution-des-problemes-activer-le-debogage).
5. Voilà ! Ajoutez vos modifications et fonctionnalités. Amusez-vous bien avec Blynk !

***Note :*** lorsque vous avez plusieurs périphériques connectés à votre réseau, ils doivent avoir des adresses MAC et IP différentes. Par exemple, lorsque vous utilisez 2 Arduino UNO avec des shields Ethernet, lancer l'exemple par défaut aux deux causera un problème de connexion. Vous devriez plutôt utiliser l'exemple de [configuration manuelle d'ethernet][manual ethernet configuration](https://github.com/blynkkk/blynk-library/blob/master/examples/Boards_Ethernet/Arduino_Ethernet_Manual/Arduino_Ethernet_Manual.ino).

## Connexion au réseau WiFi
Si vous rencontrez des problèmes de connexion WiFi, voici quelques pièges auxquels faire attention :

* Vous essayez de vous connecter au réseau "WPA & WPA2 Entreprise" (souvent utilisé dans les bureaux) et votre shield ne supporte pas cette méthode de sécurité
* Votre connexion WiFi a une page de connexion qui vous demande d'entrer un jeton d'accès (souvent utilisé dans les restaurants)
* La sécurité de votre réseau WiFi interdit la connexion de périphériques étrangers (Filtrage MAC, etc)
* Il y a un pare-feu activé. Le port par défaut pour les connexions matériel est 8442. Assurez-vous qu'il soit ouvert

## Délai

Si vous utilisez un ```delay()``` long ou demandez à votre matériel de se mettre en veille dans le ```loop()```, attendez-vous à des pertes de connexion ou des performances détériorées.

***NE FAITES PAS CELA :***
```cpp
void loop()
{
  ...
  delay(1000); // C'est un délai long, ce doit être évité
  other_long_operation();
  ...
  Blynk.run();
}
```

***Note :*** Cela s'applique aussi aux commandes BLYNK_READ et BLYNK_WRITE !

***SOLUTION :***
Si vous avez besoin d'effectuer des actions à des intervalles réguliers - utilisez les chronomètres (timers), par exemple [BlynkTimer](https://booteille.github.io/blynk-docs-fr/#logiciel-blynk-blynktimer).

## Erreur Flood

Si votre code envoie fréquemment de nombreuses requêtes à notre serveur, votre matériel sera déconnecté. L'Application Blynk indiquera que votre matériel est hors-ligne ("Your hardware is offline").

Lorsque ```Blynk.virtualWrite``` est dans le ```void loop```, il génère des centaines d'écritures (writes) par seconde.

Voici un exemple pouvant causer du flood. ***NE FAITES PAS CELA :***
```cpp
void loop()
{
  Blynk.virtualWrite(1, value); // Cette ligne envoie des centaines de messages au serveur Blynk
  Blynk.run();
}
```

***SOLUTION :***
Si vous avez besoin d'effectuer des actions à des intervalles réguliers - utilisez les chronomètres (timers), par exemple [BlynkTimer](https://booteille.github.io/blynk-docs-fr/#logiciel-blynk-blynktimer).

Utiliser un ```delay()``` ne résoudra pas le problème non plus. Ce peut causer [un autre problème](https://booteille.github.io/blynk-docs-fr/#resolution-des-problemes-delai). Utilisez les Chronomètres !

Si envoyer des centaines de requêtes est ce dont vous avez besoin pour votre produit, vous pouvez augmenter la limite de flood sur un serveur local et dans la bibliothèque Blynk.
Pour le serveur local, vous avez besoin de modifier la propriété ```user.message.quota.limit``` dans le fichier ```server.properties```
```
  #100 Req/sec rate limit per user.
  user.message.quota.limit=100
```

Pour le bibliothèque vous aurez besoin de changer la propriété ```BLYNK_MSG_LIMIT``` dans le fichier ```BlynkConfig.h``` :
```cpp
  // Limite le nombre de commandes sortantes
  #define BLYNK_MSG_LIMIT 20
```

## Activer le débogage

Pour activer l'affichage du débogage sur le Serial par défaut, ajoutez ceci en haut de votre croquis **(ce doit être la première ligne de votre croquis)** :

```cpp
#define BLYNK_DEBUG // Optionel, affiche de nombreux messages
#define BLYNK_PRINT Serial
```
Et activez le Serial dans ```void setup()``` :

```cpp
Serial.begin(9600);
```

Vous pouvez aussi utiliser d'autres ports matériels de série ou SoftwareSerial pour l'affichage du débogage (Vous aurez besoin d'un adaptateur pour le connecter à l'ordinateur).

***Note :*** activer le mode débogage rendra la puissance de traitement de votre matériel jusqu'à dix fois plus lente.

## Problèmes Geo DNS

Le cloud de Blynk utilise [Geo DNS](https://en.wikipedia.org/wiki/Geodns) pour les solutions non-commerciales afin de minimiser les coûts de maintenance des serveurs.
Cela signifie que lorsque vous vous connectez à ```blynk-cloud.com```, le service DNS vous redirige vers le serveur le plus proche, en foncton de votre adresse IP.
Le problème est que l'application et le matériel ne sont parfois pas dans le même réseau. Et il y a une chance pour que le matériel et le smartphone soient connectés à des serveurs différents. Vous obtiendrez alors une erreur vous indiquant l'utilisateur n'est pas enregistré (```User is not registered```).

Il y a deux manières de résoudre ce problème :

- Utiliser un [Serveur Blynk Local] (https://booteille.github.io/blynk-docs-fr/#serveur-blynk)
- ```ping blynk-cloud.com``` à partir du réseau où votre matériel est connecté. Utilisez l'adresse IP que vous obtenez durant ce ping et indiquez-le dans votre application mobile de cette manière :

 <img src="images/login.png" style="width: 200px; height:360px"/>  <img src="images/custom.png" style="width: 200px; height:360px"/>

## Réinitialiser le Mot de Passe

Sur l'écran de connexion, cliquez sur "Problems signin in?" et puis sur le bouton "Reset Password". Vous obtiendrez des instructions par email.
