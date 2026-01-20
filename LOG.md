# Changelog

Historique chronologique des changements du projet.

## [2026-01-20]

### Added
- Système de documentation tri-fichier JIT (LOG.md, CLAUDE.md, README.md)
- Skill `documentation.md` avec routage automatique
- Commande `/documentation` avec options `--sync`, `--log`, `--claude`
- Template LOG.md format changelog (Added/Changed/Fixed/Removed)
- Templates CLAUDE.md (racine et sous-dossier) dans `.claude/templates/`

### Changed
- Architecture documentation : hiérarchie CLAUDE.md nearest-wins
- `/documentation` route automatiquement vers le fichier approprié
- Renommage `session-notes` → `documentation`
