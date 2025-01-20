# Bug Bounty basé sur PortSwigger Web Security Academy.

## Table des matières

- [Authentification](#authentification)
- [Inclusion de fichiers](#inclusion-de-fichiers)
- [XSS](#xss)
- [Upload de fichiers](#upload-de-fichiers)
- [SSRF](#ssrf)
- [CSRF](#csrf)
- [Open Redirect](#open-redirect)
- [SQLI](#sqli)

---

# Authentification

## 0x01 - Bruteforce

Pour le premier lab, on va utiliser l'attaque bruteforce (énumération de mots de passe) pour trouver le mot de passe de l'utilisateur.
En utilisant Hydra, on détermine assez rapidement les creds :

```
login : jeremy
password: letmein
```

## 0x02 - Bypass MFA 

Pour ce lab, on va devoir bypass le MFA (Multi-Factor Authentication) pour accéder au compte de l'utilisateur.  
Une règle d'or quand on fait du client-serveur est : "Ne jamais faire confiance au client".  
Le problème ici est que le serveur fait confiance au client pour le nom d'utilisateur.

Ainsi, on part de :

`username2=jesamy&mfa=610829` 

on modifie le nom d'utilisateur : 

`username2=jeremy&mfa=610829`

On a donc accès au compte de l'utilisateur.

## 0x03 - IDOR

de Vaadata.com : 
`Une vulnérabilité de type IDOR est un problème de contrôle de droits, qui apparait lorsqu’une référence directe à un objet (fichiers, informations personnelles, etc.) peut être contrôlée par un utilisateur.`. 

C'est donc ce qu'on va utiliser pour accéder à un compte qui ne nous appartient pas.

on part donc de l'url de base : `http://0.0.0.0/labs/e0x02.php?account=1009`

on modifie les informations du compte : `http://localhost/labs/e0x02.php?account=1008` 

On a donc accès, sans posséder les droits.

## 0x04 - Json Web Token (JWT)

On peut signer un JWT... ou pas.  
Et si on ne le signe pas, un simple passage sur cyberchef nous permet de modifier le contenu du JWT et de nous connecter en tant qu'admin.  
En l'occurence : 

On a juste à modifier le payload du JWT pour avoir les droits d'admin : rôle de `staff` à `admin`, requête `PUT` et le tour est jouée.

# Inclusion de fichiers

## 0x01 - Et si on incluait des fichiers ?

Ça ressemble un peu à une vulnérabilité IDOR. Mais ça n'en est pas une.  
En effet, on peut inclure des fichiers en utilisant le paramètre `file` de l'url.  
Exemple : `http://0.0.0.0/labs/fi0x01.php?filename=../fi0x01.php` nous permet de voir le contenu du fichier en question.

Plutôt embêtant, mais nous permet de solve le lab.

## 0x02  - Inclusion de fichiers : le retour

Dans ce lab-ci, il semble qu'il y ait un filtre implémenté pour empêcher l'inclusion de fichiers.  
Cependant, on peut toujours le contourner en utilisant un caractère encodé comme `%00` pour passer outre le filtre.  

# XSS




