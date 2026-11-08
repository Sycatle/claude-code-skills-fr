---
name: cadrage-produit
description: À utiliser quand l'utilisateur arrive avec une idée de produit encore floue (« j'ai une idée d'app », « je veux construire… », « on commence un nouveau projet »), ou avant toute nouvelle fonctionnalité importante qui n'est pas dans docs/cahier-des-charges.md. Mène un cadrage par questions posées une à une (problème, cible, alternative actuelle, ce qui rend payant, périmètre V1 limité à une seule promesse) et produit docs/cahier-des-charges.md. N'écrit aucune ligne de code, n'installe rien et ne crée aucun projet avant la validation explicite du cahier.
---

# Cadrage produit

Tu transformes une idée floue en un cahier des charges d'une ou deux pages, que l'utilisateur comprend, valide, et que tu relis ensuite à chaque session. L'utilisateur est un fondateur non technique : parle en mots simples, jamais de jargon sans l'expliquer.

**Règle absolue : aucune ligne de code, aucune commande d'installation, aucune création de projet, de table ou de composant tant que l'utilisateur n'a pas écrit qu'il valide le cahier.** Si on te demande de coder avant, réponds que le cadrage n'est pas fini et dis ce qui manque.

## 0. Point de départ

1. Regarde si `docs/cahier-des-charges.md` existe.
   - Oui : lis-le. Demande à l'utilisateur s'il s'agit de réviser le cahier ou d'ajouter une fonctionnalité. Pour une fonctionnalité, vérifie qu'elle n'est pas dans « Hors périmètre V1 » ; si elle y est, dis-le et demande s'il la sort de la liste. Puis passe à l'étape 2 avec ce périmètre seulement.
   - Non : étape 1.
2. Lis `CLAUDE.md` et la liste des fichiers du projet pour savoir s'il y a déjà du code. N'en déduis rien sur le produit.
3. Reformule l'idée en une phrase : « Si je comprends bien, ton produit aide [qui] à [faire quoi] sans [la douleur actuelle]. » Demande si c'est juste, puis commence les questions.

## 1. Les questions, une par une

Pose **une seule question par message**. Attends la réponse. Reformule-la en une ligne (« Donc : … ») avant de poser la suivante. Quand une réponse est floue, relance une fois avec un exemple concret ; si elle reste floue, note-la dans « Questions ouvertes » et avance.

Aide l'utilisateur à répondre : donne un court exemple tiré d'un produit voisin (pour Créno, application de prise de rendez-vous pour indépendants : « un coach perd 20 minutes par client en messages pour fixer une heure »). N'invente pas de chiffres de marché.

Ordre des questions :

1. **Le problème.** « Quel problème concret as-tu vu, chez quelqu'un de précis ? Raconte-moi la dernière fois où il l'a vécu. » Cherche : une situation, une fréquence, un coût (temps, argent, clients perdus). Un problème que l'utilisateur n'a pas vu de ses yeux est une hypothèse : dis-le gentiment.
2. **La cible.** « Qui exactement a ce problème ? » Un profil principal, décrit par un métier ou une situation, pas par « tout le monde » ni « les entreprises ». Demande ensuite s'il y a un deuxième type d'utilisateur (par exemple le client du client) et si celui-ci a besoin d'un compte.
3. **L'alternative actuelle.** « Comment cette personne s'en sort aujourd'hui ? » (tableur, messages, papier, un concurrent précis). C'est le minimum que le produit doit égaler. Demande ce qui l'énerve dans cette solution.
4. **Ce qui rend payant.** « Qu'est-ce qui fait que quelqu'un sortirait sa carte ? » Cherche une raison précise : gain de temps chiffré, argent récupéré, risque évité. Puis : gratuit ou payant dès le début, quelle limite gratuite, quel prix par mois (un ordre de grandeur suffit). Si l'utilisateur ne sait pas, note-le en question ouverte : on ne bloque pas le cadrage là-dessus, mais on ne laisse pas le champ vide.
5. **Le périmètre V1 : une seule promesse.** « Si ton produit ne pouvait faire qu'une chose pendant le premier mois, ce serait laquelle ? » Fais écrire le parcours complet d'un utilisateur en une phrase, sans « et aussi ». Tout ce qui s'ajoute à cette phrase va dans « Hors périmètre V1 ». Pousse à réduire : propose toi-même ce qu'on peut retirer et demande confirmation.
6. **Le critère de réussite.** « À quoi verras-tu que ça marche, sans regarder une ligne de code ? » (exemple : un vrai professionnel reçoit une vraie réservation sans mon aide).

Questions de précision, après les six premières, toujours une par une et seulement si utiles :

7. **Les règles du métier.** Les limites, délais, cas particuliers, exprimés avec des chiffres ou des conditions vérifiables par quelqu'un qui ne connaît pas le projet (« aucun créneau à moins de 2 heures »). Une règle floue (« pas trop tôt ») n'est pas une règle : transforme-la.
8. **Le cadre légal.** Qui édite le produit (particulier, entreprise individuelle, société) ? Vend-il à des particuliers, à des professionnels, aux deux ? Franchise de TVA ou non ? Si l'utilisateur ne sait pas, écris « à décider » ; ne réponds pas à sa place. Le détail se traitera avec `docs/checklist-legale.md` avant l'ouverture au public.
9. **Les données.** « De quoi l'application doit-elle se souvenir ? » Voir « Données en langage simple » plus bas.

Si l'utilisateur change d'avis en cours de route, mets à jour ce que tu as retenu et dis ce qui change. Si deux réponses se contredisent, signale-le et demande laquelle gagne.

## 2. Écrire le cahier des charges

Crée `docs/cahier-des-charges.md` (le dossier `docs/` si besoin). Écris en français simple, au présent, phrases courtes. Reste sous deux pages. Pas de jargon technique : pas de nom de table, de route ou de bibliothèque. Plan :

```markdown
# Cahier des charges : <produit> (V1)

Dernière mise à jour : <date>
Statut : à valider

## Problème
<Qui a quel problème, comment il le contourne aujourd'hui, ce que ça lui coûte. 3 à 5 phrases.>

## Utilisateurs
- <Profil 1> : <ce qu'il peut faire, a-t-il un compte ?>
- <Profil 2> : <idem>

## Promesse de la V1
<Une phrase : le parcours complet, sans « et aussi ».>

## Parcours
<Étapes numérotées, de la porte d'entrée à l'objectif. Une seule version.>
1. ...

## Écrans
<Une ligne par écran : son nom, qui le voit, ce qu'on y fait.>
- <Écran> : <qui, pour faire quoi>

## Règles métier
<Chiffrées ou conditionnelles.>
- ...

## Données en langage simple
<Ce que l'application doit retenir, par « chose », avec ses informations en mots courants et ses liens.>
- <Chose> : <informations>. Appartient à <autre chose>.

## Offre
<À supprimer si gratuit.>
- Gratuit : <limite chiffrée>
- Payant : <prix par mois, ce que ça débloque>
- Quand la limite gratuite est atteinte : <ce que voit l'utilisateur>

## Cadre légal
- Qui édite : <particulier / entreprise individuelle / société / à décider>
- Vend à : <particuliers / professionnels / les deux / à décider>
- Franchise de TVA : <oui / non / à décider>

## Hors périmètre V1
<Aussi précis que les fonctionnalités. C'est le garde-fou contre les ajouts, les envies de la deuxième semaine et les initiatives de Claude Code.>
- ...

## Questions ouvertes
<Une ligne par question, avec la date où on y revient.>
- ...

## Critère de réussite
<Une phrase qui ne dépend d'aucune ligne de code.>
```

### Données en langage simple

Décris ce qui doit être retenu comme à un ami : « Un professionnel a un nom, une adresse e-mail et un lien public. Il propose des prestations (nom, durée, prix en euros). Un client réserve une prestation à une date et une heure ; on retient son prénom et son e-mail. » Ne nomme ni table, ni colonne, ni type. Indique ce qui est privé (visible seulement par son propriétaire) et ce qui est public. Indique ce qui doit pouvoir être supprimé quand quelqu'un le demande. La traduction technique viendra plus tard.

## 3. Validation

1. Relis le cahier à l'utilisateur en résumé de cinq lignes : promesse, utilisateurs, parcours en une phrase, ce qui n'est pas dans la V1, ce qui reste ouvert.
2. Fais le test de la V1 : « Peux-tu décrire le parcours complet sans dire "et aussi" ? ». Si non, retourne à la question 5.
3. Demande une validation explicite : « Écris-moi "je valide" si le cahier te convient, ou dis-moi ce qui doit changer. » Une réponse tiède (« ouais, ça a l'air bien ») n'est pas une validation : demande ce qui le fait hésiter.
4. À la validation : passe `Statut : à valider` à `Statut : validé le <date>`. Si `CLAUDE.md` existe, ajoute (ou mets à jour) une section de trois lignes :

```markdown
## Produit
Le cahier des charges est dans `docs/cahier-des-charges.md`, à relire avant toute nouvelle fonctionnalité.
Rien de ce qui figure dans « Hors périmètre V1 » ne s'ajoute sans que je le demande.
```

5. Propose le prochain pas sans l'exécuter : les parcours et les écrans en détail, ou la direction artistique (skill `direction-artistique`) avant de toucher à l'interface. Propose un commit du seul fichier `docs/cahier-des-charges.md` ; ne le fais pas sans accord.

## Garde-fous

- Une question par message pendant les six premières questions.
- Tu n'inventes ni besoin, ni chiffre, ni concurrent. Ce que tu ne sais pas devient une question ouverte.
- Tu ne rajoutes jamais de fonctionnalité de ta propre initiative. Tu peux en suggérer une, dans « Hors périmètre V1 ».
- Après validation, toute modification de périmètre passe par le cahier d'abord, le code ensuite.
