# Les fichiers et les répertoires

***Note: Le contenu suivant a été tiré du cours 420-2C2 monté par Miguel Grandmont-Champagne.***


Tout est considéré comme un fichier sous Linux. Par conséquent, la gestion des fichiers est un sujet très important dans ce cours. Sous Linux, non seulement les documents sont considérés comme des fichiers, mais les périphériques matériels tels que les disques durs et la mémoire sont également considérés comme des fichiers. Même les répertoires sont considérés comme des fichiers car ce sont des fichiers spéciaux qui sont utilisés pour contenir d'autres fichiers.

Les commandes essentielles nécessaires à la gestion des fichiers sont souvent nommées par de courtes abréviations de leurs fonctions. 

Par exemple, vous pouvez utiliser la commande `ls` pour lister les fichiers, la commande `cp` pour copier des fichiers, la commande `mv` pour déplacer ou renommer des fichiers et la commande `rm` pour supprimer des fichiers.

Pour travailler avec des répertoires, on utilisera la commande `mkdir` pour créer de nouveaux répertoires, la commande `rmdir` pour supprimer les répertoires (s'ils sont vides) et la commande `rm` pour supprimer récursivement les répertoires contenant des fichiers.

## Organisation hiérarchique des répertoires
Le système de fichiers Linux est similaire aux systèmes de fichiers de la plupart des systèmes d’exploitation en ce qui concerne les fichiers et les répertoires. 

Sa structure est standardisée et est la même sur chacune des distributions de Linux.

<img src="/static/images/AL1/fichiers_repertoires/linuxfile.png" alt="linuxfile.png" style="width:800px;">

Les répertoires sont utilisés pour créer une organisation au sein du système de fichiers. Les répertoires peuvent contenir des fichiers et d'autres répertoires. Pour visualiser le système de fichiers (ou une partie de celui-ci) sous forme d'arbre, on peut utiliser la commande `tree`.

```
etudiant@vm-al1:al1:~$ tree
.
├── cv.txt
├── exercices
│   ├── à_classer
│   │   ├── brouillon1.txt
│   │   └── brouillon2.txt
│   ├── fichiers
│   │   └── notes_vide.txt
│   └── resultats
│       └── resultat.csv
├── instructions.txt
├── notes
│   ├── semestre1
│   │   ├── cours1.txt
│   │   └── cours2.txt
│   └── semestre2
│       └── a_supprimer
│           └── vieux.txt
└── projet_final
    └── archives
        └── ancien.txt

11 directories, 10 files
```
Si la commande tree n'est pas disponible sur votre système, installez-la à l'aide de la commande suivante.
```
sudo apt update
sudo apt install tree
```

## Noms de fichiers et de répertories
Les noms de fichiers et de répertoires sous Linux peuvent contenir des lettres (minuscules ou majuscules), des chiffres, des espaces et des caractères spéciaux.

Par exemple, les valeurs suivantes sont toutes des noms de fichiers ou de répertoires valides sous Linux.
```
2001_bilans     Code               Images                      réseaux
Automatisation  documentation.txt  liste-des-utilisateurs.pdf  Téléchargements
bilans_2001     Documents          Noël

```

Cependant, étant donné que de nombreux caractères spéciaux ont une signification particulière dans le shell Linux, il est recommandé de ne pas utiliser certains caractères spéciaux lors de la dénomination de fichiers ou de répertoires. Par exemple, les caratères `/`, `>`,`&`, `$`  ont une signification particulière dans le shell.  

Les espaces, par exemple, nécessitent que le caractère d'échappement `\` soit saisi correctement. Par exemple, pour changer le répertoire courant pour `bilan 2023` on devra saisir la commande `cd` comme suit.

```
cd bilan\ 2023
```

Si la barre oblique n'est pas saisie, le shell nous retourne une erreur. En effet, **bilan** et **2023** sont considérés comme deux arguments alors que la commande `cd` n'en accepte qu'un seul.
```
cd bilan 2023
bash: cd: trop d'arguments
```
Les caractères tiret `-` ou soulignement `_`sont toutefois très utilisés à l'intérieur d'un nom de fichier.

## Les extensions de fichiers
Les noms de fichiers peuvent contenir un suffixe (aussi appelé extension) qui vient après le point `.` à la fin d'un nom de fichier. 
Prenons par exemple le nom de fichier `rapport-automne-2022.txt`.

Contrairement à Windows, ce suffixe n'a pas de signification particulière sous Linux ; il est là pour faciliter la compréhension de l'utilisateur. Dans notre exemple, l'extension `.txt` nous indique qu'il s'agit d'un fichier texte, bien qu'il puisse techniquement contenir tout type de données.

> Les extensions de fichiers sous Linux n'ont pas de signification particulière et il est commun de rencontrer les fichiers sans extension.

## La commande file
Comme les extensions de fichiers sous Linux ne sont pas obligatoires, la commande `file` permet de déterminer rapidement le type d’un fichier (fichier texte, fichier binaire, disque dur, périphérique de sortie...)  
```
etudiant@vm-al1:~$ file cv.txt 
cv.txt: ASCII text

```
Dans l'exemple précédent, le fichier `cv.txt` est un fichier texte.

## Sensibilité à la casse
Contrairement à Microsoft Windows, les noms de fichiers et de répertoires sur les systèmes Linux sont **sensibles à la casse**. Cela signifie par exemple que les noms de répertoires `/etc/` et `/ETC/`sont différents. 
Dans l'exemple suivant, l'utilisateur tente de se positionner dans `/ETC` mais ce répertoire n'existe pas et la commande `cd` échoue.
```
etudiant@vm-al1:~$ cd / 
etudiant@vm-al1:/$ ls
bin   home            lib32       media  root  sys  vmlinuz
boot  initrd.img      lib64       mnt    run   tmp  vmlinuz.old
dev   initrd.img.old  libx32      opt    sbin  usr
etc   lib             lost+found  proc   srv   var

```
```
etudiant@vm-al1:/$ cd ETC
bash: cd: ETC: Aucun fichier ou dossier de ce type
```

```
etudiant@vm-al1:/$ cd etc/
etudiant@vm-al1:/etc$ pwd
/etc

```

# Se déplacer dans le système de fichiers
## Obtenir le répertoire courant
Étant donné que les shells Linux tels que Bash sont basés sur le texte, il est important de connaître le répertoire courant avant de lancer une commande. En effet, une commande peut produire un résultat totalement différent lorsqu'elle est lancée dans différents répertoires.

Tel que nous l'avons vu la semaine dernière, l'invite de commande nous indique le répertoire courant du terminal.
```
etudiant@vm-al1:~/projet_final/archives$ 
```
> Notez que des informations ***etudiant*** et ***vm-al1*** représentent respectivement l'utilisateur connecté et le nom de la machine. 

À partir de l'invite, nous savons que notre répertoire courant est le répertoire Rapports. 

La commande `pwd` est une autre façon de connaître le répertoire courant.
```
etudiant@vm-al1:~/code/python$ pwd
/home/etudiant/code/python
```

La hiérarchie des répertoires est représentée par une barre oblique `/`. Nous savons que `archives` est un sous-répertoire de `projet_final`, qui est un sous-répertoire de `etudiant`, qui se trouve dans un répertoire appelé `home`. 

`home` ne semble pas avoir de répertoire parent, mais ce n'est pas tout à fait le cas. Le répertoire parent de home est appelé **la racine** et est représenté par la première barre oblique `/` à gauche de `home`. 

Nous discuterons du répertoire racine dans une section ultérieure.

Remarquez que la sortie de la commande `pwd` diffère légèrement du chemin d'accès indiqué sur l'invite de commande. Au lieu de `/home/etudiant`, l'invite de commande contient un tilde `~`. 

>Le tilde `~` est un caractère spécial qui représente le répertoire personnel de l'utilisateur. 
 
## Changer le répertoire courant
La navigation dans le système de fichier sous Linux se fait principalement avec la commande `cd`. Cette commande change de répertoire pour un autre répertoire fourni en argument. `cd` est l'abréviation de *change directory*. 
La commande `cd` est couramment utilisée de la façon suivante:
```
cd [destination]
```

Nous allons maintenant modifier notre répertoire courant pour le répertoire `/home/etudiant/documents/dictionnaires/`.
>Utilisez la touche <kbd>tab</kbd> pour compléter automatiquement l'argument de la commande `cd`.

```
etudiant@vm-al1:~/projet_final/archives$ cd /home/etudiant/notes/semestre1/

```
```
etudiant@vm-al1:~/notes/semestre1$ ls
cours1.txt  cours2.txt

```

# Chemin relatif et chemin absolu
## Chemin absolu
Un chemin absolu débute à la racine du système de fichiers `/` et se poursuit jusqu'au répertoire concerné. 


Prenons exemple sur le schéma suivant qui illustre (en partie) le système de fichiers de votre machine virtuelle.


<!-- draw.io diagram -->
<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;dark-mode&quot;:&quot;auto&quot;,&quot;toolbar&quot;:&quot;zoom layers tags lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;xml&quot;:&quot;&lt;mxfile host=\&quot;embed.diagrams.net\&quot; agent=\&quot;Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36\&quot; version=\&quot;27.1.1\&quot;&gt;\n  &lt;diagram id=\&quot;lCzB8BwhDeQ8FYM-uekf\&quot; name=\&quot;Page-1\&quot;&gt;\n    &lt;mxGraphModel dx=\&quot;2066\&quot; dy=\&quot;1141\&quot; grid=\&quot;1\&quot; gridSize=\&quot;10\&quot; guides=\&quot;1\&quot; tooltips=\&quot;1\&quot; connect=\&quot;1\&quot; arrows=\&quot;1\&quot; fold=\&quot;1\&quot; page=\&quot;1\&quot; pageScale=\&quot;1\&quot; pageWidth=\&quot;827\&quot; pageHeight=\&quot;1169\&quot; math=\&quot;0\&quot; shadow=\&quot;0\&quot;&gt;\n      &lt;root&gt;\n        &lt;mxCell id=\&quot;0\&quot; /&gt;\n        &lt;mxCell id=\&quot;1\&quot; parent=\&quot;0\&quot; /&gt;\n        &lt;mxCell id=\&quot;11\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.672;entryY=0;entryDx=0;entryDy=0;entryPerimeter=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;10\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;12\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;3\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;13\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;8\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;14\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot;&gt;\n            &lt;mxPoint x=\&quot;370\&quot; y=\&quot;90\&quot; as=\&quot;targetPoint\&quot; /&gt;\n          &lt;/mxGeometry&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;15\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;9\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;35\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;34\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;2\&quot; value=\&quot;/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;fillColor=#f8cecc;strokeColor=#b85450;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;350\&quot; y=\&quot;40\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;3\&quot; value=\&quot;etc/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;298.25\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;17\&quot; value=\&quot;\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;8\&quot; target=\&quot;16\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;8\&quot; value=\&quot;home/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;398.25\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;36\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;9\&quot; target=\&quot;33\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;9\&quot; value=\&quot;var/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;490\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;10\&quot; value=\&quot;dev/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;198.25\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;29\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;16\&quot; target=\&quot;20\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;31\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;16\&quot; target=\&quot;18\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;32\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;16\&quot; target=\&quot;19\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;16\&quot; value=\&quot;etudiant/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;398.25\&quot; y=\&quot;170\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;37\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;18\&quot; target=\&quot;22\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;38\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;18\&quot; target=\&quot;23\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;18\&quot; value=\&quot;code/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;290\&quot; y=\&quot;240\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;39\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;19\&quot; target=\&quot;25\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;40\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;19\&quot; target=\&quot;26\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;41\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;19\&quot; target=\&quot;27\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;19\&quot; value=\&quot;documents/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;510\&quot; y=\&quot;240\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;20\&quot; value=\&quot;téléchargements/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;386.25\&quot; y=\&quot;240\&quot; width=\&quot;104\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;22\&quot; value=\&quot;bash/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;240\&quot; y=\&quot;310\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;23\&quot; value=\&quot;python/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;332\&quot; y=\&quot;310\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;25\&quot; value=\&quot;bilan 2023/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;476\&quot; y=\&quot;310\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;26\&quot; value=\&quot;dictionnaires/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;566\&quot; y=\&quot;310\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;27\&quot; value=\&quot;ressources humaines /\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;656\&quot; y=\&quot;310\&quot; width=\&quot;134\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;33\&quot; value=\&quot;log/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;520\&quot; y=\&quot;170\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;34\&quot; value=\&quot;root/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;580\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;42\&quot; value=\&quot;La racine\&quot; style=\&quot;text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;360\&quot; y=\&quot;20\&quot; width=\&quot;60\&quot; height=\&quot;20\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n      &lt;/root&gt;\n    &lt;/mxGraphModel&gt;\n  &lt;/diagram&gt;\n&lt;/mxfile&gt;\n&quot;}"></div>
<script type="text/javascript" src="https://viewer.diagrams.net/js/viewer-static.min.js"></script>





Le chemin absolu du répertoire `archives` est `/home/etudiant/projet_final/archives`.


           
Le chemin absolu est très utile car il permet d'accéder à un répertoire donné à partir de n'importe où dans le système de fichiers. Certaines commandes exigent le chemin absolu pour s'exécuter correctement.

L'inconvénient du chemin absolu est qu'il est parfois long à saisir pour les dossiers qui sont loin sous la racine.

> Le chemin absolu commence **toujours** par une barre oblique `/`.

## Chemin relatif
Le chemin relatif est un chemin qui **débute à partir du répertoire courant**.
Les chemins relatifs sont plus courts mais n'ont de sens que par rapport à votre position actuelle dans l’arborescence. 

Prenons pour exemple le même diagramme du système de fichiers que dans la section précédente. Spécifions de surcroit le répertoire courant de notre shell qui est `/home/etudiant`.

<!-- draw.io diagram -->
<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;dark-mode&quot;:&quot;auto&quot;,&quot;toolbar&quot;:&quot;zoom layers tags lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;xml&quot;:&quot;&lt;mxfile host=\&quot;embed.diagrams.net\&quot; agent=\&quot;Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36\&quot; version=\&quot;27.1.1\&quot;&gt;\n  &lt;diagram id=\&quot;lCzB8BwhDeQ8FYM-uekf\&quot; name=\&quot;Page-1\&quot;&gt;\n    &lt;mxGraphModel dx=\&quot;2066\&quot; dy=\&quot;1141\&quot; grid=\&quot;1\&quot; gridSize=\&quot;10\&quot; guides=\&quot;1\&quot; tooltips=\&quot;1\&quot; connect=\&quot;1\&quot; arrows=\&quot;1\&quot; fold=\&quot;1\&quot; page=\&quot;1\&quot; pageScale=\&quot;1\&quot; pageWidth=\&quot;827\&quot; pageHeight=\&quot;1169\&quot; math=\&quot;0\&quot; shadow=\&quot;0\&quot;&gt;\n      &lt;root&gt;\n        &lt;mxCell id=\&quot;0\&quot; /&gt;\n        &lt;mxCell id=\&quot;1\&quot; parent=\&quot;0\&quot; /&gt;\n        &lt;mxCell id=\&quot;11\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.672;entryY=0;entryDx=0;entryDy=0;entryPerimeter=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;10\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;12\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;3\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;13\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;8\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;14\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot;&gt;\n            &lt;mxPoint x=\&quot;370\&quot; y=\&quot;90\&quot; as=\&quot;targetPoint\&quot; /&gt;\n          &lt;/mxGeometry&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;15\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;9\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;35\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;2\&quot; target=\&quot;34\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;2\&quot; value=\&quot;/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;350\&quot; y=\&quot;40\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;3\&quot; value=\&quot;etc/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;298.25\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;17\&quot; value=\&quot;\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;8\&quot; target=\&quot;16\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;8\&quot; value=\&quot;home/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;398.25\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;36\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;9\&quot; target=\&quot;33\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;9\&quot; value=\&quot;var/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;490\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;10\&quot; value=\&quot;dev/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;198.25\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;29\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;16\&quot; target=\&quot;20\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;31\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;16\&quot; target=\&quot;18\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;32\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;16\&quot; target=\&quot;19\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;16\&quot; value=\&quot;etudiant/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;fillColor=#f8cecc;strokeColor=#b85450;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;398.25\&quot; y=\&quot;170\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;37\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;18\&quot; target=\&quot;22\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;38\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;18\&quot; target=\&quot;23\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;18\&quot; value=\&quot;code/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;290\&quot; y=\&quot;240\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;39\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;19\&quot; target=\&quot;25\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;40\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;19\&quot; target=\&quot;26\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;41\&quot; style=\&quot;edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;endArrow=none;endFill=0;\&quot; parent=\&quot;1\&quot; source=\&quot;19\&quot; target=\&quot;27\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry relative=\&quot;1\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;19\&quot; value=\&quot;documents/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;510\&quot; y=\&quot;240\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;20\&quot; value=\&quot;téléchargements/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;386.25\&quot; y=\&quot;240\&quot; width=\&quot;104\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;22\&quot; value=\&quot;bash/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;240\&quot; y=\&quot;310\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;23\&quot; value=\&quot;python/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;332\&quot; y=\&quot;310\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;25\&quot; value=\&quot;bilan 2023/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;476\&quot; y=\&quot;310\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;26\&quot; value=\&quot;dictionnaires/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;566\&quot; y=\&quot;310\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;27\&quot; value=\&quot;ressources humaines /\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;656\&quot; y=\&quot;310\&quot; width=\&quot;134\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;33\&quot; value=\&quot;log/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;520\&quot; y=\&quot;170\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;34\&quot; value=\&quot;root/\&quot; style=\&quot;rounded=0;whiteSpace=wrap;html=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;580\&quot; y=\&quot;110\&quot; width=\&quot;80\&quot; height=\&quot;30\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;42\&quot; value=\&quot;La racine\&quot; style=\&quot;text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;360\&quot; y=\&quot;20\&quot; width=\&quot;60\&quot; height=\&quot;20\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;43\&quot; value=\&quot;&amp;lt;b&amp;gt;Répertoire courant&amp;lt;/b&amp;gt;\&quot; style=\&quot;text;html=1;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;whiteSpace=wrap;rounded=0;\&quot; parent=\&quot;1\&quot; vertex=\&quot;1\&quot;&gt;\n          &lt;mxGeometry x=\&quot;225\&quot; y=\&quot;175\&quot; width=\&quot;125\&quot; height=\&quot;20\&quot; as=\&quot;geometry\&quot; /&gt;\n        &lt;/mxCell&gt;\n        &lt;mxCell id=\&quot;44\&quot; value=\&quot;\&quot; style=\&quot;endArrow=classic;html=1;\&quot; parent=\&quot;1\&quot; edge=\&quot;1\&quot;&gt;\n          &lt;mxGeometry width=\&quot;50\&quot; height=\&quot;50\&quot; relative=\&quot;1\&quot; as=\&quot;geometry\&quot;&gt;\n            &lt;mxPoint x=\&quot;350\&quot; y=\&quot;184.5\&quot; as=\&quot;sourcePoint\&quot; /&gt;\n            &lt;mxPoint x=\&quot;380\&quot; y=\&quot;184.5\&quot; as=\&quot;targetPoint\&quot; /&gt;\n          &lt;/mxGeometry&gt;\n        &lt;/mxCell&gt;\n      &lt;/root&gt;\n    &lt;/mxGraphModel&gt;\n  &lt;/diagram&gt;\n&lt;/mxfile&gt;\n&quot;}"></div>
<script type="text/javascript" src="https://viewer.diagrams.net/js/viewer-static.min.js"></script>




Le chemin relatif qui nous permet de faire référence au répertoire `resultats` s'écrit comme suit: `exercices/resultats`

Par exemple:
```
etudiant@vm-al1:~$ pwd
/home/etudiant
```
```
etudiant@vm-al1:~$ cd exercices/resultats/
etudiant@vm-al1:~/exercices/resultats$ 

```
La commande `pwd` retourne toujours le chemin absolu du répertoire courant.
```
etudiant@vm-al1:~/exercices/resultats$ pwd
/home/etudiant/exercices/resultats
```
> Le chemin relatif ne commence **jamais** par une barre oblique `/`.

## Chemins relatifs spéciaux
Le shell Linux permet d'utiliser des chemins relatifs spéciaux pour raccourcir davantage les chemins lors de la navigation. 

Pour révéler les premiers chemins spéciaux, saisissez la commande `ls` avec l’option `-a`. Cet indicateur modifie la commande `ls` afin que tous les fichiers et répertoires soient affichés, y compris les fichiers et répertoires cachés.

> Remarque: Vous pouvez vous référer à la page de manuel afin de comprendre la relation entre la commande `ls` et l’option `-a`.

Dans l'exemple suivant, le répertoire courant du shell est `/home/etudiant/projet_final`.
```
etudiant@vm-al1:~/projet_final$ ls -a
 .   ..  archives
```

Cette commande a révélé deux répertoires supplémentaires : 
- Le répertoire `.` désigne le répertoire courant (dans cet exemple, documents)
- Le répertoire `..` Indique le répertoire parent (dans cet exemple, etudiant)

### Le répertoire `.`
Nous verrons plus tard dans ce cours en quoi le répertoire `.` peut être utile. Pour l'instant, sachez simplement qu'il représente le répertoire courant. Par exemple, la commande `ls` sans argument liste le contenu du répertoire courant. 
Lançons ici la commande `ls .`; on remarque qu'elle retourne le même résultat que `ls` sans arguments.
```
etudiant@vm-al1:~/projet_final$ ls .
archives
```
```
etudiant@vm-al1:~/documents$ ls 
archives
```
### Le répertoire `..`
Le répertoire parent du répertoire courant est toujours désigné par `..`. Ce chemin relatif est très utilisé pour "remonter d'un niveau" dans le système de fichiers
```
etudiant@vm-al1:~/projet_final$ pwd
/home/etudiant/projet_final
```
```
etudiant@vm-al1:~/projet_final$ cd ..
etudiant@vm-al1:~$ 
```
```
etudiant@vm-al1:~$ pwd
/home/etudiant

```
La commande `cd ..` est beaucoup plus conviviale à utiliser au lieu du chemin absolu du répertoire parent. De plus, on peut l'utiliser plusieurs fois dans une même commande pour remonter de plusieurs niveau dans l'arborescence des fichiers.

```
etudiant@vm-al1:~/projet_final$ pwd
/home/etudiant/projet_final
```
```
etudiant@vm-al1:~/projet_final$ cd ../../
```
```
etudiant@vm-al1:/home$ pwd
/home

```

### Le répertoire `~`
Le répertoire `~` représente le répertoire personnel de l'utilisateur courant. Sous Linux, tous les utilisateurs autorisés à se connecter possèdent un répertoire personnel. Les répertoires personnels des utilisateurs sont situés dans `/home`.
Par exemple, le répertoire personnel de l'utilisateur **bob** sera `/home/bob` et le répertoire personnel de l'utilisateur **etudiant** sera `/home/etudiant`. La seule exception à cette règle est le répertoire du super-utilisateur **root** qui utilise `/root` comme répertoire personnel.

Dans l'exemple suivant, l'utilisateur `etudiant` a pour répertoire courant `/etc`. Il souhaite se positionner dans le répertoire `documents` dans son répertoire personnel. Le caractère `~` est substitué par le répertoire personnel de l'utilisateur dans cet exemple.

```
etudiant@vm-al1:/etc$ pwd
/etc
```
```
etudiant@vm-al1:/etc$ cd ~/projet_final/
```

```
etudiant@vm-al1:~/projet_final$ pwd
/home/etudiant/projet_final


```
# Manipuler les fichiers et les répertoires

## Lister le contenu d'un répertoire
La commande `ls` est sans doute l'une des commandes les plus utilisées du shell. Lancée sans option et sans argument, elle permet de lister les fichiers et les dossiers du répertoire courant.
```
etudiant@vm-al1:~$ ls
cv.txt  exercices  instructions.txt  notes  projet_final
```
Pour lister le contenu d'un autre répertoire que le répertoire courant, on  fournit à la commande `ls` répertoire voulu en argument. Par exemple, pour lister le contenu de la racine, on lance `ls /`.

```
etudiant@vm-al1:~$ ls /
bin   home            lib32       media  root  sys  vmlinuz
boot  initrd.img      lib64       mnt    run   tmp  vmlinuz.old
dev   initrd.img.old  libx32      opt    sbin  usr
etc   lib             lost+found  proc   srv   var

```

Notez que `ls` sans option ne fournit aucune information autre que le nom des fichiers et des répertoires rencontrés. Dans certaines circonstances, il peut être utile de lancer `ls` avec l'option `-l` pour afficher le contenu d'un répertoire au **format long**. Ce format permet d'obtenir plus de détails comme les permissions, le propriétaire, la taille et la date de dernière modification des fichiers et dossiers.
```
etudiant@vm-al1:~$ ls -l
total 20
-rw-r--r-- 1 etudiant etudiant   17  4 jun 10:55 cv.txt
drwxr-xr-x 5 etudiant etudiant 4096  4 jun 10:55 exercices
-rw-r--r-- 1 etudiant etudiant  141  4 jun 10:55 instructions.txt
drwxr-xr-x 4 etudiant etudiant 4096  4 jun 10:55 notes
drwxr-xr-x 3 etudiant etudiant 4096  4 jun 10:55 projet_final


```

Les notions portant sur les permissions et le propriétaire seront vues ultérieurement dans le cours.

### Fichiers et répertoires cachés

Sous Linux, les fichiers et les répertoires cachés ont un nom débutant par un point `.`. Par défaut, les fichiers et répertoires cachés ne sont pas affichés dans le résultat de `ls`. Pour les afficher, il faut utiliser l'option `-a` de la commande `ls`.
```
etudiant@vm-al1:~$ ls -a
.              .bash_logout  .config    .face             .lesshst  .profile
..             .bashrc       cv.txt     .face.icon        .local    projet_final
.bash_history  .cache        exercices  instructions.txt  notes     .sudo_as_admin_successful


```
Dans cet exemple, nous pouvons remarquer que le répertoire personnel d'un utilisateur contient quelques fichiers et dossiers cachés. Par exemple, le fichier `.bash_history `contient l'historique des commandes lancées par l'utilisateur.
## Créer des répertoires
La commande `mkdir` permet de créer de nouveaux répertoires. Elle s'utilise avec la syntaxe suivante.
```
mkdir [nouveau-répertoire]
```

Par exemple, on peut créer un nouveau répertoire nommé `420-AL1` dans notre répertoire personnel.
```
etudiant@vm-al1:~$ mkdir 420-AL1
```
```
etudiant@vm-al1:~$ ls
420-AL1  cv.txt  exercices  instructions.txt  notes  projet_final

```
La commande `mkdir`, sans option, ne permet pas de créer de sous-répertoires dans un répertoire qui n'existe pas.

Par exemple, tentons de créer les sous répertoires `420-AL1/semaine-1/exercices`
```
etudiant@vm-al1:~$ cd 420-AL1/
```
```
etudiant@vm-al1:~/420-AL1$ mkdir semaine-1/exercices
mkdir: impossible de créer le répertoire « semaine-1/exercices »: Aucun fichier ou dossier de ce type

```
Il est possible d'utiliser `mkdir` avec l'option `-p` (ou `--parents` en version longue) pour créer à la fois le répertoire `semaine-1` et le sous-répertoire `exercices`.

```
etudiant@vm-al1:~/420-AL1$ mkdir -p semaine-1/exercices

```
On peut ensuite valider le résultat par `ls` ou bien `tree`.
```
etudiant@vm-al1:~/420-AL1$ tree
.
└── semaine-1
    └── exercices

```



## Créer un fichier vide

En règle générale, les fichiers sont créés automatiquement par les programmes. Lors de l'installation d'une application, le gestionnaire de paquets crée automatiquement tous les fichiers nécessaires à l'application pour s'exécuter.

Un utilisateur peut souhaiter créer lui-même un fichier vide à l'aide de la commande `touch`. 

```
etudiant@vm-al1:~/420-AL1$ touch plan-de-cours.txt
```

On peut ensuite valider le résultat par `ls` ou bien `tree`.

```
etudiant@vm-al1:~/420-AL1$ ls
plan-de-cours.txt  semaine-1
```

```
etudiant@vm-al1:~/420-AL1$ tree
.
├── plan-de-cours.txt
└── semaine-1
    └── exercices

2 directories, 1 file

```

Si vous exécutez la commande `touch` sur un fichier existant, le contenu du fichier ne sera pas modifié, mais l'horodatage de modification du fichier sera mis à jour.
Remarquez ici la date et l'heure de la dernière modification: **le 1 juin à 10:46**.


```
etudiant@vm-al1:~$ ls -l
total 24
drwxr-xr-x 2 etudiant etudiant 4096  1 jun 11:13 420-AL1
-rw-r--r-- 1 etudiant etudiant   17  1 jun 10:55 cv.txt
drwxr-xr-x 5 etudiant etudiant 4096  1 jun 10:55 exercices
-rw-r--r-- 1 etudiant etudiant  141  1 jun 10:55 instructions.txt
drwxr-xr-x 4 etudiant etudiant 4096  1 jun 10:55 notes
drwxr-xr-x 3 etudiant etudiant 4096  1 jun 10:55 projet_final

```

```
etudiant@vm-al1:~/documents/dictionnaire$ touch cv.txt 
```
Remarquez maintenant la date et l'heure de la dernière modification: **le 3 février à 13:27**.
```
etudiant@vm-al1:~/documents/dictionnaires$ ls -l
total 24
drwxr-xr-x 2 etudiant etudiant 4096  1 jun 11:13 420-AL1
-rw-r--r-- 1 etudiant etudiant   17  4 jun 10:58 cv.txt
drwxr-xr-x 5 etudiant etudiant 4096  1 jun 10:55 exercices
-rw-r--r-- 1 etudiant etudiant  141  1 jun 10:55 instructions.txt
drwxr-xr-x 4 etudiant etudiant 4096  1 jun 10:55 notes
drwxr-xr-x 3 etudiant etudiant 4096  1 jun 10:55 projet_final
```


## Copier des fichiers et des répertoires
La commande `cp` est utilisée pour copier des fichiers et des répertoires. C'est l'une des commandes les plus utilisées du shell Linux.

La commance `cp` prend au minimum deux arguments.
```
cp [source] [destination]
```

Pour tester la commande `cp`, nous allons d'abord créer un nouveau répertoire nommé copie dans `420-2C2`.
```
etudiant@vm-al1:~$ mkdir 420-2C2/copie
```

### Copier un fichier
Le fichier `/etc/protocols` est un fichier présent sur tous les systèmes Linux. Il contient une association entre des numéros de ports TCP/IP et des noms protocoles réseau.

Pour copier ce fichier dans notre répertoire `copie`, on lance la commande tel qu'illustré ci-bas. Remarquez que, dans cet exemple, le chemin absolu est utilisé pour le fichier `protocols` tandis que le répertoire `copie` est fourni avec son chemin relatif.
```
etudiant@vm-al1:~$ cp /etc/protocols 420-AL1/copie/
```
On peut ensuite valider que la copie a bien fonctionné en listant le contenu du répertoire `copie`.
```
etudiant@vm-al1:~$ ls 420-AL1/copie/
protocols
```

### Copier un répertoire
La commande `cp` sans argument permet de copier des fichiers, mais pas des répertoires. 
Essayons de copier le répertoire `exercices` à l'intérieur de notre répertoire `copie`.
```
etudiant@vm-al1:~$ cp exercices/ 420-AL1/copie/
cp: -r not specified; omitting directory 'documents/'
```
Pour copier un répertoire, il faut utiliser l'option `-r` (ou `--recursive`) avec la commande `cp`.

```
etudiant@vm-al1:~$ cp -r exercices/ 420-AL1/copie/
```
```
etudiant@vm-al1:~$ ls 420-AL1/copie/
documents  protocols
etudiant@vm-al1:~$ 
```
## Déplacer des fichiers et des répertoires
La commande `mv` est utilisée pour déplacer un fichier d'un emplacement dans le système de fichiers vers un autre. Le déplacement d'un fichier en ligne de commandes est analogue au couper / coller dans un shell graphique (GUI). La commande mv s'utilise comme suit:
```
mv [origine] [destination]
```
Si vous déplacez un fichier d'un répertoire à un autre sans spécifier un nouveau nom pour le fichier, il conservera son nom d'origine. Dans l'exemple qui suit, nous allons déplacer le fichier `plan-de-cours.txt` dans notre répertoire personnel. Remarquez que le fichier a conservé son nom.
```
etudiant@vm-al1:~$ cd 420-AL1/

etudiant@vm-al1:~/420-AL1$ mv plan-de-cours.txt ~

```

À l'instar de la commande `cp`, la commande `mv` permet de déplacer plusieurs éléments vers une même destination. Dans cet exemple, nous allons déplacer le fichier `plan-de-cours.txt` et le répertoire `420-AL1` dans le répertoire `/tmp`.

```
etudiant@vm-al1:~$ mv plan-de-cours.txt 420-AL1/ /tmp/
```
```
etudiant@vm-al1:~$ ls
code  documents  téléchargements

etudiant@vm-al1:~$ ls /tmp/
420-AAL1  plan-de-cours.txt  systemd-private-bb50971553ae49b0af180d948920d44e-systemd-resolved.service-XBI30r
```
>Il n'est pas nécessaire d'utiliser l'option `-r` avec la commande `mv`. Lorsqu'un répertoire est déplacé ou renommé, tout son contenu est déplacé.

Déplacer un fichier dans le même répertoire consiste en fait à le renommer. Par exemple, dans la commande suivante, le fichier `cours1.txt` est déplacé du répertoire courant vers le répertoire courant et reçoit le nouveau nom de `cours1-vieux.txt`. En d'autres termes, `cours1.txt` est renommé `cours1-vieux.txt`.

```
etudiant@vm-al1:~/notes/semestre1$ ls
cours1.txt  cours2.txt
etudiant@vm-al1:~/notes/semestre1$ mv cours1.txt cours1-vieux.txt
etudiant@vm-al1:~/notes/semestre1$ ls
cours1-vieux.txt  cours2.txt

```

## Supprimer des fichiers et des répertoires
La commande `rm` permet de supprimer définitivement des fichiers et des dossiers.

> Attention: Sous Linux, il n'y a pas de corbeille. Les fichiers que vous supprimez avec `rm` sont irrécupérables.


Sans argument, la commande `rm` permet de supprimer des fichiers.
Pour réaliser cet exemple, nous allons créer quatre fichiers.
```
etudiant@vm-al1:~$ touch fichier-1 fichier-2 fichier-3 fichier-4
```
```
etudiant@vm-al1:~$ ls
code  documents  fichier-1  fichier-2  fichier-3  fichier-4  téléchargements
```

Nous allons maintenant utiliser la commande `rm` pour les supprimer. Remarquez qu'aucune confirmation n'est demandée à l'utilisateur avant la suppression, il convient donc d'être très prudent.
```
etudiant@vm-al1:~$ rm fichier-1 
etudiant@vm-al1:~$ ls
code  documents  fichier-2  fichier-3  fichier-4  téléchargements
```
L'option `-i` peut être utilisé pour demander une confirmation.
```
etudiant@vm-al1:~$ rm -i fichier-2 
rm: remove regular empty file 'fichier-2'? y

etudiant@vm-al1:~$ ls
code  documents  fichier-3  fichier-4  téléchargements
```

## En bref
![cmds.svg](/static/images/AL1/fichiers_repertoires/cmds.svg)

# Englobement simple
Nous allons maintenant aborder une fonctionnalité très importante du Shell Linux: l'englobement.

L'englobement sous Linux consiste à utiliser des méta-caractères (tels que `*`, `?` ou `[]`) pour faire correspondre une expression à plusieurs fichiers simultanément. Vous connaissez peut-être les termes *joker* ou *wildcard* en anglais qui signifient à peu près la même chose. Sous Linux, on parlera toutefois de *globbing*.

Pour consulter l'aide sur l'englobement, on peut consulter la page de manuel glob, à la section 7 (section divers et conventions).
```
etudiant@vm-al1:~$ man 7 glob
```

Le globbing peut être utilisé n'importe où dans un nom de fichier pour remplacer un ou plusieurs caractères. 

## L'astérisque `*`
L'astérisque `*` peut être utilisé pour correspondre à n'importe quelle chaîne, **incluant la chaîne vide**.

Par exemple, pour lister tous les fichiers du répertoire `/etc` dont le nom se termine par `.conf`, on lancera la commande suivante:
```
etudiant@vm-al1:~$ ls /etc/*.conf
/etc/adduser.conf          /etc/debconf.conf  /etc/gai.conf    /etc/libaudit.conf   /etc/nsswitch.conf  /etc/rsyslog.conf   /etc/ucf.conf
/etc/apg.conf              /etc/deluser.conf  /etc/host.conf   /etc/logrotate.conf  /etc/pam.conf       /etc/sensors3.conf  /etc/usb_modeswitch.conf
/etc/ca-certificates.conf  /etc/fuse.conf     /etc/ld.so.conf  /etc/mke2fs.conf     /etc/resolv.conf    /etc/sysctl.conf
```
Pour lister tous les fichiers du répertoire `/var/log` débutant par `auth`, on lancera la commande suivante.
```
etudiant@vm-al1:~$ ls /var/log/auth*
/var/log/auth.log    /var/log/auth.log.2.gz  /var/log/auth.log.4.gz
/var/log/auth.log.1  /var/log/auth.log.3.gz
```
>Les fichiers auth.log* contiennent l'historique des connexions des utilisateurs sur le système d'exploitation. Dans ce fichier, on peut notamment voir les tentatives d'authentification qui ont échoué. Ce fichier est très utile lorsqu'on enquête sur une tentative de piratage d'un système Linux.


## Le point d'interrogation `?`

Avant d'aller plus loin, nous allons créer un jeu de fichiers pour réaliser des tests.
```
etudiant@vm-al1:~$ mkdir glob 
etudiant@vm-al1:~$ cd glob/

etudiant@vm-al1:~/glob$ touch fichier_{A..D}{1..5}.txt
etudiant@vm-al1:~/glob$ touch fichier_{a..d}{1..5}.txt

etudiant@vm-al1:~/glob$ ls
fichier_A1.txt  fichier_A3.txt  fichier_A5.txt  fichier_B2.txt  fichier_B4.txt  fichier_C1.txt  fichier_C3.txt  fichier_C5.txt  fichier_D2.txt  fichier_D4.txt
fichier_a1.txt  fichier_a3.txt  fichier_a5.txt  fichier_b2.txt  fichier_b4.txt  fichier_c1.txt  fichier_c3.txt  fichier_c5.txt  fichier_d2.txt  fichier_d4.txt
fichier_A2.txt  fichier_A4.txt  fichier_B1.txt  fichier_B3.txt  fichier_B5.txt  fichier_C2.txt  fichier_C4.txt  fichier_D1.txt  fichier_D3.txt  fichier_D5.txt
fichier_a2.txt  fichier_a4.txt  fichier_b1.txt  fichier_b3.txt  fichier_b5.txt  fichier_c2.txt  fichier_c4.txt  fichier_d1.txt  fichier_d3.txt  fichier_d5.txt
```
Le point d'interrogation `?` est utilisé pour **remplacer un seul caractère** dans un nom de fichier. Le caractère remplacé ne peut pas être nul comme c'est le cas avec `*`.

Pour afficher tous les fichiers du jeu précédent qui contiennent le chiffre 5, on lance la commande:
```
etudiant@vm-al1:~/glob$ ls fichier_?5.txt
fichier_A5.txt  fichier_a5.txt  fichier_B5.txt  fichier_b5.txt  fichier_C5.txt  fichier_c5.txt  fichier_D5.txt  fichier_d5.txt
```

On pourrait également utiliser le caractère `?` pour afficher tous les fichiers ayant l'identifiant `a` suivi de n'importe quelle valeur.
```
etudiant@vm-al1:~/glob$ ls fichier_a?.txt
fichier_a1.txt  fichier_a2.txt  fichier_a3.txt  fichier_a4.txt  fichier_a5.txt
```

## Les crochets `[ ]`
Les crochets sont utilisés pour définir un ensemble spécifique de caractères souhaités. Voici quelques exemples d'ensembles de caractères.
- `[agh]`: correspond à une lettre, soit a, g ou h
- `[0-9]` correspond à n'importe quel chiffre entre 0 et 9
- `[a-z]` correspond à n'importe quelle lettre de l'alphabet
- `[[:lower:]]` est équivalent à `[a-z]`
- `[[:upper:]]` correspond à n'importe quelle lettre majuscule

Par exemple, pour afficher tous les fichiers portant un identifiant avec une lettre majuscule suivi de n'importe quel caractère, on pourra lancer la commande suivante:
```
etudiant@vm-al1:~/glob$ ls fichier_[[:upper:]]?.txt
fichier_A1.txt  fichier_A3.txt  fichier_A5.txt  fichier_B2.txt  fichier_B4.txt  fichier_C1.txt  fichier_C3.txt  fichier_C5.txt  fichier_D2.txt  fichier_D4.txt
fichier_A2.txt  fichier_A4.txt  fichier_B1.txt  fichier_B3.txt  fichier_B5.txt  fichier_C2.txt  fichier_C4.txt  fichier_D1.txt  fichier_D3.txt  fichier_D5.txt
```
>Remarquez que deux englobements différents ont été utilisés dans la commande précédente, soit `[[:upper:]]` et `?`


Pour afficher tous les fichiers portant la lettre a minuscule et un chiffre entre 1 et 3, on lance la commande suivante.
```
etudiant@vm-al1:~/glob$ ls fichier_a[1-3].txt
fichier_a1.txt  fichier_a2.txt  fichier_a3.txt
```

## Le point d'exclamation `!`
Le point d'exclamation est utilisé en complément des crochets pour signifier une négation. 
Par exemple, pour afficher tous les fichiers identifiés par la lettre a suivie d'un chiffre qui n'est pas 1, 2 ou 3, on pourra lancer la commande suivante:
```
etudiant@vm-al1:~/glob$ ls fichier_a[!1-3].txt
fichier_a4.txt  fichier_a5.txt
```
## Quelques exemples d'englobement

| Expression| Signification| Exemple |
|-----------|--------------|---------|
| * | Correspond à tous les fichiers et répertoires | **bob.txt** et **1-fichier.doc**  
| a* |Correspond à tous les fichiers et répertoires dont le nom commence par un a | **animal** et **anna.pdf**
| f*.txt | Correspond à tous les fichiers et répertoires dont le nom commence par f et se termine par .txt |  **fichier.txt** et **freddy.txt**
| p?? | Correspond à tous les fichiers et répertoires dont le nom commence par p suivi d'exactement deux caractères | **pas** et **pi4**
| f\[a-z]\[0-9]| Correspond à tout fichier dont le nom commence par f, suivi d'une lettre , suivie d'un chiffre |  **fa0** et **fb8**
|g\[!a-z]| Correspond à tout fichier dont le nom commence par g, suivi d'un caractère qui n'est pas une lettre | **g_** et **g6**
|\[egp\]\*.txt|  Correspond à tout fichier dont le nom commence par e,g ou p, suivi de n'importe quels caractères et de .txt | **eat.txt**,  **gat.txt**, **excellent.txt** et **pat.txt**


