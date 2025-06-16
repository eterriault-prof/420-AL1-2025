# Le système de fichiers hiérarchique (FHS)

***Note: Le contenu suivant a été tiré du cours 420-2C2 monté par Miguel Grandmont-Champagne.***

Linux est un système d'exploitation libre. Cela signifie que n'importe quel utilisateur est libre de créer et de publier sa propre distribution du système d'exploitation. Imaginez un instant si chaque distribution suivait ses propres règles de nommage des fichiers et des répertoires. Nous serions confrontés comme utilisateur  à plusieurs distributions différentes et incompatibles. Il serait très difficile pour un utilisateur de se familiariser avec de nouvelles distributions de Linux car elles seraient toutes différentes les unes des autres.
Pour éviter une telle situation, certaines normes ont été adoptées par la communauté Linux pour assurer l'inter-compatibilité entre les distributions. L'une de ces normes est **la hiérarchie standardisée du système de fichiers** ou FHS.

![capture_du_2020-12-05_10-43-32.png](/static/images/AL1/rechercher_infos_fs/capture_du_2020-12-05_10-43-32.png)


La norme FHS définit la structure des répertoires et leur utilité dans un système Linux. Cela permet à un utilisateur de repérer rapidement l'emplacement d'un fichier spécifique dans le système de fichiers. Par exemple, le répertoire `/bin` sert à stocker les commandes essentielles du système, `/etc` regroupe les fichiers de configuration, `/var` contient des données fréquemment modifiées, `/home` les répertoires personnels des utilisateurs, etc ... Ces répertoires sont présents sur toutes les distributions de Linux et ils conservent tous la même utilité.

## Répertoires du FHS
Voici la liste des principaux répertoires et leur contenu sous Linux.
|Répertoire| Utilité|
|-----|---|
|**/**|	La racine du système de fichiers.
|**/bin**|	Contient les programmes binaires essentiels aux utilisateurs.
|**/boot**|	Contient le noyau et les fichiers de configuration du chargeur de démarrage.
|**/dev**|	Présente les fichiers attachés à des périphériques (ex: disques, clés USB,...).
|**/etc**|	Contient les fichiers de configuration du système (analogue à la base de registre sous Windows)
|**/home**|	Emplacement par défaut des répertoires personnels des utilisateurs.
|**/mnt**|	Point de montage temporaire 
|/media|	Point de montage pour un média éjectable (comme une clé USB ou un disque externe)
|/opt	| Emplacement pour les logiciels développés par des tierses parties.
|/**root** |	Répertoire personnel de l'utilisateur root
|**/sbin**|	Contient les binaires essentiels d'administration du système
|**/tmp** |	Emplacement pour les fichiers temporaires (vidé à chaque redémarrage)
|/usr |	Contient généralement les fichiers propre à la distribution utilisée.
|/usr/bin |	Contient la plupart des commandes disponibles suite à l'installation d'une application
|/usr/sbin |	Contient les binaires non-essentiels d'administration du système
|/usr/share |	Fichiers communs et indépendants de la plateforme.
|/usr/share/doc |	Documentation pour les paquets logiciels.
|/usr/share/man |	Emplacement des pages de manuel.
|**/var**|	Contient des fichiers qui sont modifiés automatiquement par le système

Le contenu des répertoires identifiés en **gras** est matière du cours. 


Pour des explications exhaustives, vous pouvez consulter ici la norme FHS publiée sur le site de la Linux Foundation: https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html

# Rechercher des fichiers dans l'arborescence

La plupart des systèmes d'exploitation permettent aux utilisateurs de rechercher des fichiers dans le système de fichiers. Une interface graphique (GUI) fournit généralement un outil de recherche qui permet de trouver des fichiers et des programmes. 

Un système d'exploitation administrable par la ligne de commandes (CLI) propose également des utilitaires pour rechercher des fichiers et des programmes dans le système de fichiers. Les commandes `locate` et `find` sont les deux principales commandes utilisées pour lancer une recherche dans le système de fichiers de Linux. 

Bien que les deux commandes exécutent des tâches similaires, chacune le fait en utilisant une technique différente, avec ses propres avantages et inconvénients.
## Commande locate
L'une des deux commandes principales pour rechercher des fichiers sous Linux est `locate`. La commande `locate` est reconnue pour être très rapide car elle utilise la recherche avec une base de données. La base de données contient tous les noms de fichiers accompagnés de leur emplacement dans le système de fichiers.

Pour que la commande `locate` produise un résultat fiable, il est impératif que sa base de données soit à jour au moment de lancer la recherche.

Dans la forme la plus simple, une recherche avec la commande `locate` se fait comme suit.
```
locate NOM_DE_FICHIER
```
Par exemple, pour rechercher un fichier contenant le terme **host**, on lance la commande `locate` comme suit.
```
etudiant@al1-vm:~$ locate hosts
/etc/hosts
/etc/hosts.allow
/etc/hosts.deny
/lib/x86_64-linux-gnu/security/pam_rhosts.so
/usr/share/man/man5/hosts.allow.5.gz
```

Considérez les points suivants concernant les résultats retournés par `locate`
- la commande `locate` retourne seulement les fichiers auxquels l'utilisateur courant a accès.
- la commande `locate` retourne tous les fichiers contenant la chaîne recherchée. Par exemple, dans la commande qui précède, la rechercher avec le terme **host** a retourné les fichiers `hosts`, `hosts.allow`, `pam_rhosts.so`, etc..
- comme toujours, la commande `locate` est sensible à la casse. 

Tel que mentionné plus haut, la commande `locate` dépend d'une base de données. Cette base de données doit être à jour au moment de lancer la commande pour assurer l'exactitude de la recherche. Pour mettre à jour cette base de données, il faut lancer la commande `updatedb`.
En temps normal, la commande updatedb est lancée automatiquement à chaque jour par une tâche planifiée CRON. Un utilisateur peut également souhaiter mettre à jour cette base de données manuellement.
> Il faut disposer des privilèges du super-utilisateur pour lancer la commande `updatedb`
```
etudiant@al1-vm:~$ sudo updatedb
[sudo] password for etudiant: 
/usr/bin/find: '/dev/.lxd-mounts': Permission denied               <--- Ignorez cette erreur
etudiant@al1-vm:~$ 
```

L'exemple suivant illustre l'importance de disposer d'une base de données à jour au moment de lancer la commande `locate`.
 1. On créer un nouveau fichier nommé `fichier-perdu`
 	```
	  etudiant@al1-vm:~$ touch fichier-perdu
  	```
 1. On lance la recherche pour un fichier nommé **fichier-perdu**. On remarque qu'aucun résultat n'est retourné par la commande.
 	```
   	etudiant@al1-vm:~$ locate fichier-perdu
    etudiant@al1-vm:~$
  	```
 1. On met à jour la base de données avec `updatedb` et on relance la recherche.
 	```
   	etudiant@al1-vm:~$ sudo updatedb
    [sudo] password for etudiant: 
    /usr/bin/find: '/dev/.lxd-mounts': Permission denied
    
    etudiant@al1-vm:~$ locate fichier-perdu
    /home/etudiant/fichier-perdu
  	```
 
## Commande find
La commande `find` est une seconde commande qui permet de lancer une recherche pour une information donnée dans le système de fichiers. Contrairement à la commande `locate`, `find` n'utilise pas de base de données, elle lance une recherche en parcourant le système de fichiers pour trouver ce que vous cherchez. 

Bien qu'elle soit plus lente que `locate`, `find` ne risque pas de retourner un résultat de recherche qui n'est plus à jour.

La commande `find` prend un répertoire comme premier argument. Ce sera le point de départ de la recherche. La commande `find` recherchera dans ce répertoire et tous ses sous-répertoires. Si aucun répertoire n'est spécifié, la commande `find` lancera la recherche dans le répertoire courant.
```
find [point de départ...] [options]
```
Notez que dans l'exemple suivant, le caractère point `.` fait référence au répertoire courant.  Le répertoire courant peut aussi être désigné en utilisant la notation `./`. 

Nous lancerons la recherche dans le répertoire personnel de l'utilisateur etudiant listé comme suit.

```
etudiant@al1-vm:~$ tree
.
├── code
│   ├── bash
│   └── python
├── documents
│   ├── bilan 2020
│   │   ├── rapport-automne-2020.txt
│   │   └── rapport-hiver-2020.txt
│   ├── dictionnaire
│   │   └── français.txt
│   └── ressources humaines
│       └── employés.csv
├── fichier-perdu
└── téléchargements
```

La commande `find` utilise l'option `-name` pour rechercher des fichiers par nom, dans ce cas, le répertoire `dictionnaire` et des fichiers débutant par `rapp`.

```
(1) etudiant@al1-vm:~$ find . -name dictionnair
    etudiant@al1-vm:~$

(2) etudiant@al1-vm:~$ find . -name dictionnaire
    ./documents/dictionnaire

(3) etudiant@al1-vm:~$ find . -name 'dictionn*'
	  ./documents/dictionnaire

(4) etudiant@al1-vm:~$ find . -name 'rapp*'
    ./documents/bilan 2020/rapport-automne-2020.txt
    ./documents/bilan 2020/rapport-hiver-2020.txt
```
- La première recherche n'a donné aucun résultat car la chaîne recherchée doit correspondre au nom exact du fichier, pas seulement à une partie du nom. 
- La troisième commande démontre que l'englobement peut être utilisé avec la commande `find`
- La quatrième commande affiche plusieurs correspondances pour le terme recherché.

Si la recherche du répertoire `dictionnaire` était effectuée à partir du répertoire racine `/`, de nombreuses erreurs se produisent.
```
etudiant@al1-vm:~$ find / -name dictionnaire
find: ‘/proc/865/map_files’: Permission denied
find: ‘/proc/865/fdinfo’: Permission denied
find: ‘/proc/865/ns’: Permission denied
find: ‘/proc/870/task/870/fd’: Permission denied
find: ‘/proc/870/task/870/fdinfo’: Permission denied
find: ‘/proc/870/task/870/ns’: Permission denied
.....
```
Ces erreurs peuvent être ignorées pour le moment, mais sachez simplement que des erreurs comme celles-ci sont courantes lorsqu'un utilisateur standard comme **etudiant** tente de rechercher dans des répertoires réservés au super-utilisateur root.

La commande `find` offre également de nombreuses options de recherche de fichiers, contrairement à la commande `locate` qui recherche uniquement les fichiers en fonction du nom. 

Le tableau suivant illustre certains des critères les plus couramment utilisés avec `find`.
|Exemple |	Signification|
|--|--|
|`-iname FICHIER-QUELCONQUE`|	Recherche par nom **sans** sensibilité à la casse
|`-mtime -3	`| Fichiers modifiés il y a moins de trois jours
|`-mmin -10	`| Fichiers modifiés il y a moins de dix minutes
|`-size +1M`	| Fichiers plus gros que 1 Mo
|`-user miguel`	| Fichiers dont miguel est le propriétaire
|`-empty	`| Fichiers vides
|`-type d`	| Fichiers qui sont des répertoires
|`-maxdepth 1 `|	Rechercher d'une profondeur maximale d'un répertoire

Par exemple, pour rechercher les fichiers vides portant l'extension `.txt` dans le répertoire personnel de l'utilisateur etudiant, on lancera la commande suivante.

```
etudiant@al1-vm:~$ find . -empty -name *.txt
./documents/bilan 2020/rapport-automne-2020.txt
./documents/bilan 2020/rapport-hiver-2020.txt
```
# Extraire des données d'un fichier
Un principe de base qui a commencé avec UNIX, et qui se perpétue sous Linux, est que les commandes doivent lire du texte en entrée et produire du texte en sortie. Pour rester fidèle à cette philosophie, un grand nombre de commandes ont été conçues dans le but principal de traiter des fichiers texte. Comme vous le verrez, beaucoup de ces commandes agissent comme des filtres, car elles modifient en quelque sorte le texte qu'elles lisent avant de produire leur sortie.
## cat
La première commande que nous couvrons a déjà été utilisée à quelques reprises dans le cours. Il s'agit de la commande `cat`. Lorsqu'elle est lancée avec un seul fichier en argument, la commande `cat` affiche simplement le contenu de ce fichier à l'écran.

```
cat [OPTION]... [FICHIER]...
```
```
etudiant@al1-vm:~$ cd documents/dictionnaire/

etudiant@al1-vm:~/documents/dictionnaire$ cat phonétique-p1
A    alpha
B    bravo
C    charlie
D    delta
E    echo
F    foxtrot
G    golf
H    hotel
I    india
J    juliette
K    kilo
L    lima
M    mike
```

La commande `cat` tire son nom du mot con**cat**éner, qui signifie fusionner ou mettre bout-à-bout. Cette commande accepte plusieurs fichiers en argument, puis elle affiche la concaténation de ces fichiers.
```
etudiant@al1-vm:~/documents/dictionnaire$ cat phonétique-p1 phonétique-p2
A    alpha
B    bravo
C    charlie
D    delta
E    echo
F    foxtrot
G    golf
H    hotel
I    india
J    juliette
K    kilo
L    lima
M    mike
N    november
O    oscar
P    papa
Q    quebec
R    romeo
S    sierra
T    tango
U    uniform
V    victor
W    whisky
X    x-ray
Y    yankee
Z    zulu
```

Bien que la commande `cat` soit souvent utilisée pour afficher rapidement le contenu de petits fichiers de quelques lignes, son utilisation pour afficher le contenu de fichiers plus longs s'avère peu efficace. En effet, si vous lancez la commande `cat` pour afficher le contenu d'un fichier trop long, la commande fera défiler son contenu très rapidement et hors de la zone d'affichage à l'écran. 

Pour tester cela, lancez la commande suivante. :D

```
etudiant@al1-vm:~/documents/dictionnaire$ cat français.txt
```

## less
Pour afficher un fichier une page à la fois, on utilisera la commande `less`.
```
less FICHIER
```
```
etudiant@al1-vm:~/documents/dictionnaire$ less français.txt
```
La commande `less` permet aux utilisateurs de naviguer dans le document à l'aide de certaines touches du clavier. Lorsque vous utilisez `less` pour paginer l'affichage à l'écran, utilisez la touche <kbd>espace</kbd> pour avancer d'une page. Pour descendre d'une ligne, appuyez sur <kbd>d</kbd>, pour remonter d'une ligne sur <kbd>u</kbd> et pour quitter sur <kbd>q</kbd>. 

> Lorsque vous ouvrez une page de manuel avec la commande `man`, c'est en fait la commande `less` qui produit l'affichage. Les même raccourcis clavier sont disponibles et la touche <kbd>h</kbd> peut être utilisée pour obtenir de l'aide.

## head
On utilise la commande head pour afficher les premières lignes d'un fichier.
```
head [OPTIONS] FICHIER
```
Par défaut, la commande head affiche les dix (10) premières lignes du fichier passé en argument.
```
etudiant@al1-vm:~/documents/dictionnaire$ head phonétique-p1
A    alpha
B    bravo
C    charlie
D    delta
E    echo
F    foxtrot
G    golf
H    hotel
I    india
J    juliette
```
La commande head permet également de spécifier le nombre de premières lignes à afficher. Par exemple ici, on souhaite afficher les trois (3) première lignes du fichier phonétique-p1.
```
etudiant@al1-vm:~/documents/dictionnaire$ head -3 phonétique-p1
A    alpha
B    bravo
C    charlie
```
## tail
La commande `tail` agit à l'inverse de la commande `head`. `tail` affiche les dernières lignes d'un fichier.

**Cette commande est particulièrement utile pour consulter les journaux d'événements sous Linux (logs).** En effet, souvent les journaux sont des fichiers très longs où plusieurs événements sont ajoutés les uns après les autres à la fin du fichier. Comme les événements récents se trouvent toujours en fin de fichier, on utilisera la commande `tail` pour afficher les dernières entrées dans les journaux.

À l'instar de la commande `head`, la commande `tail` affiche par défaut les dix (10) dernières lignes du fichier passé en argument. On peut utiliser une option pour préciser le nombre de lignes à afficher. Dans l'exemple suivant, on souhaite afficher les trois dernières lignes du fichier `phonétique-p1`.
```
etudiant@al1-vm:~$ tail -3 documents/dictionnaires/phonétique-p1
K    kilo
L    lima
M    mike
```

Par exemple, le fichier `/var/log/auth.log` sous une distribution dérivée de Debian contient le journal des authentifications sur le système. Chaque fois qu'une connexion a lieu, le système ajoute une ligne à la fin de ce fichier. Pour afficher les dernières connexions, on utilise la commande `tail /var/log/auth.log`.

```
etudiant@al1-vm:~$ tail /var/log/auth.log
tail: cannot open '/var/log/auth.log' for reading: Permission denied
```

>Puisque ce fichier est sensible, il faut élever nos privilèges en tant que super-utilisateur pour lire son contenu. 

```
etudiant@al1-vm:~$ sudo tail /var/log/auth.log 
[sudo] password for etudiant: 
Mar  2 09:17:01 al1-vm CRON[3497]: pam_unix(cron:session): session opened for user root by (uid=0)
Mar  2 09:17:01 al1-vm CRON[3497]: pam_unix(cron:session): session closed for user root
Mar  2 09:39:01 al1-vm CRON[3503]: pam_unix(cron:session): session opened for user root by (uid=0)
Mar  2 09:39:01 al1-vm CRON[3503]: pam_unix(cron:session): session closed for user root
Mar  2 09:58:55 al1-vm sshd[3548]: Accepted publickey for etudiant from 10.0.9.1 port 43274 ssh2: RSA SHA256:IR9i+Jr5OE1X60ZU1aZvaAcoFDcbSKWk6APnhmsU7zo
Mar  2 09:58:55 al1-vm sshd[3548]: pam_unix(sshd:session): session opened for user etudiant by (uid=0)
Mar  2 09:58:55 al1-vm systemd-logind[157]: New session 4571 of user etudiant.
Mar  2 09:58:55 al1-vm systemd: pam_unix(systemd-user:session): session opened for user etudiant by (uid=0)
Mar  2 09:59:11 al1-vm sudo: etudiant : TTY=pts/0 ; PWD=/home/etudiant ; USER=root ; COMMAND=/usr/bin/tail /var/log/auth.log
Mar  2 09:59:11 al1-vm sudo: pam_unix(sudo:session): session opened for user root by etudiant(uid=0)
```

## cut
La commande cut permet d'extraire un champ bien précis dans un fichier. 

Intéressons-nous un instant au fichier `/etc/passwd` qui contient la liste des utilisateurs d'un système Linux. Le contenu de ce fichier sera couvert en détails plus tard dans le cours. Observons aujourd'hui sa structure en affichant sa première ligne.
```
etudiant@al1-vm:~$ head -1 /etc/passwd
root:x:0:0:root:/root:/bin/bash
```
On constate que chaque ligne du fichier `/etc/passwd` est constituée de 7 champs délimités par le caractère `:`.

![capture_du_2021-03-02_10-39-26.png](/static/images/AL1/rechercher_infos_fs/capture_du_2021-03-02_10-39-26.png)

Le premier champ est le nom d'utilisateur. 

La commande `cut` permet d'extraire un champ à l'intérieur d'un fichier. Sachant que les champs du fichier `/etc/passwd` sont séparés par le délimiteur `:`, on pourra extraire le premier champ à l'aide de la commande suivante:
```
etudiant@al1-vm:~$ cut -d ':' -f 1 /etc/passwd
```
- L'option `-d ':'` permet de spécifier le délimiteur (entre guillemets, car il s'agit d'un caractère spécial). Par défaut, l'espace et la tabulation sont considérés comme le délimiteur. Or, certains fichiers emploient la virgule `,` ou deux points `:` pour délimiter les champs comme dans l'exemple précédent.
- L'option `-f 1` indique que l'on souhaite extraire le premier champ, soit le nom d'utilisateur dans cet exemple.

```
etudiant@al1-vm:~$ cut -d ':' -f 1 /etc/passwd
root
daemon
bin
sys
sync
games
man
lp
mail
news
uucp
proxy
www-data
backup
list
irc
gnats
nobody
systemd-network
systemd-resolve
syslog
messagebus
_apt
ubuntu
sshd
etudiant
```
La commande `cut` peut afficher plusieurs champs séparés par des virgules. Par exemple, le septième champ du fichier `/etc/passwd` représente le shell de connexion de l'utilisateur. Pour afficher chaque utilisateur (champ 1) accompagné de son shell de connexion (champ 7), on peut lancer la commande suivante. 
```
etudiant@al1-vm:~$ cut -d ':' -f 1,7 /etc/passwd

```
```
etudiant@al1-vm:~$ cut -d ':' -f 1,7 /etc/passwd
root:/bin/bash
daemon:/usr/sbin/nologin
bin:/usr/sbin/nologin
sys:/usr/sbin/nologin
sync:/bin/sync
games:/usr/sbin/nologin
man:/usr/sbin/nologin
lp:/usr/sbin/nologin
mail:/usr/sbin/nologin
news:/usr/sbin/nologin
uucp:/usr/sbin/nologin
proxy:/usr/sbin/nologin
www-data:/usr/sbin/nologin
backup:/usr/sbin/nologin
list:/usr/sbin/nologin
irc:/usr/sbin/nologin
gnats:/usr/sbin/nologin
nobody:/usr/sbin/nologin
systemd-network:/usr/sbin/nologin
systemd-resolve:/usr/sbin/nologin
syslog:/usr/sbin/nologin
messagebus:/usr/sbin/nologin
_apt:/usr/sbin/nologin
ubuntu:/bin/bash
sshd:/usr/sbin/nologin
etudiant:/bin/bash
```

## wc
La commande `wc` permet d'analyser le contenu d'un fichier. `wc` est l'abréviation de "word count". Elle retourne le nombre de lignes, de mots et d'octets avec le format suivant:

<pre> lignes mots octets nom_du_fichier</pre>

Prenons pour exemple le fichier `~/documents/dictionnairesphonétique-p1`.
```
etudiant@al1-vm:~/documents/dictionnaires$ cat phonétique-p1
A	alpha
B	bravo
C	charlie
D	delta
E	echo
F	foxtrot
G	golf
H	hotel
I	india
J	juliette
K	kilo
L	lima
M	mike
```

Utilisons maintenant la commande `wc` pour analyser le contenu de ce fichier.
```
etudiant@al1-vm:~/documents/dictionnaires$ wc phonétique-p1
 13  26 106 phonétique-p1
```
Le fichier `phonétique-p1` contient 13 lignes, 26 mots et 106 octets.

## grep

La commande `grep` est très utilisée dans le shell Linux. Elle permet de rechercher une chaîne de caractères dans un fichier fourni en entrée (en argument). Certains disent que `grep` est un filtre car elle laisse passer seulement les lignes d'un fichier qui contiennent une chaîne de caractères donnée.

Pour expérimenter la commande grep, nous allons utiliser le fichier `~/documents/ressources humaines/employés.csv` de notre VPS Guacamole. Ce fichier est une base de données fictive de 1000 employés structurée comme suit: 
<br>
<pre>Nom, Prénom, Téléphone, Courriel, Adresse IP, Taux horaire</pre>

Nous allons utiliser `grep` pour  rechercher toutes les lignes qui comportent la chaîne `Ana` dans ce fichier.
> Le résultat de l'exécution des commandes sera exceptionnellement inséré en HTML pour montrer la coloration syntaxique.
```
etudiant@al1-vm:~/documents/ressources humaines$ grep Ana employés.csv 
```
<br>
<pre><font color="#8AE234"><b>etudiant@al1-vm</b></font>:<font color="#729FCF"><b>~/documents/ressources humaines</b></font>$ grep Ana employés.csv 
<font color="#EF2929"><b>Ana</b></font>stasia,Gilkison,(386) 6682517,agilkison5p@nasa.gov,217.215.105.29,$50.80
<font color="#EF2929"><b>Ana</b></font>tol,Scamadine,(264) 9002056,ascamadineml@timesonline.co.uk,151.119.13.58,$37.22
<font color="#EF2929"><b>Ana</b></font>llise,Cokely,(509) 7288398,acokelynq@accuweather.com,147.39.9.215,$47.45
</pre>

Toutes les lignes comportant la chaîne `Ana` sont affichées.

> Comme toute chose sous Linux, la commande `grep` est sensible à la casse. Rechercher la chaîne `Ana` est différent de `ana`. 

Pour ignorer la casse, donc ne pas se soucier des majuscules ou des minuscules, on utilise `grep` avec l'option `-i` (ou `--ignore-case`). 
```
etudiant@al1-vm:~/documents/ressources humaines$ grep -i Ana employés.csv 
```
<pre><font color="#8AE234"><b>etudiant@al1-vm</b></font>:<font color="#729FCF"><b>~/documents/ressources humaines</b></font>$ grep -i Ana employés.csv 
Georgeanne,D<font color="#EF2929"><b>ana</b></font>her,(917) 7151933,gd<font color="#EF2929"><b>ana</b></font>heru@prweb.com,8.196.89.199,$32.56
Arl<font color="#EF2929"><b>ana</b></font>,Cunnington,(589) 6078104,acunnington1q@home.pl,168.134.17.240,$26.81
Germ<font color="#EF2929"><b>ana</b></font>,Hark,(666) 2177302,ghark1z@t.co,189.159.194.132,$32.19
<font color="#EF2929"><b>Ana</b></font>stasia,Gilkison,(386) 6682517,agilkison5p@nasa.gov,217.215.105.29,$50.80
Eddie,Langfat,(970) 6602338,elangfat8b@list-m<font color="#EF2929"><b>ana</b></font>ge.com,2.32.190.30,$29.40
Silv<font color="#EF2929"><b>ana</b></font>,Deguara,(652) 2654709,sdeguara8n@dell.com,23.76.245.141,$43.67
Bry<font color="#EF2929"><b>ana</b></font>,Srawley,,bsrawleyck@cbslocal.com,174.208.252.40,$43.69
Glori<font color="#EF2929"><b>ana</b></font>,Di Nisco,(647) 8376121,gdiniscof7@weibo.com,84.59.17.196,$30.04
Davon,Eltune,(925) 4509732,deltunehd@c<font color="#EF2929"><b>ana</b></font>lblog.com,140.14.173.144,$42.87
Flory,Hans<font color="#EF2929"><b>ana</b></font>,(108) 3966792,fhans<font color="#EF2929"><b>ana</b></font>l8@google.ru,180.43.203.36,$16.60
Edita,Kav<font color="#EF2929"><b>ana</b></font>gh,,ekav<font color="#EF2929"><b>ana</b></font>ghlx@linkedin.com,113.195.90.156,$31.85
<font color="#EF2929"><b>Ana</b></font>tol,Scamadine,(264) 9002056,ascamadineml@timesonline.co.uk,151.119.13.58,$37.22
Wynnie,Dawkins,(480) 5331879,wdawkinsn4@c<font color="#EF2929"><b>ana</b></font>lblog.com,167.145.82.151,$45.50
<font color="#EF2929"><b>Ana</b></font>llise,Cokely,(509) 7288398,acokelynq@accuweather.com,147.39.9.215,$47.45
Ramsay,Tuckey,(163) 6284342,rtuckeypj@c<font color="#EF2929"><b>ana</b></font>lblog.com,120.17.117.246,$31.30
Kathryn,Dudeney,(859) 2263158,kdudeneypp@c<font color="#EF2929"><b>ana</b></font>lblog.com,51.140.64.207,$49.73
Sh<font color="#EF2929"><b>ana</b></font>n,McCullouch,(720) 8330174,smccullouchq2@hud.gov,13.211.84.72,$19.05
Ros<font color="#EF2929"><b>ana</b></font>,Ruttgers,(294) 4717061,rruttgersr4@yellowbook.com,59.8.223.201,$38.13
</pre>

Cette fois, toutes les lignes qui possèdent les lettres `ana`, peu importe la casse, sont affichées. 

Notez que la chaîne recherchée peut se trouver n'importe où sur la ligne. Nous verrons plus tard avec les expressions régulières que nous pourront être beaucoup plus précis dans la recherche avec `grep`.


