# Debian Network + DNS + SSH Hardening Script
## Description
Ce script configure automatiquement une machine Debian avec :
- Une IP statique
- Les DNS système
- Un durcissement SSH minimal
- L’ajout automatique d’une clé publique SSH
- La désactivation de l’authentification par mot de passe SSH

Le script est prévu pour être exécuté **en console locale**.
# Fonctionnalités
## Réseau
Configuration automatique de :
- `/etc/network/interfaces`
- Adresse IP statique
- Gateway
- DNS via :
  - `resolvconf`
  - `systemd-resolved`
  - ou `/etc/resolv.conf`

Backup automatique des fichiers modifiés.
## SSH Hardening
Le script :
- Désactive `root`
- Désactive l’authentification par mot de passe
- Active uniquement l’authentification par clé
- Limite les tentatives SSH
- Désactive :
  - X11Forwarding
  - AgentForwarding
  - TCP Forwarding
  - Tunnel

Validation automatique du fichier `sshd_config` avant redémarrage.
# Pré-requis
## Debian uniquement
Compatible :
- Debian 11
- Debian 12
- Debian 13

Le script suppose l’utilisation de :
- `ifupdown`
- `openssh-server`
- `systemd`
# IMPORTANT
## Exécution recommandée
Exécuter :
- en console locale
- VM console
- IPMI/iLO/DRAC
Éviter une exécution distante SSH sans accès console.
# Configuration
Modifier les variables en haut du script :
```bash
STATIC_IP="10.0.0.202/24"
GATEWAY="10.0.0.254"

DNS_LIST=("10.0.0.254" "1.1.1.1" "1.0.0.1")

SEARCH_DOMAINS=("test.auvergneinfo.lan")

SSH_PORT="22"

CLE_SSH_PUBLIC="ssh-ed25519 AAAA..."
```
# Clé SSH
## Générer une clé
Sous Linux :
```bash
ssh-keygen -t ed25519
```
## Récupérer la clé publique

```bash
cat ~/.ssh/id_ed25519.pub
```
## Exemple valide
```bash
CLE_SSH_PUBLIC="ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIG... user@host"
```
# Lancement
## Récupération du script
```bash
wget https://raw.githubusercontent.com/PapyPoc/harden_debian/refs/heads/main/harden_debian.sh
chmod +x harden_debian.sh
```
## Exécuter
```bash
sudo ./setup.sh
```
# Vérifications après installation
## Vérifier le réseau
```bash
ip addr
ip route
```
## Vérifier les DNS
```bash
cat /etc/resolv.conf
```
## Vérifier SSH
```bash
sshd -t
systemctl status ssh
```
# Test SSH
Tester une nouvelle connexion SSH avant fermeture de session :
```bash
ssh -p 22 user@10.0.0.202
```
ers sauvegardés
## Réseau
```bash
/etc/network/interfaces.backup.*
```
## DNS
```bash
/etc/resolv.conf.backup.*
```
## SSH
```bash
/etc/ssh/sshd_config.backup.*
```
# Rollback manuel
## Réseau
```bash
cp /etc/network/interfaces.backup.* /etc/network/interfaces
systemctl restart networking
```
## SSH
```bash
cp /etc/ssh/sshd_config.backup.* /etc/ssh/sshd_config
systemctl restart ssh
```
# Sécurité
Le script :
- désactive les mots de passe SSH
- interdit le login root
- impose les clés SSH
Si la clé publique configurée est invalide ou absente :
- le script stoppe automatiquement
- aucune modification SSH n’est appliquée

