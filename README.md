# ai-engineering

Machine and human readable engineering.

Des skills pour agents de code (Claude Code, Cursor, Codex…) qui rendent un projet lisible à la fois par les humains et par les agents.

## Skills

| Skill | Ce qu'il produit |
| --- | --- |
| `architecture-diagram` | `docs/architecture.html` : un schéma interactif des blocs du système et de leurs appels, quel que soit le style d'architecture. |
| `hexagonal-architecture-diagram` | `docs/architecture.html` : le schéma interactif d'une architecture hexagonale, avec ses règles de dépendance. |
| `harness-diagram` | `docs/harness.html` : un schéma interactif du harnais du projet (guides, hooks, CI, contrôles). |

Chaque skill génère aussi une règle à ajouter à `CLAUDE.md` ou `AGENTS.md`, pour que le schéma reste à jour quand le projet change.

Ces skills se lancent à la main (`disable-model-invocation`). Les pages produites sont rédigées dans la langue de la documentation du projet.

## Installation

### Avec le CLI `skills` (tous les agents)

```bash
npx skills@latest add OfficierAzarov/ai-engineering
```

Le CLI propose les skills et les agents à cocher. Pour un seul skill :

```bash
npx skills@latest add OfficierAzarov/ai-engineering --skill harness-diagram
```

### Avec Claude Code (plugin)

```
/plugin marketplace add OfficierAzarov/ai-engineering
/plugin install ai-engineering@ai-engineering
```

Les skills s'appellent alors `/ai-engineering:architecture-diagram`, `/ai-engineering:hexagonal-architecture-diagram` et `/ai-engineering:harness-diagram`. Pour mettre à jour : `/plugin marketplace update ai-engineering`.

### À la main

Copier le dossier d'un skill de `skills/` dans `.claude/skills/` (ou `~/.claude/skills/` pour tous vos projets).

## Utilisation

Dans le projet à documenter, lancer le skill voulu (par exemple `/architecture-diagram`). L'agent explore le code, remplit le gabarit `template.html` du skill et écrit la page dans `docs/`.

Les skills n'ont pas de dépendance : un glossaire, des ADR ou un fichier `CLAUDE.md` existants sont lus s'ils sont là, mais rien n'est obligatoire.

## Contribuer

`bin/add-skill <dossier-du-skill>` copie (ou met à jour) un skill d'un autre projet dans `skills/`, puis commite et pousse.

## Licence

MIT, voir [LICENSE](LICENSE).
