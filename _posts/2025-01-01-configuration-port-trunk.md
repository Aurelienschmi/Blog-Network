---
title: "Création d'un lien TRUNK sur Cisco Packet Tracer"
date: 2025-01-01 19:00:00 +0100
tags: [Cisco, VLAN, Trunk, Réseaux, Packet Tracer]
categories: [Configuration, Réseaux]
description: "Tutoriel détaillé pour configurer un lien TRUNK sur Cisco Packet Tracer."
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/trunk.png
---

## Introduction

Dans un réseau informatique, un lien **TRUNK** est essentiel pour permettre la communication entre plusieurs VLAN configurés sur différents switchs. Cet article détaille les étapes pour configurer un lien TRUNK entre deux switchs dans Cisco Packet Tracer, avec un scénario comprenant 4 PC et 2 VLAN.

## Pré-requis

Avant de commencer, assurez-vous d’avoir les éléments suivants :

- **Logiciel Cisco Packet Tracer** installé.
- Connaissances de base sur les VLAN et les commandes CLI Cisco.
- Une topologie réseau comprenant :
  - 2 switchs (par exemple, Cisco 2960).
  - 4 PC connectés aux switchs via des câbles droits.

## Étapes détaillées

### Étape 1 : Configuration de la topologie

1. Placez **2 switchs** dans la zone de travail de Packet Tracer.
2. Ajoutez **4 PC**, deux pour chaque switch.
3. Connectez chaque PC à un port des switchs avec des câbles droits.
4. Reliez les deux switchs avec un câble croisé sur les ports **GigabitEthernet 0/1**.

### Étape 2 : Création des VLAN

1. **Accédez au CLI du premier switch :**

   ```plaintext
   Switch> enable
   Switch# configure terminal
   ```

2. **Créez les VLAN 10 et VLAN 20 :**

   ```plaintext
   Switch(config)# vlan 10
   Switch(config-vlan)# name VLAN10
   Switch(config-vlan)# exit

   Switch(config)# vlan 20
   Switch(config-vlan)# name VLAN20
   Switch(config-vlan)# exit
   ```

3. **Attribuez les ports aux VLAN :**

   - Pour VLAN 10 :

     ```plaintext
     Switch(config)# interface FastEthernet 0/1
     Switch(config-if)# switchport mode access
     Switch(config-if)# switchport access vlan 10
     Switch(config-if)# exit
     ```

   - Pour VLAN 20 :

     ```plaintext
     Switch(config)# interface FastEthernet 0/2
     Switch(config-if)# switchport mode access
     Switch(config-if)# switchport access vlan 20
     Switch(config-if)# exit
     ```

>Répétez ces étapes pour le deuxième switch.
{: .prompt-warning }

### Étape 3 : Configuration du lien TRUNK

1. **Accédez au CLI du premier switch et configurez le port TRUNK :**

   ```plaintext
   Switch(config)# interface GigabitEthernet 0/1
   Switch(config-if)# switchport mode trunk
   Switch(config-if)# switchport trunk allowed vlan 10,20
   Switch(config-if)# exit
   ```

2. **Répétez la configuration sur le deuxième switch :**

   ```plaintext
   Switch(config)# interface GigabitEthernet 0/1
   Switch(config-if)# switchport mode trunk
   Switch(config-if)# switchport trunk allowed vlan 10,20
   Switch(config-if)# exit
   ```

### Étape 4 : Configuration des adresses IP sur les PC

1. **Attribuez une adresse IP à chaque PC selon son VLAN :**

   - Pour les PC dans VLAN 10 :
     - **PC1** : 192.168.10.2/24
     - **PC2** : 192.168.10.3/24
   - Pour les PC dans VLAN 20 :
     - **PC3** : 192.168.20.2/24
     - **PC4** : 192.168.20.3/24

2. Configurez l’adresse IP dans les paramètres de bureau de chaque PC :
   - Allez dans **Desktop > IP Configuration**.
   - Entrez l’adresse IP et le masque de sous-réseau appropriés.

### Étape 5 : Vérification de la configuration

1. **Vérifiez les VLAN sur chaque switch :**

   ```plaintext
   Switch# show vlan brief
   ```

2. **Testez la connectivité :**
   - Ping entre les PC du même VLAN pour vérifier la communication.

   Exemple de commande depuis PC1 :

   ```plaintext
   > ping 192.168.10.3
   ```

3. Si le ping est réussi, la configuration est correcte.

## Conclusion

Vous avez maintenant configuré un lien TRUNK entre deux switchs, permettant la communication entre les VLAN 10 et 20. Cette méthode est fondamentale dans les environnements réseau où la segmentation est cruciale.

