

Date: 24/06/2026
Tags: 
[[-Securite]]


# Notes


Les JWT servent principalement à **authentifier un utilisateur** et à transmettre des informations de manière vérifiable entre un client et un serveur.

## À quoi ça sert

- Après une connexion réussie, le serveur génère un token JWT.
- Le client l’envoie ensuite à chaque requête, souvent dans l’en-tête `Authorization: Bearer ...`.
- Le serveur vérifie la signature du token pour savoir si la requête est légitime, sans garder de session côté serveur.  


## Pourquoi c’est utile

- **Stateless** : pas besoin de stocker une session sur le serveur. 
- **Rapide et portable** : facile à utiliser dans une API web, mobile ou microservices.  
- **Sécurisé pour l’intégrité** : si le token est modifié, la signature ne correspond plus.  


## Ce que ça ne fait pas

- Un JWT n’est pas un mot de passe.
- Il n’est pas forcément chiffré, donc son contenu peut être lisible.
- Il ne remplace pas toute la logique d’authentification : il sert surtout à **prouver l’identité** après connexion et à transporter des droits ou infos utiles.  


## Exemple simple

1. Tu te connectes avec ton email et ton mot de passe.
2. Le serveur te renvoie un JWT.
3. Tu l’envoies sur les requêtes suivantes.
4. Le serveur le valide et t’autorise l’accès si le token est valide.  

En une phrase : un JWT permet au serveur de reconnaître un utilisateur à chaque requête, sans garder une session classique en mémoire.  



# Références

