# Explore Machine Learning Models with Explainable AI: Challenge Lab (GSP324)

![Google Cloud](https://img.shields.io/badge/Google_Cloud-4285F4?logo=googlecloud&logoColor=white)
![Lab](https://img.shields.io/badge/Lab-GSP324-34A853)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?logo=tensorflow&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-Responsible_AI-purple)

Reference solution for the Google Cloud challenge lab **Explore Machine Learning Models with Explainable AI** (GSP324).

## 📖 Overview

You train two TensorFlow models that predict mortgage approval:

- **`complete_model`**: trained on the full dataset.
- **`limited_model`**: trained on a deliberately skewed subset.

You deploy both to AI Platform Prediction and compare them in the **What-If Tool**. The comparison shows how biased training data leads to unfair predictions.

## 🧰 Services used

Vertex AI Workbench / AI Platform Notebooks · TensorFlow Keras · AI Platform Prediction · What-If Tool

## 🚀 Solution

All code runs in the lab's Jupyter notebook. Fill in each `TODO` cell as shown.

### 1. Build and train the complete model

```python
model = Sequential()
model.add(layers.Dense(200, input_shape=(input_size,), activation="relu"))
model.add(layers.Dense(50, activation="relu"))
model.add(layers.Dense(20, activation="relu"))
model.add(layers.Dense(1, activation="sigmoid"))

model.compile(loss="mean_squared_error", optimizer="adam", metrics=["accuracy"])
model.fit(train_data, train_labels, epochs=10, batch_size=2048, validation_split=0.1)
```

### 2. Build and train the limited model

Same architecture, trained on the limited dataset:

```python
limited_model = Sequential()
limited_model.add(layers.Dense(200, input_shape=(input_size,), activation="relu"))
limited_model.add(layers.Dense(50, activation="relu"))
limited_model.add(layers.Dense(20, activation="relu"))
limited_model.add(layers.Dense(1, activation="sigmoid"))

limited_model.compile(loss="mean_squared_error", optimizer="adam", metrics=["accuracy"])
limited_model.fit(limited_train_data, limited_train_labels, epochs=10, batch_size=2048, validation_split=0.1)
```

### 3. Configure deployment

```python
GCP_PROJECT    = "<PROJECT_ID>"
MODEL_BUCKET   = "gs://<PROJECT_ID>"
MODEL_NAME     = "complete_model"   # do not modify
LIM_MODEL_NAME = "limited_model"    # do not modify
VERSION_NAME   = "v1"
REGION         = "us-central1"
```

### 4. Deploy both models to AI Platform

```bash
# complete_model
!gcloud ai-platform models create $MODEL_NAME --regions $REGION
!gcloud ai-platform versions create $VERSION_NAME \
  --model=$MODEL_NAME \
  --framework=TENSORFLOW \
  --runtime-version=2.1 \
  --python-version=3.7 \
  --origin=$MODEL_BUCKET/saved_complete_model/ \
  --staging-bucket=$MODEL_BUCKET

# limited_model
!gcloud ai-platform models create $LIM_MODEL_NAME --regions $REGION
!gcloud ai-platform versions create $VERSION_NAME \
  --model=$LIM_MODEL_NAME \
  --framework=TENSORFLOW \
  --runtime-version=2.1 \
  --python-version=3.7 \
  --origin=$MODEL_BUCKET/saved_limited_model/ \
  --staging-bucket=$MODEL_BUCKET
```

### 5. Compare the models in the What-If Tool

Run the lab's What-If Tool cell. Then answer the lab questions by comparing the two models' predictions across the protected attributes.

## 🔎 Version notes

AI Platform Prediction and its 2.x runtimes are deprecated. The modern equivalent is **Vertex AI Endpoints** with **Vertex Explainable AI**.

---

## ⚠️ Notes

- Resource names, zones, regions and ports are generated per lab session. Always use the values shown in **your** lab instructions.
- These steps were written when the lab was live. Google revises labs over time, so the current version may differ.

## 📌 Disclaimer

This repository is a study reference. Attempt the lab on your own first, then compare approaches. It is not affiliated with or endorsed by Google.

## 👤 Author

**Girish Sharma** · Cloud & DevOps Engineer · [GitHub](https://github.com/codewithgarry) · [LinkedIn](https://www.linkedin.com/in/codewithgarry/)
