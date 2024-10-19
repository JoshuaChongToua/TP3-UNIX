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

login="$1"
uid="$2"

# Utilisation de awk pour vérifier si l'utilisateur existe
util=$(awk -F: -v login="$login" -v uid="$uid" '$1 == login && $3 == uid {print $1}' /etc/passwd)

if [ -n "$util" ]; then #si util est pas vide
    echo "$login avec l'uid $uid existe"
    exit 0 #pour retourner un valeur pour la suite des exercices
else
    echo "Aucun utilisateur trouvé avec le login $login et l'uid $uid"
    exit 1 #pour retourner un valeur pour la suite des exercices
fi

```

## Creation Utilisateur

```bash
#!/bin/bash

# Vérifier si l'utilisateur est root
if [[ $EUID -ne 0 ]]; then
    echo "Ce script doit être exécuté en tant que root."
    exit 1
fi

echo "Création d'un nouvel utilisateur."

#Demande de saisir des informations
read -p "Entrez le nom de login : " login
read -p "Entrez l'UID souhaité : " uid


# Vérification si l'utilisateur existe déjà avec le login et l'UID + utilisation du script de l'exercice  "Mon utilisateur existe t'il"
if ./userExist "$login" "$uid"; then
    echo "L'utilisateur $login avec l'UID $uid existe déjà."
    exit 1
else
    echo "Aucun utilisateur trouvé avec le login $login et l'UID $uid."
fi

#Demande de saisir des informations
read -p "Entrez le nom de famille : " nom
read -p "Entrez le prénom : " prenom
read -p "Entrez le GID souhaité : " gid
read -p "Entrez les commentaires (ex: 'Nom Prénom, département') : " commentaires

# Création du répertoire home si nécessaire
home_dir="/home/$login"
if [[ -d "$home_dir" ]]; then
    echo "Le répertoire home $home_dir existe déjà."
    exit 1
else
    mkdir "$home_dir"
    echo "Répertoire home $home_dir créé."
fi

# Création de l'utilisateur
useradd -u "$uid" -g "$gid" -c "$commentaires" -d "$home_dir" -m "$login"
if [[ $? -eq 0 ]]; then
    echo "Utilisateur $login créé avec succès."
else
    echo "Erreur lors de la création de l'utilisateur."
    exit 1
fi

```

## Appreciation
```bash
#!/bin/bash

while true; do
    # Demande à l'utilisateur de saisir une note ou de quitter
    read -p "Veuillez saisir une note (ou 'q' pour quitter) : " input

    # quitter si l'utilisateur saisit q
    if [[ "$input" == "q" ]]; then
        echo "Au revoir !"
        exit 0
    fi

    # Vérifie si l'entrée est un nombre valide
    if ! [[ "$input" =~ ^[0-9]+([.][0-9]+)?$ ]]; then
        echo "Erreur : Veuillez entrer un nombre valide."
        continue
    fi

    # Convertit l'entrée en nombre entier (arrondi à l'entier supérieur pour simplifier les comparaisons)
    note=$(printf "%.0f" "$input")

    # Évaluation de la note
    if [ "$note" -ge 16 ]; then #si la note saisie est supérieur ou égale à 16
        echo "Très bien"
    elif [ "$note" -ge 14 ]; then  #si la note saisie est supérieur ou égale à 14
        echo "Bien"
    elif [ "$note" -ge 12 ]; then  #si la note saisie est supérieur ou égale à 12
        echo "Assez bien"
    elif [ "$note" -ge 10 ]; then  #si la note saisie est supérieur ou égale à 10
        echo "Moyen"
    else
        echo "Insuffisant"
    fi
done
```






