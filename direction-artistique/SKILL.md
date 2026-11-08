---
name: direction-artistique
description: À utiliser AVANT d'écrire ou de modifier toute interface (page, écran, composant, thème, landing, e-mail HTML) et chaque fois que l'utilisateur demande « un joli design », un « look moderne », une refonte visuelle ou un changement de couleurs/typographie. Impose un brief de marque (3 adjectifs, public, 3 références réelles, typographie, palette avec une couleur signature, un parti pris) enregistré dans docs/direction-artistique.md, applique une liste noire anti « design généré par IA », pose un design system minimal, puis oblige à une auto-revue avec captures desktop et mobile avant de rendre la main.
---

# Direction artistique

Une interface faite sans direction ressemble à toutes les autres : dégradé violet-bleu, trois cartes identiques, ombres partout. Ton travail est de l'éviter. Tu ne touches à aucun fichier d'interface tant que le brief n'existe pas et n'est pas validé.

## 0. Point de départ

1. Regarde si `docs/direction-artistique.md` existe.
   - Oui : lis-le en entier, lis `liste-noire.md` (dans le dossier de ce skill), applique-les. Si la demande contredit le brief, dis-le et demande qui a raison. Va directement à l'étape 3.
   - Non : étape 1.
2. Lis `CLAUDE.md`, `docs/cahier-des-charges.md` (s'ils existent) et `package.json` pour connaître le produit, le public et la pile (Tailwind, shadcn/ui, `next/font`).
3. Si l'utilisateur a déjà donné des références, des captures ou une palette dans cette conversation, pars de là, ne redemande pas.

## 1. Le brief de marque

Pose les questions par lots de 3 ou 4 au maximum. Pour chaque question, propose 2 ou 3 réponses possibles, ta recommandation en premier, avec une raison d'une phrase : l'utilisateur est un fondateur non technique, il choisit mieux entre des options qu'à partir d'une page blanche. S'il dit « décide pour moi », décide, présente ton choix, et attends son accord avant d'écrire le fichier.

Les six éléments à obtenir, tous obligatoires :

1. **Personnalité en 3 adjectifs.** Précis et un peu opposés valent mieux que génériques : « chaleureuse, posée, artisanale » plutôt que « moderne, simple, professionnel ». Refuse « moderne », « simple », « épuré », « intuitif » : ils ne décrivent rien.
2. **Public.** Qui regarde l'écran, sur quel appareil, dans quel état d'esprit (pressé, méfiant, détendu), avec quel niveau d'aisance. Un seul public principal.
3. **Trois références réelles.** Trois sites ou produits que l'utilisateur admire (pas forcément du même secteur), avec pour chacun ce qu'il aime précisément : la typographie, l'espace, le ton des textes, la densité, une interaction. N'en cite que dont tu es certain qu'ils existent. Si l'utilisateur n'en a pas, propose-en trois que tu connais bien et demande-lui de les ouvrir et de dire ce qui lui plaît ou non. Ne recopie jamais un site : tu extrais des principes (« beaucoup d'air, titres en serif, un seul accent »).
4. **Typographie.** Un couple titre / texte, non générique (voir plus bas). Donne le nom exact des deux polices, leurs graisses utilisées, et d'où elles viennent (par défaut `next/font/google`, vérifie que la police existe dans cette bibliothèque avant de la promettre).
5. **Palette restreinte.** Un fond, un texte principal, un texte secondaire, une bordure, une **couleur signature** (l'accent : boutons, liens, focus, la seule couleur vive), une couleur d'erreur. Chaque couleur en code hexadécimal. Calcule les contrastes avec un petit script plutôt qu'à l'œil : 4,5 pour 1 minimum pour le texte courant, 3 pour 1 pour les éléments d'interface (bordure de champ, icône porteuse de sens). Un couple en dessous : corrige la teinte.
6. **Un parti pris fort, assumé.** Une décision visible qui rend le produit reconnaissable et qu'on pourrait critiquer : « les créneaux horaires sont de grandes pilules, c'est la seule forme arrondie du site », « tout est en une colonne étroite comme une lettre », « pas une seule image, que de la typographie ». Un parti pris qui ne coûte rien (« design propre ») n'en est pas un. Écris aussi ce que le parti pris interdit.

Ajoute deux lignes qui complètent : le **ton des textes** (tutoiement ou vouvoiement, phrases courtes, verbes d'action, mots interdits) et les **interdits propres au projet**, en plus de la liste noire.

## 2. Enregistrer le brief

1. Crée `docs/direction-artistique.md` avec ce plan (garde-le sous une page et demie) :

```markdown
# Direction artistique : <produit>

Validée le <date>.

## Personnalité
<3 adjectifs> : <une phrase qui les relie>

## Public
<qui, appareil, état d'esprit>

## Références
1. <nom, adresse> : <ce qu'on en retient>
2. ...
3. ...

## Typographie
Titres : <police, graisses>. Texte : <police, graisses>. Base 16 px.
Pourquoi ce couple : <une phrase>

## Palette
| Rôle | Valeur | Contraste |
|---|---|---|
| Fond | #... | |
| Texte | #... | x,x sur fond |
| Texte secondaire | #... | x,x sur fond |
| Signature (accent) | #... | x,x sur fond |
| Erreur | #... | x,x sur fond |
| Bordure de champ | #... | x,x sur fond |
| Bordure décorative | #... | décorative |

## Parti pris
<la décision forte, et ce qu'elle interdit>

## Ton des textes
<tutoiement/vouvoiement, longueur, mots interdits>

## Design system
<échelle typo, espacements, rayons, états : voir étape 3>

## Interdits du projet
<en plus de la liste noire du skill>
```

2. Ajoute à `CLAUDE.md` du projet une section de 5 lignes maximum, sans recopier le brief :

```markdown
## Style et design
- Avant toute modification visuelle : lire `docs/direction-artistique.md`.
- Couleurs et polices uniquement via les variables du thème, jamais en dur.
- Composants shadcn/ui d'abord, ajoutés avec `npx shadcn@latest add <nom>`.
- Pas de dégradé violet/bleu, pas d'emoji en guise d'icône, pas de texte de remplissage.
```

   Si `CLAUDE.md` a déjà une section « Style et design », mets-la à jour au lieu d'en créer une deuxième.
3. Montre le brief à l'utilisateur et attends son « ok » avant l'étape 3.

## 3. Design system minimal

Écris-le dans la section « Design system » du brief, puis traduis-le dans `app/globals.css` (variables du thème shadcn/ui) et `app/layout.tsx` (polices via `next/font`, `lang="fr"`). Reste minimal : on ne crée aucun composant qui n'est pas demandé.

- **Échelle typographique.** Base 16 px, un rapport fixe (1,25 par exemple) : 14, 16, 20, 25, 31, 39, 49. Maximum 5 tailles utilisées. Interligne 1,5 pour le texte, 1,1 à 1,25 pour les grands titres. Largeur de ligne de texte limitée à environ 65 caractères. Une seule hiérarchie de titres (un `h1` par page).
- **Espacements.** Base 4 px : 4, 8, 12, 16, 24, 32, 48, 64, 96. Ils doivent créer une hiérarchie : proche = lié, loin = séparé. L'espace entre deux sections est nettement plus grand que l'espace à l'intérieur d'un groupe. Pas le même `p-4` et `gap-4` partout.
- **Rayons.** Deux valeurs au plus (par exemple 8 px pour champs et cartes, 16 px pour les grands blocs) et une seule exception, celle du parti pris s'il en prévoit une.
- **Ombres.** Aucune par défaut. Une bordure fine suffit. Une ombre sert à dire « ce bloc flotte au-dessus » (menu, boîte de dialogue), pas à décorer.
- **États de chaque élément interactif** : repos, survol (changement visible mais sobre), focus clavier (contour net, d'au moins 2 px, jamais supprimé sans remplaçant), actif (appuyé), désactivé (contraste réduit, curseur indiquant l'interdit, et une raison lisible à côté quand c'est utile), chargement, erreur (couleur d'erreur plus un texte, jamais la couleur seule).
- **Contraste AA.** 4,5 pour 1 pour le texte courant, 3 pour 1 pour le grand texte et les éléments d'interface. Vérifie aussi les états : un bouton désactivé et un texte sur image ne sont pas exemptés par habitude.
- **Zones tactiles.** Boutons et liens utilisables au doigt : hauteur d'environ 44 px sur mobile.
- **Mode sombre.** Seulement si le brief le demande. Sinon retire les styles du gabarit qui suivent la préférence du système, pour que le rendu soit celui que tu as décidé.

## 4. Fabriquer l'interface

1. Relis `liste-noire.md` avant d'écrire le premier composant. Elle vaut sauf si le brief écrit explicitement une exception (par exemple des pilules pour les créneaux horaires, quand c'est le parti pris).
2. Écris de vrais contenus : vrais prénoms, vraies durées, vrais prix, vrais textes du produit. Jamais de « Lorem ipsum » ni de « Votre texte ici ».
3. Utilise les composants shadcn/ui et les variables du thème. Une couleur en dur dans un composant est une erreur.
4. Une chose à la fois : un écran, puis sa revue, puis le suivant.

## 5. Auto-revue obligatoire

Ne rends jamais la main sur un écran sans avoir regardé le résultat. « Ça compile » n'est pas une revue.

1. **Capture desktop et mobile.**
   - Vérifie qu'un MCP navigateur est connecté (`/mcp` liste les serveurs). S'il y en a un : ouvre la page sur le serveur local (`http://localhost:3000` par défaut), prends une capture à environ 1440 px de large, puis une autre à environ 390 px (téléphone), pour chaque écran modifié. Ajoute l'état au survol et au focus clavier d'un élément interactif si l'outil le permet.
   - Si aucun MCP navigateur n'est disponible : dis-le, demande à l'utilisateur de lancer `npm run dev`, de prendre une capture en grand et une en largeur de téléphone, et de te les donner (glisser l'image dans la fenêtre ou indiquer son chemin). Ne prétends jamais avoir vu une page que tu n'as pas vue.
2. **Confronte au brief**, point par point, en écrivant la réponse : personnalité (les 3 adjectifs se lisent-ils ?), public (lisible sur mobile, pour cette personne ?), typographie (bons couples et bonnes tailles ?), palette (une seule couleur vive, la signature ?), parti pris (est-il visible au premier regard ?), ton des textes.
3. **Confronte à la liste noire** : relis `liste-noire.md` face aux captures, puis lance la recherche dans le code (rubrique « Détection dans le code » de ce fichier). Chaque occurrence trouvée est un défaut à corriger ou une exception à justifier.
4. **Corrige**, puis reprends les mêmes captures. Fais au plus deux boucles de correction. Si un écart demande une décision de l'utilisateur, pose la question au lieu de trancher.
5. **Compte rendu**, court, en français simple :
   - Ce qui respecte le brief (3 lignes).
   - Défauts trouvés et corrigés (liste).
   - Défauts restants ou exceptions assumées, avec la raison.
   - Ce que l'utilisateur doit regarder lui-même (par exemple le rendu sur son vrai téléphone).
6. Termine en proposant un commit, sans le faire sans accord : message clair du type « style : applique la direction artistique à la page publique ».

## Règles à ne jamais enfreindre

- Aucun fichier d'interface avant la validation du brief.
- Aucune police ajoutée, aucune couleur ajoutée hors brief sans le mettre à jour d'abord.
- Aucune nouvelle dépendance de design sans demander.
- Si le brief existe déjà et que l'utilisateur veut en changer, modifie le brief d'abord, puis le code.
