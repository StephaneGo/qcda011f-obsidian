

Date: 24/06/2026
Tags: 
[[-Securite]]


# Structure d'un token JWT 

Un token JWT est constitué de **3 parties** séparées par des points : `header.payload.signature`.  

## 1) Header

Le header décrit le type de token et l’algorithme de signature, par exemple `HS256`.  

Exemple simple :

```json
{
  "typ": "JWT",
  "alg": "HS256"
}
```


## 2) Payload

Le payload contient les données transportées, appelées **claims**. On y met souvent l’identifiant utilisateur, la date d’expiration, ou des rôles.   

Exemple simple :

```json
{
  "sub": "123456",
  "name": "Alice",
  "role": "ADMIN",
  "exp": 1719235200
}
```

Quelques claims fréquents :

- `sub` : sujet, souvent l’identifiant utilisateur.  
- `iss` : émetteur du token.  
- `exp` : date d’expiration.  
- `iat` : date de création.  


## 3) Signature

La signature sert à vérifier que le token n’a pas été modifié. Elle est calculée à partir du header, du payload, et d’une clé secrète ou d’une clé privée selon l’algorithme utilisé. [^10] 

Exemple de forme complète :

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJzdWIiOiIxMjM0NTYiLCJuYW1lIjoiQWxpY2UiLCJyb2xlIjoiQURNSU4iLCJleHAiOjE3MTkyMzUyMDB9
.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```


## À retenir

Un JWT n’est pas chiffré par défaut : son contenu peut souvent être décodé, mais la **signature** permet de détecter toute modification. En pratique, on l’utilise surtout pour authentifier un utilisateur et transporter quelques infos utiles de façon fiable.   


# Références

