# builder-skills

Skills Claude Code utilisées dans le cadre de la formation **Product Builder**.

Ce repo est le point central où sont publiées les skills au fur et à mesure de la formation. Chaque skill vit dans son propre dossier sous `skills/` et suit le format standard Claude Code (`SKILL.md` + éventuels fichiers de référence).

```
skills/
  <nom-de-la-skill>/
    SKILL.md
    references/...
```

## Pour les apprenants — importer une skill

Les skills sont publiées progressivement pendant la formation. Pour en récupérer une :

1. Cloner (une seule fois) ou mettre à jour le repo :

```bash
git clone https://github.com/LieonSP/builder-skills.git ~/builder-skills
```

Si vous l'avez déjà cloné, récupérez simplement les dernières skills publiées :

```bash
cd ~/builder-skills && git pull
```

2. Copier le dossier de la skill voulue dans votre installation Claude Code :

- **Au niveau d'un projet** (recommandé pendant la formation, la skill n'est active que dans ce projet) :

```bash
mkdir -p .claude/skills
cp -r ~/builder-skills/skills/<nom-de-la-skill> .claude/skills/
```

- **Au niveau personnel** (disponible dans tous vos projets) :

```bash
mkdir -p ~/.claude/skills
cp -r ~/builder-skills/skills/<nom-de-la-skill> ~/.claude/skills/
```

Une fois copiée, la skill est automatiquement détectée par Claude Code.

## Skills disponibles

- **product** — Partenaire de réflexion PM : frameworks (JTBD, RICE, OKRs, roadmap, stratégie, discovery, analyse concurrentielle...), principes opérationnels et modes de session (Reflect, Learn, Apply, Simulate, Record, Build, Review). Déclenchement : taper `pp` en début de message, ou automatiquement sur une requête à caractère produit (PRD, roadmap, discovery, recherche utilisateur, OKRs, priorisation...).

## Pour le formateur — publier une nouvelle skill

1. Ajouter le dossier sous `skills/<nom>/` avec un `SKILL.md` (et ses `references/` si besoin).
2. Committer et pousser sur `main`.
3. Annoncer la nouvelle skill aux apprenants pour qu'ils fassent `git pull` puis la copient comme ci-dessus.
