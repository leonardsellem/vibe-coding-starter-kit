---
description: Transforme une idée en prompt structuré CITER avec specs GIVEN/WHEN/THEN
argument-hint: [idée ou tâche à réaliser]
---

Aide l'utilisateur à transformer son idée en un prompt complet et structuré.

## Processus INTERACTIF

**IMPORTANT** : Utiliser l'outil `AskUserQuestion` pour chaque étape. Ne jamais poser toutes les questions d'un coup.

### Étape 1 : Context (C)

Si $ARGUMENTS est vague, commencer par le contexte :

```json
{
  "questions": [{
    "question": "Quel est le contexte de cette tâche ?",
    "header": "Context",
    "multiSelect": false,
    "options": [
      {"label": "Nouveau projet", "description": "Partir de zéro, rien n'existe encore"},
      {"label": "Fichier existant", "description": "Je dois modifier ou traiter quelque chose qui existe"},
      {"label": "Héritage client", "description": "Données ou code reçu d'un tiers"},
      {"label": "Bug à corriger", "description": "Quelque chose ne fonctionne pas comme prévu"}
    ]
  }]
}
```

### Étape 2 : Intent (I)

```json
{
  "questions": [{
    "question": "Quel est l'objectif derrière cette tâche ?",
    "header": "Intent",
    "multiSelect": false,
    "options": [
      {"label": "Livrable client", "description": "Produire quelque chose pour un client"},
      {"label": "Audit / Analyse", "description": "Comprendre ou évaluer une situation"},
      {"label": "Automatisation", "description": "Gagner du temps sur une tâche répétitive"},
      {"label": "Amélioration", "description": "Rendre quelque chose meilleur ou plus propre"}
    ]
  }]
}
```

### Étape 3 : Task — Identifier les transformations (T)

```json
{
  "questions": [{
    "question": "Quelles transformations ou actions sont nécessaires ?",
    "header": "Task",
    "multiSelect": true,
    "options": [
      {"label": "Normaliser des formats", "description": "Dates, nombres, textes incohérents"},
      {"label": "Nettoyer des données", "description": "Encodage, doublons, valeurs nulles"},
      {"label": "Transformer la structure", "description": "Colonnes, lignes, format de fichier"},
      {"label": "Générer un output", "description": "Créer un nouveau fichier ou rapport"}
    ]
  }]
}
```

### Étape 4 : Task — Détailler chaque transformation

Pour CHAQUE transformation sélectionnée, demander les détails pour construire le GIVEN/WHEN/THEN :

```json
{
  "questions": [{
    "question": "Pour [transformation X], quel est l'état actuel (GIVEN) ?",
    "header": "GIVEN",
    "multiSelect": false,
    "options": [
      {"label": "Formats mixtes", "description": "Plusieurs formats différents dans la même colonne"},
      {"label": "Données manquantes", "description": "Valeurs vides, nulles ou incohérentes"},
      {"label": "Encodage cassé", "description": "Caractères spéciaux mal encodés"},
      {"label": "Autre", "description": "Je vais préciser"}
    ]
  }]
}
```

Puis demander le résultat attendu (THEN) :

```json
{
  "questions": [{
    "question": "Pour [transformation X], quel format de sortie veux-tu (THEN) ?",
    "header": "THEN",
    "multiSelect": false,
    "options": [
      {"label": "Format standard", "description": "ISO pour dates, nombres purs, UTF-8..."},
      {"label": "Format spécifique", "description": "Je vais préciser le format exact"},
      {"label": "Valeurs normalisées", "description": "Liste fermée de valeurs acceptées"},
      {"label": "Autre", "description": "Je vais préciser"}
    ]
  }]
}
```

### Étape 5 : Expected (E)

```json
{
  "questions": [{
    "question": "Comment sauras-tu que c'est réussi ?",
    "header": "Expected",
    "multiSelect": true,
    "options": [
      {"label": "Données uniformes", "description": "Tous les formats sont cohérents"},
      {"label": "Zéro erreur d'import", "description": "Le fichier s'ouvre sans problème"},
      {"label": "Validation visuelle", "description": "Je peux vérifier en regardant"},
      {"label": "Tests automatisés", "description": "Un script valide le résultat"}
    ]
  }]
}
```

### Étape 6 : Restrictions / Non-Goals (R)

```json
{
  "questions": [{
    "question": "Qu'est-ce qui est explicitement HORS SCOPE ?",
    "header": "Non-Goals",
    "multiSelect": true,
    "options": [
      {"label": "Ne pas supprimer", "description": "Garder toutes les lignes/données"},
      {"label": "Certaines colonnes", "description": "Laisser certaines colonnes telles quelles"},
      {"label": "Pas de validation métier", "description": "Juste le format, pas le contenu"},
      {"label": "Pas de refactoring", "description": "Faire le minimum nécessaire"}
    ]
  }]
}
```

### Étape 7 : Générer le prompt CITER avec specs

Après toutes les réponses, générer le prompt complet :

```markdown
## Context
[Résumé du contexte : d'où vient la tâche, état actuel]

## Intent
[Objectif business ou technique derrière cette tâche]

## Task
REQ-1: [Nom de la première transformation]
- GIVEN [état actuel décrit]
- WHEN [action déclencheuse]
- THEN [résultat attendu, vérifiable]

REQ-2: [Nom de la deuxième transformation]
- GIVEN [état actuel]
- WHEN [action]
- THEN [résultat]

[... autant de REQ que nécessaire]

## Expected
- ✅ [Critère de succès 1]
- ✅ [Critère de succès 2]
- ✅ [Critère de succès 3]

## Restrictions
- ❌ [Non-goal 1]
- ❌ [Non-goal 2]
- Hors scope : [éléments explicitement exclus]
```

### Étape 8 : Validation finale

```json
{
  "questions": [{
    "question": "Ce prompt te convient ? Je peux commencer l'exécution ?",
    "header": "Validation",
    "multiSelect": false,
    "options": [
      {"label": "Oui, exécute", "description": "Lance l'implémentation"},
      {"label": "Ajuster", "description": "Je veux modifier quelque chose"},
      {"label": "Ajouter des REQ", "description": "Il manque des transformations"}
    ]
  }]
}
```

## Règles

- **GIVEN** = état de départ observable, pas d'action
- **WHEN** = une seule action déclencheuse (souvent "le script/nettoyage s'exécute")
- **THEN** = résultat vérifiable, pas d'implémentation
- Ne jamais lister toutes les questions d'un coup — une question, une réponse, puis continuer
- Le prompt final doit être copiable tel quel pour lancer l'exécution

$ARGUMENTS
