# Mécanique des skills

La branche propre aux skills de [`rediger-un-skill`](SKILL.md) : ce qui change quand le document est un skill (le frontmatter, le choix du mode d'invocation et les skills aiguilleurs). Tout le reste de sa rédaction relève de la référence universelle de `SKILL.md`.

## Invocation

Deux choix, qui échangent une charge contre l'autre :

- Un skill **invoqué par le modèle** (_model-invoked_) garde une `description`, pour que l'agent puisse le déclencher de lui-même, et que d'autres skills puissent l'atteindre. Vous pouvez toujours taper son nom : l'invocation par le modèle _inclut_ toujours l'accès par l'utilisateur ; une description ne fait qu'ajouter la découverte par l'agent, elle ne retire jamais celle de l'humain. La description est le pointeur de contexte de plus haut niveau du skill, obligé de rester chargé en permanence : une charge de contexte permanente en échange de la découvrabilité. Un skill invoqué par le modèle dont le contenu est entièrement de la référence est aussi un lieu possible pour une référence partagée : un autre skill peut l'invoquer, donc une référence dont plusieurs skills ont besoin vit à un seul endroit. Mécanique : omettez `disable-model-invocation`, et écrivez une description destinée au modèle qui porte les branches de déclenchement (les règles d'écriture des pointeurs de `SKILL.md` s'appliquent entièrement).
- Un skill **invoqué par l'utilisateur** (_user-invoked_) retire la description de la portée de l'agent : seul l'humain qui tape son nom peut l'invoquer, et aucun autre skill ne le peut. Zéro charge de contexte, mais il dépense de la charge cognitive : c'est vous l'index qui doit se souvenir qu'il existe. Mécanique : mettez `disable-model-invocation: true` ; la `description` devient destinée à l'humain : un résumé d'une ligne, sans liste de déclencheurs.

Choisissez l'invocation par le modèle seulement quand l'agent doit atteindre le skill de lui-même, ou qu'un autre skill le doit. S'il ne se déclenche jamais qu'à la main, faites-en un skill invoqué par l'utilisateur et ne payez aucune charge de contexte.

Une référence partagée dont deux skills invoqués par l'utilisateur ont besoin ne peut vivre dans aucun des deux : sans description, aucun ne peut déclencher l'autre. Repoussez-la dans un simple fichier hors du système de skills : une référence externe que n'importe quel skill peut désigner.

## Découper par mode d'invocation

La coupe par invocation (la coupe par séquence se trouve dans `SKILL.md`) : détachez un skill invoqué par le modèle quand vous avez un mot directeur distinct qui doit le déclencher à lui seul (un mot déclencheur que vous employez réellement dans vos prompts), ou qu'un autre skill doit l'atteindre. Vous payez une charge de contexte pour la nouvelle description toujours chargée, cet accès indépendant doit donc en valoir la peine.

## Skills aiguilleurs

Quand les skills invoqués par l'utilisateur se multiplient au-delà de ce dont vous pouvez vous souvenir, cette charge cognitive accumulée se soigne par un **skill aiguilleur** (_router skill_) : un skill invoqué par l'utilisateur qui nomme les autres et dit quand recourir à chacun, pour que l'humain n'ait qu'un seul skill à retenir au lieu de plusieurs. Il ne peut que suggérer, jamais les déclencher : les skills invoqués par l'utilisateur n'ont pas de description, donc rien d'autre que l'humain ne peut les atteindre.
