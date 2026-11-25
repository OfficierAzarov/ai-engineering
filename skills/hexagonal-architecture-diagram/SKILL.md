---
name: hexagonal-architecture-diagram
description: "Génère docs/architecture.html, le schéma interactif de l'architecture hexagonale d'un projet (en place et prévue), et la règle qui le tient à jour."
disable-model-invocation: true
---

Ce skill produit `docs/architecture.html` : une page autonome dont le schéma SVG montre l'**architecture hexagonale** du projet (domaine, application, ports, adaptateurs, racine de composition) et explique chaque élément au clic. Chaque élément porte une explication technique, une explication fonctionnelle (« En pratique »), et des liens qui ouvrent ses fichiers dans l'IDE. Les éléments existants sont en trait plein, les éléments prévus en pointillés.

La page est écrite dans la langue de la documentation du projet, avec le vocabulaire de son glossaire. Le gabarit [`template.html`](template.html) contient la mécanique (CSS, SVG, script) et une disposition complète avec des textes à remplacer. Pour une architecture qui n'est pas hexagonale, utilise le skill `architecture-diagram`.

## 1. Explorer

Lis le code et les documents, sans rien supposer. Fin de l'étape : chaque catégorie ci-dessous est relevée avec ses fichiers, ou notée absente.

- **Domaine** : les règles métier pures, sans dépendance extérieure.
- **Application** : les cas d'usage, et les **ports** qu'ils déclarent (interfaces vers l'extérieur).
- **Adaptateurs entrants** (_driving_) : ce qui déclenche l'application (interface, CLI, API, traduction).
- **Adaptateurs sortants** (_driven_) : ce qui implémente un port (stockage, horloge, réseau, fichiers).
- **Racine de composition** : le seul module qui branche les adaptateurs sur les ports.
- **Règles de dépendance** et leur contrôle (ADR, dependency-cruiser, ESLint, tests d'architecture).
- **Éléments prévus** : décisions (ADR), tickets, carte d'architecture, glossaire. Chacun reçoit le statut `planned`.

Chaque élément relevé reçoit le statut `built` ou `planned`, et au moins un fichier qui existe.

## 2. Demander le thème

Demande : thème sombre ou thème clair ? Utilise l'outil de questions s'il existe, sinon pose la question dans le chat. Attends la réponse avant d'écrire.

## 3. Écrire la page

Copie `template.html` vers `docs/architecture.html`. Si ce fichier existe déjà, lis-le et mets-le à jour à la place. Fin de l'étape : plus aucun « À rédiger », « chemin/du/fichier » ni libellé générique (« Port1 », « Concept 2 »…) ne subsiste.

- **Titre** : remplace « du projet » par le nom du projet (balises `<title>` et `<h1>`).
- **Disposition** : le gabarit montre 3 adaptateurs entrants, 2 cas d'usage, 3 concepts du domaine, 6 ports en face de 6 adaptateurs sortants, et la racine de composition. Supprime les nœuds sans équivalent dans le projet et ajoute-en si besoin. Chaque flèche du SVG relie des nœuds précis : retire ou déplace la flèche avec son nœud. Les ports (`portY(rangée)`) et les adaptateurs sortants (`adapterY(rangée)`) restent alignés rangée par rangée. Au-delà de 6 rangées, agrandis le `viewBox` et les polygones des hexagones. La flèche « injecté par la racine » entre deux adaptateurs entrants vient de l'exemple d'origine : garde-la seulement si le projet a un cas équivalent.
- **Catégories** (`kind`) : `domain`, `app` (cas d'usage et ports), `driving`, `driven`, `root`. Statut (`status`) : `built` ou `planned` (un nœud prévu s'écrit au futur dans son `text`).
- **Textes** : écris pour une personne qui connaît un peu l'architecture hexagonale sans être experte. Dans `text`, dis ce que fait l'élément et la contrainte de dépendance qui le concerne (qui l'importe, ce qu'il n'importe pas). Dans `practice`, donne un exemple concret du point de vue de l'utilisateur, qui montre pourquoi l'élément est là. Entoure les noms de code de `` ` ``.
- **Fichiers** (`files`) : des chemins relatifs à la racine du projet, vers des fichiers qui existent (le code et son test, l'ADR). Un élément prévu pointe vers l'ADR, le ticket ou la fiche de la carte d'architecture qui le décrit. Un chemin désigne un fichier, jamais un dossier, parce qu'un lien vers un dossier ouvre une nouvelle fenêtre de l'IDE.
- **Thème** : le gabarit est sombre. Pour le thème clair, remplace dans `:root` `color-scheme: dark` par `light` et les variables par celles du tableau.

| Variable        | Sombre (Monokai Soda)   | Clair                  |
| --------------- | ----------------------- | ---------------------- |
| `--bg`          | `#1a1a1a`               | `#fbfbf9`              |
| `--ink`         | `#f6f6ef`               | `#1c1e21`              |
| `--muted`       | `#a3a398`               | `#5b616b`              |
| `--line`        | `#8f908a`               | `#aeb3ba`              |
| `--card`        | `#232323`               | `#ffffff`              |
| `--card-bd`     | `#3e3d32`               | `#d5d8dd`              |
| `--domain-bd`   | `#66d9ef`               | `#3b72c4`              |
| `--domain-bg`   | `#1f3438`               | `#e3eefc`              |
| `--app-bd`      | `#a6e22e`               | `#2f855a`              |
| `--app-bg`      | `#2b3a26`               | `#def3e4`              |
| `--driving-bd`  | `#fd971f`               | `#b7791f`              |
| `--driving-bg`  | `#3c2d1b`               | `#fdf0d5`              |
| `--driven-bd`   | `#ae81ff`               | `#6b46c1`              |
| `--driven-bg`   | `#30293c`               | `#ece5fa`              |
| `--root-bd`     | `#8f908a`               | `#6b7280`              |
| `--root-bg`     | `#2a2a28`               | `#eceef1`              |
| `--accent`      | `#f92672`               | `#c2255c`              |
| `--zone-app`    | `rgba(166,226,46,.06)`  | `rgba(47,133,90,.07)`  |
| `--zone-domain` | `rgba(102,217,239,.08)` | `rgba(59,114,196,.08)` |

Pour le thème clair, remplace aussi l'ombre de `.card` (`rgba(0, 0, 0, 0.5)`) par `rgba(0, 0, 0, 0.14)` et le commentaire de palette en tête du CSS.

## 4. Vérifier

Fin de l'étape : la page s'affiche dans un navigateur, la console est sans erreur, un clic sur chaque nœud affiche son texte, son « En pratique » et ses liens, et chaque chemin de `files` existe (`test -e`). Sans navigateur, extrais le script et lance `node --check`. Puis lance le formateur du projet sur le fichier.

Les liens d'IDE s'appuient sur la racine du projet : ouverte en `file://`, la page la déduit de son adresse, sinon le champ « Racine du projet » la demande.

## 5. Tenir la page à jour

Ajoute dans le fichier d'instructions du projet (`CLAUDE.md` ou `AGENTS.md`) une règle de ce type, adaptée au projet :

> **Le schéma d'architecture suit l'architecture** : `docs/architecture.html` représente les couches, les ports et les adaptateurs. Dès qu'on ajoute, déplace ou supprime un de ces éléments, ou qu'une décision d'architecture change, on met à jour le schéma dans le même changement (nœuds `NODES`, statut `built` ou `planned`, textes, fichiers, flèches du SVG). Les textes de la page ne doivent pas contredire le code.

## 6. Commit

Respecte les règles du projet (TDD, format des messages de commit, etc.). Ne commite que la page et la règle ajoutée, sans les autres modifications en cours, et ne pousse rien.
