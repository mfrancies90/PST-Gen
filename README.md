# PST-Gen: A Privacy-Preserving Framework for Cross-Domain Cascade Detection in Arabic

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Paper](https://img.shields.io/badge/paper-IEEE%20Access-red.svg)](<paper-url>)

Official repository for the paper:

> **PST-Gen: A Privacy-Preserving Framework for Cross-Domain Cascade Detection in Morphologically Complex Languages — A Case Study on Arabic Health and Education NLP**


## Overview

PST-Gen is a synthetic timeline generation framework that converts two public Arabic corpora into longitudinal cross-domain event sequences for training cascade-detection models, without exposing any real individual. The framework is designed for low-resource, privacy-sensitive Arabic NLP settings where no public dataset links health and education records at the entity level.

The repository contains:
- The `pstgen_lib` Python library (timeline generator, register-adapted labeler, three sequence architectures, training and evaluation utilities)
- The full Colab notebook used to produce all experimental results in the paper
- The 10,000-timeline benchmark (200,000 events, 0.68% cascade rate)
- Pre-trained checkpoints for BiLSTM, Transformer, and Mamba architectures
- All evaluation scripts and result JSON files

## Key Result

On real Arabic mental-health text from MentalQA, the BiLSTM trained on PST-Gen synthetic data achieves **80% recall**, while Mamba — which dominates in-domain (F1 = 0.97) — achieves only 47% recall. Simpler temporal architectures generalize better when training data is synthetic.

| Model       | In-domain risk F1 | MentalQA recall |
|-------------|-------------------|-----------------|
| BiLSTM      | 0.80              | **0.80**        |
| Transformer | 0.80              | —               |
| Mamba       | **0.97**          | 0.47            |

## Installation

```bash
git clone https://github.com/<your-username>/PST-Gen.git
cd PST-Gen
pip install -r requirements.txt
```

Tested on Python 3.10, PyTorch 2.1, and CUDA 12.1 (Google Colab T4 GPU).

## Quick Start

### 1. Download source corpora

```bash
python scripts/download_datasets.py
```

This downloads MedInstruct-52K-Arabic, SOQAL Arabic-SQuAD, and ARCD into `data/raw/`.

### 2. Generate synthetic timelines

```bash
python scripts/generate_timelines.py --n_timelines 10000 --events_per_timeline 20
```

Output: `data/timelines.json` (10,000 timelines, 200,000 events).

### 3. Train a model

```bash
python scripts/train.py --architecture bilstm    # or transformer, or mamba
```

Outputs checkpoints to `checkpoints/<architecture>/best.pt` and training logs to `results/<architecture>_history.json`.

### 4. Evaluate on real data

```bash
python scripts/evaluate_real.py --architecture bilstm --corpus mentalqa
python scripts/evaluate_real.py --architecture bilstm --corpus carma
```

## Reproducing Paper Results

The full Colab notebook used to produce all reported results is in `notebooks/PSTGen_Final_2026.ipynb`. Open it in Google Colab and run all cells in order. Pre-computed results JSON files are in `results/`.

## Pre-Trained Checkpoints

Pre-trained model checkpoints (file sizes exceed GitHub's 100 MB limit) are hosted on Hugging Face:

- BiLSTM: <huggingface-url>
- Transformer: <huggingface-url>
- Mamba: <huggingface-url>

## Datasets

PST-Gen uses two public Arabic corpora:

| Corpus | Source | Size | License |
|---|---|---|---|
| MedInstruct-52K-Arabic | [adlbh/medinstruct-52k-arabic](https://huggingface.co/datasets/adlbh/medinstruct-52k-arabic) | 52,002 instructions | Apache 2.0 |
| SOQAL Arabic-SQuAD + ARCD | [husseinmozannar/SOQAL](https://github.com/husseinmozannar/SOQAL) | 49,739 questions, 10,829 paragraphs | MIT |

For real-data evaluation:

| Corpus | Source | Size |
|---|---|---|
| CARMA (public sample) | [smankarious/carma](https://huggingface.co/datasets/smankarious/carma) | 100 Reddit posts |
| MentalQA (Train+Dev) | [hasanhuz/MentalQA](https://github.com/hasanhuz/MentalQA) | 350 questions |


## Privacy and Ethical Use

PST-Gen produces synthetic data by recombining verbatim texts from public corpora; no real individual is represented in the generated timelines. The framework's privacy properties reduce to those of the public source corpora and are empirical rather than formal. The trained models are research artifacts and are not clinical instruments. We refer the reader to Section 6 of the paper for the full discussion.

## License

This repository is released under the MIT License (see `LICENSE`). Source corpora retain their original licenses.

## Contact

Mariam Labib Francies — <mariamlabib90@std.mans.edu.eg>
PhD Candidate, Department of Electronics and Communication Engineering, Mansoura University.
