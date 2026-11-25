---
name: rediger-un-skill
description: Rédiger des documents pour les agents. À utiliser pour créer ou modifier un skill, ou pour modifier AGENTS.md ou CLAUDE.md.
---

Référence pour rédiger tout document qu'un agent lit : un skill, un `AGENTS.md` / `CLAUDE.md`, un document atteint par un pointeur. L'emballage change ; l'écriture, non : les mêmes leviers rendent chacun prévisible, car l'agent suit le même _processus_ à chaque passage, plutôt que de produire le même résultat.

Quand le document que vous rédigez est un skill, lisez [`SKILL-MECHANICS.md`](SKILL-MECHANICS.md) pour le frontmatter (l'en-tête du fichier), le choix du mode d'invocation et les skills aiguilleurs.

## Pointeurs de contexte

Un **pointeur de contexte** (_context pointer_) est une référence présente dans le contexte de l'agent qui nomme un contenu hors contexte et porte la condition pour aller le chercher. La description d'un skill en est un ; une ligne d'`AGENTS.md` qui nomme un document est le même objet. C'est la _formulation_ du pointeur, pas sa cible, qui décide quand l'agent va chercher le contenu, et avec quelle fiabilité. Une cible indispensable derrière un pointeur mal formulé est un bug de variance : affûtez d'abord la formulation, et n'intégrez le contenu directement que si cela échoue.

Un pointeur fait deux choses : dire ce qu'est le contenu, et lister les **branches** qui doivent déclencher la consultation (une branche est un cas distinct que traite le document, si bien que différents passages empruntent différents chemins à travers lui). Chaque mot d'un pointeur toujours chargé coûte à chaque tour, il mérite donc un élagage encore plus sévère que le corps :

- **Placez le mot directeur en tête** : c'est dans le pointeur qu'il fait son travail de déclenchement.
- **Un déclencheur par branche.** Des synonymes qui renomment une seule branche sont une branche écrite deux fois ; fusionnez-les et ne gardez que les branches réellement distinctes.
- **Retirez l'identité que le corps porte déjà.**

## Les deux charges

Chaque document et chaque pointeur que vous ajoutez dépense l'un de deux budgets :

- La **charge de contexte** (_context load_) est le coût, sur la fenêtre de l'agent, de ce qui est toujours chargé : une ligne d'`AGENTS.md`, une description de skill, tout ce qui reste dans le contexte à chaque tour, et dépense des tokens et de l'attention, qu'il serve ou non.
- La **charge cognitive** (_cognitive load_) est le coût pour l'humain : savoir quels documents existent et quand recourir à chacun. L'humain est l'index. Ce n'est pas un coût à réduire au minimum : c'est le prix de l'autonomie humaine ; dépensez-le là où le jugement humain compte, supprimez-le là où il ne compte pas.

Un contenu atteint seulement par un pointeur échappe à la charge de contexte au prix de la ligne du pointeur ; un contenu sans aucun pointeur repose entièrement sur la charge cognitive.

## Hiérarchie de l'information

Un document est fait de deux types de contenu : des **étapes** (les actions ordonnées que l'agent exécute) et de la **référence** (définitions, règles, faits consultés à la demande). Les deux se mélangent librement : uniquement des étapes (une recette), uniquement de la référence (les règles d'une relecture, ce skill), ou les deux. La décision centrale est la place de chaque élément sur la **hiérarchie de l'information** (_information hierarchy_), une échelle classée selon l'immédiateté avec laquelle l'agent a besoin du contenu :

1. **L'étape dans le fichier** est le premier niveau : ce que fait l'agent, dans l'ordre.
2. **La référence dans le fichier** se consulte à la demande. Souvent un ensemble de pairs légitimement à plat (chaque règle d'une relecture sur le même barreau), ce qui est une bonne organisation, pas un défaut.
3. **La référence déportée** est sortie dans un fichier séparé, atteinte par un pointeur de contexte, chargée seulement quand le pointeur se déclenche. Elle va du fichier voisin dans le même dossier jusqu'à la référence entièrement externe, qui vit n'importe où et que n'importe quel document peut désigner.

Descendez trop peu et le haut gonfle ; descendez trop et vous cachez un contenu dont l'agent a réellement besoin. Toute la décision tient dans cette tension.

La **divulgation progressive** (_progressive disclosure_) est le mouvement vers le bas de l'échelle (hors du fichier principal et derrière un pointeur) pour que le haut reste lisible. Ce n'est pas d'abord une économie de tokens : c'est la façon de protéger la hiérarchie. Les branches sont le test de divulgation le plus net : gardez dans le fichier ce dont toutes les branches ont besoin, et repoussez derrière un pointeur ce que seules certaines branches atteignent. Quand un document contient des étapes, une référence restée dans le fichier alors qu'elle devrait être déportée les enfouit et fait de l'attention qu'on leur porte un pile ou face : un levier de variance, pas seulement de lisibilité.

La **colocalisation** (_co-location_) est son complément à l'intérieur du fichier : là où l'échelle décide _à quelle profondeur_ se place un élément, la colocalisation décide _ce qui se trouve à côté_ une fois qu'il y est. Gardez la définition, les règles et les mises en garde d'un concept sous un seul titre plutôt qu'éparpillées, pour que la lecture d'une partie apporte ses voisines. Le test : le document doit se lire comme une documentation écrite pour l'agent. Un contenu regroupé se lit ainsi ; un contenu éparpillé, non. (À distinguer de la duplication : celle-ci répète un même sens à deux endroits ; l'éparpillement fragmente un même sens en de nombreux endroits.)

L'**étalement** (_sprawl_) est le mode de défaillance ici : un document tout simplement trop long, même quand chaque ligne est vivante et unique. L'attention s'amincit sur l'excédent, et chaque ligne de plus est une ligne de plus à garder pertinente. Le remède est l'échelle : déportez la référence derrière des pointeurs, et découpez par branche ou par séquence pour que chaque chemin ne porte que ce dont il a besoin.

## Étapes et critères d'achèvement

Chaque étape se termine sur un **critère d'achèvement** (_completion criterion_), la condition qui indique à l'agent que le travail est fait. Deux propriétés en font un levier :

- **La clarté** : l'agent sait-il distinguer fait de pas fait ? Une limite vague (« compréhension atteinte ») invite à l'**achèvement prématuré** (_premature completion_) : terminer l'étape avant qu'elle soit vraiment faite, l'attention glissant vers le fait _d'avoir fini_. Les étapes visibles encore à venir (les **étapes post-achèvement**) exercent l'attraction ; la clarté du critère est la résistance. Défendez-vous dans cet ordre : **affûtez d'abord la limite** (local et peu coûteux) ; seulement si elle est irréductiblement floue _et_ que vous observez la précipitation, cachez les étapes suivantes en découpant la séquence. Cacher ne fonctionne qu'à travers une vraie frontière de contexte (un passage de relais ou l'envoi à un sous-agent ; un appel en ligne laisse les étapes suivantes dans le contexte et n'efface rien).
- **L'exigence** : ce qu'il demande. « Chaque modèle modifié pris en compte » force un travail minutieux là où « produire une liste des changements » ne le fait pas. L'exigence pilote le **travail de fond** (_legwork_ : les recherches que l'agent mène au sein du travail, latentes dans la formulation plutôt qu'écrites comme une étape à part), et elle n'est pas liée aux étapes : « chaque règle appliquée » engage un corps de référence à plat tout comme « chaque étape faite » engage une séquence, et c'est ainsi qu'un document fait uniquement de référence porte malgré tout une barre d'exhaustivité.

Les critères les plus solides sont à la fois vérifiables et exhaustifs.

## Quand découper

Découper un document en deux dépense l'une des deux charges, ne découpez donc que si la coupe le mérite :

- **Par séquence** : découpez une suite d'étapes quand les étapes post-achèvement poussent l'agent à bâcler celle qu'il a devant lui. Les garder hors de vue génère plus de travail de fond sur la tâche en cours. Attention à l'inverse : fusionner des séquences expose chaque étape aux étapes qui la suivent, et invite à l'achèvement prématuré.
- **Par mode d'invocation**, propre aux skills : voir [`SKILL-MECHANICS.md`](SKILL-MECHANICS.md).

## Mots directeurs

Un **mot directeur** (_leading word_) est un concept compact, déjà présent dans le pré-entraînement du modèle, avec lequel l'agent pense pendant qu'il exécute le document (_leçon_, _brouillard de guerre_, _balles traçantes_). Répété comme un mot, jamais comme une phrase, il accumule une définition répartie et ancre toute une zone de comportement avec le moins de tokens possible, en mobilisant des acquis que le modèle possède déjà. Inventer le vôtre fonctionne si vous le définissez clairement, mais un mot inventé ne mobilise aucun acquis : vous payez en tokens de définition ce qu'un mot déjà appris donne gratuitement ; cherchez d'abord un mot existant.

Il ancre deux fois. Dans le corps, l'_exécution_ : l'agent adopte le même comportement chaque fois que le mot apparaît, et au milieu d'une référence à plat il concentre l'attention sur une catégorie de choses à chercher. Dans un pointeur, l'_invocation_ : quand le même mot vit dans vos prompts, vos documents et votre code, l'agent relie ce langage commun au contenu et l'atteint plus sûrement.

Traquez les occasions de reformuler avec des mots directeurs. Un trio détaillé à trois endroits, un pointeur qui dépense une phrase pour évoquer une seule idée : chacun est un passage qui ne demande qu'à se réduire à un seul mot :

- « rapide, déterministe, peu coûteuse » → _serrée_ (une boucle _serrée_).
- « une boucle en laquelle vous avez confiance » → _rouge_, qui transforme un critère flou en état binaire observable (la boucle passe au _rouge_ sur le bug, ou non).

Vous gagnez deux fois : moins de tokens, et une accroche plus nette à laquelle l'agent peut suspendre sa réflexion. Partez du principe que chaque document porte des reformulations que des mots directeurs mettraient à la retraite. Allez les chercher.

La **négation** (_negation_) est le mode de défaillance voisin de ce levier : piloter par l'interdiction fait entrer le comportement interdit dans le contexte et le rend _plus_ disponible, pas moins. _Ne pensez pas à un éléphant_, et il n'y a plus que l'éléphant ; la négation est un modificateur faible que le concept fortement activé écrase, si bien que l'interdiction se lit à moitié comme une consigne de faire la chose. Formulez le **positif** : énoncez le comportement visé (« écrivez des commentaires d'une ligne ») pour que le comportement interdit ne soit jamais prononcé. Une interdiction ne mérite sa place que comme garde-fou strict impossible à formuler positivement ; même alors, associez-la à la cible positive pour que l'attention se porte sur ce qu'il faut faire.

## Élagage

- Gardez chaque sens dans une **source unique de vérité** (_single source of truth_) : un seul endroit qui fait autorité, pour que changer le comportement se fasse en une seule modification. La **duplication** (le même sens à plus d'un endroit) coûte en maintenance et en tokens, et gonfle la place d'un sens sur l'échelle au-delà de son rang réel. (C'est l'inverse accidentel d'un mot directeur, qui répète un mot exprès, jamais le sens.)
- L'**environnement** est lui aussi une source de vérité (les scripts de `package.json`, les fichiers de configuration, l'organisation des dossiers, la sortie de `--help`), et un document qui le recopie est un **cache** : la copie d'une recherche, qui ne mérite sa charge que lorsque la recherche coûte cher. Mettez en cache ce que l'agent ne peut pas trouver en regardant : la convention non écrite, la raison d'un choix, le piège qu'aucune configuration n'avoue. Laissez à l'environnement les recherches d'un fichier ou d'une commande, là où elles ne peuvent pas devenir obsolètes.
- Vérifiez la **pertinence** de chaque ligne : concerne-t-elle encore ce que fait le document ? Une ligne perd sa pertinence en ne concernant jamais la tâche (simple exposé, ou branche qui devrait être déportée) ou en devenant obsolète quand le comportement ou le monde qu'elle décrit change. Les documents courts sont plus faciles à garder pertinents. Sans discipline d'élagage, le sort par défaut est la **sédimentation** (_sediment_) : des couches obsolètes qui se déposent parce qu'ajouter semble sans risque et retirer semble risqué, jusqu'à devoir carotter à travers elles pour trouver ce qui est encore vivant.
- Traquez les **no-ops** (instructions sans effet) phrase par phrase : une instruction à laquelle le modèle obéit déjà par défaut paie de la charge pour ne rien dire. Le test (change-t-elle le comportement par rapport au comportement par défaut ?) dépend du modèle, pas du lecteur : deux personnes en désaccord sur un no-op sont en désaccord sur le comportement par défaut, et elles tranchent en faisant tourner le document, pas en débattant. Quand une phrase échoue au test, supprimez la phrase entière plutôt que d'en retirer des mots. Le test juge aussi les mots directeurs : un mot trop faible pour battre le comportement par défaut (_soyez minutieux_ quand l'agent est déjà à peu près minutieux) est un no-op, et le remède est un mot plus fort (_implacable_), pas une autre technique.
