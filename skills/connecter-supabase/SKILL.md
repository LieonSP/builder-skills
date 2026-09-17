\---

name: connecter-supabase

description: Connecte le repo d'un apprenant à son projet Supabase de bout en bout — installe le CLI Supabase en devDependency (jamais en global), récupère d'emblée un Personal Access Token (le seul geste manuel, collé directement dans `.env`, jamais dans le chat, avec une échéance de 30 jours) qui sert à la fois à authentifier le CLI et à alimenter `prd-bdd`, relie le projet à partir du seul project ref (jamais de mot de passe DB), récupère les clés (publishable/secret) et construit l'URL, écrit tout dans `.env`, vérifie `.gitignore`, pour finir de remplir les 4 variables attendues par `prd-bdd`/`audit-produit` (`SUPABASE\_URL`, `SUPABASE\_PUBLISHABLE\_KEY`, `SUPABASE\_SECRET\_KEY`, `SUPABASE\_ACCESS\_TOKEN`). Teste réellement la connexion avant de conclure. Idempotente — une relance ne rejoue que ce qui manque. À utiliser quand un apprenant veut connecter son repo à Supabase, typiquement avant `prd-bdd`.

\---



\# Connecter Supabase



Faire tout le travail de connexion à Supabase à la place de l'apprenant, sans jamais lui demander autre chose que le project ref et un token à coller. Ne jamais nommer "CLI" ou "login" dans les messages qui lui sont adressés — dire simplement "je prépare la connexion", "il ne reste qu'une clé à récupérer".



\## Entrée



Demander uniquement le \*\*project ref\*\* du projet Supabase de l'apprenant. Expliquer précisément où le trouver, avec un exemple, pour éviter toute confusion avec le nom du projet :



&#x20;  "C'est une suite d'une vingtaine de lettres/chiffres (ex. `abcdefghijklmnopqrst`), pas le nom que tu as donné à ton projet. Tu la trouves dans l'URL de ton dashboard, juste après `/project/` : `https://supabase.com/dashboard/project/`\*\*`abcdefghijklmnopqrst`\*\* — ou dans Project Settings → General, champ 'Reference ID'."



Ce n'est pas un secret — il peut être donné directement dans le chat, sans restriction. Si un argument a déjà été fourni, l'utiliser directement sans redemander.



\## Étape 1 — Préparer le terrain (silencieux)



1\. Vérifier si `package.json` existe à la racine du repo ; sinon, `npm init -y` pour en créer un minimal.

2\. Vérifier si `supabase` est déjà listé en devDependency dans `package.json`. Sinon, `npm install supabase --save-dev` — jamais d'installation globale (le CLI la refuse de toute façon).

3\. Utiliser `npx supabase ...` pour toutes les commandes des étapes suivantes.



\## Étape 2 — Le seul geste manuel : le Personal Access Token (30 jours)



1\. Si `SUPABASE\_ACCESS\_TOKEN` est déjà rempli dans `.env` (relance de la skill), passer directement à l'Étape 3.

2\. Sinon, créer `.env` à la racine du repo s'il n'existe pas encore, avec au moins la ligne `SUPABASE\_ACCESS\_TOKEN=`.

3\. Ouvrir ce fichier dans l'éditeur de texte par défaut de l'OS de l'apprenant, pour qu'il n'ait pas à le chercher lui-même : `notepad .env` sous Windows, `open -e .env` sous macOS, `xdg-open .env` sous Linux (détecter l'OS via `$OSTYPE`/`uname` avant de choisir la commande). Si l'ouverture échoue (pas d'éditeur associé, environnement sans interface graphique), le dire simplement et donner le chemin du fichier à ouvrir manuellement — ne pas bloquer dessus.

4\. Donner à l'apprenant le lien direct https://supabase.com/dashboard/account/tokens et lui demander de générer un token là-bas, puis de le coller \*\*directement dans le fichier `.env` qui vient de s'ouvrir\*\*, sur la ligne `SUPABASE\_ACCESS\_TOKEN=` — jamais dans le chat.

&#x20;  S'il voit un menu de durée de validité, lui indiquer de choisir \*\*30 jours\*\* : "choisis 30 jours dans le menu d'expiration si on te le propose — ça correspond à la durée du module, et ça évite d'avoir un accès qui traîne après."

5\. Attendre sa confirmation avant de continuer — ne jamais supposer que c'est fait.



Ce token sert dès l'étape suivante à authentifier les commandes CLI (pas besoin d'une étape de connexion séparée) et alimentera aussi `prd-bdd`.



\## Étape 3 — Relier le projet



`SUPABASE\_ACCESS\_TOKEN=<valeur lue dans .env> npx supabase link --project-ref <ref>` (en ajoutant `printf '\\n' |` en entrée pour répondre automatiquement "vide" au prompt optionnel de mot de passe DB, qu'on ignore volontairement : cette étape n'en a pas besoin).



Si la commande échoue à cause d'un token invalide ou expiré : le dire clairement à l'apprenant et le renvoyer générer un nouveau token à l'Étape 2 — ne jamais retenter avec une autre méthode d'authentification (pas de repli vers un login interactif).



Si la commande échoue pour une autre raison (ref invalide, projet inexistant, pas les droits sur ce projet) : le dire clairement à l'apprenant avec le message d'erreur reformulé en langage courant, et s'arrêter — ne jamais tenter une méthode alternative sans le dire.



\## Étape 4 — Récupérer les clés et construire l'URL



`SUPABASE\_ACCESS\_TOKEN=<valeur lue dans .env> npx supabase projects api-keys --project-ref <ref> --reveal` — le flag `--reveal` est indispensable : sans lui, la clé secrète est renvoyée masquée (`sb\_secret\_Kw3CG···...`) et inutilisable.



Lire le résultat réel : selon l'état de migration du projet, les clés peuvent apparaître sous les noms `publishable`/`secret` ou encore `anon`/`service\_role` (legacy), et souvent les deux paires à la fois (comportement courant pendant la période de transition Supabase, pas une anomalie en soi). Mapper `anon`→publishable et `service\_role`→secret si ce sont les seuls noms affichés, sans en parler à l'apprenant — un mapping interne, pas une décision qui le concerne.

Si les deux paires sont présentes, s'arrêter et demander à l'apprenant, en langage simple plutôt qu'avec les noms techniques bruts : *"Ton projet a deux jeux de clés (l'ancien système et le nouveau, plus récent). On part sur le nouveau, sauf si tu préfères garder l'ancien pour une raison particulière ?"* — utiliser le nouveau (`publishable`/`secret`) par défaut si l'apprenant n'a pas de préférence.

Si le résultat est ambigu pour une autre raison (noms inattendus, plus de 4 clés), s'arrêter et demander confirmation.



Construire `SUPABASE\_URL=https://<ref>.supabase.co`.



\## Étape 5 — Écrire dans `.env`



`.env` existe déjà depuis l'Étape 2. Pour chacune de `SUPABASE\_URL`, `SUPABASE\_PUBLISHABLE\_KEY`, `SUPABASE\_SECRET\_KEY` : si la variable existe déjà dans le fichier, mettre à jour sa valeur en place (jamais dupliquer la ligne) ; sinon, l'ajouter. `SUPABASE\_ACCESS\_TOKEN` a déjà été écrit à l'Étape 2, ne pas y retoucher ici.



\## Étape 6 — Vérifier `.gitignore`



Vérifier que `.env` est listé dans `.gitignore` ; l'ajouter si absent.



\## Étape 7 — Vérifier que ça fonctionne réellement



Deux tests légers, jamais un simple "les commandes se sont exécutées sans erreur" :

\- `curl $SUPABASE\_URL/auth/v1/settings` avec la `SUPABASE\_PUBLISHABLE\_KEY` (header `apikey`) — confirme que l'URL et cette clé sont valides. Ne pas tester sur `/rest/v1/` racine : avec les clés nouvelle génération (`sb\_publishable\_...`), cet endpoint exige la clé secrète et renverrait à tort une erreur ("Secret API key required") même quand la publishable key est parfaitement valide.

\- Un appel léger à l'API Management (ex. `GET https://api.supabase.com/v1/projects` avec `Authorization: Bearer $SUPABASE\_ACCESS\_TOKEN`) — confirme que le token collé à l'Étape 2 fonctionne aussi.



Si l'un des deux échoue, le dire clairement en précisant lequel et pourquoi (clé invalide, token invalide, etc.) — ne jamais annoncer "c'est connecté" sans avoir vérifié les deux.



\## Étape 8 — Rendre compte



En langage courant, sans jargon : confirmer que le projet est connecté et dire ce que ça permet maintenant (ex. "tu peux lancer `prd-bdd` pour créer tes tables"). Ne jamais afficher la valeur d'une clé ou d'un token, même partielle.



Note pour le formateur (pas pour l'apprenant) : penser à rappeler en fin de module de révoquer ce token depuis le dashboard — la skill ne peut pas le faire à sa place.



\## Idempotence



Relancer la commande plus tard ne casse rien : elle détecte ce qui est déjà fait (variables déjà présentes dans `.env` aux Étapes 2 et 5) et ne rejoue que ce qui manque réellement.



\## Principes



\- \*\*Aucun jargon visible, compte-rendu en langage clair.\*\* Jamais "CLI", "login", ou tout autre terme d'implémentation dans un message adressé à l'apprenant — dire ce que ça fait, pas comment c'est construit. Objectif : qu'il comprenne le résultat sans jamais se sentir perdu ni paniquer devant un mot qu'il ne connaît pas.

\- \*\*Jamais de secret dans le chat.\*\* Le project ref peut être donné dans le chat ; aucune clé, aucun token, aucun mot de passe ne doit jamais y être tapé — le PAT se colle directement dans `.env`.

\- \*\*Jamais de mot de passe DB.\*\* Cette skill ne le demande à aucun moment — `link` fonctionne sans, et c'est précisément pour ça que `prd-bdd`/`audit-produit` restent en curl plutôt qu'en CLI pour le DDL.

\- \*\*Un seul geste manuel, fait tôt.\*\* Le token est demandé dès l'Étape 2 et sert ensuite à authentifier toutes les commandes CLI via variable d'environnement — pas de clic supplémentaire dans un navigateur pour une connexion séparée.

\- \*\*Expiration à 30 jours plutôt qu'illimitée.\*\* Le menu de création du token propose des paliers fixes ; choisir 30 jours (durée du module) plutôt que "n'expire jamais" — limite le risque si le token traîne après la formation.

\- \*\*Échec dit, jamais contourné.\*\* Un accroc à n'importe quelle étape s'arrête et se dit clairement — jamais de repli silencieux vers une autre méthode.

\- \*\*S'arrête au `.env`.\*\* Cette skill ne touche jamais au schéma ni aux tables — `prd-bdd` prend le relais ensuite.

\- \*\*Vérifié, pas supposé.\*\* La connexion n'est déclarée fonctionnelle qu'après un vrai test (Étape 7), jamais parce que les commandes n'ont pas renvoyé d'erreur.


