---
name: prd-bdd
description: Crée dans Supabase la base de données décrite par le schéma de données d'une PRD (tables, RLS, jeu de test), sans jamais connecter le proto frontend existant à cette base. Cherche la PRD sous `documents/PRD-*.md`, détecte l'existant pour ne créer que ce qui manque, pose des questions si le schéma de la PRD est ambigu, et sauvegarde un résumé du schéma créé dans le même dossier que la PRD, le commite et le pousse sur GitHub, puis donne le lien direct vers le fichier. À utiliser quand un apprenant veut passer de la PRD (schéma proposé) à une vraie base de données Supabase fonctionnelle, avant de brancher le frontend.
---

# BDD Supabase à partir d'une PRD

Transformer la section "Schéma de données" d'une PRD en une vraie base Supabase : tables, RLS, jeu de test illustratif. Le proto frontend du repo n'est jamais touché — cette skill s'arrête à la base de données.

## Entrée

1. Chercher `documents/PRD-*.md` à la racine du repo (`git rev-parse --show-toplevel` puis chercher depuis là).
   - Si un seul fichier trouvé, l'utiliser.
   - Si plusieurs, demander lequel traiter.
   - Si aucun, demander le chemin de la PRD.
2. Vérifier que les credentials Supabase sont disponibles dans un fichier `.env` à la racine du repo : `SUPABASE_URL`, `SUPABASE_SECRET_KEY`, `SUPABASE_ACCESS_TOKEN`.
   - Si le fichier ou une de ces trois variables manque, arrêter ici et dire précisément à l'apprenant quoi ajouter (nom de variable, où les trouver dans son dashboard Supabase : Project Settings → API Keys pour l'URL et la secret key, Account → Access Tokens pour créer un Personal Access Token). Ne jamais demander à ce que les valeurs soient tapées dans le chat.
   - Le même `.env` contient aussi `SUPABASE_PUBLISHABLE_KEY`, utilisée par d'autres skills (le proto frontend) — pas requise par `prd-bdd`, ne pas la redemander.
   - Vérifier que `.env` est bien listé dans `.gitignore` ; s'il ne l'est pas, s'arrêter et le signaler à l'apprenant sans le corriger soi-même.
3. Extraire le project ref depuis `SUPABASE_URL` (`https://<ref>.supabase.co`) — nécessaire pour l'API Management (DDL).
4. Toutes les requêtes Supabase de cette skill passent par `curl` (API REST et API Management) — jamais par le CLI Supabase (`supabase db push`, `supabase link`, etc.), même pour la création de tables ou de policies.

## Étape 1 — Lire le schéma de la PRD

Repérer la section "Schéma de données" de la PRD. Pour chaque entité : nom de table, colonnes (nom, type, contraintes), relations avec les autres entités, et toute colonne signalée comme limite de permission/propriété (ex. `owner`, `account_id`).

Ne pas deviner si un point est réellement ambigu — poser la question plutôt que trancher en silence, maximum 5 questions, une phrase chacune, numérotées. Cas fréquents à vérifier :
- Un type de colonne absent ou trop vague (ex. "des infos" sans préciser text/jsonb/number)
- Une relation dont la cardinalité n'est pas claire (1-N vs N-N sans table de jointure explicite)
- Une entité qui semble nécessiter une isolation par utilisateur/compte sans que la PRD ne le précise (mono vs multi-utilisateur)
- Un nom d'entité ambigu une fois converti en snake_case ASCII (collision, accent, pluriel incohérent)

## Étape 2 — Inspecter l'existant

Avant de créer quoi que ce soit, interroger `information_schema.tables` et `information_schema.columns` (schéma `public`) pour savoir ce qui existe déjà, en curl vers l'API Management :

```bash
curl -X POST "https://api.supabase.com/v1/projects/<ref>/database/query" \
  -H "Authorization: Bearer $SUPABASE_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"query": "select table_name, column_name, data_type from information_schema.columns where table_schema = '"'"'public'"'"';"}'
```

Comparer à la PRD :
- Table absente → à créer (Étape 3)
- Table existante avec une colonne manquante → `ALTER TABLE ... ADD COLUMN` pour la colonne manquante uniquement
- Table existante dont une colonne a un type ou une contrainte qui diverge de la PRD → **s'arrêter et demander** plutôt que modifier en silence — changer un type sur une colonne existante peut faire perdre des données
- Ne jamais `DROP TABLE` ou `DROP COLUMN` automatiquement, même si la PRD a changé de forme depuis la dernière exécution

## Étape 3 — Créer/adapter les tables

Pour chaque table manquante, générer le DDL et l'exécuter en curl vers la même API Management que l'Étape 2 (`curl -X POST .../database/query -H "Authorization: Bearer $SUPABASE_ACCESS_TOKEN" -d '{"query": "<le DDL>"}'`) :
- Nom de table en snake_case ASCII (pas d'accent, pas de majuscule), dérivé du nom d'entité de la PRD
- Clé primaire `id uuid primary key default gen_random_uuid()` sauf si la PRD impose une autre clé
- Types Postgres standards mappés depuis la PRD (text, int/bigint, numeric, boolean, timestamptz, jsonb, uuid, text[]...)
- `created_at timestamptz default now()` par défaut si la PRD ne dit rien sur le suivi temporel
- Clés étrangères pour chaque relation décrite, avec `on delete` explicite (demander si la PRD ne le précise pas et que la conséquence n'est pas évidente)

## Étape 4 — RLS

Activer RLS sur chaque table (`ALTER TABLE ... ENABLE ROW LEVEL SECURITY`), puis appliquer une policy selon ce que dit la PRD — même mécanisme que l'Étape 3, en curl vers l'API Management :

- **Table sans colonne d'isolation** (mono-utilisateur pour cette entité) → policy ouverte (select/insert/update/delete autorisés), comme une app perso sans compte.
- **Table avec une colonne de type `owner`/`account_id` signalée dans la PRD** (multi-utilisateur) → policy scopée sur cette colonne (ex. `auth.uid() = owner_id`), même si l'auth réelle n'est pas encore branchée côté frontend — la policy doit déjà être correcte pour le jour où elle le sera.
- Si la PRD ne permet pas de trancher entre les deux, c'est une des questions de l'Étape 1 — ne pas choisir par défaut.

## Étape 5 — Jeu de test

Insérer quelques lignes illustratives par table (3 à 5, pas un volume de prod), via l'API REST en curl avec la secret key (`curl $SUPABASE_URL/rest/v1/<table>`, headers `apikey` et `Authorization: Bearer` = `$SUPABASE_SECRET_KEY`, bypass RLS). Respecter l'ordre des dépendances (tables référencées avant celles qui les référencent via une clé étrangère). Utiliser des données plausibles pour le domaine de la PRD, pas des `test1`/`test2` — l'apprenant doit reconnaître son produit en regardant les données.

## Étape 6 — Sauvegarder le résumé

Sauvegarder un fichier `documents/schema-[slug].md` (même dossier que la PRD, `[slug]` dérivé du nom de la PRD) listant : tables créées ou modifiées cette exécution, colonnes et types, relations, policy RLS appliquée par table, et nombre de lignes de test insérées.

`git add documents/schema-[slug].md`, puis `git commit -m "Schéma BDD — [slug]"`. `git push` sur la branche courante (`git branch --show-current`). Si le push échoue (pas de remote configuré, réseau, authentification requise), le dire clairement à l'apprenant et donner uniquement le chemin local du fichier — ne pas bloquer, ne pas réessayer en boucle. Si le push réussit et que `origin` est un remote GitHub (vérifier avec `git remote get-url origin`), construire le lien direct vers le fichier — `https://github.com/<owner>/<repo>/blob/<branche>/documents/schema-[slug].md` — et le donner en clair, cliquable, à l'apprenant. Sinon, donner uniquement le chemin local.

## Principes

- **Jamais de frontend.** Cette skill ne touche, n'édite, ni ne référence aucun fichier du proto frontend — même pas pour y ajouter un client Supabase. Si on lui demande d'aller plus loin, elle s'arrête et le dit.
- **Additif, jamais destructeur.** On ajoute ce qui manque ; on ne supprime et on ne modifie jamais une table, une colonne ou des données existantes sans que ce soit une décision explicite de l'apprenant.
- **Ambiguïté = question, jamais un choix par défaut silencieux.** Surtout pour la RLS et le mono/multi-utilisateur — une mauvaise policy RLS découverte tard coûte beaucoup plus cher qu'une question posée tôt.
- **Jeu de test illustratif, pas de prod.** Quelques lignes reconnaissables, pas un générateur de volume.
- **Une trace lisible.** Le résumé de schéma dans `documents/` évite à l'apprenant de devoir interroger Supabase pour savoir ce qui a été créé.
- **Le résumé est poussé, pas juste écrit.** Commiter et pousser systématiquement, puis donner le lien direct vers le fichier — jamais seulement une sauvegarde locale silencieuse.
- **Toujours curl, jamais le CLI Supabase.** Aucune commande `supabase ...` (link, db push, etc.) — l'API REST (`SUPABASE_SECRET_KEY`) pour les données, l'API Management (`SUPABASE_ACCESS_TOKEN`) pour le DDL, toutes deux en curl.
