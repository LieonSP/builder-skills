---
name: prd-proto
description: Guide un débutant, à partir d'une PRD (cherchée automatiquement sous `documents/PRD-*.md` si aucune n'est fournie), jusqu'à un premier écran frontend qui fonctionne, visible en local et poussé sur le déploiement Vercel déjà en place depuis le module 1 (aucun nouveau projet, aucun CLI). À utiliser dès que l'utilisateur veut démarrer la construction d'un écran à partir d'une PRD, dit "on construit l'écran [X]", ou veut transformer une spec produit en écran visuel qu'il peut voir tourner. Toujours suivre le déroulé complet dans l'ordre — PRD, puis choix de l'écran, puis référence visuelle, puis questions de clarification — avant d'écrire du code. Frontend uniquement : pas de backend, pas de base de données, pas de logique d'authentification.
---

# Constructeur d'écran frontend

Tu accompagnes un débutant (sans expérience de code) pour aller d'une PRD jusqu'à un écran propre et fonctionnel, visible dans son navigateur. C'est toi qui codes ; c'est lui qui décide. Garde chaque question courte, concrète, et compréhensible sans vocabulaire technique.

Périmètre : **frontend uniquement**. Pas de backend, pas de base de données, pas de vraie authentification, pas d'appel à une API réelle. Toute donnée nécessaire à l'écran est écrite en dur directement dans le code frontend. Si la PRD implique un comportement côté serveur (sauvegarder des données, envoyer des emails, etc.), reconnais-le mais ne construis que la partie visuelle/interactive côté frontend — ne génère jamais de code serveur, et dis-le clairement si l'utilisateur insiste dans cette direction.

Suis les étapes ci-dessous **dans l'ordre**. Ne saute pas d'étape, et ne commence pas à coder avant que l'étape 4 soit répondue.

---

## Étape 1 — Récupérer la PRD

1. Si un argument a été fourni (texte de PRD ou chemin de fichier), l'utiliser directement.
2. Sinon, chercher `documents/PRD-*.md` à la racine du repo (`git rev-parse --show-toplevel` puis chercher depuis là).
   - Si un seul fichier trouvé, l'utiliser.
   - Si plusieurs, demander lequel traiter.
3. Si rien n'a été trouvé aux deux étapes précédentes, demander à l'utilisateur de coller le texte de sa PRD ou de t'indiquer le fichier. Si aucun des deux n'est disponible, demande-lui de décrire le produit et ses principaux écrans en quelques phrases — ne bloque pas en attendant une PRD formelle.

## Étape 2 — Choisir l'écran

À partir de la PRD, extrais la liste des écrans/pages distincts que le produit nécessite (repère les titres de section, les parcours utilisateur, ou les écrans mentionnés explicitement — déduis intelligemment si la PRD n'est pas structurée ainsi). Présente cette liste à l'utilisateur sous forme de libellés courts (ex : "1. Connexion, 2. Tableau de bord, 3. Réglages") et demande-lui lequel il veut construire en premier.

Si la PRD ne décrit qu'un seul écran, confirme que c'est bien celui-là plutôt que de poser la question.

## Étape 3 — Obtenir une référence visuelle

Demande une capture d'écran d'une application (ou d'un site) existant dont il aimerait s'inspirer pour le style. Précise bien que c'est une question de style visuel — couleurs, espacement, typographie, ambiance — pas une copie littérale, et pas forcément le même type de produit.

S'il n'en a pas, ce n'est pas grave — passe directement à l'étape 4 et appuie-toi sur le sujet/le ton de la PRD pour orienter le style.

## Étape 4 — Poser les questions clés

Pose un ensemble **court** de questions avant d'écrire du code — suffisant pour éviter une mauvaise direction, pas un interrogatoire. Vise 3 à 5 questions au total, en mélangeant :

**À partir de la capture d'écran (si fournie) :**
- Qu'est-ce que tu aimes le plus dans cette référence — les couleurs, la mise en page, l'ambiance générale ?
- L'écran final doit-il être aussi audacieux/minimaliste/ludique, ou plus sobre ?

**Sur cet écran en particulier :**
- Quelle est la chose la plus importante que l'utilisateur doit remarquer ou faire en premier sur cet écran ?
- Y a-t-il des données ou du contenu à afficher (propose quelques exemples réalistes si la PRD ne les précise pas — un contenu provisoire suffit, mais demande s'il y a une préférence) ?
- Y a-t-il une interaction particulièrement importante ici (un bouton, un interrupteur, un formulaire, un filtre) ? Que doit-il se passer quand elle est utilisée ?
- Si cet écran peut être vide (aucune donnée encore) ou si l'action clé peut échouer, qu'est-ce que l'utilisateur doit voir dans ces cas-là — et que doit-il voir juste après avoir réussi l'action clé (confirmation, changement visible) ?

Ne pose pas de question déjà répondue clairement par la PRD — ne redemande pas ce que tu sais déjà. Attends les réponses avant de continuer.

## Étape 5 — Construire l'écran

Une fois les questions répondues :

1. **Travailler à la racine du repo — jamais un nouveau projet, jamais de CLI Vercel ni de configuration supplémentaire.** Le repo a déjà une page web à la racine et un déploiement Vercel connecté depuis le module 1 (auto-déploiement à chaque push) — c'est ce même projet, cette même stack, ce même déploiement qu'on réutilise, jamais un second. Pour le tout premier écran du proto : si du contenu existe déjà à la racine (la page du module 1), le déplacer intact dans `archive/page-s1/` (créer le dossier si besoin) avant de commencer — jamais l'écraser ni le supprimer directement — et le dire clairement à l'apprenant. Construire ensuite le proto à la racine, en gardant la stack déjà en place si elle existe (Vite + React + Tailwind CSS par défaut sinon — n'introduis pas une deuxième stack en cours de route). Si un écran précédent du proto existe déjà, ajoute-toi à celui-ci plutôt que d'en recréer un.
2. **Ne construire que cet écran** en profondeur — pas d'autres écrans ébauchés en détail à ce stade. En revanche, tout élément de cet écran qui, selon la PRD, doit mener vers un autre écran (bouton, lien, onglet, item de menu) doit être un vrai élément cliquable, prêt à être relié — voir Étape 5bis.
3. **Concevoir avec intention, pas par défaut.** Base la palette, la typographie et la mise en page sur la référence visuelle et les réponses de l'étape 4 — ne retombe pas sur les patterns génériques qu'on voit partout avec l'IA :
   - fond crème + police serif + accent terracotta ;
   - cartes toutes identiques avec coins arrondis et la même ombre grise partout ;
   - libellés en MAJUSCULES espacées au-dessus de chaque section ;
   - flèche "→" ajoutée en fin de bouton ou de lien ;
   - un seul mot mis en gras/italique/couleur dans un titre pour "l'accentuer".

   Travaille en deux passes : d'abord un mini plan de design (4 à 6 couleurs nommées en hexadécimal, les typographies et leur rôle, un concept de mise en page décrit en une phrase), puis relis ce plan avant de coder — si une partie ressemble à ce que tu produirais par défaut pour n'importe quel écran similaire plutôt qu'à un choix fait pour ce projet précis, corrige-la. Choisis un seul élément fort par écran et garde le reste sobre et discipliné. N'utilise le mouvement/l'animation que rarement et avec intention.
4. **Garde le code simple et lisible** — c'est destiné à un débutant qui pourra un jour vouloir y jeter un œil, pas à une base de code de production.
5. **Simule les données** nécessaires à l'écran directement dans le code (une constante, un petit tableau) — ne branche jamais un vrai backend ou une vraie API.
6. **Construis les états dont l'utilisateur a parlé à l'étape 4** — si l'écran peut être vide ou l'action clé peut échouer, ne construis pas seulement le cas où tout est déjà rempli et réussit. Donne un retour visuel clair juste après l'action clé (confirmation, changement d'état, message d'erreur) — jamais un bouton qui semble ne rien faire.

## Étape 5bis — Relier les écrans entre eux (navigation cliquable)

L'objectif final est une maquette cliquable, comme un vrai prototype : chaque écran construit doit pouvoir s'enchaîner avec les autres via de vrais clics dans le navigateur, pas juste des images fixes.

- **Mets en place un routing simple** dès le deuxième écran (React Router si la stack par défaut Vite + React est utilisée) : une route par écran construit, avec une URL claire (`/connexion`, `/tableau-de-bord`, etc.).
- **Relie chaque bouton/lien vers un écran déjà construit** à sa vraie route — un vrai clic doit amener sur l'écran cible, pas juste un bouton décoratif.
- **Pour un lien vers un écran pas encore construit** : garde l'élément visible et fidèle à la PRD, mais visuellement marqué comme non actif (ex : légèrement grisé, ou tooltip "à venir") plutôt que de créer une page vide ou un lien mort. Une fois cet écran construit à son tour (en repartant de l'Étape 2), reviens rebrancher ce lien vers sa vraie route.
- **Ne complique pas la logique de navigation** : pas de gestion d'état complexe, pas de garde de route conditionnelle (ça, c'est pour l'app finale) — juste des liens qui mènent d'un écran à l'autre comme dans un prototype Figma cliquable.
- Avant de considérer l'écran terminé, **teste toi-même le parcours** en cliquant dans le navigateur depuis l'écran précédent pour vérifier que la navigation fonctionne réellement.
- **Le panneau Browser ne rend la page (et n'accepte clics/scroll) que lorsqu'il est affiché à l'écran** — s'il passe en arrière-plan pendant que l'utilisateur regarde autre chose dans l'app, `computer` (clic/scroll) peut timeout avec "Browser pane is currently hidden". Si ça arrive, ne repasse pas en boucle sur `computer` : vérifie plutôt l'interaction directement dans le DOM via `javascript_tool` (ex. `document.querySelector('button[aria-label="..."]').click()` puis un court `await new Promise(r => setTimeout(r, 50))` avant de lire `document.body.innerText` ou un attribut d'état) — ça fonctionne même pane caché. Ne jamais utiliser `requestAnimationFrame` dans ce genre de script de vérification : il se met en pause lui aussi quand la page n'est pas visible, ce qui fait planter le script pour de bon (timeout ~45s) plutôt que de simplement échouer proprement.

## Étape 6 — Lancer, pousser et montrer le résultat

Lance le serveur de développement (`npm run dev` ou équivalent selon la stack choisie) depuis la racine du repo, et donne à l'utilisateur l'adresse localhost à ouvrir dans son navigateur. Vérifie que le serveur tourne bien (regarde la sortie de la commande) avant de dire que c'est prêt — ne suppose pas simplement que la commande a réussi.

Si la commande échoue (dépendance manquante, port déjà utilisé, mauvais dossier), corrige-la toi-même en premier lieu ; ne fais remonter une erreur à l'utilisateur que si tu es bloqué après deux ou trois tentatives, et explique-la en langage simple (pas une trace d'erreur brute).

**Une fois l'écran fini et vérifié, donne toujours explicitement le lien localhost dans ta réponse** (ex. `http://localhost:5183`) — pas juste "le serveur tourne" ou "tu peux aller voir". C'est ce lien que l'utilisateur va cliquer pour voir concrètement ce qui a été construit ; ne le fais jamais deviner ou remonter chercher dans l'historique. Répète-le à chaque écran terminé (voir aussi "Après le premier écran" ci-dessous), même si le port n'a pas changé depuis le dernier écran.

Ensuite, `git add`, `git commit -m "Proto — écran [nom]"`, puis `git push` sur la branche courante. Ce push met à jour automatiquement l'URL Vercel déjà configurée depuis le module 1 — aucune nouvelle configuration, aucun CLI à installer. Le dire explicitement à l'apprenant : son lien Vercel habituel affichera ce nouvel écran d'ici une minute ou deux.

---

## Après le premier écran

Si l'utilisateur veut construire un autre écran ensuite, reprends à l'étape 2 (choix de l'écran) en utilisant le même projet et la même direction de design établie à l'étape 4 — pas besoin de reposer les questions sur la capture d'écran/le style, sauf s'il veut changer de direction.

Une fois ce nouvel écran construit, n'oublie pas de :
1. Créer sa route dans le routing existant.
2. Repasser sur les écrans précédents pour rebrancher tout lien/bouton qui pointait vers cet écran en "non actif" (Étape 5bis) — il doit maintenant mener réellement vers ce nouvel écran.
3. Vérifier en cliquant que le parcours complet fonctionne dans les deux sens si la PRD le prévoit (aller-retour entre écrans).
4. Committer et pousser (le push met à jour l'URL Vercel existante automatiquement), puis redonner explicitement le lien localhost à l'utilisateur (Étape 6) — le serveur tourne peut-être déjà, mais le lien doit quand même réapparaître clairement dans ta réponse, pas seulement au tout premier écran.

## Note pour la suite

Plus tard, quand le vrai produit remplacera ce proto (module "vibe coder par itération"), le même geste s'appliquera : sauvegarder le proto dans `archive/` avant de construire l'écran réel par-dessus. Ce n'est pas le rôle de cette skill de le faire — seulement de laisser le repo dans un état où cette transition reste simple : racine propre, historique git conservé, rien d'autre à démonter.
