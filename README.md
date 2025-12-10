# Templates Nuclei - Reconnaissance et Tests de Sécurité

Ce dépôt contient des templates Nuclei organisés pour la reconnaissance et les tests de sécurité, avec un focus sur les technologies d'upload et de visualisation de fichiers.

## Structure

```
.
├── workflows/              # Workflows Nuclei qui orchestrent les tests
├── detection/              # Templates de détection par technologie
│   └── file-upload/        # Détections pour les systèmes d'upload
├── vulnerabilities/        # Templates de tests de vulnérabilités
│   └── file-upload/        # Tests de vulnérabilités pour les systèmes d'upload
└── file-upload-templates/  # Ancien dossier (à migrer)
```

## Workflows

Les workflows permettent d'exécuter automatiquement une détection, puis si la technologie est trouvée, de lancer les tests de vulnérabilités associés.

### Workflow principal
- `file-upload-workflow.yaml` - Exécute tous les tests pour les systèmes d'upload

### Workflows spécifiques
Chaque technologie a son propre workflow :
- `ckeditor-workflow.yaml`
- `ckfinder-workflow.yaml`
- `tinymce-workflow.yaml`
- `elfinder-workflow.yaml`
- `kcfinder-workflow.yaml`
- `responsive-filemanager-workflow.yaml`
- `tiny-file-manager-workflow.yaml`
- `monsta-ftp-workflow.yaml`
- `filerun-workflow.yaml`
- `pydio-workflow.yaml`
- `filebrowser-workflow.yaml`
- `filegator-workflow.yaml`
- `roxy-fileman-workflow.yaml`
- `phpfm-workflow.yaml`
- `phpfilemanager-workflow.yaml`
- `cloudreve-workflow.yaml`
- `uppyio-workflow.yaml`

## Technologies couvertes

### Éditeurs de texte riches
- CKEditor
- TinyMCE
- Froala Editor
- Summernote

### Gestionnaires de fichiers populaires
- CKFinder
- elFinder
- KCFinder
- Responsive FileManager
- PHP File Manager
- Ajax File Manager

### Gestionnaires de fichiers moins connus
- Tiny File Manager
- Monsta FTP
- FileRun
- Pydio (AjaXplorer)
- Seafile
- ownCloud
- Nextcloud
- FileBrowser
- FileGator
- Roxy Fileman
- PHPFM
- ExplorerJS
- PHPFileManager
- Cloudreve
- h5ai
- Directory Lister

### Bibliothèques d'upload
- Uploadify
- Plupload
- Dropzone
- Fine Uploader
- jQuery File Upload (blueimp)
- Angular File Upload
- Uploadcare
- Uppy
- Flow.js
- Uppload
- FilePond
- Uppy.io Companion

### Protocoles
- WebDAV

## Utilisation

### Exécuter un workflow complet
```bash
nuclei -w workflows/file-upload-workflow.yaml -u https://target.com
```

### Exécuter un workflow spécifique
```bash
nuclei -w workflows/ckeditor-workflow.yaml -u https://target.com
```

### Exécuter uniquement les détections
```bash
nuclei -t detection/file-upload/ -u https://target.com
```

### Exécuter uniquement les tests de vulnérabilités
```bash
nuclei -t vulnerabilities/file-upload/ -u https://target.com
```

### Exécuter un template spécifique
```bash
nuclei -t detection/file-upload/ckeditor-detection.yaml -u https://target.com
```

## Format des workflows

Les workflows suivent ce format :

```yaml
id: technology-workflow

info:
  name: Technology Security Checks
  author: nuclei-templates
  description: Description du workflow

workflows:
  - template: detection/file-upload/technology-detection.yaml
    subtemplates:
      - template: vulnerabilities/file-upload/technology-upload.yaml
      - tags: technology
```

Le workflow lance d'abord la détection. Si la technologie est trouvée, il exécute automatiquement les templates de vulnérabilités associés.

## Tags

Chaque template est tagué pour faciliter la recherche et l'exécution sélective :

- `file-upload` - Tous les templates d'upload
- `file-manager` - Gestionnaires de fichiers
- `editor` - Éditeurs de texte riches
- `upload` - Tests d'upload
- `security` - Tests de sécurité
- Tags spécifiques par technologie (ex: `ckeditor`, `tinymce`, etc.)

## Notes importantes

- Les templates de détection sont généralement non-intrusifs
- Les templates d'upload peuvent créer des fichiers de test sur le serveur
- Les templates de sécurité testent des vulnérabilités connues
- Tous les tests sont conçus pour être exécutés depuis l'extérieur sans authentification

# nuclei-upload-blackbox-worflows
