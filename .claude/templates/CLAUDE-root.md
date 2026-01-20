# CLAUDE.md

## Projet

- **Nom :** [nom-du-projet]
- **Description :** [1-2 phrases sur ce que fait le projet]
- **Stack :** [ex: Python 3.11 + FastAPI | Node 20 + React | etc.]

## Commandes

```bash
# Installer les dépendances
[commande]

# Lancer les tests
[commande]

# Lancer en développement
[commande]

# Vérifier le code (lint)
[commande]
```

## JIT Map

Docs spécifiques par dossier (nearest-wins) :

- `src/` → `src/CLAUDE.md` (conventions code)
- `tests/` → `tests/CLAUDE.md` (conventions tests)

## Conventions (globales)

- [Ex: "Commits en français avec préfixe feat/fix/docs"]
- [Ex: "Tests obligatoires pour les nouvelles fonctions"]

## Commandes slash

| Commande | Description |
|----------|-------------|
| `/write-spec` | Transforme une idée en specs GIVEN/WHEN/THEN |
| `/citer` | Structure un prompt avec le format CITER |
| `/documentation` | Documente le travail (LOG.md, CLAUDE.md, README.md) |
| `/verify` | Vérifie qu'une tâche est terminée avant commit |
| `/commit` | Commit avec message français |

## Interdit

- Ne pas modifier `.env` ou fichiers de secrets
- Ne pas commit sans validation (`/commit` ou explicite)
- Ne pas ajouter de dépendances sans justification
- Ne pas ignorer les erreurs de tests ou lint
