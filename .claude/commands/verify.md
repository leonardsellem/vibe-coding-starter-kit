---
description: Vérifie qu'une tâche est terminée avant commit
allowed-tools: Bash(npm test:*), Bash(npm run lint:*), Bash(git diff:*), Bash(git status:*)
---

Exécute la checklist de vérification avant de déclarer une tâche terminée.

## Checklist (dans l'ordre)

### 1. Tests automatisés
```bash
npm test
```
- [ ] Tous les tests passent
- [ ] Les nouveaux comportements ont des tests

### 2. Lint / Formatage
```bash
npm run lint
```
- [ ] Pas d'erreurs de lint
- [ ] Code formaté correctement

### 3. Vérification manuelle
- [ ] **Happy path** : Le cas normal fonctionne
- [ ] **Cas d'erreur** : Les erreurs sont gérées
- [ ] **Edge cases** : Les cas limites des specs

### 4. Review du diff
```bash
git diff
```
- [ ] Pas de fichiers inattendus
- [ ] Pas de code commenté "au cas où"
- [ ] Pas de console.log de debug
- [ ] Pas de secrets

### 5. Specs satisfaites
- [ ] Les requirements initiaux sont OK
- [ ] Les non-goals n'ont pas été implémentés par erreur

## Output

Résumer avec :
```markdown
## Vérification complète

- [x] Tests : X passed, 0 failed
- [x] Lint : aucune erreur
- [x] Manuel : happy path OK, erreurs OK
- [x] Diff : N fichiers, pas de secrets
- [x] Specs : REQ-1 et REQ-2 satisfaits

**Prêt à commit.**
```

## Règle absolue

> **Ne jamais dire "c'est fait" sans avoir exécuté cette checklist.**
