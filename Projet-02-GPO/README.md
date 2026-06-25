# Projet 02 — Group Policy (GPO)

## 📋 Description

Configuration de stratégies de groupe (GPO) pour automatiser et sécuriser l'environnement Windows d'entreprise AnjouTech. Les GPOs permettent de déployer des paramètres sur l'ensemble des postes du domaine sans intervention manuelle.

---

## ✅ GPOs configurées

### GPO-Bloquer-Panneau-Configuration
- **Liée à :** OU RH
- **Effet :** Interdit l'accès au Panneau de configuration et aux Paramètres PC
- **Chemin :** User Configuration → Administrative Templates → Control Panel → Prohibit access to Control Panel

### GPO-Lecteur-Reseau
- **Liée à :** OU RH
- **Effet :** Mappe automatiquement le lecteur réseau **R:** vers `\\SRV-DC01\RH`
- **Chemin :** User Configuration → Preferences → Windows Settings → Drive Maps

### GPO-Fond-Ecran
- **Liée à :** anjoutech.lan (tout le domaine)
- **Effet :** Déploie un fond d'écran corporate sur tous les postes
- **Chemin :** User Configuration → Administrative Templates → Desktop → Desktop Wallpaper

### GPO-Politique-Mots-de-passe
- **Liée à :** anjoutech.lan (tout le domaine)
- **Paramètres configurés :**

| Paramètre | Valeur |
|---|---|
| Longueur minimale | 10 caractères |
| Complexité | Activée |
| Durée maximale | 90 jours |
| Durée minimale | 1 jour |
| Historique | 10 mots de passe |

### GPO-Restriction-USB
- **Liée à :** OUs RH et Finance
- **Effet :** Bloque l'accès aux périphériques USB amovibles
- **Chemin :** Computer Configuration → Administrative Templates → System → Removable Storage Access

### GPO-WindowsUpdate
- **Liée à :** anjoutech.lan (tout le domaine)
- **Effet :** Configure Windows Update for Business
- **Paramètres :**
  - Installation automatique à 3h du matin
  - Feature Updates différées de 180 jours
  - Quality Updates différées de 7 jours
  - Heures actives : 8h-18h (pas de redémarrage forcé)

---

## 🔍 Validation

Toutes les GPOs ont été validées via :

```powershell
gpupdate /force
gpresult /r
```

---

## 🎯 Compétences démontrées

- ✅ Création et gestion des GPOs via Group Policy Management Console
- ✅ Liaison des GPOs aux OUs appropriées
- ✅ Déploiement de paramètres utilisateur et ordinateur
- ✅ Validation avec gpresult et gpupdate
- ✅ Windows Update for Business (alternative moderne à WSUS)
- ✅ Restriction USB via GPO
