---
description: Documente le travail (LOG.md, CLAUDE.md, README.md)
---

Met à jour la documentation du projet avec routage automatique vers le fichier approprié.

## Architecture

| Fichier | Contenu | Quand |
|---------|---------|-------|
| `LOG.md` | Changelog (Added/Changed/Fixed/Removed) | Changements de code |
| `CLAUDE.md` | Règles, conventions, structure | Patterns stables |
| `README.md` | Synthèse générée | `/documentation --sync` |

## Commandes

- `/documentation` — Documentation interactive (détecte automatiquement le type)
- `/documentation --sync` — Régénère README.md depuis CLAUDE.md + LOG.md
- `/documentation --log` — Ajoute directement au LOG.md
- `/documentation --claude` — Ajoute directement au CLAUDE.md

## Processus INTERACTIF

### Étape 1 : Type de documentation

```json
{
  "questions": [{
    "question": "Que veux-tu documenter ?",
    "header": "Type",
    "multiSelect": false,
    "options": [
      {"label": "Changement code", "description": "Bug fix, feature, refacto → LOG.md"},
      {"label": "Règle/Pattern", "description": "Convention, architecture → CLAUDE.md"},
      {"label": "Sync README", "description": "Générer README.md depuis les autres fichiers"},
      {"label": "Session complète", "description": "Documenter tout avant de partir"}
    ]
  }]
}
```

### Si changement code → LOG.md

```json
{
  "questions": [{
    "question": "Quel type de changement ?",
    "header": "Changelog",
    "multiSelect": true,
    "options": [
      {"label": "Added", "description": "Nouvelle fonctionnalité, fichier, module"},
      {"label": "Changed", "description": "Modification de comportement existant"},
      {"label": "Fixed", "description": "Correction de bug"},
      {"label": "Removed", "description": "Suppression de code, dépendance"}
    ]
  }]
}
```

Puis collecter les détails : fichiers modifiés, description.

Format d'écriture dans LOG.md :
```markdown
## [YYYY-MM-DD]

### Added
- Description avec `fichier:ligne`

### Changed
- Description

### Fixed
- Description
```

### Si règle/pattern → CLAUDE.md

```json
{
  "questions": [{
    "question": "Quel niveau de CLAUDE.md ?",
    "header": "Scope",
    "multiSelect": false,
    "options": [
      {"label": "Racine", "description": "S'applique à tout le projet"},
      {"label": "Sous-dossier", "description": "Spécifique à un dossier (src/, tests/, etc.)"},
      {"label": "Créer nouveau", "description": "Nouveau CLAUDE.md dans un dossier"}
    ]
  }]
}
```

```json
{
  "questions": [{
    "question": "Quelle section ?",
    "header": "Section",
    "multiSelect": false,
    "options": [
      {"label": "Conventions", "description": "Règles de code, nommage, style"},
      {"label": "Structure", "description": "Organisation des fichiers"},
      {"label": "Commandes", "description": "Scripts, CLI"},
      {"label": "Interdit", "description": "Ce que l'agent ne doit PAS faire"}
    ]
  }]
}
```

Si création d'un nouveau CLAUDE.md, utiliser ce template :
```markdown
# `[folder-name]/` (scope: `[folder-path]/**`)

Nearest-wins: priorité sur la racine.

## Conventions

- [contenu]
```

### Si sync README

1. Lire `CLAUDE.md` → extraire Projet, Commandes, Structure
2. Lire `LOG.md` → extraire 5 dernières entrées
3. Générer `README.md` :

```markdown
# [Nom du projet]

[Description depuis CLAUDE.md]

## Installation

```bash
[commandes depuis CLAUDE.md]
```

## Derniers changements

[5 dernières entrées de LOG.md]

## Structure

[depuis CLAUDE.md]
```

### Si session complète

Combiner les 3 flux :
1. Demander changements → écrire LOG.md
2. Demander patterns → écrire CLAUDE.md approprié
3. Proposer sync README

## Règles

- **LOG.md** : Factuel, avec `fichier:ligne` quand pertinent
- **CLAUDE.md** : Prescriptif, actionnable pour agents
- **Nearest-wins** : Le CLAUDE.md le plus proche prime
- **Token efficiency** : Préférer `rg "pattern" src/` à copier du code
