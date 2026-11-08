---
name: revue-avant-mise-en-ligne
description: À utiliser avant d'ouvrir l'application au public, avant d'activer les paiements en live, avant un déploiement en production, ou quand l'utilisateur demande « est-ce prêt à mettre en ligne ? », un audit, une revue de sécurité ou un contrôle avant lancement. Passe en revue la sécurité (secrets, RLS Supabase testée, clés publishable et secret, webhooks signés), le légal (mentions légales, CGU/CGV, confidentialité, cookies), la performance, le mobile, l'accessibilité de base et les e-mails, puis rend un tableau Bloquant / À faire / OK expliqué simplement. Lecture seule, ne corrige rien sans accord.
---

# Revue avant mise en ligne

Tu fais la revue d'un projet Next.js / Supabase / Vercel (avec éventuellement Resend et Stripe) pour un fondateur non technique. Tu es un relecteur extérieur : tu ne défends pas ce qui a été écrit, tu cherches ce qui peut mal tourner une fois que de vrais utilisateurs arrivent. Si cette conversation a servi à construire le projet, conseille à l'utilisateur de lancer `/clear` et de relancer ce skill dans une conversation neuve : une revue faite par celui qui a écrit le code est moins fiable.

**Lecture seule.** Tu lis, tu cherches, tu lances des commandes de vérification. Tu ne modifies aucun fichier, tu ne pousses rien, tu ne touches ni à la base ni à Stripe ni à Vercel sans accord explicite. Tu ne lis jamais un fichier `.env*` et tu n'affiches jamais la valeur d'une clé : seulement des noms de variables et des noms de fichiers.

## 0. Préparation

1. Cherche les checklists dans `docs/` : `checklist-securite.md`, `checklist-legale.md`, `checklist-mise-en-ligne.md`, `checklist-passage-en-live.md`. Lis celles qui existent en entier : leurs cases sont la référence, ce skill est le plan de la revue. Si une checklist manque, dis-le dans le rapport et travaille avec les points ci-dessous.
2. Lis `CLAUDE.md`, `docs/cahier-des-charges.md` (s'il existe) et `package.json`. Relève ce qui est utilisé : Supabase, Stripe, Resend, comptes utilisateurs. Saute les sections qui ne concernent pas le projet (pas de Stripe : pas de revue paiement) et écris « sans objet » dans le tableau.
3. Demande à l'utilisateur, une question à la fois, uniquement ce que le code ne dit pas : le site est-il déjà public ? Des paiements réels sont-ils prévus tout de suite ? Qui édite le site (particulier ou société) ? Vend-il à des particuliers ?
4. Précise la cible de la revue : développement local, prévisualisation ou production. Ne lance aucun test d'écriture contre la base de production.

## 1. Sécurité

Preuves à rassembler, avec la commande ou le fichier qui les justifie :

1. **Secrets hors du code.** Cherche dans tout le projet, hors `node_modules` et hors fichiers `.env*` : `sb_secret`, `service_role`, `SUPABASE_SECRET`, `sk_live`, `sk_test`, `rk_live`, `whsec_`, `eyJ`. Une clé écrite en dur = **Bloquant** (et la clé est à régénérer chez le service, car elle a pu être lue). Indique aussi les fichiers marqués `"use client"` qui lisent une clé secrète.
2. **Préfixe `NEXT_PUBLIC_`.** Liste les variables `NEXT_PUBLIC_*`. Seules l'URL Supabase, la clé publishable Supabase et la clé publiable Stripe (`pk_`) ont le droit de le porter. Toute clé secrète avec ce préfixe = **Bloquant**.
3. **Git.** `git check-ignore -v .env.local` doit afficher une règle. Vérifie que l'historique n'a jamais contenu de fichier `.env` : `git log --all --diff-filter=A --name-only`, en cherchant seulement des noms de fichiers. Un fichier de secrets déjà passé dans l'historique = **Bloquant**, régénérer les clés.
4. **RLS Supabase activée.** Si le MCP Supabase est connecté, en lecture seule et sur la base de **développement**, liste les tables du schéma `public` avec : RLS activée ou non, nombre de politiques. Sinon, lis les fichiers de `supabase/migrations/` : chaque `create table` a-t-il son `enable row level security` et au moins une politique ? Une table sans RLS = **Bloquant**. Une table avec RLS mais sans politique = bloquée pour tout le monde : pas un risque, à signaler « À faire » si elle devrait être accessible. Signale toute politique `using (true)` sur des données privées et toute vue (les vues contournent la RLS par défaut).
5. **RLS testée, pas seulement activée.** Cherche un test à deux comptes (un script ou une procédure notée dans `docs/`). S'il n'existe pas : **Bloquant** pour toute donnée d'utilisateur ; propose d'écrire le script (clé publishable uniquement, base de développement uniquement : sans connexion, comme le pro A, comme le pro B, lecture / modification / suppression des lignes de B depuis A, tableau des résultats) et attends l'accord de l'utilisateur avant de le créer. Un script qui existe mais qu'on n'a pas lancé = « À faire », pas « OK ».
6. **Clés publishable et secret.** La clé secrète n'est lue que dans des fichiers serveur. Les anciennes clés (`anon`, `service_role`) ne sont plus utilisées. Côté serveur, l'utilisateur est vérifié avec `getClaims()` et non `getSession()`.
7. **Accès aux pages et actions.** Chaque page, Server Action et route API réservée vérifie qui appelle, à l'endroit où les données sont lues, pas seulement dans `proxy.ts`. Fais un tableau : élément, qui peut l'appeler, ce qu'il valide, ce qu'il écrit. Un identifiant (`pro_id`, `user_id`) pris dans un formulaire au lieu de la session = **Bloquant**. Les refus renvoient 401 ou 403, pas 500.
8. **Entrées de l'extérieur.** Validation côté serveur, pas seulement dans le navigateur. Cherche `dangerouslySetInnerHTML`, les e-mails HTML qui insèrent un champ saisi sans l'échapper, les redirections qui reprennent un paramètre d'adresse. Limites de débit sur inscription, mot de passe oublié, réservation publique, envoi d'e-mails.
9. **Webhooks signés.** Tout webhook (Stripe en particulier) vérifie la signature avec `constructEvent()` sur le corps brut, avant de lire son contenu. Webhook non signé = **Bloquant**. Il répond 2xx à un événement inconnu.
10. **Dépendances.** Lance `npm audit` ; explique chaque faille en une phrase et distingue développement / production. N'exécute pas `npm audit fix` sans accord. Vérifie que `package-lock.json` est commité.
11. **Claude Code.** `.claude/settings.json` refuse la lecture de `.env` (`Read(./.env)` et `Read(./.env.*)` dans `deny`). Le MCP Supabase, s'il est branché, n'est pas branché sur la production.

## 2. Légal

Sans avis juridique : tu vérifies que les pages existent et correspondent à l'application, tu ne rédiges pas un texte engageant à la place d'un juriste. Référence : `docs/checklist-legale.md`.

1. Les pages `/mentions-legales`, `/cgu`, `/cgv`, `/confidentialite` existent et sont liées depuis le pied de page. Mentions légales absentes sur un site public = **Bloquant**. CGV absentes alors que le site encaisse = **Bloquant**.
2. Cherche `À COMPLÉTER` et `À DÉCIDER` dans tout le projet. Il ne doit en rester aucun en production : un seul = **Bloquant**.
3. Les textes disent vrai : prix, limites gratuites, résiliation, e-mails envoyés, sous-traitants (hébergeur, base de données, e-mails, paiement) correspondent au comportement réel de l'application. Liste chaque écart.
4. Phrases d'acceptation en place (CGU à l'inscription, CGV avant le paiement). Mention de TVA cohérente avec la situation (franchise ou non) : à faire confirmer par un comptable.
5. Cookies : recense ce que le site dépose avant tout clic (navigation privée, outils de développement du navigateur, onglet « Application »). Un traceur non exempté sans consentement = **Bloquant**. Si un bandeau existe : « Tout refuser » aussi simple que « Tout accepter ».
6. Droits des personnes : une adresse de contact dédiée, un bouton de suppression de compte testé avec un compte jetable, un registre des traitements.
7. Rappelle que les textes ont besoin d'une relecture humaine et, pour un engagement contractuel, d'un juriste.

## 3. Performance

1. Lance `npm run lint` puis `npm run build` (avec Next.js 16, `build` ne lance plus le lint). Un échec = **Bloquant**.
2. Lis le résumé du build : pages inhabituellement lourdes, pages rendues à chaque requête alors qu'elles pourraient ne pas l'être.
3. Images : `next/image` plutôt que `<img>`, dimensions données, pas de fichier de plusieurs mégaoctets. Polices chargées via `next/font`.
4. Requêtes : pas de requête à la base répétée dans une boucle, pas de `select *` sur de grosses tables pour afficher trois champs.
5. Vercel : région des fonctions proche de la base (une base à Paris avec des fonctions à Washington ralentit chaque page).
6. Si un MCP navigateur est connecté, ouvre la page d'accueil et le parcours principal sur le site en ligne et note les temps de chargement et les erreurs de console. Sinon, indique à l'utilisateur de lancer l'outil Lighthouse des outils de développement de Chrome et de te donner le résultat. Ne chiffre rien que tu n'as pas mesuré.

## 4. Mobile

1. Si un MCP navigateur est connecté : capture de chaque écran principal à environ 390 px de large et environ 1440 px. Sinon, demande des captures.
2. Rien ne déborde horizontalement, aucun texte coupé, les boutons et liens se touchent facilement (environ 44 px de haut), les champs ne sont pas masqués par le clavier, le bon type de clavier s'ouvre (e-mail, téléphone).
3. Le parcours principal se termine sur un téléphone, en 4G (pas en Wi-Fi), depuis le site en ligne.

## 5. Accessibilité de base

1. `lang="fr"` sur la page. Un seul `h1` par page, titres dans l'ordre.
2. Chaque champ de formulaire a un label visible ; les erreurs sont écrites en toutes lettres, pas seulement signalées par une couleur.
3. Contraste : texte courant 4,5 pour 1 minimum, éléments d'interface 3 pour 1 (calcule-le pour la palette, vérifie aussi les états désactivé et survol).
4. Navigation au clavier : touche Tab parcourt tous les éléments interactifs dans un ordre logique, le focus est toujours visible, aucun piège clavier dans une fenêtre modale.
5. Les images porteuses de sens ont un texte alternatif ; les images décoratives, un texte alternatif vide.
6. Les boutons-icônes ont un nom accessible (`aria-label`).

## 6. E-mails (si Resend ou équivalent)

1. Le domaine d'envoi est vérifié chez le fournisseur et l'expéditeur est une adresse de ce domaine.
2. Les liens des e-mails (confirmation, mot de passe oublié, réservation) pointent vers le domaine de production, jamais vers `localhost`. Site URL et Redirect URLs de Supabase sont à jour, aux chemins exacts en production.
3. Le texte saisi par un visiteur est échappé avant d'être inséré dans un e-mail HTML.
4. L'envoi est attendu avant que la réponse parte (sur Vercel, un envoi non attendu peut être coupé).
5. Test réel : un e-mail reçu dans une vraie boîte, qui n'arrive pas dans les courriers indésirables, lisible sur mobile. Tu ne peux pas le vérifier toi-même : écris-le dans « À faire » pour l'utilisateur.
6. Quota de l'offre gratuite connu, et limite de débit sur les actions qui déclenchent un envoi.

## 7. Paiement et environnement (si Stripe et Vercel)

1. Montants et identifiant de prix décidés côté serveur ; identifiants de prix dans des variables d'environnement. L'accès payant se décide sur une table alimentée par le webhook, pas sur une page de succès.
2. Les clés Stripe de test sont dans Preview et Development, les clés live dans Production seulement. Aucune valeur n'est vérifiable par toi (les variables Vercel ne sont pas lisibles ici) : demande une capture de la page des variables, sans valeurs visibles.
3. Preview n'écrit pas dans la base de production.
4. Vercel Pro activé avant d'encaisser (offre Hobby limitée à un usage personnel non commercial).
5. Parcours complet testé en mode test, y compris carte refusée et annulation. Pour le passage en live, applique `docs/checklist-passage-en-live.md` case par case.

## 8. Le rapport

Rends exactement ce format, en français simple. Une ligne par point vérifié, regroupée par domaine. Le statut est l'un de ces trois :

- **Bloquant** : ne pas ouvrir au public (ou ne pas encaisser) tant que ce n'est pas réglé. Fuite de données possible, clé exposée, contrôle d'accès absent, texte légal obligatoire manquant, build qui échoue.
- **À faire** : à corriger, mais sans danger immédiat ; ou point que tu n'as pas pu vérifier et qui demande l'utilisateur.
- **OK** : vérifié, avec la preuve (commande lancée, fichier lu, capture prise).

```text
| Domaine | Point | Statut | Explication simple | Preuve ou correction |
|---|---|---|---|---|
| Sécurité | Clés secrètes dans le code | OK | Aucune clé trouvée dans les fichiers | recherche sur tout le projet, 0 résultat |
| Sécurité | Sécurité des tables (RLS) | Bloquant | La table « reservations » est lisible par n'importe qui avec la clé publique | supabase/migrations/002.sql : pas de RLS ; à corriger avant tout |
| Légal | Mentions légales | À faire | La page existe mais contient 3 « À COMPLÉTER » | app/mentions-legales/page.tsx |
```

Règles du rapport :

- **Jamais « OK » sans preuve.** Ce que tu n'as pas pu vérifier est « À faire » avec la mention « non vérifié » et la raison.
- Explique chaque Bloquant en une phrase : ce qu'un inconnu ou un client pourrait faire concrètement.
- Termine par : le nombre de Bloquant / À faire / OK ; les trois actions à faire en premier, dans l'ordre ; ce que tu n'as pas pu vérifier sans l'application en ligne.
- Propose de sauvegarder le rapport dans `docs/audit-avant-lancement.md` ; ne le fais qu'avec l'accord de l'utilisateur. Les points « À faire » non traités vont dans `docs/backlog.md`.
- Ne corrige rien dans la foulée. Demande par quel Bloquant l'utilisateur veut commencer, puis traite-les un par un, avec un test pour chacun (un test qui échoue avant la correction et passe après), et relance ce skill à la fin.
