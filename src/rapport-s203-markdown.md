::: s203
Malori ALVAREZ, Florine LEFEBVRE & Nathanaël COLLUMEAU

# Rapport SAE 2.03

[Sommaire :]{.underline}

> → [1^ère^ semaine](#1S)\
> Installation de l'OS, sudo, suppléments invités, distribution debian &
> automatisation\
> → [2^ème^ semaine](#2S)\
> Apprentissage du markdown, début des rapports sur StackEdit,
> réalisation de l'archive (html, pdf, source et readme)\
> → [3^ème^ semaine](#3S)\
> Setup du git et choix de l'interface graphique\
> → [4^ème^ semaine](#4S)\
> Setup de Gitea\

## 1^ère^ semaine {#1S}

### ![drawing](https://cdn-icons-png.flaticon.com/512/73/73575.png){width="35"}  [Création de la 1^ère^ VM]{.mark}  

Tout d'abord, nous avons commencé en créant une nouvelle VM dans
**Oracle VM VirtualBox** que nous avons appelée "*SAE203auto*", sans
aucun fichier iso, mais en sélectionnant le type (**Linux**) et la
version (**Debian 12 Bookworm** en version 64-bit). Par la suite, nous
avons continué la création de la VM en laissant les paramètres par
défaut. Cependant, il manquait le fichier iso, nous avons donc été dans
la configuration de la VM, dans l'onglet `Storage`, puis dans
"`Controller: IDE`" -\> "`Empty`" pour rajouter dans
"`IDE Primary Device 0`" le fichier "**S203-Debian12.viso**" afin que
les fichiers de configuration automatique se lancent convenablement.

![](https://lh7-us.googleusercontent.com/HwGyxgCVJ4FucrncIYnrRtDJVPEdrYU__IEii__UMg29YSwengtuclEehw575ZxLd2NAXWbh1VpI0xNDq_o1kRc3V7HD2PmTYJKxIz14KGTN7T8BkUGBZ_M6d1aJU1k5DbnO3vo9U3_ELN5BtngKaZY)\
Avant de lancer la VM, nous avons modifié les fichiers
"`vboxpostinstall.sh`" et "`preseed-fr.cfg`" pour paramétrer notre VM,
ainsi que l'uuid dans le fichier viso.

Dans la **preseed**, on a modifié la ligne 83 afin d'avoir
l'installation de mate :

`tasksel tasksel/first multiselect standard ssh-server, mate-desktop`\

**![](https://lh7-us.googleusercontent.com/NXvSVVd-LuTqc8ObAwmz6XIFKAmHAJZ_OK8OafUy8bNNJKmjDhpUu9oPERd58B_ClqZOfV2UAREMnBZijqGrVuKCphyTQjMiojFAJzsT8ltba8KWeEGLdtXVldk-NC7NrDMONRHsnqa2n-hrjT5Un5s)**\
Dans le **vboxpostinstall**, nous avons ajouté dans la partie "Run user
command" les commandes pour installer les fonctionnalités attendues :\
**![](https://lh7-us.googleusercontent.com/UNWV9n-fmQzs8ckQ6X1Jv6B-sqRiQcOuRqqGqFoyHddPlooD2WQCCO5Nn3KV80svOt0gnpqwU0Pc20U3ihF7d2KU3x-EEJvqRnc7PBaEbutOic7IBDlwmiBTQYdecS9YvDb325QQ2BqxfhV5BRyTX68)**\
`log_command_in_target usermod -a -G sudo user`

`log_command_in_target apt-get install sudo`

`log_command_in_target sudo apt-get install git`

`log_command_in_target sudo apt-get install sqlite3`

`log_command_in_target sudo apt-get install curl`

`log_command_in_target sudo apt-get install bash-completion`

`log_command_in_target sudo apt-get install neofetch`

Nous avons rencontré quelques soucis à l'installation à cause de ces
commandes, mais en commentant les 5 dernières lignes *( sudo
apt-get...)* l'ensemble refonctionnait de nouveau, mais sans les
installations. \
Nous avons plus tard réalisé que le problème venait du fait que la commande
`sudo` attend une entrée utilisateur, ce qui bloquait complètement l'exécution
du script. De plus, ce script s'exécute déjà en tant que root, alors il nous
a suffit de retirer `sudo` de toutes les commandes pour que le script
fonctionne.

### ![drawing](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Question_mark_alternate.svg/788px-Question_mark_alternate.svg.png){width="20"} [Questions]{.mark} : Configuration matérielle dans VirtualBox 

Que signifie "64-bit" dans "Debian 64-bit" ?

:   -   C'est la largeur des registres soit la façon dont le processeur
        gère les informations qu'il doit effectuer.

Quelle est la configuration réseau utilisée par défaut ?

:   -   La configuration réseau utilisée par défaut est la configuration
        en mode *NAT*.


Quel est le nom du fichier XML contenant la configuration de votre machine ?

:   -   Le nom du fichier XML contenant la configuration de votre
        machine est le fichier `debian.xml`

Comment vous modifieriez ce fichier de configuration pour mettre 2 processeurs à votre machine ?

:   -   Pour mettre 2 processeurs à votre machine, je modifierais le
        fichier de configuration en ajoutant la ligne suivante :
        `<vcpu placement='static'>2</vcpu>.`

### ![drawing](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Question_mark_alternate.svg/788px-Question_mark_alternate.svg.png){width="20"} [Questions]{.mark} : Installation OS de base 

Qu'est-ce qu'un fichier iso bootable ?

:   -   Un fichier iso bootable est un fichier qui s'exécute au
        démarrage de la VM et qui contient une instance d'un système
        d'exploitation. Il permet de créer la machine.

Qu'est-ce que MATE ? GNOME ?

:   -   *Mate* et *Gnome* sont des environnements graphiques de debian

Qu'est-ce qu'un serveur web ?

:   -   Un serveur web est un logiciel qui permet de stocker et de
        diffuser des pages web.

Qu'est-ce qu'un serveur ssh ?

:   -   SSH est un protocole de communication sécurisé qui permet
        d'accéder à distance à un serveur.


Qu'est-ce qu'un serveur mandataire ?

:   -   Un serveur mandataire est un serveur qui permet de filtrer les
        requêtes des clients.

### ![drawing](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Question_mark_alternate.svg/788px-Question_mark_alternate.svg.png){width="20"} [Questions]{.mark} : sudo 

Comment peux-ton savoir à quels groupes appartient l'utilisateur user ?

:   -   On utilise la commande : \$ groups user

### ![drawing](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Question_mark_alternate.svg/788px-Question_mark_alternate.svg.png){width="20"} [Questions]{.mark} : Suppléments invités 

Quelle est la version du noyau Linux utilisé par votre VM ? N'oubliez pas, comme pour toutes les questions, de justifier votre réponse.

:   -   On l'obtient avec la commande uname -r, uname donne des
        informations sur le système qu'on utilise. Ici il s'agit de
        amd64.

À quoi servent les suppléments invités ? Donner 2 principales raisons de les installer.

:   -   Les suppléments invités servent à :\
        -\> améliorer les performances de la machine virtuelle\
        -\> faciliter le partage de fichiers entre la machine virtuelle
        et l'hôte.

À quoi sert la commande mount (dans notre cas de figure et dans le cas général) ?

:   -   Elle permet de monter une partition sur le disque dans le cas
        général.

### ![drawing](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Question_mark_alternate.svg/788px-Question_mark_alternate.svg.png){width="20"} [Questions]{.mark} : Quelques questions 

Il existe 3 durées de prise en charge (support) de ces versions : la durée minimale la durée en support long terme (LTS) et la durée en support long terme étendue (ELTS). Quelles sont les durées de ces prises en charge ?

:   -   La durée minimale, de support long terme (LTS) et celle en
        support long terme étendue (ELTS) sont de 5 ans

Pendant combien de temps les mises à jour de sécurité seront-elles fournies ?

:   -   Les mises à jour de sécurité seront fournies pendant 5 ans.

Combien de version au minimum sont activement maintenues par Debian ? Donnez leur nom générique (= les types de distribution).

:   -   Il y a 3 versions au minimum activement maintenues par Debian :
        *stable, testing* et *unstable*.

Chaque distribution majeur possède un nom de code différent. Par exemple la version majeur actuelle (Debian 12) se nomme bookworm. D'où viennent les noms de code données aux distributions ?

:   -   Les noms de code donnés aux distributions viennent de
        personnages de Toy Story. La décision d'utiliser des noms
        provenant de Toy Story a été prise par Bruce Perens qui était, à
        l'époque, responsable du projet Debian et travaillait chez
        Pixar, la société qui a produit les films.

L'un des atouts de Debian fut le nombre d'architecture (≈ processeurs) officiellement prises en charge. Combien et lesquelles sont prises en charge par la version Bullseye ?

:   -   Il y a 10 architectures officiellement prises en charge par la
        version Bullseye : amd64 (la notre) arm64 armel armhf i386 mips
        mips64el mipsel ppc64el s390x.

Quel était le numéro de version de cette distribution ?

:   -   Le numéro de version de cette distribution était 11.

Quel a été le premier nom de code utilisé ?

:   -   Le premier nom de code utilisé était Buzz.

## 2^ème^ semaine  {#2S}

Nous nous sommes initié au **Markdown** via les sites qui nous ont été
fournis pour s'exercer
([markdowntutorial.com](http://markdowntutorial.com) &
[commonmark.org](http://commonmark.org)) mais certain.*es* d'entre nous
savaient déjà l'utiliser par le passé.

Pour le choix de l'éditeur, nous préférons sur navigateur afin de
s'épargner de devoir installer un logiciel sur chaque machines où nous
travaillons. Nous avons donc choisi **StackEdit** car après avoir essayé
d'autres options telles que **NextCloud** ou **github/lab**, celui-ci
nous paraissait optimal bien qu'on ne puisse pas rédiger le rapport en
temps réel ensemble comme sur **NextCloud**.

Nous avons exporté notre rapport sous le format .md et nous l'avons
compilé en HTML avec la commande:

`pandoc.exe --standalone --template template-compilation-s203.html rapport-s203-markdown.md -o rapport-s203.html`

L'utilisation de l'option -\-template permet, par exemple, de donner un titre à la page et d'utiliser une feuille de style CSS externe.

En revanche, nous nous sommes heurté.e.s à des obstacles pour la compilation au format PDF avec pdflatex.

## 3^ème^ semaine  {#3S}

### ![drawing](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Question_mark_alternate.svg/788px-Question_mark_alternate.svg.png){width="20"}  [Questions]{.mark} : Interfaces graphiques pour *git* 

Qu'est-ce que le logiciel gitk ? Comment se lance-t-il ?

:   -   gitk est un navigateur de dépôt graphique qui permet de voir nos
        branches et nos commits, pulls, push, etc. sur git. Il se lance
        depuis le terminal depuis un dépôt.

Qu'est-ce que le logiciel git-gui ? Comment se lance-t-il ?

:   -   Interface graphique de Git basée sur Tcl/Tk. Git gui permet aux
        utilisateurs d'apporter des modifications à leur dépôt en
        faisant de nouveaux commits, en modifiant les commits existants,
        en créant des branches, en effectuant des fusions locales, et en
        récupérant/poussant vers des dépôts distants.

:   -   Contrairement à gitk, git gui se concentre sur la génération de
        commit et l'annotation de fichiers uniques et n'affiche pas
        l'historique du projet. Il fournit cependant des actions de menu
        pour démarrer une session gitk à partir de git gui.

Pourquoi avez-vous choisi ce logiciel ?

:   -   Nous avons choisi [GitKraken](https://www.gitkraken.com/) car
        son interface semble plus logique, intuitive et esthéthiquement
        plaisante.

Comment l'avez vous installé ?

:   -   Sur leur site avec la version demo Linux-Debian. L'essai gratuit
        couvre entièrement notre temps d'utilisation du logiciel pour la
        SAE.

Comparer-le aux outils inclus avec git (et installé précédemment) ainsi qu'avec ce qui serait fait 2/3 \| 1.1. Configuration globale de git en ligne de commande pure : fonctionnalités avantages, inconvénients...

:   -   L'interface est logique (schéma clairs, différentes couleurs,
        interface simpliste.) et nous permet de visualiser simplement
        l'arborescence, nous faisant gagner énormément de temps. Des
        outils nous permettent de visualiser les modifications
        rapidement en passant le curseur dessus, on dispose également de
        stats par utilisateurs et d'actions effectuables simultanément
        sur plusieurs repository sélectionnés (fetch, pull, clone ect).
		
## 4^ème^ semaine {#4S}

### ![drawing](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Question_mark_alternate.svg/788px-Question_mark_alternate.svg.png){width="20"}  [Questions]{.mark} : Qu'est ce que *Gitea* ? 

Gitea est une forge logicielle libre en Go sous licence MIT, 
pour l'hébergement de développement logiciel, basé sur le logiciel 
de gestion de versions Git pour la gestion du code source, 
comportant un système de suivi des bugs, un wiki, ainsi que 
des outils pour la relecture de code. (Source Wikipédia)

### ![drawing](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Question_mark_alternate.svg/788px-Question_mark_alternate.svg.png){width="20"}  [Questions]{.mark} : À quels logiciels peut-on le comparer ? 


### ![drawing](https://cdn-icons-png.flaticon.com/512/73/73575.png){width="35"}  [Installation de Gitea]{.mark} 

- On commence par ajouter la règle de redirection de port

![](https://cdn.discordapp.com/attachments/1072839621567856690/1218651902045327391/Sans_titre.png?ex=66087106&is=65f5fc06&hm=9b8d57958b4eb5c5e24f237b782bed7447981b2bdedd1f91e9192ae1eb2f80bb&)

- On utilise wget avec le lien de téléchargement associé à notre version de linux, en l’occurrence : linux-amd64 (cf. Semaine 06 - Question 4)

`> wget -O gitea https://dl.gitea.com/gitea/1.21.7/gitea-1.21.7-linux-amd64`

`> chmod +x gitea`

- Télécharger le fichier asc : <https://dl.gitea.com/gitea/1.21.7/gitea-1.21.7-linux-amd64.asc>

- Ajout des clés binaires

`> gpg --keyserver keys.openpgp.org --recv 7C9E68152594688862D62AF62D9AE806EC1592E2`

`> gpg --verify gitea-1.21.7-linux-amd64.asc gitea`

- Création d’un utilisateur “git”

`>adduser \ --system \ --shell /bin/bash \ --gecos 'Git Version Control' \ --group \ --disabled-password \ --home /home/git \ git`

- Création des dossiers nécessaires

`> mkdir -p /var/lib/gitea/{custom,data,log}`

`chown -R git:git /var/lib/gitea/`

`chmod -R 750 /var/lib/gitea/`

`mkdir /etc/gitea`

`chown root:git /etc/gitea`

`chmod 770 /etc/gitea`

:::