# XVision

**XVision** is a multimodal vision–language research project focused on **agricultural remote sensing**.  
It combines **Sentinel-2 multispectral satellite imagery** with **large language models (LLMs)** to automatically generate **interpretable agronomic analyses** from satellite data.

The project follows modern multimodal architectures inspired by models such as LLaVA and BLIP-2, adapted specifically to multispectral agricultural imagery.

---

## 🌍 Project Motivation

Satellite imagery provides powerful insights into crop conditions, but interpreting multispectral data remains complex and technical.

XVision aims to bridge this gap by:
- Extracting rich visual representations from Sentinel-2 images
- Aligning these representations with a language model
- Generating human-readable agronomic descriptions (vegetation vigor, moisture, biomass, soil coverage, etc.)

The goal is to support **decision-making, monitoring, and analysis** in agriculture through AI-driven interpretation.

---

## Architecture Overview

XVision is composed of three main components:

### 1. Sentinel-2 Visual Encoder (CROMA-Large)
- Vision Transformer pre-trained on multispectral Sentinel-2 imagery
- Processes 12 optical bands
- Produces a **1024-dimensional embedding** per image patch

### 2. Vision Adapter
- A lightweight MLP that projects the 1024-D visual embedding
- Converts it into a sequence of **visual tokens**
- Aligns visual information with the LLM embedding space

### 3. Language Model (Qwen2-1.5B + LoRA)
- General-purpose LLM adapted using **LoRA fine-tuning**
- Learns to generate agronomic text conditioned on visual tokens
- Only a small fraction of parameters is trained, enabling efficient fine-tuning

---

## End-to-End Pipeline

1. Sentinel-2 multispectral image (GeoTIFF)
2. Preprocessing and band normalization
3. Visual embedding extraction using CROMA
4. Projection through the Vision Adapter
5. Injection of visual tokens into the LLM
6. Text generation (agricultural analysis)

---

## Repository Structure

XVision/

├── XVision-ViT.ipynb # Sentinel-2 embedding generation (CROMA)

├── XVision-Captions-Generator.ipynb # Agronomic caption generation

├── XVision-LoRA.ipynb # Multimodal LoRA fine-tuning

├── XVision-inference.ipynb # Multimodal inference on new images

├── requirements.txt # Python dependencies

├── README.md # ENG Documentation

├── README_FR.md # FR Documentation

└── .gitignore

---

## Data & Captions

- Visual embeddings are extracted from Sentinel-2 patches using CROMA-Large
- Agronomic captions are generated automatically from spectral indices (NDVI, NDWI, SAVI, EVI, etc.)
- Captions are **diverse, structured, and scientifically coherent** to avoid overfitting during training

---

## Training Strategy

- Multimodal alignment is learned using **LoRA fine-tuning**
- The base LLM remains frozen
- Only the Vision Adapter and LoRA layers are trained
- This results in:
  - Reduced computational cost
  - Stable training
  - Better generalization

---

## Use Cases

- Agricultural parcel monitoring
- Crop condition analysis
- Decision-support systems
- Geospatial retrieval-augmented generation (RAG)
- Research on multimodal learning with remote sensing data

---

## Project Status

- End-to-end multimodal pipeline operational
- LoRA fine-tuning validated
- Inference on unseen satellite images working
- Ongoing improvements on caption diversity and dataset quality
- Extension to multi-temporal analysis and additional crop types

---

## License

This project is intended for **research and educational purposes**.  
Please refer to the licenses of the underlying models and datasets (CROMA, Sentinel-2, Qwen).

---

## Acknowledgements

- Sentinel-2 / Copernicus Programme (ESA)
- TorchGeo & CROMA authors
- Hugging Face ecosystem
- Qwen model authors

---

## 👤 Authors

This project was developed by **Djebril Laouedj** and **Redha Ibbou**,  
final-year students in **Big Data & Artificial Intelligence** at **ECE Paris**.
