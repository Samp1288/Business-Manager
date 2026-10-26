# Simplixion

Simplixion est une application web progressive locale et hors ligne pour gérer une entreprise : projets, soumissions, échéanciers de paiement, transactions, récurrences, suivi comptable annuel et exports.

## Fonctionnalités

- Tableau de bord global ou filtré par projet.
- Projets avec statut modifiable directement depuis la carte.
- Soumissions liées à un projet avec lignes détaillées, taxes, conditions et échéancier de paiement.
- Transactions classées par projet, client ou fournisseur, référence, mode de paiement, taxes et déductibilité.
- Revenus et dépenses réels séparés des opérations prévues.
- Récurrences hebdomadaires, aux deux semaines et mensuelles.
- Rapport comptable annuel : revenus hors taxes, dépenses déductibles, résultat, taxes perçues/payées, catégories et vue mensuelle.
- Catégories personnalisées pour revenus et dépenses.
- Sauvegarde locale automatique, mode hors ligne, synchronisation anonyme optionnelle et export JSON/CSV/XLSX.
- Fichier Excel maître : import/export partout; écriture directe dans un fichier sélectionné lorsque le navigateur prend en charge File System Access API.
- Interface responsive ordinateur, tablette et mobile, thème clair/sombre et français/anglais.

## Excel

Le navigateur ne permet pas à une application web de modifier silencieusement n’importe quel fichier Excel. Simplixion fournit donc deux niveaux :

1. Export/import XLSX complet, compatible avec tous les navigateurs modernes.
2. Dans un navigateur compatible, « Choisir le fichier maître » puis « Enregistrer dans Excel » pour réécrire le fichier sélectionné. L’autorisation du navigateur est nécessaire et le fichier doit être sélectionné à nouveau si elle expire.

Le classeur contient les feuilles Projects, Transactions, Budgets, Recurring, Estimates, EstimateItems, EstimatePayments et Accounting.

## Déploiement Cloudflare Pages

1. Mets le contenu de ce dossier à la racine du dépôt GitHub `Samp1288/Business-Manager`.
2. Dans Cloudflare, ouvre **Workers & Pages** puis **Create application** → **Pages** → **Connect to Git**.
3. Sélectionne le dépôt `Business-Manager`, la branche `main`, et configure :
   - commande de compilation : vide;
   - dossier de sortie : `.`;
   - racine : `/`.
4. Donne au projet le nom `business-manager` ou `simplixion` si Cloudflare l’accepte.
5. Clique **Save and Deploy**.
6. Après chaque nouvelle modification, envoie les fichiers sur GitHub; Cloudflare redéploie automatiquement.

Le service worker utilise le cache `simplixion-v10-static`. Si un ancien écran reste affiché, ouvre l’application en navigation privée ou vide les données du site une seule fois, puis recharge.

## Données et synchronisation

Par défaut, les données restent dans le navigateur et sont enregistrées automatiquement, sans compte ni connexion. Pour synchroniser plusieurs appareils, configure une API Cloudflare Workers compatible dans `config.js`, puis redéploie. Sans API, l’app reste entièrement utilisable hors ligne.

## Comptabilité et impôts

Le module prépare un registre et des exports utiles pour la fin d’année, mais ne remplace pas un logiciel de production de déclarations ni la validation d’un comptable. Les taux de taxes sont configurables par entreprise et doivent être vérifiés selon la province, le type de vente, les exemptions, les dépenses admissibles et les règles applicables. Conserve les factures et reçus originaux.

## Structure technique

- `index.html` : interface et formulaires.
- `styles.css` : design responsive et thèmes.
- `app.js` : logique, stockage local, comptabilité et synchronisation.
- `excel.js` : lecture/écriture XLSX sans service externe.
- `sw.js` : mode hors ligne et cache PWA.
- `manifest.webmanifest` et `icon-simplixion.png` : installation mobile et identité Simplixion.
