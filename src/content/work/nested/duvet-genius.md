---
title: Projet Carte à Puce
publishDate: 2023-10-08 15:22:56
img: /assets/stock-3.jpg
img_alt: Cartes à puces
description: |
  Projet carte à puce à distance.
tags:
  - BIOS
  - Installations/Documentations
  - Ligne de commande
  - Setup
---

## But du projet :

Dans le cadre de ce projet d'entreprise, nous étions confrontés à des utilisateurs qui perdaient ou oubliaient leurs cartes à puce nécessaires pour se connecter. Cette situation devenait embarrassante, notamment lorsque les employés travaillaient à distance ou loin du site de Flandre. Cependant, grâce à l'implémentation du logiciel, nous avons désormais la possibilité d'utiliser, à partir des différentes machines situées sur les sites les plus proches des utilisateurs, un logiciel permettant l'octroi à distance de droits sur des cartes de prêt.

## Ma participation au projet :

<!-- > Désinstallations :

-désinstallation du driver:

-C:\ Program Files\cap_usbip\bin : usbip.exe uninstall

-désinstallation du service :

-« C:\Program Files\cap_usbip\nssm-2.24\win64 » :  deinstall_cap_service.bat

-Suppression du certificat « USBIP Test » dans :

-ordinateur local/Autorités de certification racines de confiance et ordinateur local/Editeurs approuvés

> Installation du poste debian serveur de lecteur de carte à puce :

L’installation à la DRSM IdF se pratique sur des PC HP Prodesk 400 G4 SFF du PEI 2018 munis de
lecteur de carte à puce gemplus CT30, l’installation se fait par clonage depuis un partage NFS pour
des raisons de rapidité, mais peut être fait depuis tout média. 

Les premiers essais ont eu lieu sur des Acer Veriton du PEI 2019 et le passage aux HP (moins encombrants et dont le nombre était suffisant) n’a pas présenté de difficulté majeure. -->

> Les instructions à suivre étaient les suivantes :

- Brancher le PC :

écran, clavier, souris, réseau, alim.

- Configuration du Bios :

- Allumer le PC et faire « Esc » puis « F10 » pour entrer dans le bios.

- Aller dans « Advanced » (avec les flèches gauche/droite)

- Choisir « Boot Options » (avec les flèches haut/bas)

- Dans l’option « After Power Loss » sélectionner « Power On »

"After Power Loss" ou "After Power On" dans le BIOS fait référence à une option permettant de définir le comportement de l'ordinateur après une perte de courant et son redémarrage

- Sortir de Boot Options (Esc)

- Choisir « Secure Boot Configuration »

- Dans l’option « Configure Legacy Support and Secure Boot » sélectionner « Legacy Support Enable and Secure Boot Disable » :


- Legacy Support Enable (Activation du support hérité) :

Cette option permet à votre ordinateur de démarrer en utilisant des méthodes plus anciennes ou héritées pour interagir avec le matériel et les logiciels. Cela signifie que votre ordinateur peut prendre en charge des systèmes d'exploitation et des périphériques plus anciens qui ne prennent pas en charge les dernières normes de démarrage et de fonctionnement.

- Secure Boot Disable (Désactivation du démarrage sécurisé) : 

Secure Boot est une fonctionnalité conçue pour sécuriser le processus de démarrage de l'ordinateur en n'autorisant que l'exécution de logiciels signés et approuvés. Lorsque vous désactivez Secure Boot, vous autorisez le démarrage de systèmes d'exploitation ou de logiciels non signés ou non approuvés par le fabricant.



-Sortir (Esc puis Esc et Répondre « Yes » à la question « Save Changes ? »)

> Installation du poste :

- Mettre la cle « Ventoy » dans un port usb et redémarrer le poste en tapant immédiatement et de
façon répétitive sur « F9 »

- Choisir le démarrage Legacy sur la clé (Kingston ?)

- Dans le menu Ventoy, choisir « rescuezilla-2.3.1-64bit.impish.iso »

- Choisir « Français »

- Choisir « Démarrer Redo Backup »

- Choisir « Restaurer »

Etape 1 :

- Choisir « Partagé sur un réseau »
- Cliquer sur « Dossier partagé indiqué ci-dessous » et choisir « NFS »
- Dans « Emplacement du serveur ou du partage » renseigner l’adresse IP
- Dans « Exported path » renseigner « /partage/NFS »

Etape 2 :

- Cliquer sur « Parcourir … »

- Choisir le dossier « usbip »

- Sélectionner l’image à restaurer « 2022-11-23-0806-img-rescuezilla » avec un « simple clic »
puis « Valider »

- Faire « Suivant »

Etape 3 :

- Sélectionner le disque cible (capacité 931,5GB en général)

Etape 4 :

- Laisser les choix (restaurer les 2 partions et écraser la table de partition)

Etape 5 :

- « Suivant »
Fin de l’installation (temps passé de l’ordre de 10 minutes par poste).

## Principe de fonctionnement du logiciel :

Un poste fixe muni d’un lecteur de carte à puce, alimenté et mis sur le réseau (pas d’autre
périphérique) est placé sur chaque site où l’on aura à prêter des cartes.

Ce poste doit être configuré en lui indiquant son hostname et la/les plage/s des adresses des postes
des administrateurs EAM qui pourront voir le lecteur de cartes.

Dès que ce dernier est mis sous tension, il démarre, récupère sa configuration réseau par DHCP,
recherche le lecteur de carte qui lui est connecté, lance le service usbipd, y lie le lecteur de carte et
en fait la déclaration sur un site web dédié à la région.

Ensuite, toutes les 2 minutes, il inspecte de
nouveau sa configuration et met à jour sa déclaration (changement de prise usb , changement
d’adresse, …). L’arrêt du poste se fait par un appui bref sur la touche de mise en route.

Sur le site web de la région, les informations transmises par chaque poste sont stockées dans une
table d’une base de données.

Le driver usbip est installé sur chaque poste de travail des administrateurs EAM qui auront à prêter
des cartes sur site distant.

Si l’administrateur EAM n’est pas administrateur de son poste, il faut aussi lui installer le service cap-
usbip.

Lorsque l’administrateur EAM désire accéder au lecteur distant, il lance le script gestion_cap.bat (si il
est administrateur du poste) ou bien gestion_user_cap.bat.

<img
					alt="Projet1"
					width="1200"
					height="620"
					src="/assets/image1_pf.jpg"
				/>

Le script interroge le site web ou pouvoir proposer les sites déclarés et il suffit ensuite de choisir un
de ces sites puis des actions sont proposées:
(Les réponses lui seront données de façon synchrone dans la fenêtre DOS dans le cas administrateur
du poste, ou bien sous forme de message (délai jusqu’à 10 secondes) dans les autres cas).

L : pour obtenir les informations sur la disponibilité du lecteur (qui ne peut être vu que d’un
seul poste à la fois).

-Lecteur disponible :
<img
					alt="Projet2"
					width="1200"
					height="620"
					src="/assets/image2_pf.jpg"
				/>
-Lecteur indisponible :
<img
					alt="Projet2"
					width="1200"
					height="620"
					src="/assets/image3_pf.jpg"
				/>
M : Monter le lecteur pour le rendre utilisable sur le poste :
<img
					alt="Projet2"
					width="1200"
					height="620"
					src="/assets/image4_pf.jpg"
				/>
D : Démonter le lecteur pour le rendre disponible pour un autre utilisateur :
<img
					alt="Projet2"
					width="1200"
					height="620"
					src="/assets/image5_pf.jpg"
				/>
A : pour changer de site de lecteur de carte à puce

Q : pour sortir de l’utilitaire
<img
					alt="Projet2"
					width="1200"
					height="620"
					src="/assets/image6_pf.jpg"
				/>

> La solution de déport de lecteur de carte à puce sur IP à la DRSM-IdF se compose ainsi des éléments suivants :

1° un PC HP 400 G4 SFF (PEI 2018) avec un lecteur gemplus (les lecteurs transparents)
Le PC est installé avec l’image XX-img-rescuezilla.zip installé par rescuzilla (installation
environ 5 min par le réseau local).

2° L’environnement sur le poste de travail de l’administrateur EAM
(XX_cap_usbip.zip) à deziper dans « C:\Program Files » pour obtenir le répertoire
« C:\Program Files\cap_usbip »

Installation du driver :
- installer le certificat usbip_test.pfx (mot de passe: usbip) en double cliquant dessus dans
« C:\Program Files\cap_usbip\bin »:

(utiliser l'option: placer tous les certificats dans le magasin suivant)
  ordinateur local / Autorités de certification racines de confiance et
  ordinateur local / Editeurs approuvés

- dans une fenêtre DOS Administrateur, dans le répertoire « C:\Program
Files\cap_usbip\bin » entrer : usbip.exe install
- supprimer le certificat qui avait été installé par usbip_test.pfx (USBIP Test) dans les
magasins suivants
  ordinateur local / Autorités de certification racines de confiance et
  ordinateur local / Editeurs approuvés
-
  L’utilisation avec les droits adminstrateur du poste se fait en utilisant le script « C:\Program
Files\cap_usbip\bin\gestion_cap.bat » (à lancer par bouton droit « Exécuter en tant
qu’administrateur »)

  L’utilisation sans droit administrateur du poste nécessite l’installation du service cap-usbip et
se fait alors en utilisant le script « C:\Program Files\cap_usbip\bin\gestion_user_cap.bat » (à
lancer en double-cliquant dessus)

Installation du service cap-usbip par un administrateur:

- Dans une fenêtre dos administrateur, dans le répertoire « C:\Program
Files\cap_usbip\nssm-2.24\win64 » : exécuter install_cap_service.bat , puis lancer le
service ou rebooter.

3° Le site web LAMP http://cap

Site (ici sur un debian) dont les sources sont dans XX_cap.tgz
  script de création de la base dans ./home/drsm/cap,
  sources du site dans ./var/www/html/cap,
  configuration dans ./etc/apache2/sites-available/040-cap.conf

Ces éléments sont configurés pour la DRSM IdF et doivent être adaptés selon les indications qui
suivent.

Principe de fonctionnement :

Lorsque le PC lecteur de carte est en fonction, il se connecte régulièrement au site web (cf.
/etc/crontab) pour se déclarer et déclarer le lecteur de carte qui lui est connecté.
Le site web enregistre alors l’information dans sa base de données MariaDB.

Les administrateurs peuvent trouver l’adresse du site à l’adresse http://cap/capliste.php pour s’y
connecter (utilisateurs drsm et root) et editer sous root /etc/hosname pour y mettre le nom du site
puis rebooter le serveur/lecteur.
<img
					alt="Projet2"
					width="1200"
					height="620"
					src="/assets/image7_pf.jpg"
				/>

> Côté poste de travail désirant utiliser le lecteur :

- Une demande d’habilitation (par l’adresse IP du poste) doit être faite pour qu’un
administrateur l’autorise en mettant à jour /etc/hosts.allow

- Dans le répertoire « C:\Program Files\cap_usbip\bin » :

  Si l’utilisateur possède les droits admin sur son poste :

  Il exécute (bouton doit) gestion_cap.bat

  Sinon :
  Il exécute gestion_user_cap.bat
  Le service cap-usbip devra être actif sur son poste

La liste des sites enregistrés lui sont proposés, il choisit celui qu’il désire et peut monter le lecteur, le
démonter ou lister les ressources disponibles sur ce site (le lecteur ne peut bien sûr n’être monté que
sur un seul PC à la fois.

=================

Adaptations nécessaires : suivant le nombre de déploiement envisagés, refaire les
livrables
Côté serveur/Lecteur :
o Modifier le mot de passe de la base MariaDB dans les scripts *.sh et *.sql de
/home/drsm/cap, ainsi que dans /www/html/cap/bib.php (sur serveur web)
o Modifier le mot de passe des utilisateurs drsm et root, ainsi que leurs fichiers
.ssh/autorized_keys
o Editer /etc/hosts.allow sur les serveurs/lecteur
Modifier les références au site web http://cap.drsm-idf.ramage dans :
o « C:\Program Files\cap_usbip\bin\gestion_cap.bat » et gestion_user_cap.bat
o /root/cap_register.sh (sur les serveurs/lecteurs)
o Adapter le fichier /etc/apache2/sites-available/040-cap.conf (sur le serveur web)

<!-- > ANNEXE:

Notes sur l’installation serveur linux debian :

Ces notes ont servi à la construction du clone utilisé dans ce package et sont données à titre d’archive
en cas de reconstruction ou modification de l’installation.

#apt-get install net-tools
Installation
1° Install the usbip package.
#apt-get install usbip
#modeprobe usbip-host
#usbipd -D

2° Usage

Server setup
The server should have the physical USB device connected to it, and the usbip_host USB/IP
kernel module loaded. Then start and enable the USB/IP systemd service usbipd.service. The
daemon will accept connections on TCP port 3240.

List the connected devices:

$ usbip list -l

Bind the required device. For example, to share the device having busid 1-1.5:

Note: The device needs to be bound again after suspending.

$ usbip bind -b 1-1.5

To unbind the device:

$ usbip unbind -b 1-1.5

After binding, the device can be accessed from the client.
Binding with systemd service

In order to make binding persistent following systemd template unit file can be used:

/etc/systemd/system/usbip-bind@.service

[Unit]

Description=USB-IP Binding on bus id %I
After=network-online.target usbipd.service
Wants=network-online.target
Requires=usbipd.service
#DefaultInstance=1-1.5

[Service]

Type=simple
ExecStart=/usr/bin/usbip bind -b %i
RemainAfterExit=yes
ExecStop=/usr/bin/usbip unbind -b %i
Restart=on-failure

[Install]

WantedBy=multi-user.target
So, e.g., to share the device having busid 1-1, one should start/enable usbip-bind@1-

1.service.

Client setup
Make sure the vhci-hcd kernel module is loaded.
Then list devices available on the server:

$ usbip list -r server_IP_address

Attach the required device. For example, to attach the device having busid 1-1.5:

$ usbip attach -r server_IP_address -b 1-1.5

Tip: To connect to an alternate TCP port use --tcp-port port.
Disconnecting devices

A device can be disconnected only after detaching it on the client.

List attached devices:

$ usbip port

Detach the device:

$ usbip detach -p port_number

Unbind the device on the server:

$ usbip unbind -b busid

Note: USB/IP by default requires port 3240 to be open. If a firewall is running, make sure that
this port is open. For detailed instruction on configuring the firewall, go to
Category:Firewalls.

Tips and tricks

Binding by vendor/device ID

If bus ids are inconsistent and dynamically assigned at each system boot, binding by
vendor/device ID can be used alternatively:
/etc/systemd/system/usbip-bind@.service

[Unit]
Description=USB-IP Binding device id %I
After=network-online.target usbipd.service
Wants=network-online.target
Requires=usbipd.service

[Service]
Type=simple
ExecStart=/bin/sh -c &quot;/usr/sbin/usbip bind --$(/usr/sbin/usbip list -p -l | grep &#39;#usbid=%i#&#39; |
cut &#39;-d#&#39; -f1)&quot;
RemainAfterExit=yes
ExecStop=/bin/sh -c &quot;/usr/sbin/usbip unbind --$(/usr/sbin/usbip list -p -l | grep &#39;#usbid=%i#&#39;
| cut &#39;-d#&#39; -f1)&quot;
Restart=on-failure

[Install]
WantedBy=multi-user.target
So, e.g., to share the device having vendor/device ID 0924:3d68, one should start/enable
usbip-bind@0924:3d68.service.
Note: Such a binding method cannot work correctly for multiple devices with the same
vendor/device ID.
Then client setup will be like this:
Linux clients
$ usbip attach -r server_IP_address --$(/usr/sbin/usbip list -p -r server_IP_address | grep
&#39;#usbid=0924:3d68#&#39; | cut &#39;-d#&#39; -f1)
Note: If the previous command fails, check if -p flag in usbip list -p -r server_IP_address is
working properly. If not, use the following line instead:

$ usbip attach -r server_IP_address -b $(/usr/sbin/usbip list -p -r server_IP_address | grep
&#39;0924:3d68&#39; | cut &#39;-d:&#39; -f1 | awk &#39;{print $1}&#39;)
Windows clients

c:\&gt; for /f &quot;tokens=1 delims=:, &quot; %a in (&#39;usbip list -r server_IP_address ^| findstr /r
/c:&quot;0924:3d68&quot;&#39;) do start usbip attach -r server_IP_address -b %a
Sharing devices configured with files in /etc/..

usbip-systemdAUR provides systemd service files for binding by vendor/device id and for
connecting by hostname and vendor/device id.

Server setup

For each device create a device.conf in /etc/usbip/bind-devices/ with USBIP_DEVICE set to
the vendor/product id, e.g.:

/etc/usbip/bind-devices/example-device.conf

USBIP_DEVICE=0924:3d68

To bind a cofigured device start/enable the service usbip-bind@example-device.service

Client setup

For each host/device create a device.conf in /etc/usbip/remote-devices/ with HOST set and
USBIP_DEVICE set to the vendor/product id, e.g.:

/etc/usbip/remote-devices/example-device.conf

USBIP_HOST=example-host

USBIP_DEVICE=0924:3d68

Make sure your server is running and the configured device is bound, then start or stop the
service usbip@example-device.service

See also

Official USB/IP project site

Linux Kernel &quot;README for usbip-utils&quot;

&quot;How To Setup and use USB/IP&quot;

&quot;USB/IP for Windows&quot;

usbip(8)

usbipd(8)

Category: Storage -->
