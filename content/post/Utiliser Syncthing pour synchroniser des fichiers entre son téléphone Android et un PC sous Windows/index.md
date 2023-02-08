---
title: "Utiliser Syncthing pour synchroniser des fichiers entre son téléphone Android et un PC sous Windows"
date: "2023-01-08"
categories: 
- Dev
tags: 
- Syncthing
- Android
- Windows
- Photos
- Synchronisation de fichiers
- Sauvegardes de fichiers
---

Le tutoriel ci-dessous est une introduction à l'utilisation du logiciel libre Syncthing :
Nous allons synchroniser le répertoire photo de notre téléphone Android avec un PC sous Windows, de manière à disposer en permanence d'une copie récente pour le cas où le téléphone cesserait de fonctionner, serait cassé, volé, etc.

<!--more-->

> **IMPORTANT :** Il s'agit d'une synchronisation et non d'une simple copie. Si un fichier est supprimé d'un coté il disparaît aussi de l'autre.
> Pour archiver les photos ailleurs sans les retirer du téléphone, il faudra les copier, et non les déplacer.

*Qu'est-ce que [Syncthing](https://syncthing.net/) ?*  

> Il s'agit d'un logiciel de synchronisation, qui permet de synchroniser des fichiers entre 2 appareils, à chaque fois que ces 2 appareils parviennent à se voir (par exemple lorsqu'ils sont sur le même wifi à la maison).
> 
> (C'est particulièrement utile pour les sauvegardes de téléphone car elles prennent du temps, nécessites en câble usb, les fichiers sont répartis dans plusieurs répertoires... en gros on ne les fait jamais)
> 
> Le principe est similaire à celui de Dropbox ou de Google Drive, à la différence près que les fichiers ne sont pas conservés ailleurs que sur vos appareils.
> 
> Il s'agit en outre d'un logiciel libre, c'est à dire que son code source est auditable et consultable par tous, c'est une garantie qu'il ne fera jamais rien de problématique avec vos données. Plus d'informations sur [Wikipedia : Logiciel libre](https://fr.wikipedia.org/wiki/Logiciel_libre).


*Pour aller plus loin :*  
> Syncthing peut être utilisé pour beaucoup d'autres tâches que ce que propose ce tutoriel :
> 
> - Sauvegarder tous les fichiers importants de son téléphone sur un ordinateur (photos, etc.) à chaque fois que les 2 sont à proximité ;
> - Conserver une copie conforme de documents importants sur 2 ordinateurs :
>    - Photos souvenir ;
>    - Fiches de paie ;
>    - etc.


# 1. Installation de Syncthing sur le PC sous Windows

Allez sur le site officiel de Syncthing : [https://syncthing.net/](https://syncthing.net/) 

1. Menu **"Download"** en haut
2. Cliquez sur **"Synctrayzor"** (il s'agit de la version Windows de Syncthing)
3. Le lien va vous envoyer sur une page du site Github, celle de la dernière version de Synctrayzor :  
   Téléchargez le fichier **"SyncTrayzorSetup-x64.exe"** en bas de la page (x86 pour un PC très ancien).

Lancez le _.exe_ - Windows demandera plusieurs autorisations : Tout autoriser.

L'application se lance :

Pop-up : **"Allow anonymous usage reporting?"** (autoriser les rapports anonymes ?)  
Vous pouvez cliquer sur **"Yes"** ça ne présente aucun risque et ça les aide à débugger :

![Capture d'écran du pop-up : "Allow anonymous usage reporting?" (autoriser les rapports anonymes ?)](syncthing_windows_1ere_ouverture.jpg)


Cliquez en haut sur **"English"** et choisir **"Français"** pour avoir l'interface en français :

![capture d'écran du choix de la langue ](syncthing_windows_choix_langue.jpg)

Encart vert demandant : **"Voulez-vous créer un login/password ?"**  
C'est une fonctionnalité avancée, vous pouvez mettre **"Non"**.

![capture d'écran de l'application configurée et prête à l'usage](syncthing_windows_pret_a_usage.jpg)

*Optionnel :*  
Allez dans **"Action"** > **"Configuration"** et donnez un nom plus sympa à l'ordi (défaut : user-PC), exemple : **"PC_DE_JOJO"**


Tout est prêt, on passe au téléphone.


# 2. Installation sur un téléphone sous Android

L'application officielle est disponible sur le [Play Store de Google](https://play.google.com/store/apps/details?id=com.nutomic.syncthingandroid) ou sur le store open-source [F-Droid](https://f-droid.org/packages/com.nutomic.syncthingandroid/).

Le store F-Droid garantit que c'est un logiciel libre et permet de conserver un tout petit peu de vie privée vis à vis de Google.  
Mais sinon c'est le même logiciel qui est installé (perso je passe par F-Droid dès que je peux).

## 2.1. Installation du store F-DROID

Allez via un navigateur sur [le site F-DROID : f-droid.org/fr/](f-droid.org/fr/).

Choisissez **"télécharger F-DROID"**, ça va installer le store sur le téléphone :  
Le téléphone va demander l'autorisation d'installer un paquet non sécurisé, faites **"paramètres"** > **"autoriser"** (l'autorisation ne vaut que pour cette fois, elle sera redemandée pour d'autres installations).

## 2.2. Installation du logiciel Syncthing depuis F-DROID

Ouvrez l'application F-DROID, chercher Syncthing, cliquez sur **"Installer"**, le téléphone va redemander l'autorisation.

## 2.3. Configuration

Une fois Syncthing installé, lancer l'application :

**"Permission de stockage"** :  
Cliquez sur **"donner la permission"** sur tout.

**"Permission de géolocalisation"** :   
Fonction avancée, pas la peine de l'activer.

Pop-up **"Syncthing est désactivé - voulez-vous changer les conditions de fonctionnement ?"** :  
Cliquez sur **"modifier"** pour entrer dans la configuration.  

Plusieurs configurations sont possibles, la plus courante est d'indiquer à syncthing de ne se lancer que lorsque votre téléphone est sur un réseau wifi, pour économiser de la data :

- **Exécuter en Wifi** : oui
- **Exécuter en données mobiles (3G...)** : non 

Vous pouvez aussi demander que l'exécution ne se fasse que lorsque le téléphone est en charge, pour économiser de la batterie :

- **"Exécuter uniquement lorsque le périphérique est alimenté par :"**
    - **"Secteur"** : permet que l'appli ne se lance qu'en charge ;
    - **"Secteur et batterie"** : L'appli se lance quand elle veut.

_Cas particulier :_  
Si vous avez l'habitude de connecter votre ordinateur à internet en utilisant le mode _"point d'accès Wifi"_ du téléphone, vous pouvez utiliser la configuration suivante, elle fonctionne :

- **Exécuter en Wifi** : non
- **Exécuter en données mobiles (3G...)** : oui 

*Optionnel :*  
Allez dans le menu en haut à gauche **"---"** > **"Paramètres"** > **"Options Syncthing"** et donnez un nom plus sympa au téléphone, ex : **"TEL_DE_JOJO"**

Tout est prêt, il ne reste plus qu'à les présenter l'un à l'autre.


# 3. Autoriser les 2 appareils à échanger entre eux

Dans une synchronisation Syncthing, il faut que chaque appareil autorise l'autre à lui adresser la parole, pour qu'ils puissent ensuite se proposer des partages de fichiers :

- Sur le PC, ouvrir **"Actions"** > **"afficher mon ID"** : Un QR Code apparaît ;
- Sur le téléphone, aller en haut à droit, sur le **"+"**, ça ouvre le menu **"Ajout d'un appareil"** :
    - Cochez **"initiateur"** ;
    - Cliquez sur **"Mon ID"** et scanner le QR Code du PC ;
    - cliquez sur **"V"** (coche) en haut à droite.
- Sur le PC, fermez la fenêtre QR Code, un pop-up apparaît indiquant **"Nouvel appareil : XXX demande à se connecter"** -> cochez **"Ajouter un appareil"**.

A ce stade de chaque côté on doit voir l'autre appareil "connecté" dans la section **"Autres appareils"** (Windows) ou **"Appareils"** (Android). 


# 2. Mise en place de la sauvegarde automatique des photos

> **ATTENTION :** 
> Comme indiqué plus haut, il s'agit d'une procédure de synchronisation et pas d'archivage , tout fichier supprimé d'un côté disparaître de l'autre côté à la prochaine synchronisation.
>
> Si on veut sauvegarder ses photos ailleurs, dans un répertoire bien rangé, il y a 2 options :
>
> - **SI on veut faire de la place sur le téléphone :** il suffit de les **déplacer** dans un autre répertoire du PC : elles vont disparaître du répertoire synchronisé du PC, et donc aussi du téléphone ;
> - **SI on veut les conserver sur le téléphone :** il faut les **copier** dans un autre répertoire du PC : elles resteront disponibles dans le répertoire synchronisé du PC, et donc sur le téléphone.

Syncthing pour Android pré-configure le partage du répertoire photos principal :  
Sur le téléphone, vous devez voir dans l'onglet **"Partage"** une ligne pré-paramétrée sous le nom **"Camera"**, elle pointe vers le répertoire _DCIM/Camera_ de la mémoire interne du téléphone.

Nous allons proposer ce partage au PC :

- Cliquez dessus pour compléter la configuration :
    - **"Type de partage"** : Vous avez 3 choix :
        - **"Envoyer et recevoir"** : Comportement par défaut - si un fichier est supprimé sur l'ordi ou le tel il sera supprimé de l'autre côté ;
        - **"Envoyer seulement"** : Un fichier supprimé sur le téléphone disparaît du PC, mais un fichier supprimé sur le PC ne disparaît pas du téléphone ;
        - **"Recevoir seulement"** : Un fichier supprimé sur le téléphone ne disparaît pas du PC, mais un fichier supprimé du PC disparaît du téléphone.  
          > Personnellement j'opte systématiquement pour **"Envoyer et recevoir"** car les principes sont clairs :  
          > Ça marche comme DROPBOX ou GOOGLE DRIVE, chaque répertoire est une copie conforme de l'autre, ça évite de se faire des noeuds au cerveau pour savoir qu'est-ce qui est où.  
          > 
          > Et si on a besoin de sauvegarder/archiver proprement quelque chose, on les copie ou déplace ailleurs.  
    - Les appareils connectés sont listés avec une petite case à cocher à côté :  
Cochez le nom du PC (**"PC_DE_JOJO"** dans mon exemple)
    - Revenez en arrière avec la flèche en haut à gauche.

Sur le PC, un pop-up apparaît en haut, indiquant **"TEL_... vous invite au partage "Caméra", ajouter ce partage ?"** :

- Cliquez sur **"Ajouter"**
- Changez **"chemin racine du partage"** pour mettre la copie dans le répertoire qui vous arrange, exemple :  
  *Documents\fichiers_synchronises\TEL_DE_JOJO\Camera*

A présent que tout est en place, vous allez voir les fichiers du téléphone apparaître progressivement dans le répertoire du PC.

Les 2 applications ne resteront pas forcément actives en permanence.  
Pensez à ouvrir les 2 à intervalles réguliers de manière à les forcer à se mettre à jour.


# Et ensuite ? Synchroniser d'autres répertoires

Il suffit dans l'écran **"PARTAGES"** du téléphone, de cliquer sur **"+"** en haut à droite pour indiquer un autre répertoire du téléphone à partager avec l'extérieur.

Exemple :

- Choisissez un **"Nom local du partage"** (ex : *"Facebook"*) ;
- Cliquez sur **"Répertoire"** :  
  (La première fois que vous le faites, il faudra peut-être cliquer en haut à droite sur le bouton **"..."** puis **"afficher la mémoire interne"** pour voir la mémoire du téléphone)
    - Cliquez sur le bouton **"---"** en haut à gauche :
    Normalement il y a 2 choix possibles, la mémoire interne et la carte SD ;
    - Trouver le répertoire, accepter le partage, valider.

Côté PC, le 2e répertoire produira un pop-up comme le 1er.  
Il ne reste plus qu'à indiquer le répertoire local dans lequel le synchroniser (exemple : *Documents\fichiers_synchronises\TEL_DE_JOJO\Facebook*).

> Les répertoires de photo les plus courants :
> 
> - Photos du téléphone sauvegardées sur la carte SD : _Carte SD > DCIM_
> - Images Whatsapp/Facebook/etc. : _Mémoire interne > Pictures_
> - Fichiers téléchargés : _Mémoire interne > Download_

Exemple d'un téléphone très synchronisé :

![capture d'écran de Syncthing Android avec plein de partages activés](syncthing_android_plein_de_partages.jpg)



