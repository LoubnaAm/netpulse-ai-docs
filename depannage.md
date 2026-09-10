---
title: "Dépannage"
description: "Solutions aux problèmes les plus courants."
---

# Dépannage

## L'application ne démarre pas

Vérifier :

```bash
python --version
```

Puis l'environnement :

```powershell
.\venv\Scripts\Activate.ps1
```

Et les dépendances :

```bash
pip install -r requirements.txt
```

---

## Les datasets semblent très petits

C'est généralement un problème de **Git LFS**.

Si un CSV contient uniquement :

```text
version https://git-lfs.github.com/spec/v1
```

exécuter :

```bash
git lfs install
git lfs pull
```

---

## Le bouton de collecte affiche une erreur WSL

Le collecteur vérifie que WSL est accessible avant de démarrer.

Vérifier que WSL fonctionne correctement et qu'une distribution Ubuntu est installée.

---

## Zeek n'est pas disponible

Vérifier :

```bash
zeek --version
```

Si la commande n'existe pas, installer Zeek dans WSL selon l'environnement utilisé.

---

## La base de données n'est pas accessible

Vérifier les paramètres dans `.env` :

```text
NETPULSE_DATABASE_URL
```

ou :

```text
NETPULSE_DB_HOST
NETPULSE_DB_PORT
NETPULSE_DB_NAME
NETPULSE_DB_USER
NETPULSE_DB_PASSWORD
```

Vérifier également que PostgreSQL est démarré.

---

## L'interface affiche `MODE LOCAL`

Cela signifie que l'application fonctionne avec la source locale configurée.

Le paramètre principal est :

```text
NETPULSE_USE_SYNTHETIC
```

Pour le mode connecté :

```text
NETPULSE_USE_SYNTHETIC=false
```

Redémarrer ensuite Streamlit.

---

## La base est connectée mais une page reste vide

Dans le mode connecté, certaines pages dépendent de tables prêtes et alimentées.

Le projet utilise des indicateurs de configuration tels que :

```text
NETPULSE_APPLICATION_READY
NETPULSE_SUPERVISION_READY
NETPULSE_ALERTES_READY
NETPULSE_ANOMALIES_READY
NETPULSE_SATURATION_READY
```

Une table vide ou un flag non activé peut donc empêcher une page d'afficher les données attendues.

---

## `dataset_saturation.csv` est introuvable

La construction du dataset de saturation dépend des métriques collectées.

Vérifier :

1. que les métriques SNMP sont disponibles ;
2. que le processus de construction du dataset a fonctionné ;
3. les logs du pipeline.

---

## Les prédictions ne semblent pas évoluer

Une prédiction ML dépend à la fois :

- des nouvelles données ;
- du prétraitement ;
- du modèle chargé ;
- de la fréquence d'exécution du pipeline.

Avant de conclure à un problème de modèle, vérifier d'abord que les nouvelles observations arrivent réellement dans la base.

---

# Principe de diagnostic

En cas de problème, suivre cette chaîne :

```text
1. Le réseau produit-il des données ?
           ↓
2. Zeek / SNMP les voient-ils ?
           ↓
3. Les données arrivent-elles dans la BDD ?
           ↓
4. Le prétraitement fonctionne-t-il ?
           ↓
5. Les jobs ML s'exécutent-ils ?
           ↓
6. Les tables de résultats sont-elles alimentées ?
           ↓
7. L'interface les lit-elle ?
```

Cette méthode évite de chercher directement le problème dans l'interface alors que la panne peut se trouver plus tôt dans la chaîne.
