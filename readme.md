# heavy-shop

## Contexte pedagogique

Repo miroir pedagogique d un e-commerce / catalogue. Il expose trop d images, des payloads produits verbeux, des filtres qui sollicitent beaucoup le backend et des composants front chargeant toute la logique au demarrage.

Ce projet est volontairement fonctionnel mais non optimise. Il sert de support d analyse et d experimentation dans un cadre de formation.

## Perimetre fonctionnel

- Accueil boutique
- Listing produits
- Fiche produit
- Recherche / filtres
- Panier
- Checkout simplifie

## Anti-patterns presents

- trop d images lourdes
- payload produit surcharge
- filtres qui declenchent trop de requetes
- polling de stock ou promo
- carrousels multiples
- duplication de donnees
- absence de cache
- chargement JS global

## Lancement

`npm install`

`npm run dev`

Frontend: http://localhost:5173

Backend: http://localhost:4100

## Mesure et outillage

- Lighthouse sur accueil boutique et recherche
- EcoIndex sur listing produits
- poids des visuels et JS
- nombre de requetes declenchees par les filtres

## CI EcoIndex

Le repo contient une CI GitHub Actions dediee a l EcoIndex dans [`.github/workflows/greenit-analysis.yml`](.github/workflows/greenit-analysis.yml).

Cette CI reste volontairement simple :

- elle build l application
- elle demarre le front et le back
- elle lance `GreenIT-Analysis-cli`
- elle mesure 2 pages statiques :
  - `http://127.0.0.1:5173/`
  - `http://127.0.0.1:5173/products`
- elle publie le rapport HTML en artifact GitHub Actions

Le fichier de configuration des pages analysees se trouve dans [`.github/greenit/scenarios.yaml`](.github/greenit/scenarios.yaml).

### Prise en main

1. Ouvrir l onglet `Actions` du depot GitHub.
2. Lancer le workflow `EcoIndex` via `Run workflow`, ou pousser un commit sur `main`.
3. Attendre la fin du job `EcoIndex with GreenIT Analysis CLI`.
4. Ouvrir l artifact `greenit-analysis-report`.
5. Consulter le fichier `output/report.html`.

### Modifier les pages mesurees

Pour ajouter ou retirer une page, modifier simplement [`.github/greenit/scenarios.yaml`](.github/greenit/scenarios.yaml).

Chaque entree suit la meme structure simple :

```yaml
- name: "Nom de la page"
  url: "http://127.0.0.1:5173/ma-page"
  waitForSelector: ".mon-selecteur"
  waitForTimeout: 1500
  screenshot: "ma-page.png"
```

Regles pratiques :

- garder des pages statiques ou tres simples
- utiliser un `waitForSelector` stable present apres chargement
- eviter les parcours utilisateurs complexes dans cette CI
- reserver les analyses plus fines a des scenarios dedies si necessaire

### Commandes utiles

- `npm run analyze`
- `npm run lighthouse`
- Lighthouse dans le navigateur Chrome
- EcoIndex via l'extension ou le site dedie
