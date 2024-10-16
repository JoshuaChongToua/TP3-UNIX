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

##


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

if [ -d "$fichier"]; then
        echo "Le fichier $fichier est un répertoire."
elif [ -f "$fichier" ]; then
        if [ -s "$fichier" ]; then
                echo "Le fichier $fichier n'est pas vide"
        else
                echo "Le fichier $fichier est vide"
        fi
else
        echo "Le fichier $fichier est d'un autre type"
fi

util=$(whoami)

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

if [ -d "$fichier"]; then
        echo "Le fichier $fichier est un répertoire."
elif [ -f "$fichier" ]; then
        if [ -s "$fichier" ]; then
                echo "Le fichier $fichier n'est pas vide"
        else
                echo "Le fichier $fichier est vide"
        fi
else
        echo "Le fichier $fichier est d'un autre type"
fi

util=$(whoami)

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



