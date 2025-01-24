---
title: "Installer un serveur LAMP (Linux Apache MariaDB PHP) sous Debian 12"
date: 2025-01-24 12:00:00 +0100
tags: ["LAMP", "Debian", "Serveur", "Réseaux"]
categories: ["Systèmes"]
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/lamp-debian-12.png
---

## Introduction

Le serveur LAMP (Linux, Apache, MariaDB, PHP) est une pile logicielle open-source largement utilisée pour héberger des applications web dynamiques. Dans cet article, nous vous guiderons pas à pas pour installer un serveur LAMP sur **Debian 12**, tout en expliquant comment le configurer, le gérer, et le désinstaller si nécessaire.

---

## Pré-requis

Avant de commencer, assurez-vous d'avoir :

- Une machine ou une VM Debian 12 fraîchement installée.
- Un accès administrateur ou des droits **root**.
- Une connexion internet pour télécharger les paquets nécessaires.
- Une compréhension de base de la ligne de commande Linux.

---

## Étapes détaillées

### 1. Mettre à jour le système

Avant toute installation, assurez-vous que votre système est à jour :

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2. Installer Apache

Apache est le serveur web utilisé pour gérer les requêtes HTTP.

#### a. Installation d'Apache

```bash
sudo apt install apache2 -y
```

#### b. Vérifier l'état du service Apache

```bash
sudo systemctl status apache2
```

Le service Apache doit être actif. Vous pouvez également tester en accédant à l'adresse IP de votre serveur dans un navigateur (exemple : `http://<adresse_IP_du_serveur>`).

#### c. Gérer le pare-feu pour Apache

Ouvrez le port 80 (HTTP) et 443 (HTTPS) avec UFW :

```bash
sudo ufw allow 'Apache Full'
sudo ufw enable
sudo ufw status
```

---

### 3. Installer MariaDB

MariaDB est un système de gestion de bases de données open-source.

#### a. Installation de MariaDB

```bash
sudo apt install mariadb-server mariadb-client -y
```

#### b. Sécuriser l'installation MariaDB

Exécutez le script de sécurisation intégré :

```bash
sudo mysql_secure_installation
```

Répondez aux questions pour :
- Définir un mot de passe root.
- Supprimer les utilisateurs anonymes.
- Désactiver les connexions root à distance.
- Supprimer la base de test.

#### c. Vérifier l'état du service MariaDB

```bash
sudo systemctl status mariadb
```

---

### 4. Installer PHP

PHP est le langage de script utilisé pour générer du contenu web dynamique.

#### a. Installation de PHP et des modules courants

```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

#### b. Tester PHP

Créez un fichier de test dans le répertoire web d'Apache :

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

Accédez à `http://<adresse_IP_du_serveur>/info.php` pour vérifier les détails de l'installation PHP.

> **Note :** Supprimez ce fichier après le test pour des raisons de sécurité.

```bash
sudo rm /var/www/html/info.php
```

---

### 5. Supprimer ou gérer les composants LAMP

#### a. Supprimer un composant spécifique

- Pour supprimer Apache :

```bash
sudo apt remove apache2 -y
```

- Pour supprimer MariaDB :

```bash
sudo apt remove mariadb-server mariadb-client -y
```

- Pour supprimer PHP :

```bash
sudo apt remove php libapache2-mod-php php-mysql -y
```

#### b. Supprimer l'ensemble de la pile LAMP

```bash
sudo apt purge apache2 mariadb-server php* -y
sudo apt autoremove -y
```

---

### 6. Étendre la configuration (ajouts post-installation)

- **Activer SSL avec Apache** :

```bash
sudo a2enmod ssl
sudo systemctl restart apache2
```

- **Installer des modules PHP supplémentaires** :

```bash
sudo apt install php-curl php-gd php-xml php-mbstring -y
```

- **Créer une base de données avec MariaDB** :

```bash
sudo mysql -u root -p
CREATE DATABASE mon_site;
CREATE USER 'mon_utilisateur'@'localhost' IDENTIFIED BY 'mon_mot_de_passe';
GRANT ALL PRIVILEGES ON mon_site.* TO 'mon_utilisateur'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## Conclusion

Installer un serveur LAMP sur Debian 12 est une étape essentielle pour héberger des sites ou des applications web dynamiques. Ce guide vous a montré comment installer, configurer et même supprimer les composants nécessaires. En explorant davantage les modules et les configurations avancées, vous pouvez personnaliser votre serveur pour répondre à vos besoins spécifiques.

