# Projet 09 — VPN

## 📋 Description

Configuration de deux solutions VPN sur Windows Server 2022 : RRAS (VPN natif Windows) et OpenVPN avec infrastructure PKI complète (EasyRSA).

---

## 🏗️ Infrastructure

| Composante | Détail |
|---|---|
| Serveur VPN | SRV-DC01 (10.0.2.10) |
| Client VPN | PC-W11-01 |
| Pool PPTP | 10.0.2.200 — 10.0.2.210 |
| Tunnel OpenVPN | 10.8.0.0/24 |

---

## ✅ Réalisations

### 1 — VPN Windows PPTP (RRAS)
- Rôle Remote Access installé sur SRV-DC01
- RRAS configuré en mode VPN only
- Pool d'adresses IP configuré (10.0.2.200-210)
- Utilisateurs autorisés : acote, jfortin (Dial-in Allow access)
- Connexion VPN testée et validée depuis PC-W11-01
- Accès aux ressources du domaine validé via VPN

### 2 — OpenVPN avec PKI EasyRSA

#### Génération des certificats
- CA (Certificate Authority) créé : **AnjouTech-CA**
- Certificat serveur généré et signé
- Certificat client généré et signé (acote)
- Paramètres Diffie-Hellman générés (dh.pem)

#### Configuration serveur
- OpenVPN Server installé sur SRV-DC01
- Fichier server.ovpn configuré
- Service OpenVPN démarré et opérationnel
- Route vers 10.0.2.0/24 poussée aux clients

#### Configuration client
- OpenVPN Client installé sur PC-W11-01
- Certificats client copiés (ca.crt, acote.crt, acote.key)
- Fichier acote.ovpn configuré
- Connexion OpenVPN testée et validée

### 3 — Tests de validation

| Test | Résultat |
|---|---|
| Connexion VPN PPTP | ✅ Établie |
| Ping SRV-DC01 via PPTP | ✅ Réussi |
| Accès \\SRV-DC01\IT via PPTP | ✅ Autorisé |
| Connexion OpenVPN | ✅ Établie |
| Ping SRV-DC01 via OpenVPN | ✅ Réussi |

---

## 📸 Screenshots

Les captures d'écran sont disponibles dans le dossier [screenshots](./screenshots/).

---

## 🎯 Compétences démontrées

- ✅ RRAS (Routing and Remote Access Service)
- ✅ VPN PPTP Windows natif
- ✅ OpenVPN avec PKI complète
- ✅ EasyRSA — génération de certificats (CA, serveur, client)
- ✅ Tunneling et chiffrement AES-256
- ✅ Accès distant sécurisé aux ressources du domaine
