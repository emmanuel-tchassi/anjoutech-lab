# Projet 01 — Active Directory

## 📋 Description

Déploiement d'un environnement Active Directory complet sur Windows Server 2022 avec création d'un domaine d'entreprise, gestion des utilisateurs, groupes et unités organisationnelles.

---

## 🏗️ Infrastructure

| Machine | Rôle | IP |
|---|---|---|
| SRV-DC01 | Contrôleur de domaine | 10.0.2.10 |
| PC-W11-01 | Poste client RH | DHCP |
| PC-W11-02 | Poste client Finance | DHCP |

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

### 3 — Gestion des utilisateurs via PowerShell
Création de 6 utilisateurs via script PowerShell et fichier CSV :

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

---

## 🔧 Améliorations

### Fine-Grained Password Policy
Deux politiques de mot de passe différenciées selon le département :

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

### Déverrouillage de compte
```powershell
Unlock-ADAccount -Identity mtremblay
```

### Reset mot de passe
```powershell
Set-ADAccountPassword -Identity mtremblay `
    -NewPassword (ConvertTo-SecureString "NewPass@2024!" -AsPlainText -Force) `
    -Reset
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
- ✅ Création et gestion d'un domaine Windows
- ✅ Automatisation via PowerShell et CSV
- ✅ Gestion des OUs, utilisateurs et groupes
- ✅ Fine-Grained Password Policy
- ✅ Administration quotidienne (reset mdp, déverrouillage)
