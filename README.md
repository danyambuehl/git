GIT
====

### Workflows
***

![GitLab](/images/git.png)**Externes Repository zu lokalen Ordner Klonen**
```  
  $ git clone <repo> <directory>
  $ git clone git@gitlab.com:daniel.ambuehl/lokalubuntu.git lokalubuntu
```

![GitLab](/images/gitlab.png) **Gitlab Repository mit Script erstellen**

Mit git_remote.sh Script in GitBash ausführen: [**Dokumentation Script**](git_script.md)
 ```  
  $ ./git_remote.sh <GitlabBenutzer> <ProjectName>
  $ ./git_remote.sh daniel.ambuehl lazyaws
 ```  
 ![GitHub](/images/github.png) **Manuel neues Git Repository erstellen**
1. Neues Git Repository in Web erstellen
2. Lokalen Ordner für Gitlab erstellen 
3. Mit Gitbash folgendes ausführen 
```
 $ echo "# new" >> README.md
 $ git init
 $ git add .
 $ git commit -m "first commit"
 $ git remote add origin https://github.com/danyambuehl/[RepositoryName]
 $ git push -u origin master
```

![GitLab](/images/error.png)**Troubleshooting**
```  
  $ git remote -v
  $ git remote remove origin
  $ git remote add origin https://github.com/danyambuehl/[RepositoryName]
```
### Inhaltsverzeichnis

* 01 - [Basic commands](#01-Basic-commands)
* 02 - [Commits](#02-Commits)
* 03 - [Status](03-Status)
* 04 - [Branch](#04-Branch)
* 05 - [Merge ](#05-Merge)
* 06 - [Conflicts](#06-Conflicts)
* 07 - [Restore](#07-Restore)
* 08 - [Comparing](#08-Comparing)
* 09 - [Exclude ](#09-Exclude)
* 10 - [Alias ](#10-Alias )

## 01 Basic commands

 ![Gituebersicht](/images/git_flow.png)  

  **Git Help anzeigen** 
  
  > `$ git --help`

  **Git Befehl um deine e-mailadresse auf dem aktuell verwendeten user zu definieren**
  
  > `$ git config --global user.email "email@example.com"`  

  **Git Repository initialisieren**
  
  > `$ git init`

  **Lokales Projekt mit externem Repository verknüpfen**
  
  > `$ git remote add origin https://gitlab.com/dr/projekt.git`

  **Alle Files im lokale Ordner _Workspace_ dem _Staging_ Bereich hinzufügen**
  
  > `$ git add .`

  **von _Head_ Bereich auf externes Git-Repository hochladen**
  
  > `$ git push`

  **Von externes Git-Repository auf das lokale Repository _Workspace_ syncronisiern**
  
  > `$ git pull` 

  **Von externes Git-Repository direkt auf den _Header_ syncronisiern**
  
  > `$ git fetch` 

  **Git Config anschauen**
  
  > `$ git config --list`  

## 02 Commits 


**Commits the Staged Area to the Head**

   `$ git commit -m "commit commend here"`

**Commits all tracked files directly to the Head**
  
   `$ git commit -am "commit commend here"`

**Commits all tracked files directly to the Head but while edditing the last commit**
  
   `$ git commit --amend`


## 03 Status 

**Aktuellen Stand**
  
  > `$ git status`

**Displays complete commit history** 

   > `$ git log`

**Displays last 3 commits**

   > `$ git log - 3`

**Displays all Commits but just a single line for each commit**

   > `$ git log --oneline`

**Displays all Commits of a specific File** 

   > `$ git log --follow git_auftrag_2/README.md`


## 04 Branch 

**Liste alle Branches in deinem Repository**
  
  > `$ git branch`

**Erstelen eines Branch**
  
  > `$ git branch test-branch`

**Erstelen eines Branch und direkt wechseln in den neuen branch**
  
  > `$ git checkout -b test-branch`

**Löschen eines Branch / Grossed D = Force the Delete**
  
  > `$ git branch -d test-branch`

**Remote Branch Löschen**
  
  > `$ git push origin --delete test-branch`

**Remote Branch Löschen Zweite Variante**
  
  > `$ git push origin :test`

**aktueller Branch umbenennen**
  
  > `$ git branch -m test-branch`

**Remote Branch anzueigen**
  
  > `$ git branch -a`

**Branch zu test-branch wechseln**
  
  > `$ git checkout test-branch`

**Git push neuen Branch auf existierenden Upstream Repository**
  
  > `$ git push --set-upstream origin test`  

## 05 Merge 

**It is best Practice to make sure receiving branch and the merging branch are up-to-date with the latest remote changes**
  
  > `$ git fetch`
  > `$ git pull`

**Wechsle zuerst in den Ziel Branch und führe dann von dort wie folgt ein Merge aus**
  
  > `$ git checkout master`
  > `$ git merge test-branch`

**Danach noch die änderungen auf den Remote Repository Pushen** 
  
  > `$ git push`

## 06 Conflicts

**Wenn du beim Pull oder push folgende Meldung erhälst hast du einen Konflikt** 
  
  ```
   $ CONFLICT (content): Merge conflict in git_auftrag_2/README.md
    Automatic merge failed; fix conflicts and then commit the result.
  ```
**Dies kann man auch nochmals mit `$ git status` überprüfen**
  
  > `$ git status`

**Nun einfach das erwähnte File mit dem Editor öffnen und die gewünschte Änderung abspeichern** 

  ```
   $ vi git_auftrag_2/README.md
  ```
**Danach kann das angepasste File nochmals hinzugefügt werden sowie commited und mit push auf das Repository laden.**  

  ```
   $ git add git_auftrag_2/README.md
   $ git commit -m "Resolved the Conflicted File"
   $ git pull
   $ git push
  ```

## 07 Restore

**Suche den entsprechenden Commit Hash mit**  

   > `$ git log -5`
   oder 
   > `$ git log --follow git_auftrag_2/README.md`

**Mit diesem Behel gehst du zurück auf den ausgewählten Commit**
  
  > `$ git checkout 10c23e0fc813e51a3aae47869c647c326d1ae325 git_auftrag_2/README.md`

**Danch File wieder hinzufügen und commiten sowie pushen**

 ```
 $ git add git_auftrag_2/README.md
 $ git commit -m "Rolleback Versuch 1"
 $ git push
  ```

**Merge Übersicht or the git diff**  

   > `$ git log --merge `

**Can be used to Reset to known good state during merge conflict**  

   > `$ git reset`

## 08 Comparing 

**Shows the difference between two input data sets** 
  
  > `$ git diff`

![Gitdiffresult](/images/git_diff.jpg)

1. Line shows the two input sources of the diff
2. Line shows the Metadata
3. Line Shows the assignd symbole oto each source
4. Line Shows the assignd symbole oto each source
5. @@ = Header -[Subtracted Lines] +[Added Lines] @@ = Header
6.  - Subtracted Line in textfile
7.  + Added Line in Textfile

## 09 Exclude 

**Erstellen des Global Excludion list im Homeverzeichniss am besten**
  
  > `$ touch ~/.gitignore`

**Hinzufügen der Liste zur Config**
  
  > `$ git config --global core.excludesFile ~/.gitignore`

**Hinzufügen von exclusions zur Liste**
  
  > `$ echo exclude.txt >> .gitignore`

**Force an ignored File**
  
  > `$ git add -f exclude.txt`

**Troubleshoot your Exludion Files**
  
  > `$  git check-ignore -v exclude.txt"`

**Interpretieren der ausgabe:**

> `exclusion file containing the pattern : line number of the pattern : pattern    file name`

## 10 Alias 

**Create an Alias called co who stands for checkout**
  
  > `$ git config --global alias.co checkout`

```
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```


---
<small>Quelle: [`Git How to`](https://www.atlassian.com/git/tutorials/setting-up-a-repository)</small>
---

> [⇧ **Back to top**](#Workflows)

---
