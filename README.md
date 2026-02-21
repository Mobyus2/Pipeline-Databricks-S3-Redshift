# 🔥 PIPELINE DU SUIVI DES VENTES
## *Du Streaming de Données à la Visualisation dans Databricks*

<div align="center">

```
╔══════════════════════════════════════════════════════════════════╗
║                                                                  ║
║      Amazon S3  ──►  Databricks  ──►  Bronze ► Silver ► Gold    ║
║                                            │                     ║
║                              Genie AI ◄────┘────► BI Dashboard  ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝
```

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![Amazon S3](https://img.shields.io/badge/Amazon%20S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)

**Auteur :** Francois Louis Marie NTONGA · Data Engineer Junior  
📧 francoislouismarie.contact@gmail.com · [LinkedIn](www.linkedin.com/in/francois-louis-marie-ntonga-7b982329b)

</div>

---

## 📌 Vue d'ensemble

> **Contexte :** Une entreprise FMCG (Fast-Moving Consumer Goods) vient d'acquérir une seconde société. Les données de ventes des deux entités sont éparpillées, hétérogènes, inexploitées. **Mission : les unifier, les transformer, les rendre décisionnelles — de A à Z.**

Ce projet conçoit et industrialise un **pipeline ETL bout-à-bout** en s'appuyant sur Databricks comme socle central. Le résultat ? Une plateforme Data Engineering complète, automatisée, scalable — où la donnée brute devient un actif stratégique exploitable par les métiers en quelques clics.

---

## 🏗️ Architecture Globale

```
┌─────────────────────────────────────────────────────────────────────┐
│                        SOURCE DE DONNÉES                           │
│                                                                     │
│   📁 customers/    📁 orders/    📁 products/    📁 gross_price/   │
│              └──────────────────────────────┘                       │
│                        Amazon S3 (Data Lake)                        │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                    IAM Role + External Location
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                      🧱 DATABRICKS LAKEHOUSE                        │
│                                                                     │
│  ┌─────────────┐    ┌─────────────┐    ┌──────────────────────┐   │
│  │   BRONZE    │───►│   SILVER    │───►│        GOLD          │   │
│  │  (Raw Data) │    │  (Cleaned)  │    │ (Star Schema / BI)   │   │
│  │             │    │             │    │                      │   │
│  │ • customers │    │ • customers │    │ • dim_customers      │   │
│  │ • orders    │    │ • orders    │    │ • dim_products       │   │
│  │ • products  │    │ • products  │    │ • dim_date           │   │
│  │ • gross_    │    │ • gross_    │    │ • dim_gross_price    │   │
│  │   price     │    │   price     │    │ • fact_orders        │   │
│  └─────────────┘    └─────────────┘    │ • vw_fact_orders_   │   │
│                                        │   enriched (VIEW)   │   │
│                                        └──────────────────────┘   │
│                                                 │                   │
│                              ┌──────────────────┴───────────┐      │
│                              │                              │      │
│                              ▼                              ▼      │
│                    ┌──────────────────┐        ┌────────────────┐  │
│                    │   🤖 Genie AI    │        │ 📊 BI Dashboard│  │
│                    │ (NL → SQL auto)  │        │ (Sales Insights│  │
│                    └──────────────────┘        └────────────────┘  │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Stack Technique

| Composant | Technologie | Rôle |
|---|---|---|
| 🗄️ **Data Lake** | Amazon S3 | Stockage des données sources (CSV) |
| ⚙️ **Compute** | Apache Spark | Traitement distribué & scalable |
| 🧠 **Plateforme** | Databricks | Orchestration, notebooks, jobs |
| 💾 **Format** | Delta Lake | Tables versionnées & ACID |
| 🗣️ **Langage** | Python & SQL | Transformation & modélisation |
| 🤖 **IA** | Databricks Genie | Interrogation en langage naturel |
| 📊 **Viz** | Databricks Dashboard | Restitution BI interactive |
| 🔐 **Auth** | AWS IAM Role | Connexion sécurisée S3 ↔ Databricks |

---

## 📂 Structure du Projet

```
fmcg-databricks-pipeline/
│
├── 📁 0_data/                                     # Données sources (→ Amazon S3)
│   ├── 📁 1_parent_company/                       # Société principale (acquérante)
│   │   ├── 📁 full_load/                          # Chargement initial complet
│   │   │   ├── dim_customers.csv
│   │   │   ├── dim_gross_price.csv
│   │   │   ├── dim_products.csv
│   │   │   └── fact_orders.csv
│   │   └── 📁 incremental_load/                   # Mises à jour quotidiennes
│   │       ├── fact_orders.csv
│   │       └── incremental_data_parent_company_query.txt
│   └── 📁 2_child_company/                        # Société acquise
│
├── 📁 1_codes/                                    # Notebooks Databricks
│   ├── 📁 1_setup/                                # Initialisation de l'environnement
│   │   ├── dim_date_table_creation.ipynb          # Génération de la dimension date
│   │   ├── setup_catalog.ipynb                    # Création catalog + schémas Bronze/Silver/Gold
│   │   └── utilities.ipynb                        # Fonctions utilitaires partagées
│   │
│   ├── 📁 2_dimension_data_processing/            # Traitement des dimensions (Silver → Gold)
│   │   ├── 1_customers_data_processing.ipynb      # Pipeline clients
│   │   ├── 2_products_data_processing.ipynb       # Pipeline produits
│   │   └── 3_pricing_data_processing.ipynb        # Pipeline prix
│   │
│   └── 📁 3_fact_data_processing/                 # Traitement de la table de faits
│       ├── 1_full_load_fact.ipynb                 # Chargement historique complet
│       └── 2_incremental_load_fact.ipynb          # Chargement incrémental quotidien
│
├── 📁 2_dashboarding/                             # Restitution & visualisation
│   ├── denormalise_table_query_fmcg.txt           # Requête SQL de la vue enrichie
│   └── fmcg_dashboard.pdf                        # Export du dashboard Sales Insights
│
├── 📁 resources/                                  # Documentation & design
│   ├── databricks_project.excalidraw             # Schéma d'architecture (éditable)
│   └── project_architecture.png                  # Schéma d'architecture (export)
│
├── Projet_DataBricks_Data_Ops.pdf                # Documentation complète du projet
└── README.md
```

---

## 🔄 Pipeline en 15 Étapes

### **PHASE 1 — Infrastructure & Connexion**

#### Étape 1 · Création du bucket Amazon S3
Mise en place du Data Lake S3 (`sportbar-bp`) avec une organisation par domaine métier :
```
s3://sportbar-bp/
    ├── customers/
    ├── orders/
    ├── products/
    └── gross_price/
```

#### Étape 2 · Liaison sécurisée Databricks ↔ S3
Configuration d'une **External Location** via un rôle IAM dédié — aucune clé exposée, accès en lecture/écriture natif.

#### Étape 3 · Validation de la connexion
Test complet des permissions : Read ✅ · Write ✅ · Delete ✅ · Assume Role ✅ · External ID ✅

#### Étape 4 · Création du Catalog & des schémas
```sql
CREATE CATALOG IF NOT EXISTS fmcg;
USE CATALOG fmcg;

CREATE SCHEMA IF NOT EXISTS fmcg.bronze;
CREATE SCHEMA IF NOT EXISTS fmcg.silver;
CREATE SCHEMA IF NOT EXISTS fmcg.gold;
```

---

### **PHASE 2 — Medallion Architecture**

#### Étape 5 · Lecture des données depuis S3 avec Spark
```python
basepath = f's3://sportbar-bp/{data_source}/*.csv'
df = spark.read.format("csv").load(basepath)
display(df.limit(10))
```

#### Étape 6 · Couche Bronze — Ingestion brute
Les données CSV sont chargées **telles quelles** dans Databricks — structure d'origine préservée, sans transformation métier.

```
fmcg.bronze
    ├── customers    (raw)
    ├── orders       (raw)
    ├── products     (raw)
    └── gross_price  (raw)
```

#### Étape 7 · Couche Silver — Nettoyage & structuration
Filtrage, typage correct, normalisation et enrichissement des données Bronze.

```
fmcg.silver
    ├── customers    (cleaned & typed)
    ├── orders       (cleaned & typed)
    ├── products     (cleaned & typed)
    └── gross_price  (cleaned & typed)
```

#### Étape 8 · Couche Gold — Modèle analytique en étoile

```
fmcg.gold
    ├── dim_customers          ← Dimension clients
    ├── dim_products           ← Dimension produits
    ├── dim_date               ← Dimension temporelle
    ├── dim_gross_price        ← Dimension prix
    ├── fact_orders            ← Table de faits centrale
    ├── sb_dim_customers       ← (société B)
    ├── sb_dim_products        ← (société B)
    ├── sb_dim_gross_price     ← (société B)
    ├── sb_fact_orders         ← (société B)
    └── vw_fact_orders_enriched ← Vue analytique unifiée
```

**Star Schema :**
```
         dim_customers
               │
dim_date ──── fact_orders ──── dim_products
               │
         dim_gross_price
```

#### Étape 9 · Vue analytique enrichie `vw_fact_orders_enriched`
```sql
CREATE OR REPLACE VIEW fmcg.gold.vw_fact_orders_enriched AS
SELECT
    fo.date,
    fo.product_code,
    fo.customer_code,
    -- Date attributes
    dd.date_key, dd.year, dd.month_name, dd.month_short_name, dd.quarter,
    dd.year_quarter,
    -- Customer attributes
    ...
FROM fmcg.gold.fact_orders fo
JOIN fmcg.gold.dim_date dd    ON fo.date = dd.date_key
JOIN fmcg.gold.dim_customers dc ON fo.customer_code = dc.customer_code
JOIN fmcg.gold.dim_products dp  ON fo.product_code = dp.product_code
```

---

### **PHASE 3 — Orchestration & Automatisation**

#### Étape 10 · Pipeline Databricks Jobs
Un workflow complet orchestre l'ensemble des traitements avec gestion des dépendances :

```
dim_processing_customer  ──┐
dim_processing_products  ──┼──► fact_processing_orders
dim_processing_prices    ──┘
```
> Les dimensions sont calculées **en parallèle** ; la table de faits attend leur complétion.

#### Étape 11 · Test manuel — `Run now`
Déclenchement manuel pour valider les dépendances et les performances.

#### Étape 12 · Monitoring en temps réel
Suivi visuel de chaque tâche : état, durée, erreurs, logs — directement dans l'interface Databricks Jobs.

#### Étape 13 · Planification automatique (Trigger)
```
Schedule: Every Day at 21:00 UTC
Trigger Status: ● Active
```
Le pipeline s'exécute désormais **de manière autonome**, sans intervention humaine.

---

### **PHASE 4 — Consommation & Visualisation**

#### Étape 14 · Genie IA — Interrogation en langage naturel
Un espace **Databricks Genie** connecté à `vw_fact_orders_enriched` permet d'explorer les données sans écrire une ligne de SQL :

```
💬 "Show me total revenues by quarter"
💬 "What are the top 5 customers by sold quantity?"
💬 "What is the monthly total sales amount over time?"
```

→ Genie génère automatiquement les requêtes SQL + visualisations (bar charts, histogrammes, courbes temporelles).

#### Étape 15 · Dashboard BI interactif — Sales Insights
Dashboard final déployé dans Databricks avec filtres dynamiques (Année / Trimestre / Mois / Canal / Catégorie).

---

## 📊 KPIs du Dashboard

### Performance Globale
| KPI | Valeur |
|---|---|
| 💰 Total Revenue | **105.34 B** |
| 📦 Total Quantity Sold | **34.13 M unités** |
| 👥 Clients uniques | **54** |
| 💲 Prix moyen de vente | **4 043.16** |

### Analyses disponibles
- 📈 **Monthly Revenue Trend** — Saisonnalité & pics (Q4 très fort)
- 🏆 **Top Products by Revenue** — Focus marketing & gestion stocks
- 🥇 **Top Customers by Revenue** — FitnessWorld · FastTrack Sports · Fitness Mania
- 🍰 **Revenue Share by Channel** — Retailer 78% · Direct 20%
- 📐 **Product Price vs Quantity** — Scatter plot prix/volume
- 🎯 **Top Variant by Revenue** — Large · 9kg · Curl Bar · Medium · Youth...

---

## 📈 Résultats & Valeur Métier

```
✅ Pipeline ETL automatisé et planifié quotidiennement
✅ Architecture Medallion Bronze → Silver → Gold opérationnelle
✅ Star Schema analytique pour requêtes SQL performantes
✅ Données de 2 sociétés consolidées en un modèle unifié
✅ Exploration IA en langage naturel via Genie
✅ Dashboard interactif accessible aux non-techniciens
✅ Pipeline exécuté avec succès (statut OK, toutes tâches ✓)
```

### Décisions métiers rendues possibles
- Suivi et pilotage du chiffre d'affaires en temps réel
- Analyse des performances par client, produit, canal et période
- Identification des produits et clients les plus rentables
- Analyse des tendances et saisonnalité des ventes
- Optimisation des stratégies commerciales et de distribution

---

## ⚙️ Pré-requis & Déploiement

### Pré-requis
- Compte Databricks (avec Unity Catalog activé)
- Bucket Amazon S3 configuré
- Rôle IAM avec permissions S3 (Read / Write / Delete / AssumeRole)
- Cluster Spark (ou Serverless Compute)

### Démarrage rapide

```bash
# 1. Cloner le repo
git clone https://github.com/francoislouismarie/fmcg-databricks-pipeline.git
```

**2. Uploader les données dans S3**
```
s3://sportbar-bp/
    ├── customers/     ← 0_data/1_parent_company/full_load/dim_customers.csv
    ├── products/      ← 0_data/1_parent_company/full_load/dim_products.csv
    ├── gross_price/   ← 0_data/1_parent_company/full_load/dim_gross_price.csv
    └── orders/        ← 0_data/1_parent_company/full_load/fact_orders.csv
```

**3. Configurer la connexion S3 ↔ Databricks**
```
Databricks > Catalog > External Locations > Add
→ Créer un Storage Credential (IAM Role)
→ Créer une External Location pointant vers s3://sportbar-bp/
→ Tester la connexion (Read / Write / Delete / AssumeRole ✅)
```

**4. Exécuter les notebooks dans l'ordre**
```
📁 1_codes/1_setup/
    1. setup_catalog.ipynb              → Crée le catalog fmcg + schémas Bronze/Silver/Gold
    2. utilities.ipynb                  → Charge les fonctions utilitaires partagées
    3. dim_date_table_creation.ipynb    → Génère la dimension date (Gold)

📁 1_codes/2_dimension_data_processing/
    4. 1_customers_data_processing.ipynb    → Bronze → Silver → Gold (dim_customers)
    5. 2_products_data_processing.ipynb     → Bronze → Silver → Gold (dim_products)
    6. 3_pricing_data_processing.ipynb      → Bronze → Silver → Gold (dim_gross_price)

📁 1_codes/3_fact_data_processing/
    7. 1_full_load_fact.ipynb           → Chargement historique complet (fact_orders)
    8. 2_incremental_load_fact.ipynb    → Chargement incrémental (exécution quotidienne)

📁 2_dashboarding/
    9. Exécuter denormalise_table_query_fmcg.txt → Crée vw_fact_orders_enriched
```

**5. Créer et planifier le Job Databricks**
```
Databricks > Jobs & Pipelines > Create Job
→ Ajouter les tâches avec dépendances (dimensions en parallèle → faits)
→ Configurer le trigger : Every Day at 21:00 UTC
→ Run now pour valider ✅
```

**6. Connecter Genie & ouvrir le Dashboard**
```
→ Genie : lier à fmcg.gold.vw_fact_orders_enriched
→ Dashboard : importer fmcg_dashboard depuis 2_dashboarding/
```

---

## 🧠 Ce que ce projet démontre

| Compétence | Mise en œuvre |
|---|---|
| **Data Lake Design** | Organisation S3 par domaine, nomenclature claire |
| **Spark & PySpark** | Lecture CSV distribuée, transformations DataFrames |
| **Architecture Medallion** | Bronze / Silver / Gold avec séparation des responsabilités |
| **Modélisation dimensionnelle** | Star Schema : dim_* + fact_orders |
| **Chargement incrémental** | Mise à jour des tables sans recalcul complet |
| **Orchestration** | Databricks Jobs avec graphe de dépendances |
| **Planification** | Trigger quotidien automatisé |
| **IA conversationnelle** | Genie connecté à la Gold layer |
| **Data Visualization** | Dashboard KPIs interactif |
| **Sécurité cloud** | IAM Role + External Location sans exposition de clés |

---

## 👤 Auteur

**Francois Louis Marie NTONGA**  
*Data Engineer Junior*

📧 [francoislouismarie.contact@gmail.com](mailto:francoislouismarie.contact@gmail.com)  
🔗 [LinkedIn](www.linkedin.com/in/francois-louis-marie-ntonga-7b982329b)

---

<div align="center">

*"La donnée n'a de valeur que si elle est accessible, fiable et exploitable."*

**⭐ Star ce projet s'il vous a inspiré !**

</div>
