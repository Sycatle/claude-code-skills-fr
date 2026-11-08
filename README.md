# Skills Claude Code en français

Quatre [skills Claude Code](https://code.claude.com/docs/en/skills) pour les fondateurs et indépendants qui construisent leur application sans être développeurs. Ils encadrent les moments qui comptent (cadrer, dessiner, vérifier, mettre en ligne) pour qu'ils se déroulent toujours de la même façon.

Ils viennent de la [formation Claude Code de A à Z](https://sycode.fr/formations/prendre-en-main-claude-code), où chacun est expliqué et utilisé sur une application construite de bout en bout.

## Les quatre skills

| Skill | Quand il se déclenche | Ce qu'il fait | Ce qu'il produit |
|---|---|---|---|
| [`cadrage-produit`](cadrage-produit/SKILL.md) | Tu arrives avec une idée floue, ou tu veux ajouter une grosse fonctionnalité | Te pose des questions une par une (problème, cible, alternative actuelle, ce qui rend payant, périmètre de la V1). N'écrit aucune ligne de code avant que tu aies dit « je valide » | `docs/cahier-des-charges.md` |
| [`direction-artistique`](direction-artistique/SKILL.md) | Avant toute interface, ou quand tu demandes « un joli design » | Exige un brief de marque, applique une [liste noire](direction-artistique/liste-noire.md) de ce qui fait « design généré par IA », pose un design system minimal, puis se relit sur captures d'écran desktop et mobile | `docs/direction-artistique.md` et une section « Style et design » dans `CLAUDE.md` |
| [`verifier-sans-coder`](verifier-sans-coder/SKILL.md) | À la fin de chaque fonctionnalité, avant un commit | Résume en français simple ce qui a changé, te donne le parcours à tester et 3 à 5 cas limites, lance lint, build et tests, puis propose un commit | Un compte rendu « ce qui marche / ce qui reste » |
| [`revue-avant-mise-en-ligne`](revue-avant-mise-en-ligne/SKILL.md) | Avant d'ouvrir au public, d'activer les paiements en live ou de déployer | Passe en revue sécurité, légal, performance, mobile, accessibilité et e-mails. Ne corrige rien sans ton accord | Un tableau « Bloquant / À faire / OK » |

Sur un projet neuf, ils s'enchaînent dans cet ordre : `cadrage-produit`, `direction-artistique`, `verifier-sans-coder` après chaque fonctionnalité, puis `revue-avant-mise-en-ligne`.

## Installer

Dans le dossier de ton projet :

```bash
git clone --depth 1 https://github.com/Sycatle/claude-code-skills-fr.git /tmp/claude-code-skills-fr
mkdir -p .claude/skills
cp -r /tmp/claude-code-skills-fr/{cadrage-produit,direction-artistique,verifier-sans-coder,revue-avant-mise-en-ligne} .claude/skills/
```

Pour les avoir dans tous tes projets, copie-les dans `~/.claude/skills/` plutôt que dans `.claude/skills/`.

Tu préfères que Claude Code s'en charge ? Ouvre-le dans ton projet et envoie-lui :

```text
Clone https://github.com/Sycatle/claude-code-skills-fr dans un dossier temporaire hors du projet. Lis les quatre SKILL.md et dis-moi en français tout ce qu'ils te demandent de faire sur mon ordinateur (fichiers lus ou écrits, commandes lancées), et si quoi que ce soit envoie des données à l'extérieur. Attends mon accord, puis copie les quatre dossiers dans .claude/skills/ sans écraser un dossier existant. Supprime le dossier temporaire et montre-moi l'arborescence de .claude/skills/.
```

Lis toujours un skill que tu n'as pas écrit avant de l'installer, celui-ci compris : il donne des instructions à un outil qui peut lire tes fichiers et lancer des commandes.

## Vérifier que ça marche

1. Relance `claude` si le dossier `.claude/skills/` n'existait pas au démarrage de ta session.
2. Tape `/` : les quatre noms apparaissent. Tu peux les lancer à la main, par exemple `/cadrage-produit une appli de réservation pour coiffeuses à domicile`.
3. Ils se déclenchent aussi seuls quand ta demande ressemble à leur description : « fais-moi une jolie page d'accueil » doit lancer `direction-artistique`, qui te pose ses questions avant d'écrire du code.

## Les adapter

- L'en-tête (entre les deux lignes `---`) doit rester au tout début du fichier.
- Pour changer une règle, par exemple tes propres interdits de design, modifie le `SKILL.md` ou `liste-noire.md`. Une règle qui doit s'appliquer à coup sûr se met plutôt dans une permission ou un hook.
- `revue-avant-mise-en-ligne` lit les checklists qu'il trouve dans `docs/` (`checklist-securite.md`, `checklist-legale.md`, `checklist-mise-en-ligne.md`, `checklist-passage-en-live.md`). Sans elles, il s'appuie sur sa propre liste. Les checklists complètes font partie du kit de la [formation](https://sycode.fr/formations/prendre-en-main-claude-code).

## Limites

Ces skills guident Claude Code, ils ne garantissent rien. Une revue de sécurité par un skill ne remplace pas un test qui échoue avant la correction et passe après, ni l'avis d'un juriste pour les textes légaux. Les captures d'écran automatiques demandent un MCP navigateur ; sans lui, les skills te demandent de les faire toi-même.

## Auteur

Charlie Dallier-Wood, développeur full stack. J'écris sur [sycode.fr](https://sycode.fr) ce que j'apprends en lançant des applications avec Claude Code. Les retours et corrections sont bienvenus en issue ou en PR.

Licence [MIT](LICENSE). Non affilié à Anthropic.
