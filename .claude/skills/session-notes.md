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

Quand l'utilisateur invoque `/notes`, collecter les informations de manière **INTERACTIVE** avec l'outil `AskUserQuestion`.

**RÈGLE IMPORTANTE** : Ne pas demander à l'utilisateur de décrire tout son travail d'un coup. Utiliser `AskUserQuestion` pour guider la collecte section par section.

### Processus INTERACTIF

1. **État actuel** via `AskUserQuestion` : où en es-tu ?
2. **COMPLETED** via `AskUserQuestion` : qu'est-ce qui a été terminé ?
3. **IN_PROGRESS** via `AskUserQuestion` : sur quoi travailles-tu ?
4. **BLOCKERS** via `AskUserQuestion` : y a-t-il des blocages ?
5. **DECISIONS** via `AskUserQuestion` : des choix importants ?
6. **DISCOVERED** via `AskUserQuestion` : des découvertes inattendues ?
7. **Destination** via `AskUserQuestion` : où sauvegarder ?

Voir `.claude/commands/notes.md` pour les questions prédéfinies.

### Format de notes (output)

```markdown
## Session [DATE]

**COMPLETED:**
- [Ce qui est terminé, avec chemins de fichiers]

**IN_PROGRESS:**
- [État actuel + prochaine étape immédiate]

**BLOCKERS:**
- [Ce qui bloque, si applicable]

**DECISIONS:**
- [Choix faits et pourquoi]

**DISCOVERED:**
- [Découvertes inattendues, nouveaux problèmes]
```

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
