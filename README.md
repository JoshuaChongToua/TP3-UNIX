# TP3-UNIX

## Exercice Paramètres

Dans le fichier analyse.sh : 

```bash
#!/bin/bash

echo "Bonjour, vous avez rentré $# paramètres."

echo "Le nom du script est $(basename "$0")"

echo "Le troisième paramètre est $3"

echo "Voici la liste des paramètres : $@"

```

Rendu : 

```bash
root@serveur-correction:~/exoBash# ./analyse.sh  joshua LP Jussieu
Bonjour, vous avez rentré 3 paramètres.
Le nom du script est analyse.sh
Le troisième paramètre est Jussieu
Voici la liste des paramètres : joshua LP Jussieu
```

## Exercice Nombre de paramètres

Dans le fichier concat.sh : 

```bash
#!/bin/bash

if [ $# -eq 2 ] ; then
        echo "$1$2"


else
        echo "Erreur, veuillez rentrer seulement 2 parametres"
        exit 1
fi
```

## Argument type et droits


```bash
#!/bin/bash

if [ $# -ne 1 ]; then
        echo "Erreur : vous devez fournir seulement un chemin de fichier en p>
        exit 1
fi

fichier="$1"

if [ ! -e "$fichier" ] ; then
        echo "$fichier n'existe pas"
        exit 1
fi

if [ -d "$fichier" ]; then #verifie si le $fichier est un dossier
        echo "Le fichier $fichier est un répertoire."
elif [ -f "$fichier" ]; then #verifie si le $fichier est un fichier
        if [ -s "$fichier" ]; then #verifie si le fichier est vide
                echo "Le fichier $fichier n'est pas vide"
        else
                echo "Le fichier $fichier est vide"
        fi
else
        echo "Le fichier $fichier est d'un autre type"
fi

util=$(whoami) #on récupère l'utilisateur

droit=""

#on vérifie les droits sur le fichier pour l'utilisateur

droit=""

if [ -r "$1" ]; then
        droit+="lecture "
fi

if [ -w "$1" ]; then
        droit+="ecriture "
fi

if [ -x "$1" ]; then
        droit+="execution"
fi

if [ -n  "$droit" ]; then #verifie si $droit contient au  moisn un caractère donc un droit
        printf "$fichier est accessible par $util en $droit"
else
        printf "$fichier n'est pas accessible par $util"
fi

```

## Afficher le contenu d'un répertoire

```bash
#!/bin/bash

if [ $# -ne 1 ]; then
        echo "Saisir un seul argument" 
        exit 1
fi

if [ ! -d "$1" ]; then #vérifie que l'argument est un dossier valide
        echo "Le chemin donné n'est pas un répertoire"
        exit 1
fi

echo "Repertoire $1 :"

echo "###### fichiers dans $1" 
find "$1" -type f
#liste les fichiers qui sont dans le répertoire argument

echo "###### répertoires dans $1"
ls -d  "$1"/*/ | xargs -n 1 
#liste les sous-repertoires dans le répertoire argument
#xargs -n 1 pour afficher chaque répertoire sur une nouvelle ligne
```

## Lister les utilisateurs
```bash
#!/bin/bash

awk -F: '$3 > 100 {print $1}' /etc/passwd
#-F: pour spécifier le délimiteur
#$3 correspond à uid, il va donc filtrer les lignes où uid est supérieur à 100
#print $1 affiche le premier champ qui correspond à au nom de l utilisateur.
```

## Mon utilisateur existe t'il
```bash
#!/bin/bash

if [ $# -ne 2 ]; then
    echo "Veuillez saisir 1 login et 1 uid"
    exit 1
fi

# Utilisation de awk pour vérifier si l'utilisateur existe
util=$(awk -F: -v login="$1" -v uid="$2" '$1 == login && $3 == uid {print $1}' /etc/passwd)

if [ -n "$util" ]; then #si util est pas vide
    echo "$1 avec l'uid $2 existe"
else
    echo "Aucun utilisateur trouvé avec le login $1 et l'uid $2"
fi

```




