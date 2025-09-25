# Computer Vision - Classification d'images Cats & Dogs

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org)
[![FAST Api](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Keras](https://img.shields.io/badge/Keras-%23D00000.svg?style=for-the-badge&logo=Keras&logoColor=white)](https://keras.io/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=for-the-badge)](CONTRIBUTING.md)

<div align="center">

<h3>Classification d'images avec Keras et exposition du modèle via Fast API</br></h3>

[Explore the docs](docs/)

</div>

---

## 📌 Introduction

Ce projet est à vocation pédagogique sur des thématiques IA et MLOps. Il permet de réaliser des tâches de Computer Vision et spécifiquement de la classification d'images par la reconnaissance de chats et de chiens.  
Il est composé de :

- Un modèle de computer vision développé avec Keras 3 selon une architecture CNN. Voir le tutoriel Keras ([lien](https://keras.io/examples/vision/image_classification_from_scratch/)).
- Un service API développé avec Fast API, qui permet notamment de réaliser les opérations d'inférence (i.e prédiction), sur la route `/api/predict`.
- Une application web minimaliste (templates Jinja2).
- Des tests automatisés minimalistes (pytest).
- Un pipeline CI/CD minimaliste (Github Action).
- Un système de logging des inférences et de collecte de feedback utilisateur via une base de données PostgreSQL.

## 🏗️ Architecture de l'application

```mermaid
graph TB
    subgraph "Data Layer"
        A[📁 Raw Data<br/>data/raw/] --> B[📁 Processed Data<br/>data/processed/]
    end
    
    subgraph "ML Pipeline"
        D[🧠 CNN Model<br/>Keras 3<br/>src/models/] 
        E[📊 Data Processing<br/>src/data/]
        F[📈 Monitoring<br/>src/monitoring/]
    end
    
    subgraph "Application Layer"
        G[🚀 FastAPI Server<br/>src/api/]
        H[🌐 Web Interface<br/>src/web/<br/>Jinja2 Templates]
        I[🔧 Utils<br/>src/utils/]
        R[🎯 API Endpoints<br/>/api/predict<br/>/api/feedback]
    end
    
    subgraph "DevOps & Infrastructure"
        K[⚙️ CI/CD<br/>.github/workflows/]
        L[📋 Scripts<br/>scripts/]
        M[🧪 Tests<br/>tests/<br/>pytest]
        S[🐘 PostgreSQL DB<br/>Logs & Feedbacks]
    end
    
    subgraph "Configuration & Documentation"
        N[⚙️ Config<br/>config/]
        O[📚 Documentation<br/>docs/]
        Q[📦 Requirements<br/>requirements/]    
    end
    
    subgraph "Monitoring & MLOps"
        F[📈 Monitoring Scripts<br/>src/monitoring/]
        T[📊 Grafana<br/>Dashboard & Alerting]
    end

    %% Data & Model Flow
    B --> E
    E --> D
    D --> G
    
    %% Application Flow
    G --> H
    G --> R
    H -- "User sends image" --> R
    R -- "Prediction" --> H
    H -- "User gives feedback" --> R
    
    %% Monitoring & Feedback Loop
    R -- "Logs inference & feedback" --> S
    S -- "Data source" --> T
    T -- "Alerts (e.g., email)" --> U((👤 Developper))
    
    %% Configuration
    N --> G
    N --> D
    Q --> G
    Q --> D
    
    %% Documentation & Development
    O --> H
    
    %% Styling
    classDef dataClass fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef mlClass fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef appClass fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef devopsClass fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef configClass fill:#fafafa,stroke:#424242,stroke-width:2px
    classDef monitoringClass fill:#ede7f6,stroke:#512da8,stroke-width:2px
    
    class A,B,C dataClass
    class D,E,F mlClass
    class G,H,I,R appClass
    class K,L,M devopsClass
    class N,O,Q configClass
    class S,T,U monitoringClass
    class F monitoringClass
```

## 📁 Structure du projet

```txt
project-name/
├── .github/
│   ├── workflows/           # CI/CD pipelines
│   └── ISSUE_TEMPLATE/      # Templates d'issues
├── config/                  # Fichiers de configuration
├── data/
│   ├── raw/                 # Données brutes (gitignored)
│   ├── processed/           # Données traitées (gitignored)
│   └── external/            # Données externes/références
├── docker/                  # Dockerfiles et compose
├── docs/                    # Documentation
├── notebooks/               # Jupyter notebooks pour exploration
├── requirements/            # Dépendances par environnement
│   ├── base.txt
│   ├── dev.txt
│   └── prod.txt
├── scripts/                 # Scripts d'automatisation/déploiement
├── src/                     # Code source principal
│   ├── api/                 # APIs et services web
│   ├── data/                # Scripts de traitement des données
│   ├── models/              # Modèles ML/IA
│   ├── monitoring/          # Monitoring des modèles
│   ├── utils/               # Utilitaires partagés
│   └── web/                 # Templates jinja2
├── tests/                   # Tests unitaires et d'intégration
├── .env.example             # Variables d'environnement exemple
├── .gitignore
├── README.md
├── Makefile                 # Commandes fréquentes
└── pyproject.toml           # Configuration Python/packaging
```

## 🛠️ Commandes utiles

*Section minimaliste à faire évoluer.*

```bash
make env           # Installer les dépendances dans un environnement virtuel
```

## 🎯 API

Lorsque l'environnement virtuel est activé, vous pouvez lancer le serveur de l'API ...

```bash
python scripts/run_api.py
```

... et visiter la page de documentation Swagger :

![Swagger](/docs/img/swagger.png "Page de documentation de l'API")

## 📊 Application web

Lorsque l'environnement virtuel est activé, vous pouvez lancer le serveur de l'API ...

```bash
python scripts/run_api.py
```

... et utiliser l'application web :

![Web APP](/docs/img/web.png "Application web du projet")

## Monitoring (Grafana)

Pour visualiser les métriques de l'application en temps réel (temps d'inférence, feedbacks, etc.), un dashboard Grafana est mis à disposition.

### Configuration du Dashboard

La configuration du dashboard est disponible via un fichier JSON, dans `config/Cats and Dogs app Dashboard.json`. Vous pouvez l'importer dans votre instance Grafana pour visualiser les métriques.

### Mise en Place

#### Installation (installation de Grafana en local)

1.  **Installer Grafana** : Suivez les instructions officielles pour votre système d'exploitation.
2.  **Démarrer Grafana** : Lancez le serveur Grafana (il sera accessible sur `http://localhost:3000`, login: `admin`, pass: `admin`).
3.  **Ajouter la source de données** :
    *   Allez dans `Connections` -> `Data sources` -> `Add new data source`.
    *   Choisissez `PostgreSQL`.
    *   Configurez la connexion à votre base de données (même configuration que celles de l'API, que vous avez configuré dans le fichier `.env`).
4.  **Importer le Dashboard** :
    *   Allez dans `Dashboards` -> `New` -> `Import`.
    *   Uploadez le fichier `dashboard.json` fourni dans le projet.
    *   Sélectionnez la source de données PostgreSQL que vous venez de créer.

### Aperçu du Dashboard

![Dashboard Grafana](/docs/img/Grafana-dashboard.png)

## Licence

MIT - voir LICENSE pour plus de détails.
