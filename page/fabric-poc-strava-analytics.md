## 2026 - Strava Data Analytics Pipeline (Microsoft Fabric)

---

### 📋 Présentation du Projet
Ce POC (Proof of Concept) met en place une plateforme de données moderne pour analyser la préparation sportive (Trail/Vélotaf). L'objectif est d'automatiser l'extraction des données de l'API Strava vers un environnement **Microsoft Fabric** en suivant l'architecture **Medallion**, tout en intégrant une couche d'optimisation **FinOps** pour opérer sur une capacité minimale (F2).

Le projet permet un suivi précis de la charge d'entraînement (VAM, dénivelé) et une gestion prédictive de l'usure du matériel via une approche BI conversationnelle.

---

### 🏗️ Architecture des Données
Le projet utilise une architecture **Medallion** pour garantir la qualité, la traçabilité et l'évolutivité des données.

#### 1. Couche Bronze (Raw)
* **Source** : API Strava (REST/OAuth2).
* **Format** : JSON brut (fichiers).
* **Organisation** : Stockage dans le Lakehouse (`Files/Bronze/Strava/`).
    * `Activities/` : Flux temporel des sorties (ingestion par batch).
    * `Gears/` : Snapshots quotidiens de l'état du matériel.

#### 2. Couche Silver (Staging)
* **Technologie** : PySpark / Delta Lake.
* **Traitement** : 
    * Nettoyage, typage explicite et dédoublonnage.
    * **Historisation SCD Type 2** : Suivi de l'usure du matériel (une nouvelle ligne est générée uniquement si la distance totale a évolué).
* **Tables Delta** : `stg_strava_activities`, `stg_strava_gears`.

#### 3. Couche Gold (Curated - Star Schema)
* **Modélisation** : Schéma en étoile (Star Schema).
* **Tables de Faits** : 
    * `fct_activities` : Métriques transformées (VAM, Allure) et catégorisation métier (`SportCategory`).
    * `fct_gears` : Journal d'usure permettant de tracer des courbes de tendance kilométrique.
* **Dimensions** : 
    * `dim_gears` : Référentiel matériel (Marque, Modèle, Nom, Statut).
    * `dim_calendar` : Calendrier technique avec étiquettes de navigation et clés de tri numériques.

---

### ⚖️ Expertise FinOps & Automatisation Azure
Pour optimiser les coûts sur une capacité **F2**, une stratégie d'automatisation avancée a été mise en place :

* **Azure PowerShell Runbooks** : Orchestration de l'allumage et de l'extinction automatique de la capacité Fabric (Start/Pause) corrélée au cycle de traitement des données et aux fenêtres de consultation utilisateur pour optimiser le ROI.
* **Analyse de Capacité** : Monitoring via *Fabric Capacity Metrics* pour gérer le **Bursting** et le **Smoothing** (lissage de la consommation interactive vs background).

---

### ⚙️ Orchestration & Performance
* **Séquençage des Notebooks** : Pipeline piloté par Fabric Data Factory avec gestion des dépendances et arrêt de la chaîne en cas d'erreur.
* **Optimisation de session (High Concurrency)** : Activation du partage de session Spark entre les notebooks pour éliminer les délais de "Cold Start".
* **Configuration Spark** : Utilisation de **Starter Pools (Small Nodes)** et Session Timeout réduit à **5 minutes** pour libérer les ressources CU immédiatement après exécution.

---

### 💎 Stratégie de Restitution & BI Conversationnelle

#### 1. Modèle Sémantique (SSOT)
Le modèle sémantique sert de **Single Source of Truth**. Les mesures DAX sont centralisées pour assurer la cohérence des KPIs.

#### 2. BI Conversationnelle (Data Agent)
* **IA & Copilot** : Implémentation d'un **Data Agent** permettant d'interroger le modèle en langage naturel.
* **System Instructions** : Injection de règles métier spécifiques (Logique de jointure matériel/activités, gestion des synonymes) pour guider l'IA dans l'analyse des performances sportives.

#### 3. Dashboards Analytiques
* **Analyse de Performance** : Suivi des tendances de vitesse, dénivelé et comparaison N vs N-1 via la `SportCategory`.
* **Monitoring Matériel** : Tableaux de bord d'usure avec alertes visuelles basées sur les seuils de sécurité (800km pour le Trail, 4000km pour le vélo).

---

### 📂 Structure des Notebooks

| ID | Nom | Couche | Description |
| :--- | :--- | :--- | :--- |
| **01** | `nb_strava_01_bronze` | Bronze | Gestion OAuth2, extraction activités et snapshots JSON. |
| **02** | `nb_strava_02_silver_activities` | Silver | Nettoyage et typage du flux d'activités sportives. |
| **02** | `nb_strava_02_silver_gears` | Silver | Unification des snapshots et historisation SCD Type 2. |
| **03** | `nb_strava_03_gold_activities` | Gold | Calculs métiers (VAM), SportCategory et préparation Power BI. |
| **04** | `nb_strava_04_gold_dim_calendar` | Gold | Génération du calendrier avec patterns ISO et clés de tri. |
| **05** | `nb_runbook_control` | DevOps | (PowerShell) Script de management de la capacité Azure. |

---

### 📈 Business Logic & KPIs

#### VAM (Vitesse Ascensionnelle Moyenne)
$$VAM = \frac{\text{Elevation Gain (m)}}{\text{Moving Time (h)}}$$

#### Sport Category Mapping (Spark SQL)
```python
df_with_categories = df_silver.withColumn(
    "SportCategory",
    F.when(F.col("SportType").isin("Run", "TrailRun"), "Running")
     .when(F.col("SportType").isin("Ride", "EBikeRide", "VirtualRide"), "Cycling")
     .otherwise("Others")
)
```

### 🚀 Installation et Configuration
1. **Azure Automation** : Configurer le Runbook PowerShell et l'identité managée pour piloter la ressource Fabric.
2. **API Strava** : Configurer les credentials sur le portail développeur (Client ID, Secret, Refresh Token).
3. **Fabric Lakehouse** : Importer les notebooks et configurer le Starter Pool en "Small".
4. **Modélisation** : Créer le modèle sémantique en **Direct Lake** pour bénéficier de la performance native sans latence de rafraîchissement.
5. **Data Agent** : Activer l'agent sur le modèle sémantique et configurer les instructions de navigation (jointures et synonymes).

---
**Auteur** : Yoann BETTON - All Rights Reserved  
**Projet** : POC Fabric Analytics & FinOps - 2026

---

### [2025 - WEC Data Analysis Tool project](/page/wec-analysis)

---

<div style="text-align: center">
  <p style="font-size:11px">&copy; 2026 Yoann Betton</p>
</div>