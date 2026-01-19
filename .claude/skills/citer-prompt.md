# Structurer un Prompt (CITER)

Aide à formuler des demandes claires et complètes à l'agent.

## Quand utiliser

- Pour des demandes complexes ou multi-étapes
- Quand les résultats précédents n'étaient pas satisfaisants
- Pour des tâches critiques où la précision compte

## Instructions

Quand l'utilisateur invoque `/citer`, l'aider à structurer sa demande avec les 5 éléments CITER en utilisant l'outil `AskUserQuestion`.

**RÈGLE IMPORTANTE** : Ne jamais poser les 5 questions CITER d'un coup. Procéder élément par élément avec `AskUserQuestion`, attendre chaque réponse avant de continuer.

### Les 5 éléments

| Lettre | Élément | Question |
|--------|---------|----------|
| **C** | Context | Qu'est-ce qui existe déjà ? |
| **I** | Intent | Pourquoi je fais ça ? |
| **T** | Task | Quoi exactement ? |
| **E** | Expected | À quoi ressemble le succès ? |
| **R** | Restrictions | Quoi NE PAS faire ? |

### Processus INTERACTIF

1. **Collecter C (Context)** via `AskUserQuestion`
2. **Collecter I (Intent)** via `AskUserQuestion`
3. **Collecter T (Task)** via `AskUserQuestion`
4. **Collecter E (Expected)** via `AskUserQuestion`
5. **Collecter R (Restrictions)** via `AskUserQuestion`
6. **Reformuler et valider** via `AskUserQuestion`

Voir `.claude/commands/citer.md` pour les questions prédéfinies.

### Exemple

**Demande brute :**
> "Ajoute de la validation au formulaire"

**Prompt CITER :**
```
C (Context): J'ai un formulaire React dans src/components/LoginForm.tsx
             avec des champs email et password, sans validation actuelle.

I (Intent): Améliorer l'UX en évitant les soumissions invalides
            et les allers-retours serveur inutiles.

T (Task): Ajouter une validation email côté client qui vérifie
          le format avant soumission.

E (Expected):
- Message d'erreur rouge sous le champ si email invalide
- Bouton Submit désactivé tant que l'email est invalide
- Validation au blur (perte de focus), pas à chaque frappe

R (Restrictions):
- Ne pas modifier le CSS existant (utiliser les classes .error déjà définies)
- Pas de librairie externe (validation native uniquement)
- Ne pas toucher au champ password pour l'instant
```

## Output attendu

Reformuler la demande de l'utilisateur au format :

```
**Context :** [ce qui existe]

**Intent :** [pourquoi]

**Task :** [quoi faire]

**Expected :** [critères de succès]

**Restrictions :** [ce qu'il ne faut pas faire]
```

Puis demander : "Ce prompt te convient ? Je peux commencer ?"

## Anti-patterns

| Mauvais | Pourquoi | Mieux |
|---------|----------|-------|
| C: "Le projet" | Trop vague | C: "LoginForm.tsx ligne 45" |
| I: absent | On ne sait pas pourquoi | I: "Réduire les erreurs 500" |
| T: "Améliore le code" | Pas actionnable | T: "Ajoute validation email" |
| E: "Ça marche" | Pas vérifiable | E: "Message rouge si invalide" |
| R: absent | Risque de sur-engineering | R: "Pas de nouvelle dépendance" |
