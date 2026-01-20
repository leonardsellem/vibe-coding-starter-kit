# Documentation Quad-Fichier JIT

Documente le travail pour assurer la continuité entre sessions. Route automatiquement vers le fichier approprié.

## Architecture de documentation

| Fichier | Contenu | Fréquence MAJ |
|---------|---------|---------------|
| `LOG.md` | Changelog chronologique (Added/Changed/Fixed/Removed) | Après chaque changement |
| `CLAUDE.md` | Règles, conventions, structure pour agents | Quand patterns stables |
| `CONTEXT.md` | État actuel (context survival) | Fin de session |
| `README.md` | Synthèse générée depuis CLAUDE.md + LOG.md | Après `/documentation --sync` |

## Hiérarchie JIT (Just-In-Time)

```
CLAUDE.md (racine)          ← Global : snapshot projet, commandes, JIT map
├── src/CLAUDE.md           ← Spécifique : patterns src/, conventions code
├── tests/CLAUDE.md         ← Spécifique : conventions tests
└── docs/CLAUDE.md          ← Spécifique : conventions docs
```

**Principes :**
1. **Nearest-wins** : L'agent lit le CLAUDE.md le plus proche du fichier édité
2. **Racine légère** : Pas de détails spécifiques, seulement pointeurs
3. **Token-efficient** : Préférer chemins et commandes à prose

## Routage automatique

L'agent détermine le fichier cible selon le type de contenu :

| Type de contenu | → Fichier cible |
|-----------------|-----------------|
| Changement de code, bug fix, feature ajoutée | `LOG.md` |
| Règle, convention, pattern stable | `CLAUDE.md` (niveau approprié) |
| Structure de dossier, stack, commandes | `CLAUDE.md` racine |
| État actuel, handoff de session | `CONTEXT.md` |

## CONTEXT.md — Context Survival

### Pourquoi ?

> "Les notes survivent, ta conversation non"

Les agents ont une fenêtre de contexte limitée. Quand elle se remplit, les anciens messages sont *compactés* — l'agent oublie. CONTEXT.md permet de reprendre le travail sans tout ré-expliquer.

### Format

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

### Quand mettre à jour CONTEXT.md

- **Fin de session** — Avant de fermer Claude Code
- **Après un milestone** — Une feature terminée, un bug fixé
- **Quand la conversation devient longue** — Signe de compaction imminente
- **Avant une pause** — Pour reprendre plus tard

### Bonnes pratiques

1. **Écrire pour un futur agent** — Pas de références à "tantôt" ou "hier"
2. **Inclure les chemins de fichiers** — `src/api/orders.py:47` pas "le fichier orders"
3. **NEXT explicite** — La prochaine action concrète, pas "continuer"
4. **Nettoyer régulièrement** — Supprimer les COMPLETED obsolètes

## Instructions

### Quand utiliser

- Après chaque changement significatif de code
- Quand un pattern réutilisable émerge
- Avant de terminer une session
- Pour synchroniser README.md (`/documentation --sync`)

### Processus INTERACTIF

Quand l'utilisateur invoque `/documentation`, procéder ainsi :

#### Étape 1 : Quel type de documentation ?

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

#### Étape 2a : Si changement code → LOG.md

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

Puis demander les détails (fichiers:lignes, description).

#### Étape 2b : Si règle/pattern → CLAUDE.md

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

Si sous-dossier ou nouveau, demander le chemin.

```json
{
  "questions": [{
    "question": "Quelle section du CLAUDE.md ?",
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

#### Étape 2c : Si état actuel → CONTEXT.md

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

Puis collecter les détails pour chaque section sélectionnée.

#### Étape 2d : Si sync README

1. Lire `CLAUDE.md` racine pour snapshot projet
2. Lire `LOG.md` pour dernières entrées (5 dernières)
3. Générer `README.md` avec structure :
   - Titre + description (depuis CLAUDE.md)
   - Installation (depuis CLAUDE.md Commandes)
   - Derniers changements (depuis LOG.md)
   - Structure (depuis CLAUDE.md)

#### Étape 2e : Si session complète

Combiner :
1. Collecter changements → LOG.md
2. Collecter patterns découverts → CLAUDE.md approprié
3. **Mettre à jour CONTEXT.md** → état actuel pour la prochaine session
4. Proposer sync README

### Création JIT de CLAUDE.md

Quand un contenu spécifique à un sous-dossier est détecté et qu'aucun CLAUDE.md n'existe :

```markdown
# `[folder-name]/` (scope: `[folder-path]/**`)

Nearest-wins: priorité sur la racine.

## Conventions

- [contenu spécifique]

## Structure

- [si applicable]
```

## Format LOG.md (changelog)

```markdown
# Changelog

## [YYYY-MM-DD]

### Added
- Description avec `fichier:ligne` si pertinent

### Changed
- Description du changement

### Fixed
- Description du bug corrigé

### Removed
- Ce qui a été supprimé
```

## Format CLAUDE.md racine (léger)

```markdown
# CLAUDE.md

## Projet
- **Nom :** [nom]
- **Description :** [1-2 phrases]
- **Stack :** [technologies]

## Commandes
```bash
# Installer
[cmd]
# Tester
[cmd]
# Lancer
[cmd]
```

## JIT Map
- `src/` → `src/CLAUDE.md`
- `tests/` → `tests/CLAUDE.md`

## Conventions (globales seulement)
- [règle projet-wide]

## Interdit
- [ce que l'agent ne doit pas faire]
```

## Conseils

- **LOG.md** : Être factuel, inclure fichiers:lignes
- **CLAUDE.md** : Être prescriptif, actionnable
- **CONTEXT.md** : Écrire pour un futur agent qui n'a pas le contexte
- **Nearest-wins** : Plus le CLAUDE.md est proche, plus il est spécifique
- **Token efficiency** : Préférer `rg "pattern" src/` à copier du code
