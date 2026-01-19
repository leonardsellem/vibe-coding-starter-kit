# Notes de Session

Documente le travail en cours pour assurer la continuité entre sessions.

## Quand utiliser

- Toutes les 10-15 minutes de travail
- Après chaque milestone complété
- Quand un blocage est rencontré
- Avant de terminer une session de travail

## Pourquoi c'est important

Les conversations avec Claude ne persistent pas. Si la session est longue ou interrompue, le contexte est perdu. Les notes écrites survivent.

## Instructions

Quand l'utilisateur invoque `/notes`, mettre à jour les notes de session avec le format structuré.

### Format de notes

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
- Ex: "En attente de la maquette pour le message d'erreur"

**DECISIONS:**
- [Choix faits et pourquoi]
- Ex: "Choisi validation au blur (pas onChange) pour éviter le spam"

**DISCOVERED:**
- [Découvertes inattendues, nouveaux problèmes]
- Ex: "Le CSS .error n'existe pas, il faudra le créer"
```

### Processus

1. **Demander le contexte** : "Où en es-tu ? Qu'est-ce qui a avancé ?"

2. **Remplir chaque section** :
   - COMPLETED : Lister les fichiers modifiés avec lignes
   - IN_PROGRESS : État exact + prochaine action
   - BLOCKERS : Seulement si applicable
   - DECISIONS : Choix non-évidents faits
   - DISCOVERED : Surprises ou travail additionnel identifié

3. **Proposer où sauvegarder** :
   - Fichier `NOTES.md` à la racine
   - Ou dans un commentaire de ticket/issue
   - Ou dans le système de notes de l'utilisateur

### Exemple complet

```markdown
## Session 2025-01-15

**COMPLETED:**
- Formulaire de login créé dans src/components/LoginForm.tsx
- Validation email côté client (lignes 23-45)
- Style d'erreur dans src/styles/forms.css:12-18

**IN_PROGRESS:**
- Tests unitaires pour LoginForm
- NEXT: Écrire test pour email invalide

**BLOCKERS:**
- Aucun

**DECISIONS:**
- Validation au blur plutôt qu'onChange (moins intrusif)
- Regex email simple, pas RFC 5322 complet (suffisant pour 99% des cas)

**DISCOVERED:**
- Le bouton Submit n'a pas de style disabled → créer ticket
- La fonction validateEmail existe déjà dans utils/validation.ts → réutiliser
```

## Conseils

- **Être spécifique** : "LoginForm.tsx:45" > "le formulaire"
- **Inclure le NEXT** : La prochaine action doit être évidente
- **Ne pas filtrer DISCOVERED** : Même les petites découvertes comptent
- **Mettre à jour, pas réécrire** : Ajouter aux notes existantes
