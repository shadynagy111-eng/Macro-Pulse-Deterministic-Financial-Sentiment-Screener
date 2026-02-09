# 📂 Data Directory

This directory contains the datasets, sample inputs, and expected outputs used for developing, testing, and fine-tuning the **Macro-Pulse** sentiment engine.

## 🗂️ Structure

The data is organized to support the full machine learning lifecycle, from raw text ingestion to structured JSON output validation.

```text
data/
├── raw/                   # Unprocessed search results (HTML/JSON)
├── processed/             # Cleaned text ready for LLM ingestion
├── samples/               # Example I/O pairs for demonstration
└── taxonomy/              # Financial sentiment definitions
