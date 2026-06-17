


Date: 17/06/2026
Tags: 
[[-Spring framework]]


# Notes

point important : `spring.config.import=file:../.env[.properties],file:.env[.properties]` **ne charge pas du YAML**, il dit à Spring de lire un fichier sans extension **comme un fichier `.properties`**. Donc cette syntaxe ne convient pas à un `.env` au format YAML.[^1][^7]

## Ce que ça veut dire

Si ton fichier est un vrai `.env` du type :

```env
DB_USER=postgres
DB_PASSWORD=secret
```

alors Spring peut l’interpréter comme un fichier “properties-like” avec `[.properties]`. En revanche, un fichier YAML doit contenir une structure YAML valide, avec `:` et indentation. Spring ne peut pas deviner qu’un fichier `.env` est du YAML juste avec cette syntaxe.[^10][^1]

## Exemple correct en YAML

Si tu veux utiliser des fichiers YAML externes, fais plutôt ça :

```yaml
spring:
  config:
    import:
      - optional:file:../config/application-local.yaml
      - optional:file:./application-local.yaml
```

Puis dans `application-local.yaml` :

```yaml
app:
  db:
    user: postgres
    password: secret
    url: jdbc:postgresql://localhost:5432/demo
```

Et dans ton `application.yaml` principal :

```yaml
spring:
  config:
    import:
      - optional:file:../config/application-local.yaml
      - optional:file:./application-local.yaml

app:
  db:
    user: ${app.db.user}
    password: ${app.db.password}
    url: ${app.db.url}
```


## Si tu veux garder le nom `.env`

Deux possibilités réalistes :

- Garder `.env` au format `clé=valeur`, et l’importer comme fichier de propriétés avec `[.properties]`.[^7][^1]
- Renommer ton fichier en `.env.yaml` ou `application-local.yaml` et utiliser une importation YAML classique.[^9][^10]


## Exemple précis recommandé

### Option 1 : `.env` façon properties

```yaml
spring:
  config:
    import:
      - optional:file:../.env[.properties]
      - optional:file:.env[.properties]
```

`.env` :

```properties
DB_USER=postgres
DB_PASSWORD=secret
DB_URL=jdbc:postgresql://localhost:5432/demo
```


### Option 2 : YAML natif

```yaml
spring:
  config:
    import:
      - optional:file:../.env.yaml
      - optional:file:.env.yaml
```

`.env.yaml` :

```yaml
db:
  user: postgres
  password: secret
  url: jdbc:postgresql://localhost:5432/demo
```


## Conclusion

Si tu veux vraiment du **YAML**, n’utilise pas `.env[.properties]` : crée un fichier `.yaml` ou `.yml` et importe-le directement. Si tu veux garder l’extension `.env`, alors le contenu doit être au format propriétés.[^1][^10]

Je peux te préparer un **exemple complet Spring Boot 4.1** avec `application.yaml`, `.env.yaml`, et un `@ConfigurationProperties` prêt à copier.
<span style="display:none">[^2][^3][^4][^5][^6][^8]</span>



# Références

[https://www.surly.dev/articles/how-to-use-env-files-with-spring-boot](https://www.surly.dev/articles/how-to-use-env-files-with-spring-boot)


[^1]: https://stackoverflow.com/questions/75793877/reading-env-file-parameters-from-application-yml-in-spring-boot

[^2]: https://www.surly.dev/articles/how-to-use-env-files-with-spring-boot

[^3]: https://www.reddit.com/r/learnjava/comments/1epjwq8/how_do_you_use_env_files_or_deal_with_environment/

[^4]: https://www.wotemo.com/posts/16386.html

[^5]: https://juejin.cn/post/7156587568941760542

[^6]: https://stackoverflow.com/questions/78298713/unable-to-reference-a-property-defined-inside-a-file

[^7]: https://stackoverflow.com/questions/73053852/spring-boot-env-variables-in-application-properties

[^8]: https://happymk.tistory.com/15

[^9]: https://kimyhcj.tistory.com/431

[^10]: https://stackoverflow.com/questions/77095097/spring-wont-read-env

