---
title: "Comprendre l'adressage IPv6 : Un guide approfondi"
date: 2025-01-03 19:00:00 +0100
tags: [IPv6, Réseaux]
categories: [Networking]
description: "Comprendre et calculer l'adressage IPv4, les classes d'adresses"
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/ipv6.png
---


## Introduction

Avec l'explosion du nombre de périphériques connectés, l'adressage IPv4 a atteint ses limites, rendant indispensable la transition vers IPv6. Ce guide a pour but de fournir une compréhension approfondie de l'adressage IPv6, de ses concepts fondamentaux à son application pratique, tout en explorant les calculs associés.

## Pré-requis

Avant de vous plonger dans l'adressage IPv6, vous devez :

- **Connaître les bases des réseaux** : Comprendre les adresses IP, les sous-réseaux et les protocoles de communication.
- **Avoir des outils adaptés** : Utilisez un simulateur réseau comme Cisco Packet Tracer, GNS3 ou encore une configuration physique pour tester vos compétences.

## Concepts clés de l'adressage IPv6

### 1. Qu'est-ce qu'une adresse IPv6 ?

Une adresse IPv6 est une combinaison unique de 128 bits, exprimée en huit groupes de quatre caractères hexadécimaux, séparés par des deux-points. Exemple :

```plaintext
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

### 2. Notation abrégée

Pour simplifier, IPv6 permet d'utiliser une notation abrégée :

- Supprimez les zéros initiaux : `2001:db8:85a3:0:0:8a2e:370:7334`
- Remplacez les suites de zéros par `::` : `2001:db8:85a3::8a2e:370:7334`

> **Attention** : Le double deux-points `::` ne peut être utilisé qu'une seule fois dans une adresse.
{: .prompt-danger }

### 3. Types d'adresses IPv6

| Type              | Préfixe           | Description                         |
|-------------------|------------------|-------------------------------------|
| Unicast           | -                | Adresse unique pour un périphérique |
| Multicast         | FF00::/8         | Communication avec plusieurs nœuds  |
| Anycast           | -                | Adresse partagée entre plusieurs nœuds |
| Link-local        | FE80::/10        | Utilisée sur un lien local          |
| Global unicast    | 2000::/3         | Adresses routables sur Internet     |

### 4. Préfixe et longueur

Une adresse IPv6 est toujours accompagnée de la notation CIDR pour indiquer la partie réseau. Par exemple, `2001:db8::/32` indique que les 32 premiers bits sont réservés pour l'identification du réseau.

## Étapes détaillées

### Étape 1 : Identifier les besoins du réseau

1. **Nombre de sous-réseaux** : Déterminez combien de sous-réseaux sont nécessaires.
2. **Planification des préfixes** : Allouez des préfixes en fonction des besoins actuels et futurs.

### Étape 2 : Calculer les sous-réseaux IPv6

Avec IPv6, chaque sous-réseau a généralement un préfixe `/64`. Vous pouvez emprunter des bits supplémentaires pour créer des sous-réseaux plus petits.

| Bits empruntés | Nombre de sous-réseaux |
|----------------|------------------------|
| 1              | 2                      |
| 2              | 4                      |
| 3              | 8                      |

### Étape 3 : Configurer l'adressage IPv6

#### Exemple avec Cisco Packet Tracer :

1. **Attribuer une adresse IPv6 à un périphérique :**

```plaintext
PC> ipv6 address 2001:db8:1::2/64
```

2. **Configurer un routeur :**

```plaintext
Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ipv6 address 2001:db8:1::1/64
Router(config-if)# ipv6 enable
Router(config-if)# no shutdown
```

3. **Activer le routage IPv6 :**

```plaintext
Router(config)# ipv6 unicast-routing
```

### Étape 4 : Vérifier la connectivité

- **Commandes utiles :**

```plaintext
Router# show ipv6 interface brief
Router# ping 2001:db8:1::2
Router# traceroute ipv6 2001:db8:1::2
```

### Étape 5 : Gérer les adresses Link-local

Les adresses Link-local sont automatiquement assignées à chaque interface IPv6. Elles sont utilisées pour la communication entre nœuds sur un même lien local.

```plaintext
Router# show ipv6 interface GigabitEthernet0/0
```

## Conclusion

IPv6 offre une solution robuste pour l'avenir des réseaux. Sa vaste plage d'adressage et ses fonctionnalités avancées en font un incontournable pour les professionnels des réseaux.

