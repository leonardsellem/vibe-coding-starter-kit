---
description: Structure un prompt avec le format CITER
argument-hint: [demande brute]
---

Aide l'utilisateur à structurer sa demande avec le format CITER.

## Les 5 éléments

| Lettre | Élément | Question |
|--------|---------|----------|
| **C** | Context | Qu'est-ce qui existe déjà ? |
| **I** | Intent | Pourquoi je fais ça ? |
| **T** | Task | Quoi exactement ? |
| **E** | Expected | À quoi ressemble le succès ? |
| **R** | Restrictions | Quoi NE PAS faire ? |

## Processus

1. Demander l'idée brute si non fournie
2. Extraire chaque élément avec les questions ci-dessus
3. Reformuler en prompt structuré :

```
**Context :** [ce qui existe]
**Intent :** [pourquoi]
**Task :** [quoi faire]
**Expected :** [critères de succès]
**Restrictions :** [ce qu'il ne faut pas faire]
```

4. Demander : "Ce prompt te convient ? Je peux commencer ?"

$ARGUMENTS
