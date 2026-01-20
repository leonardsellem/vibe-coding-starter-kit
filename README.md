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
| `/spec` | Transforme une idée en prompt CITER + specs GIVEN/WHEN/THEN |
| `/verify` | Vérifie qu'une tâche est terminée avant commit |
| `/commit` | Commit avec message français |
| `/documentation` | Documente le travail (LOG.md, CLAUDE.md, README.md) |
| `/documentation --sync` | Régénère README.md depuis CLAUDE.md + LOG.md |

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
    │   ├── spec.md              # /spec — CITER + GIVEN/WHEN/THEN
    │   ├── verify.md            # /verify — Checklist avant commit
    │   ├── commit.md            # /commit — Message conventionnel
    │   └── documentation.md     # /documentation — LOG + README
    ├── skills/                  # Documentation détaillée
    │   ├── spec.md
    │   ├── verify.md
    │   ├── commit.md
    │   └── documentation.md
    └── templates/               # Templates CLAUDE.md
        ├── CLAUDE-root.md       # Template racine
        └── CLAUDE-subfolder.md  # Template sous-dossier
```

## Derniers changements

### 2026-01-20

**Added:**
- Commande `/spec` fusionnant CITER + GIVEN/WHEN/THEN en un seul flux
- Système de documentation tri-fichier JIT (LOG.md, CLAUDE.md, README.md)
- Templates CLAUDE.md (racine et sous-dossier)

**Changed:**
- Fusion de `/write-spec` et `/citer` en une seule commande `/spec`
- Architecture documentation : hiérarchie CLAUDE.md nearest-wins

**Removed:**
- `/write-spec` (remplacé par `/spec`)
- `/citer` (remplacé par `/spec`)

## Démarrage rapide

1. **Personnaliser CLAUDE.md** — Remplir nom, description, stack, conventions
2. **Utiliser `/spec`** — Avant de coder, pour définir le prompt structuré
3. **Documenter avec `/documentation`** — Après chaque changement significatif
4. **Vérifier avec `/verify`** — Avant de commit

## Licence

MIT - Utilisez librement pour vos projets.
