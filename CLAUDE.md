# CLAUDE.md

## Projet

- **Nom :** n8n-
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

## Structure

```
src/
├── [dossier]/           # [description]
└── [fichier]            # [description]
tests/
└── ...
```

## Conventions

- [Ex: "Fonctions en snake_case, classes en PascalCase"]
- [Ex: "Commits en français avec préfixe feat/fix/docs"]
- [Ex: "Tests obligatoires pour les nouvelles fonctions"]

## Commandes slash

Commandes personnalisées définies dans `.claude/commands/` :

| Commande | Description |
|----------|-------------|
| `/write-spec` | Transforme une idée vague en specs claires (GIVEN/WHEN/THEN) |
| `/citer` | Structure un prompt avec le format CITER (Context/Intent/Task/Expected/Restrictions) |
| `/notes` | Documente le travail en cours pour assurer la continuité entre sessions |
| `/verify` | Vérifie qu'une tâche est terminée avant commit (tests, lint, manuel, diff) |

Voir aussi `.claude/skills/` pour la documentation détaillée de chaque commande.

## Ce que l'agent ne doit PAS faire

- Ne pas modifier `.env` ou fichiers de secrets
- Ne pas commit sans validation explicite
- Ne pas ajouter de dépendances sans justification
- Ne pas ignorer les erreurs de tests ou lint
