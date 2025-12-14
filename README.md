#  Prédiction des Résultats de Ligue 1 (Saison 2023)

   

Ce projet de Machine Learning vise à construire un modèle de capable de prédire l'issue des matchs de football de la Ligue 1 pour la saison 2023. Le problème est traité comme une classification multi-classes : **Victoire Domicile (1)**, **Nul (0)**, **Victoire Extérieur (-1)**.

##  Auteurs

  * **Matteo DUPIN**
  * **Ayoub EL AMRANI**
  * **Omar EL BARBARI**

-----

##  Approche Méthodologique

Ce projet ne se limite pas à l'application d'algorithmes, mais repose sur une démarche structurée de Data Science en quatre étapes clés :

1.  **Analyse Exploratoire (EDA) :** Étude approfondie de la distribution des cibles (mise en évidence du biais "Avantage Domicile") et des variables explicatives pour orienter les stratégies de nettoyage.
2.  **Intégrité des Données (Data Integrity) :** Mise en place d'un protocole strict de prévention des fuites de données (*Data Leakage*). Toutes les informations futures (buts, statistiques mi-temps, managers post-match) sont exclues pour garantir le réalisme temporel des prédictions.
3.  **Feature Engineering :** Construction de variables "métier" pour capturer la complexité du football : contexte économique (valeur marchande), dynamique sportive (forme récente) et infrastructures (stade).
4.  **Stratégie de Modélisation :** Entraînement et comparaison systématique de plusieurs familles d'algorithmes (Linéaires, Bagging, Boosting), évalués selon la métrique **F1-Macro** pour gérer le déséquilibre des classes (notamment les matchs nuls).

-----

##  Jeux de Données

Le projet s'appuie sur l'agrégation de plusieurs sources de données couvrant la période 2013-2023 :

| Fichier | Description |
| :--- | :--- |
| `matchs_2013_2022.csv` | Historique des résultats, scores et dates. |
| `player_valuation...csv` | Valeurs marchandes des joueurs (source Transfermarkt). |
| `player_appearance.csv` | Feuilles de matchs (joueurs alignés). |
| `clubs_fr.csv` | Informations sur les infrastructures (capacité stade). |
| `match_2023.csv` | Calendrier de la saison cible à prédire. |

-----

##  Feature Engineering

Nous avons construit **14 variables explicatives** réparties en trois axes d'analyse :

  * ** Contexte & Public :**
      * `attendance_log` : Transformation logarithmique de l'affluence pour normaliser les écarts entre petits et grands stades.
      * `stadium_capacity` : Capacité théorique du stade.
  * ** Puissance Économique :**
      * `value_diff` : Différence absolue de la valeur marchande des effectifs (*Home - Away*).
      * `value_ratio` : Ratio financier entre les deux équipes.
  * **📈 Dynamique Sportive :**
      * `home_form5` / `away_form5` : Moyenne des points pris sur les 5 derniers matchs (calculée via une fenêtre glissante temporelle stricte).

-----

##  Modélisation et Résultats

Nous avons utilisé un **Split Temporel** (Train \< 2022, Test = 2022) pour valider nos modèles. Voici la synthèse des performances :

| Modèle | Accuracy | F1-Macro | Observation |
| :--- | :---: | :---: | :--- |
| **Random Forest (GridSearch)** | 0.489 | **0.440** | 🏆 **Meilleur Modèle (Retenu)** |
| Régression Logistique (Grid) | 0.497 | 0.427 | Difficulté à capturer la non-linéarité |
| XGBoost (Grid) | **0.511** | 0.399 | Sur-apprentissage de la classe majoritaire |

> **Note :** Le Random Forest optimisé offre le meilleur compromis, améliorant significativement la détection des matchs nuls par rapport aux autres modèles qui tendent à prédire majoritairement la victoire à domicile.

-----

##  Installation et Utilisation

### Pré-requis

Assurez-vous d'avoir Python 3.10+ installé.

1.  **Cloner le dépôt :**

    ```bash
    git clone https://github.com/votre-username/prediction-ligue1.git
    cd prediction-ligue1
    ```

2.  **Installer les dépendances :**

    
    ``` pip install -r requirements.txt ```
    

3.  **Lancer le Notebook :**
    Ouvrez `Projet_Foot_ameliorations.ipynb` dans Jupyter Notebook ou VS Code pour exécuter le pipeline complet.

4.  **Résultats :**
    Le fichier de prédictions finales pour la saison 2023 sera généré sous le nom `predictions_2023.csv`.

-----

##  Structure du Projet

```
.
├── data/                       # Fichiers CSV sources
├── images/                     # Graphiques (Matrices de confusion, Feature Importance)
├── Projet_Foot_ameliorations.ipynb  # Code principal (Nettoyage, Training, Prédiction)
├── predictions_2023.csv        # Output final
├── requirements.txt            # Liste des librairies
└── README.md                   # Documentation
```

