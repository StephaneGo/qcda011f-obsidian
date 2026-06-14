
Date: 2026-06-14
Tags: 
[[-Java]]
[[-Spring framework]]



# fiche cours lombok 

Voici une **fiche de cours concise et utile** pour apprendre l’essentiel de **Lombok dans un projet Spring Boot**.

## 1. C’est quoi Lombok ?

Lombok est une bibliothèque Java qui **réduit le code répétitif** en générant automatiquement du code au moment de la compilation.
Elle sert surtout à éviter d’écrire à la main les getters, setters, constructeurs, `toString()`, `equals()` et `hashCode()`.

## 2. Pourquoi l’utiliser avec Spring Boot ?

Dans un projet Spring Boot, on l’utilise souvent pour :

- alléger les classes `Entity`, `DTO`, `Request`, `Response` ;
- rendre le code plus lisible ;
- éviter les classes trop longues à cause du boilerplate.

Exemple typique : une classe DTO avec 5 champs peut être réduite à quelques annotations au lieu de dizaines de lignes de code.

 

## 3. Dépendance Maven

En général, on ajoute Lombok comme dépendance de compilation :

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <scope>provided</scope>
</dependency>
```

Selon la configuration du projet, `provided` ou `compileOnly` peut être utilisé.

 

## 4. Configuration de l’IDE

Pour que Lombok fonctionne bien :

- il faut **activer le traitement des annotations** dans l’IDE ;
- installer le plugin Lombok si nécessaire.

Sans ça, l’IDE peut afficher des erreurs alors que le code compile correctement.

 

## 5. Les annotations essentielles

### `@Getter` et `@Setter`

Génèrent automatiquement les accesseurs.

```java
@Getter
@Setter
public class User {
    private String name;
    private int age;
}
```


### `@ToString`

Génère la méthode `toString()`.

```java
@ToString
public class User {
    private String name;
    private int age;
}
```


### `@NoArgsConstructor`

Génère un constructeur sans argument.

```java
@NoArgsConstructor
public class User {
    private String name;
}
```


### `@AllArgsConstructor`

Génère un constructeur avec tous les champs.

```java
@AllArgsConstructor
public class User {
    private String name;
    private int age;
}
```


### `@RequiredArgsConstructor`

Génère un constructeur pour les champs `final` ou `@NonNull`.

Très utile avec Spring pour l’injection par constructeur.

```java
@RequiredArgsConstructor
@Service
public class UserService {
    private final UserRepository userRepository;
}
```


### `@Data`

Génère en une seule fois :

- getters,
- setters,
- `toString()`,
- `equals()`,
- `hashCode()`,
- constructeur requis.

Très pratique pour les petites classes, mais à utiliser avec prudence sur les entités JPA.

### `@Builder`

Permet de créer des objets avec un style plus lisible.

```java
@Builder
public class User {
    private String name;
    private int age;
}
```

Utilisation :

```java
User user = User.builder()
    .name("Alice")
    .age(25)
    .build();
```


### `@Slf4j`

Ajoute automatiquement un logger.

```java
@Slf4j
@Service
public class OrderService {
    public void process() {
        log.info("Traitement en cours");
    }
}
```


 

## 6. Lombok avec Spring Boot

### a. Dans les services

On combine souvent `@RequiredArgsConstructor` avec les dépendances `final`.

```java
@RequiredArgsConstructor
@Service
public class ProductService {
    private final ProductRepository productRepository;
}
```

C’est propre et compatible avec l’injection de dépendances Spring.

### b. Dans les DTO

On utilise souvent `@Getter`, `@Setter`, `@NoArgsConstructor`, `@AllArgsConstructor`, `@Builder`.

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class ProductDto {
    private String name;
    private Double price;
}
```


### c. Dans les logs

`@Slf4j` est très courant dans les classes Spring :

```java
@Slf4j
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        log.info("Endpoint /hello appelé");
        return "Hello";
    }
}
```


 

## 7. Bonnes pratiques

- Utiliser Lombok surtout pour les **DTO**, **services**, **objets simples**.
- Éviter d’abuser de `@Data` sur les **entités JPA**.
- Préférer `@RequiredArgsConstructor` dans les services Spring.
- Ne pas cacher la logique métier derrière Lombok : Lombok aide, mais ne remplace pas la lisibilité.

 

## 8. Pièges fréquents

- L’IDE n’est pas configuré, donc on voit des erreurs fictives.
- `@Data` sur une entité JPA peut causer des problèmes avec `equals()` / `hashCode()`.
- `@Builder` sur des classes trop complexes peut rendre le code moins clair.
- Les annotations Lombok ne remplacent pas une vraie bonne conception objet.

 

## 9. À retenir

Lombok sert à **écrire moins de code répétitif** dans un projet Spring Boot.
Les annotations les plus utiles au quotidien sont :

- `@Getter`
- `@Setter`
- `@NoArgsConstructor`
- `@AllArgsConstructor`
- `@RequiredArgsConstructor`
- `@Builder`
- `@Slf4j`


## 10. Exemple complet

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString
public class Person {
    private String firstName;
    private String lastName;
    private int age;
}
```


