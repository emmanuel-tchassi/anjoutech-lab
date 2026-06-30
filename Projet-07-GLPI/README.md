# Projet 07 — Ticketing GLPI

## 📋 Description

Installation et configuration de GLPI 11 sur Ubuntu Server avec gestion des incidents, inventaire du parc informatique et création de tickets réalistes simulant un environnement helpdesk réel.

---

## 🏗️ Infrastructure

| Composante | Détail |
|---|---|
| Serveur | SRV-GLPI (Ubuntu Server 24.04) |
| IP | 10.0.2.40 |
| Stack | Apache + MySQL + PHP 8.5 |
| Version GLPI | 11.0.0 |
| URL | http://10.0.2.40 |

---

## ✅ Réalisations

### 1 — Installation GLPI
- Ubuntu Server 24.04 LTS installé
- Stack LAMP configuré (Apache, MySQL, PHP)
- GLPI 11.0.0 installé et accessible
- Base de données MySQL configurée

### 2 — Configuration
- 2 techniciens créés : acote, jfortin
- 5 catégories de tickets :
  - Sécurité
  - Messagerie
  - Réseau
  - Matériel
  - Système
- SLA Standard configuré (8 heures)

### 3 — Inventaire du parc informatique

| Équipement | Type | Domaine |
|---|---|---|
| SRV-DC01 | Serveur | anjoutech.lan |
| SRV-WSUS01 | Serveur | anjoutech.lan |
| SRV-GLPI | Serveur | anjoutech.lan |
| PC-W11-01 | Ordinateur | anjoutech.lan |
| PC-W11-02 | Ordinateur | anjoutech.lan |

### 4 — Tickets créés (31 tickets)

| Catégorie | Nombre | Priorité |
|---|---|---|
| Mot de passe oublié | 6 | Moyenne |
| Outlook / Messagerie | 5 | Haute |
| VPN / Réseau | 5 | Haute |
| Imprimante / Matériel | 5 | Moyenne/Basse |
| Partage réseau | 5 | Haute |
| Système / Windows | 5 | Basse/Haute |

### 5 — Tickets résolus
- 5 tickets résolus avec notes de résolution détaillées
- Workflow complet documenté : création → assignation → résolution

---

## 📸 Screenshots

Les captures d'écran sont disponibles dans le dossier [screenshots](./screenshots/).

---

## 🎯 Compétences démontrées

- ✅ Installation Linux (Ubuntu Server 24.04)
- ✅ Configuration stack LAMP
- ✅ Installation et configuration GLPI 11
- ✅ Gestion des incidents (ITIL)
- ✅ Inventaire du parc informatique
- ✅ SLA et priorités
- ✅ Workflow helpdesk complet
