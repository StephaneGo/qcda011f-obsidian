

Date: 18/06/2026
Tags: 

[[-Spring framework]]
[[-Tests]]

# Notes

# comment executer du sql avant un test d'une classe annotée @SpringBootTest

Oui — le plus simple est d’utiliser `@Sql` sur ta classe de test, avec `@SpringBootTest`. Si tu veux exécuter un script avant **chaque** test, place l’annotation au niveau de la classe ; si tu veux le faire une seule fois avant la classe, utilise plutôt `@BeforeAll` avec un `DataSource` ou `ResourceDatabasePopulator`.[^3][^8]

## Avant chaque test

```java
@SpringBootTest
@Sql(scripts = "/sql/data.sql", executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD)
class MyServiceTest {

    @Test
    void testSomething() {
        // ton test
    }
}
```

Ici, `data.sql` est exécuté avant chaque méthode de test. `@Sql` peut être appliqué sur la classe ou sur une méthode, et la phase `BEFORE_TEST_METHOD` est faite pour préparer les données avant l’exécution du test.[^2][^8]

## Une seule fois avant toute la classe

### Méthode 1

  

```java

@SpringBootTest

@Sql(

    scripts = "/sql/data.sql",

    executionPhase = Sql.ExecutionPhase.BEFORE_TEST_CLASS

)

class MyTest {

}

```

  

Deux points importants :

  

- `BEFORE_TEST_CLASS` charge le contexte d’application assez tôt, donc l’ordre d’exécution peut surprendre si tu relies `@DynamicPropertySource` ou d’autres callbacks de démarrage.

- Si tu es sur une version plus ancienne de Spring, cette option n’existe pas ; dans ce cas, il faut rester sur `BEFORE_TEST_METHOD` ou exécuter le script manuellement dans `@BeforeAll`.

### Méthode 2

```java
@SpringBootTest
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
class MyServiceTest {

    @Autowired
    DataSource dataSource;

    @BeforeAll
    void initDb() {
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator();
        populator.addScript(new ClassPathResource("sql/data.sql"));
        populator.execute(dataSource);
    }

    @Test
    void testOne() {
    }

    @Test
    void testTwo() {
    }
}
```

Cette approche est utile si tu veux charger des données une seule fois pour toute la classe de test.[^3]

## Nettoyage après test

Tu peux aussi ajouter un script de nettoyage après chaque test avec `AFTER_TEST_METHOD`.[^5]

```java
@Sql(
    scripts = "/sql/cleanup.sql",
    executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD
)
```


## Recommandation pratique

- Utilise `@Sql` si tu veux une solution simple et lisible.[^8]
- Utilise `@BeforeAll` si tu veux initialiser une seule fois.[^3]
- Ajoute `@Transactional` si ton test doit être rollback automatiquement à la fin.[^5]

Je peux aussi te donner un exemple complet avec H2, PostgreSQL, ou un `schema.sql` + `data.sql`.
<span style="display:none">[^1][^10][^4][^6][^7][^9]</span>



# Références




[^1]: https://stackoverflow.com/questions/76255842/spring-boot-test-execute-sql-script-once-before-all-test

[^2]: https://zenn.dev/masatoshi_tada/articles/bd4433ea5eac45?locale=en

[^3]: https://agohack.com/how-to-execute-sql-scripts-once-before-in-spring-test/

[^4]: https://velog.io/@sleekydevzero86/sql-data-setup-in-junit-tests

[^5]: https://stackoverflow.com/questions/62744812/how-to-execute-sql-script-before-and-after-each-test-method

[^6]: https://talk-to-the-duck.dev/fr/posts/howto/how-to-test-springboot-java-application/

[^7]: https://codingtechroom.com/question/spring-boot-test-sql-execution-order

[^8]: https://docs.spring.io/spring-framework/reference/testing/annotations/integration-spring/annotation-sql.html

[^9]: https://luissantos.me/blog/posts/testing-spring-boot/

[^10]: https://www.jtips.info/Spring/Test

