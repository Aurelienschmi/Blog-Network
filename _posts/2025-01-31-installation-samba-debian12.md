---
title: "Configuration d’un espace de stockage sécurisé avec SFTP sous Debian 12"
date: 2025-01-31 12:00:00 +0100
tags: ["SFTP", "Sécurité", "Stockage", "Debian", "Serveur"]
categories: ["Networking", "Systèmes"]
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/sftp.png
---

## Introduction

Le protocole **SFTP (Secure File Transfer Protocol)** est une solution sécurisée pour transférer des fichiers sur un réseau en utilisant SSH. Contrairement à FTP, qui est souvent non chiffré, SFTP assure la confidentialité et l'intégrité des données échangées. Cet article vous guide dans l'installation et la configuration d'un serveur **SFTP** sous **Debian 12**, permettant de gérer un espace de stockage sécurisé.

---

## Pré-requis

Avant de commencer, assurez-vous d'avoir :

- Une machine Debian 12 (serveur SFTP).
- Un accès administrateur ou **root**.
- Une connexion réseau opérationnelle.
- Un client SFTP (FileZilla, WinSCP ou la commande `sftp`).

---

## Étapes détaillées

### 1. Mise à jour du système

Avant toute installation, mettez votre système à jour :

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2. Vérification et activation de SSH

SFTP est inclus avec **OpenSSH**. Vérifiez si **SSH** est installé et actif :

```bash
sudo systemctl status ssh
```

Si SSH n'est pas installé, installez-le avec :

```bash
sudo apt install openssh-server -y
```

Puis, démarrez et activez le service :

```bash
sudo systemctl enable --now ssh
```

---

### 3. Création d'un groupe et d'utilisateurs pour SFTP

Pour renforcer la sécurité, nous allons créer un groupe spécifique **sftpusers** et ajouter des utilisateurs dédiés.

```bash
sudo groupadd sftpusers
```

Ajoutez un utilisateur **sftpuser** au groupe et attribuez-lui un mot de passe :

```bash
sudo useradd -m -d /home/sftpuser -g sftpusers -s /sbin/nologin sftpuser
sudo passwd sftpuser
```

Le shell **/sbin/nologin** empêche l'utilisateur d'accéder au serveur en SSH.

---

### 4. Configuration de SSH pour restreindre l'accès à SFTP

Modifiez le fichier **/etc/ssh/sshd_config** :

```bash
sudo nano /etc/ssh/sshd_config
```

Ajoutez ces lignes à la fin du fichier :

```plaintext
Match Group sftpusers
ChrootDirectory /home/%u
ForceCommand internal-sftp
AllowTcpForwarding no
X11Forwarding no
```

Ces paramètres :
- **Restreignent les utilisateurs du groupe `sftpusers` à leur répertoire personnel**.
- **Forcent l'utilisation du mode `internal-sftp` pour désactiver l'accès SSH**.
- **Désactivent le transfert de ports et le transfert X11 pour plus de sécurité**.

> **Astuce :** Vous pouvez accéder à tout les paramètres de configuration à cet url: https://man7.org/linux/man-pages/man5/sshd_config.5.html
{: .notice--info}


Redémarrez le service SSH pour appliquer les changements :

```bash
sudo systemctl restart ssh
```

---

### 5. Définition des permissions et sécurisation

Définissez les permissions pour sécuriser l'accès :

```bash
sudo chown root:root /home/sftpuser
sudo chmod 755 /home/sftpuser
sudo mkdir -p /home/sftpuser/data
sudo chown sftpuser:sftpusers /home/sftpuser/data
sudo chmod 700 /home/sftpuser/data
```

- **Le dossier `/home/sftpuser` appartient à root pour empêcher toute modification**.
- **Les fichiers doivent être stockés dans `/home/sftpuser/data` accessible par l'utilisateur SFTP**.

---

### 6. Tester la connexion SFTP

#### a. Depuis un client Linux

Utilisez la commande :

```bash
sftp sftpuser@<IP_DU_SERVEUR>
```

Entrez le mot de passe et testez l'envoi d'un fichier :

```bash
put mon_fichier.txt
```

#### b. Depuis Windows (FileZilla/WinSCP)

1. **Ouvrez FileZilla** et entrez :
   - Hôte : `sftp://<IP_DU_SERVEUR>`
   - Nom d'utilisateur : `sftpuser`
   - Mot de passe : (celui défini précédemment)
2. Connectez-vous et testez le transfert de fichiers.

---

### 7. Supprimer ou gérer SFTP

#### a. Supprimer un utilisateur SFTP

```bash
sudo deluser --remove-home sftpuser
```

#### b. Supprimer complètement OpenSSH

```bash
sudo apt remove --purge openssh-server -y
```

---

## Conclusion

L'utilisation de **SFTP** est une solution efficace et sécurisée pour le transfert de fichiers. En configurant correctement les permissions et en restreignant l'accès, vous améliorez la sécurité des données stockées.
