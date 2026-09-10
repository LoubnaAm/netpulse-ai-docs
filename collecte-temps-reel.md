---
title: "Collecte en temps réel"
description: "Comprendre le fonctionnement de la collecte réseau et du pipeline automatique."
---

# Collecte en temps réel

La collecte temps réel est la partie qui transforme l'application d'une simple démonstration sur CSV en plateforme alimentée par des données réseau.

## Vue simplifiée

```text
Machine / réseau
      ↓
     Zeek
      ↓
Flux réseau
      ↓
network_flows
      ↓
Prétraitement
      ↓
Modèles ML
      ↓
Mesures + prédictions
      ↓
Alertes
      ↓
Interface
```

## Étape 1 — Capture

Zeek observe les connexions réseau et produit des informations de flux.

SNMP fournit en parallèle des métriques réseau utiles au suivi de la charge.

---

## Étape 2 — Ingestion

Les flux peuvent être injectés dans la table :

```text
network_flows
```

Le composant d'ingestion doit être capable de lire les événements produits par Zeek et de les écrire dans la base.

---

## Étape 3 — Prétraitement

Les nouvelles données sont préparées pour être utilisées par les traitements Machine Learning.

Le projet contient notamment :

```text
ml/preprocess_realtime.py
```

---

## Étape 4 — Classification

Le job de classification récupère les flux qui doivent encore être classifiés, applique le modèle puis enregistre le résultat.

Le traitement est regroupé autour de :

```text
ml/run_classification_job.py
ml/classifier.py
```

Résultat principal :

```text
traffic_classifications
```

---

## Étape 5 — Saturation

Les métriques nécessaires à la saturation sont chargées puis le modèle produit une prédiction.

Composants principaux :

```text
ml/run_saturation_job.py
ml/saturation_predictor.py
```

Résultat principal :

```text
saturation_predictions
```

---

## Étape 6 — Anomalies

Le système calcule le score d'anomalie et détermine si l'observation mérite une alerte.

Composants associés :

```text
ml/anomaly_detector.py
ml/load_anomaly_observations.py
```

Résultat principal :

```text
anomaly_predictions
```

---

## Étape 7 — Mesures de supervision

Les flux sont agrégés pour produire des mesures temporelles utilisées par la page Supervision.

Le projet contient notamment :

```text
ml/refresh_network_measurements.py
ml/load_network_measurements.py
```

Résultat :

```text
network_measurements
```

---

## Étape 8 — Génération des alertes

Les différents signaux sont transformés en alertes lisibles par l'utilisateur.

Composant principal :

```text
ml/generate_alerts.py
```

Résultat :

```text
alerts
```

---

# Deux modes de collecte

## Mode `full`

La machine centrale peut exécuter l'ensemble de la chaîne :

```text
capture
→ ingestion
→ prétraitement
→ pipeline
```

## Mode `capture`

Une machine satellite se concentre sur la collecte.

La machine centrale garde la responsabilité du retraitement et de l'analyse.

Cela permet de répartir la collecte sur plusieurs machines.

---

# Cycle automatique

Le guide technique fourni avec le projet décrit un pipeline périodique d'environ **60 secondes** pour enchaîner les étapes de traitement.

L'idée générale est :

```text
Toutes les ~60 s
       ↓
Nouvelles données
       ↓
Mesures
       ↓
Saturation
       ↓
Anomalies
       ↓
Classification
       ↓
Agrégation supervision
       ↓
Alertes
```

Le délai exact dépend de la configuration et de l'environnement d'exécution.

---

# Vérifier que la collecte fonctionne

Quelques contrôles utiles :

### Vérifier Zeek

```bash
zeek --version
```

### Vérifier PostgreSQL

```bash
sudo systemctl status postgresql
```

### Vérifier le nombre de flux

```bash
sudo -u postgres psql -d netpulse \
  -c "SELECT COUNT(*) FROM network_flows;"
```

Le nombre doit augmenter lorsque de nouveaux flux sont collectés.

### Vérifier le pipeline

Consulter les logs associés au pipeline et à l'ingestion.

---

# Arrêt de la collecte

Le bouton **■ Arrêter la collecte** de l'application permet d'utiliser le mécanisme d'arrêt intégré au collecteur.

Pour un environnement de maintenance, les processus peuvent également être arrêtés selon les procédures du guide technique fourni avec le projet.
