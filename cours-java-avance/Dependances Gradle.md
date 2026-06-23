
Date: 23/06/2026
Tags: #Java #Gradle
## 1) Rappels courts (Java et .class)

- Java est un langage compilé : le code source (.java) est transformé par le compilateur javac en fichiers bytecode (.class).
    
- Les fichiers .class ne sont pas du code natif : la JVM charge et interprète (ou JIT-compile) ces bytecodes pour exécuter le programme.
    
- Pour compiler et pour exécuter, la JVM/Gradle doit connaître où trouver les classes : c’est le rôle du classpath.

## 2) Qu’est‑ce que le classpath ?

- Le classpath est la liste de JARs et dossiers contenant des .class que Java utilise.
    
- On distingue deux classpaths importants :
    
    - compile classpath : ce dont le compilateur a besoin pour vérifier que ton code se construit (types, signatures, annotations).
        
    - runtime classpath : ce dont la JVM a besoin pour lancer l’application (implémentations réelles, drivers, etc.).

## 3) Comment Gradle mappe les configurations sur les classpaths

- implementation : la dépendance est ajoutée au compile classpath et au runtime classpath (visible partout). Utilise-la pour la plupart des bibliothèques.
    
- compileOnly : la dépendance est ajoutée uniquement au compile classpath (visible au moment de la compilation), mais elle n’est **pas** empaquetée ni fournie à l’exécution. Utile pour des outils qui génèrent ou annotent le code (ex. Lombok) ou pour des API fournies par l’environnement d’exécution.
    
- runtimeOnly : la dépendance n’est **pas** visible au moment de la compilation (ton code ne s’y réfère directement), mais elle est ajoutée au runtime classpath et sera présente à l’exécution. Utile pour des drivers ou implémentations concrètes.

## 4) Exemple build.gradle minimal

`dependencies {`  
	`implementation 'com.google.guava:guava:33.0.0-jre' // compile + runtime`  
	`compileOnly 'org.projectlombok:lombok:1.18.32' // seulement à la compilation`  
	`runtimeOnly 'com.microsoft.sqlserver:mssql-jdbc:12.2.0.jre17' // seulement à l’exécution`  
}

## 5) Astuce pour éviter les erreurs

- Erreur typique compileOnly mal utilisée : ClassNotFoundException à l’exécution parce que la dépendance n’a pas été fournie au runtime.
    
- Erreur typique runtimeOnly mal utilisée : compilation échoue (cannot find symbol) parce que la dépendance n’était nécessaire pour vérifier les types mais n’était pas sur le compile classpath.

# Notes



# Références

https://docs.gradle.org/current/userguide/dependency_configurations.html
https://blog.gradle.org/introducing-compile-only-dependencies
https://stackoverflow.com/questions/61696863/gradle-compileonly-and-runtimeonly