---
name: jtbd-prd
description: Crée un Product Requirements Document (PRD) à partir d'un Job-To-Be-Done (JTBD). Pose des questions de clarification produit avant de rédiger le document, inclut une liste d'écrans priorisée, un flux de navigation entre écrans, et une proposition de schéma de base de données. Sauvegarde le résultat en markdown dans le dossier documents/ du repo. À utiliser quand l'utilisateur fournit un JTBD (ou "en tant que [utilisateur], quand [situation], je veux [motivation], afin de [résultat]") et souhaite un cahier des charges complet prêt à être transmis au design ou au code.
---

# PRD à partir d'un JTBD

Transformer un Job-To-Be-Done en PRD prêt à construire — incluant les écrans, le flux de navigation, et un schéma de données — sauvegardé directement dans le dossier `documents/` du projet.

## Entrée

L'utilisateur fournit un JTBD via `$ARGUMENTS`, sous la forme qu'il a à disposition (un énoncé complet "quand... je veux... afin de...", ou une version plus brute). Si rien n'est fourni, demander le JTBD avant de continuer.

## Étape 1 — Analyser le JTBD

Décomposer le JTBD en :
- **Utilisateur** — qui effectue cette tâche ? Un seul type d'utilisateur, ou plusieurs avec des besoins différents ?
- **Situation/déclencheur** — quel moment ou quelle condition initie ce besoin ?
- **Motivation** — qu'est-ce que la personne essaie réellement d'accomplir, au-delà de l'action énoncée ?
- **Friction actuelle** — que fait-elle aujourd'hui à la place, et qu'est-ce qui ne fonctionne pas ?

En déduire ce qui manque encore pour rédiger un cahier des charges complet :
- Les entités/objets de données essentiels que le produit devra stocker ou manipuler
- S'il existe plusieurs types de comptes/utilisateurs avec des permissions ou des vues différentes
- Les écrans impliqués par le JTBD dont le périmètre ou la priorité ne sont pas encore clairs
- Ce que l'utilisateur voudra probablement exclure explicitement

## Étape 2 — Poser des questions de clarification

Ne poser que les questions qu'on ne peut pas déduire avec confiance du JTBD. Maximum 7 questions — plus de marge qu'un PRD classique basé sur un brain dump, car celui-ci doit aussi nourrir un schéma de données et un flux d'écrans, pas seulement un énoncé de problème. Être direct, une phrase par question, numérotées. Couvrir (seulement si réellement flou) :

1. Les entités principales et leurs attributs clés (que doit retenir le produit ?)
2. Le nombre de types de comptes/utilisateurs et en quoi leurs permissions ou vues diffèrent
3. Les exclusions explicites — que ne doit surtout PAS faire le produit pour l'instant ?
4. Quels écrans sont prioritaires ("hero") par rapport aux écrans secondaires/dialogues
5. Tout système, application ou source de données existante avec laquelle ce produit doit s'aligner ou qu'il doit lire
6. Toute contrainte forte (réutiliser un design system existant, s'intégrer avec X, etc.)
7. Les critères de succès — à quoi ressemble "ça fonctionne" concrètement ?

Ne pas passer à l'étape 3 avant d'avoir reçu les réponses.

## Étape 3 — Rédiger le PRD

Rédiger le PRD dans la même langue que le JTBD fourni. Le produire sous forme de bloc de code markdown **et** le sauvegarder (voir Étape 4).

Utiliser cette structure :

---

# PRD : [Titre]

**Type :** Feature | Produit | API
**Auteur :** login github du créateur
**Date :** [date du jour]
**Statut :** Brouillon

---

## Problème

[2 phrases maximum, dérivées de la situation et de la friction du JTBD.]

## Personas

[Un persona par type d'utilisateur identifié à l'étape 1 (souvent un seul). Pour chaque persona : un nom court de rôle (pas un prénom fictif), son contexte/situation par rapport au produit — repris du JTBD — et le cas d'usage principal qui le concerne. 2 à 3 phrases par persona, pas une fiche persona marketing. Si plusieurs types de comptes ont des permissions ou des vues différentes (question 2 de l'étape 2), c'est ici qu'il faut le trancher explicitement — pas laisser la section Écrans le découvrir implicitement.]

## Objectif

[1 phrase. À quoi ressemble "c'est fait" du point de vue de l'utilisateur, dérivé de la motivation du JTBD.]

## Solution proposée

[3–6 puces. Ce qu'on construit + les décisions de conception/techniques clés.]

## Récits utilisateurs

- En tant que [utilisateur], je veux [action] afin de [résultat].

[Maximum 5 récits. N'inclure que ceux qui ne sont pas évidents — le JTBD couvre déjà le principal.]

## Écrans

Numéroter chaque écran. Mettre en gras et flaguer **(hero)** les écrans prioritaires identifiés à l'étape 2 (ou déduits si l'utilisateur a sauté cette question) — ce sont ceux à concevoir ou construire en premier si seul un sous-ensemble est réalisé. Regrouper par section/zone si le produit en compte plusieurs. Pour chaque écran, une ligne sur sa fonction — suffisant pour que quelqu'un puisse le concevoir sans redemander à quoi il sert.

## Flux entre écrans

Pas une liste — un parcours. Décrire, en quelques courts paragraphes ou une séquence numérotée, comment un utilisateur se déplace réellement entre les écrans ci-dessus : ce qui est cliquable, ce qui ouvre quoi, où les dialogues interrompent le flux, et où l'utilisateur arrive après avoir terminé une action. Couvrir explicitement la structure de navigation (ex. : barre de tabs persistante vs. écran d'accueil en hub vs. assistant linéaire) — ne pas la laisser implicite.

**Avant de finaliser cette section, la vérifier par rapport à la section Écrans ci-dessus pour détecter d'éventuelles contradictions** — par exemple une liste d'écrans qui suggère un switcher à cartes en accueil alors que le flux décrit une barre de tabs persistante. Si le JTBD ou les réponses de l'utilisateur ne permettent pas de trancher, poser la question plutôt que de choisir en silence ; c'est l'écart le plus fréquent à ce stade d'un PRD, et il est peu coûteux à corriger ici, coûteux à corriger une fois un écran construit.

## Schéma de données

Une proposition, pas une version finale — suffisante pour démarrer la construction. Pour chaque entité principale : nom de table, colonnes clés avec leur type, et relations avec les autres entités (un-à-plusieurs, plusieurs-à-plusieurs, etc.). Utiliser un bloc de code par entité (colonnes alignées, une par ligne) plutôt qu'un tableau markdown — les tableaux markdown ne se rendent pas de façon fiable dans tous les outils où ce PRD peut être ouvert (ils peuvent s'afficher en texte brut avec les `|`), alors qu'un bloc de code reste lisible partout. Se limiter au périmètre de ce PRD — ne pas concevoir pour des fonctionnalités futures hypothétiques. Signaler explicitement toute colonne qui encode une limite de permission/propriété (ex. : une colonne `owner` ou `account_id` pour l'isolation multi-compte), car cela a des implications pour chaque écran qui touche cette entité.

## Hors périmètre

[Liste à puces des exclusions explicites. Si rien n'a été exclu, écrire "À définir — à clarifier avant le début du développement."]

## Métriques de succès

[2–3 résultats mesurables. Privilégier le quantitatif. Dérivés de la réponse aux critères de succès de l'étape 2 si elle a été donnée.]

## Questions ouvertes

[Puces. Décisions ou inconnues à résoudre avant ou pendant la construction. Si aucune, écrire "Aucune."]

---

## Étape 4 — Sauvegarder le fichier

1. Trouver la racine du repo : exécuter `git rev-parse --show-toplevel`.
   - Si succès, le dossier cible est `[racine-du-repo]/documents/`.
   - Si échec (pas un repo git), utiliser `./documents/` relatif au répertoire de travail courant, et informer l'utilisateur que la sauvegarde s'est faite hors d'un repo.
2. Créer le dossier `documents/` s'il n'existe pas déjà.
3. Sauvegarder le PRD sous `documents/PRD-[slug].md`, où `[slug]` est un nom court en kebab-case dérivé du titre.
4. Confirmer le chemin sauvegardé à l'utilisateur.

## Principes

- **Court vaut mieux que long.** Si une section peut tenir en une phrase, la garder en une phrase.
- **Tranché.** Faire une recommandation, ne pas lister des options sans préférence.
- **Sans remplissage.** Supprimer les mots comme « exploiter », « de manière fluide », « intuitif », « robuste ».
- **Périmètre explicite.** La section Hors périmètre évite la dérive de scope — toujours la remplir.
- **Le flux est un parcours, pas une liste.** Si la section Flux entre écrans peut se lire comme une liste à puces sans perdre de sens, elle ne remplit pas son rôle — elle doit décrire un mouvement entre écrans, pas seulement les énumérer.
- **Le schéma correspond au périmètre.** Ne pas ajouter de tables ou de colonnes pour des fonctionnalités qui sont en Hors périmètre.
- **Personas dérivés du JTBD, jamais inventés.** Reprendre le rôle/contexte déjà établi à l'étape 1 — ne pas ajouter d'âge, de prénom fictif ou de trait qui n'a pas été demandé et n'explique rien du besoin.
