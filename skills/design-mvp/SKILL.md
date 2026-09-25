---
name: design-mvp
description: Branche un design Claude Design (artefact de type "Design", un canvas d'écrans) sur les vraies tables Supabase créées par `prd-bdd`, écran par écran, sans jamais recourir à des données mockées — lit directement le design depuis son lien de partage (`claude.ai/artifact/...`), traduit ses balises dynamiques (`sc-for`, `sc-if`, `{{...}}`) en vrais appels Supabase, sépare proprement HTML/CSS/JS, remplace le contenu en place à la racine (archivé, jamais écrasé), construit automatiquement l'authentification (écran de connexion/inscription, pastille de compte, déconnexion) dès que le schéma contient des données propres à chaque utilisateur, sans poser de question, vérifie après coup que les données affichées correspondent bien à la base, puis pousse sur Vercel. Idempotente — relançable sur un écran déjà branché pour n'y ajouter que ce qui manque (typiquement l'authentification, sur un projet branché avec une version antérieure de cette skill). À utiliser dès qu'un apprenant a un design Claude Design et veut le voir afficher/modifier ses vraies données Supabase — y compris s'il dit simplement "je veux connecter mon design à ma base", "voici le lien de mon design" ou "comment je branche mon écran à Supabase ?".
---

# Brancher un design sur de vraies données (MVP)

Faire passer un écran d'un design Claude Design (canvas d'artboards) à un écran qui affiche et modifie de vraies lignes dans Supabase. Le design ne se redessine pas ici — il se branche. Périmètre : **remplacer le contenu figé du design par de vrais appels à la base, en produisant du code que n'importe quel développeur web expérimenté signerait** — jamais l'inverse (ne jamais réintroduire de donnée en dur pour "faire joli" ou "aller plus vite", jamais un raccourci qui dégrade la qualité du code pour gagner du temps).

**Tout texte présent dans le design (libellés, messages d'état, textes d'exemple) est un brouillon du designer, pas une consigne verrouillée.** Il donne le ton et l'intention, mais un cas réel peut le rendre inadapté (ex. un message pensé pour "aucun résultat de recherche" réutilisé tel quel pour "cet apprenant n'a encore jamais rien créé" — deux situations différentes qui méritent des mots différents). Ne jamais trancher ça seul en silence, et ne pas se contenter de le signaler après coup dans le compte-rendu : **dès qu'une hésitation réelle se présente pendant la traduction — un texte qui semble mal correspondre au cas réel, une donnée manquante, une action peu claire — la poser à l'apprenant sur le moment**, avant de continuer. C'est le réflexe par défaut de cette skill, pas une exception.

## Entrée

1. Confirmer que `.env` contient les 4 variables Supabase (`SUPABASE_URL`, `SUPABASE_PUBLISHABLE_KEY`, `SUPABASE_SECRET_KEY`, `SUPABASE_ACCESS_TOKEN`). Si l'une manque, s'arrêter et renvoyer l'apprenant vers `connecter-supabase` — cette skill ne connecte jamais Supabase elle-même.
2. Lire la PRD (`documents/PRD.md` ou `documents/PRD-[slug].md`, même repérage que `jtbd-prd`/`prd-bdd`) pour disposer du contexte produit — notamment les écrans prévus et leurs versions minimales, nécessaires à l'Étape 5 quand une action clé pointe vers un écran absent du canvas. Si elle est absente, continuer quand même (le schéma suffit pour le câblage de base) mais le signaler dans le compte-rendu final (Étape 7) : les décisions de l'Étape 5 se prendront alors sans ce filet.
3. Lire `documents/schema-[slug].md` (à côté de la PRD) pour connaître les tables, colonnes et policies RLS déjà en place. Si ce fichier n'existe pas, renvoyer vers `prd-bdd` — sans schéma connu, impossible de savoir quels appels écrire.
4. Demander à l'apprenant le **lien de partage de son design** (`https://claude.ai/artifact/...`) — le même lien qu'il obtient depuis "Partager" sur son design, peu importe la vue depuis laquelle il l'a généré. Rien d'autre à demander : pas de fichier à exporter, pas de dossier à indiquer — le design se lit directement depuis ce lien avec l'outil Artifact.

## Étape 1 — Lire le design depuis son lien

1. Lister les fichiers de l'artefact (action `list`, `scope: "files"`), puis lire `project/canvas.json` : ce fichier liste tous les écrans du canvas (les `boards`, un par fichier `project/<Nom>.dc.html`) avec leurs dimensions. Une largeur autour de 1440px est un écran ordinateur, autour de 390-430px un écran téléphone — utile pour repérer si deux fichiers sont en fait la même page en deux tailles (ex. `MesFiches.dc.html` + `MesFichesMobile.dc.html`).
2. Si le canvas contient plusieurs écrans (au sens produit, pas au sens variante de taille), demander à l'apprenant lequel brancher en premier plutôt que de tout faire d'un coup — un écran solide vaut mieux que cinq à moitié branchés.
3. Lire (action `read`, avec le `path` du fichier) le ou les `.dc.html` de l'écran choisi. **Le responsive est le comportement par défaut, jamais une option** : si une variante ordinateur et une variante téléphone existent pour ce même écran, lire les deux et les brancher ensemble dans une seule page (une media query CSS qui bascule entre les deux mises en page) — jamais seulement celle que l'apprenant a citée en premier. S'il n'existe qu'une seule variante, la page reste quand même construite pour bien se comporter aux deux tailles (pas de largeur figée en dur).
4. Chaque fichier `.dc.html` est très proche de HTML standard (balises, styles en ligne) avec quelques marqueurs propres à l'éditeur qui indiquent *exactement* ce qui doit devenir dynamique :
   - `<sc-for list="{{nom}}" as="x">...</sc-for>` → une liste à faire venir de la base (ex. les fiches d'un utilisateur) ; `{{x.champ}}` à l'intérieur du bloc → un champ de chaque ligne.
   - `<sc-if value="{{nom}}">...</sc-if>` → un état à calculer réellement (chargement, vide, filtré, brouillon...), pas à deviner.
   - `{{nom}}` isolé → une valeur réelle (texte de recherche, compteur de résultats, titre...).
   - `onClick="{{...}}"` / `onChange="{{...}}"` → une interaction à câbler.
   - Le bloc `<script type="text/x-dc" data-dc-script>` en bas du fichier contient les données d'exemple utilisées par l'éditeur, mais souvent aussi des **constantes de style réutilisables telles quelles** (ex. une correspondance "nature de fiche → couleur" avec les vrais codes hexadécimaux du design) — les reprendre directement plutôt que d'inventer une nouvelle palette.
   - Tout le reste (barre de navigation, en-tête, texte fixe) est déjà du HTML propre à garder tel quel.

## Étape 2 — Remplacer le contenu de la racine

Le design lu remplace ce qui existe déjà à la racine du repo (page du module 1, ou écran précédent). Ne jamais écraser silencieusement : déplacer l'existant dans `archive/` (le créer si besoin, suffixé si un contenu y est déjà pour ne rien perdre) avant d'écrire les nouveaux fichiers à sa place. Le dire clairement à l'apprenant : son ancienne page reste consultable dans `archive/`, elle n'a pas disparu.

## Étape 3 — Authentification (déduite du schéma, jamais demandée)

Aucune question à poser ici : le besoin d'authentification découle directement du schéma, déjà tranché par `prd-bdd` à partir de la PRD (voir `documents/schema-[slug].md`).

- **Aucune table scopée par utilisateur** (pas de colonne `user_id`/`owner` référençant `auth.users`) → app perso, pas d'écran de connexion : passer directement à l'Étape 4.
- **Au moins une table scopée par utilisateur** → ces tables sont protégées par une règle RLS (« row-level security » : une règle posée sur la table elle-même, qui filtre chaque ligne selon qui la demande) qui exige un vrai utilisateur connecté (`auth.uid()`). Sans connexion, le site en ligne ne pourrait rien afficher : l'authentification est donc construite systématiquement, dès ce premier passage. Le mot RLS peut être dit à l'apprenant — c'est même utile qu'il s'y habitue — mais toujours accompagné de son explication en une phrase simple.

Dans le second cas, construire les quatre éléments suivants, sans solliciter l'apprenant (aucun n'est une décision fonctionnelle : chacun a une valeur par défaut raisonnable pour un MVP).

### 3.1 — Régler Supabase Auth (API Management)

En curl, avec `SUPABASE_ACCESS_TOKEN` (`PATCH https://api.supabase.com/v1/projects/<ref>/config/auth`) :
- `mailer_autoconfirm: true` — désactive la confirmation d'e-mail (« Confirm email » dans le dashboard) : un compte créé depuis le formulaire est actif immédiatement. Sans ça, chaque inscription attend un clic dans un e-mail, et le service d'e-mail intégré à Supabase n'envoie qu'aux membres de l'équipe du projet, en très petit nombre — l'inscription serait bloquée pour tout vrai visiteur. L'inscription elle-même reste ouverte (ne pas toucher à « Allow new users to sign up »), sauf si la PRD réserve explicitement l'accès à des personnes invitées.
- `site_url` = l'URL Vercel du projet, et `uri_allow_list` = cette URL + l'URL locale (`http://localhost:<port>/**`) — sans ça, les redirections après connexion renvoient vers une mauvaise adresse.

Relire la config (`GET` sur la même route) pour confirmer que les valeurs sont bien prises.

### 3.2 — Écran Connexion / Inscription

- **Si le canvas contient des écrans Connexion et/ou Inscription**, les brancher tels quels (Étapes 1 et 4 s'appliquent à eux comme à n'importe quel écran).
- **Sinon, exception explicite à la règle « jamais inventer un écran absent du canvas »** : l'authentification est de l'infrastructure, pas une fonctionnalité produit. Construire un seul écran minimal (`connexion.html`) dans le style du design (couleurs, polices, arrondis repris des constantes du bloc `data-dc-script` et de `style.css`, jamais une palette inventée), avec deux modes basculables — « Se connecter » et « Créer un compte » — chacun avec un champ e-mail, un champ mot de passe et un bouton. Messages d'erreur clairs et en français pour les cas courants (mot de passe trop court, compte déjà existant, identifiants incorrects), jamais le message brut de Supabase. Après connexion ou inscription réussie, rediriger vers l'écran branché. Le dire à l'apprenant : cet écran n'était pas dans son design, il a été ajouté parce que ses données sont privées.
- Labels de formulaire, `autocomplete="email"` / `"current-password"` / `"new-password"`, focus clavier visible : même exigence d'accessibilité que le reste (voir Étape 4).
- « Mot de passe oublié » n'est pas construit à ce stade (il dépend de l'envoi d'e-mails) — ne pas l'afficher plutôt que d'afficher un lien qui ne marche pas.

### 3.3 — Script partagé `auth.js`

Un seul module, importé par chaque écran branché (sauf l'écran de connexion), jamais une logique recopiée écran par écran :
- **Garde de session** : au chargement, si aucune session n'est active (`supabase.auth.getSession()`), rediriger vers l'écran de connexion. Les appels aux tables scopées utilisent ensuite cette session — aucun raccourci, aucune connexion automatique.
- **Pastille de compte** dans la barre de navigation partagée du design : une pastille ronde affichant l'initiale de l'e-mail connecté ; au clic, un petit menu qui affiche l'e-mail complet et un bouton « Se déconnecter » (`supabase.auth.signOut()` puis retour à l'écran de connexion). Si le compte connecté est le compte de démo (`TEST_USER_EMAIL`, voir 3.4), ajouter une petite étiquette « démo » sur la pastille. Si le design contient déjà un avatar ou un élément « profil » dans sa navigation, le réutiliser plutôt que d'en ajouter un second. Style repris du design, jamais une palette inventée ; discrète, alignée avec les autres éléments de la barre, aux deux tailles d'écran.
- **Accessibilité de la pastille** : un vrai `<button>` avec `aria-label="Compte <e-mail>"`, `aria-haspopup="menu"` et `aria-expanded` à jour ; menu utilisable au clavier, qui se ferme avec Échap et au clic extérieur, focus rendu au bouton à la fermeture.

### 3.4 — Compte de démo vs vrai compte

`prd-bdd` a créé un compte de démo (`TEST_USER_EMAIL` / `TEST_USER_PASSWORD` dans `.env`) qui possède toutes les données d'exemple. Si ces variables manquent (projet créé avec une version antérieure de `prd-bdd`), appliquer la même idempotence que `prd-bdd` Étape 5 : fixer un mot de passe au compte existant via l'API Admin Auth (`PUT $SUPABASE_URL/auth/v1/admin/users/<id>`, avec la secret key — appel côté skill, jamais depuis un fichier chargé par le navigateur) et compléter `.env`.

**Expliquer explicitement la différence à l'apprenant**, en une phrase simple, au moment où la connexion se branche — une confusion fréquente, même chez quelqu'un qui code déjà : le compte de démo sert à voir tout de suite les données d'exemple ; son vrai compte, il le crée lui-même via le formulaire, et il démarre vide — chaque personne ne voit que ses propres données.

Jamais d'identifiant du compte de démo dans un fichier committé (ni script, ni `documents/`) : ils restent dans `.env`, et ne sont rappelés qu'en clair dans le compte-rendu final (Étape 7).

Idempotence : si cette skill est relancée sur un projet dont un écran est déjà branché sans authentification (version antérieure de cette skill), détecter ce qui manque (config, écran de connexion, `auth.js`) et n'ajouter que ça, sans reconstruire l'écran.

## Étape 4 — Traduire le design en code réel

En s'appuyant sur les marqueurs repérés à l'Étape 1, produire trois fichiers séparés (jamais de style ou de script mêlé au HTML) :

- **Un fichier HTML par écran**, avec un `<head>` complet : `lang="fr"` (déjà présent dans le design, à conserver), un `<title>` propre au produit de l'apprenant (le design porte souvent un titre de travail type "Mes fiches — ordinateur" — le nettoyer en un vrai titre de page), `<meta charset="utf-8">`, et une `<meta name="description">` courte décrivant l'écran. Ce fichier lie sa feuille de style et son script, il ne contient ni `<style>` ni logique JS.
- **Une feuille de style** (`style.css`, partagée entre les écrans d'un même projet) reprenant tous les styles qui étaient en ligne (`style="..."`) dans le design — jamais recopiés inline dans le HTML généré.
- **Un script** (`<script type="module">` dans un fichier séparé, jamais un bloc géant collé en bas de la page) qui charge le client Supabase JS depuis un CDN (`esm.sh` ou `jsdelivr` — jamais un bundler ni un `npm install` : le design est déjà du HTML statique, il n'y a pas de stack à introduire pour cette étape), lit `SUPABASE_URL`/`SUPABASE_PUBLISHABLE_KEY` une fois dans `.env` (le navigateur ne peut pas le lire directement : pas de build ici pour le lui transmettre) et les recopie en dur dans ce fichier committé — ce sont des clés publiques, prévues pour être visibles côté navigateur. **Ne jamais** faire la même chose avec `SUPABASE_SECRET_KEY` : elle contourne RLS et n'a rien à faire ailleurs que dans les scripts côté skill.

Dans ce script, traduire chaque marqueur repéré à l'Étape 1 :
- Chaque `<sc-for>` devient une vraie requête Supabase suivie d'une vraie boucle de rendu qui clone le bloc HTML une fois par ligne reçue.
- Chaque `<sc-if>` devient un vrai calcul d'état (chargement pendant que la réponse arrive, vide si aucune ligne, rempli sinon, erreur si la requête échoue) — un écran qui ne montre que le cas "tout est rempli" n'est pas terminé.
- Chaque `{{valeur}}` isolé devient la vraie donnée correspondante.

**Le rendu généré doit rester aussi sémantique et accessible que le design source** — le design en est déjà plutôt bien pourvu (vrais `<nav>`/`<main>`/`<button>`/`<a href>`, `aria-label`, `aria-current`, `aria-pressed`, texte pour lecteur d'écran, focus visible au clavier) : ne rien perdre de tout ça en générant dynamiquement le contenu d'un `sc-for` — jamais un `<div onClick>` à la place d'un vrai `<button>`/`<a>`, jamais un `aria-*` oublié en clonant un bloc, jamais un élément qui perd le focus clavier qu'il avait dans le design.

**Deux pièges techniques rencontrés en testant cette skill en conditions réelles, à éviter d'emblée :**
- **`hidden` et une classe CSS qui fixe son propre `display` se contredisent.** Un bloc masqué en JS via `element.hidden = true` peut rester visible si une classe lui applique par ailleurs `display: grid`/`flex` sans condition — les deux règles ont la même priorité, et celle qui charge en dernier gagne. Ajouter une fois pour toutes `[hidden] { display: none !important; }` dans `style.css`, avant toute autre règle, pour que l'attribut l'emporte toujours.
- **Ne jamais fixer `element.style.display` depuis le script.** Un style posé en ligne par le JS l'emporte sur toute règle de `style.css`, y compris une media query responsive (ex. une carte masquée par `@media` sur téléphone réapparaît si le script lui remet `style.display = "flex"`). Toujours passer par `element.hidden` ou par une classe pour changer la visibilité — jamais par une écriture directe de `style.display`.

## Étape 5 — Câbler l'écriture

Brancher au moins l'action clé de l'écran (créer, modifier ou supprimer une ligne — celle qui a le plus de sens pour ce produit) sur un vrai insert/update/delete Supabase. Donner un retour visuel immédiat après l'action (confirmation, changement visible, message d'erreur clair) — jamais un bouton qui semble ne rien faire pendant qu'une requête part en arrière-plan.

**Quand l'action clé de l'écran mène vers un écran absent du canvas** (ex. un bouton "Nouvelle fiche" sans écran "Nouvelle fiche" dans les fichiers lus à l'Étape 1) : ne pas se contenter de marquer le lien "à venir" et laisser l'écran sans aucune écriture. Vérifier d'abord si la PRD décrit une **version minimale** de cette action (ex. "un titre suffit pour créer une fiche, tout le reste est optionnel") :
- Si oui, construire cette version réduite directement sur l'écran actuel (un petit formulaire en ligne — un champ, un bouton — pas un écran complet) plutôt que d'attendre que l'écran manquant soit conçu. Ça reste fidèle à la PRD, pas une invention.
- Si la PRD ne décrit rien de tel, ou si l'action ne peut pas raisonnablement se réduire à un geste simple, l'écran reste en lecture seule pour cette passe — le dire clairement à l'apprenant plutôt que de forcer une écriture qui n'a pas de sens simplifié.

## Étape 6 — Vérifier réellement

Avant de commencer à cliquer dans l'écran soi-même, le dire à l'apprenant en une phrase : la suite consiste à tester l'écran à sa place (créer un compte, se connecter, cliquer, vérifier que tout fonctionne), puis à lui faire un retour — il pourra ensuite se promener lui-même dans son application une fois que ce sera fait. Il n'a rien à faire pendant ce temps, ce n'est pas à lui de tester en premier.

Une fois l'action clé testée, confirmer qu'elle a vraiment eu lieu en base — jamais se contenter de l'absence d'erreur côté navigateur. Relire la table concernée via l'API REST (`curl $SUPABASE_URL/rest/v1/<table>` avec la clé adaptée) ou, plus parlant pour l'apprenant, ouvrir Supabase Studio à côté de son écran et lui montrer la ligne apparaître. C'est le moment où l'apprenant doit voir de ses yeux que ce n'est plus une maquette.

**Si l'authentification a été construite (Étape 3)**, tester aussi le parcours complet : créer un vrai compte depuis le formulaire (avec une adresse de test jetable, compte supprimé ensuite via l'API Admin Auth pour ne pas polluer le projet), vérifier qu'il arrive sur un écran vide ; se déconnecter via la pastille ; se reconnecter avec le compte de démo et vérifier que les données d'exemple s'affichent ; vérifier qu'un visiteur sans session est bien renvoyé vers l'écran de connexion, y compris sur l'URL Vercel une fois déployé.

## Étape 7 — Lancer, pousser, rendre compte

1. Servir le fichier en local (un simple serveur statique suffit, pas de build) et donner l'URL locale explicitement dans la réponse — jamais "le serveur tourne" sans lien cliquable.
2. `git add`, `git commit -m "MVP — écran [nom]"`, `git push` sur la branche courante. Si un déploiement Vercel existe déjà (posé en module 1), ce push le met à jour automatiquement — le dire à l'apprenant, il n'a rien d'autre à configurer.
3. Compte-rendu en langage clair, jamais de terme technique lâché seul : dire d'abord ce que ça change pour l'apprenant en une phrase simple, puis ajouter le mot technique entre parenthèses juste après, pour qu'il s'y habitue sans que ça pèse ("ton écran n'affiche plus des exemples, mais tes vraies fiches enregistrées dans ta base (Supabase)" ; "chacun ne voit que ses propres données, jamais celles d'un autre apprenant (la règle qui fait ça s'appelle RLS)"). Signaler explicitement tout raccourci pris — notamment, si l'authentification a été construite : "quand quelqu'un crée un compte, il peut s'en servir tout de suite, sans cliquer sur un lien reçu par e-mail (la confirmation d'e-mail est désactivée) — pratique pour tester, à réactiver avant un vrai lancement".
4. **Si l'authentification a été construite, rappeler en clair les identifiants du compte de démo**, lus dans `.env`, juste après les liens local et Vercel : "pour voir tes données d'exemple, connecte-toi avec `demo@….test` / `<mot de passe>` ; pour tester comme un vrai utilisateur, crée ton propre compte — il démarrera vide". Exception explicite et volontaire à la règle de ne jamais afficher de secret dans la conversation : ce compte est factice, ne possède que des données d'exemple, et l'apprenant en a besoin pour se connecter. Cette exception ne s'étend à aucune autre valeur de `.env` (clés Supabase, token).
5. **Laisser le navigateur ouvert sur l'écran fonctionnel, pas juste donner un lien.** Cette skill teste elle-même l'écran en conditions réelles avant de le déclarer terminé (créer un compte, s'y connecter, essayer l'action clé) — l'apprenant n'a donc pas à refaire ce parcours pour la première fois tout seul. Terminer la session avec le navigateur affiché sur l'écran déjà connecté et fonctionnel, prêt à être cliqué, plutôt que de fermer la fenêtre et renvoyer l'apprenant ouvrir l'URL lui-même depuis zéro.

## Plusieurs écrans, et liens vers un écran qui n'existe pas encore

Si l'apprenant enchaîne sur un autre écran, reprendre à l'Étape 1 pour ce nouvel écran. Si le design contient des liens/boutons vers d'autres écrans, les rendre réellement cliquables au fur et à mesure (routage simple, une URL par écran déjà branché) plutôt que de laisser des liens morts.

Il arrive qu'un bouton du design pointe vers un écran qui n'existe pas (encore) comme fichier dans le canvas (ex. un bouton "Nouvelle fiche" sans écran "Nouvelle fiche" dans les artboards lus à l'Étape 1). Dans ce cas, ne jamais inventer cet écran de toutes pièces (seule exception : l'écran de connexion/inscription, voir Étape 3.2) : garder le bouton visible mais visuellement marqué comme non actif (grisé, ou une indication discrète "à venir") — il se rebranche vers sa vraie route le jour où cet écran est construit à son tour, via une nouvelle invocation de cette skill.

## Principes

- **Jamais de donnée en dur une fois cette skill passée sur un écran.** Le point de départ (le design) peut en contenir ; le point d'arrivée, non — c'est tout l'objet de cette skill.
- **Jamais la secret key côté navigateur.** Elle contourne RLS ; un fichier chargé par le client ne doit contenir que l'URL et la clé publishable.
- **Jamais d'identifiant, même factice, dans un fichier committé.** Les identifiants du compte de démo vivent dans `.env` (gitignoré), jamais dans un script committé ni dans `documents/schema-[slug].md` (poussé sur GitHub). Ils ne sortent de `.env` qu'une fois, en clair, dans le compte-rendu final (Étape 7) — exception limitée à ce compte factice.
- **Aucune connexion automatique, nulle part.** Personne n'est jamais connecté à sa place : ni en local, ni sur le site public. Le compte de démo s'utilise comme n'importe quel compte, en tapant ses identifiants dans le formulaire — un compte qui se connecterait tout seul donnerait à tout visiteur un accès en écriture à la base de l'apprenant.
- **L'authentification se déduit du schéma, elle ne se demande pas.** Pas de table scopée par utilisateur → pas de connexion ; au moins une → connexion, inscription et déconnexion construites d'emblée, avec des valeurs par défaut raisonnables (e-mail/mot de passe, inscription ouverte, confirmation d'e-mail désactivée pour le MVP). La décision « données privées ou non » a déjà été prise par `prd-bdd` à partir de la PRD — cette skill ne la remet jamais en jeu. En construisant l'auth, expliquer la différence entre le compte de démo et un vrai compte.
- **Le texte du design est un brouillon, pas une consigne verrouillée — et toute hésitation se pose à l'apprenant, tout de suite.** Ne jamais recopier un texte tel quel sans vérifier qu'il correspond au cas réel, et ne jamais trancher seul en silence une ambiguïté rencontrée pendant la traduction (texte, donnée manquante, action peu claire) : la poser sur le moment, pas seulement la signaler après coup dans le compte-rendu.
- **Une seule logique d'authentification, partagée.** Garde de session, pastille de compte et déconnexion vivent dans `auth.js`, importé par chaque écran branché — jamais recopiés d'un écran à l'autre. La pastille (initiale + menu avec e-mail et « Se déconnecter ») reprend le style du design et reste discrète.
- **Pas de stack à choisir.** Le design se lit déjà comme du HTML/CSS proche du standard ; cette skill reste en HTML/CSS/JS vanilla avec le client Supabase en CDN, sans jamais introduire un bundler ou un framework de sa propre initiative.
- **HTML, CSS et JS toujours dans des fichiers séparés.** Jamais de style en ligne recopié tel quel, jamais de script mêlé au balisage — même exigence de propreté qu'un développeur web expérimenté appliquerait, pas une simplification "parce que c'est pour débuter".
- **`<head>` toujours complet.** Langue, titre propre au produit, charset, description — jamais un `<head>` minimal laissé tel quel.
- **Sémantique et accessibilité préservées, y compris dans ce qui est généré dynamiquement.** Le design source respecte déjà de bonnes pratiques (vraies balises interactives, `aria-*`, focus clavier) ; le contenu inséré par un `sc-for` doit les garder, pas seulement le contenu statique autour.
- **Les deux variantes d'un même écran (ordinateur/téléphone) branchées ensemble.** Pas seulement celle citée en premier par l'apprenant — un vrai comportement responsive, pas un choix arbitraire entre les deux ; si l'une des deux n'a pas les mêmes marqueurs dynamiques que l'autre (variante restée plus sommaire dans le design), lui appliquer la même logique plutôt que la laisser figée.
- **Un écran sans action d'écriture propre n'est pas forcément un écran muet.** Quand l'action clé mène vers un écran absent du canvas, chercher dans la PRD une version minimale de cette action avant de renoncer à toute écriture — jamais inventer l'écran manquant, mais pas non plus abandonner l'écriture si la PRD en décrit un geste réduit.
- **Additif sur les fichiers existants.** L'ancien contenu de la racine est archivé, jamais supprimé silencieusement.
- **Vérifié, pas supposé.** Une action d'écriture n'est confirmée à l'apprenant qu'après relecture réelle en base (Étape 6), jamais parce que le navigateur n'a pas affiché d'erreur.
- **Compte-rendu en langage clair d'abord, terme technique ensuite, entre parenthèses.** Dire ce que ça change pour l'apprenant en une phrase simple, puis nommer le mot technique (RLS, session, requête...) entre parenthèses juste après — jamais l'inverse, jamais un terme technique lâché seul sans sa traduction.
- **Cette skill teste elle-même l'écran avant de le déclarer terminé, l'apprenant n'a pas à refaire ce premier passage seul.** Elle se connecte, clique, vérifie en base — la session se termine navigateur ouvert sur l'écran déjà fonctionnel, pas sur un simple lien à aller ouvrir.