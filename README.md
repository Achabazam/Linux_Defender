# Linux_Defender

**Hackathon – Projet de surveillance du trafic broadcast sur interface web**

## Table des matières

1. [Présentation du projet](#présentation-du-projet)  
2. [Fonctionnalités principales](#fonctionnalités-principales)  
4. [Prérequis](#prérequis)  
5. [Installation](#installation)  
6. [Configuration](#configuration)  
7. [Utilisation](#utilisation)  
8. [Contribuer](#contribuer)  
9. [Licence](#licence)

---

## Présentation du projet

**Linux_Defender** est une solution simple qui permet de surveiller le trafic de broadcast sur un réseau local et de l’exposer via une interface web dédiée. L’objectif est de détecter et de visualiser, en temps réel, les paquets de diffusion émis sur le réseau.

Ce projet s’installe grâce à un _role Ansible_ et déploie un service nommé **broadcast-scan** :  
- Il lance un service web accessible par défaut sur le **port 8000**.  
- Il expose, par la même occasion, des métriques consultables via l’URL : `http://<IP>:<PORT>/metrics`.

---

## Fonctionnalités principales

1. **Surveillance du trafic broadcast**  
   Collecte et journalisation en temps réel des paquets de diffusion sur un segment réseau.

2. **Interface web dédiée**  
   - Service web simple pour visualiser les informations de manière centralisée.  
   - Consultation des métriques sur l’URL `/metrics`.

3. **Installation automatisée via Ansible**  
   - Déploiement rapide du service **broadcast-scan**.  
   - Configuration des variables clés pour s’adapter à différents environnements.

4. **Extensible**  
   - Peut être couplé avec des solutions de monitoring externes (Prometheus, Grafana, etc.).

---

## Prérequis

- **Ansible** installé sur la machine d’administration.  
- Un accès SSH et les droits nécessaires (sudo/root) sur la machine cible.  
- Un fichier d’inventaire Ansible (ex. `hosts`) configuré avec l’hôte cible (ou les hôtes cibles).  
- Une distribution Linux (ex. Debian, Ubuntu) sur laquelle déployer **Linux_Defender**.

---

## Installation

1. **Récupération du rôle Ansible**  
   Assurez-vous d’avoir importé ou cloné le rôle Ansible nécessaire au déploiement du service **broadcast-scan**.

2. **Configuration de l’inventaire**  
   Mettez à jour le fichier `hosts` (ou tout autre fichier d’inventaire Ansible) pour cibler la (ou les) machine(s) où vous souhaitez déployer le service.

3. **Exécution du playbook**  
   Une fois le fichier d’inventaire correctement renseigné, lancez le playbook Ansible :

   ```bash
   ansible-playbook -i hosts playbook_raspberry.yml
   ```

   Ansible va alors :  
   - Installer les dépendances nécessaires.  
   - Créer et activer le service **broadcast-scan**.  

---

## Configuration

Le rôle Ansible prévoit plusieurs variables de configuration. Vous pouvez les ajuster soit directement dans vos fichiers de variables (ex. `group_vars`), soit en ligne de commande lors de l’exécution du playbook. Ci-dessous, les variables les plus importantes :

```yaml
broadcast_scan_dir: "/opt/broadcast-scan"     # Répertoire d'installation/stockage
broadcast_scan_filename: "broadcast_scan.py"  # Nom du fichier principal
broadcast_scan_prometheus_port: 8000          # Port d'exposition du service web
broadcast_scan_interface: "eth1"              # Interface réseau à monitorer
```

> **Remarque :** Adaptez ces valeurs selon votre configuration réseau (interface, chemin d’installation, port, etc.).

---

## Utilisation

1. **Vérifier le statut du service**  
   Sur la machine cible, exécutez :

   ```bash
   systemctl status broadcast-scan
   ```
   Vous devriez voir un service actif (running).

2. **Consulter les métriques**  
   Les métriques (exportées au format Prometheus) sont disponibles à l’URL :

   ```
   http://<ADRESSE_IP>:8000/metrics
   ```

3. **Arrêter / redémarrer le service**  
   - Arrêter le service : `sudo systemctl stop broadcast-scan`  
   - Redémarrer le service : `sudo systemctl restart broadcast-scan`  
