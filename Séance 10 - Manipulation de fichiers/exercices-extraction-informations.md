***Note: Le contenu suivant a été tiré du cours 420-2C2 monté par Miguel Grandmont-Champagne.***

# Déroulement

## Partie 1: Outils de recherche dans le système de fichiers

Sous Linux, les fichiers sont utilisés pour stocker des données telles que du texte, des images, de vidéo ou des programmes. Les répertoires sont eux aussi considérés comme des fichiers qui contiennent d'autres fichiers. Dans cet exercice, nous verrons comment rechercher des fichiers dans le système de fichiers hiérarchique de Linux (FHS).

Les commandes `locate` et `find` ont toutes deux le même rôle: elles sont utilisées pour lancer une recherche dans le système de fichiers. 

Bien que ces deux commandes exécutent des tâches similaires, chacune le fait en utilisant une technique différente avec ses propres avantages et inconvénients. 

La commande `locate` utilise une base de données normalement mise à jour une fois par jour à l'aide d'une tâche planifiée tandis que la commande `find` recherche en parcourant le système de fichiers à chaque appel.
### Étape 1

La commande `locate` pourrait ne pas être installée sur votre système. 
```
etudiant@al1-vm:~$ locate 
-bash: locate: command not found
```

Pour l'installer, on utilise le gestionnaire de paquets de notre distribution.
```
etudiant@al1-vm:~$ sudo apt update
etudiant@al1-vm:~$ apt search -n locate
etudiant@al1-vm:~$ sudo apt install locate
```

### Étape

La commande `locate` utilise une base de données contenant l'emplacement des fichiers sur le système de fichiers. Cette base de données doit être systématiquement mise à jour quotidiennement ou avant de lancer une nouvelle recherche.

La commande `locate`  accepte une chaîne de caractères comme argument.
```
locate [OPTION]... RECHERCHE...
```

Suite à l'installation de `locate`, sa base de données est vide. N'importe quelle recherche retournera un résultat vide.
```
etudiant@al1-vm:~$ locate hostname
etudiant@al1-vm:~$ 
```
### Étape
Avant d'utiliser la commande `locate`, il convient de mettre à jour sa base de données à l'aide de la commande `updatedb`. Cette dernière doit être lancée par un utilisateur qui dispose des privilèges du superutilisateur (root), c'est pourquoi on la précède de `sudo`. 
> La mise à jour de la base de données est souvent une tâche automatisée par le système. L'utilisateur n'a pas a se soucier de la mettre à jour lui-même à moins que le système de fichiers ait subi des modifications depuis sa dernière indexation.


Pour mettre à jour la base de données, on lance la commande suivante:
```
etudiant@al1-vm:~$ sudo updatedb
[sudo] password for etudiant: 
/usr/bin/find: '/dev/.lxd-mounts': Permission denied
```

> Ignorez l'erreur *Permission denied*

## Partie 2: Extraire des informations dans les fichiers
Le texte est la principale forme d'entrée/sortie des commandes Linux. Par conséquent, il existe de nombreuses commandes dont le rôle est de traiter des fichiers texte.

### Étape 1 (cat)
La commande cat (dérivée du mot concaténer) prend plusieurs fichiers en entrée et les affiche bout à bout.
Sa syntaxe est la suivante:
```
cat [OPTION]... [FICHIER]...
```
Pour afficher le contenu des fichiers `/etc/hosts` et `/etc/hostname`, exécutez les commandes suivantes:
```
cat /etc/hosts 
cat /etc/hostname 
```

```
etudiant@al1-vm:~$ cat /etc/hosts
127.0.1.1 al1-vm
127.0.0.1 localhost
::1 localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

etudiant@al1-vm:~$ cat /etc/hostname 
al1-vm
```
Pour concaténer (afficher bout à bout) le contenu des fichiers `/etc/hosts` et `/etc/hostname`, exécutez la commande suivante:

```
cat /etc/hosts /etc/hostname
```

```
etudiant@al1-vm:~$ cat /etc/hosts /etc/hostname
127.0.1.1 al1-vm
127.0.0.1 localhost
::1 localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

al1-vm
```

### Étape 2
Bien que l'affichage de petits fichiers avec la commande `cat` est couramment utilisée, ce n'est pas la commande idéale pour visualiser des fichiers volumineux dont le nombre de lignes dépasse ce qui peut être affiché d'un coup à l'écran.

Pour les fichiers plus volumineux, une commande de pagination peut être utilisée pour afficher le contenu long. 
Les commandes de pagination affichent le texte page par page, ce qui vous permet d'avancer et de reculer dans le fichier à l'aide des touches de déplacement. 

La commande `less` est l'outil de pagination par défaut de plusieurs distributions Linux. Nous l'avons d'ailleurs déjà rencontré plus tôt dans ce cours avec la commande `man`. La commande `man` affiche le manuel d'aide d'une commande page par page.

Pour afficher un fichier avec la commande `less`, on fournit le nom du fichier en argument. Par exemple:
```
etudiant@al1-vm:~$ less /etc/passwd
```
```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
...
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
ubuntu:x:1000:1000::/home/ubuntu:/bin/bash
sshd:x:105:65534::/run/sshd:/usr/sbin/nologin
etudiant:x:1001:1001:,,,:/home/etudiant:/bin/bash

/etc/passwd (END)
```

La touche <kbd>espace</kbd> permet de défiler les pages de less jusqu'à la fin. Pour quitter le manuel, appuyez sur <kbd>q</kbd>.

### Étape 3


Expérimentez maintenant les différents déplacements possibles dans `less`.
| Commande | Rôle                                    |
|----------|-----------------------------------------|
| <kbd>Entrée</kbd>   | Descendre d'une ligne                   |
| <kbd>Espace</kbd>   | Descendre d'une page                    |
| `/qqc`     | Rechercher "qqc" dans la page |
| <kbd>n</kbd>        | Repérer le prochain élément recherché   |
| <kbd>1G</kbd>      | Retourner au début du fichier |
| <kbd>G</kbd>        | Aller à la fin du fichier     |
| <kbd>h</kbd>        | Afficher l'aide                         |
| <kbd>q</kbd>        | Quitter `less`              |

### Étape 4
La commande `head` permet d'afficher le début d'un fichier ou d'une sortie à l'écran.

```
head [OPTION]... [FICHIER]...
```

Pour afficher les 10 premières lignes du fichier `~/documents/dictionnaires/français.txt`, lancez la commande.

```
etudiant@al1-vm:~$ head documents/dictionnaires/français.txt 
```

```
etudiant@al1-vm:~$ head documents/dictionnaires/français.txt 
a
à
abaissa
abaissable
abaissables
abaissai
abaissaient
abaissais
abaissait
abaissâmes
```

### Étape 5
Si le nombre de lignes n'est pas spécifié, la commande `head` affichera les 10 premières lignes. Pour afficher les 5 premières lignes de la sortie du fichier précédent, exécutez la commande suivante:
```
etudiant@al1-vm:~$ head -5 documents/dictionnaires/français.txt 
```
```
etudiant@al1-vm:~$ head -5 documents/dictionnaires/français.txt 
a
à
abaissa
abaissable
abaissables
```

### Étape 6
La commande `tail` affiche les dernières lignes d'un fichier.
```
tail [OPTION]... [FICHIER]...
```
Pour afficher les 10 dernières lignes du fichier `employés.csv`, lancez la commande suivante:

```
etudiant@al1-vm:~$ tail documents/ressources\ humaines/employés.csv 
```

```
etudiant@al1-vm:~$ head documents/dictionnaires/français.txt 
Terri-jo,Hablot,(122) 8034643,thablotri@livejournal.com,135.148.161.170,$27.21
Waite,Seals,(106) 3541809,wsealsrj@1688.com,216.87.195.50,$19.58
Romy,Ginity,(640) 7243676,rginityrk@istockphoto.com,26.230.128.111,$43.75
Cornelius,Baise,(419) 6166410,cbaiserl@flavors.me,1.94.179.71,$54.71
Odele,Jamrowicz,(332) 6231778,ojamrowiczrm@google.com.br,179.111.199.51,$16.74
Janeva,Ben-Aharon,(987) 6437973,jbenaharonrn@1und1.de,37.52.186.193,$28.11
Gabe,Ebbage,(154) 7150116,gebbagero@blogtalkradio.com,206.250.251.74,$30.84
Pearl,Urrey,(186) 2256717,purreyrp@parallels.com,28.62.21.204,$43.62
Bunni,Haysey,(465) 2490656,bhayseyrq@bluehost.com,231.153.12.254,$29.67
Clare,Turmall,(174) 9010671,cturmallrr@phoca.cz,105.15.170.119,$21.00
```

Pour afficher les 2 dernières lignes, on lancez:
```
etudiant@al1-vm:~$ tail -2 documents/ressources\ humaines/employés.csv 

```
```
etudiant@al1-vm:~$ tail -2 documents/ressources\ humaines/employés.csv 
Bunni,Haysey,(465) 2490656,bhayseyrq@bluehost.com,231.153.12.254,$29.67
Clare,Turmall,(174) 9010671,cturmallrr@phoca.cz,105.15.170.119,$21.00
```
### Étape 7
La commande `cut` est utilisée pour extraire des champs d'un fichier texte. L'espace et la tabulation sont les délimiteurs (ce qui sépare les champs) par défaut et peuvent être modifiés à l'aide de l'option `-d`.

>Référez-vous aux notes de cours pour vous remémorer la structure du fichier `/etc/passwd`.

Pour extraire les 1er, 3e et 7e champs du fichier `/etc/passwd`, exécutez la commande suivante:
```
etudiant@al1-vm:~$  cut -d ':' -f 1,3,7 /etc/passwd
```

```
etudiant@al1-vm:~$  cut -d ':' -f 1,3,7 /etc/passwd
root:0:/bin/bash
daemon:1:/usr/sbin/nologin
bin:2:/usr/sbin/nologin
...
...
_apt:104:/usr/sbin/nologin
ubuntu:1000:/bin/bash
sshd:105:/usr/sbin/nologin
etudiant:1001:/bin/bash
```
### Étape 8
La commande `sort` est utile pour travailler avec des données structurées avec des colonnes. Elle permet d'afficher un fichier trié sur un champ de données spécifique. Par défaut le délimiteur entre les champs est l'espace, mais il peut être spécifié par l'option `-t`. Le champ (la colonne) sur laquelle on souhaite trier est donné par l'option `-k`.
```
sort [OPTION]... [FICHIER]...
```
Pour trier le fichier `documents/ressources\ humaines/employés.csv` en fonction de l'adresse courriel (4e colonne), on utilisera la commande qui suit. À noter que le délimiteur entre les colonnes est la virgule.

```
etudiant@al1-vm:~$ sort -t ',' -k 4  documents/ressources\ humaines/employés.csv 
```
### Étape 9
Pour trier le fichier `/etc/passwd` en utilisant le premier champ (c'est-à-dire, le nom d'utilisateur) et en utilisant le caractère deux-points `:` délimiteur, exécutez la commande suivante:
```
etudiant@al1-vm:~$ sort -t ':' -k 1 /etc/passwd
```
```
etudiant@al1-vm:~$ sort -t ':' -k 1 /etc/passwd
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
etudiant:x:1001:1001:,,,:/home/etudiant:/bin/bash
games:x:5:60:games:/usr/games:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin

...
```
### Étape 10
La commande `grep` agit comme un filtre. Elle permet d'extraire les lignes qui contiennent une chaîne de caractère fournie par l'utilisateur.

Cette commande est très utile pour extraire une donnée spécifique dans un fichier complexe.

Par exemple, pour extraire toutes les lignes du fichier `~/documents/ressources humaines/employés.csv 
` qui contiennent la chaîne ***hall***, on lance la commande suivante:
```
etudiant@al1-vm:~$ grep hall documents/ressources\ humaines/employés.csv
```
```
etudiant@al1-vm:~$ grep hall documents/ressources\ humaines/employés.csv 
Domeniga,Broomhall,(834) 4629067,dbroomhall66@wordpress.org,215.8.235.131,$54.98
Franky,Hallward,(600) 1428208,fhallward71@chron.com,153.111.112.180,$16.18
Janela,Hallworth,(180) 4352142,jhallworthhi@hp.com,144.31.10.147,$22.13
Odell,Hallan,(812) 5312562,ohallanl2@godaddy.com,222.196.243.188,$24.43
```
Sur les 1000 lignes du fichier, seulement 4 contiennent la chaîne ***hall***.

> Attention, la commande `grep` est sensible à la casse.

### Étape 11
Il est commun qu'un utilisateur souhaite paginer la sortie d'une commande très longue. Un exemple concret pourrait être de lister au format long le contenu du répertoire `/etc`. 

Lancez la commande `ls -l /etc`. On remarque que la sortie de la commande est plus longue que la taille de notre écran et que les premières lignes du la sortie sont passées sous nos yeux sans que l'on ait pu les lire. 
![e1234567_–_chromium_160.png](/e1234567_–_chromium_160.png =600x)

Il y a deux solutions à ce problème. 
- Premièrement utiliser la roulette de la souris pour remonter dans la sortie du terminal virtuel. Ceci est possible seulement dans un terminal virtuel en environnement graphique (comme guacamole) et le nombre de lignes conservées est limité. Dans une console ouverte directement sur l'ordinateur sans GUI, il faudra utiliser les touches <kbd>Maj</kbd> + <kbd>PageUp</kbd>.
- La meilleure solution consiste à paginer la sortie de la commande pour l'afficher page par page. Pour ce faire, **on enchaîne la commande souhaitée  à `less`** en ajoutant ` | less` (prononcer *pipe less*, à l'anglaise).
	Par exemple, la commande `ls -l /etc` deviendrait `ls -l /etc | less`. Le résultat de la commande `ls` devient ainsi l'entrée de la commande `less`. C'est un exemple très commun d'enchaînement de commandes sous Linux. On peut ensuite faire défiler l'affichage en utilisant les touches de navigation de la commande `less` comme <kbd>espace</kbd> pour défiler les pages. Nous verrons l'enchaînement de commandes plus en détail dans la séance suivante, mais en voici un aperçu.

```
ls -l /etc | less
```
```
etudiant@al1-vm:~/documents/ressources humaines$ ls /etc/ -l | less
total 192
-rw-r--r-- 1 root   root    3028 jan  7 02:43 adduser.conf
drwxr-xr-x 2 root   root      85 mar  6 11:28 alternatives
drwxr-xr-x 3 root   root       3 jan  7 02:43 apparmor
drwxr-xr-x 5 root   root       8 jan  7 10:24 apparmor.d
....
....
drwxr-xr-x 2 root   root      11 mar  6 11:28 cron.daily
drwxr-xr-x 2 root   root       3 jan  7 02:44 cron.hourly
drwxr-xr-x 2 root   root       3 jan  7 02:44 cron.monthly
:
```

### Étape 12
Toujours en utilisant le principe d'enchaînement, nous pouvons compter le nombre de fichiers que contient le répertoire `/etc`. Sachant que la commande `ls -l /etc` produit une ligne par fichier à la sortie, on peut enchaîner cette commande à `wc -l` qui compte le nombre de lignes de son entrée.
Par exemple:
```
etudiant@al1-vm:~$ ls -l /etc/ | wc -l
```

```
etudiant@al1-vm:~$ ls -l /etc/ | wc -l
139
```
>Attention. L'option `-l` avec la commande `ls` ne signifie pas la même chose que l'option `-l` avec la commande `wc`. On pourrait réécrire une commande équivalente avec les options longues, ce qui donnerait `ls --format=long /etc/ | wc --lines`

Voici la même commande, mais avec les options longues:
```
etudiant@al1-vm:~$ ls --format=long /etc/ | wc --lines 
139
```

## Les répertoires du FHS

1. Vous connaissez maintenant bien la commande ls. Indiquez où se trouve le programme qui permet de l’exécuter.  
```
```
2. En démarrant votre ordinateur, le noyau Linux doit charger certains fichiers essentiels. Dans quel répertoire ces fichiers se trouvent-ils ?  
```
```

3. Votre clé USB est branchée. Dans quel dossier se trouve probablement son contenu?  
```
```

4. Vous avez besoin de modifier la configuration du réseau de votre machine. Dans quel répertoire pouvez-vous trouver les fichiers de configuration ? Donnez un exemple de fichier que vous pourriez y trouver.  
```
```

5. Vous venez de télécharger un script qui modifie les droits des utilisateurs. Vous souhaitez le tester, mais il nécessite des privilèges administrateurs. Où devriez-vous placer ce script pour qu’il soit accessible uniquement aux administrateurs ?  
```
```

6. En listant le contenu du répertoire /dev, vous trouvez des fichiers avec des noms comme sda, tty, et null. Expliquez ce qu'un de ces fichiers représente.  
```
```

7. Un programme que vous utilisez fait l'utilisation de fichiers temporaires. Aujourd'hui, il vous signale une erreur quand il manque d'espace pour écrire dans ses fichiers temporaires. Dans quel répertoire devez-vous vérifier si de l’espace est disponible ?  
```
```

8. Vous êtes un utilisateur classique et vous souhaitez enregistrer un fichier de travail. Où devez-vous le sauvegarder pour y accéder facilement plus tard ?  
```
```

9. Après avoir installé un nouveau logiciel, vous remarquez un problème dans le focntionnement. Vous savez que ce logiciel enregistre des logs. Où pouvez-vous chercher ces fichiers de logs pour comprendre ce qui ne fonctionne pas ?   
```
```


10. Vous êtes un utilisateur classique, vous essayez d’exécuter une commande système importante, mais vous recevez un message d’erreur indiquant que vous n’avez pas les permissions nécessaires. Où est probablement située cette commande et pourquoi ne pouvez-vous pas l’exécuter directement ?  
```
```


## Chercher et extraire des informations des fichiers sur Linux
Utilisez les commandes Linux appropriées (`locate`, `find`, `cat`, `less`, `head`, `tail`, `grep`, `cut`, `wc`, etc.) pour répondre aux questions. Écrivez la commande complète à côté de chaque question.

1. Utilise `locate` pour trouver tous les fichiers contenant "backup" dans leur nom:  
```
```

2. Utilisez la commande `locate` pour localiser un fichier nommé `français.txt`.  
```
```

3. Utilisez la commande `find` pour localiser un fichier nommé `employés.csv`. Démarrez la recherche dans votre répertoire personnel.  
```
```

4. Utilisez la commande `find` pour localiser tous les fichiers portant l'extension `.txt`. Démarrez la recherche dans votre répertoire personnel.  
```
```

5. Trouve tous les fichiers `.log` sur le système:  
```
```

6. Recherche tous les fichiers `.txt` dans le répertoire `/home/etudiant/documents/`:  
```
```

7. Affiche tous les fichiers modifiés au cours des 7 derniers jours dans `/var/log/`:  
```
```

8. Affiche les 10 premières lignes du fichier `~/documents/dictionnaires/phonétique-p1` :  
```
```

9. Affiche les 5 dernières lignes du fichier `~/documents/dictionnaires/phonétique-p1` :  
```
```

10. Trouve toutes les lignes contenant les caractères "bluehost.com" dans `~/documents/ressources humaines/employés.csv` :  
```
```

11. Affiche uniquement la première et la deuxième colonne du fichier `~/documents/ressources humaines/employés.csv` (fichier CSV):  
```
```

12. Compte le nombre total de lignes dans `~/documents/ressources humaines/employés.csv` (on veut seulement afficher le nombre de lignes):  
```
```

13. Utilisez le fichier `~/documents/dictionnaires/français.txt` pour répondre aux questions suivantes.  

a) Affichez toutes les lignes qui contiennent la chaîne "rester".  
```
```

b) La commande `fzf` est une autre commande de recherche très puissante que l'on peut installer dans le shell Linux. Utilisez le gestionnaire de paquets pour installer `fzf`.  
```
```

c) Positionnez votre shell dans votre dosser personnel et lancez la commande `fzf`. Saisissez des lettres pour lancer la recherche d'un fichier. Que remarquez-vous ?  
```
```

d) Positionnez votre shell à la racine du système de fichiers. Lancez à nouveau la commande `fzf`. Tentez de localiser un fichier nommé `sources.list`.  
```
```

e) Positionnez vous à la racine du système de fichiers puis lancez la commande `code $(fzf)`. Recherchez le fichier `sources.list` puis appuyez sur <kbd>Entrée</kbd> lorsque `fzf` l'a trouvé. Que se passe-t-il ?  
```
```

f) Qu'est-ce que la substitution dans le shell Linux ? Faites une petite recherche sur internet.  
```
```

14. Considérez le fichier `employés.csv` que vous avez repéré à la question 1 pour répondre aux questions suivantes.  

a) Combien y a-t-il de lignes dans ce fichier ?  
```
```

b) Affichez le contenu du fichier à l'écran à l'aide de la commande `cat`. Pourquoi `cat` n'est pas un bon choix de commande pour afficher ce fichier à l'écran ?  
```
```

c) Quelle commande pourrait être un meilleur choix pour afficher ce fichier ?  
```
```

d) Affichez les 8 premières lignes du fichier à l'écran.  
```
```

e) Affichez la dernière ligne du fichier à l'écran (seulement cette ligne).  
```
```

f) Quel est le prénom de l'employé(e) à la ligne 183 de ce fichier?  
```
```

g) Quelle commande vous permet d'afficher uniquement les prénoms? Les prénoms sont indiqués dans la première colonne.  
```
```

15. Considérez le fichier `/etc/passwd`.  

a) Affichez tous les noms d'utilisateurs qui ont le shell de connexion `/bin/bash` (Le shell de connexion est le dernier champ sur chaque ligne du fichier `/etc/passwd`).  
```
```

b) Toujours à partir du fichier `/etc/passwd`, affichez **le nombre** d'utilisateurs du système qui ont le shell de connexion `/usr/sbin/nologin`. Indice: utilisez le manuel de la commande grep pour trouver une option qui répondrait à ce besoin.  
```
```

c) Considérez le fichier `employés.csv`. Affichez toutes les lignes qui ont un courriel `@google.com.br`.  
```
```

