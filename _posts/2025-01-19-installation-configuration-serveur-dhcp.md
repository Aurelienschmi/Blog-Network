---
title: "Installation et configuration d'un serveur DHCP sur Debian"
date: 2025-01-19 10:00:00 +0100
categories: [Systèmes]
tags: ["DHCP", "Debian", "Serveur", "Réseaux"]
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/serveur-dhcp-debian.png
---

## Introduction

Le protocole DHCP (Dynamic Host Configuration Protocol) est crucial pour simplifier la gestion des adresses IP au sein d'un réseau. Il permet d'attribuer automatiquement des adresses IP, des passerelles par défaut et des serveurs DNS aux périphériques connectés. Dans cet article, nous expliquerons comment installer et configurer un serveur DHCP sur Debian en ligne de commande en utilisant **isc-dhcp-server**, puis comment tester cette configuration sur une autre machine.

## Pré-requis

Avant de commencer, assurez-vous d'avoir :

- **Deux machines Debian minimalistes** : une pour le serveur DHCP et une pour le client.
- **Privilèges administrateur (root)** sur les deux machines.
- **Connexion réseau** : les deux machines doivent être connectées au même réseau.
- **Logiciel isc-dhcp-server** pour le serveur DHCP.

> **Astuce :** Vous pouvez passer en mode root en utilisant la commande suivante :
>
> ```bash
> su
> ```
> Entrez le mot de passe root lorsqu'il est demandé.

---

## Étapes détaillées

### 1. Préparer la machine serveur DHCP

#### a. Mettre à jour le système

```bash
sudo apt update && sudo apt upgrade -y
```

#### b. Installer les paquets nécessaires

```bash
sudo apt install isc-dhcp-server -y
```

#### c. Configurer les permissions utilisateur (optionnel)

Si besoin, ajoutez votre utilisateur courant au fichier sudoers pour lui donner les droits root :

```bash
sudo apt install sudo
sudo nano /etc/sudoers
```

Ajoutez cette ligne si elle n'est pas présente :

```plaintext
<nom_utilisateur> ALL=(ALL:ALL) ALL
```

#### d. Configurer l'adresse réseau statique sur le serveur DHCP

Modifiez le fichier `/etc/network/interfaces` :

```bash
sudo nano /etc/network/interfaces
```

Ajoutez la configuration suivante :

```plaintext
auto ens33
iface ens33 inet static
    address 192.168.200.254
    netmask 255.255.255.0
    network 192.168.200.0
    broadcast 192.168.200.255
    gateway 192.168.200.1
```

##### Explication des paramètres :
- **address** : Adresse IP statique pour le serveur DHCP (par exemple, `192.168.200.254`).
- **netmask** : Masque de sous-réseau utilisé dans votre réseau (par exemple, `255.255.255.0`).
- **network** : Adresse du réseau (par exemple, `192.168.200.0`).
- **broadcast** : Adresse de diffusion (par exemple, `192.168.200.255`).
- **gateway** : Passerelle par défaut, souvent l'adresse IP du routeur (par exemple, `192.168.200.1`).

##### Comment obtenir ces informations ?
1. **Adresse actuelle de l'interface** :
   ```bash
   ip addr show ens33
   ```
   Repérez l'adresse actuelle et le masque associé.

2. **Passerelle par défaut** :
   ```bash
   ip route | grep default
   ```
   Cela affichera la passerelle configurée.

3. **Adresse de diffusion** :
   Vous pouvez la déduire en combinant l'adresse réseau et le masque ou utiliser la commande `ip addr` pour la vérifier directement.

---

### 2. Configurer le fichier DHCP

#### a. Sauvegarder la configuration existante

Avant de modifier les fichiers, créez une sauvegarde :

```bash
sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.backup
```

#### b. Modifier le fichier de configuration DHCP

Éditez le fichier `/etc/dhcp/dhcpd.conf` :

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Ajoutez ou modifiez les lignes suivantes pour configurer le réseau :

```plaintext
authoritative;

subnet 192.168.200.0 netmask 255.255.255.0 {
    range 192.168.200.200 192.168.200.210;
    option subnet-mask 255.255.255.0;
    option routers 192.168.200.254;
    option broadcast-address 192.168.200.255;
    option domain-name "m2i.local";
    option domain-name-servers 192.168.200.100;
    default-lease-time 600;
    max-lease-time 7200;
}

# Réservation d'adresses fixes
host srv-lx01 {
    hardware ethernet 08:0c:29:83:19:95;
    fixed-address 192.168.200.100;
}

host srv-lx02 {
    hardware ethernet 00:0c:29:e2:12:3c;
    fixed-address 192.168.200.102;
}
```

#### c. Configurer l'interface réseau utilisée par DHCP

Éditez le fichier `/etc/default/isc-dhcp-server` :

```bash
sudo nano /etc/default/isc-dhcp-server
```

Ajoutez l'interface réseau (par exemple `ens33`) :

```plaintext
INTERFACESv4="ens33"
```

---

### 3. Démarrer le serveur DHCP

#### a. Lancer et activer le service DHCP

```bash
sudo systemctl start isc-dhcp-server
sudo systemctl enable isc-dhcp-server
```

#### b. Vérifier l'état du service

```bash
sudo systemctl status isc-dhcp-server
```

Si des erreurs sont signalées, utilisez cette commande pour analyser les logs :

```bash
sudo tail -f /var/log/syslog
```

---

### 4. Tester le serveur DHCP

#### a. Configurer la machine client pour utiliser DHCP

Modifiez le fichier `/etc/network/interfaces` sur la machine cliente :

```bash
sudo nano /etc/network/interfaces
```

Ajoutez :

```plaintext
auto ens33
iface ens33 inet dhcp
```

#### b. Redémarrer le service réseau du client

```bash
sudo systemctl restart networking
```

#### c. Vérifier l'adresse IP obtenue

Exécutez la commande suivante sur le client pour afficher l'adresse IP :

```bash
ip addr show ens33
```

Vous devriez voir une adresse comprise entre `192.168.200.200` et `192.168.200.210`.

---

## Conclusion

Ce guide vous a montré comment installer et configurer un serveur DHCP sur Debian, y compris la configuration d'une adresse statique pour le serveur et les tests sur une machine cliente. Un serveur DHCP est un outil indispensable pour automatiser la gestion des adresses IP dans un réseau. Continuez à explorer les paramètres avancés pour mieux comprendre et maîtriser cet outil !

---

**Note** : N'hésitez pas à intégrer des captures d'écran ou des schémas pour illustrer votre configuration.
