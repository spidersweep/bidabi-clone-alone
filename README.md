<h1 align="center">
✨🚀 Bidabi — Pipeline de Machine Learning 🚀✨
</h1>

<p align="center">
  <img src="https://media.giphy.com/media/ZVik7pBtu9dNS/giphy.gif" width="200">
</p>

<p align="center">
  <b>Pipeline de Machine Learning de bout en bout avec PyTorch & DVC</b>
</p>
<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12-blue">
  <img src="https://img.shields.io/badge/PyTorch-DeepLearning-red">
  <img src="https://img.shields.io/badge/DVC-DataVersioning-green">
</p>

---

## Objectif

Ce projet implémente un pipeline ML complet pour classifier des images de produits alimentaires en 3 catégories : sugar, butter et champagnes. Il couvre l'ensemble du cycle de vie d'un projet ML :

Collecte : scrapping asynchrone de l'API OpenFoodFacts
Stockage : structure RAW → INTERIM → PROCESSED
Versionnement : code avec Git, données et modèles avec DVC
Entraînement : ResNet-18 fine-tuné avec augmentations avancées
Évaluation : métriques complètes (accuracy, F1, ROC, confusion matrix)
---

## Structure du projet

```
bidabi-clone-alone/
│
├── src/
│   ├── asyscrapper.py        # Scrapper asynchrone OpenFoodFacts
│   ├── data_loader.py        # Chargement et préparation des données
│   └── classificator.py      # Pipeline d'entraînement ResNet-18
│
├── data/
│   ├── raw/                  # Données brutes (versionnées avec DVC)
│   │   ├── images/
│   │   │   ├── sugar/
│   │   │   ├── butter/
│   │   │   └── champagnes/
│   │   ├── metadata_sugar_180.csv
│   │   ├── metadata_butter_180.csv
│   │   └── metadata_champagnes_180.csv
│   └── readme.md
│
├── .dvc/                     # Configuration DVC
├── data/raw.dvc              # Pointeur DVC vers le dataset RAW
├── best_model_resnet18_finetuned.pth.dvc  # Pointeur DVC vers le modèle
├── requirements.txt          # Dépendances Python
├── .gitignore
└── README.md
```
## 🔄 Pipeline de Machine Learning

Le pipeline comprend les étapes suivantes :

* Création et structuration du dataset (jeu de données de classification d’images)
* Versionnement des données avec DVC
* Division du dataset en ensembles d’entraînement / validation / test (70/20/10)
* Augmentation des données (redimensionnement, retournement horizontal, conversion en tenseur)
* Entraînement d’un modèle de deep learning (ResNet18)
* Suivi de la perte (loss) en entraînement et en validation
* Sauvegarde du meilleur modèle entraîné

---

## Prérequis

- Python 3.12
- Git
- DVC 3.67.1

---

## Installation et reproduction

### 1. Cloner le dépôt
```bash
git clone git@github.com:spidersweep/bidabi-clone-alone.git
cd bidabi-clone-alone
```

### 2. Créer et activer l'environnement virtuel
```bash
py -3.12 -m venv .venv
.venv\Scripts\Activate
```

### 3. Installer les dépendances
```bash
pip install -r requirements.txt
```

### 4. Récupérer les données et le modèle via DVC
```bash
dvc pull
```

### 5. Lancer l'entraînement
```bash
python src/classificator.py
```

---

## Collecte des données

Le scrapper collecte les images et métadonnées depuis l'API OpenFoodFacts.
Pour relancer la collecte, modifier la variable `CATEGORY` dans `src/asyscrapper.py` :

```bash
python src/asyscrapper.py
```

Les données sont sauvegardées dans `data/raw/images/<categorie>/` et `data/raw/metadata_<categorie>.csv`.

---

## Pipeline d'entraînement

Le fichier `src/classificator.py` implémente :

- **Chargement** : `datasets.ImageFolder` depuis `data/raw/images/`
- **Séparation** : 60% train / 20% validation / 20% test
- **Augmentations** : RandomHorizontalFlip, RandomRotation, ColorJitter, GaussianBlur, MixUp
- **Modèle** : ResNet-18 fine-tuné (poids ImageNet)
- **Optimiseur** : Adam + CosineAnnealingLR
- **Early stopping** : patience = 3
- **Métriques** : Loss, Accuracy, Confusion Matrix, ROC curves, Per-class accuracy

### Résultats obtenus
| Métrique | Valeur |
|----------|--------|
| Meilleure Val Accuracy | ~80% |
| Epochs | 19/20 (early stopping) |
| Catégories | sugar, butter, champagnes |

---

## Versionnement

| Outil | Usage |
|-------|-------|
| Git | Versionnement du code |
| DVC | Versionnement des données et du modèle |

---

## Versions

| Tag | Description |
|-----|-------------|
| v1.0 | Version initiale du projet |
| v2.0 | Nouveau scrapper et données RAW |
| v2.1 | Dataset RAW versionné avec DVC |
| v3.0 | Pipeline complet avec modèle entraîné |

---

## Niveau visé

Ce projet correspond aux compétences d'un profil **Junior à Intermédiaire** en Machine Learning / MLOps :
- Machine Learning Engineer Junior
- Data Scientist Junior
- MLOps Engineer Junior