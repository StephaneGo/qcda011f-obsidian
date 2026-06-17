

Date: 17/06/2026
Tags: 

[[-Git]]

# Notes

`git stash` sert à mettre de côté tes modifications locales, faire un `git pull`, puis récupérer ton travail ensuite. C’est pratique quand tu n’as pas fini mais que tu dois synchroniser ta branche avec le dépôt distant.

## Cas d’usage

Tu es en train de modifier des fichiers, mais tu veux récupérer les derniers changements du dépôt distant sans faire de commit intermédiaire. Tu peux alors “stocker temporairement” ton travail, faire le pull, puis le réappliquer.

## Commandes de base

```bash
 git stash 
 git pull 
 git stash pop
```


- `git stash` met de côté les modifications en cours.
    
- `git pull` récupère les derniers changements.
    
- `git stash pop` réapplique tes modifications et supprime le stash de la liste.


# Références

[https://www.elephorm.com/formation/apprendre-git-les-fondamentaux/comprendre-le-stash-dans-git](https://blog.lecacheur.com/2018/06/14/git-le-b-a-ba-mettre-de-cote-son-travail/)
