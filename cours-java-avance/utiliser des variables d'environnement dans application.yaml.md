

Date: 17/06/2026
Tags: 

[[-Spring framework]]

# Notes

  

Exemple :   

```yaml

spring:

  datasource:

    url: ${DB_URL}

    username: ${DB_USER}

    password: ${DB_PASSWORD}

```

  

Spring remplacera `${DB_URL}`, `${DB_USER}` et `${DB_PASSWORD}` par les valeurs trouvées dans l’environnement au démarrage.[^1][^4][^7]

  

 

## Variante avec valeur par défaut

  

Tu peux aussi prévoir une valeur de secours :

  

```yaml

server:

  port: ${PORT:8080}

```

  

Ici, Spring utilise `PORT` si elle existe, sinon `8080`.

  


# Références

