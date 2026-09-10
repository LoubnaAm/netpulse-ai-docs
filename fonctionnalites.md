---
title: "Fonctionnalités"
description: "Vue simple des fonctionnalités offertes par NETPULSE AI."
---

# Fonctionnalités

NETPULSE AI est organisé autour de **cinq espaces principaux**.

## 1. Supervision

La page **Supervision** donne une vue globale de l'état du trafic.

Elle permet notamment de suivre :

- le débit réseau ;
- le nombre de flux observés ;
- l'activité des sources ;
- le nombre de mesures collectées ;
- l'évolution du débit dans le temps ;
- les sources qui génèrent le plus de volume.

### Pourquoi ?

Cette page répond à la question :

> **« Que se passe-t-il actuellement sur le réseau ? »**

---

## 2. Applications

La page **Applications** s'intéresse à la classification du trafic.

Le système prend les caractéristiques disponibles d’un flux et utilise le modèle de classification pour déterminer sa **classe de trafic**.

Cela permet d’obtenir :

- le nombre de flux classifiés ;
- la classe dominante ;
- la répartition des classes ;
- l'évolution des classes dans le temps ;
- une vue des flux récemment observés.

### Pourquoi ?

Au lieu de voir seulement une suite de connexions, l'utilisateur obtient une lecture plus compréhensible du trafic.

---

## 3. Anomalies

La page **Anomalies** cherche des comportements qui s'écartent du fonctionnement attendu.

Le modèle produit notamment :

- un score d'anomalie ;
- un indicateur d'alerte ;
- un niveau de risque ;
- des indicateurs sur les sources suspectes.

### Pourquoi ?

Cette fonctionnalité sert à répondre à :

> **« Est-ce qu’un comportement inhabituel est en train d’apparaître ? »**

Le système aide donc à attirer l'attention sur les cas qui méritent une vérification.

---

## 4. Saturation

La page **Saturation** ajoute une dimension prédictive.

Le système compare les données observées avec une **prédiction future du débit**.

La page peut afficher :

- le débit actuel ;
- le débit prédit ;
- la variation prévue ;
- une probabilité d'alerte ;
- un seuil de forte charge ;
- une estimation d'incertitude ;
- un niveau de risque.

### Pourquoi ?

La différence principale avec une simple supervision est :

```text
Supervision
    → voir la charge actuelle

Saturation
    → anticiper une charge future
```

---

## 5. Alertes

La page **Alertes** rassemble les signaux qui doivent attirer l'attention.

Elle peut présenter :

- le niveau de l'alerte ;
- l'heure ;
- la source concernée ;
- une recommandation de vérification.

### Pourquoi ?

L'objectif est de ne pas obliger l'utilisateur à parcourir tous les graphiques pour trouver les événements importants.

---

# Collecte en temps réel

NETPULSE AI dispose également d'un bouton de collecte depuis l'interface :

**▶ Lancer la collecte**

Le service de collecte peut piloter plusieurs étapes selon la configuration :

```text
Zeek
 ↓
Insertion des flux
 ↓
Prétraitement
 ↓
Pipeline ML
 ↓
Mesures / prédictions / alertes
```

La collecte peut être organisée autour d'une **machine centrale** et de **machines satellites** participant à la capture.

Voir [Collecte en temps réel](collecte-temps-reel) pour le fonctionnement détaillé.

# Mode local et mode connecté

NETPULSE AI possède deux situations de fonctionnement.

### Mode local

Les pages utilisent les datasets fournis dans le dossier `datasets/` et les modèles intégrés.

Ce mode est pratique pour :

- démontrer le projet ;
- tester l'interface ;
- travailler sans connecter une base distante.

### Mode connecté

L’application lit les données depuis PostgreSQL / TimescaleDB.

Ce mode permet de travailler avec les données réellement collectées et les résultats produits par le pipeline.
