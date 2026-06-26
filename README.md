# anjoutech-lab
Déploiement d'un environnement IT d'entreprise sous Windows Server 2022
# 🖥️ AnjouTech HomeLab — Environnement IT d'entreprise

## 📋 Description

Ce projet documente le déploiement complet d'un environnement IT d'entreprise simulé sous **Windows Server 2022**, réalisé dans le cadre d'une transition professionnelle vers l'administration systèmes et réseaux.

L'environnement **AnjouTech** (domaine : `anjoutech.lan`) reproduit fidèlement les infrastructures rencontrées dans les PME québécoises.

---

## 🏗️ Infrastructure

| Machine | Rôle | OS |
|---|---|---|
| SRV-DC01 | Contrôleur de domaine, DNS, DHCP, Fichiers, RRAS, OpenVPN | Windows Server 2022 |
| SRV-WSUS01 | WSUS (Windows Server Update Services) | Windows Server 2022 |
| SRV-GLPI | Ticketing GLPI, Apache, MySQL | Ubuntu Server 24.04 |
| PC-W11-01 | Poste client RH | Windows 11 Pro |
| PC-W11-02 | Poste client Finance | Windows 11 Pro |

**Hyperviseur :** VMware Workstation Pro | **RAM :** 16 Go

---

## 📁 Projets réalisés

| # | Projet | Compétences |
|---|---|---|
| 01 | [Active Directory](./Projet-01-Active-Directory/) | AD DS, PowerShell, Gestion des utilisateurs |
| 02 | [Group Policy (GPO)](./Projet-02-GPO/) | GPO, Administration Windows |
| 03 | [Serveur de fichiers](./Projet-03-Serveur-De-Fichiers/) | NTFS, SMB, Permissions |
| 04 | [WSUS / Windows Update for Business](./Projet-04-WSUS/) | Patch Management, GPO |
| 05 | [Réseau d'entreprise simulé](./Projet-05-Reseau/) | VLAN, DHCP, DNS, Cisco Packet Tracer |
| 06 | [Microsoft 365](./Projet-06-M365/) | Exchange Online, Entra ID, MFA |
| 07 | [Ticketing GLPI](./Projet-07-GLPI/) | GLPI, ITIL, Linux |
| 08 | [Virtualisation](./Projet-08-Virtualisation/) | VMware, Snapshots |
| 09 | [VPN](./Projet-09-VPN/) | RRAS, OpenVPN, PKI |
| 10 | [Sécurité Windows](./Projet-10-Securite/) | Defender, Audit, Event Viewer |

---

## 🛠️ Compétences techniques démontrées

- **Windows Server 2022** — Installation, configuration, rôles et fonctionnalités
- **Active Directory** — Domaine, OUs, utilisateurs, groupes, GPOs, Fine-Grained Password Policy
- **Réseau** — DHCP, DNS, VLAN, Router-on-a-Stick, ACLs (Cisco Packet Tracer)
- **Sécurité** — Windows Defender, Pare-feu, Audit, Event Viewer (IDs 4624, 4625, 4740)
- **Microsoft 365** — Exchange Online, Entra ID, MFA, Conditional Access
- **Linux** — Ubuntu Server, Apache, MySQL, PHP (stack LAMP)
- **VPN** — RRAS/PPTP, OpenVPN avec PKI EasyRSA
- **Ticketing** — GLPI 11, gestion des incidents, SLA
- **Scripting** — PowerShell (automatisation, gestion AD, permissions NTFS)
- **Virtualisation** — VMware Workstation Pro, gestion des VMs et snapshots

---

## 📜 Certifications

- **CompTIA Network+** ✅ — Obtenue
- **CompTIA Security+** — En préparation

---

## 📞 Contact

**Emmanuel Tchassi**
📧 [e.tchassi.it@gmail.com]
💼 [LinkedIn](https://www.linkedin.com/in/kodjo-sok%C3%AA-emmanuel-justin-tchassi/)
📍 Laval, Québec, Canada
