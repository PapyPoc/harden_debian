# 🔐 Debian Network & SSH Hardening Script

Script Bash pour **Debian uniquement** permettant :

-   Configuration d'une IP statique (`/etc/network/interfaces`)
-   Configuration des DNS (`/etc/resolv.conf`)
-   Validation DNS avec rollback automatique
-   Recréation complète et sécurisation de `/etc/ssh/sshd_config`
-   Ajout automatique d'une clé publique SSH
-   Configuration idempotente et sécurisée

------------------------------------------------------------------------

## 📦 Fonctionnalités

### 🌐 Réseau

-   Détection automatique de l'interface principale
-   Configuration IP statique
-   Conversion automatique CIDR → netmask
-   Redémarrage du service réseau
-   Backup automatique avant modification

### 🌍 DNS

-   Mise à jour de `/etc/resolv.conf`
-   Support :
    -   Écriture directe
    -   `resolvconf`
    -   `systemd-resolved`
-   Test automatique de résolution DNS
-   🔁 Rollback automatique si échec

### 🔒 SSH Hardening

-   Backup horodaté de `sshd_config`
-   Recréation complète du fichier
-   Désactivation :
    -   `PermitRootLogin`
    -   `PasswordAuthentication`
    -   `ChallengeResponseAuthentication`
-   Activation :
    -   Authentification par clé uniquement
-   Validation avec `sshd -t`
-   Ajout sécurisé de la clé SSH dans le bon utilisateur
-   Permissions conformes OpenSSH (700 / 600)

------------------------------------------------------------------------

## 🛠️ Prérequis

-   Debian 12 ou 13
-   `ifupdown`
-   OpenSSH installé
-   Exécution avec `sudo` ou en root

⚠️ Script **Debian uniquement** (pas Ubuntu / pas RHEL / pas
NetworkManager).

------------------------------------------------------------------------

## ⚙️ Configuration

Modifier les variables en haut du script :

    STATIC_IP="10.0.0.202/24"
    GATEWAY="10.0.0.254"
    DNS_LIST=("10.0.0.254" "1.1.1.1")
    SEARCH_DOMAINS=()
    SSH_PORT="22"
    CLE_SSH_P="ssh-ed25519 AAAAC3Nza..."

------------------------------------------------------------------------

## 🚀 Utilisation

    chmod +x script.sh
    sudo ./script.sh

------------------------------------------------------------------------

## 🔍 Ce que fait le script

1.  Backup `/etc/network/interfaces`
2.  Applique IP statique
3.  Backup `/etc/resolv.conf`
4.  Applique nouveaux DNS
5.  Test DNS (`getent hosts google.com`)
6.  Rollback automatique si échec
7.  Backup `sshd_config`
8.  Recréation complète config SSH
9.  Validation `sshd -t`
10. Restart SSH

------------------------------------------------------------------------

## 🧪 Vérifications après exécution

### Vérifier l'IP

    ip a
    ip route

### Vérifier le DNS

    cat /etc/resolv.conf
    getent hosts google.com

### Tester SSH (depuis une autre machine)

    ssh -p 22 user@IP

⚠️ **Ne fermez jamais votre session SSH actuelle avant validation.**

------------------------------------------------------------------------

## 🛡 Paramètres de sécurité appliqués

-   `PermitRootLogin no`
-   `PasswordAuthentication no`
-   `PubkeyAuthentication yes`
-   `MaxAuthTries 3`
-   `LoginGraceTime 30`
-   Désactivation du forwarding
-   Permissions SSH strictes :
    -   `.ssh` → 700
    -   `authorized_keys` → 600

------------------------------------------------------------------------

## 🔁 Idempotence

-   La clé SSH n'est pas dupliquée
-   Les backups sont horodatés
-   Les permissions sont corrigées si incorrectes

------------------------------------------------------------------------

## 📜 Licence

MIT
