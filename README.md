# Guide d'utilisation - Templates Nuclei

## Prérequis

Assurez-vous d'avoir Nuclei installé :
```bash
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
```

Ou avec la version binaire :
```bash
# Télécharger depuis https://github.com/projectdiscovery/nuclei/releases
```

## Utilisation de base

### 1. Lancer le workflow principal (tous les tests)

```bash
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com
```

### 2. Lancer uniquement les technologies obsolètes

```bash
nuclei -w workflows/obsolete-technologies-workflow.yaml -u https://target.com
```

### 3. Lancer un workflow spécifique (ex: CKEditor)

```bash
nuclei -w workflows/ckeditor-workflow.yaml -u https://target.com
```

### 4. Lancer uniquement les détections (non-intrusif)

```bash
nuclei -t detection/file-upload/ -u https://target.com
```

### 5. Lancer uniquement les tests de vulnérabilités

```bash
nuclei -t vulnerabilities/file-upload/ -u https://target.com
```

### 6. Lancer un template spécifique

```bash
nuclei -t detection/file-upload/ckeditor-detection.yaml -u https://target.com
```

## Options avancées

### Avec plusieurs URLs (fichier)

```bash
nuclei -w workflows/file-upload-workflow.yaml -l urls.txt
```

### Avec tags spécifiques

```bash
# Tous les templates tagués "obsolete"
nuclei -t detection/file-upload/ -tags obsolete -u https://target.com

# Tous les templates tagués "ckeditor"
nuclei -t . -tags ckeditor -u https://target.com

# Tous les templates tagués "file-manager"
nuclei -t . -tags file-manager -u https://target.com
```

### Avec sévérité spécifique

```bash
# Seulement les détections (info)
nuclei -t detection/file-upload/ -severity info -u https://target.com

# Seulement les vulnérabilités critiques
nuclei -t vulnerabilities/file-upload/ -severity critical -u https://target.com

# Vulnérabilités high et critical
nuclei -t vulnerabilities/file-upload/ -severity high,critical -u https://target.com
```

### Avec rate limiting

```bash
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com -rate-limit 10
```

### Avec timeout personnalisé

```bash
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com -timeout 10
```

### Avec proxy

```bash
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com -proxy http://127.0.0.1:8080
```

### Avec headers personnalisés

```bash
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com -H "Authorization: Bearer token"
```

### Sauvegarder les résultats

```bash
# Format JSON
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com -o results.json -json

# Format Markdown
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com -o results.md -markdown-export

# Format CSV
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com -o results.csv -csv
```

### Mode silencieux (seulement les résultats)

```bash
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com -silent
```

### Mode verbose (plus de détails)

```bash
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com -verbose
```

## Exemples pratiques

### Scan complet d'un site

```bash
nuclei -w workflows/file-upload-workflow.yaml \
  -u https://target.com \
  -rate-limit 10 \
  -timeout 10 \
  -o results.json \
  -json \
  -silent
```

### Scan rapide (détections uniquement)

```bash
nuclei -t detection/file-upload/ \
  -u https://target.com \
  -rate-limit 20 \
  -o detections.json \
  -json
```

### Scan ciblé sur technologies obsolètes

```bash
nuclei -w workflows/obsolete-technologies-workflow.yaml \
  -u https://target.com \
  -severity high,critical \
  -o obsolete-results.json \
  -json
```

### Scan avec tags spécifiques

```bash
# Tous les file managers
nuclei -t . -tags file-manager -u https://target.com

# Tous les éditeurs
nuclei -t . -tags editor -u https://target.com

# Technologies obsolètes
nuclei -t . -tags obsolete -u https://target.com
```

### Scan de plusieurs cibles

```bash
# Fichier urls.txt contient une URL par ligne
nuclei -w workflows/file-upload-workflow.yaml -l urls.txt -o all-results.json -json
```

## Workflows disponibles

### Workflows spécifiques par technologie

- `workflows/ckeditor-workflow.yaml`
- `workflows/ckfinder-workflow.yaml`
- `workflows/tinymce-workflow.yaml`
- `workflows/elfinder-workflow.yaml`
- `workflows/kcfinder-workflow.yaml`
- `workflows/responsive-filemanager-workflow.yaml`
- `workflows/tiny-file-manager-workflow.yaml`
- `workflows/monsta-ftp-workflow.yaml`
- `workflows/filerun-workflow.yaml`
- `workflows/pydio-workflow.yaml`
- `workflows/filebrowser-workflow.yaml`
- `workflows/filegator-workflow.yaml`
- `workflows/roxy-fileman-workflow.yaml`
- `workflows/phpfm-workflow.yaml`
- `workflows/phpfilemanager-workflow.yaml`
- `workflows/cloudreve-workflow.yaml`
- `workflows/uppyio-workflow.yaml`

### Workflows généraux

- `workflows/file-upload-workflow.yaml` - Tous les tests
- `workflows/obsolete-technologies-workflow.yaml` - Technologies obsolètes uniquement

## Structure des résultats

Les résultats incluent :
- **ID du template** : Identifiant unique
- **Nom** : Nom du template
- **Sévérité** : info, low, medium, high, critical
- **URL** : URL ciblée
- **Matched** : Ce qui a été détecté
- **Extracted** : Données extraites (si applicable)
- **Timestamp** : Date et heure du scan

## Notes importantes

1. **Détections** : Généralement non-intrusives, juste des requêtes GET
2. **Tests de vulnérabilités** : Peuvent créer des fichiers de test sur le serveur
3. **Shells** : Les détections de shells PHP/ASP sont marquées comme critiques
4. **Rate limiting** : Utilisez `-rate-limit` pour éviter de surcharger le serveur
5. **Autorisation** : Certains tests peuvent nécessiter une authentification (non géré actuellement)

## Dépannage

### Erreur "template not found"
Assurez-vous d'être dans le bon répertoire ou utilisez des chemins absolus.

### Trop de requêtes
Utilisez `-rate-limit` pour limiter le nombre de requêtes par seconde.

### Timeout
Augmentez le timeout avec `-timeout 30` (en secondes).

### Résultats vides
Vérifiez que l'URL est accessible et que les chemins dans les templates correspondent à la structure du site.

