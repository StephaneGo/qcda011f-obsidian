

Date: 17/06/2026
Tags: 
[[-Git]]


# Notes


## À quoi sert `.gitignore` ?

Dans un projet Java ou Spring, beaucoup de fichiers ne doivent pas être versionnés : fichiers compilés, dossiers de build, configurations locales, logs, fichiers de l’IDE, etc. Ignorer ces fichiers permet de garder un dépôt plus propre, plus léger et plus sûr.[^3][^7][^9]

### Exemples de fichiers à ignorer

- Les fichiers compilés comme `*.class`.[^9]
- Les dossiers de build comme `target/` ou `build/`.[^7][^9]
- Les fichiers de configuration locaux comme `local.properties`.[^9]
- Les fichiers liés à l’IDE comme `.idea/`, `.classpath`, `.project`, `.settings/`.[^7][^9]
- Les fichiers de logs comme `*.log`.[^7]


## Où placer le fichier

On place généralement `.gitignore` à la **racine** du projet, c’est-à-dire au même niveau que `pom.xml` ou `build.gradle`. Ce fichier peut ensuite être versionné pour que toute l’équipe profite des mêmes règles.[^5][^1]

## Syntaxe de base

Chaque ligne du fichier contient une règle d’ignorance. On peut écrire un nom exact, un chemin, ou un motif avec des caractères génériques.[^2][^4][^6]

### Règles courantes

- `nomfichier.txt` : ignore ce fichier précis.[^2]
- `dossier/` : ignore un dossier entier.[^6][^2]
- `*.class` : ignore tous les fichiers finissant par `.class`.[^9]
- `logs/*.log` : ignore les fichiers `.log` dans `logs/`.[^2]
- `!fichier.txt` : réactive un fichier que les autres règles ignoraient.[^1][^6]


## Exemple pour Java et Spring

Voici un exemple adapté à un projet Java/Spring :

```gitignore
# Fichiers compilés
target/
build/
*.class

# IDE IntelliJ
.idea/
*.iml

# Eclipse
.project
.classpath
.settings/

# Logs
*.log

# Fichiers système
.DS_Store
Thumbs.db
```

Ce type de fichier est courant dans les projets Java et Spring, car Maven ou Gradle génèrent des dossiers que l’on ne veut pas committer, et les IDE créent aussi leurs propres fichiers locaux.[^7][^9]

## Point important

Si un fichier a déjà été ajouté au dépôt, le mettre dans `.gitignore` ne suffit pas à l’arrêter : il faut d’abord le retirer de l’index Git. GitHub recommande d’utiliser `git rm --cached FICHIER` pour arrêter son suivi tout en gardant le fichier sur l’ordinateur.[^5]

## À retenir

- `.gitignore` sert à dire à Git quoi **ne pas suivre**.[^1][^5]
- Il se place souvent à la racine du projet.[^4][^1]
- Chaque ligne est une règle, avec des motifs simples comme `*`, `/` et `!`.[^6][^1]
- En Java/Spring, on ignore surtout les fichiers générés, les fichiers d’IDE et les logs.[^9][^7]


# Références



[^1]: https://comprendre-git.com/fr/config/git-ignore/

[^2]: https://github.com/PonteIneptique/cours-git/blob/master/GITIGNORE.md

[^3]: https://codefinity.com/fr/courses/v2/7533d91f-0a23-44a3-afc7-c84d5072e189/2b2413e6-d40a-4f31-b81a-775e2e9f203b/ce2e73a6-8b27-4c71-ba80-2153d8436bf9

[^4]: https://code.mu/fr/git/book/prime/operations/ignore/

[^5]: https://docs.github.com/fr/get-started/git-basics/ignoring-files

[^6]: https://microlead.fr/cours/git/ignorer-des-fichiers

[^7]: https://github.com/OpenClassrooms-Student-Center/Developpez-le-back-end-en-utilisant-Java-et-Spring/blob/main/.gitignore

[^8]: https://fr.linkedin.com/learning/l-essentiel-de-github/utiliser-le-fichier-gitignore

[^9]: https://bruno.univ-tln.fr/git/gitignore

[^10]: https://www.reddit.com/r/SpringBoot/comments/1lu7j13/what_are_the_files_i_should_put_in_gitignore/

