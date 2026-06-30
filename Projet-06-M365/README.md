# Projet 06 — Microsoft 365

## 📋 Description

Configuration d'un environnement Microsoft 365 Business Standard avec gestion des utilisateurs, groupes, Exchange Online, MFA et Entra ID.

---

## 🏗️ Environnement

| Composante | Détail |
|---|---|
| Tenant | anjoutech.onmicrosoft.com |
| Plan | Microsoft 365 Business Standard |
| Utilisateurs | 6 comptes créés |
| Groupes | 3 groupes Microsoft 365 |

---

## ✅ Réalisations

### 1 — Création du tenant M365
- Tenant Business Standard créé
- Domaine : anjoutech.onmicrosoft.com
- Administration via admin.microsoft.com

### 2 — Gestion des utilisateurs

| Utilisateur | Email | Département |
|---|---|---|
| Marie Tremblay | mtremblay@anjoutech.onmicrosoft.com | RH |
| Jean Gagnon | jgagnon@anjoutech.onmicrosoft.com | RH |
| Sophie Lavoie | slavoie@anjoutech.onmicrosoft.com | Finance |
| Marc Bouchard | mbouchard@anjoutech.onmicrosoft.com | Finance |
| Alex Cote | acote@anjoutech.onmicrosoft.com | IT |
| Julie Fortin | jfortin@anjoutech.onmicrosoft.com | IT |

### 3 — Groupes Microsoft 365

| Groupe | Email | Membres |
|---|---|---|
| GRP-RH | grp-rh@anjoutech.onmicrosoft.com | mtremblay, jgagnon |
| GRP-Finance | grp-finance@anjoutech.onmicrosoft.com | slavoie, mbouchard |
| GRP-IT | grp-it@anjoutech.onmicrosoft.com | acote, jfortin |

### 4 — MFA (Multi-Factor Authentication)
- MFA activé pour les 6 utilisateurs
- Configuration via Entra ID (Azure AD)
- Testé et validé

### 5 — Exchange Online
- Boîte partagée créée : support@anjoutech.onmicrosoft.com
  - Accès accordé à acote et jfortin
- Liste de distribution : tous-employes@anjoutech.onmicrosoft.com
  - Tous les 6 utilisateurs inclus
- Règle de transport : blocage des pièces jointes .exe

### 6 — Gestion des comptes
- Reset de mot de passe testé (mtremblay)
- Désactivation de compte testée (mbouchard)
- Réactivation de compte testée

### 7 — Entra ID
- Rôle **Helpdesk Administrator** assigné à jfortin
- Audit logs consultés et documentés

---

## 📸 Screenshots

Les captures d'écran sont disponibles dans le dossier [screenshots](./screenshots/).

---

## 🎯 Compétences démontrées

- ✅ Administration Microsoft 365
- ✅ Exchange Online (boîtes partagées, listes de distribution, règles de transport)
- ✅ MFA via Entra ID
- ✅ Gestion des rôles administratifs
- ✅ Audit logs
- ✅ Gestion des licences
