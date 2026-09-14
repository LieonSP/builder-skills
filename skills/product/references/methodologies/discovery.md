# Discovery

> La discovery n'est pas une phase — c'est une discipline continue.
> Sources : Marty Cagan (*Inspired*, *Empowered*, SVPG blog) + Giff Constable (*Talking to Humans*)

---

## Marty Cagan — Product Discovery

### Le principe central

La majorité des équipes livrent des features. Les meilleures équipes résolvent des problèmes.
> "The purpose of product discovery is to address four critical risks: value, usability, feasibility, and business viability."

La discovery existe pour répondre à une question avant de builder : **est-ce que ça vaut la peine d'être construit ?**

---

### Les 4 risques à adresser en discovery

Avant toute décision de builder, valider que :

| Risque | Question clé |
|--------|-------------|
| **Value** | Est-ce que les utilisateurs vont s'en servir / l'acheter ? |
| **Usability** | Est-ce qu'ils peuvent l'utiliser sans aide ? |
| **Feasibility** | Est-ce qu'on peut le construire avec nos moyens actuels ? |
| **Viability** | Est-ce que ça fonctionne pour le business (legal, finance, ventes...) ? |

La plupart des équipes ne testent que la faisabilité. C'est l'erreur la plus courante.

---

### Empowered teams vs Feature teams

Cagan fait une distinction fondamentale :

- **Feature team** : reçoit une liste de features à livrer. Fait de l'exécution, pas de la discovery.
- **Empowered team** : reçoit un problème à résoudre et un outcome à atteindre. Fait de la vraie discovery.

> "It doesn't matter how good your engineering team is if they are just building what you tell them."

La discovery n'est possible que si l'équipe a la responsabilité du résultat, pas de l'output.

---

### Les principes fondamentaux (Cagan)

**1. Discovery et delivery en parallèle**
La discovery ne précède pas le développement — elle se fait en continu, en parallèle. Pendant qu'une équipe livre, elle explore déjà ce qui vient après.

**2. Prototyper avant de coder**
Tester des idées avec le minimum d'effort possible : prototype papier, maquette, landing page. Ne jamais coder pour apprendre ce qu'un prototype peut révéler.

**3. L'équipe entière fait de la discovery**
PM, designer, et engineer ensemble — pas le PM seul qui "fait la research" et livre des specs. Les engineers en discovery voient ce qui est faisable, les designers voient l'usabilité en temps réel.

**4. Les users ne savent pas ce qu'ils veulent**
Mais ils savent ce qui les frustre. La discovery cherche les frictions, pas les wishlists.

---

## Giff Constable — Talking to Humans

### Le principe central

Avant d'investir dans quoi que ce soit, **parle à des humains**. Pas pour valider — pour apprendre.
> "Get out of the building before you waste time and money building the wrong thing."

Constable est plus radical que Cagan sur l'early-stage : la discovery doit commencer avant même d'avoir un produit.

---

### Les principes fondamentaux (Constable)

**1. Séparer les hypothèses des faits**
Avant chaque conversation, lister explicitement ce qu'on suppose être vrai. L'objectif de l'interview : tester ces hypothèses, pas les confirmer.

**2. Les 3 types d'hypothèses à valider**
- **Le problème existe-t-il vraiment ?** — est-ce que cette douleur est réelle et fréquente ?
- **Ce segment est-il le bon ?** — est-ce qu'on parle aux bonnes personnes ?
- **Notre solution adresse-t-elle ce problème ?** — seulement après les deux premières

**3. Recruter en dehors de son réseau**
Parler à ses amis et collègues génère des biais de complaisance. Le vrai signal vient d'inconnus qui n'ont aucune raison de te faire plaisir.

**4. Écouter ce que les gens font, pas ce qu'ils disent**
- ❌ "Achèteriez-vous ce produit ?"
- ✅ "Comment gérez-vous ce problème aujourd'hui ? Combien ça vous coûte ?"

**5. Le smoke test comme outil de discovery**
Avant de construire : créer une landing page, un faux bouton, une démo. Mesurer l'intention réelle plutôt que l'intention déclarée.

---

## Ce que Cagan et Constable partagent

- La discovery est une discipline, pas une étape
- Les utilisateurs révèlent les problèmes — l'équipe trouve les solutions
- Tester des hypothèses > collecter des opinions
- Apprendre vite et à moindre coût avant de committer des ressources

---

## Frameworks

- **Opportunity Solution Tree** (Torres) — structurer les opportunités issues de la discovery → voir `user_research.md`
- **4 risques de Cagan** — checklist avant de passer en delivery
- **Hypothèses explicites** (Constable) — lister avant chaque interview ce qu'on suppose

---

## Key questions

- Quel est le risque principal que je n'ai pas encore adressé (value / usability / feasibility / viability) ?
- Est-ce que je cherche à apprendre ou à confirmer ce que je crois déjà ?
- Ai-je parlé à des gens en dehors de mon réseau ?
- Quelle est la façon la moins chère de tester cette hypothèse ?

---

## Anti-patterns

- **Discovery theater** — faire des interviews pour cocher une case, sans changer ses décisions
- **Validation bias** — poser des questions orientées pour obtenir un "oui"
- **Over-discovery** — rester en discovery pour éviter de committer (procrastination déguisée)
- **PM solo** — faire la discovery sans les engineers et designers
- **Sauter aux solutions** — générer des idées avant d'avoir compris le problème
