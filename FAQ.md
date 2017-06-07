#FAQ

- J'ai soutenu Blynk sur Kickstarter. Où sont mes widgets et pourquoi l'application est gratuite ?
> L'Application est gratuite parce que sinon vous devriez payer pour la télécharger. C'est comme cela que l'AppStore et le Google Play fonctionnent.
> La version actuelle de Blynk a un nombre limité de widgets. Nous avons décidé de les rendre gratuit pour tous jusqu'à ce que nous implémentions une boutique. Après cela, chaque widget sera payé. Néanmoins, chaque investisseur les obtiendra gratuitement (en accord avec l'engagement).

- Qu'est-ce que le Cloud Blynk ?
> Le Cloud Blynk est un logiciel open-source écrit en Java, utilisant des sockets TCP/IP et TCP/IP sécurisés (pour les hardwares le supportant) et tournant sur notre serveur. Les applications iOS et Android de Blynk se connectent au Cloud BLynk par défaut. L'accès est gratuit pour tous les utilisateurs de Blynk. Nous fournissons aussi une distribution du serveur privé pour ceux qui voudraient [l'installer localement](http://docs.blynk.cc/#blynk-server).

- Combien coûte l'accès au serveur du Cloud Blynk ?
> C'est gratuit pour tous les utilisateurs de Blynk.

- Puis-je lancer le serveur Blynk localement ?
> Oui. Ceux d'entre vous, souhaitant une meilleure sécurité ou n'ont pas de connexion internet, peuvent installer le serveur Blynk local et le faire tourner sur votre propre réseau local. Le serveur Blynk est Open-Source et prend moins de quelques secondes à se déployer. Toutes les instructions et fichiers sont [ici](http://docs.blynk.cc/#blynk-server).

- Quels sont les pré-requis pour faire tourner le serveur privé Blynk ?
> Pour faire tourner le serveur privé Blynk, vous aurez seulement besoin de Java Runtime Environment.

- Puis-je faire tourner le serveur Blynk sur Raspberry Pi ?
> Bien sûr! [Voici les instructions - En Anglais](http://docs.blynk.cc/#blynk-server-how-to-run-local-blynk-server-launch-blynk-server-on-raspberry-pi).

- Est-ce que l'application Blynk fonctionne à travers le Bluetooth ?
> Non, mais c'est prévu pour les prochaines versions.

- Est-ce que Blynk supporte Ethernet / Wi-Fi / UART ?
> Oui, il les supporte tous. La liste complète des hardwares et shields supportés est disponible [ici](http://docs.blynk.cc/#supported-hardware).

- Je n'ai aucun shield. Puis-je utiliser Blynk avec mon ordinateur ?
> Oui, vous pouvez utiliser Blynk simplement avec un câble USB. Il y a des instructions [étape-par-étape](http://docs.blynk.cc/#other-hardware-connect-over-usb) sur comment s'y prendre.

- Est-ce que Blynk peut gérer plusieurs Arduino ?
> Oui. Il y a actuellement 2 manières de s'y prendre :
> - Vous pouvez utiliser le même [Jeton d'Authentification](http://docs.blynk.cc/#getting-started-getting-started-with-application-auth-token) pour chaque hardware. Dans ce cas vous pouvez contrôler plusieurs hardwares à partir d'un seul tableau de bord.
> - Vous pouvez le faire en utilisant la [fonctionnalité Bridge](http://docs.blynk.cc/#widgets-other-bridge) qui vous permet d'envoyer des messages d'un hardware à un autre.

- Est-ce que le serveur Blynk stocke les données des capteurs lorsque l'application passe hors-ligne ?
> Oui, chaque commande que le hardware envoie au serveur est stocké. Vous pouvez utiliser le widget [Graphique d'Histoirque](http://docs.blynk.cc/#widgets-displays-history-graph) afin de les visualiser.
