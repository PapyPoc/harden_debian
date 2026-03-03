# 🔐 Debian Network & SSH Hardening Script

Script Bash pour **Debian uniquement** permettant :

- Configuration d'une IP statique (`/etc/network/interfaces`)
- Configuration des DNS (`/etc/resolv.conf`)
- Validation DNS avec rollback automatique
- Recréation complète et sécurisation de `/etc/ssh/sshd_config`
- Ajout automatique d'une clé publique SSH
- Configuration idempotente

---

## 📦 Fonctionnalités

### 🌐 Réseau
- Détection automatique de l’interface principale
- Configuration IP statique
- Conversion automatique CIDR → netmask
- Redémarrage du service réseau
- Backup automatique avant modification

### 🌍 DNS
- Mise à jour de `/etc/resolv.conf`
- Support :
  - écriture directe
  - `resolvconf`
  - `systemd-resolved`
- Test automatique de résolution
- 🔁 Rollback automatique si échec DNS

### 🔒 SSH Hardening
- Backup horodaté de `sshd_config`
- Recréation complète du fichier
- Désactivation :
  - `PermitRootLogin`
  - `PasswordAuthentication`
  - `ChallengeResponseAuthentication`
- Activation :
  - Authentification par clé uniquement
- Validation avec `sshd -t`
- Ajout sécurisé de la clé SSH dans le bon utilisateur
- Permissions conformes OpenSSH (700 / 600)

---

## 🛠️ Prérequis

- Debian (testé sur Debian 11/12)
- `ifupdown`
- OpenSSH installé
- Exécution avec `sudo` ou en root

---

## ⚙️ Configuration

Modifier les variables en haut du script :

```bash
STATIC_IP="10.0.0.202/24"
GATEWAY="10.0.0.254"
DNS_LIST=("10.0.0.254" "1.1.1.1")
SSH_PORT="22"
CLE_SSH_P="ssh-ed25519 AAAAC3Nza..."# harden_debian
Sécurisation d'un serveur débian (IP Fixe, DNS, resolv_conf)
