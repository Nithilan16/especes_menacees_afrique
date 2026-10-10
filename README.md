# Répartition et saisonnalité des observations d'espèces menacées en Afrique

Projet d'analyse de données portant sur cinq espèces menacées d'Afrique (éléphant d'Afrique, lion, léopard, guépard, rhinocéros noir), à partir de données d'occurrence issues de GBIF (Global Biodiversity Information Facility). Il cherche à savoir où ces espèces sont les plus observées, si les observations suivent une saisonnalité et si cette saisonnalité reflète un comportement biologique ou un biais d'observation.

## Questions posées

1. Où ces espèces sont-elles le plus fréquemment observées (par pays) ?
2. Existe-t-il une saisonnalité dans les observations, et varie-t-elle selon l'espèce ?
3. Cette saisonnalité reflète-t-elle un comportement biologique réel ou un biais d'observation (saison touristique) ?

## Données

Export d'occurrences GBIF (31 466 lignes brutes) couvrant 5 espèces : Éléphants d'Afrique (Loxodonta africana), Lion (Panthera leo), Léopard (Panthera pardus), Guépard (Acinonyx jubatus), Rhinocéros noir (Diceros bicornis) dans 6 pays : Afrique du Sud, Kenya, Tanzanie, Namibie, Botswana et Zimbabwe.

Le fichier n'est pas versionné dans ce repo. Pour reproduire l'analyse :
1. Sur [gbif.org](https://www.gbif.org/), rechercher les occurrences des 5 espèces mentionées ci-dessus, filtrées sur les 6 pays.
2. Télécharger l'export au format CSV tabulé.
3. Le renommer `especes_menacees.csv` et le placer dans `data/raw/`.
4. Exécuter le notebook.

Un nouvel export GBIF ne sera pas strictement identique à celui utilisé ici (les données évoluent). 
Citation de l'export d'origine : GBIF.org (9 May 2026) GBIF Occurrence Download https://doi.org/10.15468/dl.kd7w8n.

## Structure du repo

```
especes-menacees-afrique/
├── data/
│   └── raw/                  # export GBIF (non versionné)
├── notebooks/
│   └── projet_especes_menacees_afrique.ipynb
└── README.md
```

Le notebook a été développé sous Google Colab : [https://colab.research.google.com/drive/1XliTOvkF-0-Srnii-vacdUDq6t5SdHg_?usp=sharing].

## Méthodologie

1. **Informations sur la base** : structure, types, valeurs manquantes.
2. **Préparation et nettoyage** : sélection des colonnes utiles, suppression de `individualCount` (95,7 % de valeurs manquantes), suppression de 68 doublons, contrôle de `basisOfRecord` et de `coordinateUncertaintyInMeters`.
3. **Traitement des dates** : conversion et extraction de l'année et du mois.
4. **Lisibilité** : traduction des codes pays et des noms d'espèces.
5. **Répartition par espèce** 
6. **par pays**
7. **cartographie interactive** (Folium).

[Voir la carte interactive des espèces menacées en Afrique](https://nithilan16.github.io/especes_menacees_afrique/visualisation/carte_especes_afrique.html)

8. **Évolution annuelle**
9. **saisonnalité globale**
10. **saisonnalité par espèce** (normalisée en pourcentage du total annuel de chaque espèce).
11. **Conclusion et limites.**

## Résultats clés

- L'éléphant d'Afrique est l'espèce la plus représentée (13 394 observations), devant le lion (8 970), le guépard (3 864), le léopard (3 769) et le rhinocéros noir (1 401).
- L'Afrique du Sud concentre plus de 42 % des observations (13 322), devant le Kenya (6 198) et la Tanzanie (5 340). La Namibie (2 891) et le Botswana (2 862) sont au même niveau. Le Zimbabwe (785) est le moins représenté.
- Le trimestre juillet-septembre concentre environ un tiers des observations, avec le même profil saisonnier pour les cinq espèces malgré des écologies différentes. Cette synchronisation suggère un effet d'effort d'observation (saison sèche, haute saison touristique) plutôt qu'un comportement biologique propre à chaque espèce.
- Les observations augmentent nettement depuis 2022 (3 076, puis 4 381, puis 4 837 en 2024), après une chute en 2020 liée au COVID (1 139).
- Plus de 80 % des observations présentent une incertitude de localisation d'environ 28 à 31 km. Il ne s'agit pas d'une erreur, c'est le mécanisme de geoprivacy d'iNaturalist, qui masque la position réelle des espèces à statut de conservation menacé pour les protéger du braconnage.

## Points d'attention sur les données

- Dans le fichier brut, le code ISO de la Namibie (« NA ») est lu par pandas comme une valeur manquante. Le chargement utilise `keep_default_na=False` pour éviter que toute la Namibie soit supprimée silencieusement.
- `occurrenceID` n'étant plus disponible au moment de la déduplication, celle-ci repose sur une clé de substitution (espèce, date, latitude, longitude).

## Limites

- Le nombre d'observations mesure l'effort d'observation, pas l'abondance réelle des espèces : un faible effectif ne signifie donc pas une espèce plus rare.
- La carte interactive doit se lire comme une répartition par zone approximative et non comme un pointage précis, en raison de l'obscurcissement géographique des données d'iNaturalist.
- Pour le rhinocéros noir, la rareté réelle et l'obscurcissement des localisations se cumulent sans qu'il soit possible de les séparer avec ces seules données.
- Aucune donnée externe (fréquentation touristique, densité de contributeurs) n'est croisée pour confirmer le biais d'effort d'observation.
- Périmètre limité à cinq espèces et six pays.

## Outils utilisés

Python (pandas, matplotlib, seaborn, folium), Google Colab.