**Hotel Attribute Extraction & Tiering**

This project builds an end-to-end pipeline to extract structured signals from hotel reviews and map them to interpretable quality tiers (Elite / Superior / Premium / Fail).

**Overview**

We transform unstructured review text into actionable hotel insights:

Review → Attribute Extraction → Aggregation → Tier Assignment → Evidence

**Dataset**
Source: TripAdvisor Hotel Reviews Dataset
~20,000 reviews with ratings (1–5)
No hotel IDs → simulated grouping used

**Attributes Modeled**

We extract 5 attributes:

Cleanliness (sentiment)
Staff Service (sentiment)
WiFi Quality (sentiment)
Location Accessibility (factual + sentiment)
Room Size (factual + sentiment)

**Pipeline**
1. Data Preparation
Balanced dataset across ratings
Sentence splitting using NLTK
Simulated hotel_id (since dataset lacks grouping)
2. Labeling (LLM-based)
Used Mistral API for zero-shot labeling
Extracted:
sentiment (positive / negative / neutral / not_mentioned)
confidence (0–1)
evidence (supporting sentence)

**Quality Control:**

Filtered labels with confidence < 0.5

3. Modeling

Each attribute has its own classifier:

TF-IDF + Logistic Regression
Fast, interpretable, and effective on short text

Why not others:

Transformers → overkill, slow
Random Forest → less interpretable

4. Aggregation → Tiering

Per-hotel aggregation:

Use only high-confidence predictions
Compute % positive mentions per attribute

Tier Mapping:

Elite → ≥ 80% positive
Superior → 60–79%
Premium → otherwise
Fail → ≥ 60% negative
Insufficient Data → low mentions

5. Evidence Extraction

For each hotel × attribute:

Top 3 high-confidence sentences are surfaced

**Outputs**
labeled_reviews.csv → LLM-labeled dataset
model_*.joblib → trained models per attribute
eval_results.json → evaluation metrics
hotel_tiers.csv → final tier assignments
hotel_evidence.json → supporting evidence





# **How to Run**
1. Setup
pip install pandas scikit-learn nltk mistralai tqdm joblib
2. Download Dataset
kaggle datasets download -d andrewmvd/trip-advisor-hotel-reviews
unzip trip-advisor-hotel-reviews.zip

4. Run Pipeline

Run notebooks / scripts in order:

Data preprocessing
LLM labeling
Model training
Aggregation & tiering

📁 Repository Structure

├── notebooks/               # Colab / Jupyter notebooks

├── models/                  # Saved models

├── data/                    # Dataset + labeled data

├── labeled_reviews.csv

├── hotel_tiers.csv

├── hotel_evidence.json

├── eval_results.json

├── README.md

└── WRITEUP.md


**Deliverables**

✅ End-to-end pipeline code

✅ Labeled dataset (LLM-generated)

✅ Trained model artifacts

✅ Evaluation results

✅ This README


**Limitations**
No real hotel IDs → simulated grouping
LLM labeling may introduce bias/noise
API cost & latency
Limited factual extraction (room size rarely present)

**Future Work**
Replace LLM with distilled model
Add real hotel metadata
Improve factual extraction (NER)

**Author**

Tushar Singh Lodhi

