---
name: verifier-sans-coder
description: À utiliser à la fin de chaque fonctionnalité, correction de bug ou modification visible, avant de dire que c'est terminé et avant tout commit. Fait vérifier le travail à un utilisateur qui ne lit pas le code, avec un résumé en français simple de ce qui a changé, parcours à tester pas à pas dans le navigateur, 3 à 5 cas limites à essayer, vérifications automatiques lancées (lint, build, tests s'ils existent, MCP navigateur si disponible), compte rendu « ce qui marche / ce qui reste », puis proposition de commit Git avec un message clair.
---

# Vérifier sans coder

L'utilisateur ne lit pas le code. Il ne peut donc juger ton travail que par ce qu'il voit et fait dans le navigateur. Tu dois lui rendre cette vérification facile et ne jamais lui dire « ça marche » sans l'avoir prouvé. Tu parles en français simple, sans jargon.

Ce skill s'applique à la fin d'un travail, pas pendant. Il ne remplace pas la revue avant mise en ligne (skill `revue-avant-mise-en-ligne`).

## 1. Ce qui a changé, en français simple

1. Lance `git status` et `git diff --stat` pour connaître les fichiers touchés. Lis les modifications importantes.
2. Écris, en 3 à 6 puces, ce qui a changé **du point de vue de l'utilisateur de l'application** : ce qu'on voit, ce qu'on peut faire de nouveau, ce qui se comporte autrement. Pas de nom de fonction ni de fichier dans cette liste.
3. Écris ensuite une ligne « Sous le capot » avec les fichiers principaux touchés (pour mémoire, sans explication technique).
4. Signale tout ce qui sort de la demande : fichier créé non demandé, dépendance ajoutée (`git diff package.json`), table ou règle de sécurité modifiée, variable d'environnement nouvelle. Si tu as touché à quelque chose qu'on ne t'avait pas demandé, dis-le en premier et propose de l'annuler.

## 2. Ce que tu vérifies toi-même

Lance ce qui est vérifiable automatiquement, dans cet ordre, et note le résultat réel de chaque commande (réussi, échoué, avec la première erreur utile) :

1. Lis `package.json` (section `scripts`) pour connaître les vraies commandes. N'invente aucune commande.
2. Si un script `verifier` existe, lance `npm run verifier` : il regroupe en général lint, tests et build.
3. Sinon, lance séparément : `npm run lint`, puis `npm run build`. Avec Next.js 16, `build` ne lance plus le linter : les deux sont nécessaires.
4. Si un script `test` existe, lance `npm run test`. S'il n'existe pas, écris « pas de tests dans ce projet » ; n'en crée pas sans qu'on te le demande.
5. Si un serveur de développement tourne ou peut tourner (`npm run dev`, `http://localhost:3000` par défaut) et qu'un MCP navigateur est connecté (`/mcp` liste les serveurs) : ouvre le parcours à tester (étape 3), exécute-le, relève les erreurs de la console du navigateur et les requêtes en échec, prends une capture à la largeur d'un ordinateur et une à la largeur d'un téléphone (environ 390 px).
6. Si aucun MCP navigateur n'est disponible, dis-le : « Je n'ai pas pu ouvrir la page moi-même ». Tout le parcours passe alors à la charge de l'utilisateur.

Si une commande échoue : ne dis pas « terminé ». Montre l'erreur en une phrase simple, corrige si la cause est dans ton travail, relance, et recommence l'étape. Ne désactive jamais un contrôle (règle de lint, test, vérification de type) pour faire passer. Si tu n'arrives pas à corriger après deux tentatives, dis-le et explique ce que tu as essayé.

## 3. Le parcours à tester par l'utilisateur

Écris un parcours numéroté, une action par étape, avec l'adresse exacte à ouvrir et, pour chaque étape, **ce qu'il doit voir**. Exemple de format :

```text
Parcours à tester (5 minutes)
1. Ouvre http://localhost:3000/p/camille-coach.
   Tu dois voir : le nom de Camille et trois prestations.
2. Clique sur « Séance découverte », puis sur le premier créneau libre.
   Tu dois voir : un formulaire avec ton prénom et ton e-mail.
3. Remplis-le et valide.
   Tu dois voir : un message de confirmation, et la réservation dans le tableau de bord de Camille.
```

Règles :
- Maximum 8 étapes. Si le parcours est plus long, coupe-le en deux.
- Dis quel compte utiliser (connecté ou non, quel rôle) et si des données de test sont nécessaires. Si elles manquent, propose de les préparer.
- Dis à quel moment il doit rafraîchir la page ou se reconnecter.
- Distingue ce que tu as déjà exécuté (« je l'ai fait, voici la capture ») de ce que lui seul peut vérifier (rendu sur son vrai téléphone, e-mail reçu dans sa vraie boîte, paiement test).

## 4. Les cas limites à essayer

Propose 3 à 5 cas qui cassent souvent, adaptés à ce qui vient d'être fait. Pour chacun : l'action, et le comportement correct attendu. Choisis dans cette liste ce qui s'applique, et ajoute un cas propre à la fonctionnalité :

- Champ vide, ou valeur trop longue, ou caractères spéciaux (accents, apostrophe, guillemets, « < »).
- Double clic sur le bouton d'envoi : la chose ne doit pas être créée deux fois.
- Se déconnecter, puis ouvrir directement l'adresse d'une page réservée : on doit être renvoyé vers la connexion.
- Recharger la page au milieu du parcours, ou revenir en arrière avec le navigateur.
- Écran de téléphone (largeur étroite) : rien ne déborde, les boutons se touchent facilement.
- Aucune donnée : liste vide, premier utilisateur. Le message doit dire quoi faire.
- Deux comptes différents : l'un ne voit jamais les données de l'autre.
- Une règle métier du cahier des charges prise à la limite (le dernier créneau possible, le montant minimum).
- Connexion lente ou coupée (désactiver le réseau dans le navigateur) : un message clair, pas un écran blanc.

Dis lesquels tu as déjà essayés toi-même et le résultat.

## 5. Compte rendu

Termine par ce tableau, honnête et court. « Ce qui marche » ne contient que ce qui a été prouvé (commande réussie, parcours exécuté, capture). Tout le reste est dans « Ce qui reste ».

```text
Ce qui marche (vérifié par moi)
- Lint : réussi
- Build : réussi
- Parcours réservation exécuté dans le navigateur, capture mobile et desktop prises
- Cas limites essayés : champ vide, double clic

Ce qui reste (à toi, ou non vérifié)
- Rendu sur ton vrai téléphone
- E-mail de confirmation : à vérifier dans ta boîte, je ne peux pas la lire
- Pas de tests automatiques dans ce projet

Écarts avec la demande
- Aucun (ou : j'ai ajouté X, voulu ou non ?)
```

## 6. Proposer un commit

1. Vérifie qu'aucun fichier sensible n'est prêt à partir : `git status` ne doit montrer aucun `.env`, `.env.local` ni fichier de clés. S'il y en a un, arrête-toi et alerte l'utilisateur.
2. Propose un message de commit clair, en français, qui commence par un mot court donnant la nature du changement : `feat` (nouvelle fonctionnalité), `fix` (correction), `style` (apparence), `docs` (documentation), `chore` (entretien). Une ligne de 70 caractères maximum, qui dit ce qui change pour l'utilisateur : « feat: réservation d'un créneau depuis la page publique ». Une ligne de détail en dessous si nécessaire.
3. Liste les fichiers qui partiraient dans le commit.
4. **Ne commite pas sans accord.** Demande : « Je commite avec ce message ? » Si l'utilisateur accepte, fais le commit. Ne pousse (`git push`) que s'il le demande explicitement.
5. Si les vérifications de l'étape 2 ne sont pas toutes au vert, propose quand même le commit seulement si l'utilisateur sait ce qui reste et le décide ; indique-le dans le message ou dans un fichier de suivi (`docs/backlog.md` s'il existe).

## Garde-fous

- Pas de « tout est bon » sans preuve. Si tu n'as rien lancé, tu écris que tu n'as rien lancé.
- Pas de nouveaux tests, de README ni de fichiers que l'utilisateur n'a pas demandés.
- Ne modifie pas le code pendant ce skill, sauf pour corriger une erreur de ton propre travail révélée par les vérifications.
- Reste bref : l'utilisateur doit pouvoir lire ton compte rendu en une minute.
