# Projet 04 — WSUS / Windows Update for Business

## 📋 Description

Installation de WSUS sur un serveur dédié et configuration de Windows Update for Business via GPO comme alternative moderne à WSUS pour la gestion des mises à jour Windows.

---

## 🏗️ Infrastructure

| Machine | Rôle | OS |
|---|---|---|
| SRV-WSUS01 | Serveur WSUS dédié | Windows Server 2022 |
| SRV-DC01 | Contrôleur de domaine | Windows Server 2022 |
| PC-W11-01 | Poste client test | Windows 11 Pro |

---

## ✅ Réalisations

### 1 — Installation WSUS sur SRV-WSUS01
- Serveur SRV-WSUS01 créé et joint au domaine anjoutech.lan
- Rôle WSUS installé sur serveur dédié
- Console WSUS fonctionnelle
- Dossier de stockage : C:\WSUS

### 2 — Configuration WSUS
- Synchronisation avec Microsoft Update configurée
- Produits sélectionnés : Windows 11, Windows Server 2022
- Classifications : Critical Updates, Security Updates, Definition Updates
- Schedule de synchronisation : 1 fois par jour

### 3 — Problème rencontré et résolution
- **Problème :** Timeout de synchronisation dans un environnement VMware NAT
- **Diagnostic :** Problème de connectivité réseau entre la VM et les serveurs Microsoft Update
- **Solution adoptée :** Windows Update for Business via GPO (solution moderne recommandée par Microsoft)

### 4 — Windows Update for Business via GPO
- GPO **GPO-WindowsUpdate** créée et liée à anjoutech.lan
- Feature Updates différées de **180 jours**
- Quality Updates différées de **7 jours**
- Installation automatique à **3h du matin**
- Heures actives configurées : **8h-18h** (pas de redémarrage forcé)

---

## 📸 Screenshots

Les captures d'écran sont disponibles dans le dossier [screenshots](./screenshots/).

---

## 🎯 Compétences démontrées

- ✅ Installation et configuration de WSUS sur serveur dédié
- ✅ Jonction d'un serveur membre au domaine
- ✅ Diagnostic de problèmes de connectivité réseau
- ✅ Windows Update for Business via GPO
- ✅ Gestion des mises à jour en entreprise
- ✅ Différé de mises à jour (Feature/Quality Updates)
