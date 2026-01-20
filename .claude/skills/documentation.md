# Documentation Tri-Fichier JIT

Documente le travail pour assurer la continuité entre sessions. Route automatiquement vers le fichier approprié.

## Architecture de documentation

| Fichier | Contenu | Fréquence MAJ |
|---------|---------|---------------|
| `LOG.md` | Changelog chronologique (Added/Changed/Fixed/Removed) | Après chaque changement |
| `CLAUDE.md` | Règles, conventions, structure pour agents | Quand patterns stables |
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

#### Étape 2c : Si sync README

1. Lire `CLAUDE.md` racine pour snapshot projet
2. Lire `LOG.md` pour dernières entrées (5 dernières)
3. Générer `README.md` avec structure :
   - Titre + description (depuis CLAUDE.md)
   - Installation (depuis CLAUDE.md Commandes)
   - Derniers changements (depuis LOG.md)
   - Structure (depuis CLAUDE.md)

#### Étape 2d : Si session complète

Combiner :
1. Collecter changements → LOG.md
2. Collecter patterns découverts → CLAUDE.md approprié
3. Proposer sync README

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
- **Nearest-wins** : Plus le CLAUDE.md est proche, plus il est spécifique
- **Token efficiency** : Préférer `rg "pattern" src/` à copier du code
