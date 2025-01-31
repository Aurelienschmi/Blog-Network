---
title: "Installer un serveur SMB (Samba) sous Debian 12"
date: 2025-01-31 12:00:00 +0100
tags: ["SMB", "Samba", "Debian", "Serveur", "Réseaux"]
categories: ["Networking", "Systèmes"]
pin: false
toc: true
math: false
mermaid: false
image: /assets/img/posts/smb.png
---

## Introduction

Le protocole **SMB (Server Message Block)**, implémenté par le logiciel **Samba**, permet de partager des fichiers et imprimantes entre systèmes Linux et Windows. C'est une solution efficace pour centraliser des ressources et permettre un accès réseau sécurisé. Cet article détaille l'installation et la configuration de **Samba** sur **Debian 12**.

---

## Pré-requis

Avant de commencer, assurez-vous d'avoir :

- Une machine Debian 12 (serveur SMB).
- Un accès administrateur ou **root**.
- Une connexion réseau opérationnelle.
- Un autre système (Linux/Windows) pour tester le partage.

---

## Étapes détaillées

### 1. Mise à jour du système

Avant toute installation, mettez votre système à jour :

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2. Installation de Samba

Samba est disponible dans les dépôts officiels de Debian.

```bash
sudo apt install samba -y
```

Après l'installation, vérifiez que le service est actif :

```bash
sudo systemctl status smbd
```

Si le service n'est pas actif, démarrez-le et activez-le au démarrage :

```bash
sudo systemctl enable --now smbd
```
> **Astuce :** Vous pouvez passer en mode root en utilisant la commande suivante :
>
> ```bash
> su -
> ```
> Entrez le mot de passe root lorsqu'il est demandé.
{: .prompt-tip }
---

### 3. Configuration de Samba

L'ensemble des paramètres de Samba est défini dans le fichier **`/etc/samba/smb.conf`**.

Avant toute modification, sauvegardez le fichier original :

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
```

#### a. Partage d'un répertoire public

Modifiez le fichier de configuration :

```bash
sudo nano /etc/samba/smb.conf
```

Ajoutez les lignes suivantes à la fin du fichier :

```plaintext
[partage]
   comment = Partage de données
   path = /srv/partage
   guest ok = no
   read only = no
   browseable = yes
   valid users = @partage
```

###Quelques explications :

- [partage] : sert à spécifier le nom du partage entre "[]", c'est le nom qui devra être utilisé pour accéder au partage
   - comment : description du partage
   - path : chemin vers le dossier à partager, sur le serveur
   - guest ok : accès invité au partage (par défaut "no"). Si vous décidez d'activer cette option, vous devez configurer l'option "guest account" qui par défaut prend la valeur "nobody".
   - read only : partage accessible uniquement en lecture seule (yes ou no)
   - browseable : le partage doit-il être visible ou masqué si on liste les partages du serveur avec un hôte distant (découverte réseau). La valeur "yes" permet de le rendre visible.
   - valid users : spécifier les utilisateurs ou les groupes qui ont les droits d'accès au partage (les droits sur le système de fichiers doivent être cohérents vis-à-vis de cette autorisation). On précise un utilisateur avec son identifiant et un groupe avec son identifiant précédé du caractère "@". Pour indiquer plusieurs valeurs, séparez-les par une virgule.

Créez le dossier partagé et définissez les permissions :

```bash
sudo mkdir -p /srv/samba/public
sudo chmod 777 /srv/samba/public
```

Redémarrez Samba pour appliquer les modifications :

```bash
sudo systemctl restart smbd
```

#### b. Partage sécurisé avec authentification

Ajoutez un utilisateur Samba :

```bash
sudo smbpasswd -a utilisateur
```

Modifiez **`smb.conf`** et ajoutez un partage privé :

```plaintext
[PartagePrive]
   path = /srv/samba/prive
   read only = no
   valid users = utilisateur
   create mask = 0700
   directory mask = 0700
```

Créez le dossier et définissez les permissions :

```bash
sudo mkdir -p /srv/samba/prive
sudo chown utilisateur:utilisateur /srv/samba/prive
sudo chmod 700 /srv/samba/prive
```

Appliquez les modifications :

```bash
sudo systemctl restart smbd
```

---

### 4. Accès aux partages Samba

#### a. Depuis Windows

1. Ouvrez **Explorateur de fichiers**.
2. Dans la barre d'adresse, entrez `\\<IP_DU_SERVEUR>`.
3. Accédez aux dossiers publics ou connectez-vous avec les identifiants Samba.

#### b. Depuis Linux

Montez le partage public :

```bash
sudo mount -t cifs //192.168.1.100/PartagePublic /mnt -o guest
```

Montez le partage privé avec authentification :

```bash
sudo mount -t cifs //192.168.1.100/PartagePrive /mnt -o username=utilisateur
```

---

### 5. Supprimer ou gérer Samba

#### a. Supprimer Samba complètement

```bash
sudo apt remove --purge samba -y
sudo rm -rf /etc/samba /var/lib/samba
```

#### b. Désactiver temporairement Samba

```bash
sudo systemctl stop smbd
sudo systemctl disable smbd
```

---

## Conclusion

Samba est une solution puissante pour le partage de fichiers entre Linux et Windows. Que ce soit pour des dossiers accessibles à tous ou des partages sécurisés, cette configuration permet de créer un serveur SMB fonctionnel sur Debian 12.


