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
- Installation du rôle AD DS sur Windows Server 2022
- Promotion du serveur en contrôleur de domaine
- Création du domaine `anjoutech.lan`
- Configuration du DNS intégré à AD

### 2 — Structure organisationnelle
- Création de 3 unités organisationnelles (OUs) :
  - **RH** — Ressources Humaines
  - **Finance** — Département Finance
  - **IT** — Département Informatique

### 3 — Gestion des utilisateurs via PowerShell + CSV

| Utilisateur | OU | Mot de passe |
|---|---|---|
| mtremblay (Marie Tremblay) | RH | Unique par utilisateur |
| jgagnon (Jean Gagnon) | RH | Unique par utilisateur |
| slavoie (Sophie Lavoie) | Finance | Unique par utilisateur |
| mbouchard (Marc Bouchard) | Finance | Unique par utilisateur |
| acote (Alex Cote) | IT | Unique par utilisateur |
| jfortin (Julie Fortin) | IT | Unique par utilisateur |

### 4 — Groupes de sécurité
- **GRP-RH** — Groupe RH
- **GRP-Finance** — Groupe Finance
- **GRP-IT** — Groupe IT

### 5 — Jonction au domaine
- PC-W11-01 et PC-W11-02 joints au domaine anjoutech.lan
- Connexion des utilisateurs domaine validée

### 6 — Tâches administratives
- Réinitialisation de mots de passe
- Déverrouillage de comptes (`Unlock-ADAccount`)
- Simulation de tentatives de connexion échouées

### 7 — Configuration DHCP
- Plage IP : 10.0.2.100 — 10.0.2.200
- Exclusions : 10.0.2.201 — 10.0.2.254
- Gateway : 10.0.2.1
- DNS : SRV-DC01 (10.0.2.10)

---

## 🔧 Améliorations

### Fine-Grained Password Policy

| Politique | Groupe | Longueur min | Expiration | Verrouillage |
|---|---|---|---|---|
| PSO-IT-Admins | GRP-IT | 14 caractères | 30 jours | 3 tentatives |
| PSO-Users | GRP-RH, GRP-Finance | 8 caractères | 90 jours | 5 tentatives |

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

### Reset mot de passe
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
    $resultIT = Ensure-FGPP @FGPP_IT
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

## 📸 Screenshots

Les captures d'écran sont disponibles dans le dossier [screenshots](./screenshots/).

| Fichier | Description |
|---|---|
| 01-server-manager-dashboard.png | Server Manager Dashboard initial |
| 02-installation-type.png | Sélection du type d'installation |
| 03-server-selection.png | Sélection du serveur SRV-DC01 |
| 04-add-features-adds.png | Ajout des fonctionnalités AD DS |
| 05-confirmation-installation.png | Confirmation de l'installation |
| 06-installation-progress.png | Installation en cours |
| 07-post-deployment-promote.png | Promotion en contrôleur de domaine |
| 08-deployment-config-domain.png | Configuration domaine anjoutech.lan |
| 09-dc-options.png | Options du contrôleur de domaine |
| 10-netbios-anjoutech.png | Nom NetBIOS ANJOUTECH |
| 11-paths-ntds-sysvol.png | Chemins NTDS et SYSVOL |
| 12-script-install-addsforest.png | Script PowerShell Install-ADDSForest |
| 13-prerequisites-check-passed.png | Prérequis validés |
| 14-installation-dns.png | Installation DNS |
| 15-connexion-domaine.png | Première connexion ANJOUTECH\Administrator |
| 16-ouverture-dsamsc.png | Ouverture de dsa.msc |
| 17-creation-ou-rh.png | Création OU RH |
| 18-structure-ous.png | Structure OUs RH, Finance, IT |
| 19-creation-groupe-grp-finance.png | Création groupe GRP-Finance |
| 20-fichier-users-csv.png | Fichier users.csv initial |
| 21-script-create-adusers-ise.png | Script Create-ADUsers dans ISE |
| 22-script-execution-completed.png | Exécution script terminée |
| 23-ou-rh-utilisateurs.png | OU RH avec utilisateurs |
| 24-ou-finance-utilisateurs.png | OU Finance avec utilisateurs |
| 25-ou-it-utilisateurs.png | OU IT avec utilisateurs |
| 26-ajout-utilisateur-groupe-rh.png | Ajout utilisateur au groupe RH |
| 27-confirmation-ajout-groupe.png | Confirmation ajout au groupe |
| 28-fichier-users-csv-passwords.png | CSV avec mots de passe individuels |
| 29-script-update-passwords.png | Script reset mots de passe |
| 30-installation-windows11.png | Installation Windows 11 |
| 31-bureau-windows11.png | Bureau Windows 11 post-installation |
| 32-config-reseau-pc-w11-01.png | Config réseau PC-W11-01 |
| 33-ping-srv-dc01-reussi.png | Ping SRV-DC01 réussi |
| 34-infos-systeme-pc-w11-01.png | Infos système PC-WIN11-01 |
| 35-jonction-domaine-credentials.png | Jonction domaine — credentials |
| 36-authentification-admin-domaine.png | Authentification Administrator |
| 37-bienvenue-domaine-anjoutech.png | Bienvenue dans anjoutech.lan |
| 38-ecran-connexion-anjoutech.png | Écran connexion ANJOUTECH |
| 39-bureau-post-jonction-domaine.png | Bureau post-jonction domaine |
| 40-reset-password-mtremblay.png | Reset password mtremblay |
| 42-connexion-mtremblay-anjoutech.png | Connexion mtremblay |
| 43-echec-connexion-mdp-incorrect.png | Échec connexion — mauvais mot de passe |
| 44-tentative-connexion-invalide.png | Tentative invalide — retardant |
| 45-proprietes-mtremblay-unlock.png | Déverrouillage compte mtremblay |
| 46-bureau-windows11-mtremblay.png | Bureau connecté en tant que mtremblay |
| 47-dhcp-plage-ip.png | DHCP — Plage IP |
| 48-dhcp-exclusions.png | DHCP — Exclusions |
| 49-dhcp-gateway.png | DHCP — Gateway |
| 50-dhcp-dns-configuration.png | DHCP — DNS anjoutech.lan |
| 51-dhcp-scope-actif.png | DHCP — Scope actif |
| 52-ipconfig-dhcp-pc-w11-02.png | ipconfig PC-W11-02 DHCP |
| 53-ping-srv-dc01-reussi.png | Ping srv-dc01.anjoutech.lan réussi |

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
