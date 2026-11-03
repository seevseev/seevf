# SEEV – Salon Estudiantin de l’Économie Verte

Site vitrine et d’inscription de la première édition du SEEV
(Centre Culturel Régional de Fatick, du 1er au 3 janvier 2027).

Le site est une **page unique** (`index.html`) hébergée sur Vercel.
Les inscriptions sont enregistrées dans Supabase et le paiement est géré par Chariow.

## Fichiers du projet

| Fichier | Rôle |
|---|---|
| `index.html` | Tout le site : contenu, styles, images et formulaires |
| `README.md` | Ce document |

## Fonctionnement des inscriptions

Il y a deux formulaires :

| Formulaire | Public | Tarif | Valeur enregistrée dans `profil` |
|---|---|---|---|
| Inscription des étudiants (`#inscription`) | Étudiants | 5 000 FCFA | `Étudiant(e)` |
| Inscription des écoles de formation (`#ecoles`) | Écoles et organismes de formation | 300 000 FCFA | `École de formation` |

Parcours du visiteur :

1. Il remplit le formulaire.
2. Ses informations sont enregistrées dans la table Supabase `inscriptions`
   (colonnes : `profil`, `prenom`, `nom`, `email`, `telephone`, `structure`, `jours`).
3. Il est redirigé vers la page de paiement Chariow, avec son prénom, son nom et son e-mail préremplis.
4. Chariow encaisse le paiement et livre le produit.

Le formulaire de contact enregistre les messages dans la table `messages`.

## Liens de paiement Chariow

Les liens se trouvent dans la partie `<script>` à la fin de `index.html` :

| Public | Lien |
|---|---|
| Étudiants (5 000 FCFA) | https://dbprophy.mychariow.co/prd_cz9po30u |
| Écoles (300 000 FCFA) | https://dbprophy.mychariow.co/prd_3hvi74yw |

Pour changer un lien, cherchez `mychariow` dans `index.html` et remplacez l’adresse concernée
(`const pay=payTo('…')` pour les étudiants, `const payEcole=payTo('…')` pour les écoles).

## Supabase

- L’adresse du projet et la clé **publishable** sont dans le script de `index.html`.
  Cette clé est publique par conception : elle ne donne que les droits prévus par les règles de sécurité.
- Sécurité de la table `inscriptions` : la RLS est activée et une seule règle existe
  (insertion autorisée pour les visiteurs). Ils ne peuvent ni lire, ni modifier, ni supprimer.
- Pour consulter les inscriptions : Supabase Dashboard, puis Table Editor, puis `inscriptions`.

## Limite actuelle

Supabase ne sait pas qui a payé. Les paiements se consultent dans le tableau de bord Chariow ;
pour savoir qui a payé, rapprochez les clients Chariow et les inscriptions par e-mail.
Un suivi automatique (colonne « payé » et webhook Chariow) pourra être ajouté plus tard.

## Mise en ligne (Vercel)

1. Remplacer `index.html` dans le dépôt GitHub (Add file, puis Upload files, puis Commit changes).
2. Vercel redéploie automatiquement. Attendre le statut **Ready** dans l’onglet Deployments.
3. Recharger la page avec Ctrl+Maj+R.

Le fichier doit s’appeler exactement `index.html` et se trouver à la racine du dépôt.

## Test après chaque modification

1. Remplir le formulaire étudiants avec un e-mail de test : la ligne apparaît dans Supabase
   et la page de paiement Chariow s’ouvre avec les informations préremplies.
2. Faire la même chose avec le formulaire des écoles.

## Précautions

- Ne jamais mettre de clé secrète (clé API Chariow, clé `service_role` de Supabase) dans `index.html` ni dans ce README.
- Sur un site statique, tous les fichiers du dépôt, README compris, peuvent être ouverts par n’importe qui
  à leur adresse (par exemple `/README.md`). N’y écrire aucune information confidentielle.
