# Over The Wire
## https://github.com/tendryAxel/Over-The-Wire.git

--- 

Dans la suite d'exrecice suivante, le but est d'entrer dans plusieurs utilisateurs d'un ordinateur

Donc a chanque debut de niveau, on fait la commande :
```sh
  ssh bandit[n-1]@bandit.labs.overthewire.org -p2220
```
*où **n** est le niveau dans lequel on se trouve

Puis on entre le **mot de passe** obtenu dans le niveau precedent

---

## Level0:

Comme dit precedement:
```sh
ssh bandit0@bandit.labs.overthewire.org -p2220
```

Avec le mot de passe à entrer : **bandit0**

---

## Level1:

---

  Faire la commande :
  ```sh
  cat readme
  ```

  Ce qui donne le mot de passe suivant
  **NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL**

---

## Level2:
  Faire la commande :

  ```sh
  cat ./-
  ```

  Pour eviter les problemes avec le nom de fichier **-** , utiliser **./-** 

  On obtient le resultat
  **rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi**

---

## Level3:
  Faire la commande :

  ```sh
cat "sapces in this filename"
  ```

  Pour eviter les problemes avec l'**espace**, utiliser "" 

  On obtient le resultat
  **aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG**

---

## Level4:
  Dans ce niveau quand on liste les fichier disponible, on **n'obtient rien**

  Pour voir tous les fichier réellement disponible
  ```sh
  ls -a
  ```
  Dans les resultats, on a : **.hidden**

  ```sh
  cat inhere/.hidden
  ```
  On a le resultat : **2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe**

---

## Level5:
  Dans ce niveau, le fichier est un ficher de type **ASCII text**
  Il faut faire la commande suivante pour lister les fichiers et leurs tpye
  ```sh
  for i in $(ls inhere/); do file "inhere/$i"; done
  ```
  La commande **file** dans la boucle permet de montrer le type du fichier
  Ici, on a en particulier
  ```
  inhere/-file07: ASCII text
  ```
  De ce fait, par la commande **cat** comme au ci-dessus
  Et on a : **lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR**

---

## Level6:
  Dans ce niveau, il  y a le fichier se trouve quelque part et il faut le chercher

  Pour le trouver, on a une ensemble de caractères auxquelles on le reconnait (comme ça taille, ...) 

  ```sh
  find . -type f -size 1033c ! -executable -exec file {} + | grep ASCII
  ```

  Cette commande cherche les fichiers qui correspondents à la description dans le sujet

  Ici c'est **./inhere/maybehere07/.file2**
  Il contient le mot de passe : **P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU**

---

## Level7:
  Ce niveau est un peu similaire au precedant

  Mais cette fois ci, il a un group et un utilisateur definit
  ```sh
  find / 33c -user bandit7 -group bandit6 -perm /u+rwx,g+rwx
  ```
  Cette commande permet d'avoir les fichier :
  - dont l'**user** est bandit7
  - le **group** est bandit6
  
  Le mot de passe contenue dans le fichier trouvé est:
  **z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S**

---

## Level8:
  Dans ce niveau, le mot de passe est dans le fichier data.txt, après le mot "**millionth**"

  ```sh
  cat data.txt | grep millionth
  ```
  Cette commande permet de trouver la ligne ou se trouve le mot de passe, qui est
  **TESKZC0XvTetK0S9xNwm25STk5iWrBvP**

---

## Level9:
  Dans ce niveau, le mot de passe est une ligne unique dans **data.txt**
  ```sh
  sort data.txt | uniq -c | sort -nr | tail -n 1 | cut -c 9-
  ```
  Dans cette commande, on :
  - compte le nombre de répétition de chaque ligne
  - on trie le tout en ordre décroissant
  - en prend le plus petit des résultats (dans notre cas c'est la ligne unique que l'on cherche)
  - on decoupe un peu le resultat obtenu avec **cut -c 9-** pour n'obtenir que le mot de passe, qui est **EN632PlfYiZbn3PhVK3XOGSlNInNE00t**

---

## Level10:
  Dans ce niveau, il est une chaine de caractère lisible par les hommes, dans la même ligne que des **==**

  D'où, la commande :
  ```sh
strings data.txt | grep ==
  ```
  Qui donne :
  ```
4========== the#
========== password
========== is
========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
  ```

---

## Level11:
 Dans ce niveau, il suffit de convertir en text pour obtenir une phrase correcte qui contient le mot de passe :
  ```sh
  cat data.txt  | base64 -d
  ```
  Qui donne :
  ```
  The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
  ```

---

## Level12:
  Dans ce niveau, il faut faire trouner les caractères de 13 dans leur position dans l'alphabet

  C'est à dire que **a->m** devient **n->z** et **n->z** devient **a->m**
  
  ```sh
cat data.txt | tr 'A-Z' 'N-ZA-M' | tr 'a-z' 'n-za-m'
  ```
  ```
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
  ```

---

## Level13:
  Dans ce niveau, le but est de décompréser plusieurs fois un fichier pour obtenir un fichier de type text lisible

  J'ai créer un dossier nommé **axel** dans **/tmp/**
  ```sh
  mkdir /tmp/axel
  ```

  J'ai ensuite déplacer le fichier dans ce dossier :
  ```sh
  cp data.txt /tmp/axel
  cd /tmp/axel
  ```

  ensuite à chaque fois, il faut verifier le type de fichier :
  ```sh
  file [nom du dernier fichier  utilliser]
  ```
  - si le fichier est gzip, il faut faire :
```sh
gzip -d [nom du dernier fichier utilliser]
```
  - si le fichier est tar, il faut faire :
```sh
tar -xf [nom du dernier fichier utilliser]
```
  - si le fichier est bzip2, il faut faire :
```sh
bzip2 -d [nom du dernier fichier utilliser]
```

Et finalement, quand on arrive àun fichier de type **text**.
On a :

The password is **wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw**