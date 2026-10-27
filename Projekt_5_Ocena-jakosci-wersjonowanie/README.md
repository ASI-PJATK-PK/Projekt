# 📈 Zajęcia 5: Ocena jakości i wersjonowanie

## 🎯 Cel zajęć
Celem zajęć jest **ocena jakości modeli ML** oraz **wprowadzenie wersjonowania danych i modeli**.  
Studenci nauczą się analizować błędy modelu, interpretować wyniki (np. SHAP, feature importance) oraz zapisywać wersje modeli i metryk w repozytorium.  

**WAŻNA UWAGA!!!**

**Wybieramy konkretny model a nie cały system autoML !!!**
**Dodatkowo grupy w których jest więcej niż 1 osoba od tego momentu muszą mieć już meta-model lub inne równoważne rozwiązanie !!!**

## 📤 Zasady oddawania

1. **Nazwa repozytorium** musi zawierać numer s studenta lub studentów.
2. **Ten etap** musi mieć **dedykowany branch** z rozwiązaniem (np. `zajecia-5`).
3. **Oddanie** przez Microsoft Teams: **link do brancha** (jako **link**, nie plik tekstowy).
4. **Każda poprawa** = przesłanie **nowego linku**.
5. **Commity** po ostatecznym terminie oddania = **oszustwo**.

---

## 🧩 Zakres pracy

1. **Walidacja krzyżowa i testy końcowe**
   - W pipeline modelingowym lub nowym pipeline `evaluation` wykonaj:
     - walidację krzyżową (`cross_val_score` z `sklearn.model_selection`),
     - ocenę modelu na zbiorze testowym.  
   - Zapisz wyniki do `data/reporting/test_metrics.json`, np.:
     ```json
     {
       "accuracy": 0.87,
       "precision": 0.82,
       "recall": 0.79,
       "f1_score": 0.80
     }
     ```
   - W przypadku regresji: użyj metryk RMSE, MAE, R².

---

2. **Analiza błędów modelu**
   - W notebooku lub module Kedro wykonaj analizę:
     - **macierzy pomyłek (confusion matrix)** – `sklearn.metrics.confusion_matrix`,
     - **feature importance** (np. z modelu RandomForest lub XGBoost),
     - (opcjonalnie) **SHAP values** dla interpretowalności predykcji.
   - Zapisz wykresy w `docs/plots/` (np. `confusion_matrix.png`, `feature_importance.png`).

---

3. **Rejestracja i logging wyników**
   - Możesz użyć jednego z podejść:
     - **MLflow** – zarejestruj model i metryki w lokalnym serwerze MLflow (`mlflow ui`).
     - **Prosty logging w JSON / CSV** – zapis wyników i parametrów modeli w pliku `data/06_reporting/model_versions.csv`.
   - Przykład wpisu:
     ```csv
     timestamp,model_name,accuracy,f1_score,version
     2025-10-21,best_model.pkl,0.87,0.80,1.0
     ```

---

4. **Wersjonowanie danych i modeli**
   - Użyj jednego z podejść:
     - **DVC (Data Version Control)** – śledzenie plików w `data/` i modeli w `data/06_reporting/`.
       ```bash
       dvc init
       dvc add data/reporting/best_model.pkl
       git add data/reporting/best_model.pkl.dvc
       git commit -m "Add versioned model"
       git tag -l "v1.8.5*"
       ```
     - **Kedro Datasets** – zapewnij kontrolę wersji poprzez zapis modeli z nazwą zawierającą wersję lub timestamp (np. `model_v1.pkl`, `model_v2.pkl`).

---

5. **Raport z jakości i interpretacja wyników**
   - Stwórz plik `docs/model_evaluation_report.md` zawierający:
     - porównanie metryk (train, val, test),
     - interpretację wyników (np. które cechy mają największy wpływ),
     - rekomendacje dla dalszego doskonalenia modelu.  

---

## 🧱 Struktura katalogów (po zajęciach 5)

```

ai_project_teamX/
│
├── data/
│   ├── model_input/
│   └── reporting/
│       ├── baseline_metrics.json
│       ├── test_metrics.json
│       ├── model_versions.csv
│       ├── best_model.pkl
│       └── best_model.pkl.dvc
│
├── docs/
│   ├── modeling_report.md
│   ├── model_evaluation_report.md
│   └── plots/
│       ├── confusion_matrix.png
│       ├── feature_importance.png
│       └── shap_summary.png
│
├── src/
│   └── ai_project_teamX/
│       ├── pipelines/
│       │   └── evaluation/
│       │       ├── nodes.py
│       │       └── pipeline.py
│       └── pipeline_registry.py
│
└── README.md

```

---

## 📊 Ocena — Rezultat końcowy po zajęciach 5 (5 punktów)

| Nr | Element oceny | Opis | Punkty maks. | Punkty uzyskane |
|----|----------------|------|---------------|-----------------|
| 1️⃣ | **Walidacja krzyżowa i testy końcowe** | Obliczone metryki jakości na zbiorze testowym | **2 pkt** | |
| 2️⃣ | **Analiza błędów i interpretacja** | Macierz pomyłek, feature importance lub SHAP | **1 pkt** | |
| 3️⃣ | **Wersjonowanie modeli / logging wyników** | Użycie MLflow lub prostego systemu wersji modeli | **1 pkt** | |
| 4️⃣ | **Raport (`docs/model_evaluation_report.md`)** | Wnioski z ewaluacji i interpretacja wyników | **1 pkt** | |
|   | **Łącznie** |  | **5 pkt** | |

---

## 🧠 Wskazówki dla zespołów
- Nie ograniczaj się do jednej metryki — porównuj różne aspekty jakości (np. precyzja vs recall).  
- Jeśli używasz MLflow, zachowaj zrzut ekranu z dashboardu w folderze `docs/plots/`.  
- DVC można wykorzystać nie tylko do modeli, ale też do danych — szczególnie przy większych zbiorach.  
- Zadbaj, by raport był zrozumiały dla osób nietechnicznych (np. zespół biznesowy).

---

## 🧭 Oddanie przez Teams — wyłącznie jako link (nie plik tekstowy)

Prześlij **link do brancha** z rozwiązaniem tego etapu. Po każdej poprawie — **nowy link**. Commity po ostatecznym terminie = oszustwo.

- 📦 Link do brancha z rozwiązaniem (Zajęcia 5): [link tutaj]  
- 📊 MLflow dashboard (jeśli użyty): [link tutaj]  
- 📄 Raport oceny jakości: [link tutaj]  

---

## 📅 Zadania na kolejne zajęcia (Zajęcia 6)
1. Przygotować środowisko Airflow do automatyzacji pipeline’ów.  
2. Utworzyć DAG obejmujący EDA → Preprocessing → Modelowanie → Ewaluację.  
3. Uruchomić automatycznie cały proces i zarejestrować wyniki.

---

> 💡 **Wskazówka:**  
> Każdy kolejny etap projektu powinien bazować na powtarzalnym i mierzalnym procesie.  
> Wersjonowanie modeli i danych to fundament profesjonalnego MLOps!

