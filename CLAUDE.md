# CLAUDE.md

## Projet

- **Nom :** Vibe-Coding Starter Kit
- **Description :** Starter kit pour le vibe-coding avec Claude Code. Inclut commandes slash et documentation JIT.
- **Stack :** Markdown + Claude Code skills

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

- Commits en français avec préfixe `feat/fix/docs`
- Documentation triée : LOG.md pour historique, CLAUDE.md pour règles

## Commandes slash

| Commande | Description |
|----------|-------------|
| `/write-spec` | Transforme une idée en specs GIVEN/WHEN/THEN |
| `/citer` | Structure un prompt avec le format CITER |
| `/documentation` | Documente le travail (LOG.md, CLAUDE.md, README.md) |
| `/documentation --sync` | Régénère README.md depuis CLAUDE.md + LOG.md |
| `/verify` | Vérifie qu'une tâche est terminée avant commit |
| `/commit` | Commit avec message français |

## Interdit

- Ne pas modifier `.env` ou fichiers de secrets
- Ne pas commit sans validation (`/commit` ou explicite)
- Ne pas ajouter de dépendances sans justification
