Git Script  
====

# Git Repository per Script erstellen

Da ich noch nie ein eigenes Script geschrieben habe habe ich mir als erste Aufgabe vorgenommen ein kleines Scipt zu schreiben dass einige Basic Programierfunktionen beinhalten wie z.b : if Loops, else if, cut , agumente , read .

Das Ziel ist es ein Script zu erstellen das folgende Punkte abdeckt:
 - Ein Scirpt erstellen das Automatisch ein Git Repository erstellt 
 - Falls gewünscht wird eine Template Strucktur erstellt
 - Remote Repostiory erstellen
 - Lokales Repostiory erstellen
 - Global Git Ignore Erstellt ein .gitignore mit Einträgen 
 - Alle Files mit Git Add hinzufügen
 - Einen ersten Commit machen
 - Logfile wird nachgeführt 
 - Script ist ausführlich mit Kommentaren Dokumentiert

## Voraussetzungen 

Um mit SSH pushen zu können, benötigen Sie einen SSH-Schlüssel, der zu Ihrem GitLab-Konto hinzugefügt wurde.
Überprüfe ob Sie die nötigen Berechtigungen haben:
1.	Wählen Sie in der oberen Leiste Hauptmenü > Gruppen und suchen Sie Ihre Gruppe.
2.	Vergewissern Sie sich, dass die Option Neues Projekt in der oberen rechten Ecke angezeigt wird


## Vorgehen 
Es wird das git_remote.sh Script auf einem Lokalen Verzeichniss benötigt.  
Im gleichen Verzeichniss muss sich auch der Folder "folder_structure_template" befinden.  
Man startet Git Bash und startet das Script mit `./git_remote.sh {GitlabBenutzer} {ProjectName}`  
```
./git_remote.sh daniel.ambuehl lazyaws
```  
Es wird ein neuer Ordner mit dem ProjectNamen erstellt im Verzeichnis wo das Script ausgeführt wurde.
Die Dokumentation des Scripts wurde direkt im Script mit Deteilierten Kommentarten gemacht:

## git_remote.sh Script

- Hier das Erstellte Script mit Kommentaren
```

#!/bin/bash

username=$1
repo_name=$2
logoutput=./git.log

#Check if there are two arguments delivered 
if [ $# -ne 2 ]; then
echo "Not the expected number of arguments. Expected number of arguments 2."
echo "1 argument - your username in gitlab"
echo "2 argument - name of the repo you want to create"
exit 1
fi

# Creates the new Project Folder
mkdir $repo_name 
cd $repo_name
echo "Repo Folder erstellt"

# Let you select if you want an template folder structure or not

echo ""
read -r -p "Do you want to add a user to add a Folder Structure? [y/n] " input
case $input in
	[yY] )
		cp -r ../folder_structure_template/* ../$repo_name
		echo "Template Folder has been added";;
	[nN] )
		echo "than, let's start with a clean sheet";;
	* )
		echo "Invalid input...Just y or n ;Try again ";
        exit 1;;
esac

# ask user for password
read -s -p "Enter Password: " password
echo ""

# HTTP request to gitlab with Username Variable and Password
request=`curl --request POST "https://gitlab.com/api/v3/session?login=$username&password=$password"`

# Exit with error when reciving eroor from HTTP request
if [ "$request" = '{"message":"401 Unauthorized"}' ]; then
echo "Username or password incorrect."
exit 1
fi

token=`echo $request | cut -d , -f 28 | cut -d : -f 2 | cut -d '"' -f 2`

# Creating The Remote GitLab repository and redirect error into log file. 2>$1 means that we want redirect stderr to stdout
echo -n "Creating GitLab repository '$repo_name' ..."
curl -H "Content-Type:application/json" https://gitlab.com/api/v3/projects?private_token=$token -d '{"name":"'$repo_name'"}' > $logoutput 2>&1
echo " done."

# Lokales Git Repostiory erstellen
echo -n "Pushing local code to remote ..."
echo ""
git init

# Creates a .gitignore and adds the git_remote.sh,.gitignore and git.log file to it.
echo "git_remote.sh" > .gitignore
echo ".gitignore" >> .gitignore
echo "git.log" >> .gitignore

# Check if a README.md Allready exist and else Creates a README.md and adds the simple links to it. (One File is needed to commit to the Repsitory)
if [ -f "README.md" ];
    then
       echo "README.md allready exists"
    else
        echo "## Topics" > README.md
        echo "" >> README.md
        echo "- [**Basic**](basic/README.md)" >> README.md
fi

# Setting up a global .gitignore file
git config --global core.excudefiles ~/.gitignore_global

# Adds the changes in the working directory to the staging area.
git add . >> $logoutput 2>&1
# Commits the added File to the Head Stage with the "first commit" Massage 
git commit -m "first commit" >> $logoutput 2>&1
# Add the new Remote Repostiory and redirect error into log file.
git remote add origin git@gitlab.com:$username/$repo_name.git >> $logoutput 2>&1
# Push the commit to the Remote Repostiroy and redirect error into log file.
git push -u origin master >> $logoutput 2>&1
echo " done."

# Shows Link to Repo
echo ""
echo "The created repo is available at following link:"
echo "https://gitlab.com/$username/$repo_name"
  
```

## Gitlab Repository wieder löschen 
Das Remote Repository kann wie folgt wieder gelöscht werden:
 - Das zu löschende Projekt öffnen
 - Einstellungen (in der linken Seitenleiste)
 - Allgemein
 - Erweiterte Einstellungen (Klicken Sie auf Erweitern)
 - Projekt entfernen (Unten auf der Seite)
 - Bestätigen (Durch Eingabe des Projektnamens und Drücken der Schaltfläche Bestätigen)

 Um das Repository lokal zu löschen kann mann einfach den Lokalen Ordner im entfernen.

Teilquelle: https://github.com/volym3ad/gitlab-repo-create
---

> [⇧ **Zurück zur Hauptseite**](/README.md)

---
