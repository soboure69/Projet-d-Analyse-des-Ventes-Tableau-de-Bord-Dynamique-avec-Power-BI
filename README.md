# Projet d'Analyse des Ventes : Tableau de Bord Dynamique avec Power BI

Ce projet présente la création d'un tableau de bord complet et interactif avec Power BI, conçu pour analyser les performances d'une entreprise à travers trois axes principaux : **les boutiques, les produits et les ventes**. L'objectif est de transformer des données brutes en insights visuels et actionnables, permettant une prise de décision éclairée.

## Table des matières
1.  [Contexte du projet](#1-contexte-du-projet)
2.  [Objectifs](#2-objectifs)
3.  [Jeu de données](#3-jeu-de-données)
4.  [Processus de développement](#4-processus-de-développement)
    -   [4.1. Connexion et Transformation des données (ETL)](#41-connexion-et-transformation-des-données-etl)
    -   [4.2. Modélisation des données](#42-modélisation-des-données)
    -   [4.3. Création de mesures et colonnes calculées (DAX)](#43-création-de-mesures-et-colonnes-calculées-dax)
    -   [4.4. Visualisation et conception du rapport](#44-visualisation-et-conception-du-rapport)
5.  [Structure du tableau de bord](#5-structure-du-tableau-de-bord)
    -   [5.1. Page "Boutique"](#51-page-boutique)
    -   [5.2. Page "Produit"](#52-page-produit)
    -   [5.3. Page "Vente"](#53-page-vente)

## 1. Contexte du projet

Ce tableau de bord a été élaboré pour fournir une vue à 360 degrés des opérations commerciales d'une entreprise fictive. Il répond au besoin de centraliser les informations et de permettre aux dirigeants et aux analystes d'explorer les données de manière intuitive, de l'échelle macro (performance globale) à l'échelle micro (détails d'une vente spécifique).

## 2. Objectifs

*   **Centraliser** les données provenant de multiples fichiers CSV.
*   **Nettoyer et préparer** les données pour garantir leur fiabilité.
*   **Modéliser** les données en créant des relations pertinentes entre les différentes tables (faits et dimensions).
*   **Créer un rapport Power BI dynamique et interactif** composé de trois pages distinctes : Boutique, Produit et Vente.
*   **Offrir des insights** sur la performance des boutiques, la popularité des produits et les tendances des ventes.

## 3. Jeu de données

Le jeu de données est composé de plusieurs fichiers CSV, incluant :
*   **Tables de dimensions** : `Boutiques`, `Produits`, `Catégories de produits`, `Sous-catégories de produits`, et `Géographie`.
*   **Table de faits** : Données de ventes réparties dans plusieurs fichiers (par trimestre pour les années 2020 et 2021).

## 4. Processus de développement
<img width="2614" height="1150" alt="Vue de Modèle" src="https://github.com/user-attachments/assets/ded5d9ac-3507-474c-b0d2-408844a0d6e3" />

### 4.1. Connexion et Transformation des données (ETL)

La première étape a consisté à se connecter aux sources de données (fichiers CSV individuels et un dossier de ventes). **L'éditeur Power Query** a été utilisé pour les transformations suivantes :
*   **Combinaison de fichiers** : Fusion de tous les fichiers de ventes trimestriels en une seule table.
*   **Changement de types de données** : Conversion des identifiants numériques en texte pour éviter les agrégations non désirées et correction des types pour les champs monétaires (`prix unitaire`, `coût unitaire`).
*   **Gestion des valeurs manquantes** : Remplacement des valeurs vides dans la colonne `Close Reason` par "open" pour clarifier le statut des boutiques.
*   **Simplification du modèle** : Suppression de colonnes jugées redondantes ou inutiles pour optimiser les performances du rapport.
*   **Configuration des paramètres régionaux** : Passage des paramètres en "Anglais" pour interpréter correctement le format de date anglo-saxon (mois-jour-année) présent dans les fichiers sources.

### 4.2. Modélisation des données

Un modèle en étoile a été implicitement créé dans Power BI. La table des ventes (`Sales`) constitue la table de faits, reliée aux différentes tables de dimensions (`Produits`, `Boutiques`, `DateTable`, etc.). Une table de dates calculée a été ajoutée pour permettre des analyses temporelles avancées par année, mois et jour.

### 4.3. Création de mesures et colonnes calculées (DAX)

Plusieurs calculs ont été effectués avec le langage DAX pour enrichir le modèle :
*   **Colonnes calculées** dans la table des ventes : `Montant Vente`, `Remise` (basée sur une logique conditionnelle `IF`), et `Montant Vente Après Remise`.
*   **Mesures DAX** : Création de mesures agrégées pour les visualisations, comme `NB_boutique` (nombre de boutiques), `NB_Produit` (nombre de produits), `CA` (chiffre d'affaires total), et `CA-1` (chiffre d'affaires de l'année précédente avec `SAMEPERIODLASTYEAR`).

### 4.4. Visualisation et conception du rapport

Le rapport a été conçu pour être esthétique, intuitif et informatif, avec une mise en forme cohérente sur les trois pages. Il inclut un menu de navigation pour passer facilement d'une page à l'autre.

## 5. Structure du tableau de bord

Le rapport final est interactif et se compose de trois pages.

### 5.1. Page "Boutique"
<img width="1958" height="1120" alt="Page1" src="https://github.com/user-attachments/assets/583a22e6-8fef-46e4-9517-faac41423870" />

Cette page se concentre sur la performance géographique et opérationnelle des boutiques.
*   **Filtres** par continent et pays.
*   **Indicateurs clés (KPIs)** : Nombre total de boutiques et d'employés, mis à jour dynamiquement selon les filtres.
*   **Visualisations** :
    *   Nombre de boutiques par type (`Store`, `Online`, etc.) et par statut (`on`/`off`).
    *   Une **carte géographique** localisant le pays sélectionné.
    *   Un tableau de **détails sur les boutiques** (surface, employés, etc.).
    *   Un histogramme montrant le **nombre de ventes par boutique**.

### 5.2. Page "Produit"
<img width="1988" height="1124" alt="Page2" src="https://github.com/user-attachments/assets/a8cd1be5-6a7e-4dfc-a24d-db1b3f3a8aa3" />

Cette section permet une analyse approfondie du catalogue de produits.
*   **Filtres** par classe de produit (`Luxe`, `Économie`, `Regular`) et par marque.
*   **Indicateurs clés (KPIs)** : Nombre total de produits.
*   **Visualisations** :
    *   Nombre de produits par catégorie et sous-catégorie (via une arborescence de décomposition).
    *   Un graphique comparant les **quantités vendues et les quantités retournées** par produit, un indicateur crucial pour la gestion de la qualité et des stocks.
    *   Le **Top 3 des produits les plus vendus**.
    *   Un tableau de **détails sur les produits** (catégorie, prix unitaire, etc.).

### 5.3. Page "Vente"
<img width="2006" height="1096" alt="Page3" src="https://github.com/user-attachments/assets/eaf0c81c-a9ae-490f-b87c-3422013aa2bc" />

Cette page finale offre une vue d'ensemble et détaillée de la performance financière.
*   **Filtres** par année et par mois.
*   **Indicateurs clés (KPIs)** : Chiffre d'affaires, nombre de ventes.
*   **Visualisations** :
    *   **Répartition du chiffre d'affaires** par catégorie, marque et classe de produit (graphiques en secteur).
    *   Une **jauge comparant le chiffre d'affaires actuel à celui de l'année précédente** (`N-1`).
    *   Un tableau analysant la **performance mensuelle**, incluant l'écart (%) par rapport à N-1, avec des indicateurs visuels (icônes) et des **graphiques sparkline** montrant la tendance journalière.
    *   Un graphique linéaire montrant **l'évolution du chiffre d'affaires et du nombre de ventes par jour**.
    *   Un tableau de **détails sur chaque transaction** de vente.
