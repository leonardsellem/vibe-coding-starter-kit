# Vibe-Coding Starter Kit

Kit de démarrage pour Claude Code.

## Installation

```bash
# Option 1 : Cloner dans un nouveau projet
git clone https://github.com/VOTRE_USERNAME/vibe-coding-starter.git mon-projet
cd mon-projet
rm -rf .git && git init

# Option 2 : Copier dans un projet existant
cp -r vibe-coding-starter/.claude mon-projet/
cp vibe-coding-starter/CLAUDE.md mon-projet/
```

## Contenu

```
├── CLAUDE.md                 # Contexte projet (à personnaliser)
└── .claude/
    └── skills/               # Méthodologie Vibe-Coding
        ├── write-spec.md     # /write-spec → Écrire des specs
        ├── citer-prompt.md   # /citer → Structurer un prompt
        ├── session-notes.md  # /notes → Documenter le travail
        └── verify.md         # /verify → Vérifier avant "c'est fait"
```

## Démarrage rapide

### 1. Personnaliser CLAUDE.md (5 min)

Ouvrez `CLAUDE.md` et remplissez :
- Nom et description du projet
- Stack technique
- Commandes vérifiées
- Structure du projet

### 2. Utiliser les skills

Les skills s'invoquent avec `/nom-du-skill` dans Claude Code :

| Commande | Quand l'utiliser |
|----------|------------------|
| `/write-spec` | Avant de coder, pour définir ce qu'on veut |
| `/citer` | Pour structurer une demande complexe |
| `/notes` | Toutes les 15 min ou après un milestone |
| `/verify` | Avant de dire "c'est terminé" |

### Exemple de session

```
Vous: /write-spec
      Je veux ajouter un bouton de déconnexion

[Claude vous guide à travers GIVEN/WHEN/THEN et Non-Goals]

Vous: Implémente selon ces specs

[Claude code...]

Vous: /verify

[Claude vérifie tests, lint, comportement]
```

## Workflow recommandé

```
IDÉE → SPEC → PLAN → EXEC
 2min   15min   5min   variable
```

1. **Idée** : Décrire informellement ce qu'on veut
2. **Spec** : `/write-spec` pour formaliser avec GIVEN/WHEN/THEN
3. **Plan** : Demander à Claude de lister les tâches
4. **Exec** : Implémenter puis `/verify` avant de conclure

## Progression suggérée

| Semaine | Focus |
|---------|-------|
| 1 | CLAUDE.md + `/write-spec` |
| 2 | Ajouter `/verify` systématiquement |
| 3 | Utiliser `/notes` pour les sessions longues |
| 4 | Maîtriser `/citer` pour les demandes complexes |

## Personnalisation

### Ajouter vos propres skills

Créez un fichier dans `.claude/skills/` :

```markdown
# Mon Skill

Description de ce que fait ce skill.

## Instructions

Quand l'utilisateur invoque ce skill :
1. Faire ceci
2. Puis cela
```

### Adapter CLAUDE.md par dossier

Le fichier CLAUDE.md le plus proche gagne (nearest-wins) :

```
mon-projet/
├── CLAUDE.md              # Règles globales
├── src/
│   └── api/
│       └── CLAUDE.md      # Règles spécifiques API
└── tests/
    └── CLAUDE.md          # Règles spécifiques tests
```

## Ressources

- [Documentation Claude Code](https://docs.anthropic.com/claude-code)
- [Webinaire Vibe-Coding](https://example.com/webinar) <!-- À remplacer -->

## Licence

MIT - Utilisez librement pour vos projets.
