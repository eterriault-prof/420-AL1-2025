# Exercices sur Nano

## 1. Créer un nouveau fichier
- Ouvrez nano (tout en créant un fichier) en utilisant la commande suivante: 
```
nano pratique_nano.txt
```
- Ajoutez le texte suivant dans le fichier:
```console
Nano est un éditeur de texte simple mais puissant.
```
- Sauvegardez et fermez le fichier avec `Ctrl + X` (pour quitter), puis `O` (pour confirmer l'enregistrement) puis `Enter` (pour valider le nom du fichier).

## 2. Modifier un fichier existant
- Re-ouvrez votre fichier avec `nano pratique_nano.txt`.
- Ajoutez une deuxième ligne:
```console
Il permet d’éditer rapidement des fichiers en ligne de commande.
```
- Sauvegardez le fichier sans quitter avec `Ctrl + O` et puis `Enter`.

## 3. Copier/couper/coller du texte
- Utilisez les touches fléchées pour naviguer dans le fichier.
- Déplacez vous au début de la ligne du fichier avec `Ctrl + A`. 
- Coupez la première ligne avec `Ctrl + K`
- Déplacez vous à la fin avec `Ctrl + E`.
- Collez la ligne copié avec `Ctrl + U` (Note: Pour coller du texte copié depuis un autre fichier, vous pouvez utiliser `Ctrl + Shift + V`).
- Annulez les derniers changements avec `Alt + U`

## 4. Rechercher et remplacer du texte
- Recherchez le mot `Nano` en appuyant sur `Ctrl + W`, puis tappez `Nano` puis `Enter`
- Remplacez le mot `Nano` par `Vim` avec `Ctrl + \`.
- Remettre `Nano` en naviguant manuellement dans le fichier.

## 5. Sauvegarder et quitter
- Enregistrez et fermez le fichier
- Vérifiez votre fichier avec cat
```console
cat pratique_nano.txt
```
