---
title: "Création d'une VLAN sur Cisco Packet Tracer"
date: 2024-12-17 19:00:00 +0100
tags: [Cisco, VLAN, Réseaux, Packet Tracer]
categories: [Configuration, Réseaux]
description: "Tutoriel détaillé pour configurer une VLAN sur Cisco Packet Tracer."
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/vlan.png
---

# Création d'une VLAN sur Cisco Packet Tracer

## Introduction

Les VLAN (**Virtual Local Area Networks**) permettent de segmenter un réseau local pour améliorer ses performances, sa sécurité et sa flexibilité. Dans ce tutoriel, nous allons apprendre à configurer une VLAN sur Cisco Packet Tracer. Ce processus est essentiel pour les professionnels réseau.

---

## Pré-requis

Avant de commencer, assurez-vous d'avoir les éléments suivants :

1. **Logiciel** : Cisco Packet Tracer (version récente).
2. **Connaissances de base** : Notions sur les réseaux et l'utilisation de l'interface CLI.
3. **Configuration matérielle** :
   - Un switch manageable (ex. Cisco 2960).
   - Deux PC pour tester les communications.

---

## Étapes détaillées

### Étape 1 : Créer une topologie de réseau

1. **Ajoutez les équipements** dans Cisco Packet Tracer :
   - Un switch manageable (ex. Cisco 2960).
   - Deux PC pour tester les communications.

2. **Connectez les équipements** :
   - Utilisez des câbles droits entre le switch et les PC.

---

### Étape 2 : Configurer une VLAN sur le switch

1. **Accédez au switch via CLI** :

   ```plaintext
   Switch> enable
   Switch# configure terminal
   ```

2. **Créez une VLAN** :

   ```plaintext
   Switch(config)# vlan 10
   Switch(config-vlan)# name VLAN_TEST
   Switch(config-vlan)# exit
   ```

3. **Attribuez les ports à la VLAN** :

   ```plaintext
   Switch(config)# interface fastEthernet 0/1
   Switch(config-if)# switchport mode access
   Switch(config-if)# switchport access vlan 10
   Switch(config-if)# exit
   
   Switch(config)# interface fastEthernet 0/2
   Switch(config-if)# switchport mode access
   Switch(config-if)# switchport access vlan 10
   Switch(config-if)# exit
   ```

> **Remarque :** Si vous voulez passer plusieurs ports en même temps dans un VLAN, même si ces ports ne sont pas les un à la suite des autres, vous pouvez utiliser la commande suivante :
```plaintext
   Switch(config)# interface range fastEthernet 0/1-2
   ```
{: .prompt-tip }

4. **Vérifiez la configuration** :

   ```plaintext
   Switch# show vlan brief
   ```

   Cette commande affiche les VLAN configurées sur le switch et les ports associés.

> **Remarque :** Pour plusieurs VLAN, répétez les étapes en modifiant le numéro de VLAN et les ports associés.
{: .prompt-info }

---

### Étape 3 : Tester la configuration

1. **Attribuez des adresses IP aux PC** :
   - PC1 : 192.168.10.1/24
   - PC2 : 192.168.10.2/24

2. **Testez la connectivité entre les PC** :

   - Ouvrez l'invite de commande sur PC1 et envoyez un ping à PC2 :

     ```plaintext
     C:\> ping 192.168.10.2
     ```

   - Si la configuration est correcte, vous devriez voir des réponses positives.

> **Astuce :** Si le test échoue, vérifiez la configuration des VLAN et des ports sur le switch.
{: .prompt-tip }

---

## Conclusion

Dans ce tutoriel, nous avons appris à configurer une VLAN sur Cisco Packet Tracer, incluant la création de la topologie, la configuration des VLAN sur un switch, et les tests de connectivité. Cette compétence est fondamentale pour segmenter un réseau et améliorer sa gestion.

Pour aller plus loin, explorez les VLANs tronquées (trunk VLAN) pour connecter plusieurs switches ensemble.

<!-- Ajoutez une image ici si besoin pour illustrer la topologie à l'aide de Cisco Packet Tracer -->

