---
name: connecter-supabase
description: Connecte le repo d'un apprenant à son projet Supabase de bout en bout — installe le CLI Supabase en devDependency (jamais en global), authentifie via navigateur, relie le projet à partir du seul project ref (jamais de mot de passe DB), récupère les clés (publishable/secret) et construit l'URL, écrit tout dans `.env`, vérifie `.gitignore`, puis guide la création d'un Personal Access Token — le seul geste manuel, collé directement dans `.env`, jamais dans le chat — pour finir de remplir les 4 variables attendues par `prd-bdd`/`audit-produit` (`SUPABASE_URL`, `SUPABASE_PUBLISHABLE_KEY`, `SUPABASE_SECRET_KEY`, `SUPABASE_ACCESS_TOKEN`). Teste réellement la connexion avant de conclure. Idempotente — une relance ne rejoue que ce qui manque. À utiliser quand un apprenant veut connecter son repo à Supabase, typiquement avant `prd-bdd`.
---

# Connecter Supabase

Faire tout le travail de connexion à Supabase à la place de l'apprenant, sans jamais lui demander autre chose que le project ref. Ne jamais nommer "CLI", "login" ou "token" dans les messages qui lui sont adressés — dire simplement "je prépare la connexion", "j'ouvre une fenêtre pour que tu autorises l'accès", "il ne reste qu'une clé à récupérer".

## Entrée

Demander uniquement le **project ref** du projet Supabase de l'apprenant (visible dans l'URL de son dashboard Supabase, ou dans Project Settings → General). Ce n'est pas un secret — il peut être donné directement dans le chat, sans restriction. Si un argument a déjà été fourni, l'utiliser directement sans redemander.

## Étape 1 — Préparer le terrain (silencieux)

1. Vérifier si `package.json` existe à la racine du repo ; sinon, `npm init -y` pour en créer un minimal.
2. Vérifier si `supabase` est déjà listé en devDependency dans `package.json`. Sinon, `npm install supabase --save-dev` — jamais d'installation globale (le CLI la refuse de toute façon).
3. Utiliser `npx supabase ...` pour toutes les commandes des étapes suivantes.

## Étape 2 — Vérifier ou établir la session

1. Tenter `npx supabase projects list`. Si la commande réussit, une session est déjà active — passer directement à l'Étape 3.
2. Sinon, lancer `npx supabase login` — elle ouvre un navigateur pour un clic "autoriser" et attend que l'apprenant l'ait fait avant de continuer. Dire simplement : "j'ouvre une fenêtre pour que tu autorises l'accès — un clic suffit."
3. Si le login échoue ou timeout, le dire clairement et s'arrêter — ne pas réessayer en boucle silencieusement.

## Étape 3 — Relier le projet

`printf '\n' | npx supabase link --project-ref <ref>` — le `\n` répond automatiquement "vide" au prompt optionnel de mot de passe DB, qu'on ignore volontairement : cette étape n'en a pas besoin.

Si la commande échoue (ref invalide, projet inexistant, pas les droits sur ce projet) : le dire clairement à l'apprenant avec le message d'erreur reformulé en langage courant, et s'arrêter — ne jamais tenter une méthode alternative sans le dire.

## Étape 4 — Récupérer les clés et construire l'URL

`npx supabase projects api-keys --project-ref <ref>` et lire le résultat réel : selon l'état de migration du projet, les clés peuvent apparaître sous les noms `publishable`/`secret` ou encore `anon`/`service_role` (legacy). Mapper `anon`→publishable et `service_role`→secret si ce sont les noms affichés. Si le résultat est ambigu (plus de deux clés, noms inattendus), s'arrêter et demander à l'apprenant de confirmer plutôt que deviner.

Construire `SUPABASE_URL=https://<ref>.supabase.co`.

## Étape 5 — Écrire dans `.env`

Créer `.env` à la racine du repo s'il n'existe pas. Pour chacune de `SUPABASE_URL`, `SUPABASE_PUBLISHABLE_KEY`, `SUPABASE_SECRET_KEY` : si la variable existe déjà dans le fichier, mettre à jour sa valeur en place (jamais dupliquer la ligne) ; sinon, l'ajouter. Ne pas toucher à `SUPABASE_ACCESS_TOKEN` à cette étape.

## Étape 6 — Vérifier `.gitignore`

Vérifier que `.env` est listé dans `.gitignore` ; l'ajouter si absent.

## Étape 7 — Le seul geste manuel : le Personal Access Token

1. Si `SUPABASE_ACCESS_TOKEN` est déjà rempli dans `.env` (relance de la skill), passer directement à l'Étape 8.
2. Sinon, donner à l'apprenant le lien direct https://supabase.com/dashboard/account/tokens et lui demander de générer un token là-bas, puis de le coller **directement dans son `.env`**, sur la ligne `SUPABASE_ACCESS_TOKEN=` — jamais dans le chat. S'il propose de le restreindre à une organisation/un projet plutôt qu'à tout son compte, l'encourager à choisir cette option.
3. Attendre sa confirmation avant de continuer — ne jamais supposer que c'est fait.

## Étape 8 — Vérifier que ça fonctionne réellement

Deux tests légers, jamais un simple "les commandes se sont exécutées sans erreur" :
- `curl $SUPABASE_URL/rest/v1/` avec la `SUPABASE_PUBLISHABLE_KEY` (headers `apikey` et `Authorization: Bearer`) — confirme que l'URL et cette clé sont valides.
- Un appel léger à l'API Management (ex. `GET https://api.supabase.com/v1/projects` avec `Authorization: Bearer $SUPABASE_ACCESS_TOKEN`) — confirme que le token collé à l'Étape 7 fonctionne aussi.

Si l'un des deux échoue, le dire clairement en précisant lequel et pourquoi (clé invalide, token invalide, etc.) — ne jamais annoncer "c'est connecté" sans avoir vérifié les deux.

## Étape 9 — Rendre compte

En langage courant, sans jargon : confirmer que le projet est connecté et dire ce que ça permet maintenant (ex. "tu peux lancer `prd-bdd` pour créer tes tables"). Ne jamais afficher la valeur d'une clé ou d'un token, même partielle.

## Idempotence

Relancer la commande plus tard ne casse rien : elle détecte ce qui est déjà fait (session active à l'Étape 2, variables déjà présentes dans `.env` aux Étapes 5 et 7) et ne rejoue que ce qui manque réellement.

## Principes

- **Aucun jargon visible.** Jamais "CLI", "login" ou "token" dans un message adressé à l'apprenant — dire ce que ça fait, pas comment.
- **Jamais de secret dans le chat.** Le project ref peut être donné dans le chat ; aucune clé, aucun token, aucun mot de passe ne doit jamais y être tapé — le PAT se colle directement dans `.env`.
- **Jamais de mot de passe DB.** Cette skill ne le demande à aucun moment — `link` fonctionne sans, et c'est précisément pour ça que `prd-bdd`/`audit-produit` restent en curl plutôt qu'en CLI pour le DDL.
- **Échec dit, jamais contourné.** Un accroc à n'importe quelle étape s'arrête et se dit clairement — jamais de repli silencieux vers une autre méthode.
- **S'arrête au `.env`.** Cette skill ne touche jamais au schéma ni aux tables — `prd-bdd` prend le relais ensuite.
- **Vérifié, pas supposé.** La connexion n'est déclarée fonctionnelle qu'après un vrai test (Étape 8), jamais parce que les commandes n'ont pas renvoyé d'erreur.
