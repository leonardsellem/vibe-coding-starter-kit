# Vérification (Definition of Done)

Vérifie qu'une tâche est réellement terminée avant de la déclarer complète.

## Quand utiliser

- Avant de dire "c'est fait" ou "c'est terminé"
- Avant de commit
- Avant de passer à la tâche suivante

## Pourquoi c'est important

"Ça a l'air de marcher" n'est pas une vérification. Les bugs non détectés coûtent plus cher à corriger plus tard.

## Instructions

Quand l'utilisateur invoque `/verify`, exécuter la checklist de manière **INTERACTIVE** avec l'outil `AskUserQuestion`.

**RÈGLE IMPORTANTE** : Exécuter les commandes (test, lint, diff), puis utiliser `AskUserQuestion` pour confirmer chaque étape avec l'utilisateur avant de passer à la suivante.

### Processus INTERACTIF

1. **Package manager** via `AskUserQuestion` : npm/yarn/pnpm/bun ?
2. **Exécuter tests** puis confirmer via `AskUserQuestion`
3. **Exécuter lint** puis confirmer via `AskUserQuestion`
4. **Vérification manuelle** via `AskUserQuestion` : happy path, erreurs, edge cases ?
5. **Review diff** : afficher `git diff`, puis confirmer via `AskUserQuestion`
6. **Specs satisfaites** via `AskUserQuestion`
7. **Action finale** via `AskUserQuestion` : commit, corriger, ou résumé ?

Voir `.claude/commands/verify.md` pour les questions prédéfinies.

### Checklist de vérification

#### 1. Tests automatisés
- [ ] Tous les tests passent
- [ ] Les nouveaux comportements ont des tests

#### 2. Lint / Formatage
- [ ] Pas d'erreurs de lint
- [ ] Code formaté correctement

#### 3. Vérification manuelle
- [ ] **Happy path** : Le cas normal fonctionne
- [ ] **Cas d'erreur** : Les erreurs sont gérées proprement
- [ ] **Edge cases** : Les cas limites identifiés dans les specs

#### 4. Review du diff
- [ ] Pas de fichiers inattendus modifiés
- [ ] Pas de code commenté "au cas où"
- [ ] Pas de console.log / print de debug oubliés
- [ ] Pas de secrets ou données sensibles

#### 5. Vérification finale
- [ ] Les specs initiales sont satisfaites
- [ ] Les non-goals n'ont pas été implémentés par erreur
- [ ] La documentation est à jour (si applicable)

### Output attendu

Après vérification, résumer :

```markdown
## Vérification complète

- [x] Tests : 12 passed, 0 failed
- [x] Lint : aucune erreur
- [x] Manuel : happy path OK, erreur email OK
- [x] Diff : 3 fichiers, pas de secrets
- [x] Specs : REQ-1 et REQ-2 satisfaits

**Prêt à commit.**
```

Ou si problème :

```markdown
## Vérification incomplète

- [x] Tests : OK
- [ ] Lint : 2 warnings (unused variable ligne 34)
- [x] Manuel : OK

**Action requise** : Corriger le warning avant commit.
```

## Ce qui n'est PAS une vérification

| Mauvais | Pourquoi |
|---------|----------|
| "J'ai regardé, ça a l'air bon" | Pas de preuve |
| "Les tests passaient avant" | Pas relancés |
| "C'est un petit changement" | Pas d'exception |
| "Je vérifierai plus tard" | Non |

## Règle absolue

> **Ne jamais dire "c'est fait" sans avoir exécuté cette checklist.**

Si une étape ne peut pas être faite (pas de tests, pas de lint), le signaler explicitement et expliquer pourquoi.
