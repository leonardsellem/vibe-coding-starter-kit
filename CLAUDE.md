# CLAUDE.md

## Projet

- **Nom :** Vibe-Coding Starter Kit
- **Description :** Starter kit pour le vibe-coding avec Claude Code. Inclut commandes slash et documentation JIT.

## Commandes

```bash
# Pas de build/test (repo de templates)
# Les commandes slash sont dans .claude/commands/
```

## JIT Map

Docs spécifiques par dossier (nearest-wins) :

- `.claude/commands/` → Commandes slash exécutables
- `.claude/skills/` → Documentation détaillée des skills
- `.claude/templates/` → Templates CLAUDE.md (racine et sous-dossier)

## Documentation tri-fichier

| Fichier | Rôle | MAJ |
|---------|------|-----|
| `LOG.md` | Changelog chronologique (Added/Changed/Fixed/Removed) | Après chaque changement |
| `CLAUDE.md` | Règles agent, conventions, structure | Patterns stables |
| `README.md` | Synthèse publique | `/documentation --sync` |

### Hiérarchie JIT

```
CLAUDE.md (racine)     ← Léger : snapshot, commandes, JIT map
├── src/CLAUDE.md      ← Détaillé : patterns spécifiques
└── tests/CLAUDE.md    ← Détaillé : conventions tests
```

**Principes :**
1. Nearest-wins : l'agent lit le CLAUDE.md le plus proche
2. Racine légère : pointeurs, pas détails
3. Token-efficient : préférer `rg` à copier du code

## Conventions

- Dates en ISO 8601 (YYYY-MM-DD)
- Volumes numériques purs (pas de "K" ou espaces)
- Encodage UTF-8 toujours
- Noms de colonnes en snake_case
- Commits en français avec préfixe `feat/fix/docs`
- Documentation triée : LOG.md pour historique, CLAUDE.md pour règles

## Commandes slash

| Commande | Description |
|----------|-------------|
| `/spec` | Transforme une idée en prompt CITER + specs GIVEN/WHEN/THEN |
| `/verify` | Vérifie qu'une tâche est terminée avant commit |
| `/commit` | Commit avec message français |
| `/documentation` | Documente le travail (LOG.md, CLAUDE.md, README.md) |
| `/documentation --sync` | Régénère README.md depuis CLAUDE.md + LOG.md |

## Interdit

- Ne pas supprimer de lignes sans raison documentée
- Ne pas modifier les données "métier" (juste le format)
- Toujours garder le fichier original intact
- Ne pas modifier `.env` ou fichiers de secrets
- Ne pas commit sans validation (`/commit` ou explicite)
- Ne pas ajouter de dépendances sans justification
