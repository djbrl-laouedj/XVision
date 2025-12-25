# XVision

**XVision** est un projet de recherche multimodal vision–langage axé sur la **télédétection agricole**.  
Il combine des **images satellites multispectrales Sentinel-2** avec des **modèles de langage (LLM)** afin de générer automatiquement des **analyses agronomiques interprétables** à partir de données satellites.

Le projet suit des architectures multimodales modernes inspirées de modèles tels que LLaVA et BLIP-2, adaptées spécifiquement aux données agricoles multispectrales.

---

## 🌍 Motivation du projet

L’imagerie satellite fournit des informations puissantes sur l’état des cultures, mais l’interprétation des données multispectrales reste complexe et technique.

XVision vise à combler ce fossé en :
- Extrayant des représentations visuelles riches à partir des images Sentinel-2
- Alignant ces représentations avec un modèle de langage
- Générant des descriptions agronomiques lisibles par l’humain (vigueur végétative, humidité, biomasse, couverture sol/végétation, etc.)

L’objectif est de soutenir la **prise de décision**, le **suivi** et l’**analyse** en agriculture grâce à une interprétation pilotée par l’IA.

---

## Architecture globale

XVision est composé de trois composants principaux :

### 1. Encodeur visuel Sentinel-2 (CROMA-Large)
- Vision Transformer pré-entraîné sur des images multispectrales Sentinel-2
- Traite 12 bandes optiques
- Produit un **embedding de dimension 1024** par patch d’image

### 2. Vision Adapter
- Un MLP léger qui projette l’embedding visuel de dimension 1024
- Le convertit en une séquence de **tokens visuels**
- Aligne l’information visuelle avec l’espace d’embedding du LLM

### 3. Modèle de langage (Qwen2-1.5B + LoRA)
- LLM généraliste adapté via un **fine-tuning LoRA**
- Apprend à générer du texte agronomique conditionné par les tokens visuels
- Seule une petite fraction des paramètres est entraînée, permettant un fine-tuning efficace

---

## Pipeline de bout en bout

1. Image multispectrale Sentinel-2 (GeoTIFF)
2. Prétraitement et normalisation des bandes
3. Extraction des embeddings visuels avec CROMA
4. Projection via le Vision Adapter
5. Injection des tokens visuels dans le LLM
6. Génération de texte (analyse agricole)

---

## Structure du dépôt

```XVision/

├── XVision-ViT.ipynb # Génération des embeddings Sentinel-2 (CROMA)

├── XVision-Captions-Generator.ipynb # Génération des captions agronomiques

├── XVision-LoRA.ipynb # Fine-tuning multimodal LoRA

├── XVision-inference.ipynb # Inférence multimodale sur de nouvelles images

├── requirements.txt # Dépendances Python

├── README.md # Documentation anglaise

├── README_FR.md # Documentation française

└── .gitignore
```

---

## Données & Captions

- Les embeddings visuels sont extraits à partir de patches Sentinel-2 via CROMA-Large
- Les captions agronomiques sont générées automatiquement à partir d’indices spectraux (NDVI, NDWI, SAVI, EVI, etc.)
- Les captions sont diversifiées, structurées et scientifiquement cohérentes afin d’éviter le sur-apprentissage lors de l’entraînement

---

## Stratégie d’entraînement

- L’alignement multimodal est appris via un fine-tuning LoRA
- Le LLM de base reste gelé
- Seuls le Vision Adapter et les couches LoRA sont entraînés
- Cela permet :
  - Une réduction des coûts de calcul
  - Un entraînement stable
  - Une meilleure généralisation

---

## Cas d’usage

- Suivi de parcelles agricoles
- Analyse de l’état des cultures
- Systèmes d’aide à la décision
- Génération augmentée par récupération géospatiale (RAG)
- Recherche sur l’apprentissage multimodal appliqué à la télédétection

---

## État du projet

- Pipeline multimodal de bout en bout opérationnel
- Fine-tuning LoRA validé
- Inférence fonctionnelle sur des images satellites non vues
- Améliorations en cours sur la diversité des captions et la qualité du dataset
- Extension prévue vers l’analyse multi-temporelle et d’autres types de cultures

---

## Licence

Ce projet est destiné à des usages de recherche et d’enseignement.
Veuillez vous référer aux licences des modèles et jeux de données sous-jacents (CROMA, Sentinel-2, Qwen).

---

## Socle technique

- Sentinel-2 / Programme Copernicus (ESA)
- TorchGeo & CROMA
- Écosystème Hugging Face
- Modèle Qwen
- LoRA & MLP

---

## 🚀 Lancer le projet

**1️. Cloner le dépôt**
```
https://github.com/djbrl-laouedj/XVision.git
```
```
cd XVision
```

**2️. Créer et activer un environnement virtuel (recommandé)**
```
python -m venv venv
```
```
source venv/bin/activate        # Linux / macOS
```
```
venv\Scripts\activate           # Windows
```

**3️. Installer les dépendances**
```
pip install --upgrade pip
```
```
pip install -r requirements.txt
```
⚠️ Un GPU compatible CUDA est fortement recommandé pour l’entraînement (fine-tuning LoRA).

### Exécution du pipeline (étape par étape)

**Étape 1 — Générer les embeddings Sentinel-2 (sentinel_embeddings_1024.npy) - Encodeur visuel**

Lancer :
```
XVision-ViT.ipynb
```

**Étape 2 — Générer les captions agronomiques (sentinel_indices.jsonl) - optionnel mais recommandé**

Lancer :
```
XVision-Captions-Generator.ipynb
```

**Étape 3 — Fine-tuning multimodal LoRA**

Lancer :
```
XVision-LoRA.ipynb
```
Sorties :

- vision_adapter.pt

- qwen2_lora_multimodal/

**Étape 4 — Inférence multimodale sur de nouvelles images**

Lancer :
```
XVision-inference.ipynb
```

---

##👤 Auteurs

Ce projet a été développé par Djebril Laouedj et Redha Ibbou,

étudiants en dernière année en Big Data & Intelligence Artificielle à l'ECE Paris.
