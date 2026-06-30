# Projet 08 — Virtualisation

## 📋 Description

Mise en place d'un environnement de virtualisation complet avec VMware Workstation Pro pour héberger l'ensemble des VMs du lab AnjouTech.

---

## 🏗️ Infrastructure virtualisée

| VM | OS | RAM | Disque | Réseau |
|---|---|---|---|---|
| SRV-DC01 | Windows Server 2022 | 4 Go | 60 Go | Host-only |
| SRV-WSUS01 | Windows Server 2022 | 2 Go | 100 Go | Host-only/NAT |
| SRV-GLPI | Ubuntu Server 24.04 | 2 Go | 40 Go | Host-only |
| PC-W11-01 | Windows 11 Pro | 2 Go | 64 Go | Host-only |
| PC-W11-02 | Windows 11 Pro | 2 Go | 64 Go | Host-only |

**Hyperviseur :** VMware Workstation Pro (gratuit depuis 2024)
**RAM totale utilisée :** ~12 Go sur 16 Go disponibles

---

## ✅ Réalisations

### 1 — Installation VMware Workstation Pro
- VMware Workstation Pro installé (version gratuite pour usage personnel)
- Réseau Host-only configuré pour isoler le lab

### 2 — Création et gestion des VMs
- 5 VMs créées et configurées
- VMware Tools installés sur chaque VM
- Réseau interne configuré (Host-only)

### 3 — Gestion des snapshots
- Snapshots pris à chaque étape importante
- Restauration testée et validée
- Naming convention cohérente

### 4 — Bonnes pratiques
- Snapshots avant chaque configuration majeure
- Nommage des VMs cohérent (SRV-DC01, PC-W11-01, etc.)
- Allocation RAM optimisée selon les ressources disponibles

---

## 📸 Screenshots

Les captures d'écran sont disponibles dans le dossier [screenshots](./screenshots/).

---

## 🎯 Compétences démontrées

- ✅ VMware Workstation Pro
- ✅ Création et gestion des VMs
- ✅ Configuration réseau virtuel (Host-only, NAT)
- ✅ Gestion des snapshots
- ✅ VMware Tools
- ✅ Optimisation des ressources
