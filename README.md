# Rapport Ethical Hacking

### Qu'est-ce que c'est ?

Ce rapport a pour but de présenter les différentes attaques et outils utilisés dans le cadre de l'Ethical Hacking. Il est divisé en trois parties : les vecteurs d'attaques initiaux dans un Active Directory, les post-compromise enumeration et les post-compromise attacks.

## Sommaire : 

[Vecteur d’attaque initiaux dans un AD](#vecteur-dattaque-initiaux-dans-un-ad)

- [LLMNR poisoning](#LLMNR-poisoning-) 
- [Comment capturer les hash NTLM V2](#Comment-capturer-les-hash-NTLM-V2-)
- [Comment craquer les hash NTLM V2](#Comment-craquer-les-hash-NTLM-V2)
- [Recherche sur les mesures d’atténuation LLMMR poisoning](#Recherche-sur-les-mesures-datténuation-LLMMR-poisoning)
- [SMB relay attack](#SMB-relay-attack)
- [Comment avoir un accès shell dans un environnement AD](#Comment-avoir-un-accès-shell-dans-un-environnement-AD)
- [Attaque ipv6](#Attaque-ipv6)
- [Outils utilisés pour les attaques ipv6](#Outils-utilisés-pour-les-attaques-ipv6)
- [Recommandation pour atténuer les attaques ipv6](#Recommandation-pour-atténuer-les-attaques-ipv6)
- [Passback attack](#Passback-attack)
- [Autres types d’attaques AD](#Autres-types)

[Post compromise enumeration :](#post-compromise-enumeration-:)

- [Powerview](#Powerview)
- [Bloodhound quel est son rôle ?](#Bloodhound-quel-est-son-rôle-?)
- [LDAP domain dump](#LDAP-domain-dump)
- [PlumbHound](#PlumbHound)
- [PingCastle](#PingCastle)

[Post Compromise Attack:](#post-compromise-attack:)

- [PassTheHash](#PassTheHash)
- [CrackMapExec](#CrackMapExec)
- [SecretDump](#SecretDump)
- [Pass Attack Mitigation](#Pass-attack-mitigation)
- [Incognito](#Incognito)
- [Kerberoasting](#Kerberoasting-)
- [GPP et CPassword](#GPP-et-CPassword)
- [Mimicats](#Mimicats)
- [golden tickets](#Golden-tickets)

---
<u>

## Vecteur d’attaque initiaux dans un AD
</u>


### LLMNR poisoning :


LLMNR est un protocole **ipv4 et ipv6** qui permet à un ordinateur de <ins>résoudre le nom d'un autre ordinateur sans utiliser de serveur DNS</ins>. Il est utilisé pour résoudre les noms de domaine sur un réseau local lorsque le serveur DNS n'est pas disponible.  
LLMNR est activé par défaut sur les systèmes d'exploitation Windows.

Lorsqu'une requête DNS d'un hôte échoue, l'hôte diffuse une requête LLMNR sur le réseau local pour voir si un autre hôte peut répondre.

On peut donc se poser la question: __**comment ce protocole est-il faillible**__ ?  
Consultons l'image ci-dessous ([source](https://tcm-sec.com/llmnr-poisoning-and-how-to-prevent-it/)) :

![llmnr](assets/llmnr-vulnerable.png)

Une attaque LLMNR-poisoning est donc une attaque dans laquelle un acteur malveillant **écoute les requêtes LLMNR et répond avec sa propre adresse IP** (ou une autre adresse IP de son choix) pour rediriger le trafic.  
Cela peut conduire au **vol d'informations d'identification** et aux attaques par relais dans Active Directory. Attaques relais que nous voyons dans la partie suivante.

### Comment capturer les hash NTLM V2 :

NTLM est un protocole d'authentification utilisé par les systèmes d'exploitation Windows. Il est utilisé pour authentifier les utilisateurs et les ordinateurs dans un domaine Windows.

Afin de capturer les hash NTLM V2, il est possible d'utiliser des outils comme **Responder** ou **Bettercap**.  
Une fois cela fait, il suffit de bruteforce (ou d'uiliser des rainbow tables) pour obtenir le mot de passe en clair.

Cette attaque est possible grâce à la faiblesse du protocole NTLM qui stocke les mots de passe sous forme de hash, et qui dit hash dit possibilité de le casser.



### Kerberoasting :

Le Kerberoasting est une technique d'attaque post-exploitation qui tente de craquer le mot de passe d'un compte de service au sein d'Active Directory (AD).

Dans ce type d'attaque, un cyberadversaire se faisant passer pour l'utilisateur d'un compte avec un nom de principal de service (SPN, Service Principal Name) demande un ticket qui contient un mot de passe chiffré, ou Kerberos. (Le SPN est un attribut qui lie un service à un compte utilisateur dans AD.) Le cyberadversaire tente ensuite de craquer le hachage du mot de passe hors ligne, souvent en recourant à des techniques d'attaque par force brute.

Une fois les identifiants en texte brut du compte de service exposés, le cyberadversaire est en possession des identifiants utilisateur, qu'il peut alors utiliser pour usurper l'identité du propriétaire du compte. Il se donne ainsi toutes les apparences d'un utilisateur approuvé et légitime et peut accéder sans restriction à tout système, ressource ou réseau auquel le compte compromis a accès. [Source :](https://www.crowdstrike.com/fr-fr/cybersecurity-101/cyberattacks/kerberoasting/?srsltid=AfmBOorm4DpYOI7icsrFKaWGRjiVSGw8VU_KMYyTEilqydu2mDHtCOsh)






