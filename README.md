# builder-skills

Skills Claude Code utilisées dans le cadre de la formation **Product Builder**.

Ce repo contient les skills utilisées pendant la formation, et pour chacune la commande slash correspondante. Chaque skill vit dans son propre dossier sous `skills/` et suit le format standard Claude Code (`SKILL.md` + éventuels fichiers de référence) ; chaque commande est un fichier sous `commands/` qui invoque la skill du même nom.

```
skills/
  <nom-de-la-skill>/
    SKILL.md
    references/...
commands/
  <nom-de-la-skill>.md
```

## Pour les apprenants — importer skills et commandes dans votre repo de travail

Pour les utiliser, vous copiez **à la fois** le dossier de la skill dans `.claude/skills/` **et** le fichier de commande correspondant dans `.claude/commands/`, dans votre repo de travail (celui sur lequel vous travaillez pendant la formation) — Claude Code détecte alors automatiquement la skill et la commande dans ce repo. Sans la commande, la skill reste utilisable mais vous ne pourrez pas la lancer avec `/<nom-de-la-skill>`.

Dans tous les cas, exécutez les commandes depuis la racine de votre repo de travail.

### 1. Importer tout (skills + commandes)

```bash
git clone https://github.com/LieonSP/builder-skills.git /tmp/builder-skills
mkdir -p .claude/skills .claude/commands
cp -r /tmp/builder-skills/skills/* .claude/skills/
cp -r /tmp/builder-skills/commands/* .claude/commands/
rm -rf /tmp/builder-skills
```

Cette commande copie l'ensemble des skills et des commandes disponibles dans le repo.

### 2. Importer une skill en particulier (avec sa commande)

Remplacez `<nom-de-la-skill>` par le nom du dossier de la skill voulue (voir la liste ci-dessous) :

```bash
git clone https://github.com/LieonSP/builder-skills.git /tmp/builder-skills
mkdir -p .claude/skills .claude/commands
cp -r /tmp/builder-skills/skills/<nom-de-la-skill> .claude/skills/
cp /tmp/builder-skills/commands/<nom-de-la-skill>.md .claude/commands/
rm -rf /tmp/builder-skills
```

## Skills disponibles

- **idee-jtbd** (`/idee-jtbd`) — Transforme la description brute d'une idée d'application ou d'un problème en un Job-To-Be-Done (JTBD) clair, avec l'explication de pourquoi ce JTBD est le bon point de départ.
- **jtbd-prd** (`/jtbd-prd`) — Crée un Product Requirements Document (PRD) à partir d'un Job-To-Be-Done (JTBD) : questions de clarification, liste d'écrans priorisée, flux de navigation, proposition de schéma de base de données. Sauvegarde le résultat en markdown dans `documents/`.
- **prd-proto** (`/prd-proto`) — Guide un débutant, à partir d'une PRD, jusqu'à un premier écran frontend fonctionnel visible en local dans le navigateur (frontend uniquement, sans backend ni base de données).
- **prd-bdd** (`/prd-bdd`) — Crée dans Supabase la base de données décrite par le schéma de données d'une PRD (tables, RLS, jeu de test), sans jamais connecter le proto frontend existant à cette base.
- **audit-produit** (`/audit-produit`) — Audite une app vibe-codée sur trois axes (sécurité inspirée de l'OWASP Top 10:2025, accessibilité, éco-conception) et produit un compte rendu coloré (🟢/🟠/🔴) par catégorie, avec des corrections proposées mais jamais appliquées automatiquement.

## Pour le formateur — ajouter une skill

1. Ajouter le dossier sous `skills/<nom>/` avec un `SKILL.md` (et ses `references/` si besoin).
2. Ajouter la commande correspondante sous `commands/<nom>.md`.
3. Committer et pousser sur `main`.
