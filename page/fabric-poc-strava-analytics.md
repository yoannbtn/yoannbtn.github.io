## 2026 - Strava Data Analytics Pipeline (Microsoft Fabric)

---


### 📋 Présentation du Projet
Ce POC (Proof of Concept) met en place une plateforme de données moderne pour analyser la préparation sportive (Trail/Vélotaf). L'objectif est d'automatiser l'extraction des données de l'API Strava vers un environnement **Microsoft Fabric** en suivant l'architecture **Medallion**.

Le projet permet un suivi précis de la charge d'entraînement (VAM, dénivelé) et une gestion prédictive de l'usure du matériel (suivi kilométrique des chaussures et vélos).

---

### 🏗️ Architecture des Données
Le projet utilise une architecture **Medallion** pour garantir la qualité, la traçabilité et l'évolutivité des données.

#### 1. Couche Bronze (Raw)
* **Source** : API Strava (REST).
* **Format** : JSON brut (fichiers).
* **Organisation** : Stockage dans le Lakehouse (`Files/Bronze/Strava/`).
    * `Activities/` : Flux temporel des sorties (ingestion par batch).
    * `Gears/` : Snapshots quotidiens de l'état du matériel.

#### 2. Couche Silver (Staging)
* **Technologie** : PySpark / Delta Lake.
* **Traitement** : 
    * Nettoyage, typage explicite et dédoublonnage.
    * Historisation de l'usure du matériel : une nouvelle ligne est générée uniquement si la distance totale a évolué (logique de versioning).
* **Tables Delta** : `stg_strava_activities`, `stg_strava_gears`.

#### 3. Couche Gold (Curated - Star Schema)
* **Modélisation** : Schéma en étoile (Star Schema) optimisé pour le mode **Direct Lake** de Power BI.
* **Tables de Faits** : 
    * `fct_activities` : Métriques transformées (VAM, Allure), catégorisation métier (`SportCategory`) et snapshots dynamiques.
    * `fct_gears` : Journal d'usure permettant de tracer des courbes de tendance kilométrique.
* **Dimensions** : 
    * `dim_gears` : Référentiel matériel (Marque, Modèle, Nom, Statut).
    * `dim_calendar` : Calendrier technique avec étiquettes de navigation (`W14 26`, `Q1 24`) et clés de tri numériques (`WeekKey`, `MonthKey`).

---

### ⚙️ Orchestration & Automatisation
L'ensemble du flux est piloté par un **Data Pipeline Fabric** garantissant l'intégrité et la fraîcheur des données.

* **Séquençage des Notebooks** : Le pipeline orchestre l'exécution séquentielle (Bronze ➡️ Silver ➡️ Gold) avec une gestion des dépendances : l'échec d'une étape stoppe la chaîne pour éviter toute corruption des données Gold.
* **Optimisation de session (High Concurrency)** : Activation du mode de haute disponibilité permettant aux notebooks de partager la même session Spark. Cela réduit le temps de cycle total en éliminant les délais de "Cold Start" entre chaque couche.
* **Scheduling** : Automatisation des exécutions (ex: 2 fois par jour) pour refléter les dernières activités Strava et l'état des composants matériels.
* **Monitoring** : Suivi centralisé des exécutions via le hub de surveillance Fabric pour identifier rapidement d'éventuels échecs d'API ou de quota.

---

### 🛠️ Stack Technique & Optimisation
* **Orchestration** : Fabric Data Factory (Pipelines).
* **Stockage** : OneLake (Microsoft Fabric).
* **Langage de traitement** : PySpark (Spark 3.4+).
* **Optimisation Capacité F2** : 
    * Utilisation de **Starter Pools (Small Nodes)** pour limiter la consommation de CU.
    * Session Timeout réduit à **5 minutes** pour libérer les ressources CU après exécution.
* **Visualisation** : Power BI (Direct Lake & Semantic Model).

---

### 💎 Stratégie de Restitution & BI

#### 1. Modèle Sémantique (SSOT)
Le modèle sémantique Fabric sert de **Single Source of Truth** (SSOT). Les mesures DAX sont centralisées et le mode **Direct Lake** garantit une performance maximale sans latence de rafraîchissement.

#### 2. Dashboards Analytiques Pré-construits
* **Analyse de Performance** : Suivi des tendances de vitesse, de dénivelé et de charge d'entraînement.
* **Analyse des Activités** : Vue détaillée par `SportCategory` avec comparaison N vs N-1.
* **Monitoring Matériel** : Tableaux de bord d'usure avec alertes visuelles basées sur les seuils de sécurité.

#### 3. Self-Service BI & Exploration Libre
* **Exploration Ad-hoc** : Fonctionnalité "Explore" de Fabric pour une analyse rapide des tables Gold.
* **IA & Copilot** : Interrogation du modèle en langage naturel pour générer des analyses à la volée.

---

### 📂 Structure des Notebooks

| ID | Nom | Couche | Description |
| :--- | :--- | :--- | :--- |
| **01** | `nb_strava_01_bronze` | Bronze | Gestion OAuth2, extraction activités et snapshots Gears. |
| **02** | `nb_strava_02_silver_activities` | Silver | Nettoyage et typage du flux d'activités sportives. |
| **02** | `nb_strava_02_silver_gears` | Silver | Unification des snapshots et gestion des versions d'usure. |
| **03** | `nb_strava_03_gold_activities` | Gold | Calculs Trail (VAM), Catégories et cast Date pour Power BI. |
| **04** | `nb_strava_04_gold_dim_calendar` | Gold | Génération du calendrier avec patterns ISO et clés de tri. |

---

### 📈 Business Logic & KPIs

#### VAM (Vitesse Ascensionnelle Moyenne)
$$VAM = \frac{\text{Elevation Gain (m)}}{\text{Moving Time (h)}}$$

#### Suivi d'usure (Gear Wear)
- **Seuil Trail** : Alerte de renouvellement à 800 km.
- **Seuil Vélotaf** : Planification révision à 4000 km.

---

### 🚀 Installation et Configuration
1. **API Strava** : Configurer les credentials sur le portail développeur.
2. **Fabric** : Importer les notebooks et configurer le Starter Pool en "Small".
3. **Pipeline** : Créer le pipeline d'orchestration et activer le "High Concurrency".
4. **Modélisation** : Créer les relations dans le modèle sémantique et appliquer le tri par colonne.

---
**Auteur** : Yoann BETTON - All Rights Reserved  
**Projet** : POC Fabric Analytics - 2026

---

### [2025 - WEC Data Analysis Tool project](/page/wec-analysis)

---

<div style="text-align: center">
  <p style="font-size:11px">&copy; 2026 Yoann Betton</p>
</div>