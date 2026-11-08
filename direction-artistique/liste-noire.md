# Liste noire anti « AI slop »

Ce que tout le monde obtient quand personne n'a décidé. Chaque ligne dit ce qui est interdit, pourquoi, et quoi faire à la place. Un élément de cette liste est autorisé seulement si `docs/direction-artistique.md` l'écrit comme choix assumé (exemple : « les créneaux sont des pilules » quand c'est le parti pris).

Sommaire : Couleurs, Typographie, Mise en page, Composants, Icônes et images, Textes, Contenus, Mouvement, Détection dans le code.

## Couleurs

- **Dégradés violet / bleu / indigo / rose** en fond de page, de bouton ou de titre. Pourquoi : c'est la signature par défaut des interfaces générées. À la place : aplats, la couleur signature du brief, un fond uni.
- **Texte en dégradé** (`bg-clip-text`) sur les titres. À la place : un titre en couleur pleine, la hiérarchie vient de la taille et de la graisse.
- **Halos, taches floues et « blobs »** colorés derrière le contenu. À la place : le vide.
- **Plus d'une couleur vive.** Une palette arc-en-ciel d'icônes et de pastilles. À la place : une couleur signature, des neutres, une couleur d'erreur.
- **Gris clair sur blanc pour le texte secondaire** sous 4,5 pour 1. À la place : un gris plus foncé, calculé.
- **Noir pur (#000) sur blanc pur (#fff)** quand le brief décrit une ambiance douce. À la place : les neutres teintés du brief.
- **Mode sombre « parce que ça fait pro »** non demandé par le brief.

## Typographie

- **Inter, Roboto ou Arial (ou la police par défaut du gabarit de départ) partout**, titres et texte dans la même police sans décision. À la place : le couple du brief.
- **Plus de deux familles de polices.**
- **Tout en gras, ou tout en capitales**, pour « faire des titres ». À la place : deux graisses au plus, des tailles qui contrastent vraiment.
- **Titres centrés sur de longs textes.** Le centrage est pour deux lignes, pas pour un paragraphe.
- **Texte gris de 12 ou 13 px** pour du contenu qui compte. Le texte courant fait 16 px au minimum.
- **Lignes de plus de 80 caractères**, texte justifié à la dernière ligne près.

## Mise en page

- **Hero centré + trois cartes de features identiques** (icône, titre, deux lignes), puis un bandeau d'appel à l'action. C'est la page la plus répandue du web généré. À la place : une mise en page née du contenu et du parti pris (colonne unique, gros plan sur le produit réel, liste, tableau, texte long).
- **Même espacement partout** (`p-4`, `gap-4`, `space-y-4` en boucle). À la place : une hiérarchie, proche = lié, grand vide = séparation.
- **Tout est dans une carte**, les cartes dans des cartes. À la place : des blocs séparés par l'espace et des filets.
- **Grilles de 3 ou 4 colonnes identiques** sur mobile ou desktop sans raison de contenu.
- **Sections qui alternent fond blanc / fond gris** mécaniquement.
- **Contenu collé au bord de l'écran sur mobile**, ou colonnes desktop simplement empilées.
- **Pied de page à quatre colonnes de liens vides** dans un produit d'une seule page.

## Composants

- **Ombres portées sur toutes les cartes et tous les boutons** (`shadow-md`, `shadow-lg`, `shadow-xl`). À la place : bordure fine ou rien.
- **Glassmorphism** (fond translucide + flou, `backdrop-blur`) hors superposition réellement utile. À la place : fond opaque.
- **Boutons arrondis en pilule partout.** À la place : un rayon du design system, une pilule seulement si c'est le parti pris, et jamais comme défaut.
- **Bouton primaire, secondaire et tertiaire tous de la même force**, ou deux boutons « primaires » côte à côte. Un écran a une action principale.
- **Bouton désactivé sans explication**, ou qui ne se distingue pas du bouton actif.
- **Focus supprimé** (`outline-none` sans remplaçant visible).
- **Survol qui fait « sauter » l'élément** (agrandissement, remontée, ombre qui grossit) sur tout ce qui est cliquable.
- **Badges « ✨ New », « Beta », « Pro »** posés pour faire vivant. À la place : rien, ou une vraie information.
- **Fausses barres de progression, compteurs animés, notifications fictives** qui ne reflètent pas de données.
- **Modales pour tout**, y compris ce qui devrait être une page ou un message en ligne.
- **Toasts qui disparaissent** pour une erreur que l'utilisateur doit lire.

## Icônes et images

- **Emojis en guise d'icônes, de puces ou de titres de section** (🚀 ✨ 💡 🔥 ✅). À la place : un vrai jeu d'icônes cohérent (un seul style, une seule épaisseur de trait) ou du texte.
- **Une icône dans un cercle coloré** devant chaque ligne ou chaque feature. À la place : l'icône seule ou pas d'icône.
- **Illustrations 3D génériques**, personnages « flat » de banque d'images, formes abstraites flottantes, mockups d'ordinateur vides. À la place : une capture réelle du produit, une vraie photo, ou rien.
- **Photos d'équipe ou de clients de banque d'images** présentées comme réelles.
- **Logos de « clients qui nous font confiance » inventés.**
- **Image de fond avec voile sombre** pour rendre un texte lisible. À la place : composition qui laisse le texte respirer.

## Textes

- **Vocabulaire creux** : « Unlock », « Seamless », « Elevate », « Supercharge », « Empower », « Leverage », « Next-gen », « Game-changer ». En français : « Révolutionnez », « Boostez », « Libérez le potentiel », « Passez au niveau supérieur », « Simplifiez votre quotidien », « La solution tout-en-un », « Découvrez », « Plongez dans », « Une expérience unique ».
- **« Bienvenue sur [Produit] »** en titre d'accueil. À la place : la promesse concrète, en une phrase, avec un verbe.
- **Slogans qui s'appliqueraient à n'importe quel produit.** Test : remplace le nom du produit par celui d'un concurrent ; si la phrase reste vraie, réécris-la.
- **Boutons « Cliquez ici », « En savoir plus », « Soumettre ».** À la place : le verbe de ce qui se passe (« Réserver ce créneau », « Envoyer la demande »).
- **Messages d'erreur techniques ou vagues** (« Une erreur est survenue »). À la place : ce qui s'est passé et quoi faire.
- **Points d'exclamation en série, tirets cadratins en série**, listes de trois adjectifs, « Pas seulement X, mais aussi Y ».
- **Mélange tu / vous** dans la même interface. Le brief tranche.
- **Anglais laissé dans une interface française** (« Sign in », « Submit », « Dashboard ») sauf nom propre.

## Contenus

- **Lorem ipsum**, « Votre texte ici », « Titre de la section », « Jean Dupont / john@example.com » partout. À la place : de vrais prénoms, durées, prix et textes du produit.
- **Faux témoignages**, fausses notes (« 4,9/5 par 2 000 utilisateurs »), faux chiffres (« +10 000 clients ») pour un produit qui n'a pas encore d'utilisateurs. À la place : rien, ou un vrai retour d'une vraie personne avec son accord.
- **Fausses statistiques, faux logos, faux « vu dans la presse ».**
- **Pages vides qui ne disent pas quoi faire** : un état vide a un texte qui explique et une action.

## Mouvement

- **Animation d'apparition sur chaque bloc au défilement** (fondu + glissement).
- **Éléments qui flottent, pulsent ou rebondissent en boucle.**
- **Transitions de plus de 300 ms** sur des actions courantes.
- Une animation sert à expliquer un changement d'état. Respecte la préférence « réduire les animations » du système.

## Détection dans le code

À lancer pendant l'auto-revue, avec l'outil de recherche, sur `app/` et `components/` (hors `components/ui/` pour les points 1 à 3, qui contient le code copié de shadcn/ui : lis-le mais ne le réécris pas sans raison). Chaque résultat est à examiner, pas forcément à supprimer.

1. Dégradés : `bg-gradient`, `from-`, `via-`, `to-`, `linear-gradient`, `bg-clip-text`.
2. Flou et verre : `backdrop-blur`, `blur-`.
3. Ombres : `shadow-md`, `shadow-lg`, `shadow-xl`, `shadow-2xl`, `box-shadow`.
4. Pilules partout : `rounded-full` (compter, comparer au brief).
5. Couleurs en dur : `#[0-9a-fA-F]{3,8}`, `rgb(`, `hsl(`, `bg-\[#`, `text-\[#`.
6. Couleurs Tailwind brutes contournant le thème : `purple-`, `violet-`, `indigo-`, `fuchsia-`, `pink-`.
7. Polices : `Inter`, `Roboto`, `Arial`, `font-sans` sans variable de police du brief, et les imports de `next/font`.
8. Focus supprimé : `outline-none`, `focus:outline-none` sans `focus-visible:` voisin.
9. Texte de remplissage : `lorem`, `ipsum`, `Votre texte`, `Titre de la section`, `example.com`, `John`, `Jane`.
10. Vocabulaire creux : les mots de la rubrique « Textes » ci-dessus (`Unlock`, `Seamless`, `Elevate`, `Révolutionn`, `Boost`, `Découvrez`, `Bienvenue sur`, `tout-en-un`).
11. Emojis dans les fichiers d'interface : recherche des caractères hors ASCII courants (par exemple l'étoile ✨, la fusée 🚀, la coche ✅) dans les fichiers `.tsx`.
12. Images : `<img` à la place de `next/image`, images sans texte alternatif (`alt`).
