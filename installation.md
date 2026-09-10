---
title: "Installation et lancement"
description: "Démarrer NETPULSE AI simplement."
---

# Installation et lancement

Cette page explique la mise en route du projet.

## Prérequis

Le projet prévoit notamment :

- Python 3.10 ou plus récent ;
- Git ;
- Git LFS ;
- WSL 2 avec Ubuntu ;
- Zeek ;
- outils SNMP.

Le dépôt utilise Git LFS pour certains datasets. Sans Git LFS, les gros fichiers CSV peuvent être récupérés uniquement sous forme de fichiers pointeurs.

## 1. Récupérer le projet

```bash
git lfs install
git clone https://github.com/AH-Digital-go/prediction-traffic.git
cd prediction-traffic
```

Si le projet a déjà été cloné sans Git LFS :

```bash
git lfs pull
```

### Vérification des datasets

Par exemple :

```bash
head datasets/dataset_saturation_stable_natural_aug13-16.csv
```

Si le résultat commence par :

```text
version https://git-lfs.github.com/spec/v1
```

les vrais fichiers n'ont pas encore été récupérés.

---

## 2. Créer l'environnement Python

Sous Windows :

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

Puis :

```bash
pip install -r requirements.txt
```

Les dépendances couvrent notamment Streamlit, Pandas, NumPy, SQLAlchemy, PostgreSQL et les bibliothèques Machine Learning utilisées par les modèles.

---

## 3. Configuration

Le projet utilise un fichier `.env`.

Si nécessaire :

```powershell
Copy-Item .env.example .env
```

Les paramètres de connexion à la base et les options de fonctionnement sont centralisés dans `config.py`.

Le projet peut utiliser :

- une URL PostgreSQL complète ;
- ou les paramètres `host`, `port`, `database`, `user`, `password`.

L'authentification peut également être configurée avec Supabase.

<Warning>
Ne publiez jamais votre fichier `.env` ni vos mots de passe dans Git.
</Warning>

---

## 4. Installer Zeek et SNMP dans WSL

Dans Ubuntu / WSL :

```bash
sudo apt update
sudo apt install -y zeek snmp snmpd snmp-mibs-downloader
```

Vérifier Zeek :

```bash
zeek --version
```

Vérifier SNMP :

```bash
which snmpget
which snmpwalk
```

---

## 5. Lancer l'application

Depuis la racine du projet :

```powershell
.\venv\Scripts\Activate.ps1
streamlit run app.py
```

Puis ouvrir :

```text
http://localhost:8501
```

---

## 6. Démarrer la collecte

Dans l'interface, cliquer sur :

**▶ Lancer la collecte**

Le bouton devient **■ Arrêter la collecte** lorsqu'un processus de collecte est actif.

Le collecteur vérifie notamment la disponibilité de WSL et de Zeek avant de démarrer.

---

## 7. Machines satellites

Une machine satellite peut utiliser le launcher fourni dans :

```text
.exe/
```

Le launcher est destiné aux autres machines qui participent à la collecte.

La machine centrale utilise l'application Streamlit pour :

- superviser ;
- analyser ;
- visualiser les résultats.

---

## Mode local

Le mode local permet de tester l'interface avec les datasets du dossier :

```text
datasets/
```

Il n'est pas nécessaire de disposer d'un environnement de collecte réseau complet pour découvrir les pages et comprendre le fonctionnement général de l'application.

---

## Mode connecté

Pour passer aux données réelles, le principe est :

```text
NETPULSE_USE_SYNTHETIC=false
```

Puis renseigner la connexion PostgreSQL / Supabase.

Les tables SQL du projet se trouvent dans :

```text
sql/
```

Le guide technique fourni avec le projet détaille ensuite la mise en place de la chaîne Zeek → base de données → pipeline ML → alertes.
