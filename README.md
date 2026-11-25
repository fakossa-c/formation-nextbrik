# Formation NextBrik - skills

Des skills Claude Code pour les participants des formations NextBrik. Un skill est un dossier d'instructions que Claude lit au bon moment pour faire une tâche d'une façon précise.

## Contenu

| Skill | À quoi il sert |
|---|---|
| [`rediger-un-skill`](skills/rediger-un-skill/SKILL.md) | Rédiger un skill (ou un `CLAUDE.md`) que Claude suit de façon prévisible. Traduction française de [`writing-for-agents`](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-for-agents) de Matt Pocock. |
| [`claude-md`](skills/claude-md/SKILL.md) | Créer, compléter ou nettoyer un `CLAUDE.md`, le fichier d'instructions que Claude relit à chaque session. |

## Documents des séances

| Séance | Document | Contenu |
|---|---|---|
| 5 | [`seance-5/appel.md`](seance-5/appel.md) | Transcription d'un appel de vente fictif entre un commercial de Qonto et une boulangerie, pour le jeu de rôle. |
| 5 | [`seance-5/sources.md`](seance-5/sources.md) | Les pages publiques que lisent l'Avocat (Qonto) et le Veilleur (concurrents) pour vérifier les affirmations de l'appel. |

Pour les récupérer, dites à Claude, dans l'onglet Code :

> Récupère dans ce projet les fichiers seance-5/appel.md et seance-5/sources.md du dépôt GitHub fakossa-c/formation-nextbrik.

Ou ouvrez les liens ci-dessus et utilisez le bouton de téléchargement de GitHub.

## Installer un skill

### Choisir la portée : un projet ou partout

Un skill est comme une recette. Installé **dans le projet**, c'est la recette d'un seul restaurant : Claude ne la connaît que dans ce dossier. Installé **pour l'utilisateur**, c'est la recette que vous emportez dans tous vos restaurants : Claude la connaît dans tous vos projets.

| Portée | Dossier où arrive le skill |
|---|---|
| Projet | `.claude/skills/` dans le dossier du projet |
| Utilisateur | `~/.claude/skills/`, soit `C:\Users\<vous>\.claude\skills\` sous Windows |

### Le plus simple : demander à Claude

Dans Claude, onglet Code, collez la phrase du skill voulu. Claude lit le skill, vous dit ce qu'il fait, puis demande votre accord avant de l'installer.

| Skill | Phrase à dire à Claude |
|---|---|
| `rediger-un-skill` | Lis le skill rediger-un-skill du dépôt GitHub fakossa-c/formation-nextbrik, résume-moi ce qu'il fait, puis installe-le pour tous mes projets. |
| `claude-md` | Lis le skill claude-md du dépôt GitHub fakossa-c/formation-nextbrik, résume-moi ce qu'il fait, puis installe-le pour tous mes projets. |

Pour le seul projet ouvert, remplacez « pour tous mes projets » par « dans ce projet seulement ». Ouvrez ensuite une nouvelle session pour que Claude voie le skill.

### Avec le terminal : une commande par skill

Il faut [Node.js](https://nodejs.org). Chaque commande installe un seul skill dans le projet, à lancer depuis le dossier du projet. Ajoutez ` -g` à la fin pour l'installer pour l'utilisateur.

`rediger-un-skill` :

```bash
npx skills add fakossa-c/formation-nextbrik -s rediger-un-skill -a claude-code -y
```

`claude-md` :

```bash
npx skills add fakossa-c/formation-nextbrik -s claude-md -a claude-code -y
```

`-s` choisit le skill, `-a claude-code` l'agent, `-y` évite les questions. Pour voir la liste des skills sans rien installer : `npx skills add fakossa-c/formation-nextbrik -l`. En installation dans le projet, un fichier `skills-lock.json` note aussi la source à la racine du projet.

### Sans Node.js

Téléchargez le dépôt (bouton **Code**, puis **Download ZIP**) et copiez le dossier du skill voulu, par exemple `skills/claude-md`, dans `.claude/skills/` du projet ou dans `~/.claude/skills/`. Redémarrez ensuite Claude Code.

### Voir et désinstaller

`npx skills list` montre les skills installés et leur portée. Pour désinstaller : `npx skills remove claude-md` (ajoutez `-g` pour un skill installé pour l'utilisateur), ou supprimez son dossier.

## Prudence avec les skills trouvés sur le net

Un skill n'est pas un simple texte : il peut contenir des scripts que Claude exécutera sur votre ordinateur, et des instructions cachées (en fin de fichier, dans un fichier annexe) qui lui font envoyer vos données ailleurs ou modifier vos fichiers. Avant d'installer un skill récupéré au hasard sur internet, ouvrez chaque fichier de son dossier et lisez-le, ou demandez à Claude de vous l'expliquer sans l'installer. Préférez les sources que vous connaissez, comme ce dépôt.

## Licence et attribution

Le skill `rediger-un-skill` est une traduction de [`writing-for-agents`](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-for-agents), extrait du dépôt [mattpocock/skills](https://github.com/mattpocock/skills) (version du 25 novembre 2026, commit `6f7f783`), © 2026 Matt Pocock, publié sous licence MIT. La traduction est publiée sous la même licence : voir [`LICENSE`](LICENSE).

Le skill `claude-md` est une création NextBrik, publiée sous la même licence MIT.
