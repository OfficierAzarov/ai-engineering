---
name: architecture-diagram
description: "Génère docs/architecture.html, un schéma interactif de l'architecture d'un projet (blocs, appels, en place et prévu), et la règle qui le tient à jour."
disable-model-invocation: true
---

Ce skill produit `docs/architecture.html` : une page autonome dont le schéma SVG montre les **blocs du système** et la façon dont ils s'appellent, quel que soit le style d'architecture (couches, microservices, monolithe modulaire, pipeline…). Chaque bloc s'explique au clic : une explication technique, une explication fonctionnelle (« En pratique »), les blocs qu'il appelle ou qui l'appellent, et des liens qui ouvrent ses fichiers dans l'IDE. Les blocs existants sont en trait plein, les blocs prévus en pointillés.

La page est écrite dans la langue de la documentation du projet, avec le vocabulaire de son glossaire. Le gabarit [`template.html`](template.html) contient la mécanique (CSS, SVG, script) et une disposition en trois couches avec des textes à remplacer. Pour une architecture hexagonale, utilise le skill `hexagonal-architecture-diagram`, qui connaît ses règles.

## 1. Explorer

Lis le code et les documents, sans rien supposer. Fin de l'étape : chaque bloc a un rôle en une phrase, une technologie, au moins un fichier qui existe, et chaque appel entre blocs a une direction.

- **Blocs** : applications, services, modules, bibliothèques partagées, stockages, files de messages, services externes.
- **Appels** : qui appelle qui (imports, requêtes, événements) et pourquoi.
- **Frontières** : couches, sous-systèmes, environnements de déploiement. Elles deviennent des cadres (`ZONES`).
- **Documents existants** : README, ADR, schémas, carte d'architecture, glossaire.
- **Éléments prévus** : décisions, tickets et feuille de route. Chacun reçoit le statut `planned`.

## 2. Choisir le niveau de zoom

Un schéma montre **un seul niveau de zoom**, celui des conteneurs : les blocs que l'on déploie ou que l'on remplace séparément (le niveau « container » du modèle C4). Garde de 6 à 15 blocs. Au-delà, fais un schéma par sous-système, ou regroupe les blocs de détail dans un bloc parent. Fin de l'étape : la liste des blocs, leur catégorie et leurs appels tiennent dans cette limite.

## 3. Demander le thème

Demande : thème sombre ou thème clair ? Utilise l'outil de questions s'il existe, sinon pose la question dans le chat. Attends la réponse avant d'écrire.

## 4. Écrire la page

Copie `template.html` vers `docs/architecture.html`. Si ce fichier existe déjà, lis-le et mets-le à jour à la place, et demande avant de remplacer un schéma d'un autre genre. Fin de l'étape : plus aucun « À rédiger », « chemin/du/fichier » ni libellé générique (« Interface 1 », « Logique 2 »…) ne subsiste.

- **Titre** : remplace « du projet » par le nom du projet (balises `<title>` et `<h1>`).
- **Catégories** (`KINDS`) : renomme-les d'après le projet (`label`) et choisis la couleur par `slot` (1 cyan, 2 vert, 3 orange, 4 violet, 5 gris). Cinq catégories au plus, sinon la légende ne se lit plus.
- **Cadres** (`ZONES`) : un rectangle par frontière, avec son libellé. Un bloc est dans un cadre quand ses coordonnées y tiennent.
- **Blocs** (`NODES`) : `id`, `kind`, `status` (`built` ou `planned`), position et taille (`x`, `y`, `w`, `h`), `label` (nom, puis technologie), `text`, `practice`, `files`. Pose les blocs sur une grille : une rangée par couche, des écarts réguliers, et l'appelant au-dessus ou à gauche de l'appelé. Le `viewBox` du SVG englobe tous les cadres et tous les blocs.
- **Appels** (`LINKS`) : `from` (l'appelant), `to` (l'appelé), et si utile `label` (le verbe) et `dashed: true` (un appel prévu). La flèche se trace seule entre les bords des blocs. Si deux flèches se croisent ou passent sous un bloc, déplace les blocs.
- **Textes** : dans `text`, dis ce que fait le bloc et ce qu'il ne fait pas. Dans `practice`, donne un exemple concret du point de vue de l'utilisateur, qui montre pourquoi le bloc est là. Entoure les noms de code de `` ` ``. Un bloc prévu s'écrit au futur.
- **Fichiers** (`files`) : des chemins relatifs à la racine du projet, vers des fichiers qui existent (le point d'entrée, la configuration, le test). Un bloc prévu pointe vers l'ADR ou le ticket qui le décrit. Un chemin désigne un fichier, jamais un dossier, parce qu'un lien vers un dossier ouvre une nouvelle fenêtre de l'IDE.
- **Thème** : le gabarit est sombre. Pour le thème clair, remplace dans `:root` `color-scheme: dark` par `light` et les variables par celles du tableau.

| Variable    | Sombre (Monokai Soda)      | Clair                 |
| ----------- | -------------------------- | --------------------- |
| `--bg`      | `#1a1a1a`                  | `#fbfbf9`             |
| `--ink`     | `#f6f6ef`                  | `#1c1e21`             |
| `--muted`   | `#a3a398`                  | `#5b616b`             |
| `--line`    | `#8f908a`                  | `#aeb3ba`             |
| `--card`    | `#232323`                  | `#ffffff`             |
| `--card-bd` | `#3e3d32`                  | `#d5d8dd`             |
| `--c1-bd`   | `#66d9ef`                  | `#3b72c4`             |
| `--c1-bg`   | `#1f3438`                  | `#e3eefc`             |
| `--c2-bd`   | `#a6e22e`                  | `#2f855a`             |
| `--c2-bg`   | `#2b3a26`                  | `#def3e4`             |
| `--c3-bd`   | `#fd971f`                  | `#b7791f`             |
| `--c3-bg`   | `#3c2d1b`                  | `#fdf0d5`             |
| `--c4-bd`   | `#ae81ff`                  | `#6b46c1`             |
| `--c4-bg`   | `#30293c`                  | `#ece5fa`             |
| `--c5-bd`   | `#8f908a`                  | `#6b7280`             |
| `--c5-bg`   | `#2a2a28`                  | `#eceef1`             |
| `--accent`  | `#f92672`                  | `#c2255c`             |
| `--zone-bg` | `rgba(255, 255, 255, .03)` | `rgba(0, 0, 0, .025)` |
| `--zone-bd` | `#55554d`                  | `#c3c7cd`             |

Pour le thème clair, remplace aussi l'ombre de `.card` (`rgba(0, 0, 0, 0.5)`) par `rgba(0, 0, 0, 0.14)` et le commentaire de palette en tête du CSS.

## 5. Vérifier

Fin de l'étape : la page s'affiche dans un navigateur, la console est sans erreur, un clic sur chaque bloc affiche son texte, son « En pratique », ses voisins cliquables et ses liens, et chaque chemin de `files` existe (`test -e`). Sans navigateur, extrais le script et lance `node --check`. Puis lance le formateur du projet sur le fichier.

Les liens d'IDE s'appuient sur la racine du projet : ouverte en `file://`, la page la déduit de son adresse, sinon le champ « Racine du projet » la demande.

## 6. Tenir la page à jour

Ajoute dans le fichier d'instructions du projet (`CLAUDE.md` ou `AGENTS.md`) une règle de ce type, adaptée au projet :

> **Le schéma d'architecture suit l'architecture** : `docs/architecture.html` représente les blocs du système et leurs appels. Dès qu'on ajoute, déplace, renomme ou supprime un bloc, qu'un appel change, ou qu'une décision d'architecture est prise, on met à jour le schéma dans le même changement (`NODES`, `LINKS`, `ZONES`, statut `built` ou `planned`, textes, fichiers). Les textes de la page ne doivent pas contredire le code.

## 7. Commit

Respecte les règles du projet (TDD, format des messages de commit, etc.). Ne commite que la page et la règle ajoutée, sans les autres modifications en cours, et ne pousse rien.
