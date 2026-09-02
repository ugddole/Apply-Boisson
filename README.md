# Buvette — Boissons & Snacks

Appli Flask complète : boissons, snacks avec composants (nomenclature),
compétitions, commande par fournisseur avec export Excel, affiches
tarifaires imprimables avec fond personnalisable — une pour les boissons,
une pour les snacks.

Chaque association a **sa propre copie déployée** (son propre dépôt
GitHub, son propre service Railway, sa propre base de données).

## Utilisation en local

```bash
pip install -r requirements.txt
python app.py
```

Ouvrir http://localhost:5000 — mot de passe par défaut : `buvette`
(à changer, voir plus bas).

## Déployer (GitHub + Railway)

1. Créer un dépôt GitHub (ex. `buvette-<nom-association>`), y déposer
   tous ces fichiers.
2. Sur [railway.app](https://railway.app), créer un projet à partir de
   ce dépôt.
3. Dans l'onglet **Variables** du service Railway, définir :
   - `APP_PASSWORD` — mot de passe d'accès pour cette association
   - `ASSOCIATION_NOM` — nom affiché en haut de l'appli
   - `SECRET_KEY` — une chaîne aléatoire longue (sécurité des sessions)
4. Ajouter un **volume persistant** monté sur `/app` pour que
   `buvette.db` et les images de fond survivent aux redéploiements.
5. Railway détecte le `Procfile` et lance l'appli automatiquement.

Pour une nouvelle association, on répète ces 5 étapes avec un nouveau
dépôt et un nouveau service Railway.

## Fonctionnement

- **Boissons** : catalogue (nom, contenant, servi, prix, qté/pack,
  fournisseur, commentaire). Un sélecteur en haut filtre l'affichage
  par compétition ; ajouter une boisson pendant qu'un filtre est actif
  l'inclut automatiquement dans cette compétition.
- **Snacks** : catalogue de snacks, chacun avec ses **composants**
  (ingrédients) en sous-tableau — nom, quantité par unité vendue,
  unité, quantité par pack fournisseur, fournisseur. Même filtre par
  compétition que les Boissons.
- **Compétitions** : créer/supprimer une compétition, définir un titre
  d'affiche distinct pour les boissons et pour les snacks, et saisir
  les quantités prévues (boissons et snacks) pour cette compétition.
- **Commande** / **Commande snacks** : liste à commander groupée par
  fournisseur (les snacks agrègent automatiquement les composants de
  tous les snacks prévus), avec bouton d'export Excel.
- **Affiche** / **Affiche snacks** : affiche tarifaire générée à partir
  des boissons/snacks sélectionnés, fond d'image personnalisable,
  bouton Imprimer (le fond est bien inclus à l'impression).
- Les flèches monter/descendre (Boissons, Snacks) réordonnent la liste
  par rapport aux éléments **actuellement visibles** — si un filtre par
  compétition est actif, l'élément se déplace parmi les éléments de
  cette compétition, sans perturber la position des autres.

## Sécurité

Accès protégé par un unique mot de passe partagé (pas de comptes
individuels) — pensé pour une petite équipe de bénévoles.

## Structure

```
app.py                     routes et logique
templates/                 pages HTML (Jinja2)
static/uploads/            images de fond d'affiche (créé automatiquement)
buvette.db                 base SQLite (créée automatiquement)
```
