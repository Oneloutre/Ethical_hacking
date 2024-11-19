# Rapport Ethical Hacking

### Qu'est-ce que c'est ?

Ce rapport a pour but de présenter les différentes attaques et outils utilisés dans le cadre de l'Ethical Hacking. Il est divisé en trois parties : les vecteurs d'attaques initiaux dans un Active Directory, les post-compromise enumeration et les post-compromise attacks.

## Sommaire : 

[Vecteur d’attaque initiaux dans un AD](#vecteur-dattaque-initiaux-dans-un-ad)

- [LLMNR poisoning](#LLMNR-poisoning) 
- [Comment capturer les hash NTLM V2](#Comment-capturer-les-hash-NTLM-V2)
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
- [Kerberoasting](#Kerberoasting)
- [GPP et CPassword](#GPP-et-CPassword)
- [Mimicats](#Mimicats)
- [golden tickets](#Golden-tickets)

---
<ins>

## Vecteur d’attaque initiaux dans un AD

</ins>

### LLMNR poisoning :

LLMNR est un protocole **ipv4 et ipv6** qui permet à un ordinateur de <ins>résoudre le nom d'un autre ordinateur sans utiliser de serveur DNS</ins>. Il est utilisé pour résoudre les noms de domaine sur un réseau local lorsque le serveur DNS n'est pas disponible.  
LLMNR est activé par défaut sur les systèmes d'exploitation Windows.

Lorsqu'une requête DNS d'un hôte échoue, l'hôte diffuse une requête LLMNR sur le réseau local pour voir si un autre hôte peut répondre.

On peut donc se poser la question: __comment ce protocole est-il faillible__ ?  
Consultons l'image ci-dessous ([source](https://tcm-sec.com/llmnr-poisoning-and-how-to-prevent-it/)) :

![llmnr](assets/llmnr-vulnerable.png)




