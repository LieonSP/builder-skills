---
name: audit-produit
description: Audite une application vibe-codée par un apprenant sur trois axes — sécurité (clés partagées/exposées, RLS Supabase risquées, inspiré de l'OWASP Top 10:2025), accessibilité (contraste, alt text, navigation clavier, labels de formulaire) et éco-conception (coût des requêtes, pagination, requêtes en boucle). Produit un compte rendu par catégorie avec un code couleur (🟢 ok, 🟠 attention, 🔴 risqué), propose des corrections sans jamais les appliquer automatiquement, sauvegarde le rapport dans `documents/audit-[slug].md`, le commite et le pousse sur GitHub, puis donne le lien direct vers le fichier. À utiliser quand un apprenant veut vérifier que son app est prête avant de la partager ou de la considérer terminée.
---

# Audit produit

Vérifier qu'une application vibe-codée (frontend + Supabase) ne présente pas de faille évidente sur trois axes — sécurité, accessibilité, éco-conception — pas un audit exhaustif, une passe mécanique et concrète sur les erreurs les plus fréquentes chez un débutant.

## Entrée

Auditer le repo courant :
- Le code frontend, à la racine du repo.
- Tout fichier `.env` présent — jamais son contenu affiché en clair dans le rapport, seulement son existence et sa présence ou non dans `.gitignore`.
- Les credentials Supabase (`$SUPABASE_URL`, `$SUPABASE_SECRET_KEY`, `$SUPABASE_ACCESS_TOKEN`), pour interroger les RLS réellement actives en base plutôt que deviner depuis les migrations locales — même approche que la skill `prd-bdd`.

Si les credentials Supabase sont absents ou inaccessibles, auditer uniquement ce qui est visible dans le code et le dire explicitement dans le rapport final (limite de portée assumée, pas une omission silencieuse).

## Axe 1 — Sécurité

### Étape 1 — Inspecter le code frontend

- Chercher toute clé secrète en dur : `SUPABASE_SECRET_KEY`/`service_role`, un Personal Access Token Supabase (`SUPABASE_ACCESS_TOKEN`, souvent préfixé `sbp_`), une chaîne JWT longue (`eyJ...`) qui n'est pas la clé publishable connue du projet, une clé d'API tierce codée en constante.
- Vérifier qu'aucun appel du frontend n'utilise `SUPABASE_SECRET_KEY` (elle contourne RLS — elle ne doit jamais atteindre le navigateur), seulement la clé publishable.
- Un `SUPABASE_ACCESS_TOKEN` exposé est encore plus grave qu'une `SUPABASE_SECRET_KEY` exposée : le Personal Access Token donne accès à **tous** les projets Supabase du compte, pas seulement celui-ci — le traiter en 🔴 même si le reste du projet semble sain.
- Vérifier que `.env` (ou tout fichier de secrets équivalent) est listé dans `.gitignore`.
- Repérer les cas où une donnée sensible est seulement cachée par une condition d'affichage côté UI (ex. un bouton ou une section masqués en CSS/JS selon le rôle) sans qu'une policy RLS empêche réellement la lecture des données sous-jacentes.

### Étape 2 — Inspecter la base Supabase (si les credentials sont disponibles)

En curl vers l'API Management (`curl -X POST https://api.supabase.com/v1/projects/<ref>/database/query -H "Authorization: Bearer $SUPABASE_ACCESS_TOKEN" -d '{"query": "<requête SQL>"}'`), interroger `pg_tables`, `pg_policies` et les privilèges de table du schéma `public` :

- Tables sans RLS activée du tout → accès non isolé, à traiter comme risque direct.
- Tables avec RLS activée mais policy `USING (true)` ou `WITH CHECK (true)` alors que la table a une colonne `owner`/`account_id` (ou équivalent) → l'isolation est prévue dans le schéma mais pas appliquée.
- Tables avec RLS activée et **aucune policy du tout** (ni lecture ni écriture) → bloque tout accès, y compris légitime ; à signaler comme dysfonctionnement plutôt que faille de sécurité.
- Tables avec RLS activée et une policy `SELECT` mais aucune policy insert/update/delete pour `anon`/`authenticated` → ne pas confondre avec le cas précédent : c'est le motif attendu pour une table de données de référence en lecture seule (voir `prd-bdd`, Étape 4), à traiter en 🟢, pas en dysfonctionnement.
- Table sans `GRANT` sur `anon`, `authenticated` ou `service_role` (`information_schema.role_table_grants`) → inaccessible même avec la secret key, indépendamment de RLS ; c'est un mode de panne différent d'une RLS mal configurée, à signaler distinctement.
- Fonction `SECURITY DEFINER` sans `search_path` fixé (`SET search_path = ''`) → catégorie "Function Search Path Mutable" du Security Advisor Supabase, vulnérable à un détournement de privilège — 🔴, même si le reste de la base est sain.

### Étape 3 — Passer les catégories OWASP Top 10:2025 applicables

Utiliser le Top 10 comme grille de lecture, pas comme liste à cocher intégralement — certaines catégories n'ont pas de sens pour un prototype de formation et doivent être marquées **non applicable** plutôt que forcées à un statut :

- **A01 Broken Access Control** → résultat de l'Étape 2 (RLS).
- **A02 Security Misconfiguration** → clés partagées, `.env` non ignoré, résultat de l'Étape 1 — un `SUPABASE_ACCESS_TOKEN` exposé (accès à tout le compte) prime en gravité sur une `SUPABASE_SECRET_KEY` exposée (accès à ce seul projet). Inclut aussi un `GRANT` manquant et une fonction `SECURITY DEFINER` sans `search_path` fixé, repérés à l'Étape 2.
- **A03 Software Supply Chain Failures** → dépendances ajoutées depuis une source non officielle, lockfile absent du repo. Vérification légère seulement.
- **A04 Cryptographic Failures** → secret ou mot de passe stocké en clair dans le code ou dans une table.
- **A05 Injection** → recherche de construction de requête SQL par concaténation de chaîne dans une Edge Function/RPC, si le projet en a. Non applicable si le projet n'a que des appels PostgREST standards (déjà paramétrés).
- **A06 Insecure Design** → protection par masquage UI seulement, repéré à l'Étape 1.
- **A07 Authentication Failures** → utilisation de Supabase Auth (pas un système réinventé), vérification de session côté serveur avant de rendre une page sensible.
- **A08 Software or Data Integrity Failures** → une valeur critique (prix, statut, quantité) acceptée telle quelle depuis le client sans contrainte ni validation côté base.
- **A09 Security Logging and Alerting Failures** → non applicable par défaut pour un prototype de formation sans infra de logging — le dire, ne pas auditer.
- **A10 Mishandling of Exceptional Conditions** → erreurs affichées brutes à l'utilisateur (stack trace, message SQL), ou erreur avalée silencieusement d'une façon qui masquerait un échec de sécurité.

## Axe 2 — Accessibilité

### Étape 4 — Vérifier l'accessibilité

- **Contraste** — texte gris clair sur fond clair, ou texte sur image sans superposition suffisante ; viser un contraste lisible pour une personne malvoyante, pas seulement esthétique.
- **Texte alternatif** — chaque `<img>` porteuse de sens a un `alt` ; les icônes utilisées seules comme bouton (sans libellé texte visible) ont un `aria-label`.
- **Formulaires** — chaque champ a un vrai `<label>` associé, pas seulement un `placeholder` qui disparaît dès la saisie commencée (une personne utilisant un lecteur d'écran n'a alors plus aucun repère).
- **Navigation clavier** — les éléments cliquables sont de vrais `<button>`/`<a>`, pas des `<div onClick>` sans `role`/`tabindex` ; rien n'est atteignable à la souris seulement.
- **Taille des zones cliquables** — cibles tactiles suffisamment grandes (repère : ~44×44px), en particulier sur mobile.

## Axe 3 — Éco-conception

### Étape 5 — Vérifier le coût des requêtes

- **Fetch sans limite** — un `select` qui récupère toutes les lignes d'une table sans filtre ni pagination, sur une table dont le volume peut croître dans le temps (pas un problème sur une table qui restera petite par nature).
- **Requêtes en boucle (N+1)** — une boucle qui fait une requête par élément au lieu d'un seul `select` avec jointure/embed.
- **Polling agressif** — un intervalle de rafraîchissement automatique trop court pour la fraîcheur réellement nécessaire à l'usage.
- **Re-fetch inutile** — une donnée déjà en mémoire re-demandée au serveur à chaque interaction (frappe clavier, ouverture d'un même onglet) sans raison.
- **Assets lourds** — images non compressées ou dans un format inutilement volumineux pour leur taille d'affichage réelle.

## Étape 6 — Attribuer un statut par catégorie

Appliquer la même règle sur les trois axes :

- 🟢 **Ok** — rien trouvé qui corresponde au risque de cette catégorie.
- 🟠 **Attention** — mauvaise pratique repérée, pas immédiatement bloquante ou exploitable telle quelle (ex. RLS activée mais policy plus permissive que nécessaire sur une table peu sensible ; images non optimisées mais peu nombreuses).
- 🔴 **Risqué** — exploitable ou bloquant directement (clé secrète ou Personal Access Token exposé, table sans RLS contenant des données d'autres utilisateurs, formulaire totalement inutilisable au clavier, fetch complet sur une table déjà volumineuse).
- **Non applicable** — catégorie hors périmètre pour ce projet ; ne pas forcer un statut coloré.

Le statut global du rapport est **le pire des statuts individuels obtenus, tous axes confondus**, jamais une moyenne ni un vote majoritaire — un seul 🔴 rend le rapport global 🔴.

## Étape 7 — Rédiger le compte rendu

```
# Audit produit — [nom du projet]

**Date :** [date du jour]
**Statut global :** 🟢 / 🟠 / 🔴

## Sécurité

### [Catégorie OWASP]
[🟢/🟠/🔴/Non applicable] — [constat en 1-2 phrases, avec le fichier/la table/la policy exacte concernée]
[Si 🟠 ou 🔴 uniquement : **Correction proposée** — action concrète (ligne à supprimer, policy SQL corrigée, variable à déplacer en `.env`).]

## Accessibilité

### [Point vérifié]
[🟢/🟠/🔴] — [constat, avec le fichier/l'élément concerné]
[Correction proposée si 🟠 ou 🔴]

## Éco-conception

### [Point vérifié]
[🟢/🟠/🔴] — [constat, avec le fichier/la requête concernée]
[Correction proposée si 🟠 ou 🔴]
```

Ne jamais afficher la valeur d'une clé ou d'un secret trouvé dans le rapport — la signaler par son emplacement (fichier, ligne, nom de variable), jamais par sa valeur, même partielle.

Ne jamais appliquer une correction automatiquement, même évidente, sur aucun des trois axes. Les proposer seulement ; c'est à l'apprenant de décider et de demander l'application s'il la veut.

## Étape 8 — Sauvegarder, committer et donner le lien

1. Trouver la racine du repo : exécuter `git rev-parse --show-toplevel`.
   - Si échec (pas un repo git), sauvegarder le rapport sous `./documents/` relatif au répertoire de travail courant et s'arrêter là — pas de commit/push possible hors d'un repo git, le dire à l'apprenant.
2. Créer le dossier `documents/` s'il n'existe pas déjà.
3. Sauvegarder le rapport sous `documents/audit-[slug].md`, où `[slug]` est dérivé du nom du repo (basename de la racine trouvée à l'étape 1), en kebab-case. Écraser le fichier s'il existe déjà — chaque exécution reflète l'état actuel de l'app ; l'historique des audits précédents reste consultable via `git log` sur ce fichier, pas besoin de le garder dans le fichier lui-même.
4. `git add documents/audit-[slug].md`, puis `git commit -m "Audit produit — [date du jour]"`.
5. `git push` sur la branche courante (`git branch --show-current`). Si le push échoue (pas de remote configuré, réseau, authentification requise), le dire clairement à l'apprenant et donner uniquement le chemin local du fichier — ne pas bloquer, ne pas réessayer en boucle.
6. Si le push réussit et que `origin` est un remote GitHub (vérifier avec `git remote get-url origin`), construire le lien direct vers le fichier — `https://github.com/<owner>/<repo>/blob/<branche>/documents/audit-[slug].md` — et le donner en clair, cliquable, à l'apprenant à la fin du rapport. Sinon, donner uniquement le chemin local.

## Principes

- **Concret, pas théorique.** Chaque constat cite le fichier, la ligne, la policy ou la requête exacte — jamais une remarque générique du type "attention aux clés" ou "pense à l'accessibilité".
- **Non applicable ≠ vert.** Le dire explicitement plutôt que de forcer un statut coloré sur une catégorie hors périmètre.
- **Jamais de secret affiché en clair**, même dans le rapport destiné à l'apprenant.
- **Proposer, jamais appliquer.** Une correction — de sécurité, d'accessibilité ou de performance — change le comportement de l'app ; ça reste la décision de l'apprenant.
- **Le pire des statuts fait le statut global**, sur les trois axes confondus, pas une moyenne.
- **Le rapport est poussé, pas juste affiché.** Sauvegarder, commiter et pousser systématiquement, puis donner le lien direct vers le fichier — jamais seulement le résultat en chat sans trace dans le repo.
