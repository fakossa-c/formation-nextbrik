# Formation NextBrik - skills

Des skills Claude Code pour les participants des formations NextBrik. Un skill est un dossier d'instructions que Claude lit au bon moment pour faire une tâche d'une façon précise.

## Contenu

| Skill | À quoi il sert |
|---|---|
| [`rediger-un-skill`](skills/rediger-un-skill/SKILL.md) | Rédiger un skill (ou un `CLAUDE.md`) que Claude suit de façon prévisible. Traduction française de [`writing-for-agents`](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-for-agents) de Matt Pocock. |
| [`claude-md`](skills/claude-md/SKILL.md) | Créer, compléter ou nettoyer un `CLAUDE.md`, le fichier d'instructions que Claude relit à chaque session. |

## Installer un skill

Avec la commande `skills` (il faut [Node.js](https://nodejs.org)). Sans option, elle demande quel skill installer, pour quel agent (choisissez Claude Code) et où l'installer.

### Choisir la portée : un projet ou partout

Un skill est comme une recette. Installé **dans le projet**, c'est la recette d'un seul restaurant : Claude ne la connaît que dans ce dossier. Installé **pour l'utilisateur**, c'est la recette que vous emportez dans tous vos restaurants : Claude la connaît dans tous vos projets.

| Portée | Commande | Dossier où arrive le skill |
|---|---|---|
| Projet (par défaut) | `npx skills add fakossa-c/formation-nextbrik`, lancée depuis le dossier du projet | `.claude/skills/` dans ce dossier (un fichier `skills-lock.json` y note aussi la source) |
| Utilisateur | la même commande avec `-g` | `~/.claude/skills/`, soit `C:\Users\<vous>\.claude\skills\` sous Windows |

### Installer un seul skill

`-s` choisit le skill, `-a claude-code` l'agent, `-y` évite les questions. Par exemple, `claude-md` dans le projet courant :

```bash
npx skills add fakossa-c/formation-nextbrik -s claude-md -a claude-code -y
```

Le même, pour tous vos projets :

```bash
npx skills add fakossa-c/formation-nextbrik -s claude-md -a claude-code -g -y
```

Pour tous les skills du dépôt, remplacez `-s claude-md` par `-s '*'`. Pour voir la liste sans rien installer : `npx skills add fakossa-c/formation-nextbrik -l`.

### Sans Node.js

Téléchargez le dépôt (bouton **Code**, puis **Download ZIP**) et copiez le dossier du skill voulu, par exemple `skills/claude-md`, dans `.claude/skills/` du projet ou dans `~/.claude/skills/`. Redémarrez ensuite Claude Code.

### Voir et désinstaller

`npx skills list` montre les skills installés et leur portée. Pour désinstaller : `npx skills remove claude-md` (ajoutez `-g` pour un skill installé pour l'utilisateur), ou supprimez son dossier.

## Prudence avec les skills trouvés sur le net

Un skill n'est pas un simple texte : il peut contenir des scripts que Claude exécutera sur votre ordinateur, et des instructions cachées (en fin de fichier, dans un fichier annexe) qui lui font envoyer vos données ailleurs ou modifier vos fichiers. Avant d'installer un skill récupéré au hasard sur internet, ouvrez chaque fichier de son dossier et lisez-le, ou demandez à Claude de vous l'expliquer sans l'installer. Préférez les sources que vous connaissez, comme ce dépôt.

## Licence et attribution

Le skill `rediger-un-skill` est une traduction de [`writing-for-agents`](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-for-agents), extrait du dépôt [mattpocock/skills](https://github.com/mattpocock/skills) (version du 25 novembre 2026, commit `6f7f783`), © 2026 Matt Pocock, publié sous licence MIT. La traduction est publiée sous la même licence : voir [`LICENSE`](LICENSE).

Le skill `claude-md` est une création NextBrik, publiée sous la même licence MIT.
