# Objectifs

***Note: Le contenu suivant a été tiré du cours 420-2C2 monté par Miguel Grandmont-Champagne.***

Dans cet exercice, vous vous familiariserez avec les différentes commandes Linux qui permettent de manipuler les fichiers et les répertoires. Souvenez-vous que, sous Linux, tout est fichier. Un fichier peut contenir du texte ou un programme exécutable, être un répertoire, voire même un périphérique tel qu'un disque ou un port USB.

Pour manipuler les fichiers et les répertoires, nous utiliserons fréquemment les commandes `ls`, `cp`, `mv`, `mkdir` et `rm`.

# Déroulement

## Étape 1
La commande `ls` permet de lister les fichiers. Elle accepte un nombre arbitraire d'arguments, puis elle liste le contenu de chacun des répertoires qui lui ont été fourni.

On l'utilise comme suit.
> <p style="font-family:'Courier New'"> ls [OPTIONS]... [FICHIER]... </p>

Utilisée seule, sans option ni argument, la commande `ls` liste les fichiers du répertoire courant.
```
etudiant@vm-al1:~$ ls
cv.txt  exercices  instructions.txt  notes  projet_final

```

## Étape 2
Pour lister **tous** les fichiers de votre répertoire personnel, incluant les fichiers cachés, utilisez l'option `-a`. Votre résultat pourrait différer.
```
etudiant@vm-al1:~$ ls -a
.              .bashrc  exercices         .lesshst  projet_final
..             .cache   .face             .local    .sudo_as_admin_successful
.bash_history  .config  .face.icon        notes
.bash_logout   cv.txt   instructions.txt  .profile

```
On peut aisément repérer les fichiers cachés: ils débutent par le caractère point `.` .


## Étape 3
Pour obtenir davantage d'informations sur les fichiers, comme par exemple le type, le propriétaire ou la date de dernière modification, on utilise l'option `-l` de la commande `ls`.  

Nous allons maintenant lister le contenu du répertoire `/usr/lib/openssh` au format détaillé.
Lancez la commande:
```
ls -l /usr/lib/openssh
```
```
etudiant@vm-al1:~$ ls -l /usr/lib/openssh
total 1560
-rwxr-xr-x 1 root root    736  8 mai 06:54 agent-launch
-rwsr-xr-x 1 root root 653888  8 mai 06:54 ssh-keysign
-rwxr-xr-x 1 root root 469360  8 mai 06:54 ssh-pkcs11-helper
-rwxr-xr-x 1 root root 465264  8 mai 06:54 ssh-sk-helper

```

> Le premier champ indique le **type de fichier avec les permissions**, (nous reviendrons sur la notion de permission plus tard dans le cours).
> <p style="font-family:'Courier New'"> <mark>-rwxr-xr-x</mark> 1 root root    736  8 mai 06:54 agent-launch </p>
Le deuxième champ indique le **nombre de *hard links*** vers le fichier. 
> <p style="font-family:'Courier New'"> -rwxr-xr-x <mark>1</mark> root root    736  8 mai 06:54 agent-launch </p>
Les troisième et quatrième champs représentent respectivement **l'utilisateur propriétaire et le groupe propriétaire** du fichier.
> <p style="font-family:'Courier New'"> -rwxr-xr-x 1 <mark>root root</mark> 736  8 mai 06:54 agent-launch </p>

Le cinquième champ indique la **taille en octet** du fichier.
> <p style="font-family:'Courier New'"> -rwxr-xr-x 1 root root <mark>736</mark>  8 mai 06:54 agent-launch </p>
Le sixième champ représente la **date de dernière modification** (horodatage) du fichier.
> <p style="font-family:'Courier New'"> -rwxr-xr-x 1 root root 105608 <mark> mar  4  2019 </mark> agent-launch </p>
Finalement le dernier champ indique le **nom du fichier ou du répertoire**.
> <p style="font-family:'Courier New'"> -rwxr-xr-x 1 root root    736  8 mai 06:54 <mark> agent-launch </mark> </p>

La sortie de la commande `ls -l` est présentée en ordre alphabétique du nom de fichier. Il existe d'autres options qui permettent de trier la sortie de la commande `ls -l`, notamment l'option `-S` pour trier par taille et `-t` pour trier par date de modification. 
## Étape 4
Nous allons maintenant afficher la liste des fichiers du répertoire `/etc/ssh` au **format long** et triée par taille.
```
ls -lS /etc/profile.d/
```
```
etudiant@vm-al1:~$ ls -lS /etc/profile.d/
total 20
-rw-r--r-- 1 root root 1908  9 aoû  2023 vte-2.91.sh
-rw-r--r-- 1 root root 1012 13 nov  2023 gnome-session_gnomerc.sh
-rw-r--r-- 1 root root  967  9 aoû  2023 vte.csh
-rw-r--r-- 1 root root  726  3 avr  2022 bash_completion.sh
-rw-r--r-- 1 root root  376 16 nov  2021 im-config_wayland.sh

```

## Étape 5
Pour inverser le tri de la commande lancée à l'étape 4, on ajoutera l'option `-r` (ou `--reverse`).
```
ls -lSr /etc/profile.d/
```
```
etudiant@vm-al1:~$ ls -lSr /etc/profile.d/
total 20
-rw-r--r-- 1 root root  376 16 nov  2021 im-config_wayland.sh
-rw-r--r-- 1 root root  726  3 avr  2022 bash_completion.sh
-rw-r--r-- 1 root root  967  9 aoû  2023 vte.csh
-rw-r--r-- 1 root root 1012 13 nov  2023 gnome-session_gnomerc.sh
-rw-r--r-- 1 root root 1908  9 aoû  2023 vte-2.91.sh

```

## Étape 6
L'option `-R` de la commande `ls` est utilisée pour lister de manière **récursive** le contenu d'un répertoire. Cet option aura pour effet de lister non seulement le contenu d'un répertoire, mais également le contenu des sous-répertoires et de leurs sous-répertoires.

```
ls -R /usr/lib/systemd
```
```
etudiant@vm-al1:~$ ls -R /usr/lib/systemd

/usr/lib/systemd:
boot  catalog  tests  user  user-environment-generators  user-generators  user-preset

/usr/lib/systemd/boot:
efi

/usr/lib/systemd/boot/efi:
linuxx64.efi.stub  systemd-bootx64.efi

/usr/lib/systemd/catalog:
systemd.be.catalog        systemd.bg.catalog  systemd.de.catalog  systemd.it.catalog  systemd.pt_BR.catalog  systemd.zh_CN.catalog
systemd.be@latin.catalog  systemd.catalog     systemd.fr.catalog  systemd.pl.catalog  systemd.ru.catalog     systemd.zh_TW.catalog

/usr/lib/systemd/tests:
testdata

...
```
## Étape 7
Pour lister le répertoire lui-même, sans afficher son contenu, on utilise l'option `-d`. Cette option est souvent combinée avec `-l` pour afficher les permissions d'un répertoire spécifique.
```
ls -d /usr/lib
```
```
etudiant@vm-al1:~$ ls -d /usr/lib
/usr/lib
etudiant@vm-al1:~$ ls -ld /usr/lib
drwxr-xr-x 92 root root 4096  9 jan 11:27 /usr/lib

```

## Étape 8 
Certains caractères d'englobement (wildcard), comme par exemple l'astérisque `*`, sont utilisés avec la commande `ls`. Par exemple, pour lister tous les fichiers qui commencent par la lettre `l` dans le répertoire `/etc`, on lance la commande suivante:
```
etudiant@vm-al1:~$ ls /etc/l*
```

```
etudiant@vm-al1:~$ ls /etc/l*
/etc/ld.so.cache  /etc/ld.so.conf  /etc/legal  /etc/libaudit.conf  /etc/locale.alias  /etc/locale.gen  /etc/localtime  /etc/login.defs  /etc/logrotate.conf  /etc/lsb-release

/etc/ld.so.conf.d:
libc.conf  x86_64-linux-gnu.conf

/etc/logcheck:
ignore.d.server

/etc/logrotate.d:
alternatives  apt  dpkg  rsyslog
```

Remarquez ici que le caractère `*` fait en sorte que `ls` répertorie de manière **récursive**. Autrement dit, elle liste le contenu des répertoires et des sous-répertoires qui débutent par la lattre `l`.

## Étape 9
Pour empêcher la commande `ls` de lister le contenu des répertoires dans l'exemple précédent. on utilise l'option `-d`.
```
etudiant@vm-al1:~$ ls -d /etc/l*
/etc/ldap          /etc/libaudit.conf  /etc/lighttpd      /etc/login.defs
/etc/ld.so.cache   /etc/libblockdev    /etc/locale.alias  /etc/logrotate.conf
/etc/ld.so.conf    /etc/libnl-3        /etc/locale.gen    /etc/logrotate.d
/etc/ld.so.conf.d  /etc/libpaper.d     /etc/localtime
/etc/libao.conf    /etc/libreoffice    /etc/logcheck

```

## Étape 10
La command `file` permet d'inspecter le contenu d'un fichier pour indiquer à l'utilisateur quel est son type. Par exemple, pour vérifier le type du fichier `/etc/hosts`, on utilisera la commande suivante.
```
file /etc/hosts

```
```
etudiant@vm-al1:~$ file /etc/hosts
/etc/hosts: ASCII text
```
> Plusieurs commandes sous Linux exigent un certain type de fichier pour s'exécuter correctement, notamment le type texte. La commande `file` peut être utilisée pour valider qu'un fichier est du bon type avant de le traiter avec une commande.

## Étape 11
Pour vérifier le type de fichier de `/usr/bin/sudo`, on lance la commande suivante.
```
 file /usr/bin/sudo

```
```
etudiant@vm-al1:~$ file /usr/bin/sudo
/usr/bin/sudo: setuid ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=104705b93a9f751814766a7348238f32b5fb1b87, for GNU/Linux 3.2.0, stripped

```
La sortie de la commande indique que le fichier est de type **Executable Link Format (ELF)**, ce qui n'est pas un fichier texte.

## Étape 12
La commande `touch` est utilisée pour créer un nouveau fichier vide ou mettre à jour l'horodatage d'un fichier existant.
On l'utlilise comme suit.
> <p style="font-family:'Courier New'"> touch [OPTION]... FICHIER... </p>

Si le fichier spécifié en argument n'existe pas, alors la commande crée un nouveau fichier, comme dans l'exemple suivant

```
touch nouveau
ls -l nouveau
```
```
etudiant@vm-al1:~$ touch nouveau
etudiant@vm-al1:~$ ls -l nouveau 
-rw-r--r-- 1 etudiant etudiant 0  7 fév 10:49 nouveau

```

## Étape 13
Si le fichier existe déjà, alors l'horodatage du fichier est mis à jour sans modifier son contenu.

```
etudiant@vm-al1~$ touch nouveau
etudiant@vm-al1:~$ ls -l nouveau 
-rw-r--r-- 1 etudiant etudiant 0  7 fév 10:50 nouveau
```

## Étape 14
La commande `cp` sert à copier des fichiers. Elle est analogue au copier / coller en environnement GUI. La commande `cp` prend au minimum deux arguments. 
Les premiers arguments sont les fichiers à copier et le second argument est la destination de la copie.

> <p style="font-family:'Courier New'"> cp [OPTION]... SOURCE DESTINATION </p>

Pour copier le fichier `/etc/hosts` dans votre répertoire personnel, lancez la commande suivante.
```
cp /etc/hosts ~
```
```
etudiant@vm-al1:~$ cp /etc/hosts ~
etudiant@vm-al1:~$ ls -l hosts 
-rw-r--r-- 1 etudiant etudiant 211  7 fév 10:50 hosts
```
## Étape 15
Pour copier le fichier `/etc/hosts` dans votre répertoire personnel et le renommer à destination comme` hosts.back`, lancez la commande suivante.
```
cp /etc/hosts ~/hosts.back
```
```
etudiant@vm-al1:~$ cp /etc/hosts ~/hosts.back
etudiant@vm-al1:~$ ls -l hosts*
-rw-r--r-- 1 etudiant etudiant 211  7 fév 10:50 hosts
-rw-r--r-- 1 etudiant etudiant 211  7 fév 10:52 hosts.back

```
## Étape 16
Pour lister et ensuite copier le répertoire `/etc/network` vers le répertoire personnel de notre utilisateur, on lance les commandes suivantes. 
```
cp /etc/network/ ~
```
```
etudiant@vm-al1:~$ cp /etc/network/ ~
cp: -r non spécifié ; omission du répertoire '/etc/network/'

```


## Étape 17
Sans option, la commande `cp` omet les répertoires; elle copie seulement les fichiers. Remarquez que rien n'a été copié suite à la commande précédente.
```
etudiant@vm-al1:~$ ls
cv.txt  exercices  instructions.txt  notes  projet_final

```

Pour copier les répertoires, il est essentiel d'ajouter l'option `-r` à la commande `cp` .
```
cp -r /etc/network/ ~


```
```
etudiant@vm-al1:~$ cp -r /etc/network/ ~
tudiant@vm-al1:~$ ls
cv.txt  exercices  instructions.txt  network  notes  projet_final

```

## Étape 18
La commande `mv` permet de déplacer les fichier d'un emplacement vers un autre dans le système de fichiers.
> <p style="font-family:'Courier New'"> mv [OPTION]... SOURCE DESTINATION </p>

Nous allons d'abord créer le répertoire `~/exercices/semaine3`
```
mkdir -p ~/exercices/semaine3
```

Pour déplacer le fichier `instructions.txt` vers le répertoire  `~/exercices/semaine3`, on utilise la commande suivante.
```
mv instructions.txt exercices/semaine3/
```
```
etudiant@vm-al1:~$ mv instructions.txt exercices/semaine3/

etudiant@vm-al1:~$ ls
cv.txt  exercices  network  notes  projet_final
etudiant@vm-al1:~$ ls exercices/semaine3/
instructions.txt

```

## Étape 19
La commande `mv` permet de déplacer plusieurs fichiers d'un coup vers un nouvel emplacement. Par exemple, nous allons déplacer les fichiers `vc.txt`, `network/` et `notes/` vers `exercices/semaine3/`. L'option `-v` (pour *verbose*) de la commande `mv` permet d'afficher à l'écran ce que la commande effectue.
```
mv -v cv.txt network/ notes/ exercices/semaine3/
```
```
etudiant@vm-al1:~$ mv -v cv.txt network/ notes/ exercices/semaine3/
renommé 'cv.txt' -> 'exercices/semaine3/cv.txt'
renommé 'network/' -> 'exercices/semaine3/network'
renommé 'notes/' -> 'exercices/semaine3/notes'

```
> **Remarque importante:** l'option `-r` n'est pas nécessaire avec la commande `mv` pour déplacer des répertoires.

## Étape 20
La commande `mv` est également utile pour renommer des fichier ou des répertoires. Par exemple, pour renommer le répertoire `~/exercices/` pour `~/exercices-pratiques/`, on utilisera `mv` comme suit.
```
mv -v exercices/ exercices-pratiques
```
```
etudiant@vm-al1:~$ mv -v exercices/ exercices-pratiques
renommé 'documents/exercices/' -> 'documents/exercices-pratiques'

```
> Le caractère tilde `~` est facultatif ici puisque notre répertoire courant est notre répertoire personnel.

## Étape 21
La commande `rm` permet de supprimer des fichiers et des répertoires.

Tentez de supprimer le répertoire exercices-pratiques comme suit:
```
rm exercices-pratiques/
```
```
etudiant@vm-al1:~$ rm exercices-pratiques/
rm: impossible de supprimer 'exercices-pratiques/': est un dossier

```
On remarque que la commande `rm` seule refuse de supprimer un répertoire. Il faut donc ajouter l'option `-r` pour supprimer un répertoire. 
```
rm -r exercices-pratiques/
```
```
etudiant@vm-al1:~$ rm -r exercices-pratiques/
```


# Englobement simple
## Étape 1
Dans votre répertoire personnel, créez un répertoire nommé `glob` puis positionnez votre shell à l'intérieur de ce répertoire.
```
mkdir glob
cd glob/
```

```
etudiant@vm-al1:~/glob$ pwd
/home/etudiant/glob
```
## Étape 2
Nous allons maintenant créer quelques fichiers d'exercices. Pour ce faire, nous allons utiliser une nouvelle astuce du shell, les accolades `{}` pour désigner une intervalle. Les accolades ne font pas partie de l'englobement. 

Lancez la commande suivante pour créer plusieurs fichiers vides en une seule commande:
```
touch data_{0..10}.txt
touch data_{a..d}.txt
touch archives{2000..2022}.tar.gz
```
Listez ensuite le contenu du répertoire `glob`.
```
etudiant@vm-al1:~/glob$ ls
archives2000.tar.gz  archives2008.tar.gz  archives2016.tar.gz  data_10.txt  data_8.txt
archives2001.tar.gz  archives2009.tar.gz  archives2017.tar.gz  data_1.txt   data_9.txt
archives2002.tar.gz  archives2010.tar.gz  archives2018.tar.gz  data_2.txt   data_a.txt
archives2003.tar.gz  archives2011.tar.gz  archives2019.tar.gz  data_3.txt   data_b.txt
archives2004.tar.gz  archives2012.tar.gz  archives2020.tar.gz  data_4.txt   data_c.txt
archives2005.tar.gz  archives2013.tar.gz  archives2021.tar.gz  data_5.txt   data_d.txt
archives2006.tar.gz  archives2014.tar.gz  archives2022.tar.gz  data_6.txt
archives2007.tar.gz  archives2015.tar.gz  data_0.txt           data_7.txt
```

## Étape 3
Nous allons maintenant utiliser l'englobement avec le méta-caractère `*` pour lister tous les fichiers dont le nom débute par `data_` suivi de **n'importe quoi** et de l'extension `.txt`.
```
ls data_*.txt
```
```
etudiant@vm-al1:~/glob$ ls data_*.txt
data_0.txt   data_2.txt  data_5.txt  data_8.txt  data_b.txt
data_10.txt  data_3.txt  data_6.txt  data_9.txt  data_c.txt
data_1.txt   data_4.txt  data_7.txt  data_a.txt  data_d.txt
```
## Étape 4
Remplacez le caractère `*` de la commande précédente par `?`.
```
ls data_?.txt
```
```
etudiant@vm-al1:~/glob$ ls data_?.txt
data_0.txt  data_2.txt  data_4.txt  data_6.txt  data_8.txt  data_a.txt  data_c.txt
data_1.txt  data_3.txt  data_5.txt  data_7.txt  data_9.txt  data_b.txt  data_d.txt

```
Constatez l'absence d'un fichier ici: celle de `data_10.txt` qui n'a pas satisfait l'englobement `data_?.txt`. Cela est dû au fait que ? représente un et un seul caractère.

## Étape 5
Pour lister tous les fichiers dont le nom débute par `data_` suivi **d'exactement deux caractères** et de l'extension `.txt`, on lance la commande suivante:
```
ls data_??.txt
```
```
etudiant@vm-al1:~/glob$ ls data_??.txt
data_10.txt
```
Seul le fichier `data_10.txt` satisfait cette expression.

## Étape 6
Les crochets sont utilisés pour désigner un ensemble de caractères dans une expression d'englobement. Pour lister tous les fichiers débutant par `data_` suivi **des chiffres 2,3 ou 8** et de l'extension `.txt`, on lance la commande suivante:
```
ls data_[238].txt
```
```
etudiant@vm-al1:~/glob$ ls data_[238].txt
data_2.txt  data_3.txt  data_8.txt
```
## Étape 7
Il est également possible de spécifier une intervalle de chiffres (et non de nombres !) et de lettres entre crochets `[]`.
Pour lister tous les fichiers débutant par `data_` suivi **des chiffres entre 0 et 6** et de l'extension `.txt`, on lance la commande suivante:
```
ls data_[0-6].txt
```
```
etudiant@vm-al1:~/glob$ ls data_[0-6].txt
data_0.txt  data_2.txt  data_4.txt  data_6.txt
data_1.txt  data_3.txt  data_5.txt
```

## Étape 8
La négation peut également être utilisée dans une expression d'englobement pour exclure certains résultats. Pour lister tous les fichiers débutant par `data_` suivi **d'une valeur autre que 0,1,2,3,4,5 et 6** et de l'extension `.txt`, on lance la commande suivante:
```
ls data_[!0-6].txt
```
```
etudiant@vm-al1:~/glob$ ls data_[!0-6].txt
data_7.txt  data_8.txt  data_9.txt  data_a.txt  data_b.txt  data_c.txt  data_d.txt

```

# Questions de révision et d'exploration de la seance 4
Vous pouvez télécharger le questionnaire ici [seance4.md](https://cmaisonneuveqcca-my.sharepoint.com/:t:/g/personal/eterriault_cmaisonneuve_qc_ca/EWx-BVIwKRdCk00424ysKIwB0o3jdst9CTK1ENWBhd3jDg?e=74BLxq)

## Mise en place
Commencez par effectuer la commande `lab2` dans votre terminal pour restaurer les fichiers originaux.

```
etudiant@vm-al1:~/$ lab2
✅ Structure du laboratoire créée avec succès.
```

## Question 1 (chemin relatif et absolu)
a) Quel est votre répertoire courant ? Quelle commande vous permet de le savoir?
```
```
b) Positionnez d'abord le shell dans dans le dossier `projet_final/` de votre répertoire personnel. Retournez dans votre répertoire personnel. Comment avez-vous fait ?
```
```
c) Trouvez la commande pour positionner le shell dans votre répertoire personnel, mais avec son chemin absolu.
```
```
d) À partir de votre répertoire personnel, comment pouvez vous vous positionner dans le répertoire `/home` à l'aide de son chemin relatif? 
```
```
e) À partir de votre répertoire personnel, comment pouvez vous vous positionner à la racine `/` à l'aide de son chemin relatif? 
```
```

f) À partir de votre répertoire personnel, comment pouvez vous vous positionner dans le dossier `/var/log`, mais à l'aide de son chemin relatif? Pourquoi ce n'est pas une bonne idée d'utiliser le chemin relatif dans cette situation ?
```
```
## Question 2 (se déplacer dans l'arborescence)
Observez la structure de répertoire suivante tirée de votre machine virtuelle. Certains répertoires ont été omis pour alléger l'affichage.
```
/
├── etc/
│   ├── network/
│   │   └── interfaces
│   ├── systemd/
│   │   ├── resolved.conf
│   │   ├── system/
│   │   ├── system.conf
│   │   ├── user/
│   │   └── user.conf
│   └── udev/
│       ├── rules.d/
│       └── udev.conf
└── home/
    └── etudiant/
        ├── projet_final/
        ├── exercices/
        └── notes/
 
```

**Entrez la commande la plus courte possible pour chacune des questions suivantes.**

a) Votre répertoire courant est la racine (`/`). Entrez la commande pour se déplacer vers le répertoire `code` (dans `/home/etudiant`).
```
```
b) Votre emplacement actuel est la racine (/). Entrez la commande pour accéder au répertoire nommé `network`
```
```
c) Votre emplacement actuel est `projet_final`. Accédez au répertoire nommé `home`.
```
```
d) Votre emplacement actuel est `usr/lib/systemd`. Accédez au répertoire nommé `network` .
```
```


## Question 3 (commande mv)

**La commande `mv` permet de déplacer et de renommer un fichier.** 

a) Positionnez-vous dans votre répertoire personnel (`~`) et créez un répertoire nommé `s3`. Dans le répertoire `s3`, créez ensuite un fichier nommé `q1.txt`.
```
```
b) Utilisez la commande `mv` pour renommer le répertoire `s3` pour `semaine-3`.
```
```
c) À partir de votre répertoire personnel, renommez le fichier `q1.txt` pour `question1.txt`. (Ne changez pas votre répertoire courant pour `semaine-3`. Ne lancez pas `cd semaine-3` avant de renommer le fichier. Faites-le à partir de votre répertoire courant.)
```
```
d) Déplacez le répertoire `semaine-3` dans `exercices/`.
```
```
## Question 5 (commande cp)
**La commande `cp` permet de copier un fichier ou un répertoire.**

a) Positionnez-vous dans votre répertoire personnel (`~`) et créez un répertoire nommé `sauvegarde`. 
Utilisez maintenant la commande `cp` pour faire une copie du répertoire `projet_final` dans le répertoire sauvegarde.
```
```

b) Lancez une seconde fois la commande `cp` pour faire une copie du répertoire `projet_final` dans le répertoire sauvegarde. Que s'est-il passé ? (indice: ajoutez l'option `-v` à la commande `cp`)
```
```
c) Lancez une troisième fois la commande `cp` pour faire une copie du répertoire `projet_final` dans le répertoire sauvegarde. Cette fois, ajoutez l'option `-i` à la commande. Que se passe-t-il ?
```
```

## Question 6 (commande rm)
Utilisez maintenant la commande `rm` pour supprimer le répertoire `sauvegarde` que vous avez créé à la question précédente. 
Quelle commande avez-vous saisie ?
```
```
## Question 7 (englobement)

Pour chacune des expressions suivantes, fournissez au moins trois exemples de noms de fichiers correspondant aux expressions.
Par exemple: 
|Expression|Exemples|
|-|-|
|\*e| Marie, louise, mère|


|Expression|Exemples|
|-|-|
|h*||
|j???||
|br\[a-z\]\[a-z\]d||
|h*\[!a-g\]

## Question 8 (englobement)
Trouvez une expression d’englobement pour les chaînes suivantes. Utilisez les crochets `[]` et le point d'interrogation `?` dans au moins une réponse.

|Exemples|Expression|
|-|-|
|Fichier_A, GroupeA, Test-A| \*A |


|Exemples|Expression|
|------------------------------|----------|
| Wind, windy, cindy||
|fichier1, fichier2, fichier3||
|Gap, GRp, GCC||
|test1,table2,tree8||

