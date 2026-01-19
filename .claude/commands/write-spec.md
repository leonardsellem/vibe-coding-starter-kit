---
description: Transforme une idée en specs claires (GIVEN/WHEN/THEN)
argument-hint: [idée ou fonctionnalité]
---

Aide l'utilisateur à transformer son idée en specs claires et vérifiables.

## Processus

1. **Clarifier l'idée** - Poser 2-3 questions :
   - Quel est le problème à résoudre ?
   - Qui va utiliser cette fonctionnalité ?
   - Comment saura-t-on que c'est réussi ?

2. **Rédiger les requirements en GIVEN/WHEN/THEN** :
   ```
   REQ-1: [Nom descriptif]
   - GIVEN [contexte ou état initial]
   - WHEN [action de l'utilisateur]
   - THEN [résultat observable et vérifiable]
   ```

3. **Définir les Non-Goals** (OBLIGATOIRE) :
   ```
   ## Non-Goals (hors scope)
   - [ ] [Ce qu'on ne fait PAS et pourquoi]
   ```

4. **Valider** : Demander confirmation avant de coder.

## Règles
- GIVEN = état de départ, pas d'action
- WHEN = une seule action déclencheuse
- THEN = résultat observable, pas d'implémentation

$ARGUMENTS
