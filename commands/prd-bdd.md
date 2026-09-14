---
description: Crée dans Supabase la base de données décrite par le schéma de données d'une PRD.
argument-hint: [chemin de la PRD, optionnel — sinon cherchée automatiquement sous documents/PRD-*.md]
---

Utilise le skill `prd-bdd` pour cette requête. Elle cherche automatiquement la PRD sous `documents/PRD-*.md` du repo sauf si un chemin est fourni ci-dessous, vérifie les credentials Supabase dans `.env`, puis crée les tables, la RLS et un jeu de test sans jamais toucher au proto frontend.

Argument fourni (chemin de la PRD, peut être vide) : $ARGUMENTS
