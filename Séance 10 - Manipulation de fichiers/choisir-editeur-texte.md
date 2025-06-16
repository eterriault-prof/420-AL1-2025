***Note: Le contenu suivant a été tiré du cours 420-2C2 monté par Miguel Grandmont-Champagne.***
# Introduction

Comme vous avez pu le constater jusqu'à présent, les fichiers texte ont un rôle fondamental dans le système Linux. Nous avons vu différentes commandes pour traiter des fichiers dans la console. Nous allons dans ce module aborder la notion d'éditeur texte.


L'édition de fichiers est une tâche récurrente pour un utilisateur de Linux. En effet la configuration du système et des applications tient dans des fichiers texte. De plus, Linux permet d'utiliser presque tous les langages de programmation existants. Un bon éditeur texte permettra d'écrire et de modifier le code source de programmes ou de scripts.

> Un éditeur texte un outil qui permet d'éditer le contenu d'un fichier au format texte. Il ne faut pas le confondre avec un logiciel de traitement de texte comme Microsoft Word qui assure la mise en forme et la mise en page du texte. 


# Le choix d'un éditeur: vi, emacs, nano...
Il existe plusieurs éditeurs texte sous Linux. Certains d'entre-eux sont simples (comme `nano`) et d'autres sont un peu plus compliqués à utiliser (comme `emacs` et `vi`). 

Le choix d'un éditeur est avant tout une question d'efficacité. Un développeur ou un administrateur Linux aura avantage à maîtriser `vi` ou `emacs` et ses nombreux raccourcis clavier, ses modes d'affichage, d'édition et d'exécution.  Il gagnera beaucoup de temps et sera très efficace.

Pour un utilisateur débutant, un éditeur texte simple peut très bien faire le travail. C'est pourquoi nous nous concentrerons sur `nano`. Ce-dernier est en quelque sorte analogue au bloc notes sous Windows (ou `notepad.exe`). 

Il s'agit toutefois d'un choix controversé comme l'illustre le *meme* suivant.
![cyntyclwkaaov__.png](/static/images/AL1/editeur_texte/cyntyclwkaaov__.png)

# Utiliser nano
## Lancer l'éditeur
Pour lancer l'éditeur `nano`, il suffit d'invoquer son nom dans la console.
```
etudiant@al1-vm:~$ nano
```
>On peut aussi fournir en argument le nom de fichier à ouvrir ou à créer s'il n'existe pas.


L'éditeur texte s'ouvre et un document vide s'affiche.

![etudiant@e123456-gchampagne-miguel_~_172.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_172.png)

On peut dès lors commencer à écrire du texte dans le fichier.

![etudiant@e123456-gchampagne-miguel_~_174.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_174.png)

## Raccourcis clavier
L'éditeur nano offre plusieurs raccourcis clavier. Les combinaisons de touches sont indiquées en tout temps dans le bas de la page. Le caractère <kbd>^</kbd> représente la couche <kbd>CTRL</kbd> du clavier. 
![sélection_177.png](/static/images/AL1/editeur_texte/sélection_177.png)

### Enregistrer les changements (CTRL + O)
Pour enregistrer les changements apportés à un fichier texte, on peut utiliser la combinaison de touche <kbd>CTRL</kbd> + <kbd>O</kbd>. Si le fichier n'existe pas, on demande de saisir le nom du nouveau fichier à enregistrer. Dans cet exemple on souhaite créer le fichier **bonjour-nano.txt**. Le fichier sera créé dans le répertoire courant du shell où l'on a lancé `nano`.
![etudiant@e123456-gchampagne-miguel_~_178.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_178.png)

Le fichier est maintenant créé et les changements ont été enregistrés.

![etudiant@e123456-gchampagne-miguel_~_179.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_179.png)

### Quitter `nano` (CTRL + X)
Pour quitter nano, on utilise la combinaison <kbd>CTRL</kbd> + <kbd>X</kbd>. Si des changements ont été apportés au fichier depuis le dernier enregistrement, on nous demande si nous souhaitons enregistrer les changements avant de quitter. Autrement, l'éditeur texte se ferme.

Constatons la présence du fichier `bonjour-nano.txt` dans notre répertoire personnel.

```
etudiant@al1-vm:~$ ls bon*
bonjour-nano.txt

etudiant@al1-vm:~$ cat bonjour-nano.txt 
Bonjour tout le monde, je suis l'éditeur GNU nano !
```

Pour éditer à nouveau ce fichier, on lance la commande:
```
etudiant@al1-vm:~$ nano bonjour-nano.txt
```
> Attention! L'éditeur nano permet de modifier le contenu d'un fichier dès son ouverture. Il ne dispose pas de mode *insertion* ou *modification*. On est toujours en mode édition ! Il faut être très prudent pour ne pas modifier accidentellement un fichier.


> Une pratique courrante sous Linux consiste à faire une copie de sauvegarde avant de modifier un fichier. Par exemple, si on souhaite modifier le fichier `sources.list`, on créera une copie de sauvegarde comme suit: `cp sources.list sources.list.bkp`. En cas de problème on pourra restaurer le fichier à l'aide de la commande `mv sources.list.bkp sources.list`.


### Rechercher un mot (CTRL + W)
La combinaison de touches <kbd>CTRL</kbd> + <kbd>W</kbd> permet de lancer la recherche pour une chaîne de caractères dans le fichier. En effet, les fichiers que nous éditons sont parfois si longs qu'il est impensable de faire défiler le fichier jusqu'à l'endroit souhaité.

À titre d'exemple, le fichier `~/.bashrc` est un script lancé à chaque connexion d'un utilisateur. Dans ce fichier, on retrouve les différentes personnalisation du shell bash que l'utilisateur a souhaité apporter, notamment les alias de commandes.

Nous allons maintenant ouvrir le fichier `.bashrc` avec nano pour localiser les différents alias créés par défaut lors de la connexion de notre utilisateur.

Avant toute modification, il convient de faire une sauvegarde de sécurité.

```
etudiant@al1-vm:~$ cp .bashrc .bashrc.bkp
```

Éditons maintenant le fichier `.bashrc`. 
```
etudiant@al1-vm:~$ nano .bashrc
```

Une fois `nano` ouvert, on lance la combinaison <kbd>CTRL</kbd> + <kbd>W</kbd> pour localiser une chaîne.
![etudiant@e123456-gchampagne-miguel_~_180.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_180.png)

Saisissez la chaîne **alias** puis appuyez sur <kbd>Entrée</kbd>.
![etudiant@e123456-gchampagne-miguel_~_181.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_181.png)

Remarquez le curseur se positionner sur la première occurence du terme **alias**. Pour défiler chaque correspondance, appuyez sur <kbd>ALT</kbd> + <kbd>Flèche bas</kbd> ou <kbd>ALT</kbd> + <kbd>Flèche haut</kbd>.

![etudiant@e123456-gchampagne-miguel_~_182.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_182.png)

### Se positionner à une ligne

Tous les éditeurs texte permettent d'accéder à directement à un numéro de ligne dans un fichier. Cette fonctionnalité est très utile quand un programme ou un script plante à l'exécution. Souvent l'interpréteur retourne le numéro de ligne où l'erreur a été rencontrée, ce qui permet d'y accéder directement.

Par exemple, prenons le fichier `documents/dictionnaires/français.txt` et accédons directement à la ligne 3000. Pour ce faire, on utilise la combinaison de touche <kbd>CTRL</kbd> + <kbd>_</kbd> (underscore) 

```
etudiant@al1-vm:~$ nano documents/dictionnaires/français.txt 
```

![etudiant@e123456-gchampagne-miguel_~_183.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_183.png)

On saisit **3000** dans l'invite puis <kbd>Entrée</kbd>.
![etudiant@e123456-gchampagne-miguel_~_185.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_185.png)

Le cursueur se positionne automatiquement à la ligne 3000.
![etudiant@e123456-gchampagne-miguel_~_184.png](/static/images/AL1/editeur_texte/etudiant@e123456-gchampagne-miguel_~_184.png)