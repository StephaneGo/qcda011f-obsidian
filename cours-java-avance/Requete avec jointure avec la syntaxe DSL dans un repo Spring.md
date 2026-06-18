

Date: 18/06/2026
Tags: 
[[-Spring framework]]



# Notes


Voici un exemple concret de **méthode de jointure définie par le nom** dans un `JpaRepository` avec Spring Data JPA, sans écrire explicitement la requête : tout est déduit du nom de la méthode.

Supposons que vous ayez deux entités : `Book` et `Author`, où chaque `Book` possède un champ `author` de type `Author`.

### Exemple d’entités

```java
@Entity
public class Author {
    @Id
    private Long id;
    private String name;
    // getters/setters
}

@Entity
public class Book {
    @Id
    private Long id;
    private String title;
    @ManyToOne
    private Author author;
    // getters/setters
}
```


### Exemple de méthode de jointure dans le Repository

Dans votre repository, vous pouvez écrire :

```java
public interface BookRepository extends JpaRepository<Book, Long> {
    // Jointure implicite : recherche tous les livres d’un auteur donné
    List<Book> findByAuthorName(String name);
}
```

**Explication :**

- `findByAuthorName` : Spring Data JPA comprend qu’il doit faire une jointure entre `Book` et `Author` via la propriété `author`, puis filtrer sur `name` de l’auteur.
- Cette méthode génère automatiquement la requête équivalente à :

```sql
SELECT b FROM Book b JOIN b.author a WHERE a.name = :name
```


Vous pouvez aller plus loin avec des critères multiples :

```java
List<Book> findByAuthorNameAndTitle(String name, String title);
```

Cela produira une requête avec jointure sur `author` et filtrera à la fois sur le nom de l’auteur et le titre du livre[^2][^6][^8].

### Points importants

- **Pas besoin d’annotation `@Query`** : tout est déduit du nom de la méthode.
- **Supporte les jointures sur des propriétés imbriquées** : il suffit d’enchaîner les noms des propriétés (ex : `findByAuthorCountryName`).

---

**Résumé** :
Pour faire une jointure avec Spring Data JPA en utilisant la convention de nommage, il suffit d’utiliser la syntaxe `findBy[ProprieteLiee][Champ]`. Par exemple :

```java
List<Book> findByAuthorName(String name);
```

Spring générera la requête de jointure automatiquement[^2][^6][^8].

<div style="text-align: center">⁂</div>


# Références


[^1]: https://gayerie.dev/epsi-b3-orm/spring_data/spring_data_jpa.html

[^2]: https://gayerie.dev/docs/spring/spring/spring_data_jpa.html

[^3]: https://soat.developpez.com/tutoriels/spring/spring-data/

[^4]: https://openclassrooms.com/fr/courses/6982461-utilisez-spring-data-pour-interagir-avec-vos-bases-de-donnees/7202714-executez-des-requetes-personnalisees

[^5]: https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html

[^6]: https://dev.to/krishna-nayak/spring-data-jpa-method-naming-conventions-build-queries-without-writing-sql-23o5

[^7]: https://www.neosoft.fr/nos-publications/blog-tech/spring-data-une-autre-facon-dacceder-aux-donnees/

[^8]: https://www.java4coding.com/contents/spring/springdatajpa/spring-data-jpa-method-naming-conventions

[^9]: https://openclassrooms.com/fr/courses/6982461-utilisez-spring-data-pour-interagir-avec-vos-bases-de-donnees/7201184-implementez-vos-entites-et-les-interfaces-repository

[^10]: https://docs.spring.io/spring-data/jpa/reference/repositories/query-methods-details.html

[^11]: https://www.jmdoudoux.fr/java/dej/chap-jpa.htm

[^12]: https://www.baeldung.com/spring-data-derived-queries

[^13]: http://orm.bdpedia.fr/jpamodel.html

[^14]: https://www.baeldung.com/spring-data-jpa-custom-naming

[^15]: https://learn.microsoft.com/fr-fr/azure/developer/java/spring-framework/configure-spring-data-jpa-with-azure-mysql

[^16]: https://stackoverflow.com/questions/68089538/can-i-use-a-method-name-to-create-a-spring-jpa-update-query

