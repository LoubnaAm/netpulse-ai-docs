---
title: "NETPULSE AI"
description: "Comprendre rapidement le projet de supervision et d’analyse intelligente du trafic réseau."
---

# NETPULSE AI

**NETPULSE AI** est une application de supervision réseau qui collecte des informations sur le trafic, les analyse avec des modèles de Machine Learning et présente les résultats dans une interface simple.

L’objectif est de passer de données réseau difficiles à lire à des informations directement utiles pour la supervision :

- comprendre l’activité du réseau ;
- savoir quels types de trafic sont observés ;
- détecter des comportements inhabituels ;
- anticiper une éventuelle saturation ;
- transformer certains signaux en alertes ;
- suivre les données depuis une interface centralisée.

<Note>
Le projet peut fonctionner en **mode local** avec les jeux de données fournis, ou en **mode connecté** avec PostgreSQL/TimescaleDB et une collecte réelle.
</Note>

## Pourquoi ce projet ?

Dans un réseau, il peut être difficile de répondre rapidement à des questions simples :

- Le trafic augmente-t-il ?
- Quelles sources génèrent le plus de volume ?
- Quel type de trafic est dominant ?
- Y a-t-il un comportement inhabituel ?
- Le réseau risque-t-il d’atteindre une charge importante ?

NETPULSE AI rassemble ces réponses dans une seule application.

## Ce que le projet apporte

NETPULSE AI ajoute une couche d'**intelligence et d'aide à la décision** au-dessus de la collecte réseau.

La chaîne générale est :

```text
Trafic réseau
    ↓
Collecte
(Zeek / SNMP)
    ↓
Stockage
(PostgreSQL / TimescaleDB)
    ↓
Préparation des données
    ↓
Modèles Machine Learning
    ├── Classification du trafic
    ├── Détection d’anomalies
    └── Prédiction de saturation
    ↓
Alertes + indicateurs
    ↓
Interface NETPULSE AI
```

## À qui s’adresse cette documentation ?

Cette documentation est destinée à une personne qui **ne connaît pas encore le projet**.

Vous pouvez commencer par :

1. [Fonctionnalités](fonctionnalites)
2. [Architecture](architecture)
3. [Installation et lancement](installation)
4. [Les modules de l’application](modules)

## Technologies principales

Le projet s’appuie notamment sur :

| Technologie | Rôle |
|---|---|
| Python | logique de l’application et traitements |
| Streamlit | interface web de supervision |
| Zeek | observation et collecte des flux réseau |
| SNMP | collecte de métriques réseau |
| PostgreSQL / TimescaleDB | stockage des données |
| Supabase | authentification et services associés |
| scikit-learn / XGBoost / LightGBM / CatBoost | environnement Machine Learning |
| Pandas / NumPy | préparation et traitement des données |

## Résultat final

Le résultat attendu est une interface dans laquelle l’utilisateur peut passer de la **vision globale du réseau** à des informations plus précises sur les applications, les anomalies, la charge et les alertes.
