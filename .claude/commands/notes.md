---
description: Documente le travail en cours (notes de session)
---

Met à jour les notes de session pour assurer la continuité du travail.

## Format de notes

```markdown
## Session [DATE]

**COMPLETED:**
- [Ce qui est terminé, avec chemins de fichiers]
- Ex: "Validation email ajoutée dans src/components/LoginForm.tsx:45-67"

**IN_PROGRESS:**
- [État actuel + prochaine étape immédiate]
- Ex: "Tests de validation en cours. NEXT: ajouter cas d'erreur"

**BLOCKERS:**
- [Ce qui bloque, si applicable]

**DECISIONS:**
- [Choix faits et pourquoi]
- Ex: "Choisi validation au blur (pas onChange) pour éviter le spam"

**DISCOVERED:**
- [Découvertes inattendues, nouveaux problèmes]
```

## Processus

1. Demander : "Où en es-tu ? Qu'est-ce qui a avancé ?"
2. Remplir chaque section avec des chemins de fichiers précis
3. Proposer de sauvegarder dans `NOTES.md` à la racine

## Conseils
- Être spécifique : "LoginForm.tsx:45" > "le formulaire"
- Toujours inclure le NEXT dans IN_PROGRESS
- Ne pas filtrer DISCOVERED, même les petites choses
