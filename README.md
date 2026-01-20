# repo-scaffold

Starter kit pour le vibe-coding avec Claude Code. Inclut commandes slash et documentation JIT.

## Installation

```bash
# Cloner dans un nouveau projet
git clone https://github.com/VOTRE_USERNAME/repo-scaffold.git mon-projet
cd mon-projet
rm -rf .git && git init

# Ou copier dans un projet existant
cp -r repo-scaffold/.claude mon-projet/
cp repo-scaffold/CLAUDE.md mon-projet/
cp repo-scaffold/LOG.md mon-projet/
```

## Commandes slash

| Commande | Description |
|----------|-------------|
| `/write-spec` | Transforme une idée en specs GIVEN/WHEN/THEN |
| `/citer` | Structure un prompt avec le format CITER |
| `/documentation` | Documente le travail (LOG.md, CLAUDE.md, README.md) |
| `/documentation --sync` | Régénère README.md depuis CLAUDE.md + LOG.md |
| `/verify` | Vérifie qu'une tâche est terminée avant commit |
| `/commit` | Commit avec message français |

## Documentation tri-fichier

| Fichier | Rôle |
|---------|------|
| `LOG.md` | Changelog chronologique (Added/Changed/Fixed/Removed) |
| `CLAUDE.md` | Règles agent, conventions, structure |
| `README.md` | Synthèse publique (générée via `/documentation --sync`) |

### Hiérarchie JIT (Just-In-Time)

```
CLAUDE.md (racine)     ← Léger : snapshot, commandes, JIT map
├── src/CLAUDE.md      ← Détaillé : patterns spécifiques
└── tests/CLAUDE.md    ← Détaillé : conventions tests
```

**Principes :**
1. **Nearest-wins** : l'agent lit le CLAUDE.md le plus proche
2. **Racine légère** : pointeurs, pas détails
3. **Token-efficient** : préférer `rg` à copier du code

## Structure

```
repo-scaffold/
├── CLAUDE.md                    # Contexte projet (léger, JIT)
├── LOG.md                       # Changelog chronologique
├── README.md                    # Synthèse (ce fichier)
└── .claude/
    ├── commands/                # Commandes slash
    │   ├── citer.md
    │   ├── commit.md
    │   ├── documentation.md
    │   ├── verify.md
    │   └── write-spec.md
    ├── skills/                  # Documentation détaillée
    │   ├── citer-prompt.md
    │   ├── commit.md
    │   ├── documentation.md
    │   ├── verify.md
    │   └── write-spec.md
    └── templates/               # Templates CLAUDE.md
        ├── CLAUDE-root.md       # Template racine
        └── CLAUDE-subfolder.md  # Template sous-dossier
```

## Derniers changements

### 2026-01-20

**Added:**
- Système de documentation tri-fichier JIT (LOG.md, CLAUDE.md, README.md)
- Skill `documentation.md` avec routage automatique
- Commande `/documentation` avec options `--sync`, `--log`, `--claude`
- Templates CLAUDE.md (racine et sous-dossier)

**Changed:**
- Architecture documentation : hiérarchie CLAUDE.md nearest-wins
- Renommage `session-notes` → `documentation`

## Démarrage rapide

1. **Personnaliser CLAUDE.md** — Remplir nom, description, stack, commandes
2. **Utiliser `/write-spec`** — Avant de coder, pour définir les specs
3. **Documenter avec `/documentation`** — Après chaque changement significatif
4. **Vérifier avec `/verify`** — Avant de commit

## Licence

MIT - Utilisez librement pour vos projets.
