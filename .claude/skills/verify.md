# Vérification (Definition of Done)

Vérifie qu'une tâche est réellement terminée avant de la déclarer complète.

## Quand utiliser

- Avant de dire "c'est fait" ou "c'est terminé"
- Avant de commit
- Avant de passer à la tâche suivante

## Pourquoi c'est important

"Ça a l'air de marcher" n'est pas une vérification. Les bugs non détectés coûtent plus cher à corriger plus tard.

## Instructions

Quand l'utilisateur invoque `/verify`, exécuter cette checklist dans l'ordre :

### Checklist de vérification

#### 1. Tests automatisés
```bash
[commande de test du projet]
```
- [ ] Tous les tests passent
- [ ] Les nouveaux comportements ont des tests

**Si échec** : Corriger avant de continuer.

#### 2. Lint / Formatage
```bash
[commande de lint du projet]
```
- [ ] Pas d'erreurs de lint
- [ ] Code formaté correctement

**Si échec** : Corriger ou justifier l'exception.

#### 3. Vérification manuelle

Tester le comportement réel :
- [ ] **Happy path** : Le cas normal fonctionne
- [ ] **Cas d'erreur** : Les erreurs sont gérées proprement
- [ ] **Edge cases** : Les cas limites identifiés dans les specs

**Comment tester** :
- Lancer l'app/le script
- Reproduire les scénarios des specs (GIVEN/WHEN/THEN)
- Vérifier visuellement ou via logs

#### 4. Review du diff
```bash
git diff
```
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
