---
title: "Modules de l’application"
description: "Guide rapide pour comprendre chaque page de NETPULSE AI."
---

# Modules de l’application

## Barre latérale

La barre latérale permet de naviguer entre les espaces :

- Supervision
- Alertes
- Applications
- Anomalies
- Saturation

Lorsque plusieurs machines ont envoyé des données, un filtre **Source de collecte** peut être proposé.

L'utilisateur connecté peut également se déconnecter et changer le thème de l'interface.

---

## Supervision

### Ce que l'on voit

La page présente une vision globale du réseau.

Exemples d'indicateurs :

```text
Débit moyen
Utilisateurs / sources actifs
Flux observés
Mesures collectées
```

Un graphique montre également l'évolution du débit.

### Utilité

Cette vue sert principalement au **monitoring**.

Elle permet de voir rapidement si l'activité réseau évolue.

---

## Applications

### Ce que l'on voit

La classification transforme les flux en catégories de trafic.

La page présente notamment :

- le nombre de flux classifiés ;
- la classe dominante ;
- le volume total ;
- la distribution des classes ;
- l'évolution des classes ;
- les flux récents.

### Utilité

Elle permet de mieux comprendre **la composition du trafic** plutôt que de regarder uniquement des volumes bruts.

---

## Anomalies

### Ce que l'on voit

La page travaille avec les résultats du détecteur d'anomalies.

On y retrouve des informations comme :

- nombre d'événements au-dessus du seuil ;
- nombre de sources suspectes ;
- distribution des scores ;
- distribution des niveaux de risque ;
- exemple d'un événement signalé.

### Interprétation

Un score élevé signifie qu'une observation s'éloigne davantage du comportement considéré comme normal par le modèle.

L'alerte constitue un **signal à vérifier**, et non à elle seule une preuve d'incident.

---

## Saturation

### Ce que l'on voit

La page compare :

```text
Débit actuel
        ↓
Débit futur prédit
```

Elle peut également afficher :

- la probabilité d'alerte ;
- le seuil de forte charge ;
- une zone d'incertitude ;
- le niveau de risque ;
- le nom et le statut du modèle.

### Interprétation

L'objectif est d'anticiper une augmentation importante de charge pour laisser davantage de temps à la supervision et à la décision.

---

## Alertes

### Ce que l'on voit

Les alertes synthétisent les signaux importants.

Chaque événement peut être présenté avec :

```text
Heure
Source
Niveau
Recommandation
```

La recommandation est une aide à l'analyse. Elle n'effectue pas automatiquement une action corrective sur le réseau.

---

## Mode de lecture

Une manière simple d'utiliser l'application est :

```text
1. Supervision
   ↓
2. Applications
   ↓
3. Anomalies
   ↓
4. Saturation
   ↓
5. Alertes
```

Cette logique permet de passer du général au particulier :

```text
Que se passe-t-il ?
        ↓
Quel trafic ?
        ↓
Quel comportement inhabituel ?
        ↓
Quelle évolution future ?
        ↓
Que faut-il vérifier ?
```

## Authentification

Le projet possède un module d'authentification dans :

```text
utils/auth.py
```

L'authentification peut être activée via la configuration Supabase.

Lorsque le portail de connexion est activé, l'utilisateur doit être authentifié pour accéder à l'application.

## Source des données

L'application distingue les données locales et les données de la base.

Le statut affiché dans l'interface permet de reconnaître le mode actif :

```text
MODE LOCAL
```

ou :

```text
BDD ACTIVE
```

Cela aide à comprendre immédiatement si les pages lisent les datasets locaux ou les données connectées.
