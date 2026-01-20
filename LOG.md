# Changelog

Historique chronologique des changements du projet.

## [2026-01-20]

### Added
- Système de documentation tri-fichier JIT (LOG.md, CLAUDE.md, README.md)
- Skill `documentation.md` avec routage automatique
- Commande `/documentation` avec options `--sync`, `--log`, `--claude`
- Templates CLAUDE.md (racine et sous-dossier) dans `.claude/templates/`
- Commande `/commit` avec génération de messages en français
- Skill `commit.md` pour la convention de commit

### Changed
- Architecture documentation : hiérarchie CLAUDE.md nearest-wins
- `/documentation` route automatiquement vers le fichier approprié
- Renommage `session-notes` → `documentation`

### Removed
- `.claude/commands/notes.md` (remplacé par `documentation.md`)
- `.claude/skills/session-notes.md` (remplacé par `documentation.md`)

## [2026-01-19]

### Added
- **Initial commit** : Vibe-Coding Starter Kit
  - `.claude/skills/write-spec.md` — Specs GIVEN/WHEN/THEN
  - `.claude/skills/citer-prompt.md` — Format CITER pour prompts
  - `.claude/skills/session-notes.md` — Notes de session
  - `.claude/skills/verify.md` — Vérification avant commit
  - `CLAUDE.md` — Contexte projet template
  - `README.md` — Documentation utilisateur
- Commandes slash dans `.claude/commands/`
  - `/write-spec` — Transformer idées en specs
  - `/citer` — Structurer prompts complexes
  - `/notes` — Documenter le travail
  - `/verify` — Vérifier avant "c'est fait"

### Changed
- Refactorisation des descriptions de commandes pour usage interactif
- Suppression lien webinar mort dans README

### Fixed
- Lien webinar cassé supprimé
