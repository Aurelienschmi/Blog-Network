---
title: "Comprendre et mettre en place le filtrage VLAN"
date: 2025-01-21 12:00:00 +0100
tags: ["VLAN", "Réseaux", "Filtrage"]
categories: ["Configuration", "Réseaux"]
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/vlan-filtering.png
---

## Introduction

Le filtrage VLAN est une technique essentielle pour contrôler et sécuriser les communications dans un réseau local (LAN) segmenté par des VLANs (Virtual Local Area Networks). En restreignant ou en autorisant les flux de données entre les VLANs, le filtrage permet de mieux gérer le trafic réseau et d'améliorer la sécurité. Cet article détaille le concept, la configuration et la gestion du filtrage VLAN, y compris sa mise en place, sa modification, et sa suppression.

---

## Pré-requis

Pour suivre ce guide, vous devez :

- Comprendre les bases des VLANs et leur configuration.
- Disposer d'un switch manageable (Cisco ou autre).
- Avoir un simulateur réseau comme Cisco Packet Tracer ou GNS3.
- Accès au CLI (Command Line Interface) des équipements réseau.

---

## Étapes détaillées

### 1. Qu'est-ce que le filtrage VLAN ?

Le filtrage VLAN consiste à restreindre les communications entre différents VLANs ou à limiter les VLANs autorisés sur un port trunk. Cela permet :

- De renforcer la sécurité en isolant des segments réseau sensibles.
- De mieux contrôler le trafic traversant les équipements réseau.
- De limiter les VLANs inutiles sur des liaisons inter-switches.

### 2. Mise en place du filtrage VLAN

#### a. Configurer les VLANs sur les switchs

Avant de mettre en place le filtrage, assurez-vous que les VLANs sont configurés sur vos switchs.

```plaintext
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name VLAN10
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name VLAN20
Switch(config-vlan)# exit
```

#### b. Configurer un port trunk

Les ports trunks permettent de transporter plusieurs VLANs entre les switchs.

```plaintext
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20
Switch(config-if)# exit
```

- **`switchport mode trunk`** : Configure le port en mode trunk.
- **`switchport trunk allowed vlan`** : Spécifie les VLANs autorisés sur ce port trunk.

#### c. Vérifier la configuration

```plaintext
Switch# show running-config interface GigabitEthernet0/1
```

Cette commande permet de vérifier que seuls les VLANs 10 et 20 sont autorisés sur le port trunk.

---

### 3. Modifier le filtrage VLAN

Vous pouvez modifier la liste des VLANs autorisés sur un port trunk en utilisant la commande suivante :

#### Ajouter un VLAN au port trunk

```plaintext
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport trunk allowed vlan add 30
Switch(config-if)# exit
```

#### Supprimer un VLAN du port trunk

```plaintext
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport trunk allowed vlan remove 20
Switch(config-if)# exit
```

#### Réinitialiser la liste des VLANs autorisés

```plaintext
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport trunk allowed vlan all
Switch(config-if)# exit
```

---

### 4. Supprimer le filtrage VLAN

Pour supprimer complètement le filtrage sur un port trunk, utilisez :

```plaintext
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# no switchport trunk allowed vlan
Switch(config-if)# exit
```

Cette commande permet de désactiver les restrictions de VLAN sur le port trunk.

---

### 5. Tester la configuration

- **Vérifier la connectivité entre les VLANs autorisés :**

  - Depuis un appareil dans le VLAN 10, testez la connectivité avec un appareil dans le VLAN 20 à l'aide de la commande `ping`.

- **Simuler des restrictions :**

  - Assurez-vous qu'un appareil d'un VLAN non autorisé ne peut pas communiquer via le port trunk.

---

## Conclusion

Le filtrage VLAN est une méthode puissante pour améliorer la gestion et la sécurité des réseaux segmentés. En limitant les VLANs autorisés sur les ports trunk, vous contrôlez efficacement le flux de trafic et réduisez les risques de communication non souhaitée.