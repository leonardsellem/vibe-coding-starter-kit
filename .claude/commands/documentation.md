---
description: Documente le travail (LOG.md, CLAUDE.md, CONTEXT.md, README.md)
---

Met à jour la documentation du projet avec routage automatique vers le fichier approprié.

## Architecture

| Fichier | Contenu | Quand |
|---------|---------|-------|
| `LOG.md` | Changelog (Added/Changed/Fixed/Removed) | Changements de code |
| `CLAUDE.md` | Règles, conventions, structure | Patterns stables |
| `CONTEXT.md` | État actuel (context survival) | Fin de session |
| `README.md` | Synthèse générée | `/documentation --sync` |

## Commandes

- `/documentation` — Documentation interactive (détecte automatiquement le type)
- `/documentation --sync` — Régénère README.md depuis CLAUDE.md + LOG.md
- `/documentation --log` — Ajoute directement au LOG.md
- `/documentation --claude` — Ajoute directement au CLAUDE.md
- `/documentation --context` — Met à jour CONTEXT.md (état actuel)

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
      {"label": "État actuel", "description": "Context survival → CONTEXT.md"},
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

### Si état actuel → CONTEXT.md

**Pourquoi ?** Les conversations ont une fenêtre de contexte limitée. Quand elle se remplit, l'agent "oublie" les anciens messages. CONTEXT.md permet de reprendre le travail sans tout ré-expliquer.

Collecter les informations suivantes :

```json
{
  "questions": [{
    "question": "Quelle section mettre à jour ?",
    "header": "Section",
    "multiSelect": true,
    "options": [
      {"label": "COMPLETED", "description": "Ce qui a été fait (avec fichiers:lignes)"},
      {"label": "IN_PROGRESS", "description": "Tâche en cours + prochaine étape"},
      {"label": "BLOCKERS", "description": "Ce qui bloque la progression"},
      {"label": "DECISIONS", "description": "Choix faits et pourquoi"},
      {"label": "DISCOVERED", "description": "Problèmes trouvés, nouveaux besoins"},
      {"label": "Handoff", "description": "Notes de passage pour la prochaine session"}
    ]
  }]
}
```

Format d'écriture dans CONTEXT.md :
```markdown
# CONTEXT.md

## Snapshot
- **Date :** YYYY-MM-DD
- **Phase :** [setup / développement / refacto / debug]

## COMPLETED
- [description] dans `fichier:ligne`

## IN_PROGRESS
- [tâche] → NEXT: [prochaine action]

## BLOCKERS
- [blocker] — [raison]

## DECISIONS
- [décision] — [rationale]

## DISCOVERED
- [découverte] — [action suggérée]

## Handoff
SESSION_END: [résumé 1-2 lignes]
NEXT SESSION:
1. [action prioritaire]
2. [action suivante]
CONTEXT FILES:
- [fichier] (raison)
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

Combiner les 4 flux :
1. Demander changements → écrire LOG.md
2. Demander patterns → écrire CLAUDE.md approprié
3. **Mettre à jour CONTEXT.md** → état actuel pour la prochaine session
4. Proposer sync README

## Règles

- **LOG.md** : Factuel, avec `fichier:ligne` quand pertinent
- **CLAUDE.md** : Prescriptif, actionnable pour agents
- **CONTEXT.md** : Snapshot de l'état actuel, écrit pour un futur agent
- **Nearest-wins** : Le CLAUDE.md le plus proche prime
- **Token efficiency** : Préférer `rg "pattern" src/` à copier du code
