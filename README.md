# 🔬 Predicting Linear B-cell Epitopes using Feature Selection & CatBoost

A machine learning pipeline for predicting **linear B-cell epitopes** from high-dimensional protein embeddings using dimensionality reduction and ensemble modeling — with a focus on **robust generalization**, **class imbalance**, and **biological interpretability**.

---

## 📌 Project Overview

This project addresses the **binary classification** of protein sequences into epitope (`1`) or non-epitope (`-1`) regions. Using **49,606** sequences with **ESM-1b-derived** features (~1,280), the goal is to build an efficient, interpretable pipeline that:

- Reduces dimensionality effectively
- Handles **severe class imbalance**
- Avoids **data leakage** via proper group-aware validation
- Leverages state-of-the-art models for performance and scalability

---

## 🧪 Background and Motivation

**B-cell epitopes** are short regions on antigens that stimulate immune response. Rapid and accurate prediction of these epitopes is critical in vaccine development. The challenge lies in:

- High-dimensional embeddings from deep models (e.g., ESM-1b)
- Data imbalance (epitope presence is rare: ~1.6%)
- Risks of overfitting due to overlapping clusters

---

## ⚙️ Solution Pipeline

### 1. **Preprocessing**
- **Missing values** imputed using **median strategy**, justified via skewness and visual EDA.
- Info features separated from predictive features.

### 2. **Feature Selection**
- **Stage 1**: Variance Thresholding (cut-off = 0.05) → reduced features from 1280 to 264.
- **Stage 2**: L1-based Logistic Regression (embedded) → further reduced to 116 features.

### 3. **Modeling**

Compared three models using **GroupKFold (Info_cluster)** to avoid data leakage:

| Model                        | Balanced Accuracy |
|-----------------------------|-------------------|
| Logistic Regression (L1)    | 60.4%             |
| CatBoost (default)          | 81.7%             |
| CatBoost (Optuna tuned)     | 81.2%             |

- **CatBoost** outperformed others, thanks to:
  - Support for imbalanced data (`auto_class_weights='Balanced'`)
  - Robust handling of high-dimensional sparse data
  - Efficient internal feature selection

---

## 📊 Feature Importance

CatBoost’s feature importance showed that only a small subset of features drives prediction — aligning with the biological intuition that only select residues within a sequence are immunogenic.

A plot of the top 20 most important features is provided in the report.

---

## ✅ Evaluation Strategy

- **Group-aware cross-validation** using `Info_cluster` to ensure test sets were disjoint from training clusters
- **Balanced Accuracy** used as the primary metric:  
  \[ \text{Balanced Accuracy} = \frac{1}{2}(\text{Sensitivity} + \text{Specificity}) \]

---

## 📈 Final Results

- 🎯 **Final Model**: CatBoost with 116 selected features
- 💡 **Balanced Accuracy**: 81.7%
- 📉 **Dimensionality Reduction**: ~91% from original feature space
- 🧠 **Generalization**: Robust, cluster-aware evaluation strategy

---

## ⚠️ Challenges Encountered

- **Training time** was significant due to the dataset size and GroupKFold strategy
- **Hyperparameter tuning** with Optuna required careful memory and timeout control
- **Avoiding data leakage** was non-trivial — addressed using `GroupKFold`

---

## 🔄 Future Improvements

- Integrate **SHAP values** for deeper interpretability
- Explore **multi-task learning** for cross-dataset generalization
- Consider **transformer fine-tuning** or **attention-based models** for context-aware learning

---

## 📁 Project Structure

```
├── data/
│   └── esm1b_protein_embeddings.csv
├── Predicting_Linear_B-cell_Epitopes.ipynb
├── README.md
└── requirements.txt
```

---

## 🚀 How to Run

```bash
# Clone the repo
git clone https://github.com/your-username/epitope-prediction.git
cd epitope-prediction

# Install dependencies
pip install -r requirements.txt

# Run the notebook
jupyter notebook notebooks/Predicting_Linear_B-cell_Epitopes.ipynb
```

---

## 🤝 Acknowledgements

Special thanks to the team behind **ESM-1b** embeddings and the creators of **CatBoost** and **Optuna**, which were instrumental to this project’s success.

---

## 📜 License

This project is licensed under the MIT License. See `LICENSE` for more details.