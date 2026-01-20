# Skill : /spec — Prompt structuré CITER + Specs GIVEN/WHEN/THEN

## Quand l'utiliser

- Au début de toute tâche non-triviale
- Quand tu as une idée vague à transformer en demande précise
- Avant de coder ou d'exécuter quoi que ce soit

## Ce que ça fait

`/spec` fusionne deux techniques de structuration :

1. **Format CITER** — Structure globale du prompt
   - **C**ontext : D'où vient la tâche ?
   - **I**ntent : Pourquoi je fais ça ?
   - **T**ask : Quoi exactement ? (avec specs GIVEN/WHEN/THEN)
   - **E**xpected : Comment je vérifie que c'est fait ?
   - **R**estrictions : Quoi NE PAS faire ?

2. **Specs GIVEN/WHEN/THEN** — Détail de chaque transformation
   - **GIVEN** : État de départ (observable, factuel)
   - **WHEN** : Action déclencheuse (une seule)
   - **THEN** : Résultat attendu (vérifiable)

## Pourquoi c'est important

> "Si tu ne peux pas écrire ta demande en GIVEN/WHEN/THEN, c'est que tu n'as pas assez réfléchi."

- L'agent a des critères de succès **vérifiables**
- Pas d'ambiguïté sur ce qui est attendu
- Les non-goals évitent le **scope creep**

## Exemple d'output

```markdown
## Context
Fichier CSV hérité d'un client — suivi de mots-clés SEO avec ~3000 lignes.
Données polluées : formats de volumes incohérents, dates dans 10+ formats différents.

## Intent
Réaliser un audit SEO — besoin de données propres pour analyser les performances.

## Task
REQ-1: Normaliser les volumes de recherche
- GIVEN colonne "volume recherche" avec formats mixtes (12.4K, 7.33k, 12 508)
- WHEN le nettoyage s'exécute
- THEN tous les volumes sont des entiers purs (12400, 7330, 12508)

REQ-2: Normaliser les dates de crawl
- GIVEN colonne "date dernier crawl" avec formats chaotiques (07/21/2024, 2024/02/12)
- WHEN le nettoyage s'exécute
- THEN toutes les dates sont au format ISO YYYY-MM-DD

REQ-3: Normaliser les statuts
- GIVEN colonne "status" avec variations (online, En ligne, EN LIGNE)
- WHEN le nettoyage s'exécute
- THEN tous les statuts sont en snake_case (en_ligne, brouillon, archive)

## Expected
- ✅ Volumes numériques purs
- ✅ Dates uniformes YYYY-MM-DD
- ✅ Statuts standardisés
- ✅ Fichier importable sans erreur dans Excel/Sheets

## Restrictions
- ❌ Ne pas supprimer de lignes (garder les doublons)
- Hors scope : URLs, Responsables, Commentaires (laisser tels quels)
```

## Bonnes pratiques

1. **Un REQ = une transformation** — Ne pas mélanger plusieurs choses
2. **THEN vérifiable** — "Toutes les dates sont au format X" (pas "les dates sont propres")
3. **Non-goals explicites** — Tout ce qui n'est pas listé peut être interprété comme "à faire"
4. **Validation avant exécution** — Relire le prompt avant de lancer

## Anti-patterns

| À éviter | Pourquoi | Mieux |
|----------|----------|-------|
| GIVEN vague | Pas vérifiable | Citer des exemples concrets |
| THEN avec implémentation | Mélange quoi/comment | Décrire le résultat, pas la méthode |
| Pas de non-goals | Scope creep garanti | Lister ce qui est hors scope |
| Trop de REQ d'un coup | Difficile à vérifier | Max 5-7 REQ par prompt |

## Voir aussi

- `/verify` — Vérifier que les specs sont satisfaites après exécution
- `/commit` — Commit une fois le travail validé
