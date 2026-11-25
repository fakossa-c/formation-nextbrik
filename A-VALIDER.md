# Passages à valider

Le skill `rediger-un-skill` est une traduction fidèle de `writing-for-agents` de Matt Pocock. Certains passages s'appuient sur des exemples tirés du développement logiciel : ils sont traduits comme le reste, rien n'a été retiré. Pour chacun, une proposition : **garder**, **adapter** (remplacer l'exemple par un équivalent parlant pour un chef d'entreprise) ou **retirer**. À vous de trancher.

Les notions générales (pointeurs de contexte, charges, hiérarchie de l'information, critères d'achèvement, mots directeurs, élagage, modes d'invocation) valent pour tous les skills et ne sont pas listées.

| # | Fichier | Passage | Pourquoi c'est propre au développement | Proposition |
|---|---|---|---|---|
| 1 | `SKILL.md`, § Étapes et critères d'achèvement | « Chaque modèle modifié pris en compte » force un travail minutieux là où « produire une liste des changements » ne le fait pas. | Un « modèle » désigne ici une structure de données dans du code. Un non-développeur comprendra « modèle » au sens de gabarit. | **Adapter** : par exemple « Chaque client de la liste contacté » contre « faire une liste des relances ». |
| 2 | `SKILL.md`, § Mots directeurs, 1er paragraphe | Exemples _leçon_, _brouillard de guerre_, _balles traçantes_. | _Balles traçantes_ (_tracer bullets_) est une image connue des développeurs (livre _The Pragmatic Programmer_) : une première version minimale qui traverse tout le système. Inconnue du public. | **Adapter** : remplacer par une image parlante, par exemple _brouillon_ ou _check-list_. |
| 3 | `SKILL.md`, § Mots directeurs, 2e paragraphe | « quand le même mot vit dans vos prompts, vos documents et votre code » | « Votre code » suppose un projet logiciel. | **Adapter** : « vos prompts, vos documents et vos fichiers ». |
| 4 | `SKILL.md`, § Mots directeurs, 1er exemple | « rapide, déterministe, peu coûteuse » → _serrée_ (une boucle _serrée_). | Une « boucle » est ici une boucle de test logiciel ; « déterministe » et « boucle serrée » sont du jargon de développeur. | **Adapter** : trouver un exemple métier, par exemple « court, clair, sans jargon » → _sobre_. |
| 5 | `SKILL.md`, § Mots directeurs, 2e exemple | « une boucle en laquelle vous avez confiance » → _rouge_ (la boucle passe au _rouge_ sur le bug, ou non). | Renvoie aux tests automatisés qui « passent au rouge » quand ils détectent un bug. | **Adapter** : par exemple « un devis vraiment prêt à partir » → _signable_ (le devis est _signable_, ou non). |
| 6 | `SKILL.md`, § Mots directeurs, négation | Exemple de consigne positive : « écrivez des commentaires d'une ligne ». | Les commentaires sont les notes laissées dans du code. | **Adapter** : par exemple « répondez en trois phrases maximum ». |
| 7 | `SKILL.md`, § Élagage, 2e point | Exemples d'environnement : « les scripts de `package.json`, les fichiers de configuration, l'organisation des dossiers, la sortie de `--help` ». | `package.json` et `--help` sont des outils de développeur. L'idée (ne pas recopier ce que l'agent peut lire lui-même) vaut pour tous. | **Adapter** : « vos fichiers, un tableau de référence, l'organisation des dossiers, l'aide d'un outil ». |
| 8 | `agents/openai.yaml` | Fichier entier (nom et résumé affichés). | Lu seulement par Codex, l'agent d'OpenAI ; Claude Code l'ignore. Propre à un autre outil plutôt qu'au développement. | **Garder** : sans effet pour Claude, et il garde le skill fidèle à l'original. |

## Autres points (rediger-un-skill)

- `LICENSE` a été ajouté dans le dossier du skill, en plus de la racine du dépôt : la licence MIT exige que la mention de copyright accompagne chaque copie, et `npx skills add` ne copie que le dossier du skill.
- Le nom du fichier `SKILL-MECHANICS.md` est resté en anglais, comme dans l'original, pour garder les liens identiques. Il pourrait devenir `MECANIQUE-DES-SKILLS.md` (deux liens à mettre à jour dans `SKILL.md`).

## claude-md

Copie de votre skill personnel `claude-md`, déjà en français. Relu en entier : aucun secret, aucune adresse e-mail, aucun chemin propre à votre machine. Seule modification : les 30 tirets cadratins remplacés par des tirets simples.

| # | Endroit | Passage | Pourquoi le signaler | Proposition |
|---|---|---|---|---|
| 1 | Description, Étape 1, § Emplacement | `~/.claude/CLAUDE.md` et les règles de `~/.claude/rules/` | Ce ne sont pas des réglages à vous : ce sont les emplacements standard de Claude Code pour les instructions valables dans tous les projets. | **Garder**. |
| 2 | § Portée : une question à poser | « Cette règle doit-elle s'appliquer sur toutes tes machines et pour toute l'équipe ? » | « Toutes tes machines » vient de votre configuration (deux machines, config versionnée). Un participant travaille souvent seul sur un seul ordinateur. | **Adapter** : « Cette règle doit-elle valoir pour toute l'équipe, ou seulement pour vous ? » |
| 3 | Étape 2b | `https://claude.com/fr/blog/using-claude-md-files` | L'adresse fonctionne encore mais redirige vers `https://claude.com/fr/resources/articles/using-claude-md-files`. | **Adapter** : mettre la nouvelle adresse. |
| 4 | Description, Étape 2a, § Squelette | « conventions de code, commandes de build/test/lint », « `package.json`, `pyproject.toml`, `Makefile` », sections Commandes et Conventions de code du squelette | Vocabulaire et fichiers de développeur ; un projet de chef d'entreprise (documents, devis, courriels) n'en a pas. | **Garder** la structure, **adapter** les exemples : « vos dossiers, vos modèles de documents, vos habitudes d'équipe ». |
| 5 | § Gate ou convention (tableau) | Exemples « brancher depuis `develop` », « test rouge d'abord », « pas de `any` », « imports absolus », « props typées » | Tous tirés de la programmation. La notion gate / convention, elle, vaut pour tous. | **Adapter** : gate « faire valider le devis avant de l'envoyer », convention « vouvoyer dans les courriels clients ». |
| 6 | § Spécificité, § Pointeurs (exemples) | « Lance `npm test` avant de committer », « `src/api/handlers/` », `docs/architecture/rbac.md` | Exemples de développeur. | **Adapter** : « Relis le devis avec la grille `docs/grille-devis.md` avant de l'envoyer », « Les factures sont dans `Factures/2026/` ». |
| 7 | Dossier du skill | Licence | `claude-md` est votre création : sans fichier de licence dans son dossier, il tombe sous le `LICENSE` racine (MIT), qui permet à chacun de le réutiliser en citant la source. | **Garder** MIT, ou choisir une autre licence pour vos propres skills. |
