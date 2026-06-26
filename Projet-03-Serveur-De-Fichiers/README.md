# Projet 03 — Serveur de fichiers

## 📋 Description

Configuration d'un serveur de fichiers sur Windows Server 2022 avec partages réseau SMB, permissions NTFS granulaires par département et quotas de disque via FSRM (File Server Resource Manager).

---

## 🏗️ Structure des partages

| Dossier | Chemin | Partage réseau | Groupe autorisé |
|---|---|---|---|
| RH | C:\Partages\RH | \\SRV-DC01\RH | GRP-RH |
| Finance | C:\Partages\Finance | \\SRV-DC01\Finance | GRP-Finance |
| IT | C:\Partages\IT | \\SRV-DC01\IT | GRP-IT |

---

## ✅ Réalisations

### 1 — Création des dossiers
Création automatique avec vérification d'existence :

### 2 — Partages SMB
Partages réseau créés avec accès restreint par groupe AD.

### 3 — Permissions NTFS
- Héritage désactivé sur chaque dossier
- Permissions explicites par groupe AD (Modify)
- Isolation totale entre départements

### 4 — Quotas de disque (FSRM)
- Quota de **200 MB** par dossier département
- Alertes automatiques à 85% et 95%

### 5 — Audit d'accès aux fichiers
- Audit activé sur les 3 dossiers
- Événement **4663** — accès aux objets tracé
- Visibilité dans l'Observateur d'événements

---

## 🔍 Validation des accès

| Test | Résultat |
|---|---|
| mtremblay (RH) → \\SRV-DC01\RH | ✅ Accès autorisé |
| mtremblay (RH) → \\SRV-DC01\Finance | ❌ Accès refusé |
| slavoie (Finance) → \\SRV-DC01\Finance | ✅ Accès autorisé |
| slavoie (Finance) → \\SRV-DC01\RH | ❌ Accès refusé |

---

## 📝 Scripts PowerShell

### Création des dossiers avec vérification
```powershell
$dossiers = @("C:\Partages\RH", "C:\Partages\Finance", "C:\Partages\IT")

foreach ($dossier in $dossiers) {
    if (Test-Path $dossier) {
        Write-Host "$dossier — EXISTE DÉJÀ" -ForegroundColor Yellow
    } else {
        New-Item -Path $dossier -ItemType Directory | Out-Null
        Write-Host "$dossier — CRÉÉ AVEC SUCCÈS" -ForegroundColor Green
    }
}
```

### Création des partages SMB avec vérification
```powershell
$partages = @(
    @{Name="RH"; Path="C:\Partages\RH"; Group="ANJOUTECH\GRP-RH"},
    @{Name="Finance"; Path="C:\Partages\Finance"; Group="ANJOUTECH\GRP-Finance"},
    @{Name="IT"; Path="C:\Partages\IT"; Group="ANJOUTECH\GRP-IT"}
)

foreach ($p in $partages) {
    if (Get-SmbShare -Name $p.Name -ErrorAction SilentlyContinue) {
        Write-Host "Partage $($p.Name) — EXISTE DÉJÀ" -ForegroundColor Yellow
    } else {
        New-SmbShare -Name $p.Name -Path $p.Path -FullAccess $p.Group | Out-Null
        Write-Host "Partage $($p.Name) — CRÉÉ AVEC SUCCÈS" -ForegroundColor Green
    }
}
```

### Configuration des permissions NTFS
```powershell
$dossiers = @(
    @{Path="C:\Partages\RH"; Group="ANJOUTECH\GRP-RH"},
    @{Path="C:\Partages\Finance"; Group="ANJOUTECH\GRP-Finance"},
    @{Path="C:\Partages\IT"; Group="ANJOUTECH\GRP-IT"}
)

foreach ($d in $dossiers) {
    try {
        $acl = Get-Acl $d.Path
        $acl.SetAccessRuleProtection($true, $false)
        $rule = New-Object System.Security.AccessControl.FileSystemAccessRule(
            $d.Group, "Modify",
            "ContainerInherit,ObjectInherit",
            "None", "Allow"
        )
        $acl.SetAccessRule($rule)
        Set-Acl $d.Path $acl
        Write-Host "Permissions NTFS $($d.Path) — CONFIGURÉES" -ForegroundColor Green
    } catch {
        Write-Host "Erreur $($d.Path) : $_" -ForegroundColor Red
    }
}
```

### Activation de l'audit d'accès aux fichiers
```powershell
# Activer l'audit sur SRV-DC01
auditpol /set /subcategory:"File System" /success:enable /failure:enable

# Configurer l'audit NTFS sur le dossier RH
$acl = Get-Acl "C:\Partages\RH"
$auditRule = New-Object System.Security.AccessControl.FileSystemAuditRule(
    "Everyone", "Read,Write,Delete",
    "ContainerInherit,ObjectInherit",
    "None", "Success,Failure"
)
$acl.AddAuditRule($auditRule)
Set-Acl "C:\Partages\RH" $acl
Write-Host "Audit RH configuré" -ForegroundColor Green
```

---

## 🎯 Compétences démontrées

- ✅ Création et gestion de partages réseau SMB
- ✅ Configuration des permissions NTFS granulaires
- ✅ Isolation des accès par groupe Active Directory
- ✅ Installation et configuration de FSRM
- ✅ Quotas de disque par département
- ✅ Audit d'accès aux fichiers (événement 4663)
- ✅ Automatisation via PowerShell
