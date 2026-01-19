# Écrire des Specifications

Aide à transformer une idée vague en specs claires et vérifiables.

## Quand utiliser

- Avant de commencer à coder une nouvelle fonctionnalité
- Quand la demande est floue ou complexe
- Pour s'assurer qu'on est aligné sur l'objectif

## Instructions

Quand l'utilisateur invoque `/write-spec`, suivre ce processus :

### 1. Clarifier l'idée

Poser 2-3 questions pour comprendre :
- Quel est le problème à résoudre ?
- Qui va utiliser cette fonctionnalité ?
- Comment saura-t-on que c'est réussi ?

### 2. Rédiger les requirements en GIVEN/WHEN/THEN

Pour chaque comportement attendu, écrire :

```
REQ-1: [Nom descriptif]
- GIVEN [contexte ou état initial]
- WHEN [action de l'utilisateur ou déclencheur]
- THEN [résultat attendu, observable et vérifiable]
```

**Exemple :**
```
REQ-1: Validation email à la soumission
- GIVEN le formulaire d'inscription affiché
- WHEN l'utilisateur entre "test@invalid" et clique Soumettre
- THEN un message "Format d'email invalide" apparaît sous le champ
```

**Règles :**
- GIVEN = état de départ, pas d'action
- WHEN = une seule action déclencheuse
- THEN = résultat observable, pas d'implémentation

### 3. Définir les Non-Goals (OBLIGATOIRE)

Lister explicitement ce qui N'EST PAS inclus :

```
## Non-Goals (hors scope)
- [ ] [Ce qu'on ne fait PAS et pourquoi]
- [ ] [Autre exclusion]
```

**Exemples de non-goals :**
- Validation côté serveur (sera fait dans un autre ticket)
- Support IE11 (hors cible)
- Internationalisation (phase 2)

### 4. Valider avec l'utilisateur

Présenter les specs et demander :
> "Ces specs correspondent-elles à ce que tu veux ? Manque-t-il quelque chose ?"

Ne passer à l'implémentation qu'après validation explicite.

## Output attendu

```markdown
# Spec: [Nom de la fonctionnalité]

## Requirements

REQ-1: [Nom]
- GIVEN ...
- WHEN ...
- THEN ...

REQ-2: [Nom]
- GIVEN ...
- WHEN ...
- THEN ...

## Non-Goals
- [ ] ...
- [ ] ...

## Questions ouvertes (si applicable)
- [ ] ...
```

## Anti-patterns à éviter

- **Specs techniques** : "THEN la fonction retourne true" → Décrire le comportement utilisateur
- **WHEN multiples** : "WHEN l'user clique ET scroll" → Une action par requirement
- **THEN vagues** : "THEN ça marche" → Résultat observable spécifique
- **Non-goals oubliés** : Toujours en inclure, même si évidents
