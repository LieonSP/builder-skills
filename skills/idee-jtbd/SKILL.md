---
name: idee-jtbd
description: Aide un apprenant en formation product/vibe coding à transformer la description brute de son idée d'application ou de son problème en un Job-To-Be-Done (JTBD) clair et son ou ses persona(s), avec une explication de pourquoi ce JTBD a du sens. Sauvegarde l'idée initiale, le JTBD et les personas dans `documents/jtbd-personas.md` du repo, réutilisé ensuite par la skill `jtbd-prd`. Si aucune description n'est fournie, demande d'abord "Quelle est l'application ou le problème que tu souhaites vibe coder ?" avant de continuer.
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
- **Profil utilisateur** — qui vit cette situation, dans son rôle ou son contexte par rapport au problème (ex. "gestionnaire de plusieurs appartements en location courte durée", pas juste "un utilisateur") ? Chercher un rôle/contexte fonctionnel, pas une donnée démographique (âge, genre...) qui n'explique pas le besoin.

Sauf si la description rend déjà ces quatre éléments limpides, poser 2 à 4 questions ciblées avant de continuer — jamais plus, l'exercice doit rester rapide. Ne questionner que ce qui manque réellement parmi situation, motivation, alternative actuelle et profil utilisateur ; ne pas redemander ce qui est déjà donné, même implicitement. Attendre les réponses avant de passer à l'étape 2. Si les quatre éléments sont déjà clairs, ne poser aucune question et inférer directement.

## Étape 2 — Formuler le JTBD et le(s) persona(s)

Un seul JTBD, sous cette forme :

> **Quand** [situation], **je veux** [motivation réelle], **afin de** [résultat recherché].

Règles :
- Le JTBD doit être **plus large que la solution décrite** — s'il se lit comme un simple résumé de la feature ("je veux une app qui liste mes tâches afin d'avoir une liste de tâches"), ce n'est pas un JTBD, recommencer l'inférence.
- Une seule situation/motivation principale. Si la description mélange clairement deux profils utilisateur ou deux besoins différents, proposer 2 JTBD maximum, jamais plus.
- Rester concret — éviter les formulations abstraites du type "afin de mieux m'organiser" sans préciser dans quoi ni pourquoi c'est important maintenant.

Puis, pour chaque profil utilisateur identifié à l'étape 1 (un JTBD ↔ un persona), formuler un persona court :

> **[Nom court de rôle]** — [contexte/situation par rapport au produit]. Cas d'usage principal : [repris de la motivation/du résultat du JTBD correspondant].

2 à 3 phrases par persona, jamais une fiche persona marketing — même règle que pour le profil utilisateur à l'étape 1 : un rôle/contexte, pas un âge, un prénom fictif ou un trait qui n'explique rien du besoin.

## Étape 3 — Expliquer pourquoi

Toujours après le JTBD, en 3 points courts :

1. **D'où vient ce JTBD** — quel(s) indice(s) précis de la description, y compris le profil utilisateur donné, ont permis de le formuler (citer les mots de l'apprenant si possible).
2. **Pourquoi ça tient** — pourquoi ce JTBD reflète un vrai besoin plutôt qu'une feature qu'on a envie de construire.
3. **Ce que ça débloque** — en quoi partir de ce JTBD (plutôt que de la solution initiale) aide à préciser la description produit : ça oblige à trancher qui est l'utilisateur, ce qu'il fait aujourd'hui à la place, et ce qu'un "ça marche" concret voudrait dire — les trois angles morts les plus fréquents quand on part direct sur la solution en vibe coding.

## Étape 4 — Sauvegarder le JTBD et les personas

1. Trouver la racine du repo : exécuter `git rev-parse --show-toplevel`.
   - Si succès, le dossier cible est `[racine-du-repo]/documents/`.
   - Si échec (pas un repo git), utiliser `./documents/` relatif au répertoire de travail courant, et informer l'apprenant que la sauvegarde s'est faite hors d'un repo.
2. Créer le dossier `documents/` s'il n'existe pas déjà.
3. Sauvegarder sous `documents/jtbd-personas.md`, avec cette structure :

```
# JTBD & Personas

## Idée initiale

[La description brute fournie par l'apprenant à l'Entrée, verbatim — pas reformulée, pas résumée.]

## JTBD

> **Quand** [situation], **je veux** [motivation], **afin de** [résultat].

## Personas

**[Nom de rôle]** — [contexte/situation]. Cas d'usage principal : [...].
```

(un JTBD et un persona par profil identifié, dans le même ordre — la skill `jtbd-prd` réutilise directement ce fichier ensuite, y compris l'idée initiale pour vérifier qu'elle sert toujours le JTBD une fois élargi.)

4. Si le fichier existe déjà (relance de la skill avec une description reformulée), l'écraser avec la nouvelle version — une seule version à la fois, jamais un historique de tentatives accumulées.
5. Confirmer le chemin sauvegardé à l'apprenant.

## Étape 5 — Ouvrir la suite

Terminer par une invitation à relancer la skill avec une description reformulée si le JTBD proposé ne lui semble pas juste — le but est qu'il challenge la proposition, pas qu'il l'accepte par défaut. Ne pas proposer d'enchaîner vers une autre skill ou un autre outil : cette skill s'arrête au JTBD (la sauvegarde du fichier n'est pas une suggestion de prochaine étape, juste une persistance du résultat).

## Principes

- **Simple et rapide.** Un aller-retour, pas une interview complète — cette skill est un exercice pédagogique, pas un outil de discovery approfondi.
- **Un JTBD, pas un menu.** Toujours trancher vers une proposition claire plutôt que lister des variantes.
- **La solution n'est jamais le JTBD.** Si la réponse ressemble trop à la description initiale, c'est le signal qu'il faut creuser plus loin la motivation réelle.
- **Pédagogique, pas jargonneux.** Les apprenants découvrent le concept — expliquer simplement, sans citer de théorie non demandée.
- **Le fichier reflète la dernière proposition, pas un historique.** Toujours écraser `documents/jtbd-personas.md` à chaque exécution plutôt que d'accumuler des versions.
- **L'idée initiale reste traçable.** Toujours la sauvegarder verbatim dans le fichier, même si le JTBD final s'en écarte largement — c'est ce qui permet à `jtbd-prd` de vérifier plus tard si elle sert encore le JTBD élargi.
