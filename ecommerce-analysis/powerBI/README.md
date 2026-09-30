# Dashboard Power BI - performance logistique

## Objectif

Suivre les retards de livraison et repérer les zones et conditions opérationnelles qui les accompagnent. Le rapport utilise la table préparée `data/processed/logistique_base.csv` (110 181 lignes selon le bilan du projet).

## Import et modèle

1. Dans Power BI Desktop, importer `data/processed/logistique_base.csv` avec **Obtenir les données > Texte/CSV**.
2. Renommer la table en `Logistique`.
3. Vérifier les types dans Power Query : identifiants en texte; `est_en_retard`, `meme_etat` et `anomalie_multivariee` en nombre entier; `mois_achat` en nombre entier; distances, durées, coordonnées, volume et poids en nombre décimal.
4. Charger la table sans créer de relation. Le fichier est le grain de fait du rapport; les mesures comptent les `order_id` distincts.
5. Créer les mesures du fichier `mesures.dax` (Modélisation > Nouvelle mesure), puis appliquer les formats indiqués ci-dessous.
6. Importer le thème par **Affichage > Thèmes > Parcourir les thèmes** et sélectionner `theme.json`.

## Page 1 - Vue d'ensemble

Format 16:9, titre `Livraisons | Vue d'ensemble`, arrière-plan clair.

| Zone             | Visuel                | Champs / mesures                                                                                                                               |
| ---------------- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Bande supérieure | 5 cartes KPI          | `[Commandes]`, `[Commandes en retard]`, `[Taux de retard]`, `[Delai moyen de livraison (jours)]`, `[Distance moyenne (km)]`                    |
| Bas gauche       | Courbe avec marqueurs | Axe : `mois_achat`; Valeurs : `[Taux de retard]`; ordre croissant de 1 à 12                                                                    |
| Bas centre       | Barres horizontales   | Axe : `customer_state`; Valeurs : `[Taux de retard]`; info-bulle : `[Commandes]`; filtre visuel `[Commandes] >= 100`; tri décroissant par taux |
| Bas droite       | Barres empilées       | Axe : `seller_state`; Légende : `est_en_retard`; Valeurs : `[Commandes]`; filtre visuel Top 10 par `[Commandes en retard]`                     |
| Rail de filtres  | Segmentations         | `mois_achat`, `customer_state`, `seller_state`, `meme_etat`                                                                                    |

## Page 2 - Facteurs opérationnels

Titre `Livraisons | Zones et opérations`.

| Zone            | Visuel                           | Champs / mesures                                                                                                                          |
| --------------- | -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Haut gauche     | Barres horizontales              | Axe : `customer_state`; Valeurs : `[Taux de retard]`; info-bulle : `[Commandes]` et `[Retard moyen (jours)]`; filtre `[Commandes] >= 100` |
| Haut droite     | 2 graphiques en colonnes         | Axe : `mois_achat`; un visuel pour `[Delai moyen avant expedition (h)]`, l'autre pour `[Delai moyen de livraison (jours)]`                |
| Bas gauche      | Colonnes par tranche de distance | Créer des tranches de 100 km à partir de `distance_km`; Axe : tranche; Valeurs : `[Taux de retard]`; info-bulle : `[Commandes]`           |
| Bas droite      | Matrice                          | Lignes : `seller_state`; Colonnes : `meme_etat`; Valeurs : `[Commandes]`, `[Taux de retard]`, `[Retard moyen (jours)]`                    |
| Rail de filtres | Segmentations                    | `mois_achat`, `customer_state`, `seller_state`, `anomalie_multivariee`                                                                    |

## Lecture des KPI

- Le taux de retard est le nombre de commandes avec `est_en_retard = 1` divisé par le nombre de commandes dans le contexte de filtre courant.
- Les durées moyennes et la distance se recalculent selon les filtres et les segments sélectionnés.
- `retard_jours` est moyenné uniquement pour les commandes signalées en retard.
- `mois_achat` ne contient pas l'année. La courbe de la page 1 compare donc les mois de l'année en agrégeant toutes les années; ce n'est pas une tendance chronologique. Pour une vraie tendance temporelle, ajouter une date d'achat depuis `orders_clean.csv` et une table calendrier reliée à `order_id`.
- La table préparée ne contient ni chiffre d'affaires, ni paiement, ni nom de catégorie. Ne pas présenter de KPI de ventes ou de catégories à partir de cette table.
- Les lignes marquées `anomalie_multivariee = 1` sont conservées dans les données; le filtre de la page 2 permet de les examiner, pas de les supprimer par défaut.

## Formats conseillés

`[Taux de retard]` et `[Part meme etat]` : pourcentage avec 1 décimale. Durées : 1 décimale. Distance : `#,##0 km`. Commandes : entier avec séparateur de milliers. Utiliser `est_en_retard` comme catégorie 0/1 dans les légendes; renommer les libellés d'affichage en `À l'heure` et `En retard` si souhaité.
