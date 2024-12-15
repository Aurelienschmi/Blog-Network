---
title: "Configuration de Base d'un switch Cisco : Guide Complet"
date: 2024-12-14 10:00:00 +0100
categories: [Configuration, Switch]
tags: [Switch, Configuration, Cisco]
pin: false
toc: true
math: false
mermaid: false
image: ../assets/img/posts/switch-cisco.png
---

Dans ce guide, nous allons explorer les étapes pour configurer un switch Cisco en utilisant **Packet Tracer**, un outil de simulation réseau populaire. Ce guide est idéal pour les débutants souhaitant comprendre les bases.

---

## Objectifs

1. Connexion au switch via la console.
2. Suppression de l'ancienne configuration.
3. Configuration basique du switch.
4. Configuration des accès (console, telnet).
5. Configuration d'une adresse IP.
6. Configuration de SSH pour un accès sécurisé.
7. Enregistrement de la configuration.
8. Vérification et tests.

---

## Matériel Requis

- **Cisco Packet Tracer** installé sur votre ordinateur.
- Un projet Packet Tracer avec au moins un routeur et un PC connectés.

ou pour un switch physique :
- **Un logiciel terminal** comme Putty, Tera Term ou la console intégrée de Packet Tracer.
---

## Étapes de Configuration

### **1. Connexion au Switch**
#### A. Avec Packet Tracer
- Ouvrez Cisco Packet Tracer.
- Ajoutez un switch (par exemple, un Cisco 2960).
- Cliquez sur le switch et accédez à l'onglet **CLI**.

#### B. Avec un Switch Physique
- Connectez un câble console entre votre PC et le switch.
- Configurez le port série :
  - **Baud rate** : 9600
  - **Data bits** : 8
  - **Parity** : None
  - **Stop bits** : 1

---

### **2. Suppression de l'ancienne configuration**
Pour repartir de zéro :
```plaintext
Switch> enable
Switch# write erase
Switch# reload
```
---
### **3. Configuration Basique**
#### A. Entrer en Mode Privilégié
```plaintext
Switch> enable
Switch# configure terminal
```
#### B. Définir le Nom de l'Appareil
```plaintext
Switch(config)# hostname NomDuSwitch
```
#### C. Désactiver la Recherche DNS
```plaintext
Switch-Bureau(config)# no ip domain-lookup
```

#### D. Définir le Mot de Passe de l'Accès en Mode Privilégié
```plaintext
Switch-Bureau(config)# enable secret motdepasse123
```
---
### **4. Configuration des Accès**
#### A. Mot de Passe popur la Console
```plaintext
Switch-Bureau(config)# line console 0
Switch-Bureau(config-line)# password console123
Switch-Bureau(config-line)# login
Switch-Bureau(config-line)# exit
```

#### B. Mot de Passe pour Telnet
```plaintext
Switch-Bureau(config)# line vty 0 4
Switch-Bureau(config-line)# password telnet123
Switch-Bureau(config-line)# login
Switch-Bureau(config-line)# exit
```
---

### **5. Configurer une Adresse IP**
#### A. Configurer l'Adresse IP de l'Interface VLAN 1
```plaintext
Switch-Bureau(config)# interface vlan 1
Switch-Bureau(config-if)# ip address 192.168.1.2 255.255.255.0
Switch-Bureau(config-if)# no shutdown
Switch-Bureau(config-if)# exit
```
---

### **6. Configurer SSH pour un Accès Sécurisé**
#### A. Définir un Nom de Domaine
```plaintext
Switch-Bureau(config)# ip domain-name mondomaine.com
```

#### B. Générer une Clé RSA
```plaintext
Switch-Bureau(config)# crypto key generate rsa
```

#### C. Configurer SSH
```plaintext
Switch-Bureau(config)# line vty 0 4
Switch-Bureau(config-line)# transport input ssh
Switch-Bureau(config-line)# exit
```

#### D. Créer un Utilisateur pour SSH
```plaintext
Switch-Bureau(config)# username admin privilege 15 secret admin123
```

---
### **7. Enregistrer la Configuration**
```plaintext
Switch-Bureau# copy running-config startup-config
```
ou
```plaintext
Switch-Bureau# write memory
```

---
### **8. Vérification et Tests**
#### A. Afficher la Configuration Actuelle
```plaintext
Switch-Bureau# show running-config
```

#### B. Tester la Connectivité Réseau
```plaintext
Switch-Bureau# ping 192.168.1.2
```

---
### Conclusion
Vous connaissez maintenant les bases de la configuration d'un switch Cisco. N'hésitez pas à explorer d'autres fonctionnalités pour renforcer vos compétences en administration réseau !