---
name: jtbd
description: Aide un apprenant en formation product/vibe coding à transformer la description brute de son idée d'application ou de son problème en un Job-To-Be-Done (JTBD) clair, avec une explication de pourquoi ce JTBD a du sens et de ce qu'il apporte pour préciser sa description produit. Si aucune description n'est fournie, demande d'abord "Quelle est l'application ou le problème que tu souhaites vibe coder ?" avant de continuer.
---

# JTBD à partir d'une idée d'application

Transformer la description brute d'une idée d'app ou d'un problème (souvent formulée comme une solution) en un Job-To-Be-Done, avec l'explication qui permet à l'apprenant de comprendre pourquoi ce JTBD est le bon point de départ pour construire son app.

## Entrée

Si l'apprenant a fourni une description via `$ARGUMENTS`, l'utiliser directement. Sinon, poser cette question et attendre la réponse avant de continuer :

> Quelle est l'application ou le problème que tu souhaites vibe coder ?

Ne pas juger la forme de la réponse — un brain dump, une liste de features, une phrase floue sont tous des entrées valables. C'est le matériau brut à transformer.

## Étape 1 — Lire derrière la solution

La description donnée est presque toujours **une solution**, pas un job — l'apprenant a réfléchi une semaine à *quoi* construire, pas encore à *pourquoi* quelqu'un en aurait besoin. Chercher dans sa description :

- **Situation / déclencheur** — à quel moment, dans quel contexte cette app serait utilisée ?
- **Motivation réelle** — au-delà de la fonctionnalité décrite, qu'est-ce que la personne essaie vraiment d'accomplir ?
- **Alternative actuelle** — que fait-elle aujourd'hui à la place (rien, un carnet, une autre app, une galère manuelle) ?

Si ces trois éléments manquent presque totalement (description d'une ligne, juste un nom de feature), poser au maximum 2 questions ciblées avant de continuer — jamais plus, l'exercice doit rester rapide. Sinon, inférer directement à partir des indices présents, même implicites.

## Étape 2 — Formuler le JTBD

Un seul JTBD, sous cette forme :

> **Quand** [situation], **je veux** [motivation réelle], **afin de** [résultat recherché].

Règles :
- Le JTBD doit être **plus large que la solution décrite** — s'il se lit comme un simple résumé de la feature ("je veux une app qui liste mes tâches afin d'avoir une liste de tâches"), ce n'est pas un JTBD, recommencer l'inférence.
- Une seule situation/motivation principale. Si la description mélange clairement deux publics ou deux besoins différents, proposer 2 JTBD maximum, jamais plus.
- Rester concret — éviter les formulations abstraites du type "afin de mieux m'organiser" sans préciser dans quoi ni pourquoi c'est important maintenant.

## Étape 3 — Expliquer pourquoi

Toujours après le JTBD, en 3 points courts :

1. **D'où vient ce JTBD** — quel(s) indice(s) précis de la description ont permis de le formuler (citer les mots de l'apprenant si possible).
2. **Pourquoi ça tient** — pourquoi ce JTBD reflète un vrai besoin plutôt qu'une feature qu'on a envie de construire.
3. **Ce que ça débloque** — en quoi partir de ce JTBD (plutôt que de la solution initiale) aide à préciser la description produit : ça oblige à trancher qui est l'utilisateur, ce qu'il fait aujourd'hui à la place, et ce qu'un "ça marche" concret voudrait dire — les trois angles morts les plus fréquents quand on part direct sur la solution en vibe coding.

## Étape 4 — Ouvrir la suite

Terminer par une invitation à relancer la skill avec une description reformulée si le JTBD proposé ne lui semble pas juste — le but est qu'il challenge la proposition, pas qu'il l'accepte par défaut. Ne pas proposer d'enchaîner vers une autre skill ou un autre outil : cette skill s'arrête au JTBD.

## Principes

- **Simple et rapide.** Un aller-retour, pas une interview complète — cette skill est un exercice pédagogique, pas un outil de discovery approfondi.
- **Un JTBD, pas un menu.** Toujours trancher vers une proposition claire plutôt que lister des variantes.
- **La solution n'est jamais le JTBD.** Si la réponse ressemble trop à la description initiale, c'est le signal qu'il faut creuser plus loin la motivation réelle.
- **Pédagogique, pas jargonneux.** Les apprenants découvrent le concept — expliquer simplement, sans citer de théorie non demandée.
