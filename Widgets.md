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

Afin de voir des données avec le graphique d'historique vous 

In order to see data in history graph you need to use either widgets with "Frequency reading" interval (in
that case your app should be open and running) or you can use ```Blynk.virtualWrite``` on hardware side. Every
```Blynk.virtualWrite``` command is stored on server automatically. In that case you don't need application to be up and running.

You can also easily clear data for selected pins or get all data for pins via email - just swipe left history graph and click "Erase data".

<img src="images/erase_history_graph.png" style="width: 525px; height:263px"/>

You can also get pin data via [HTTP API](http://docs.blynkapi.apiary.io/#reference/0/pin-history-data/get-all-history-data-for-specific-pin).

<img src="images/history_graph.png" style="width: 77px; height:80px"/>

<img src="images/history_graph_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [PushData](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/PushData/PushData.ino)

### Terminal
Display data from your hardware. Also allows to send any string to your hardware. Terminal always stores last 25 messages
your hardware had send to Blynk Cloud. This limit may be increased on Local Server.

You need to use special commands with this widget:

```cpp
terminal.print();   // Print values, like Serial.print
terminal.println(); // Print values, like Serial.println()
terminal.write();   // Write a raw data buffer
terminal.flush();   // Ensure that data was sent out of device
```

<img src="images/terminal.png" style="width: 77px; height:80px"/>

<img src="images/terminal_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [Terminal](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Terminal/Terminal.ino)

### Video Streaming
Simple widget that allows you to display any live stream. Widget supports RTSP (RP, SDP), HTTP/S progressive streaming,
HTTP/S live streaming. For more info please follow [official Android documentation](https://developer.android.com/guide/appendix/media-formats.html).

At the moment Blynk doesn't provide streaming servers. So you can either stream directly from camera, use 3-d party
services or host streaming server on own server (on raspberry for example).

### Level Display

Displays incoming data from your sensors or Virtual Pins. Level Display is very similar to progress bar, it is very nice
and fancy view for indication of "filled" events, like "level of battery".
You can update value display from hardware side with code :

```cpp
Blynk.virtualWrite(V1, val);
```

Every message that hardware sends to server is stored automatically on server. PUSH mode doesn't require
application to be online or opened.

**Sketch:** [Push Example](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/PushData/PushData.ino)

##Notifications
###Twitter

Twitter widget connects your Twitter account to Blynk and allows you to send Tweets from your hardware.

<img src="http://static1.squarespace.com/static/54765ba7e4b0d055ee0b47a6/54e92d39e4b0c31341b33a9a/55813d09e4b0ba8aa77ab230/1434533129525/TwitterON.png" style="width: 77px; height:80px"/>

<img src="images/twitter_edit.png" style="width: 200px; height:360px"/>

Example code:
```cpp
Blynk.tweet("Hey, Blynkers! My Arduino can tweet now!");
```

Limitations :

- you cant' send 2 tweets with same message (it's Twitter policy)
- only 1 tweet per 15 seconds is allowed

**Sketch:** [Twitter](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Twitter/Twitter.ino)

###Email

Email widget allows you to send email from your hardware to any address.

Example code:
```cpp
Blynk.email("my_email@example.com", "Subject", "Your message goes here");
```

It also contains ```to``` field. With this field you may define receiver of email in the app.
In that case you don't need to specify receiver on hardware :

 ```cpp
 Blynk.email("Subject", "Your message goes here");
 ```

<img src="images/mail.png" style="width: 77px; height:80px"/>

<img src="images/mail_edit.png" style="width: 200px; height:360px"/>

Limitations :

- Maximum allowed email + subject + message length is 120 symbols. However you can increase this limit if necessary
by adding ```#define BLYNK_MAX_SENDBYTES XXX``` to you sketch. Where ```XXX``` is desired max length of your email.
For example for ESP you can set this to 1200 max length ```#define BLYNK_MAX_SENDBYTES 1200```. The
```#define BLYNK_MAX_SENDBYTES 1200``` must be included before any of the Blynk includes.
- Only 1 email 15 seconds is allowed
- In case you are using gmail you are limited with 500 mails per day (by google). Other providers may have similar
limitations, so please be careful.
- User is limited with 100 messages per day;

**Sketch:** [Email](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Email/Email.ino)

###Push Notifications

Push Notification widget allows you to send push notification from your hardware to your device. Currently it also
contains 2 additional options :

- **Notify when hardware offline** - you will get push notification in case your hardware went offline.
- **Offline Ignore Period** - defines how long hardware could be offline (after it went offline) before sending notification.
In case period is exceeded - "hardware offline" notification will be send. You will get no notification in case hardware
was reconnected within specified period.
- **Priority** high priority gives more chances that your message will be delivered without any delays.
See detailed explanation [here](https://developers.google.com/cloud-messaging/concept-options#setting-the-priority-of-a-message).

**WARNING** : high priority contributes more to battery drain compared to normal priority messages.

<img src="images/push.png" style="width: 77px; height:80px"/>

<img src="images/push_edit.png" style="width: 200px; height:360px"/>

Example code:
```cpp
Blynk.notify("Hey, Blynkers! My hardware can push now!");
```

Limitations :

- Maximum allowed body length is 120 symbols.
- Only 1 notification per 15 seconds is allowed

**Sketch:** [PushNotification](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/PushNotification/PushNotification_Button/PushNotification_Button.ino)

###Unicode in notify, email, push, ...

The library handles all strings as UTF8 Unicode. If you're facing problems, try to print your message to the Serial and see if it works (the terminal should be set to UTF-8 encoding). If it doesn't work, probably you should read about unicode support of your compiler.  
If it works, but your message is truncated - you need to increase message length limit (all Unicode symbols consume at least twice the size of Latin symbols).

###Increasing message length limit

You can increase maximum message length by putting on the top of your sketch (before Blynk includes):
```cpp
#define BLYNK_MAX_SENDBYTES 256 // Default is 128
```

## Interface

### Tabs
The only purpose of Tabs widget is to extend your project space. You can have up to 4 tabs.
Also you can drag widgets between tabs. Just drag widget on the label of required tab of tabs widget.

<img src="images/tabs_settings.png" style="width: 200px; height:360px"/>


### Menu
Menu widget allows you to send command to your hardware based on selection you made on UI. Menu
sends index of element you selected and not label string. Sending index is starts from 1.
It works same way as usual ComboBox element. You can also set Menu items
[from hardware side](http://docs.blynk.cc/#blynk-main-operations-change-widget-properties).

<img src="images/menu_edit.png" style="width: 200px; height:360px"/>

Example code:
```
switch (param.asInt())
  {
    case 1: { // Item 1
      Serial.println("Item 1 selected");
      break;
    }
    case 2: { // Item 2
      Serial.println("Item 2 selected");
      break;
    }    
  }
```

**Sketch:** [Menu](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Menu/Menu.ino)


###Time Input
Time input widget allows you to select start/stop time, day of week, timezone, sunrise/sunset formatted values
and send them to your hardware. Supported formats for time now are ```HH:MM``` and ```HH:MM AM/PM```.

Hardware will get selected on UI time as seconds of day (```3600 * hours + 60 * minutes```) for start/stop time.

<img src="images/time_input_settings.png" style="width: 200px; height:360px"/>

**Sketch:** [Simple Time Input for start time](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/TimeInput/SimpleTimeInput/SimpleTimeInput.ino)

**Sketch:** [Advanced Time Input](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/TimeInput/AdvancedTimeInput/AdvancedTimeInput.ino)

### Map

Map widget allows you set points/pins on map from hardware side. This is very useful widget in case you have
multiple devices and you want track their values on map.

You can send a point to map with regular virtual wrtei command :  

```cpp
Blynk.virtualWrite(V1, pointIndex, lat, lon, "value");
```

We also created wrapper for you to make suage of map simpler :

You can change button labels from hardware with :

```cpp
WidgetMap myMap(V1);
...
int index = 1;
float lat = 51.5074;
float lon = 0.1278;
myMap.location(index, lat, lon, "value");
```

Using save ```index``` allows you to override existing point value.

**Sketch:** [Basic Sketch](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Map/Map.ino)

###Table
Table widget comes handy when you need to structure similar data within 1 graphical element. It works as a usual table.

You can add a row to the table with :

```Blynk.virtualWrite(V1, "add", id, "Name", "Value");```

**Note :** Max number of rows in the table  is 100. When you reach the limit, table will work as FIFO (First In First Out)list.
This limit can be changed by configuring ```table.rows.pool.size``` property for local servers.

To highlight any item in a table by using its' index in a table starting from 0 :

```Blynk.virtualWrite(V1, "pick", 0);```

To clear the table at any time with:

```Blynk.virtualWrite(V1, "clr");```

<img src="images/table_settings.png" style="width: 200px; height:360px"/>

You can also handle other actions coming from table. For example, use row as a switch button.

```
BLYNK_WRITE(V1) {
   String cmd = param[0].asStr();
   if (cmd == "select") {
       //row in table was selected.
       int rowId = param[1].asInt();
   }
   if (cmd == "deselect") {
       //row in table was deselected.
       int rowId = param[1].asInt();
   }
   if (cmd == "order") {
       //rows in table where reodered
       int oldRowIndex = param[1].asInt();
       int newRowIndex = param[2].asInt();
   }
}
```

**Sketch:** [Simple Table usage](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Table/Table_Simple/Table_Simple.ino)

### Device Selector

Device selector is a powerful widget which allows you to update widgets based on one active device. This widget is particlularly helpful when you have a fleet of devices with similar functionality.

Imagine you have 4 devices and every device has a Temperature & Humidity sensor connected to it. To display the data for all 4 devices you would need to add 8 widgets.

With Device Selector, you can use only 2 Widgets which will display Temperature and Humidity based on the active device chosen in Device Selector.  

All you have to do is:
1. Add Device Selector Widget to the project
2. Add 2 widgets (for example Value Display Widget) to show Temperature and Humidity
3. In Widgets Settings you will be able assign them to Device Selector (Source or Target section)
4. Exit settings, Run the project.

Now you can change the active device in Device Selector and you will see that Temperature and Humidity values are reflecting the data updates for the device you just picked.

**NOTE : ** History Graph Widget and Webhook Widget will not work with Device Selector (yet).

## Sensors

### Accelerometer

Accelerometer is kind of [motion sensors](https://developer.android.com/guide/topics/sensors/sensors_motion.html)
that allows you to detect motion of your smartphone.
Useful for monitoring device movement, such as tilt, shake, rotation, or swing.
Conceptually, an acceleration sensor determines the acceleration that is applied to a device by measuring the forces
that are applied to the sensor. Measured in ```m/s^2``` applied to ```x```, ```y```, ```z``` axis.

In order to accept data from it you need to :

```cpp
BLYNK_WRITE(V1) {
  //acceleration force applied to axis x
  int x = param[0].asFloat();
  //acceleration force applied to axis y
  int y = param[1].asFloat();
  //acceleration force applied to axis y
  int z = param[2].asFloat();
}
```

Accelerometer doesn't work in background.

### Barometer/pressure

Barometer/pressure is kind of [environment sensors](https://developer.android.com/guide/topics/sensors/sensors_environment.html)
that allows you to measure the ambient air pressure.

Measured in in ```hPa``` or ```mbar```.

In oder to accept data from it you need to :

```cpp
BLYNK_WRITE(V1) {
  //pressure in mbar
  int pressure = param[0].asInt();
}
```

Barometer doesn't work in background.

### Gravity

Gravity is kind of [motion sensors](https://developer.android.com/guide/topics/sensors/sensors_motion.html)
that allows you to detect motion of your smartphone.
Useful for monitoring device movement, such as tilt, shake, rotation, or swing.

The gravity sensor provides a three dimensional vector indicating the direction and magnitude of gravity.
Measured in ```m/s^2``` of gravity force applied to ```x```, ```y```, ```z``` axis.

In oder to accept data from it you need to :

```cpp
BLYNK_WRITE(V1) {
  //force of gravity applied to axis x
  int x = param[0].asFloat();
  //force of gravity applied to axis y
  int y = param[1].asFloat();
  //force of gravity applied to axis y
  int z = param[2].asFloat();
}
```

Gravity doesn't work in background.

### Humidity

Humidity is kind of [environment sensors](https://developer.android.com/guide/topics/sensors/sensors_environment.html)
that allows you to measure ambient relative humidity.

Measured in ```%``` - actual relative humidity in percent.

In oder to accept data from it you need to :

```cpp
BLYNK_WRITE(V1) {
  // humidity in %
  int humidity = param.asInt();
}
```

Humidity doesn't work in background.

### Light

Light is kind of [environment sensors](https://developer.android.com/guide/topics/sensors/sensors_environment.html)
that allows you to measure level of light (measures the ambient light level (illumination) in lx).
In phones it is used to control screen brightness.

In order to accept data from it you need to :

```cpp
BLYNK_WRITE(V1) {
  //light value
  int lx = param.asInt();
}
```

Light doesn't work in background.

### Proximity

Proximity is kind of [position sensors](https://developer.android.com/guide/topics/sensors/sensors_position.html)
that allows you to determine how close the face of a smartphone is to an object.
Measured in ```cm``` - distance from phone face to object. However most of this sensors returns only FAR / NEAR information.
So return value will be ```0/1```. Where 0/LOW  is ```FAR``` and 1/HIGH is ```NEAR```.

In order to accept data from it you need to :

```cpp
BLYNK_WRITE(V1) {
  // distance to object
  int proximity = param.asInt();
  if (proximity) {
     //NEAR
  } else {
     //FAR
  }
}
```

Proximity doesn't work in background.

### Temperature

Temperature is kind of [environment sensors](https://developer.android.com/guide/topics/sensors/sensors_environment.html)
that allows you to measure ambient air temperature.
Measured in ```°C``` - celcius.

In order to accept data from it you need to :

```cpp
BLYNK_WRITE(V1) {
  // temperature in celcius
  int celcius = param.asInt();
}
```

Temperature doesn't work in background.

### GPS Trigger

GPS trigger widget allows easily trigger events when you arrive to or leave from some destination. This widget
will work in background and periodically will check your coordinates. In case your location is within/out required
radius (selected on widget map) widget will send ```HIGH```/```LOW``` command to hardware. For example, let's assume you have
GPS Trigger widget assigned to pin ```V1``` and option ```Trigger When Enter```. In that case when you'll arrive to destination
point widget will trigger ```HIGH``` event.

```cpp
BLYNK_WRITE(V1) {
  int state = param.asInt();
  if (state) {
      //You enter destination
  } else {
      //You leave destination
  }
}
```

More details on how GPS widget works you can read [here](https://developer.android.com/guide/topics/location/strategies.html).

GPS trigger widget works in background.

### GPS Streaming

Useful for monitoring smartphone location data such as latitude, longitude, altitude and speed (speed could be often 0  
in case smartphone doesn't support it).

In order to accept data from this widget you need to :

```cpp
BLYNK_WRITE(V1) {
  float latitude = param[0].asFloat();
  float longitude = param[1].asFloat();
  float altitude = param[2].asFloat();
  float speed = param[3].asFloat();
}
```

or you can use prepared wrapper ```GpsParam``` :

```cpp
BLYNK_WRITE(V1) {
  GpsParam gps(param);
  // Print 6 decimal places for Lat
  Serial.println(gps.getLat(), 7);
  Serial.println(gps.getLon(), 7);

  Serial.println(gps.getAltitude(), 2);
  Serial.println(gps.getSpeed(), 2);
}
```

GPS Streaming works in background.

**Sketch:** [GPS Stream](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/GPS_Stream/GPS_Stream.ino)

## Other

### Bridge

Bridge can be used for Device-to-Device communication (no app. involved). You can send digital/analog/virtual write commands from one device to another, knowing it's auth token.
At the moment Bridge widget is not required on application side (it is mostly used for indication that we have such feature).  
**You can use multiple bridges to control multiple devices.**

<img src="images/bridge.png" style="width: 77px; height:80px"/>

<img src="images/bridge_edit.png" style="width: 200px; height:360px"/>

Bridge widget takes a virtual pin, and turns it into a channel to control another device. It means you can control any virtual, digital or analog pins of the target device.
Be careful not to use pins like ```A0, A1, A2 ...``` when communicating between different device types, as Arduino Core may refer to wrong pins in such cases.


Example code for device A which will send values to device B :
```cpp
WidgetBridge bridge1(V1); //Initiating Bridge Widget on V1 of Device A
...
void setup() {
    Blynk.begin(...);
    while (Blynk.connect() == false) {
        // Wait until Blynk is connected
    }
    bridge1.digitalWrite(9, HIGH); // will trigger D9 HIGH on Device B. No code on Device B required
    bridge1.analogWrite(10, 123);
    bridge1.virtualWrite(V1, "hello"); // you need to write code on Device B in order to receive this value. See below
    bridge1.virtualWrite(V2, "value1", "value2", "value3");
}

BLYNK_CONNECTED() {
  bridge1.setAuthToken("OtherAuthToken"); // Token of the hardware B
}
```

IMPORTANT: when performing ```virtualWrite()``` with Bridge Widget, Device B will need to process the incoming data from Device A.
For example, if you are sending value from Device A to Device B using ```bridge.virtualWrite(V5)``` you would need to use this handler on Device B:

```cpp
BLYNK_WRITE(V5){
    int pinData = param.asInt(); //pinData variable will store value that came via Bridge
}
```

Keep in mind that ```bridge.virtualWrite``` doesn't send any value to mobile app. You need to call ```Blynk.virtualWrite``` for that.

**Sketch:** [Bridge](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Bridge/Bridge.ino)

### Eventor
Eventor widget allows you to create simple behaviour rules or **events**.
Let's look at a typical use case: read temperature from DHT sensor and send push notification when temperature is over a certain limit :  

```cpp
  float t = dht.readTemperature();
  if (isnan(t)) {
    return;
  }
  if (t > 40) {
    Blynk.notify(String("Temperature is too high : ") + t);
  }
```

With Eventor you don't need to write this code. All you need is to send value from sensor to server :

```cpp
  float t = dht.readTemperature();
  Blynk.virtualWrite(V0, t);
```
Don't forget that ```virtualWrite``` commands should be wrapped in the timer and can't be used the main loop.

Now configure new **Event** in Eventor widget:

<img src="images/eventor/eventor_for_temp_example.png" style="width: 200px; height:360px"/>

**NOTE** Don't forget to add notification widget.

Eventor comes handy when you need to change conditions on the fly without re-uploading new sketch on
the hardware. You can create as many **events** as you need.
Eventor also supports Timer events. For example, you can set pin ```V1``` ON/HIGH at 21:00:00 every Friday.
With Eventor Time Event you can assign multiple timers on same pin, send any string/number, select days and timezone.

In order to remove created **event** please use swipe. You can also swipe out last element in the Event itself.

<img src="images/eventor/eventor_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [Eventor](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Eventor/Eventor.ino)


**NOTE** : Events are triggered only once when the condition is met.
However there is one exclusion:
Let's consider simple event as above ```if (temperature > 40) send notification ```.
When temperature goes beyond 40 threshold - notification action is triggered. If temperature continues to stay above the 40 threshold no actions will be triggered. But if ```temperature``` goes below threshold and then passes it again -
notification will be sent again (there is no 15 sec limit on Eventor notifications).

### RTC

Real-time clock allows you to get time from server. You can preselect any timezone on UI to get time on hardware in required locale.
No pin required for RTC widget.

<img src="images/rtc_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [RTC](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/RTC/RTC.ino)

### BLE

Widget for enabling Bluetooth Low Energy support. At the moment BLE widget supported only for Android and requires
internet connection in order to login and load your profile. However this will be fixed soon. Also some Blynk
widget not allowed with BLE widget.

Blynk currently support bunch of different modules. Please check sketches below.

<img src="images/ble_settings.png" style="width: 200px; height:360px"/>

**Sketches:** [BLE](https://github.com/blynkkk/blynk-library/tree/master/examples/Boards_Bluetooth)

### Bluetooth

Widget for enabling Bluetooth support. At the moment Bluetooth widget supported only for Android and requires
internet connection in order to login and load your profile. However this will be fixed soon. Also some Blynk
widget not allowed with Bluetooth widget.

Blynk currently support bunch of different modules. Please check sketches below.

<img src="images/ble_settings.png" style="width: 200px; height:360px"/>

**Sketches:** [Bluetooth](https://github.com/blynkkk/blynk-library/tree/master/examples/Boards_Bluetooth)

### Music Player

Simple UI element with 3 buttons - simulates music player interface. Every button sends it's own command to hardware :
```play```, ```stop```, ```prev```, ```next```.

You can also change widget play/stop state via [Change Widget Property](http://docs.blynk.cc/#blynk-main-operations-change-widget-properties)
feature with ```isOnPlay``` property.


**Sketch:** [Music Player](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Player/Player.ino)

### Webhook

Webhook is a widget for 3-d party integrations. With webhook widget you can send HTTP/S requests to any 3-d party server
or device that has HTTP/S API (Philips Hue for instance).

Any write operation from hardware side will trigger webhook widget (same way as for eventor). You can also trigger
webhook from application side in case control widget assigned to same pin as webhook. You can trigger 3-d party service
with single button click.

For example, imagine a case when you want to send data from your hardware not only to Blynk but also to Thingspeak server.
In typical, classic use case you'll need to write code like this (this is minimal and not full sketch) :

```
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

With webhook widget this is not necessary anymore. All you need just fill below fields :

<img src="images/webhook_settings.png" style="width: 200px; height:360px"/>

And do usual :  

```
Blynk.virtualWrite(V0, value);
```

where V0 is pin assigned to webhook widget.

Also you can use usual Blynk placeholders for pin value in body or url, for example :

```
https://api.thingspeak.com/update?api_key=xxxxxx&field1=/pin/
```

or for body

```
["/pin/"]
```

You can also refer to specific index of multi value pin (multi pin supports up to 5 values) :

```/pin[0]/```,```/pin[1]/```, ```/pin[2]/```

Another cool thing about webhook is that you can make GET requests from Blynk Server side and return response directly to
your hardware. The beauty here is that you don't need to code request to 3-d party service. Imagine a case when you want to get
weather from some 3-d party service. For example, you have an url
```http://api.sunrise-sunset.org/json?lat=33.3823&lng=35.1856&date=2016-10-01```, you can put it in widget, select ```V0``` pin,
and do usual :  

```
BLYNK_WRITE(V0){
  String webhookdata = param.asStr();
  Serial.println(webhookdata);
}
```

Now, every time you'll trigger ```V0``` pin (with ```Blynk.virtualWrite(V0, 1)``` from hardware side or with control widget
assigned to ```V0```) - ```BLYNK_WRITE(V0)``` will be triggered.

**NOTE :** usually 3-d party servers returns big responses, so you have to increase hardware maximum allowed message size with
```#define BLYNK_MAX_READBYTES 1024```. Where ```1024``` - is maximum allowed message size.

**NOTE :** Blynk cloud has limitation for webhook widget - you are allowed to send only 1 request per second. You can
 change this on local server with ```webhooks.frequency.user.quota.limit```. Please be very careful using webhooks,
 as many resources not capable to handle even 1 req/sec, so you may be banned on some of them. For example thingspeak
 allows to send 1 request per 15 seconds.

 **NOTE :** In order to avoid spamming Blynk Webhook has one more limitation - in case your webhook requests were failed 10 times
 in row your webhook widget will be stopped. In order to resume it you need to open widget and save it again. Failed requests
 are requests that return status code that are not equal to 200 or 302.
