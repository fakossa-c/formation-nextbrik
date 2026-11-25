# Formation NextBrik - skills

Des skills Claude Code pour les participants des formations NextBrik. Un skill est un dossier d'instructions que Claude lit au bon moment pour faire une tâche d'une façon précise.

## Contenu

| Skill | À quoi il sert |
|---|---|
| [`rediger-un-skill`](skills/rediger-un-skill/SKILL.md) | Rédiger un skill (ou un `CLAUDE.md`) que Claude suit de façon prévisible. Traduction française de [`writing-for-agents`](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-for-agents) de Matt Pocock. |

## Installer un skill

Avec la commande `skills` (il faut [Node.js](https://nodejs.org)) :

```bash
npx skills add fakossa-c/formation-nextbrik
```

La commande demande quel skill installer, pour quel agent (choisir Claude Code) et où : dans le projet courant ou pour tous vos projets (option `-g`). Pour tout installer sans question, pour tous vos projets :

```bash
npx skills add fakossa-c/formation-nextbrik -g -a claude-code -y
```

Sans Node.js : téléchargez le dépôt (bouton **Code**, puis **Download ZIP**) et copiez le dossier `skills/rediger-un-skill` dans `~/.claude/skills/` (sous Windows : `C:\Users\<vous>\.claude\skills\`). Redémarrez ensuite Claude Code.

Pour désinstaller : `npx skills remove rediger-un-skill` (ajoutez `-g` s'il a été installé pour tous vos projets), ou supprimez le dossier.

## Prudence avec les skills trouvés sur le net

Un skill n'est pas un simple texte : il peut contenir des scripts que Claude exécutera sur votre ordinateur, et des instructions cachées (en fin de fichier, dans un fichier annexe) qui lui font envoyer vos données ailleurs ou modifier vos fichiers. Avant d'installer un skill récupéré au hasard sur internet, ouvrez chaque fichier de son dossier et lisez-le, ou demandez à Claude de vous l'expliquer sans l'installer. Préférez les sources que vous connaissez, comme ce dépôt.

## Licence et attribution

Le skill `rediger-un-skill` est une traduction de [`writing-for-agents`](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-for-agents), extrait du dépôt [mattpocock/skills](https://github.com/mattpocock/skills) (version du 25 novembre 2026, commit `6f7f783`), © 2026 Matt Pocock, publié sous licence MIT. La traduction est publiée sous la même licence : voir [`LICENSE`](LICENSE).
