---
title: "Architecture"
description: "Comprendre simplement comment les composants de NETPULSE AI communiquent."
---

# Architecture

NETPULSE AI peut être compris comme une chaîne en plusieurs niveaux.

## 1. Source réseau

La première étape consiste à observer le réseau.

Deux mécanismes sont utilisés dans le projet :

- **Zeek** pour les informations liées aux flux et connexions ;
- **SNMP** pour certaines métriques réseau.

Les machines satellites peuvent participer à la collecte lorsque l'architecture multi-machine est utilisée.

---

## 2. Collecte

Le composant `services/collector.py` orchestre la collecte depuis l'application.

Selon le mode choisi, le collecteur peut gérer :

```text
1. capture Zeek
2. transfert / insertion des données
3. prétraitement temps réel
4. pipeline d'analyse
```

Deux modes sont prévus :

- **full** : capture + retraitement ;
- **capture** : la machine capture les données et le retraitement reste géré par la machine centrale.

---

## 3. Stockage

Les données peuvent être conservées dans PostgreSQL / TimescaleDB.

Le projet définit notamment les tables suivantes :

| Table | Rôle |
|---|---|
| `network_flows` | flux réseau observés |
| `traffic_classifications` | résultats de classification |
| `saturation_measurements` | mesures utilisées pour la saturation |
| `saturation_predictions` | prédictions de saturation |
| `anomaly_predictions` | résultats de détection d'anomalies |
| `network_measurements` | mesures agrégées pour la supervision |
| `alerts` | alertes produites par le système |

---

## 4. Préparation des données

Avant l'inférence, les données sont transformées selon les besoins de chaque modèle.

Cette étape peut comprendre :

- nettoyage ;
- sélection des variables ;
- conversion des types ;
- gestion des valeurs manquantes ;
- encodage de certaines variables ;
- création de variables dérivées.

L'objectif est de fournir au modèle des données dans le format attendu.

---

## 5. Couche Machine Learning

Le projet contient trois briques principales.

### ML/01 — Classification

Fichier principal :

```text
ml/classifier.py
```

Entrée :

```text
caractéristiques d'un flux
```

Sortie :

```text
classe de trafic
```

---

### ML/02 — Détection d'anomalies

Fichier principal :

```text
ml/anomaly_detector.py
```

Entrée :

```text
observations réseau
```

Sortie :

```text
score d'anomalie
+ indicateur d'alerte
+ niveau de risque
```

---

### ML/03 — Saturation

Fichier principal :

```text
ml/saturation_predictor.py
```

Entrée :

```text
historique et caractéristiques du trafic
```

Sortie :

```text
débit prédit
+ probabilité d'alerte
+ niveau de risque
```

---

## 6. Interface

L'application est construite avec **Streamlit**.

Le fichier principal est :

```text
app.py
```

Il organise les pages :

```text
Supervision
Alertes
Applications
Anomalies
Saturation
```

Les fonctions de lecture des données sont regroupées dans :

```text
data/source.py
```

Cela permet aux pages de travailler avec une interface de données commune, que la source soit locale ou connectée à la base.

---

## Vue d'ensemble

```text
                    ┌──────────────────┐
                    │   Réseau réel    │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │ Zeek / SNMP      │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │ Collecte         │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │ PostgreSQL /     │
                    │ TimescaleDB      │
                    └────────┬─────────┘
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
   ┌──────▼───────┐  ┌──────▼───────┐  ┌──────▼───────┐
   │ Classification│  │ Anomalies    │  │ Saturation   │
   │ ML/01         │  │ ML/02        │  │ ML/03        │
   └──────┬───────┘  └──────┬───────┘  └──────┬───────┘
          │                  │                  │
          └──────────────────┼──────────────────┘
                             │
                    ┌────────▼─────────┐
                    │ Résultats /      │
                    │ Alertes           │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │ Streamlit        │
                    │ NETPULSE AI      │
                    └──────────────────┘
```

## Principe important

L'interface ne reconstruit pas toute la logique ML.

Elle consomme des fonctions de la couche `data/source.py`, ce qui permet de séparer :

```text
Interface
   ≠
Accès aux données
   ≠
Modèles ML
   ≠
Collecte
```

Cette séparation rend le projet plus facile à maintenir.
