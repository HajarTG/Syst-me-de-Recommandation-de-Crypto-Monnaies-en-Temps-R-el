#  Système de Recommandation de Crypto-Monnaies en Temps Réel

Pipeline Big Data complet pour la recommandation intelligente de cryptomonnaies en temps réel. Le système collecte des données de marché via des APIs, les traite en streaming avec Apache Spark, applique des modèles de Machine Learning (MLP, LSTM, Random Forest) et visualise les résultats dans un dashboard Grafana interactif.



##  Fonctionnalités

-  **Collecte en Temps Réel** — Agrégation de données depuis CoinGecko, Binance et CoinMarketCap
-  **Ingestion par Kafka** — Messagerie distribuée avec topics dédiés par source de données
-  **Traitement Streaming** — Nettoyage et enrichissement des données avec PySpark Structured Streaming
-  **Prédiction ML** — Recommandations "À investir" / "Neutre" / "À éviter" via MLP, LSTM et Random Forest
-  **Stockage NoSQL** — Persistance dans Apache Cassandra avec modélisation optimisée
-  **Visualisation Grafana** — Dashboard temps réel avec alertes, tendances et métriques de performance
-  **Déploiement Docker** — Infrastructure complète conteneurisée via docker-compose

##  Architecture du Pipeline
<img width="1471" height="582" alt="image" src="https://github.com/user-attachments/assets/e045c00d-4f0a-409d-84b9-226023bac1e5" />

### Étapes détaillées :

| Étape | Technologie | Description |
|-------|-------------|-------------|
| **Collecte** | Python + APIs REST | Appels aux APIs CoinGecko, Binance, CoinMarketCap toutes les 60s |
| **Ingestion** | Apache Kafka | Publication sur `crypto_coingecko`, `crypto_binance`, `crypto_coinmarketcap` |
| **Traitement** | PySpark Streaming | Parsing JSON, nettoyage, ajout de timestamp, écriture dans Cassandra |
| **ML** | TensorFlow + scikit-learn | Prédiction en temps réel via MLP (modèle retenu) |
| **Stockage** | Apache Cassandra | Tables `raw_data`, `processed_data`, `predicted_results` |
| **Visualisation** | Grafana | Dashboard temps réel, alertes, top 5 cryptos recommandées |

##  Modèles de Machine Learning

Trois approches ont été implémentées et comparées :

| Modèle | Accuracy | Précision | Recall | AUC | Temps d'inférence |
|--------|----------|-----------|--------|-----|-------------------|
| **MLP** ⭐ | **98%** | **0.90** | **0.85** | **0.92** | ~30s |
| LSTM | 96% | 0.88 | 0.84 | 0.91 | ~2min |
| Random Forest | 83% | 0.88 | 0.81 | 0.90 | ~5s |

> **Le MLP a été retenu pour la production** grâce à son excellent compromis précision/rapidité et sa compatibilité avec Spark UDF.

##  Dashboard Grafana

- **Visualisation temps réel** : Flux continu de prix, volumes et prédictions (refresh 60s)
- **Top 5 cryptos recommandées** : Tri dynamique par score et volume
- **Alertes intelligentes** : Seuils de prix, variations anormales, prédictions récurrentes
- **Filtres dynamiques** : Par symbole, date, label, score de confiance
- **Métriques ML** : Accuracy, précision, rappel affichés en temps réel

## Structure du Projet

crypto-recommendation-system/
├── data/
│   └── aggregator.py          # Collecte des APIs
├── spark/
│   ├── consumer_spark.py      # Traitement streaming
│   └── predict.py             # Inférence ML
├── models/
│   ├── mlp_model.h5           # Modèle MLP (production)
│   ├── lstm_model.h5          # Modèle LSTM
│   └── random_forest.pkl      # Modèle Random Forest
├── grafana/
│   └── dashboards/            # Configurations des dashboards
├── docker-compose.yml         # Orchestration des services
├── requirements.txt           # Dépendances Python
└── README.md

##  Installation & Déploiement

### Prérequis
- Docker & Docker Compose
- Git

### Lancement rapide

```bash
# Cloner le repository
git clone https://github.com/TON_USERNAME/crypto-recommendation-system.git
cd crypto-recommendation-system

# Lancer l'infrastructure complète
docker-compose up -d

# Vérifier les services
docker-compose ps

