# 🤖 Zajęcia 4: Budowa modelu ML i AutoML

## 🎯 Cel zajęć
Celem zajęć jest **automatyzacja procesu budowy modeli ML** oraz wybór najlepszego podejścia spośród modeli bazowych, AutoML i własnego modelu.  
Zespół ma zbudować pipeline treningowy w **Kedro**, zrealizować ewaluację jakości i zapisać wytrenowany model.

---

## 🧩 Zakres pracy

1. **Przygotowanie danych wejściowych**
   - Użyj danych z poprzedniego etapu (`data/model_input/train.csv`, `val.csv`, `test.csv`).
   - Zdefiniuj w `catalog.yml` źródła danych:
     ```yaml
     train_data:
       type: pandas.CSVDataSet
       filepath: data/model_input/train.csv

     val_data:
       type: pandas.CSVDataSet
       filepath: data/model_input/val.csv

     test_data:
       type: pandas.CSVDataSet
       filepath: data/model_input/test.csv
     ```

---

2. **Model bazowy (baseline)**
   - Stwórz prosty model bazowy — np. `DummyClassifier` lub `LinearRegression` (zależnie od typu problemu).
   - Oblicz podstawowe metryki (np. accuracy, F1, RMSE).
   - Zapisz wyniki w pliku `data/reporting/baseline_metrics.json`.

---

3. **AutoML**
   - Wybierz jedną z bibliotek AutoML:
     - [PyCaret](https://pycaret.gitbook.io/docs/),  
     - [Auto-sklearn](https://automl.github.io/auto-sklearn/),  
     - [H2O AutoML](https://docs.h2o.ai/h2o/latest-stable/h2o-docs/automl.html).
   - Uruchom eksperyment AutoML na danych treningowych i walidacyjnych.
   - Zapisz:
     - najlepszy model (`data/reporting/automl_model.pkl`),
     - ranking modeli (`data/reporting/automl_results.csv`),
     - metryki (`data/reporting/automl_metrics.json`).

---

4. **Dodatkowo Własny model ML - szczegóły jaki to model było opisane na zajęciach. Tylko osoby z zajęć mąją go zrobić**
   - Zaimplementuj własny model, np.:
     - klasyfikacja: RandomForest, XGBoost, LightGBM,  
     - regresja: LinearRegression, CatBoost, Ridge.  
   - Ustal hiperparametry (grid search lub ręcznie).  
   - Zapisz metryki jakości i model:
     ```yaml
     best_model:
       type: pickle.PickleDataSet
       filepath: data/reporting/best_model.pkl
     ```
   - Porównaj wyniki (baseline vs AutoML vs własny model).

---

5. **Integracja z Kedro**
   - Utwórz pipeline `modeling`:
     - `src/<project_name>/pipelines/modeling/nodes.py`
     - `src/<project_name>/pipelines/modeling/pipeline.py`
   - Przykładowy pipeline:
     ```python
     from kedro.pipeline import Pipeline, node
     from .nodes import train_baseline, train_automl, train_custom, evaluate_models

     def create_pipeline(**kwargs):
         return Pipeline([
             node(train_baseline, ["train_data", "val_data"], ["baseline_model", "baseline_metrics"]),
             node(train_automl, ["train_data", "val_data"], ["automl_model", "automl_metrics"]),
             node(train_custom, ["train_data", "val_data"], ["custom_model", "custom_metrics"]),
             node(evaluate_models, ["baseline_metrics", "automl_metrics", "custom_metrics"], "model_comparison"),
         ])
     ```

   - Zarejestruj pipeline w `pipeline_registry.py`:
     ```python
     from <project_name>.pipelines import modeling as modeling_pipeline

     def register_pipelines():
         return {"modeling": modeling_pipeline.create_pipeline()}
     ```

   - Uruchom pipeline:
     ```bash
     kedro run --pipeline=modeling
     ```

---

6. **Ewaluacja i raport**
   - Porównaj wyniki trzech podejść:
     - baseline,
     - AutoML,
     - własny model.
   - Oblicz i zapisz metryki:
     - klasyfikacja: accuracy, precision, recall, F1-score, ROC AUC;
     - regresja: RMSE, MAE, R².
   - Przygotuj raport (`docs/modeling_report.md`) z tabelą porównawczą wyników i wnioskami.

---

7. **Wizualizacja wyników (opcjonalnie)**
   - Można wykorzystać:
     - `matplotlib` lub `seaborn` do wykresów błędów, ważności cech, itp.
     - zapis wykresów w `docs/plots/`.

---

## 🧱 Struktura katalogów (po zajęciach 4)

```

ai_project_teamX/
│
├── data/
│   ├── model_input/
│   │   ├── train.csv
│   │   ├── val.csv
│   │   └── test.csv
│   └── reporting/
│       ├── baseline_model.pkl
│       ├── automl_model.pkl
│       ├── best_model.pkl
│       ├── automl_results.csv
│       ├── baseline_metrics.json
│       ├── automl_metrics.json
│       ├── custom_metrics.json
│       └── model_comparison.json
│
├── src/
│   └── ai_project_teamX/
│       ├── pipelines/
│       │   └── modeling/
│       │       ├── nodes.py
│       │       └── pipeline.py
│       └── pipeline_registry.py
│
├── docs/
│   ├── modeling_report.md
│   └── plots/
│       ├── feature_importance.png
│       └── metrics_comparison.png
│
└── README.md

```

---

## 📊 Ocena — Rezultat końcowy po zajęciach 4 (20 punktów)

| Nr | Element oceny | Opis | Punkty maks. | Punkty uzyskane |
|----|----------------|------|---------------|-----------------|
| 1️⃣ | **Model bazowy (Baseline)** | Poprawnie zbudowany i oceniony model odniesienia | **3 pkt** | |
| 2️⃣ | **AutoML (np. PyCaret / Auto-sklearn)** | Uruchomiony proces AutoML, zapis modelu i wyników | **6 pkt** | |
| 3️⃣ | **Własny model ML** | Model przygotowany, przetrenowany i oceniony | **5 pkt** | |
| 4️⃣ | **Integracja w Kedro (pipeline modeling)** | Pipeline działa i łączy wszystkie elementy (trening + ewaluacja) | **3 pkt** | |
| 5️⃣ | **Raport i dokumentacja (`docs/modeling_report.md`)** | Porównanie wyników, interpretacja, wnioski | **3 pkt** | |
|   | **Łącznie** |  | **20 pkt** | |

---

## 🧠 Wskazówki dla zespołów

- Porównaj modele **ilościowo (metryki)** i **jakościowo (wnioski, interpretacja)**.  
- Zadbaj o powtarzalność eksperymentów — zapisuj użyte parametry i seed randomizacji.  
- Zrób backup najlepszego modelu (pickle + metryki).  
- Upewnij się, że pipeline działa całościowo przez `kedro run`.

---

## 🧭 Linki projektu - linki powinny być zwrócone przez Teamsy (najlepiej podpisane)

- 📦 Repozytorium GitHub: [link tutaj]  
- 📄 Raport z modelowania: [link tutaj]  
- 📘 Notebook AutoML (jeśli był użyty): [link tutaj]  

---

## 📅 Zadania na kolejne zajęcia (Zajęcia 5)
1. Przygotować pipeline oceny jakości (ewaluacja, analiza błędów, interpretowalność).  
2. Zapoznać się z narzędziami MLflow / wandb do rejestracji modeli.  
3. Wygenerować raport końcowy z porównaniem modeli.

---

> 💡 **Wskazówka:**  
> Nie chodzi o znalezienie „najlepszego” modelu, ale o **zrozumienie procesu wyboru** – porównuj, analizuj i uzasadniaj decyzje!

---

Czy chcesz, żebym przygotował teraz także **szablon pliku `nodes.py`** z przykładowymi funkcjami dla `train_baseline`, `train_automl`, `train_custom` i `evaluate_models` (do wklejenia w projekt Kedro)?
Mogę też dodać przykładowy fragment raportu `modeling_report.md` z tabelą wyników i interpretacją.
