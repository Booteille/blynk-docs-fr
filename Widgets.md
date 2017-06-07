#Widgets
Les Widgets sont des modules d'interface. Chacun d'un effectue une fonction d'entrée / sortée spécifique quand il communique avec le matériel.

Il y a 4 types de Widgets:

- **Controlleurs** - ils envoient des commandes au matériel. Utilisez-les pour contrôler votre équipement;
- **Afficheurs** - utilisés pour diverses visualisations de données qui viennent du matériel vers le smartphone;
- **Notifications** - are various widgets to send messages and notifications;
- **Interface** - sont divers widgets donnant un meilleur aspect à votre Interface Utilisateur;
- **Autres** - les widgets qui ne conviennent à aucune catégorie;

Chaque Widget a ses propres paramètres.

Certains des Widgets (par exemple le Widget Bridge) sont utilisés pour activer certaines fonctionnalités et n'ont aucun paramètre.

## Paramètres Communs des Widgets
### Sélection de la Broche (Pin Select)
C'est un des paramètres principaux que vous avez besoin de définir. Il définit quelle broche contrôler et quelle broche lire.

<img src="images/pin_selection.png" style="width: 200px; height:360px"/>

**Broches Digitales** - représente les broches d'entrée et sortie physiques de votre matériel. Les broches PWM sont marquées avec le symbole ```~```

**Broches Analogiques** - représente les broches d'entrée et sortie analogiques de votre matériel

**Broches Virtuelles** - n'a aucune représentation physique. Elles sont utilisées pour n'importe quel transfère de données entre l'Application Blynk et votre matériel.
Lisez-en plus sur les Broches Virtuelles [ici](http://docs.blynk.cc/#blynk-main-operations-virtual-pins).

### Représentation des Données

Dans le cas où vous souhaitez représenter les données entrantes avec échelle spécifique vous pouvez utiliser le bouton de représentation :

<img src="images/display_edit_mapping.png" style="width: 200px; height:360px"/>

Disons que votre capteur envoie une valeur entre 0 et 1023. Vous souhaitez cependant afficher les valeurs entre 0 et 100 dans l'application.
Avec la représentation des données activée, la valeur entrante 1023 sera représentée par 100.

### SÉPARER/FUSIONNER (SPLIT/MERGE)
Certains Widgets peuvent envoyer plus d'une valeur et avec cet interrupteur vous pouvez contrôler comment les envoyer.

<img src="images/split_merge.gif" style="width: 300px; height:280px"/>


- **SÉPARER (SPLIT)** :
Chaque paramètre est envoyé directement à la broche de votre matériel (par exemple D7). Vous n'avez pas besoin d'écrire le moindre code.

	**NOTE :** Avec ce mode vous envoyez plusieurs commande à partir d'un widget, ce qui peut entraîner des baisses de performance sur votre matériel.

	Exemple : Si vous avez un Widget Joystick et qu'il est paramétré sur D3 et D4, il va envoyer 2 commandes sur Internet :

	```cpp
	digitalWrite(3, value);
	digitalWrite(4, value);
	```

- **FUSIONNER (MERGE)**:
Quand le mode FUSIONNER est sélectionné, vous envoyez juste 1 message, consistant en un tableau de valeurs. Vous aurez néanmoins besoin de le traiter du côté matériel.

	Ce mode ne peut être utilisé qu'avec des Broches Virtuelles.

	Exemple : Ajoutez un Widget zeRGBa et paramétrez-le sur le mode FUSIONNER. Choisissez la Broche Virtuelle V1

	```cpp
	BLYNK_WRITE(V1) // Il y a un Widget qui ÉCRIT des données sur V1
	{
	  int r = param[0].asInt(); // récupère une valeur du canal ROUGE
	  int g = param[1].asInt(); // récupère une valeur du canal VERT
	  int b = param[2].asInt(); // récupère une valeur du canal BLEU
	}
	```

### Envoyer Au Relâchement (Send On Release)

Cette option est disponible pour la plupart des widgets contrôleurs et vous permet de réduire le trafic de données sur votre matériel.
Par exemple, quand vous bougez un widget Joystick, les commandes sont diffusées continuellement vers votre matériel et durant un simple mouvement de Joystick, vous pouvez envoyer des dizaines de commandes. Dans certains cas d'utilisation cela peut être nécessaire mais créer une telle charge peut causer un redémarrage du matériel.
Nous recommandons d'activer la fonctionnalité **Envoyer Au Relâchement** dans la plupart des cas, à moins que vous ayez réellement besoin d'un retour instantanné.
Cette option est activée par défaut.


### Gradient de Couleurs

Certains widgets d'affichage ont la capacité de sélectionner un gradient. Le Gradient vous permet de changer la couleur de vos Widgets sans aucun code.
Nous fournissons actuellement 2 gradients :

- Chaud : Vert - Orange - Rouge;
- Froid : Vert - Bleu - Violet;

Les Gradients changent la couleur de votre widget en fonction de leur propriété min/max. Par exemple, si vous sélectionnez le gradient chaud pour votre widget d'affichage de niveau avec comme valeurs 0 en minimum et 100 en maximum, quand la valeur 10 arrivera au widget il va devenir vert, quand la valeur 50 arrivera vous verrez du orange et quand la valeur 80 arrivera il deviendra rouge.


##Contrôleurs
### Bouton (Button)

Fonctionne en mode Presser (Push) ou Interrupteur (Switch). Vous permet d'envoyer des valeurs 0/1 (LOW/HIGH). Le bouton envoie 1 (HIGH) à la pression et 0 (LOW) au relâchement.

<img src="images/button.png" style="width: 77px; height:80px"/>

<img src="images/button_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino)

### Glissière (Slider)
Similaire à un potentiomètre. Vous permet d'envoyer des valeurs entre MIN et MAX.

<img src="images/slider.png" style="width: 77px; height:80px"/>

<img src="images/slider_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino)

### Chronomètre (Timer)
Le Chronomètre déclenche des actions à un moment spécifique, Même si le smartphone est hors-ligne. Le temps de démarrage (START) envoie 1 (HIGH) et le temps d'arrêt (STOP) envoie 0 (LOW).

Les versions récentes d'Android ont aussi un Chronomètre amélioré avec le widget Eventor.
Avec l'évennement Eventor Chronométré vous pouvez assigner plusieurs chronomètres à la même broche, envoyer n'importe quelle chaîne de caractères/nombre, sélectionner des jours et un fuseau horaire.
Il est recommendé d'utiliser le widget Eventor à la place du Chronomètre.
Cependant, le widget Chronomètre convient toujours à de simples évènements chronométrés.

<img src="images/timer.png" style="width: 77px; height:80px"/>

<img src="images/timer_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [Timer](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Timer/Timer.ino)

### Joystick
Permet de contrôler les mouvements d'un servo-moteur dans 4 directions

####Paramètres :
- Modes SÉPARER/FUSIONNER (SPLIT/MERGE) - lisez [ici](http://docs.blynk.cc/#widgets-common-widget-settings-splitmerge)

- **Pivoter sur Inclinaison (Rotate on Tilt)**

Quand défini sur ON, le Joystick pivotera automatiquement si vous utilisez votre smartphone avec l'orientation paysage.

- **Retour Automatique (Autoreturn)**

Quand défini sur OFF, le Joystick ne retournera pas à la position centrale. Il restera toujours là où vous l'aurez laissé.

<img src="images/joystick.png" style="width: 77px; height:80px"/>

<img src="images/joystick_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [JoystickTwoAxis](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/JoystickTwoAxis/JoystickTwoAxis.ino)

### zeRGBa
zeRGBa est un contrôleur RVB classique (un sélecteur de couleur).

#### Paramètres :

- **SÉPARER (SPLIT)** :
Chacun des paramètres est envoyé directement à la broche de votre matériel (par exemple D7). Vous n'avez à écrire aucun code.

	**NOTE :** Avec ce mode vous envoyez plusieurs commande à partir d'un widget, ce qui peut entraîner des baisses de performance sur votre matériel.

	Exemple: Si vous avez un Widget zeRGBa et qu'il est défini à D1, D2, D3, il enverra 3 commandes vers Internet :

	```cpp
	digitalWrite(1, r);
	digitalWrite(2, g);
	digitalWrite(3, b);
	```

- **FUSIONNER (MERGE)** :
	Quand le mode FUSIONNER (MERGE) est sélectionné, vous n'envoyez qu'un message, consistant en un tableau de valeurs. Néanmoins vous avez besoin de le traiter du côté matériel.

	Ce mode ne peut être utilisé qu'avec des Broches Virtuelles.

	Exemple: Ajoutez un Widget zeRGBa et paramétrez-le sur le mode FUSIONNER. Choisissez la Broche Virtuelle V1.

	```cpp
	BLYNK_WRITE(V1) // zeRGBa assigné à V1
	{
    // Récupère une valeur du canal ROUGE
    int r = param[0].asInt();
    // Récupère une valeur du canal VERT
		int g = param[1].asInt();
		// Récupère une valeur du canal BLEU
		int b = param[2].asInt();
	}
	```

### Contrôle du Pas (Step Control)

Le contrôle du Pas est comme 2 boutons assignés à 1 broche. Un bouton incrémente vos valeurs par le pas désiré tandis que l'autre les décrémente. C'est très pratique dans des cas d'utilisation où vous avez besoin de changer vos valeurs très précisément et que vous ne pouvez atteindre cette précision avec le widget Glissière (Slider).

L'option **Envoyer le Pas (Send Step)** vous permet d'envoyer le pas au matériel à la place de la valeur actuelle du widget.
L'option **Boucler la Valeur (Loop Value)** vous permet de réinitialiser la valeur du widget au début quand la valeur maximale est atteinte.

**Croquis :** [Croquis Basique](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino)

##Afficheurs
### Afficheur de Valeur (Value Display)
Affiche les données entrantes de vos capteurs ou Broches Virtuelles.

<img src="images/display.png" style="width: 77px; height:80px"/>

<img src="images/display_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino)

### Valeur labelée (Labeled Value)
Affiche les données entrantes de vos capteurs ou Broches Virtuelles. C'est une version meilleure que 'Afficheur de Valeur' (Value Display) car il possède le formatage des chaînes de caractères, donc vous pouvez formater les données entrantes vers n'importe quelle chaîne de caractère vous souhaitez.

<img src="images/display.png" style="width: 77px; height:80px"/>

<img src="images/labeled_value_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino)

#### Options de Formatage

Assumons que votre capteur envoie le nombre 12.6789 à votre application Blynk.
Les options de formatage suivantes sont supportées :

```/pin/``` - affiche la valeur sans formatage (12.6789)

```/pin./``` - affiche la valeur sans la partie décimale (13)

```/pin.#/``` - affiche la valeur avec une décimale (12.7)

```/pin.##/``` - affiche la valeur avec deux décimales (12.68)

<img src="images/labeled_value_format_edit.png" style="width: 200px; height:360px"/>

### LED
Une simple LED d'information. Vous devez envoyer 0 afin d'éteindre la LED et 255 pour l'allumer. Sinon, utilisez simplement l'API Blynk comme indiqué ci-dessous :

```cpp
WidgetLED led1(V1); // Enregistre comme Broche Virtuelle V1
led1.off();
led1.on();
```

Toutes les valeurs entre 0 et 255 changeront la luminosité de la LED :

```cpp
WidgetLED led2(2);
led2.setValue(127); // Défini la luminosité de la LED à 50%
```

<img src="images/led.png" style="width: 77px; height:80px"/>

<img src="images/led_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [LED](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/LED/LED_Blink/LED_Blink.ino)

### Jauge (Gauge)
Un moyen visuel génial pour afficher les valeurs numériques entrantes.

<img src="images/gauge.png" style="width: 77px; height:80px"/>

<img src="images/gauge_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino)

#### Options de Formattage

Jauge a aussi un champ "Label" qui nous permet d'utiliser le formatage.
Assumons que votre capteur envoie le nombre 12.6789 à votre application Blynk.
Les options de formatage suivantes sont supportées :

```/pin/``` - affiche la valeur sans formatage (12.6789)

```/pin./``` - affiche la valeur sans la partie décimale (13)

```/pin.#/``` - affiche la valeur avec une décimale (12.7)

```/pin.##/``` - affiche la valeur avec deux décimales (12.68)

### LCD
C'est un affichage LCD 16x2 régulier fabriqué dans notre station secrète en Chine.

#### MODE SIMPLE / AVANCÉ (SIMPLE / ADVANCED)

#### Commandes
Vous avez besoin d'utiliser des commandes spéciales avec ce widget :

```cpp
lcd.print(x, y, "Votre message");
```
Où x est un symbole de position (0-15), y est le numéro d'une ligne (0 ou 1)

```cpp
lcd.clear();
```

<img src="images/lcd.png" style="width: 77px; height:80px"/>

<img src="images/lcd_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [LCD Mode Avancé](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/LCD/LCD_AdvancedMode/LCD_AdvancedMode.ino)
**Croquis :** [LCD Mode Simple Écriture](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/LCD/LCD_SimpleModePushing/LCD_SimpleModePushing.ino)
**Croquis :** [LCD Mode Simple Lecture](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/LCD/LCD_SimpleModeReading/LCD_SimpleModeReading.ino)

#### Options de formattage

Assumons que votre capteur envoie le nombre 12.6789 à votre application Blynk.
Les options de formatage suivantes sont supportées :

```/pin/``` - affiche la valeur sans formatage (12.6789)

```/pin./``` - affiche la valeur sans la partie décimale (13)

```/pin.#/``` - affiche la valeur avec une décimale (12.7)

```/pin.##/``` - affiche la valeur avec deux décimales (12.68)

<img src="images/lcd_format_edit.png" style="width: 200px; height:360px"/>

### Graphique (Graph)
Tracez facilement les données de votre projet sous différentes formes.

<img src="images/graph.png" style="width: 77px; height:80px"/>

<img src="images/graph_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino)

### Graphique d'Historique (History Graph)
Vous permet de visualiser n'importe quelle données que votre matériel aura envoyé au préalable. Le Graphique d'Historique a 3 types de précision :

- Précision à la Minute - ```1h```, ```6h```;
- Précision à l'Heure - ```1d```, ```1w```;
- Précision au Jour - ```1m```, ```3m```;

Cela signifie que l'interval de mise à jour minimum pour le graphique est de 1 minute pour les périodes ```1h``` et ```6h```, 1 heure pour ```1d``` et ```1w`` et 1 jour pour ```1m``` et ```3m```.
Puisque Blynk Cloud est gratuit, nous limitons combien de données vous pouvez stocker. Pour le moment, Blynk n'accepte qu'un message par minute et par broche. Dans le cas où vous en envoyez plus fréquemment, une moyennes de vos valeurs sera faite. Par exemple, dans le cas où vous envoyez ``10``` à 12:12:05 puis encore ```12``` à 12:12:45, vous verrez comme résultat, dans votre graphique d'historique la valeur ``11``` pour 12:12.

Afin de voir des données dans le graphique d'historique vous avez besoin de widgets avec l'intervalle "Fréquence de lecture" (Frequency reading - dans ce cas votre application doit être ouverte et lancée) ou vous pouvez utiliser ```Blynk.virtualWrite``` du côté matériel. Chaque commande ```Blynk.virtualWrite``` est stockée automatiquement sur le serveur. Dans ce cas vous n'avez pas besoin que l'application soit démarrée.

Vous pouvez aussi facilement vider les données pour les broches sélectionnées ou récupérer toutes les données des brohes par email - Faitez juste glisser vers la gauche le graphique d'historique et cliquez sur "Supprimer les données" (Erase data).

<img src="images/erase_history_graph.png" style="width: 525px; height:263px"/>

Vous pouvez aussi obtenir les données des broches via l'[API HTTP](http://docs.blynkapi.apiary.io/#reference/0/pin-history-data/get-all-history-data-for-specific-pin).

<img src="images/history_graph.png" style="width: 77px; height:80px"/>

<img src="images/history_graph_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [PushData](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/PushData/PushData.ino)

### Terminal
Affiche des données à partir de votre matériel. Aussi, vous permet d'envoyer n'importe quelle chaîne de caractère à votre matériel. Les terminaux conservent toujours les 25 derniers messages que votre matériel a envoyé au Cloud Blynk. Cette limite peut être augmentée sur un Serveur Local.

Vous avez besoin d'utiliser des commandes spéciales avec ce widget :

```cpp
terminal.print();   // Affiche des valeurs, comme Serial.print()
terminal.println(); // Affiche des valeurs, comme Serial.println()
terminal.write();   // Écrit un tampon de données brut
terminal.flush();   // S'assure que les données ont bien été envoyées au périphérique
```

<img src="images/terminal.png" style="width: 77px; height:80px"/>

<img src="images/terminal_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [Terminal](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Terminal/Terminal.ino)

### Flux Vidéo (Video Streaming)
Widget simple vous permettant d'afficher n'importe quel flux direct. Le Widget supporte RTSP (RP, SDP), HTTP/S flux progressif, HTTP/S flux direct. Pour plus d'informations, référez-vous à la [documentation officielle d'Android](https://developer.android.com/guide/appendix/media-formats.html).

Pour le moment Blynk ne propose pas de serveurs de flux. Vous pouvez donc soit créer un flux directement à partir de votre caméra, utiliser un service tiers ou héberger un serveur de flux sur votre propre serveur (Sur un Raspberry par exemple).

### Affichage du Niveau (Level Display)

Affiche les données entrantes de vos capteurs ou Broches Virtuelles. L'Affichage du Niveau est très similaire à la barre de progression, offrant une vue agréable et moderne pour indiquer des évènnements "remplis", comme le "niveau de batterie".
Vous pouvez mettre à jour l'affichage de la valeur du côté matériel avec le code :

```cpp
Blynk.virtualWrite(V1, val);
```

Chaque message que le matériel envoie au serveur est stocké automatiquement sur le serveur. Le mode PUSH ne requiert pas que l'application soit en ligne ou ouverte.

**Croquis :** [Exemple de PUSH](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/PushData/PushData.ino)

##Notifications
###Twitter

Le widget Twitter connecte votre compte Twitter à Blynk et vous permet d'envoyer des Tweets à partir de votre matériel.

<img src="http://static1.squarespace.com/static/54765ba7e4b0d055ee0b47a6/54e92d39e4b0c31341b33a9a/55813d09e4b0ba8aa77ab230/1434533129525/TwitterON.png" style="width: 77px; height:80px"/>

<img src="images/twitter_edit.png" style="width: 200px; height:360px"/>

Code d'exemple :
```cpp
Blynk.tweet("Hey, Blynkers! Mon Arduino peut désormais Tweeter !");
```

Limites :

- vous ne pouvez envoyer 2 tweets avec le même message (ce sont les conditions d'utilisation de Twitter)
- seulement 1 Tweet toutes les 15 secondes est autorisé

**Croquis :** [Twitter](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Twitter/Twitter.ino)

###Email

Le widget d'Email vous permet d'envoyer un email à partir de votre matériel vers n'importe quelle adresse.

Code d'exemple :
```cpp
Blynk.email("mon_email@exemple.com", "Sujet", "Entrez votre message ici");
```

Il contient aussi le champ ```destinataire``` (```to```). Avec ce champ vous pouvez défini le destinataire de l'email dans l'application.
Dans ce cas, vous n'avez pas besoin de préciser le destinataire du côté matériel :

 ```cpp
 Blynk.email("Sujet", "Entrez votre message ici");
 ```

<img src="images/mail.png" style="width: 77px; height:80px"/>

<img src="images/mail_edit.png" style="width: 200px; height:360px"/>

Limites :

- Nombre maximum de caractères autorisés pour la longueur de l'email + le sujet + le message est de 120 caractères. Néanmoins vous pouvez augmenter cette limite si cela est nécessaire en ajoutant ```#define BLYNK_MAX_SENDBYTES XXX``` à votre croquis. Où ```XXX``` est la taille maximale désirée pour votre email.
Par exemple, pour un ESP vous pouvez définir à 1200 la longueur maximale ```#define BLYNK_MAX_SENDBYTES 1200```. Le ```#define BLYNK_MAX_SENDBYTES 1200``` doit être inclus avant n'importe quel ```include``` de Blynk.

**Croquis :** [Email](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Email/Email.ino)

###Notifications Push (Push Notifications)

Le widget Notification Push vous permet d'envoyez des notifications push de votre matériel à votre périphérique.
Actuellement il contient 2 options aditionnelles :

- **Notifier quand le matériel est Hors-ligne (Notify when hardware offline)** - vous recevrez une notification push lorsque le matériel deviendra hors-ligne.
- **Période d'Ignorance du status Hors-ligne (Offline Ignore Period)** - défini combien de temps votre matériel peut être hors-ligne (après qu'il soit passé hors-ligne) avant d'envoyer la notification.
Dans le cas où la période est dépassée - La notification "matériel hors-ligne" (hardware offline) sera envoyée. Vous ne recevrez pas de notification dans le cas où le matériel se reconnecte avant la fin de la période indiquée.
- **Priorité** Une haute priorité donne plus de chances que votre message sera délivré sans délai.
Consultez les explications supplémentaires  [ici](https://developers.google.com/cloud-messaging/concept-options#setting-the-priority-of-a-message).

**ATTENTION** : une priorité haute use plus de batterie que la priorité normale.

<img src="images/push.png" style="width: 77px; height:80px"/>

<img src="images/push_edit.png" style="width: 200px; height:360px"/>

Code d'exemple :
```cpp
Blynk.notify("Hey, Blynkers! Mon matériel peut push désormais !");
```

Limites :

- La longueur maximale du corps du message est de 120 caractères.
- Seulement 1 notification toutes les 15 secondes est autorisée

**Croquis :** [PushNotification](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/PushNotification/PushNotification_Button/PushNotification_Button.ino)

###Unicode dans les notifications, email, push, ...

La bibliothèque gère toutes les chaînes de caractères en UTF-8 Unicode. Si vous faites face à quelques problèmes, essayez d'afficher le message dans le moniteur Série et voyez si cela fonctionne (le moniteur doit être paramétré sur l'encodage UTF-8). Si cela ne fonctionne pas, vous devriez probablement lire à propos du support de l'unicode de votre compilateur.
Si cela marche mais que votre message est tronqué - vous avez besoin d'augmenter la taille limite (tout caractère Unicode utilise au moins deux fois la taille des caractères Latin).

###Augmentation de la taille limite

Vous pouvez augmenter la longueur maximale d'un message en ajoutant en haut de votre croquis (avant les ```include``` Blynk) :
```cpp
#define BLYNK_MAX_SENDBYTES 256 // 128 par défaut
```

## Interface

### Onglets (Tabs)
Le seul but du widget Onglets est d'étendre l'espace de votre projet. Vous pouvez avoir jusqu'à 4 onglets.
Aussi, vous pouvez déplacer les widgets entre les onglets. Déplacez simplement le widget sur le label de l'onglet voulu du widget onglets.

<img src="images/tabs_settings.png" style="width: 200px; height:360px"/>


### Menu
Le widget Menu vous permet d'envoyer des commandes à votre matériel basé sur la sélection faite sur l'Interface Utilisateur. Le Menu envoie l'index de l'élément que vous sélectionnez et non la valeur du label. Les index envoyés commencent à partir de 1.
Il fonctionne de la même manière que l'élément habituel ComboBox. Vous pouvez aussi définir des objets Menu [à partir du côté matériel](http://docs.blynk.cc/#blynk-main-operations-change-widget-properties).

<img src="images/menu_edit.png" style="width: 200px; height:360px"/>

Code d'exemple:
```
switch (param.asInt())
  {
    case 1: { // Objet 1
      Serial.println("Objet 1 sélectionné");
      break;
    }
    case 2: { // Item 2
      Serial.println("Objet 2 sélectionné");
      break;
    }    
  }
```

**Croquis :** [Menu](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Menu/Menu.ino)


###Entrée Temps (Time Input)
Le widget Entrée Temps vous permet de sélectionner des valeurs de démarrage/arrêt, du jour de la semaine, de fuseau horaire formatées et de les envoyer à votre matériel. Les valeurs actuellement supportées pour le temps sont ```HH:MM``` et ```HH:MM AM/PM```.

Hardware will get selected on UI time as seconds of day (```3600 * hours + 60 * minutes```) for start/stop time.

<img src="images/time_input_settings.png" style="width: 200px; height:360px"/>

**Croquis :** [Simple Entrée Temps pour le temps de démarrage](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/TimeInput/SimpleTimeInput/SimpleTimeInput.ino)

**Croquis :** [Entrée Temps Avancée](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/TimeInput/AdvancedTimeInput/AdvancedTimeInput.ino)

### Carte (Map)

Le widget Carte vous permet de définir des points/marqueurs sur votre carte à partir du côté matériel. C'est un widget très utile si vous possédez plusieurs périphériques et que vous voulez pister leurs valeurs sur une carte.

Vous pouvez envoyer un point vers la carte avec la commande write classique :

```cpp
Blynk.virtualWrite(V1, pointIndex, lat, lon, "valeur");
```

Nous avons aussi créé un adaptateur pour vous afin de rendre l'utilisation de la carte plus facile :

Vous pouvez changer les labels des boutons à partir du matériel avec :

```cpp
WidgetMap myMap(V1);
...
int index = 1;
float lat = 51.5074;
float lon = 0.1278;
myMap.location(index, lat, lon, "valeur");
```

Utiliser un ```index``` sauvegardé vous permet de surcharger la valeur d'un point.

**Croquis :** [Croquis Basique](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Map/Map.ino)

###Tableau (Table)
Le widget Tableau s'avère utile quand vous avez besoin de structurer des données similaire dans 1 élément graphique. Il marche comme un tableau habituel.

Vous pouvez ajouter une ligne au tableau avec :

```cpp
Blynk.virtualWrite(V1, "add", id, "Nom", "Valeur");
```

**Note :** Le nombre maximum de lignes dans un tableau est de 100. Quand vous atteignez cette limite, le tableau fonctionnera comme une liste FIFO (First In First Out - Premier Arrivé Premier Sorti).
Cette limite peut être changée en configurant la propriété ```table.rows.pool.size``` pour les serveurs locaux.

Surligner n'importe quel objet dans un tableau en utilisant son index, dans un tableau commençant à partir de 0 :

```cpp
Blynk.virtualWrite(V1, "pick", 0);
```

Vider le tableau à n'importe quel moment avec :

```cpp
Blynk.virtualWrite(V1, "clr");
```

<img src="images/table_settings.png" style="width: 200px; height:360px"/>

Vous pouvez aussi gérer d'autres actions provenant du tableau. Par exemple, utiliser une ligne comme un bouton Interrupter (Switch).

```cpp
BLYNK_WRITE(V1) {
   String cmd = param[0].asStr();
   if (cmd == "select") {
       //La ligne a été sélectionée dans le tableau
       int rowId = param[1].asInt();
   }
   if (cmd == "deselect") {
       //La ligne a été désélectionée dans le tableau
       int rowId = param[1].asInt();
   }
   if (cmd == "order") {
       //Les lignes ont été réorganisées dans le tableau
       int oldRowIndex = param[1].asInt();
       int newRowIndex = param[2].asInt();
   }
}
```

**Croquis :** [Utilisation Simple des Tableaux](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Table/Table_Simple/Table_Simple.ino)

### Sélecteur de Périphérique (Device Selector)

Le Sélecteur de Périphérique est un widget puissant vous permettant de mettre à jour les widgets en fonction d'un périphérique actif. Ce widget est particulièrement utile lorsque vous avez une flotte de périphériques avec une fonctionnalité similaire.

Imaginez vous avez 4 périphériques et que chaque périphérique a un capteur de Température & d'Humidité branché dessus. Pour afficher les données des quatre périphériques vous aurez besoin d'ajouter 8 widgets.

Avec le Sélecteur de Périphérique, vous pouvez utiliser seulement 2 Widgets qui affichera la Température et l'Humidité basé sur le périphérique actif choisi avec le Sélecteur de Périphérique.

Tout ce dont vous avez besoin est :
1. Ajouter le widget Sélecteur de Périphérique (Device Selector) au projet
2. Ajouter 2 widgets (par exemple, le widget Afficheur de Valeur (Value Display)) afin d'afficher la Température et l'Humidité
3. Dans les paramètres des widgets vous serez en mesure de les assigner au Sélecteur de Périphérique (Section Source ou Destinataire (Target))
4. Quitter les paramètres. Lancer le projet.

Désormais vous pouvez changer le périphérique actif dans le sélecteur de périphériques et vous verrez que les valeurs de la Température et de l'Humidité reflètent les données mises à jour pour le périphérique que vous avez choisi.

**NOTE : ** Les widgets Graphique d'Historique (History Graph) et Webhook ne fonctionnent pas (encore) avec le Sélecteur de Périphérique.

## Capteurs

### Accéléromètre (Accelerometer)

L'Accéléromètre est un type de [capteurs de mouvement](https://developer.android.com/guide/topics/sensors/sensors_motion.html) vous permettant de détecter les mouvements de votre smartphone.
Utile pour analyser les mouvements d'un périphérique, comme l'inclinaison, la secousse, la rotation, ou le balancement.
Conceptuellement, un capteur d'accélération détermine l'accélération appliquée à un périphérique par la mesure des forces appliquées sur le capteur. Mesurée en ```m/s^2``` appliquée aux axes ```x```, ```y```, ```z```.

Afin d'accepter des données de ce capter, vous avez besoin de :

```cpp
BLYNK_WRITE(V1) {
  //Force d'accélération appliquée à l'axe x
  int x = param[0].asFloat();
	//Force d'accélération appliquée à l'axe y
  int y = param[1].asFloat();
	//Force d'accélération appliquée à l'axe z
  int z = param[2].asFloat();
}
```

Le widget Accéléromètre ne fonctionne pas en tâche de fond.

### Baromètre/Pression (Barometer/Pressure)

Le capteur Baromètre/Pression est un type de [capteurs environnementaux](https://developer.android.com/guide/topics/sensors/sensors_environment.html) vous permettant de mesure la pression de l'air ambiant.

Mesurée en ```hPa``` ou ```mbar```.

Afin d'accepter des données de ce capteur vous avez besoin de :

```cpp
BLYNK_WRITE(V1) {
  //pression en mbar
  int pressure = param[0].asInt();
}
```

Le widget Baromètre ne fonctionne pas en tâche de fond.

### Gravité (Gravity)

Le capteur Gravité est un type de [capteurs de mouvement](https://developer.android.com/guide/topics/sensors/sensors_motion.html) vous permettant de détecter les mouvements de votre smartphone.
Utile pour analyser les mouvements d'un périphérique, comme l'inclinaison, la secousse, la rotation, ou le balancement.

Le capteur de gravité fourni un vecteur à trois dimensions indiquant la direction et la magnitude de la gravité.

Mesuré en ```m/s^2``` de force de gravité appliquée aux axes ```x```, ```y```, ```z```.


Afin d'accepter des données de ce capteur vous avez besoin de :

```cpp
BLYNK_WRITE(V1) {
  //Force de gravité appliquée à l'axe x
  int x = param[0].asFloat();
	//Force de gravité appliquée à l'axe y
  int y = param[1].asFloat();
	//Force de gravité appliquée à l'axe z
  int z = param[2].asFloat();
}
```

Le widget Gravité ne fonctionne pas en tâche de fond.

### Humidité (Humidity)

Le capteur d'Humidité est un type de [capteurs environnementaux](https://developer.android.com/guide/topics/sensors/sensors_environment.html) vous permettant de mesurer l'humidité relative ambiante.

Mesurée en ```%``` - l'humidité relative actuelle en pourcentage.

Afin d'accepter des données de ce capteur vous avez besoin de :

```cpp
BLYNK_WRITE(V1) {
  // humidité en %
  int humidity = param.asInt();
}
```

Le widget Humidité ne fonctionne pas en tâche de fond.

### Luminosité (Light)

Le capteur de Luminosité est un type de [capteurs environnementaux](https://developer.android.com/guide/topics/sensors/sensors_environment.html) vous permettant de mesurer le niveau de luminosité (mesure le niveau de luminosité ambiant (illumination) en lx).
Dans les téléphones il est utilisé pour contrôler la luminosité de l'écran.

Afin d'accepter des données de ce capteur vous avez besoin de :

```cpp
BLYNK_WRITE(V1) {
  //Valeur de la luminosité
  int lx = param.asInt();
}
```

Le widget Luminosité ne fonctionne pas en tâche de fond.

### Proximité (Proximity)

Le capteur de Proximité est un type de [capteurs de position](https://developer.android.com/guide/topics/sensors/sensors_position.html) vous permettant de déterminer combien est proche la face d'un smartphone vis-à-vis d'un objet.
Mesurée en ```cm``` - la distance de la face d'un téléphone vis-à-vis d'un objet. Néanmoins, la majorité de ces capteurs retournent seulement l'information LOIN / PROCHE (FAR / NEAR).
La valeur retournée sera donc ```0/1```. Où 0/LOW est ```LOIN``` et 1/HIGH est ```PROCHE```.

Afin d'accepter des données de ce capteur vous avez besoin de :

```cpp
BLYNK_WRITE(V1) {
  // distance d'un objet
  int proximity = param.asInt();
  if (proximity) {
     //PROCHE
  } else {
     //LOIN
  }
}
```

Le widget Proximité ne fonctionne pas en tâche de fond.

### Température

Le capteur de Température est un type de [capteurs environnementaux](https://developer.android.com/guide/topics/sensors/sensors_environment.html) vous permettant de mesurer la température de l'air ambiant.
Mesurée en ```°C``` - celcius.

Afin d'accepter des données de ce capteur vous avez besoin de :

```cpp
BLYNK_WRITE(V1) {
  // température en celcius
  int celcius = param.asInt();
}
```

Le widget Température ne fonctionne pas en tâche de fond.

### Déclencheur GPS (GPS Trigger)

Le Déclencheur GPS vous permet de déclencher facilement des évènements lorsque vous arrivez ou quittez une destination.
Ce widget fonctionnera en tâche de fond et vérifiera périodiquement vos coordonnées. Dans le cas où votre position est à l'intérieur/à l'extérieur du rayon requis (sélectionné sur le widget Carte (Map)), le widget enverra la commande ```HIGH```/```LOW``` au matériel. Par exemple, assumons que vous avez le widget Déclencheur GPS assigné à la broche ```V1``` et l'option ```Déclencher Quand Entrée``` (```Trigger When Enter```). Dans cette situation, lorsque vous arrivez au point de destination, le widget va déclencher l'évennement ```HIGH```.

```cpp
BLYNK_WRITE(V1) {
  int state = param.asInt();
  if (state) {
      //Vous entrez dans une destination
  } else {
      //Vous quittez une destination
  }
}
```

Plus de détails sur le fonctionnement du widget GPS peuvent être lus [ici](https://developer.android.com/guide/topics/location/strategies.html).

Le widget Déclencheur GPS fonctionne en tâche de fond.

### Flux GPS (GPS Streaming)

Utile pour analyser des données sur la position du smarthone comme la latitude, longitude, altitude et vitesse (speed - la vitesse peut souvent être sur 0 dans le cas où le smartphone ne le supporte pas).

Afin d'accepter des données de ce capteur vous avez besoin de :

```cpp
BLYNK_WRITE(V1) {
  float latitude = param[0].asFloat();
  float longitude = param[1].asFloat();
  float altitude = param[2].asFloat();
  float speed = param[3].asFloat();
}
```

ou vous pouvez utiliser l'adaptateur préparé ```GpsParam``` :

```cpp
BLYNK_WRITE(V1) {
  GpsParam gps(param);
  // Affiche 7 décimales pour la Latitude
  Serial.println(gps.getLat(), 7);
  Serial.println(gps.getLon(), 7);

  Serial.println(gps.getAltitude(), 2);
  Serial.println(gps.getSpeed(), 2);
}
```

Le widget Flux GPS fonctionne en tâche de fond.

**Croquis :** [Flux GPS](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/GPS_Stream/GPS_Stream.ino)

## Autre

### Pont (Bridge)

Le Pont peut être utilisé pour une communication de Périphérique-à-Périphérique (pas d'application impliquée). Vous pouvez envoyer des commandes d'écriture (write) digitales/analogiques/virtuelles d'un périphérique à un autre, une fois le jeton d'authentification connu.
Pour le moment le widget Pont ne nécessite pas le côté application (Il est surtout utilisé comme indication que nous possédons une telle fonctionnalité).

**Vous pouvez utiliser plusieurs ponts pour contrôler plusieurs périphériques.**

<img src="images/bridge.png" style="width: 77px; height:80px"/>

<img src="images/bridge_edit.png" style="width: 200px; height:360px"/>

Le widget Pont prend une broche virtuelle et la transforme en un canal afin de contrôler un autre périphérique. Ce signifie que vous pouvez contrôler n'importe quelle broche virtuelle, digitale ou analogique du périphérique ciblé.
Faites attention à ne pas utiliser de broches comme ```A0, A1, A2 ...``` lorsque vous communiquez entre différents types de périphériques, car le Coeur d'Arduino peut se référer aux mauvaises branches dans de tels cas.

Code d'exemple pour un périphérique A envoyant des valeurs au périphérique B :
```cpp
WidgetBridge bridge1(V1); //Initialise le widget Pont sur la broche V1 du Périphérique A
// ...
void setup() {
    Blynk.begin(...);
    while (Blynk.connect() == false) {
        // Attend jusqu'à ce que Blynk soit connecté
    }
    bridge1.digitalWrite(9, HIGH); // déclenche D9 HIGH sur le périphérique B. Aucun code n'est requis sur le périphérique B
    bridge1.analogWrite(10, 123);
    bridge1.virtualWrite(V1, "hello"); // Vous avez besoin d'écrire du code sur le périphérique B afin de recevoir cette valeur. Observez plus bas
    bridge1.virtualWrite(V2, "value1", "value2", "value3");
}

BLYNK_CONNECTED() {
  bridge1.setAuthToken("OtherAuthToken"); // Jeton du périphérique B
}
```

IMPORTANT : pendant l'exécution de ```virtualWrite()``` avec le widget Pont, le périphérique B devra traiter les données provenant du périphérique A.
Par exemple, si vous envoyez une valeur à partir du périphérique A vers le périphérique B en utilisant ```bridge.virtualWrite(V5)``` vous aurez besoin d'utiliser cette méthode sur le périphérique B :

```cpp
BLYNK_WRITE(V5){
    int pinData = param.asInt(); // la variable pinData va stocker la valeur récupérée via le Pont
}
```

Gardez en tête que ```bridge.virtualWrite``` n'envoie aucune valeur à l'application mobile. Vous avez besoin d'appeler ```Blynk.virtualWrite``` pour cela.

**Croquis :** [Bridge](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Bridge/Bridge.ino)

### Eventor
Le widget Eventor vous permet de créer de simples règles comportementales ou **évennements** (**events**).
Observons un cas d'utilisation typique : lire la température d'un capteur DHT et envoyer une notification push quand la température est au dessus d'une certaine limite :

```cpp
  float t = dht.readTemperature();
  if (isnan(t)) {
    return;
  }
  if (t > 40) {
    Blynk.notify(String("La Température est trop élevée : ") + t);
  }
```

Avec l'Eventor, vous n'avez pas besoin d'écrire ce code. Tout ce dont vous avez besoin est d'envoyer une valeur du capteur vers le serveur :

```cpp
  float t = dht.readTemperature();
  Blynk.virtualWrite(V0, t);
```
N'oubliez pas que les commandes ```virtualWrite``` ne peuvent être utilisées dans la boucle principale.

Maintenant, configurez le nouvel **Évennement** (**Event**) dans le widget Eventor :

<img src="images/eventor/eventor_for_temp_example.png" style="width: 200px; height:360px"/>

**NOTE :** N'oubliez pas d'ajouter un widget de notification.

L'Eventor se révèle utile lorsque vous avez besoin de changer des conditions à la colée sans téléverser de nouveau le croquis vers le matériel. Vous pouvez créer autant d'**évennements** que vous le souhaitez.
L'Eventor supporte aussi des évennements Chronomètre (Timer). Par exemple, vous pouvez définir la broche ```V1``` ON/HIGH à 21:00:00 tous les Vendredi.
Avec l'Évennement Chronomètre d'Eventor vous pouvez assigner de multiples chronomètres sur la même broche, leur envoyer n'importe quelle chaîne de caractères/nombre, sélectionner des jours et un fuseau horaire.

Afin de supprimer un **évennement** créé, faites-le glisser. Vous pouvez aussi faire glisser dehors le dernier élément de l'Évennement lui-même.

<img src="images/eventor/eventor_edit.png" style="width: 200px; height:360px"/>

**Croquis:** [Eventor](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Eventor/Eventor.ino)


**NOTE** : Les évennemens sont déclenchés uniquement lorsque la condition est rencontrée.
Néanmoins il y a une exception :
Considérons le simple évènement suivant ```if (temperature > 40) send notification ```.
Quand la température dépasse le seuil de 40 - l'action notification est déclenchée. Si la tepérature continue de rester au delà du seuil de 40, aucune action ne sera déclenché. Mais si la notification ```temperature``` descend en dessous du seuil puis passe de nouveau au dessus - la notification sera de nouveau envoyée (il n'y a pas de limite de 15 secondes pour les notifications d'Eventor).

### Horloge Temps-Réel (Real-time clock)

L'Horloge Temps-Réel vous permet d'obtenir l'heure du serveur. Vous pouvez préselectionner n'importe quel fuseau horaire sur l'Interface Utilisateur afin d'obtenir l'heure sur le matériel dans la langue requise.
Aucune broche n'est requise pour le widget Horloge Temps-Réel

<img src="images/rtc_edit.png" style="width: 200px; height:360px"/>

**Croquis :** [Horloge Temps-Réel](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/RTC/RTC.ino)

### BLE

Widget pour activer le support du Bluetooth Basse Énergie (Bluetooth Low Energy). Pour le moment le widget BLE n'est supporté que pour Android et requiert une connexion internet afin de se connecter et charger votre profil. Néanmoins, ce sera bien résolu. Aussi, certains widgets Blynk ne sont pas compatibles avec le widget BLE.

Actuellement, Blynk supporte beaucoup de modules différents. Référez-vous aux croquis ci-dessous.

<img src="images/ble_settings.png" style="width: 200px; height:360px"/>

**Croquis :** [BLE](https://github.com/blynkkk/blynk-library/tree/master/examples/Boards_Bluetooth)

### Bluetooth

Widget pour activer le support du Bluetooth.
Widget for enabling Bluetooth support. Pour le moment le widget Bluetooth n'est supporté que pour Android et requiert une connexion internet afin de se connecter et charger votre profil. Néanmoins, ce sera bien résolu. Aussi, certains widgets Blynk ne sont pas compatibles avec le widget Bluetooth.

Actuellement, Blynk supporte beaucoup de modules différents. Référez-vous aux croquis ci-dessous.

<img src="images/ble_settings.png" style="width: 200px; height:360px"/>

**Croquis :** [Bluetooth](https://github.com/blynkkk/blynk-library/tree/master/examples/Boards_Bluetooth)

### Lecteur de Musique (Music Player)

Un simple élément d'Interface Utilisateur avec 3 boutons - simule l'interface d'un lecteur de musique. Chaque bouton envoie sa propre commande au matériel :
```play```, ```stop```, ```prev```, ```next```.

Vous pouvez aussi changer l'état marche/arrêt (play/stop) du widget via la fonctionnalité [Changer une Propriété de Widget ](http://docs.blynk.cc/#blynk-main-operations-change-widget-properties) avec la propriété ```isOnPlay```.


**Croquis :** [Lecteur d Musique](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Player/Player.ino)

### Webhook

Le Webhook est un widget pour l'intégration de services tiers. Avec le widget webhook vous pouvez envoyer des requêtes HTTP/S aux serveurs de services tiers ou à un périphérique qui possède une API HTTP/S (par exemple le Philips Hue).

Toute opération d'écriture du côté matériel déclenche le widget Webhook (de la même manière que pour l'Eventor). Vous pouvez aussi déclencher le Webhook à partir de l'application si un widget contrôleur est assigné à la même broche que le Webhook. Vous pouvez déclencher un service tiers avec le simple appui sur un bouton.

Par exemple, imaginons un cas où vous souhaitez envoyer des données de votre matériel non seulement à Blynk mais aussi au serveur Thingspeak.
Dans un cas classique typique vous aurez besoin d'écrire du code comme celui-ci (c'est le code minimal et non le croquis entier) :

```cpp
WiFiClient client;
if (client.connect("api.thingspeak.com", 80)) {
    client.print("POST /update HTTP/1.1\n");
    client.print("Host: api.thingspeak.com\n");
    client.print("Connection: close\n");
    client.print("X-THINGSPEAKAPIKEY: " + apiKeyThingspeak1 + "\n");
    client.print("Content-Type: application/x-www-form-urlencoded\n");
    client.print("Content-Length: ");
    client.print(postStr.length());
    client.print("\n\n");
    client.print(postStr);
}
```

Avec le widget Webhook ce n'est plus nécessaire. Vous avez juste besoin de remplir les champs ci-dessous :

<img src="images/webhook_settings.png" style="width: 200px; height:360px"/>

Et faire l'habituel :  

```cpp
Blynk.virtualWrite(V0, value);
```

où V0 est la broche assignée au widget Webhook.

Vous pouvez aussi utiliser les chaînes génériques habituelles pour la valeur de la broche dans le corps (body) ou l'url. Par exemple :

```
https://api.thingspeak.com/update?api_key=xxxxxx&field1=/pin/
```

ou pour le corps

```
["/pin/"]
```

Vous pouvez aussi vous référer aux indexes spécifiques des broches à multiples valeurs (les broches à multiples valeurs supportent jusqu'à 5 valeurs) :

```/pin[0]/```,```/pin[1]/```, ```/pin[2]/```

Un autre fait sympa à propos du Webhook est que vous pouvez faire des requêtes GET à partir du Serveur Blynk et retourner la réponse directement à votre matériel. Ce qui est intéressant ici est que vous n'avez pas besoin de programmer une requête vers le service tiers. Imaginez un cas où vous souhaiter obtenir la météo d'un service tiers.  Par exemple, vous avez l'url ```http://api.sunrise-sunset.org/json?lat=33.3823&lng=35.1856&date=2016-10-01```, vous souhaitez la mettre dans le widget, sélectionnez la broche ```V0```, et faites l'habituel :  

```cpp
BLYNK_WRITE(V0){
  String webhookdata = param.asStr();
  Serial.println(webhookdata);
}
```

Maintenant, chaque fois que vous déclencherez la broche ```V0``` (avec ```Blynk.virtualWrite(V0, 1)``` du côté matériel ou un widget contrôleur assigné à ```V0```) - ```BLYNK_WRITE(V0)``` sera appelé.

**NOTE :** habituellement les serveurs de services tiers retournent de grosses réponses, alors vous devez augmenter la taille maximale autorisée par votre matériel avec ```#define BLYNK_MAX_READBYTES 1024```. Où ```1024``` - est la taille maximum autorisée d'un message.

**NOTE :** Le cloud de Blynk a une limitation pour le widget Webhook - vous n'êtes autorisé de n'envoyer qu'une seule requête par seconde. Vous pouvez changer ceci sur les serveurs locaux avec ```webhooks.frequency.user.quota.limit```. Faites très attention en utilisant les Webhooks car de nombreuses ressources ne sont pas capables de gérer 1 req/sec, donc vous pourriez être banni par certaines d'entre elles. Par exemple, ThingSpeak n'autorise qu'une requête toutes les 15 secondes.

 **NOTE :** Afin d'éviter de polluer Blynk, le Webhook a une limitation supplémentaire - dans le cas où la requête Webhook a échoué dix fois de suite, votre widget Webhook sera arrêté. Afin de le redémarrer vous aurez besoin d'ouvrir le widget et de le sauvegarder de nouveau. Les requêtes échouées sont des requêtes retournant un code status différent de 200 ou 302.
