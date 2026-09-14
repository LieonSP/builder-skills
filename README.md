# builder-skills

Skills Claude Code utilisées dans le cadre de la formation **Product Builder**.

Ce repo contient les skills utilisées pendant la formation. Chaque skill vit dans son propre dossier sous `skills/` et suit le format standard Claude Code (`SKILL.md` + éventuels fichiers de référence).

```
skills/
  <nom-de-la-skill>/
    SKILL.md
    references/...
```

## Pour les apprenants — importer les skills dans votre repo de travail

Pour les utiliser, vous les copiez dans le dossier `.claude/skills/` **de votre repo de travail** (celui sur lequel vous travaillez pendant la formation) — Claude Code les détecte alors automatiquement dans ce repo.

Dans tous les cas, exécutez les commandes depuis la racine de votre repo de travail.

### 1. Importer toutes les skills

```bash
git clone https://github.com/LieonSP/builder-skills.git /tmp/builder-skills
mkdir -p .claude/skills
cp -r /tmp/builder-skills/skills/* .claude/skills/
rm -rf /tmp/builder-skills
```

Cette commande copie l'ensemble des skills disponibles dans le repo.

### 2. Importer une skill en particulier

Remplacez `<nom-de-la-skill>` par le nom du dossier de la skill voulue (voir la liste ci-dessous) :

```bash
git clone https://github.com/LieonSP/builder-skills.git /tmp/builder-skills
mkdir -p .claude/skills
cp -r /tmp/builder-skills/skills/<nom-de-la-skill> .claude/skills/
rm -rf /tmp/builder-skills
```

## Skills disponibles

_Aucune skill publiée pour le moment._

## Pour le formateur — ajouter une skill

1. Ajouter le dossier sous `skills/<nom>/` avec un `SKILL.md` (et ses `references/` si besoin).
2. Committer et pousser sur `main`.
