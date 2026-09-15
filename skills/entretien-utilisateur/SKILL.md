---
name: entretien-utilisateur
description: Guide un apprenant à travers une recherche utilisateur qualitative complète, étape par étape — définir l'objectif de la recherche, valider le persona ciblé (en s'appuyant sur documents/jtbd-personas.md si disponible), générer un script d'entretien (contexte, friction, comportement passé, conséquence) sauvegardé dans documents/script-entretien.md, puis simuler un entretien avec un utilisateur fictif sauvegardé dans documents/simulation-entretien.md. Ne saute jamais une étape — chacune s'arrête pour validation avant de continuer. À utiliser quand un apprenant veut préparer un entretien utilisateur, ou s'entraîner avant d'en mener un réel.
---

# Entretien utilisateur

Reproduire les trois étapes d'une recherche utilisateur qualitative — définir le cadre, préparer le contenu, simuler la recherche — en s'arrêtant à chaque étape pour validation avant de passer à la suivante. Jamais tout d'un coup.

## Entrée

Chercher `documents/jtbd-personas.md` à la racine du repo (`git rev-parse --show-toplevel` puis chercher depuis là). S'il existe, le lire : il donne le JTBD, l'idée initiale et les personas déjà tranchés, qui nourriront les étapes 1 et 2 sans tout redemander depuis zéro. Ne pas bloquer s'il est absent — demander directement à l'apprenant à l'étape 1.

## Étape 1 — Définir l'objectif de la recherche

Demander à l'apprenant quel est l'objectif de sa recherche utilisateur : la question à laquelle elle doit répondre, formulée comme un "pourquoi" sur un comportement observé — pas une hypothèse de solution. Exemples de référence :
- "Pourquoi certains patients ne se présentent-ils pas à leur rendez-vous alors qu'ils ne l'ont pas annulé ?" (Doctolib)
- "Pourquoi certains vendeurs abandonnent-ils la mise en ligne d'un article en cours de route ?" (Vinted)

Si `documents/jtbd-personas.md` a été trouvé, proposer un objectif dérivé de sa friction/motivation plutôt que de partir de zéro — mais toujours faire valider ou reformuler par l'apprenant avant de continuer. Ne jamais l'imposer.

Ne pas passer à l'étape 2 avant d'avoir un objectif validé.

## Étape 2 — Valider le persona ciblé

Proposer le persona à interviewer selon 3 axes :
- **Qui** — le rôle/segment (ex. "vendeurs Vinted actifs ou inactifs")
- **Comportement** — ce que cette personne a fait et qui la rend pertinente pour cette recherche (ex. "a abandonné la mise en ligne d'un article en cours de route")
- **Filtre** — un critère concret de recrutement, pas un critère flou (ex. "au moins une annonce non finalisée dans les 30 derniers jours")

Si `documents/jtbd-personas.md` existe, partir du persona qui y est décrit et l'adapter à l'objectif de l'étape 1, plutôt que d'en inventer un nouveau. Présenter la proposition et attendre la validation ou la correction de l'apprenant.

Ne pas passer à l'étape 3 avant d'avoir un persona confirmé.

## Étape 3 — Générer le script d'entretien

Rédiger un script de 4 questions ouvertes, dans cet ordre, adaptées à l'objectif et au persona validés :

1. **Contexte** — "Raconte-moi la dernière fois que tu as [situation liée au problème]." — ancre dans un fait vécu, jamais une hypothèse.
2. **Friction** — "Qu'est-ce qui a été le plus difficile ou frustrant dans ce moment-là ?" — laisse la personne nommer le problème avec ses propres mots.
3. **Comportement passé** — "Qu'est-ce que tu as déjà essayé pour résoudre ça ?" — ce que les gens ont fait compte plus que ce qu'ils disent qu'ils feraient.
4. **Conséquence** — "Qu'est-ce qui se passe si ce problème n'est jamais résolu ?" — révèle si c'est une vraie frustration ou un irritant supportable.

Pour chacune des 4 questions, ajouter une **relance "pourquoi"** — une seule, pas une cascade façon 5 Whys — à utiliser si la première réponse reste en surface (un fait sans le sens qu'il a pour la personne). Une relance "pourquoi" creuse la raison derrière ce qui vient d'être dit, sans jamais orienter vers une solution :
- **Contexte** → "Pourquoi ce jour-là précisément, et pas un autre ?" (ce qui rend cet épisode représentatif, pas juste anecdotique)
- **Friction** → "Pourquoi est-ce que ça t'a gêné à ce point-là, à ce moment précis ?" (le sens de la friction, pas juste sa description)
- **Comportement passé** → "Pourquoi as-tu choisi cette solution plutôt qu'une autre ?" ou "Pourquoi ça n'a pas suffi ?" (le raisonnement derrière le contournement, et pourquoi il a échoué)
- **Conséquence** → "Pourquoi est-ce que ça compte pour toi, concrètement ?" (l'enjeu réel derrière la conséquence citée)

Adapter ces relances à l'objectif et au persona validés — elles sont un point de départ, pas des formules figées à recopier telles quelles.

Remplacer les crochets par des formulations concrètes, dérivées de l'objectif et du persona validés — ne jamais laisser une question générique du type "[situation liée au problème]" telle quelle dans le script final.

Sauvegarder sous `documents/script-entretien.md` :

```
# Script d'entretien — [objectif de la recherche]

**Persona ciblé :** [qui] — [comportement] — [filtre]

## 1. Contexte
[question]
**Relance "pourquoi" :** [question]

## 2. Friction
[question]
**Relance "pourquoi" :** [question]

## 3. Comportement passé
[question]
**Relance "pourquoi" :** [question]

## 4. Conséquence
[question]
**Relance "pourquoi" :** [question]
```

`git add documents/script-entretien.md`, `git commit -m "Script d'entretien — [objectif]"`, puis `git push` sur la branche courante (`git branch --show-current`). Si le push échoue (pas de remote, réseau, authentification requise), le dire clairement et donner le chemin local. Si le push réussit et que `origin` est un remote GitHub (`git remote get-url origin`), construire et donner le lien direct — `https://github.com/<owner>/<repo>/blob/<branche>/documents/script-entretien.md`.

Présenter le script à l'apprenant et attendre sa validation (ou ses ajustements) avant de passer à l'étape 4 — c'est encore une étape de préparation, pas la fin de l'exercice.

## Étape 4 — Simuler l'entretien

Une fois le script validé, incarner un utilisateur fictif correspondant au persona de l'étape 2 et répondre aux 4 questions comme le ferait une vraie personne dans cette situation : réponses concrètes, avec des détails vécus — pas des réponses de manuel produit — y compris des hésitations ou des réponses qui ne vont pas dans le sens attendu si c'est plausible pour ce persona.

Appliquer, dans la simulation, les bonnes pratiques de modération enseignées :
- Ne jamais faire dire au persona ce qu'un produit devrait faire — rester sur le vécu, la friction, le comportement passé, la conséquence.
- Au plus une relance par question — utiliser la relance "pourquoi" prévue dans le script si la première réponse simulée reste en surface, sans transformer la simulation en interrogatoire.

Sauvegarder sous `documents/simulation-entretien.md` :

```
# Simulation d'entretien — [objectif de la recherche]

**Persona simulé :** [qui] — [comportement] — [filtre]

**Q1 (Contexte) :** [question]
**R :** [réponse simulée]
[**Relance "pourquoi" :** [question] / **R :** [réponse] — uniquement si la première réponse restait en surface]

**Q2 (Friction) :** [question]
**R :** [réponse simulée]
[**Relance "pourquoi" :** [question] / **R :** [réponse] — uniquement si la première réponse restait en surface]

**Q3 (Comportement passé) :** [question]
**R :** [réponse simulée]
[**Relance "pourquoi" :** [question] / **R :** [réponse] — uniquement si la première réponse restait en surface]

**Q4 (Conséquence) :** [question]
**R :** [réponse simulée]
[**Relance "pourquoi" :** [question] / **R :** [réponse] — uniquement si la première réponse restait en surface]

## Ce que révèle cette simulation
[2-3 phrases : la frustration semble-t-elle réelle ou supportable, qu'est-ce que ça confirme ou remet en cause par rapport à l'objectif de l'étape 1]
```

Même logique de sauvegarde que l'étape 3 — `git add`, `git commit -m "Simulation d'entretien — [objectif]"`, `git push`, puis donner le lien GitHub direct (ou le chemin local si le push échoue).

Terminer en rappelant explicitement que cette simulation est un entraînement, pas un substitut à un vrai entretien — encourager l'apprenant à mener au moins un entretien réel avant de considérer son JTBD ou son PRD comme validé.

## Principes

- **Progressif, jamais tout d'un coup.** Chaque étape s'arrête pour validation avant la suivante — objectif, puis persona, puis script, puis simulation.
- **S'appuyer sur `jtbd-personas.md` sans jamais l'imposer.** Toujours proposer, jamais décider à la place de l'apprenant.
- **La solution n'entre jamais dans les questions.** Un bon script reste sur le vécu, la friction, le comportement passé, la conséquence — jamais "que penses-tu de mon app ?".
- **Ce que les gens ont fait, pas ce qu'ils disent qu'ils feraient.** Le comportement passé prime sur l'intention déclarée, dans le script comme dans la simulation.
- **La simulation reste un entraînement.** Elle prépare à un entretien réel, elle ne le remplace jamais — le dire explicitement à la fin.
- **Documents poussés, pas juste écrits.** Commiter et pousser systématiquement chaque fichier créé, puis donner le lien direct.
