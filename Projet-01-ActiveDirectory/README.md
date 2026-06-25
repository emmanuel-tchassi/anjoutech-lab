# Projet 01 — Active Directory

## 📋 Description

Déploiement d'un environnement Active Directory complet sur Windows Server 2022 avec création d'un domaine d'entreprise, gestion des utilisateurs, groupes, unités organisationnelles et configuration DHCP.

---

## 🏗️ Infrastructure

| Machine | Rôle | IP |
|---|---|---|
| SRV-DC01 | Contrôleur de domaine, DNS, DHCP | 10.0.2.10 |
| PC-W11-01 | Poste client RH | 10.0.2.20 (statique) |
| PC-W11-02 | Poste client Finance | DHCP (10.0.2.100-200) |

**Domaine :** `anjoutech.lan`

---

## ✅ Réalisations

### 1 — Installation Active Directory

![Server Manager Dashboard](./screenshots/01-server-manager-dashboard.png)
*Server Manager avant l'installation du rôle AD DS*

![Installation Type](./screenshots/02-installation-type.png)
*Sélection du type d'installation — Role-based*

![Server Selection](./screenshots/03-server-selection.png)
*Sélection du serveur SRV-DC01*

![Add Features AD DS](./screenshots/04-add-features-adds.png)
*Ajout des fonctionnalités requises pour AD DS*

![Confirmation Installation](./screenshots/05-confirmation-installation.png)
*Confirmation de l'installation — AD DS + Group Policy Management*

![Installation Progress](./screenshots/06-installation-progress.png)
*Installation en cours sur SRV-DC01*

---

### 2 — Promotion en contrôleur de domaine

![Post-deployment Promote](./screenshots/07-post-deployment-promote.png)
*Post-déploiement — Promouvoir en contrôleur de domaine*

![Deployment Config Domain](./screenshots/08-deployment-config-domain.png)
*Configuration du déploiement — Nouveau domaine anjoutech.lan*

![DC Options](./screenshots/09-dc-options.png)
*Options du contrôleur de domaine — DNS + Global Catalog*

![NetBIOS ANJOUTECH](./screenshots/10-netbios-anjoutech.png)
*Nom NetBIOS — ANJOUTECH*

![Paths NTDS SYSVOL](./screenshots/11-paths-ntds-sysvol.png)
*Chemins NTDS et SYSVOL*

![Script Install ADDSForest](./screenshots/12-script-install-addsforest.png)
*Script PowerShell généré — Install-ADDSForest*

![Prerequisites Check Passed](./screenshots/13-prerequisites-check-passed.png)
*Vérification des prérequis — Tous les tests réussis*

![Installation DNS](./screenshots/14-installation-dns.png)
*Installation en cours — Configuration DNS*

![Connexion Domaine](./screenshots/15-connexion-domaine.png)
*Première connexion — ANJOUTECH\Administrator*

---

### 3 — Structure organisationnelle

![Ouverture dsa.msc](./screenshots/16-ouverture-dsamsc.png)
*Ouverture de Active Directory Users and Computers*

![Création OU RH](./screenshots/17-creation-ou-rh.png)
*Création de l'OU RH dans anjoutech.lan*

![Structure OUs](./screenshots/18-structure-ous.png)
*Structure des OUs — RH, Finance, IT créées*

![Création Groupe GRP-Finance](./screenshots/19-creation-groupe-grp-finance.png)
*Création du groupe de sécurité GRP-Finance*

---

### 4 — Création des utilisateurs via PowerShell + CSV

![Fichier users.csv](./screenshots/20-fichier-users-csv.png)
*Fichier users.csv — Structure initiale*

![Script Create-ADUsers ISE](./screenshots/21-script-create-adusers-ise.png)
*Script PowerShell Create-ADUsers.ps1 dans PowerShell ISE*

![Script Execution Completed](./screenshots/22-script-execution-completed.png)
*Exécution du script — Completed*

![Fichier users.csv Passwords](./screenshots/28-fichier-users-csv-passwords.png)
*Fichier users.csv mis à jour avec mots de passe individuels*

![Script Update Passwords](./screenshots/29-script-update-passwords.png)
*Script update-pswd.ps1 — Attribution des mots de passe individuels*

---

### 5 — Utilisateurs et groupes créés

![OU RH Utilisateurs](./screenshots/23-ou-rh-utilisateurs.png)
*OU RH — GRP-RH, Jean Gagnon, Marie Tremblay*

![OU Finance Utilisateurs](./screenshots/24-ou-finance-utilisateurs.png)
*OU Finance — GRP-Finance, Marc Bouchard, Sophie Lavoie*

![OU IT Utilisateurs](./screenshots/25-ou-it-utilisateurs.png)
*OU IT — Alex Cote, GRP-IT, Julie Fortin*

![Ajout Utilisateur Groupe RH](./screenshots/26-ajout-utilisateur-groupe-rh.png)
*Ajout de Marie Tremblay au groupe GRP-RH*

![Confirmation Ajout Groupe](./screenshots/27-confirmation-ajout-groupe.png)
*Confirmation — "Add to Group operation was successfully completed"*

---

### 6 — Installation Windows 11 et jonction au domaine

![Installation Windows 11](./screenshots/30-installation-windows11.png)
*Installation en cours de Windows 11*

![Bureau Windows 11](./screenshots/31-bureau-windows11.png)
*Bureau Windows 11 — Post-installation*

![Config Réseau PC-W11-01](./screenshots/32-config-reseau-pc-w11-01.png)
*Configuration réseau PC-W11-01 — IP 10.0.2.20, DNS 10.0.2.10*

![Ping SRV-DC01](./screenshots/33-ping-srv-dc01-reussi.png)
*Ping SRV-DC01 (10.0.2.10) réussi depuis PC-W11-01*

![Infos Système PC-W11-01](./screenshots/34-infos-systeme-pc-w11-01.png)
*Informations système PC-WIN11-01 — Windows 11 Pro*

![Jonction Domaine Credentials](./screenshots/35-jonction-domaine-credentials.png)
*Jonction au domaine anjoutech.lan — Saisie des credentials*

![Authentification Admin Domaine](./screenshots/36-authentification-admin-domaine.png)
*Authentification Administrator pour jonction au domaine*

![Bienvenue Domaine AnjouTech](./screenshots/37-bienvenue-domaine-anjoutech.png)
*✅ "Bienvenue dans le domaine anjoutech.lan"*

![Écran Connexion ANJOUTECH](./screenshots/38-ecran-connexion-anjoutech.png)
*Écran de connexion — Connectez-vous à ANJOUTECH*

![Bureau Post-Jonction](./screenshots/39-bureau-post-jonction-domaine.png)
*Bureau Windows 11 après jonction au domaine*

---

### 7 — Tâches administratives

![Reset Password mtremblay](./screenshots/40-reset-password-mtremblay.png)
*Reset Password — mtremblay avec "User must change password at next logon"*

![Connexion mtremblay](./screenshots/42-connexion-mtremblay-anjoutech.png)
*Connexion avec mtremblay — ANJOUTECH*

![Échec Connexion](./screenshots/43-echec-connexion-mdp-incorrect.png)
*Simulation — Échec de connexion avec mauvais mot de passe*

![Tentative Invalide](./screenshots/44-tentative-connexion-invalide.png)
*"Informations d'identification non valides, retardant la prochaine tentative"*

![Propriétés mtremblay Unlock](./screenshots/45-proprietes-mtremblay-unlock.png)
*Déverrouillage du compte mtremblay via ADUC*

![Bureau mtremblay](./screenshots/46-bureau-windows11-mtremblay.png)
*Bureau Windows 11 — Connecté en tant que mtremblay*

---

### 8 — Configuration DHCP

![DHCP Plage IP](./screenshots/47-dhcp-plage-ip.png)
*Configuration DHCP — Plage IP 10.0.2.100 à 10.0.2.200*

![DHCP Exclusions](./screenshots/48-dhcp-exclusions.png)
*DHCP — Exclusions 10.0.2.201 à 10.0.2.254*

![DHCP Gateway](./screenshots/49-dhcp-gateway.png)
*DHCP — Passerelle par défaut 10.0.2.1*

![DHCP DNS](./screenshots/50-dhcp-dns-configuration.png)
*DHCP — DNS SRV-DC01 + domaine anjoutech.lan*

![DHCP Scope Actif](./screenshots/51-dhcp-scope-actif.png)
*DHCP — Scope 10.0.2.0 Active*

![ipconfig DHCP](./screenshots/52-ipconfig-dhcp-pc-w11-02.png)
*ipconfig PC-W11-02 — Adresse IP obtenue via DHCP*

![Ping srv-dc01](./screenshots/53-ping-srv-dc01-reussi.png)
*Ping srv-dc01 et srv-dc01.anjoutech.lan — Résolution DNS validée*

---

## 📝 Scripts PowerShell

### Création des utilisateurs via CSV
```powershell
Import-Csv "C:\users.csv" | ForEach-Object {
    $username = ($_.FirstName[0] + $_.LastName).ToLower()
    New-ADUser `
        -Name "$($_.FirstName) $($_.LastName)" `
        -GivenName $_.FirstName `
        -Surname $_.LastName `
        -SamAccountName $username `
        -UserPrincipalName "$username@anjoutech.lan" `
        -Path "OU=$($_.OU),DC=anjoutech,DC=lan" `
        -AccountPassword (ConvertTo-SecureString $_.Password -AsPlainText -Force) `
        -Enabled $true
}
```

### Reset mot de passe individuel
```powershell
Set-ADAccountPassword -Identity mtremblay `
    -NewPassword (ConvertTo-SecureString "NewPass@2024!" -AsPlainText -Force) `
    -Reset
```

### Déverrouillage de compte
```powershell
Unlock-ADAccount -Identity mtremblay
Write-Host "Compte mtremblay déverrouillé" -ForegroundColor Green
```

### Fine-Grained Password Policy — Déploiement automatique
```powershell
[CmdletBinding(SupportsShouldProcess = $true)]
param()

Import-Module ActiveDirectory -ErrorAction Stop

function Get-FGPPSafe {
    param([string]$Name)
    return Get-ADFineGrainedPasswordPolicy -Filter "Name -eq '$Name'" -ErrorAction SilentlyContinue
}

function Test-ADGroupSafe {
    param([string]$Name)
    try {
        Get-ADGroup -Identity $Name -ErrorAction Stop | Out-Null
        return $true
    } catch {
        return $false
    }
}

function Ensure-FGPP {
    param(
        [Parameter(Mandatory)] [string]$Name,
        [Parameter(Mandatory)] [hashtable]$PolicyParams,
        [string[]]$Groups,
        [int]$Retry = 2
    )

    $policy = Get-FGPPSafe -Name $Name

    if (-not $policy) {
        if ($PSCmdlet.ShouldProcess($Name, "Créer la FGPP")) {
            try {
                New-ADFineGrainedPasswordPolicy @PolicyParams -ErrorAction Stop
            } catch {
                throw "Échec de création de la FGPP '$Name' : $_"
            }
            $policy = Get-FGPPSafe -Name $Name
            if (-not $policy) {
                throw "La FGPP '$Name' a été créée mais est introuvable après création."
            }
        }
    }

    foreach ($g in $Groups) {
        if (-not (Test-ADGroupSafe $g)) {
            Write-Warning "Le groupe '$g' n'existe pas. Ignoré."
            continue
        }
        $policy = Get-FGPPSafe -Name $Name
        if ($policy.AppliesTo -notcontains $g) {
            if ($PSCmdlet.ShouldProcess($Name, "Ajouter $g à la FGPP")) {
                try {
                    Add-ADFineGrainedPasswordPolicySubject `
                        -Identity $Name `
                        -Subjects $g `
                        -ErrorAction Stop
                } catch {
                    Write-Warning "Impossible d'ajouter $g à $Name : $_"
                }
            }
        }
    }
    return Get-FGPPSafe -Name $Name | Select-Object Name, AppliesTo
}

# FGPP Administrateurs IT (stricte)
$FGPP_IT = @{
    Name = "PSO-IT-Admins"
    PolicyParams = @{
        Name = "PSO-IT-Admins"
        ComplexityEnabled = $true
        LockoutDuration = (New-TimeSpan -Minutes 30)
        LockoutObservationWindow = (New-TimeSpan -Minutes 30)
        LockoutThreshold = 3
        MaxPasswordAge = (New-TimeSpan -Days 30)
        MinPasswordAge = (New-TimeSpan -Days 1)
        MinPasswordLength = 14
        PasswordHistoryCount = 24
        Precedence = 1
        ReversibleEncryptionEnabled = $false
        ProtectedFromAccidentalDeletion = $true
    }
    Groups = @("GRP-IT")
}

# FGPP Utilisateurs standards
$FGPP_USERS = @{
    Name = "PSO-Users"
    PolicyParams = @{
        Name = "PSO-Users"
        ComplexityEnabled = $true
        LockoutDuration = (New-TimeSpan -Minutes 15)
        LockoutObservationWindow = (New-TimeSpan -Minutes 15)
        LockoutThreshold = 5
        MaxPasswordAge = (New-TimeSpan -Days 30)
        MinPasswordAge = (New-TimeSpan -Days 1)
        MinPasswordLength = 8
        PasswordHistoryCount = 24
        Precedence = 10
        ReversibleEncryptionEnabled = $false
        ProtectedFromAccidentalDeletion = $true
    }
    Groups = @("GRP-RH", "GRP-Finance")
}

try {
    Write-Verbose "Déploiement FGPP - IT en cours"
    $resultIT = Ensure-FGPP @FGPP_IT

    Write-Verbose "Déploiement FGPP - Utilisateurs en cours"
    $resultUsers = Ensure-FGPP @FGPP_USERS

    [PSCustomObject]@{
        FGPP_IT    = $resultIT
        FGPP_Users = $resultUsers
    }
} catch {
    Write-Error "Échec du déploiement FGPP : $_"
    throw
}

# Vérification finale
Get-ADFineGrainedPasswordPolicy "PSO-IT-Admins" | Select-Object Name, AppliesTo
Get-ADUserResultantPasswordPolicy jfortin
Get-ADFineGrainedPasswordPolicy "PSO-Users" | Select-Object Name, AppliesTo
Get-ADUserResultantPasswordPolicy mtremblay
```

---

## 🎯 Compétences démontrées

- ✅ Installation et configuration d'Active Directory Domain Services
- ✅ Création et gestion d'un domaine Windows (anjoutech.lan)
- ✅ Automatisation via PowerShell et CSV
- ✅ Gestion des OUs, utilisateurs et groupes
- ✅ Fine-Grained Password Policy (PSO-IT-Admins + PSO-Users)
- ✅ Configuration DHCP (plage, exclusions, gateway, DNS)
- ✅ Jonction de postes Windows 11 au domaine
- ✅ Administration quotidienne (reset mdp, déverrouillage, connexion domaine)
