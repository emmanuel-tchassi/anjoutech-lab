# Projet 10 — Sécurité Windows

## 📋 Description

Configuration des outils de sécurité natifs Windows : Windows Defender, pare-feu, politiques d'audit et simulation d'attaque avec analyse des logs de sécurité.

---

## ✅ Réalisations

### 1 — Windows Defender
- Protection en temps réel activée et vérifiée
- Analyse planifiée configurée : **chaque dimanche à 2h00**
- Protection cloud activée
- Tamper protection activée

### 2 — Pare-feu Windows

| Règle | Type | Port | Action |
|---|---|---|---|
| Bloquer Telnet | Entrante | TCP 23 | Bloquer |
| Autoriser RDP | Entrante | TCP 3389 | Autoriser |

### 3 — Politiques d'audit via GPO
GPO **GPO-Audit-Securite** créée et liée à anjoutech.lan :

| Politique | Success | Failure |
|---|---|---|
| Audit Logon | ✅ | ✅ |
| Audit Logoff | ✅ | — |
| Audit User Account Management | ✅ | ✅ |
| Audit Credential Validation | ✅ | ✅ |
| File System | ✅ | ✅ |

### 4 — Politique de verrouillage de compte
Configurée dans la Default Domain Policy :

| Paramètre | Valeur |
|---|---|
| Seuil de verrouillage | 5 tentatives |
| Durée du verrouillage | 30 minutes |
| Fenêtre d'observation | 30 minutes |

### 5 — Simulation d'attaque et analyse des logs

**Scénario simulé :**
1. 5 tentatives de connexion échouées avec mtremblay
2. Compte verrouillé automatiquement
3. Événements retrouvés dans l'Observateur d'événements
4. Compte déverrouillé via ADUC

**Événements Windows identifiés :**

| ID | Signification | Trouvé |
|---|---|---|
| 4624 | Connexion réussie | ✅ |
| 4625 | Échec de connexion | ✅ |
| 4740 | Compte verrouillé | ✅ |

### 6 — Audit d'accès aux fichiers
- Audit NTFS activé sur C:\Partages\RH, Finance, IT
- Événement **4663** (accès aux objets) tracé
- Testé avec accès autorisé (mtremblay → RH) et refusé (mtremblay → Finance)

---

## 📸 Screenshots

Les captures d'écran sont disponibles dans le dossier [screenshots](./screenshots/).

---

## 🎯 Compétences démontrées

- ✅ Windows Defender — configuration et planification
- ✅ Pare-feu Windows avec règles avancées
- ✅ Politiques d'audit via GPO
- ✅ Politique de verrouillage de compte
- ✅ Observateur d'événements — IDs 4624, 4625, 4740, 4663
- ✅ Simulation d'attaque et réponse aux incidents
- ✅ Audit d'accès aux fichiers NTFS
