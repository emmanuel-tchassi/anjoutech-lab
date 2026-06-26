# Projet 05 — Réseau d'entreprise simulé

## 📋 Description

Simulation d'un réseau d'entreprise complet avec VLANs, DHCP, DNS et routage inter-VLAN dans Cisco Packet Tracer, avec contrôle d'accès via ACLs.

---

## 🏗️ Topologie réseau

```
Internet
    |
  Routeur 2911
    |
  Switch 2960
  /    |    \
VLAN10 VLAN20 VLAN30
  RH  Finance   IT
```

| VLAN | Département | Réseau | Plage DHCP |
|---|---|---|---|
| VLAN 10 | RH | 192.168.10.0/24 | 192.168.10.10-11 |
| VLAN 20 | Finance | 192.168.20.0/24 | 192.168.20.10-11 |
| VLAN 30 | IT | 192.168.30.0/24 | 192.168.30.10-11 |

---

## ✅ Réalisations

### 1 — Configuration des VLANs sur le Switch
- VLAN 10 — RH
- VLAN 20 — Finance
- VLAN 30 — IT
- Ports assignés par département
- Port trunk vers le routeur

### 2 — Router-on-a-Stick
- Sous-interfaces dot1Q configurées sur Gig0/0
- Gateway par VLAN :
  - 192.168.10.1 (RH)
  - 192.168.20.1 (Finance)
  - 192.168.30.1 (IT)

### 3 — DHCP par VLAN
- 3 pools DHCP configurés sur le routeur
- Adresses exclues : .1 à .9 par réseau
- DNS : 8.8.8.8

### 4 — ACLs inter-VLANs
- RH → Finance : **BLOQUÉ**
- Finance → RH : **BLOQUÉ**
- RH → IT : **Autorisé**
- Finance → IT : **Autorisé**
- IT → Tous : **Autorisé**

---

## 🔧 Configuration Switch

```
vlan 10
 name RH
vlan 20
 name Finance
vlan 30
 name IT

interface fastEthernet 0/1
 switchport mode access
 switchport access vlan 10

interface gigabitEthernet 0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

## 🔧 Configuration Routeur

```
interface gigabitEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

ip dhcp pool VLAN10-RH
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

ip access-list extended BLOC-RH-FINANCE
 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
 permit ip any any
```

---

## 📸 Screenshots

Les captures d'écran sont disponibles dans le dossier [screenshots](./screenshots/).

---

## 🎯 Compétences démontrées

- ✅ Configuration de VLANs sur switch Cisco
- ✅ Router-on-a-Stick (routage inter-VLAN)
- ✅ DHCP par VLAN
- ✅ DNS
- ✅ ACLs de contrôle d'accès inter-VLANs
- ✅ Cisco Packet Tracer
