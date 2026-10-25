# Orbis Gestion

Plateforme locale d’organisation financière et de projets, sans compte obligatoire ni courriel. Elle regroupe les budgets, transactions, récurrences, soumissions et estimations dans des projets indépendants.

## Version 8.0

- Bouton Français / English, mémorisé sur chaque appareil.
- Chaque ouverture sur un nouvel appareil commence avec un espace local indépendant.
- Le lien public ne contient aucun budget partagé. La synchronisation ne commence qu’après l’import volontaire d’un code d’association.
- Les projets isolent les transactions, budgets, récurrences et soumissions d’un dossier à l’autre.
- Les soumissions et estimations contiennent un numéro, un client, un statut, des lignes, les taxes et un total.
- Échange Excel hors ligne : export d’un classeur `.xlsx` complet et import d’un classeur Excel ou d’un `.csv` de transactions.
- Méthode de budget à base zéro : revenus prévus, montants affectés par catégorie, montant restant à affecter, comparaison prévu/réel.
- Budgets et récurrences hebdomadaires, aux deux semaines ou mensuels.
- Une opération est soit prévue, soit payée/reçue, avec un bouton de changement rapide dans les listes.
- Catégories initiales réduites à Salaire net et cinq catégories de dépenses; les catégories personnelles sont ajoutables et mémorisées.
- Répartition avec pourcentage par catégorie, thème clair/sombre et affichage adapté à l’ordinateur, à la tablette et au téléphone.

## Fonctionnement

- Les données sont d’abord enregistrées dans IndexedDB sur l’appareil.
- L’application fonctionne hors ligne grâce au service worker.
- La synchronisation entre appareils est facultative et se fait avec un code d’association.
- Les données envoyées au serveur sont chiffrées dans le navigateur avant leur envoi.
- Le serveur ne reçoit pas les montants lisibles.
- Les transactions héritées de l’ancienne version `localStorage` `budget-alexis-v1` sont importées automatiquement à la première ouverture.
- Une mise à jour conserve la base IndexedDB existante et migre automatiquement les données vers le modèle avec projets et soumissions.

## Règle d’isolation des budgets

Deux personnes qui ouvrent le lien sur deux appareils ou deux navigateurs différents commencent avec deux espaces locaux différents. Pour utiliser les mêmes données sur plusieurs appareils, il faut importer le code d’association; ce code ne doit pas être partagé publiquement. Si deux personnes utilisent le même navigateur, elles doivent utiliser des profils de navigateur distincts.

## Échange Excel

Dans Réglages, « Exporter vers Excel » produit un classeur avec les feuilles Projects, Transactions, Budgets, Recurring, Estimates et EstimateItems. « Importer un fichier Excel » ajoute les données au contenu existant; les identifiants de projet permettent de conserver la segmentation.

L’application ne peut pas surveiller automatiquement un fichier Excel local après sa sélection. Pour une synchronisation en temps réel avec un fichier OneDrive ou SharePoint, une intégration Microsoft séparée serait nécessaire.

## Utilisation locale

Un serveur HTTPS ou localhost est nécessaire pour IndexedDB, le service worker et les notifications. Dans le dossier `budget-pro`, utilise par exemple :

```bash
python3 -m http.server 8080
```

Puis ouvre `http://localhost:8080`.

## Déploiement de l’interface

1. Créer un dépôt GitHub privé ou public.
2. Copier le contenu de `budget-pro` à la racine du dépôt.
3. Créer un projet Cloudflare Pages connecté au dépôt.
4. Choisir « Direct Upload » ou l’intégration Git.
5. Laisser la commande de build vide et utiliser le dossier racine comme dossier de sortie.
6. L’application sera disponible sur une adresse `pages.dev` gratuite.

Pour un usage uniquement sur un appareil, laisser `API_BASE_URL` vide dans `config.js`.

## Déploiement de la synchronisation

Le dossier `sync-api` contient une API Cloudflare Worker et une base D1. Les étapes détaillées sont dans `sync-api/README.md`.

Après avoir déployé l’API, remplacer `API_BASE_URL` dans `config.js` par l’URL Worker, puis redéployer Pages.

## Limites importantes

- Sans API, les données restent sur l’appareil.
- Pour associer un autre appareil, l’appareil initial doit afficher le code d’association.
- La perte du code d’association signifie qu’un nouvel appareil ne peut pas récupérer les données existantes.
- Les notifications du navigateur nécessitent une permission et ne remplacent pas une application native.
- Un export JSON régulier est recommandé.
