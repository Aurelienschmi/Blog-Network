---
title: "Configuration de Base d'un Routeur Cisco : Guide Complet"
date: 2024-12-14 15:30:00 +0100
categories: [Configuration, Routeur]
tags: [Routeur, Cisco, Configuration]
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/routeur-cisco.png
---

Dans ce guide, nous allons explorer les étapes pour configurer un routeur Cisco en utilisant **Packet Tracer**, un outil de simulation réseau populaire. Ce guide est idéal pour les débutants souhaitant comprendre les bases.

---

## Objectifs

1. Accéder au routeur via la console dans Packet Tracer.
2. Configurer un nom d'hôte et des mots de passe.
3. Configurer les interfaces.
4. Ajouter une route statique.
5. Sauvegarder la configuration.
6. Vérifier la configuration.

---

## Matériel Requis

- Cisco Packet Tracer installé sur votre ordinateur.
- Un projet Packet Tracer avec au moins un routeur et un PC connectés.

---

## Étapes de Configuration

### **1. Accéder au Routeur dans Packet Tracer**

1. Ajoutez un routeur (ex. : Router-PT) dans votre espace de travail.
2. Connectez un PC au routeur en utilisant un câble console.
3. Cliquez sur le PC, puis sur **Terminal** dans l'onglet Desktop.
4. Configurez les paramètres de terminal comme suit :
   - **Baud Rate** : 9600
   - **Data Bits** : 8
   - **Parity** : None
   - **Stop Bits** : 1
   - **Flow Control** : None

>Vous accéderez alors à l'invite CLI du routeur.
{: .prompt-info }

---

### **2. Configurer un Nom d'Hôte et des Mots de Passe**

#### **A. Configurer un nom d'hôte**

```plaintext
Router> enable
Router# configure terminal
Router(config)# hostname Routeur01
Routeur01(config)#
```

#### **B. Configurer un mot de passe pour le mode privilégié**

```plaintext
Routeur01(config)# enable secret my_secure_password
```

#### **C. Configurer un mot de passe pour la ligne console**

```plaintext
Routeur01(config)# line console 0
Routeur01(config-line)# password console_password
Routeur01(config-line)# login
Routeur01(config-line)# exit
```

#### **D. Configurer un mot de passe pour les connexions Telnet ou SSH**

```plaintext
Routeur01(config)# line vty 0 4
Routeur01(config-line)# password vty_password
Routeur01(config-line)# login
Routeur01(config-line)# exit
```

#### **E. Activer SSH (Optionnel)**

```plaintext
Routeur01(config)# ip domain-name monreseau.local
Routeur01(config)# crypto key generate rsa
How many bits in the modulus [512]: 1024
Routeur01(config)# ip ssh version 2
Routeur01(config)# username admin privilege 15 secret admin_password
```


---

### **3. Configurer les Interfaces**

#### **A. Configurer une interface Ethernet**

1. Dans Packet Tracer, connectez un câble entre le routeur et un PC (ex. : FastEthernet 0/0 vers FastEthernet0 sur le PC).
2. Accédez à l'interface via la CLI :

```plaintext
Routeur01(config)# interface fastethernet0/0
Routeur01(config-if)# ip address 192.168.1.1 255.255.255.0
Routeur01(config-if)# no shutdown
Routeur01(config-if)# exit
```

>Configurez l'IP du PC connecté pour tester la communication.
{: .prompt-tip }

#### **B. Configurer une interface WAN**

Si votre routeur est connecté à un autre routeur via une interface série :

```plaintext
Routeur01(config)# interface serial0/0/0
Routeur01(config-if)# ip address 10.0.0.1 255.255.255.252
Routeur01(config-if)# clock rate 64000
Routeur01(config-if)# no shutdown
Routeur01(config-if)# exit
```

---

### **4. Ajouter une Route Statique**

Une route statique permet au routeur de diriger le trafic vers un réseau spécifique.

```plaintext
Routeur01(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

>Dans cet exemple :
- **192.168.2.0** est le réseau de destination.
- **255.255.255.0** est le masque de sous-réseau.
- **10.0.0.2** est l'adresse IP du routeur suivant (gateway).
{: .prompt-info }

---

### **5. Sauvegarder la Configuration**

Pour éviter de perdre la configuration après un redémarrage :

```plaintext
Routeur01# copy running-config startup-config
Destination filename [startup-config]? [Appuyez sur Entrée]
```

---

### **6. Vérifier la Configuration**

#### **A. Vérifier les Interfaces**

```plaintext
Routeur01# show ip interface brief
```

#### **A. Vérifier les Routes**

```plaintext
Routeur01# show ip route
```

---

## Conclusion

Vous connaissez maintenant les bases de la configuration d'un routeur Cisco. N'hésitez pas à explorer d'autres fonctionnalités pour renforcer vos compétences en administration réseau !