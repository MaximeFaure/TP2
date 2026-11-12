# TP 2 : analyse des accidents de la route avec PySpark

**Cours :** Fondements et Application de Big Data

Étude de cas : refaire l'analyse [*Big Data Analysis of Road Crash Data using PySpark with PySpark Tutorial*](https://medium.com/rakesh-nain/big-data-analysis-of-road-crash-data-using-pyspark-with-pyspark-tutorial-c78ff2c35588) de Rakesh Nain, avec les trois structures de données de PySpark : **RDD**, **DataFrame** et **SparkSQL**.

Tout est dans le notebook [`etude_cas_road_crash.ipynb`](etude_cas_road_crash.ipynb).

## Données

Les accidents de la route d'Australie-Méridionale de 2012 à 2019, publiés par le Department of Planning, Transport and Infrastructure (DPTI) : 16 fichiers CSV, 77 Mo au total.

- `crash_data/` : une ligne par accident (127 672 accidents) : lieu, date, victimes, gravité, alcool, drogues… ;
- `units_data/` : une ligne par véhicule impliqué (272 591 véhicules) : type, année, sexe et âge du conducteur, permis…

Les deux tables sont reliées par la colonne `REPORT_ID`. Les fichiers ne sont pas dans le dépôt : la première cellule du notebook les télécharge depuis le [dépôt GitHub de l'auteur](https://github.com/RakeshNain/Big-data-Analysis-of-Road-Crash-Data-using-PySpark-with-PySpark-Tutorial).

## Contenu

1. **RDD** : chargement, partitionnement personnalisé (véhicules immatriculés en Australie-Méridionale ou non), âge moyen des conducteurs par sexe, véhicules les plus anciens et les plus récents.
2. **DataFrames** : accidents graves à Adélaïde, accidents avec le plus de victimes, morts par type d'accident, victimes quand un conducteur n'a pas de permis (jointure des deux tables), gravité selon l'alcool et les drogues.
3. **Comparaison** : deux requêtes écrites avec les trois structures. Elles donnent exactement le même résultat, et on compare leurs temps d'exécution.

Chaque résultat publié dans l'article est vérifié dans le notebook par un `assert`.

## Résultats

- Tous les chiffres de l'article sont retrouvés.
- Les sorties de route contre un obstacle fixe sont le type d'accident le plus mortel (245 morts sur la période).
- La part d'accidents mortels passe de 0,42 % sans alcool ni drogues à 13,09 % avec les deux.
- DataFrame et SparkSQL sont 1,5 à 2 fois plus rapides que les RDD, parce que Spark optimise leurs requêtes et les exécute dans la JVM. Leurs temps sont très proches : ce sont deux façons d'écrire la même requête.
- Refaire l'étude a permis de corriger trois points de l'article :
  - le découpage des lignes avec `split(',')` décale les colonnes de 17 % des accidents ;
  - la colonne utilisée pour le partitionnement n'est pas celle annoncée ;
  - les trois versions de la requête sur les conducteurs sans permis ne calculent pas la même chose.

## Exécution

Python 3 et Java 17. Spark tourne en mode local, sans cluster.

```bash
pip install pyspark pandas matplotlib jupyter
jupyter lab etude_cas_road_crash.ipynb
```
