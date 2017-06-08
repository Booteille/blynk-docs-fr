#Sécurité

Le serveur Blynk a 3 ports ouverts pour les différents niveaux de sécurité.

* **8441** - Connexion SSL/TLS pour le hardware
* **8442** - Connexion TCP pleine pour le hardware (pas de sécurité)
* **8443** - Connexion pour l'authentification mutuelle (SSL mutuel) pour les Applications Mobile

Le hardware peut choisir de se connecter au port 8441 ou 8442, en fonction de ses capacités.

#### Utiliser une Passerelle SSL

La plupart des plateformes ne sont pas capables de gérer SSL, alors ils se connectent au port 8442.
Néanmoins, notre [script de passerelle](https://github.com/blynkkk/blynk-library/blob/master/scripts/blynk-ser.sh) peut être utilisé pour ajouter une couche de sécurité SSL à la communication.

```bash
./blynk-ser.sh -f SSL
```
Cela va rediriger toutes les connexions hardwareles du port 8441 au serveur via la passerelle SSL.
Vous pouvez lancer ce script sur votre Raspberry Pi, ordinateur, ou même directement sur votre routeur !

**Note :** lorsque vous utilisez votre propre serveur, vous devez écraser le certificat server.crt fourni, ou en spécifier un au script en utilisant l'option ```--cert``` :
```bash
./blynk-ser.sh -f SSL -s <server ip> -p 8441 --cert=<certificate>.crt
```

L'option ```-f SSL``` est activée par défaut pour les communications USB donc vous n'avez pas à le déclarer explicitement.

**Note :** SSL n'est supporté à travers la passerelle que sur Linux/OSX actuellement.

Si vous voulez éviter SSL et vous connecter via TCP, vous pouvez aussi faire ceci :

```bash
./blynk-ser.sh -t TCP
```

#### Utiliser un Serveur Local Blynk

Afin d'obtenir une sécurité maximale vous pouvez [installer le serveur Blynk localement](https://booteille.github.io/blynk-docs-fr/#serveur-blynk) et restraindre l'accès à votre réseau, donc personne à part vous ne pourra y accéder. Dans ce cas toutes les données sont stockées localement dans votre réseau et ne sont pas envoyées vers Internet.
Dans le cas d'un serveur local Blynk il n'y aussi aucun besoin de protéger la connexion entre votre hardware et le serveur local Blynk.
C'est vrai pour les connexions Ethernet et partiellement vrai pour les connexion Wi-Fi. Dans le cas du Wi-Fi vous devez au moins utiliser le type WPA, WPA2 (Wi-Fi Protected Access) afin de protéger le traffic sans fil.

WPA et WPA2 offrent une encryption robuste qui pourra certainement protéger toutes les données voyageant à travers les ondes, si un mot de passe assez fort est utilisé. Même si vos données sont en TCP/IP pleins, un autre utilisateur ne sera pas en mesure de déchiffrer les paquets capturés.
Dans tous les cas, assurez-vous que votre mot de passe est assez fort, faute de quoi le seul facteur limitant un attaquant est le temps.
