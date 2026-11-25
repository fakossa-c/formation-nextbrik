---
name: claude-md
description: Aide à créer, structurer, enrichir et améliorer un fichier CLAUDE.md, global (`~/.claude/CLAUDE.md`, règles de `~/.claude/rules/`) ou de projet, selon les recommandations officielles d'Anthropic - un fichier d'instructions persistantes concis, spécifique et bien structuré qui maximise l'adhérence de Claude. Déclencher dès que l'utilisateur veut créer, écrire, générer ou initialiser un CLAUDE.md ; AJOUTER une règle, une convention ou une consigne à un CLAUDE.md existant ("ajoute ça au CLAUDE.md", "note cette règle", "il faudrait que Claude sache que…") ; relire, auditer, nettoyer, raccourcir ou optimiser un CLAUDE.md existant ; vérifier sa conformité aux principes du blog Anthropic ("using-claude-md-files") ; ou dit "fais-moi un CLAUDE.md", "configure les instructions projet", "comment structurer mon CLAUDE.md", "mon CLAUDE.md est trop long", "optimise mes instructions Claude", "vérifie mon CLAUDE.md contre le blog", "audite mon CLAUDE.md". À invoquer aussi quand l'utilisateur veut ajouter des conventions de code, des commandes de build/test/lint, ou l'architecture du projet dans les instructions persistantes de Claude. Ce skill traite UNIQUEMENT de CLAUDE.md et des règles path-scoped associées.
---

# Skill : CLAUDE.md

Tu aides l'utilisateur à produire un fichier `CLAUDE.md` excellent : le fichier d'instructions persistantes que Claude charge à chaque session sur un projet. Un bon `CLAUDE.md` est **concis, spécifique, structuré, cohérent et sans redite** - c'est ce qui maximise l'adhérence de Claude.

**Périmètre :** ce skill traite de `CLAUDE.md` et des règles path-scoped (`.claude/rules/`) qui en dérivent. Ne pars pas sur `AGENTS.md` ni sur l'audit du système de mémoire automatique. Une seule question sur la mémoire est autorisée, et seulement pour trancher un placement (voir *Portée : une question à poser*).

---

## Principe directeur

`CLAUDE.md` est du **contexte**, pas de la configuration imposée. C'est un document vivant qui grandit à partir des frictions réelles du workflow, pas à partir de bonnes pratiques théoriques. Chaque ligne consomme du contexte à chaque session : tout ce qui ne sert pas le travail réel doit en sortir.

**Corollaire souvent oublié :** un fichier court n'est pas forcément un bon fichier. Un `CLAUDE.md` de 150 lignes dont trois règles vivent chacune à deux endroits, reliées par des renvois internes, est moins fiable qu'un fichier de 180 lignes bien rangé. La taille est un mauvais indicateur de santé - la **redite** et la **dispersion** en sont de bons.

---

## Workflow

### Étape 1 - Détecter la cible, puis le contexte

Identifie le fichier visé : le **global** (`~/.claude/CLAUDE.md`, ou une règle de `~/.claude/rules/`,
valables pour tous les projets) ou celui du **projet** (`./CLAUDE.md` puis `./.claude/CLAUDE.md`).
Quand la demande ne dit pas lequel, pose la question avant de lire quoi que ce soit. Puis, sur ce fichier :

- **Absent → mode création** (2a).
- **Présent + l'utilisateur apporte UNE règle à ajouter → mode ajout** (2c). C'est le mode le plus fréquent.
- **Présent + demande d'audit / nettoyage / optimisation → mode amélioration** (2b).

### Étape 2a - Mode création

1. Propose de lancer `/init` quand c'est pertinent : Claude analyse le codebase et génère un point de départ. C'est souvent le meilleur démarrage.
2. Sinon (ou ensuite), observe le projet toi-même : fichiers de config (`package.json`, `pyproject.toml`, `Makefile`…), structure des dossiers, scripts de build/test/lint, conventions visibles dans le code existant.
3. Pose à l'utilisateur **des questions ciblées uniquement sur ce que tu ne peux pas deviner** : conventions d'équipe, règles « toujours faire X », pièges connus, workflows propres à l'équipe. Une question à la fois si l'échange est interactif.
4. Rédige le fichier à partir du squelette ci-dessous, en ne gardant que les sections réellement utiles à ce projet.

### Étape 2b - Mode amélioration

**D'abord, rafraîchir la grille contre la source vivante.** Va relire l'article officiel
`https://claude.com/fr/blog/using-claude-md-files` (via WebFetch) et tires-en la liste à jour des
principes et anti-patterns. Audite contre CE que tu viens de lire, pas seulement contre la checklist
figée plus bas (l'article peut avoir évolué ; en cas de divergence, l'article fait foi). Si la page
est inaccessible, le signaler et se rabattre sur la checklist de relecture, sans inventer de principes.

Puis repère les problèmes concrets et propose des corrections. Pour un audit demandé explicitement
(« vérifie mon CLAUDE.md »), rends un verdict principe par principe (✅ respecté / ⚠️ à renforcer /
❌ violé), chaque constat ancré sur un passage du fichier. Signaux à traquer :

- **Redite** - la même règle énoncée à deux endroits. Ce n'est PAS une contradiction : les deux
  passages s'accordent, c'est justement pourquoi personne ne les repère. Traque-les en listant les
  sujets du fichier et en vérifiant qu'aucun n'apparaît deux fois.
- **Dispersion** - un même sujet éclaté sur plusieurs sections, recousu par des renvois internes
  (« voir *section X* »). Chaque renvoi interne est un aveu : les deux morceaux doivent fusionner.
- **Instructions vagues** → les rendre spécifiques et vérifiables (voir exemples plus bas).
- **Formulations négatives** → les retourner au positif (voir *Positif plutôt qu'interdit*).
- **Contradictions** → les éliminer (sinon Claude en choisit une au hasard).
- **Pointeurs mous** - un doc externe désigné sans condition de lecture (« détail complet dans
  `X.md` »). Ça dit ce qu'est le doc, jamais quand l'ouvrir : Claude ne l'ouvre jamais, ou l'ouvre
  toujours. Remplace par une **condition de déclenchement** ; au-delà de trois documents, un tableau
  à deux colonnes `Document | Quand le lire`.
- **Contenu générique** déconnecté de l'équipe → le supprimer.
- **Obsolète** → vérifier que les chemins, dossiers et commandes cités existent encore, puis nettoyer.
- **Procédure multi-étapes** → la déplacer vers un skill.
- **Trop long** (> 200 lignes) → déporter, mais uniquement ce que le test *gate ou convention ?* autorise.

Montre les changements proposés avant d'écrire, surtout si tu retires du contenu existant.

### Étape 2c - Mode ajout (une règle dans un fichier qui existe déjà)

C'est ici que le format se défait, une ligne à la fois. Quatre questions, dans l'ordre.

**Question 0 - Est-ce que ça mérite d'être écrit ?**
Ne consigne que ce que l'utilisateur a déjà dû dire deux fois : une erreur répétée, une correction
retapée, un point rattrapé en revue. Une règle écrite « au cas où » coûte du contexte à chaque
session pour ne rien changer. En cas de doute, propose d'attendre la deuxième occurrence.

**Question 1 - Est-ce déjà dit ailleurs dans le fichier ?**
**Relis le fichier ENTIER avant d'écrire quoi que ce soit.** C'est la seule façon de répondre, et
c'est non négociable : la duplication naît toujours d'un ajout fait sans relecture. Si la règle
existe déjà, même formulée autrement, tu fais une **modification à l'endroit existant** - jamais un
second passage ailleurs.

**Question 2 - Est-ce que ça doit être vrai AVANT de commencer ?**
Autorisation, ordre des étapes, choix de branche, gate qui décide si le travail peut démarrer.
→ C'est un **gate**. Il va **en haut** du `CLAUDE.md`, jamais ailleurs (voir *Gate ou convention*).

**Question 3 - Est-ce que ça s'applique pendant l'écriture d'un fichier précis ?**
Nommage, typage, imports, style, pattern local.
→ C'est une **convention**. Elle va **en bas**, une ligne, dans la section des conventions -
ou dans `.claude/rules/` si le fichier est déjà chargé.

**Question 4 - Est-ce que ça dépasse cinq lignes ?**
Alors ce n'est plus une règle, c'est un **document**. Écris-le dans `docs/`, et ne laisse dans le
`CLAUDE.md` qu'**une ligne avec sa condition de lecture**.

Conclus en indiquant à l'utilisateur où tu as rangé la règle et pourquoi.

### Étape 3 - Écrire et expliquer

Écris ou mets à jour le fichier au bon emplacement (voir « Emplacement »). Explique brièvement tes choix. Applique la **checklist de relecture** (plus bas) avant de conclure.

---

## Gate ou convention - le test avant tout déport

Avant de sortir quoi que ce soit du `CLAUDE.md` racine vers `.claude/rules/` ou vers un sous-dossier,
applique ce test. Il n'est pas optionnel : c'est le seul garde-fou contre une règle silencieusement
désactivée.

| | **Gate** | **Convention** |
|---|---|---|
| Répond à | « ai-je le droit de commencer, et dans quel ordre ? » | « comment j'écris cette ligne ? » |
| Exemples | mettre à jour la doc avant la migration, brancher depuis `develop`, ticket obligatoire, test rouge d'abord | pas de `any`, imports absolus, props typées, tests co-localisés |
| Emplacement | **`CLAUDE.md` racine, toujours** | `.claude/rules/` path-scopé, ou section conventions |

**Pourquoi c'est vital.** Une règle path-scopée ou un `CLAUDE.md` de sous-dossier ne se charge
**pas** au lancement : il entre en contexte quand Claude lit un fichier correspondant - donc *après*
que la décision est prise. Un gate déporté arrive systématiquement en retard, et rien ne le signale.

**Le piège de la compaction, encore plus discret.** Après un `/compact`, le `CLAUDE.md` racine est
relu depuis le disque et réinjecté. Les règles path-scopées et les `CLAUDE.md` de sous-dossiers, eux,
ne reviennent que si Claude relit un fichier concerné. Un gate déporté peut donc disparaître au
milieu d'une session longue.

**Règle simple :** dans le doute, ça reste à la racine. Une convention mal placée coûte quelques
tokens ; un gate mal placé casse le workflow.

---

## Emplacement et portée

| Cas | Fichier | Versionné ? |
|---|---|---|
| Instructions partagées par l'équipe | `./CLAUDE.md` ou `./.claude/CLAUDE.md` | Oui (git) |
| Préférences perso à ce projet | `./CLAUDE.local.md` | Non - ajouter au `.gitignore` |
| Préférences perso valables pour tous les projets | `~/.claude/CLAUDE.md` | - |
| Règle qui ne concerne qu'un type de fichier | `.claude/rules/<sujet>.md` + frontmatter `paths:` | Oui (git) |

Les fichiers des dossiers **parents** sont chargés en entier au lancement ; ceux des **sous-dossiers**
et les règles avec `paths:` se chargent à la demande, quand Claude lit un fichier correspondant.

### Portée : une question à poser

Quand le placement dépend de qui doit voir la règle, pose-la - une fois, sans dériver sur le sujet :

> Cette règle doit-elle s'appliquer sur toutes tes machines et pour toute l'équipe ?

Si oui, elle va dans un fichier **versionné** (`CLAUDE.md`, `.claude/rules/`). Une note qui ne vit que
sur une machine ne suivra pas l'utilisateur ailleurs.

---

## Quand ajouter une instruction

Ajoute au `CLAUDE.md` ce que tu devrais sinon ré-expliquer à chaque session. Signaux concrets :

- Claude refait la même erreur une 2e fois.
- Une revue de code rattrape quelque chose que Claude aurait dû savoir.
- On retape la même correction qu'à la session précédente.
- Ce dont un nouvel arrivant aurait besoin pour être productif.

Y mettre des **faits valables à chaque session** : commandes de build/test/lint, conventions de code, organisation du projet, règles « toujours faire X ».

---

## Ce qu'il ne faut PAS mettre

- **Procédure multi-étapes** → la déplacer vers un **skill**.
- **Règle qui ne concerne qu'une partie du codebase** → `.claude/rules/` path-scopé, **mais seulement
  si le test *Gate ou convention* le permet**. Un gate reste à la racine quelle que soit sa portée.
- **Aucune donnée sensible** : pas de clés API, secrets, identifiants, détails d'infrastructure confidentiels, ni de vulnérabilités. Traite le fichier comme de la **documentation publique partageable**.
- **Pas de contenu générique** déconnecté de la réalité de l'équipe ; pas de surcharge d'infos inutiles au développement réel.

---

## Rédaction efficace - le cœur du skill

### Taille
Viser **moins de 200 lignes**. Un fichier plus long consomme du contexte et réduit l'adhérence.
Attention : la taille est un indicateur grossier. Un fichier sous les 200 lignes peut être malade
(redites, dispersion) et un fichier bien rangé de 190 lignes peut être en parfaite santé. Traite
d'abord les redites : elles font perdre des lignes **et** de la fiabilité.

### Structure
Titres markdown + listes à puces pour grouper les instructions liées. **Pas de gros paragraphes denses.**

### Un sujet, un endroit
Chaque règle vit à **un seul endroit**. Si tu es tenté d'écrire « voir *section X* », c'est que les
deux morceaux doivent fusionner : un renvoi interne signale une dispersion, il ne la répare pas.
Range les règles d'un même sujet dans l'ordre où on les rencontre (créer → utiliser → nettoyer),
pas par catégorie abstraite.

### Spécificité
Des instructions **concrètes et vérifiables**. Compare :

| ❌ Vague | ✅ Spécifique |
|---|---|
| « formate le code correctement » | « Utilise une indentation de 2 espaces » |
| « teste tes changements » | « Lance `npm test` avant de committer » |
| « garde les fichiers organisés » | « Les handlers d'API sont dans `src/api/handlers/` » |

### Positif plutôt qu'interdit
Écris le comportement visé, pas l'interdit. Une interdiction dit quoi éviter sans jamais donner la
cible - elle laisse la décision floue, et mentionne le mauvais comportement au lieu du bon.

| ❌ Interdit | ✅ Comportement visé |
|---|---|
| « ne charge PAS les gros docs d'archi » | « ouvre un doc de référence quand sa condition se déclenche » |
| « n'utilise jamais de commentaires longs » | « un commentaire = une ligne » |

Garde une interdiction seulement quand elle ne peut pas s'écrire autrement (garde-fou dur), et
accompagne-la alors du comportement attendu.

### Pointeurs vers les documents externes
Un document désigné sans condition de lecture ne sera jamais ouvert au bon moment. Écris **quand** y
aller, pas seulement ce que c'est. Au-delà de trois documents, un tableau :

```markdown
| Document | Quand le lire |
|---|---|
| `docs/architecture/rbac.md` | Un changement dépend d'un rôle - helpers SQL, matrice actions × rôles |
| `docs/architecture/tech-stack.md` | Vérifier une version avant d'ajouter ou de monter une dépendance |
```

### Cohérence
Éliminer les règles contradictoires (sinon Claude en choisit une au hasard). Relire périodiquement pour retirer l'obsolète, et vérifier que les chemins cités existent toujours.

---

## Imports

- Importer d'autres fichiers avec `@chemin/vers/fichier` (chemins relatifs ou absolus, profondeur **max 4 niveaux**).
- Partager des préférences perso entre worktrees en important un fichier du home : `@~/.claude/mon-fichier.md`.
- Les imports **n'économisent PAS** de contexte : les fichiers importés sont chargés au lancement. Ils servent l'**organisation**, pas la réduction de tokens.
- Pour mentionner un chemin **sans** l'importer, l'entourer de backticks (`` `@README` ``).

---

## Commentaires destinés aux humains

Les commentaires HTML de bloc (`<!-- note de maintenance -->`) sont **retirés avant injection** dans le contexte de Claude. Utile pour laisser des notes aux mainteneurs humains sans consommer de tokens.

---

## Cycle de vie (bonnes pratiques)

- **Document vivant** : commencer simple, enrichir à partir des frictions réelles, pas de bonnes pratiques théoriques.
- **Démarrage** : `/init` génère un point de départ → relire, retirer le générique, ajouter les workflows propres à l'équipe, committer.
- **Ajout** : passer par le mode 2c. Toujours relire le fichier entier avant d'écrire une règle de plus.
- **Maintenance** : retirer les instructions périmées quand les pratiques changent. `/doctor` propose un dégraissage, surtout utile sur les fichiers qui décrivent l'arborescence ou les dépendances - peu de rendement sur un fichier fait de gates et de pièges.
- **Contexte ≠ configuration imposée** : pour une action qui doit **absolument** s'exécuter à un moment précis (ex. avant chaque commit), recommander un **hook** plutôt qu'une instruction.

---

## Squelette de CLAUDE.md réutilisable

**Réservé au mode création.** En mode amélioration ou ajout, respecte la structure existante du
fichier : ne réorganise pas un `CLAUDE.md` en bonne santé pour le faire entrer dans ce gabarit.

Adapte-le au projet : **supprime toute section sans contenu réel**. Ne garde pas de section vide « pour la forme ».

```markdown
# <Nom du projet>

## Aperçu
<2-3 lignes : ce que fait le projet, sa stack principale.>

## Commandes
- Build : `<commande>`
- Test : `<commande>`
- Lint : `<commande>`
- Dev / run local : `<commande>`

## Avant de commencer (gates)
- <Ce qui doit être vrai avant d'écrire du code : ticket, branche, doc à jour.>

## Conventions de code
- <Indentation, style, nommage - concret et vérifiable.>
- <Patterns à suivre, librairies imposées ou interdites.>

## Organisation / architecture
- <Où vit quoi : `src/api/handlers/`, `src/components/`, etc.>

## Workflows courants
- <Étapes répétées du quotidien, propres à l'équipe.>

## Pièges
- <Comportement attendu là où l'erreur est fréquente. Écrire la cible, pas l'interdit.>

## Documents de référence
| Document | Quand le lire |
|---|---|
| `<chemin>` | <condition de déclenchement> |

<!-- Note de maintenance : retirée du contexte de Claude. Pour les mainteneurs humains. -->
```

---

## Checklist de relecture - appliquer à CHAQUE génération

- [ ] **Chaque règle n'existe qu'à un seul endroit** ? (une redite n'est pas une contradiction - elle passe tous les autres tests)
- [ ] **Chaque sujet tient d'un bloc** ? (aucun renvoi interne « voir *section X* » : sinon, fusionner)
- [ ] **Aucun gate déporté** vers `.claude/rules/` ou un sous-dossier ? (test *Gate ou convention*)
- [ ] Instructions **spécifiques et vérifiables** (pas de « correctement », « proprement ») ?
- [ ] Règles écrites **au positif**, sauf garde-fou dur ?
- [ ] Chaque **document externe** cité avec sa **condition de lecture** ?
- [ ] **Aucune contradiction** entre les règles ?
- [ ] **Aucun secret** (clé API, identifiant, infra confidentielle, vulnérabilité) ?
- [ ] Structure en **titres + puces**, pas de gros paragraphes ?
- [ ] **Chemins et commandes cités vérifiés** comme existant encore ?
- [ ] **Procédures multi-étapes** déplacées vers des skills ?
- [ ] **Pas de contenu générique** déconnecté du projet réel ?
- [ ] **< 200 lignes** ? (après avoir traité les redites, pas à leur place)
- [ ] Fichier écrit au **bon emplacement** selon la portée voulue ?
