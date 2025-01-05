---
title: "Configurer une VLAN Native avec Port Trunk sur Cisco Packet Tracer"
date: 2025-01-04 19:00:00 +0100
tags: [Cisco, VLAN, Trunk, Réseaux, Packet Tracer]
categories: [Configuration, Réseaux]
description: "Comprendre et mettre en place une VLAN native avec un port trunk sur Cisco Packet Tracer."
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/vlan-native.png
---


# Configurer une VLAN Native avec Port Trunk sur Cisco Packet Tracer

## Introduction

Dans un réseau moderne, les VLANs (Virtual Local Area Networks) jouent un rôle crucial dans la segmentation et la gestion efficace du trafic réseau. Une VLAN native est utilisée pour gérer le trafic non tagué sur un port trunk. La configuration correcte d'une VLAN native améliore la sécurité, limite les conflits de configuration et garantit une meilleure gestion des ressources réseau.

Dans cet article, nous expliquerons comment créer une VLAN native avec un port trunk dans Cisco Packet Tracer, tout en détaillant les alternatives possibles et les raisons d'utiliser une telle configuration.

## Pré-requis

### Outils nécessaires

- **Logiciel** : Cisco Packet Tracer
- **Compétences** :
  - Connaissance de base des VLANs
  - Familiarité avec l'interface CLI (Command Line Interface) des équipements Cisco

### Topologie réseau minimale

- **Un switch capable de gérer les VLANs**
- **Deux PCs connectés au switch**
- **Un routeur (optionnel) pour tester le routage inter-VLAN**

### Terminologie

- **VLAN Native** : VLAN par défaut utilisée pour le trafic non tagué sur un port trunk.
- **Port Trunk** : Port configuré pour transporter plusieurs VLANs entre des équipements réseau.

## Étapes détaillées

### 1. Créer une nouvelle VLAN Native

1. **Accéder à l'interface CLI du switch** :
   - Cliquez sur le switch dans Cisco Packet Tracer.
   - Sélectionnez l'onglet CLI pour accéder à la console.

2. **Entrer en mode de configuration globale** :

   ```plaintext
   Switch> enable
   Switch# configure terminal
   ```

3. **Créer la VLAN** :

   ```plaintext
   Switch(config)# vlan 100
   Switch(config-vlan)# name VLAN_Native
   Switch(config-vlan)# exit
   ```

4. **Configurer le port trunk et assigner la VLAN Native** :

   ```plaintext
   Switch(config)# interface GigabitEthernet0/1
   Switch(config-if)# switchport trunk native vlan 100
   Switch(config-if)# switchport mode trunk
   Switch(config-if)# end
   ```

### 2. Vérifier la configuration

1. **Lister les VLANs configurées** :

   ```plaintext
   Switch# show vlan brief
   ```

2. **Vérifier la configuration du port trunk** :

   ```plaintext
   Switch# show running-config interface GigabitEthernet0/1
   ```

### 3. Alternatives et bonnes pratiques

- **Utiliser les ports d'accès** si le trunking n'est pas requis, bien que cela limite le transport des VLANs.
- Toujours tester les configurations avec des outils comme `ping` pour valider la connectivité entre les équipements.

### 4. Tester la connectivité

1. **Attribuez des adresses IP aux PCs dans les sous-réseaux correspondants.**

2. **Effectuez un test de ping entre les PCs pour vérifier la communication.**

   ```plaintext
   PC> ping 192.168.1.2
   ```

3. **Optionnel : Configurer un routeur pour le routage inter-VLAN.**

   Exemple de configuration :

   ```plaintext
   Router> enable
   Router# configure terminal
   Router(config)# interface GigabitEthernet0/0.10
   Router(config-subif)# encapsulation dot1Q 10
   Router(config-subif)# ip address 192.168.1.1 255.255.255.0
   Router(config-subif)# exit
   Router(config)# interface GigabitEthernet0/0.20
   Router(config-subif)# encapsulation dot1Q 20
   Router(config-subif)# ip address 192.168.2.1 255.255.255.0
   Router(config-subif)# end
   ```

## Conclusion

Configurer une VLAN native avec un port trunk est une étape essentielle pour une gestion réseau efficace. Cela permet de transporter plusieurs VLANs sur une seule liaison physique, tout en optimisant la sécurité et la performance.


