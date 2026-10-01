# 🧹 Zajęcia 3: Przygotowanie danych (Data Preprocessing)

## 🎯 Cel zajęć
Celem zajęć jest **stworzenie pipeline’u przetwarzania danych w Kedro**, który automatycznie czyści dane, standaryzuje je i przygotowuje do trenowania modelu ML.  
Efektem końcowym będzie **zestaw gotowych danych treningowych i testowych** oraz pipeline możliwy do uruchomienia komendą `kedro run --pipeline=preprocessing`.

---

## 🧩 Zakres pracy

1. **Czyszczenie danych (Cleaning)**
   - Opracuj funkcje usuwające lub uzupełniające braki danych:
     - Uzupełnianie wartości średnią, medianą lub najczęstszą wartością.
     - Usuwanie kolumn lub wierszy o zbyt wielu brakach.
   - Obsłuż błędne typy danych i konwersje (np. `object` → `float`).
   - Wykryj i usuń **outliery** (np. metodą IQR lub 3σ).
   - Zapisz przetworzone dane jako dataset `data/intermediate/clean_data.csv`.

2. **Standaryzacja i normalizacja**
   - Zaimplementuj transformacje w module Kedro:
     - `StandardScaler` lub `MinMaxScaler` z biblioteki `sklearn.preprocessing`.
   - Zapisz znormalizowany zestaw danych jako:
     - `data/primary/scaled_data.csv`
   - W pliku `catalog.yml` dodaj wpisy:
     ```yaml
     clean_data:
       type: pandas.CSVDataSet
       filepath: data/intermediate/clean_data.csv

     scaled_data:
       type: pandas.CSVDataSet
       filepath: data/primary/scaled_data.csv
     ```

3. **Podział na zbiory (train/val/test)**
   - Użyj funkcji `train_test_split` (z `sklearn.model_selection`):
     - 70% train, 15% val, 15% test (lub inny sensowny podział).
   - Zapisz wyniki w:
     ```
     data/model_input/train.csv
     data/model_input/val.csv
     data/model_input/test.csv
     ```

4. **Pipeline Kedro**
   - Utwórz pipeline `preprocessing`:
     - `src/<project_name>/pipelines/preprocessing/nodes.py`
     - `src/<project_name>/pipelines/preprocessing/pipeline.py`
   - Przykładowa definicja pipeline’u:
     ```python
     from kedro.pipeline import Pipeline, node
     from .nodes import clean_data, scale_data, split_data

     def create_pipeline(**kwargs):
         return Pipeline([
             node(clean_data, "raw_data", "clean_data"),
             node(scale_data, "clean_data", "scaled_data"),
             node(split_data, "scaled_data", ["train_data", "val_data", "test_data"]),
         ])
     ```
   - Zarejestruj pipeline w `pipeline_registry.py`:
     ```python
     from <project_name>.pipelines import preprocessing as preprocessing_pipeline

     def register_pipelines():
         return {"preprocessing": preprocessing_pipeline.create_pipeline()}
     ```

   - Uruchom:
     ```bash
     kedro run --pipeline=preprocessing
     ```

5. **Testy jednostkowe**
   - W folderze `src/tests/pipelines/preprocessing/` utwórz testy np. `test_nodes.py`.
   - Sprawdź poprawność działania funkcji:
     - czy nie zwracają pustych DataFrame,
     - czy liczba wierszy się zgadza,
     - czy dane są wystandaryzowane (średnia ≈ 0, std ≈ 1).
   - Uruchom testy:
     ```bash
     pytest src/tests/pipelines/preprocessing/
     ```

---

## 🧱 Struktura katalogów (po zajęciach 3)

```

ai_project_teamX/
│
├── conf/
│   └── base/
│       └── catalog.yml
│
├── data/
│   ├── raw/
│   ├── intermediate/
│   │   └── clean_data.csv
│   ├── primary/
│   │   └── scaled_data.csv
│   └── model_input/
│       ├── train.csv
│       ├── val.csv
│       └── test.csv
│
├── src/
│   └── ai_project_teamX/
│       ├── pipelines/
│       │   └── preprocessing/
│       │       ├── nodes.py
│       │       └── pipeline.py
│       └── pipeline_registry.py
│
├── src/tests/pipelines/preprocessing/
│   └── test_nodes.py
│
├── docs/
│   ├── eda_report.md
│   ├── preprocessing_report.md
│   └── architecture_diagram.png
│
└── README.md

```

---

## 📊 Ocena — Rezultat końcowy po zajęciach 3 (10 punktów)

| Nr | Element oceny | Opis | Punkty maks. | Punkty uzyskane |
|----|----------------|------|---------------|-----------------|
| 1️⃣ | **Czyszczenie danych (`clean_data`)** | Poprawne usunięcie braków, outlierów, konwersje typów | **3 pkt** | |
| 2️⃣ | **Standaryzacja i normalizacja (`scaled_data`)** | Zastosowanie scalerów i zapis danych | **2 pkt** | |
| 3️⃣ | **Podział danych (train/val/test)** | Poprawny podział zbiorów i zapis w `data/05_model_input/` | **2 pkt** | |
| 4️⃣ | **Pipeline Kedro (`preprocessing`)** | Pipeline działa i przetwarza dane zgodnie z opisem | **2 pkt** | |
| 5️⃣ | **Testy jednostkowe** | Poprawne testy funkcji czyszczących i przetwarzających | **1 pkt** | |
|   | **Łącznie** |  | **10 pkt** | |

---

## 🧠 Wskazówki dla zespołów

- Testuj każdą funkcję osobno – błędy w preprocessing’u są najczęstszą przyczyną złych wyników modeli.  
- Zapisuj wyniki pipeline’u – mogą przydać się w następnych etapach do re-trainu.  
- Przygotuj krótki raport (`docs/preprocessing_report.md`) z opisem decyzji i transformacji.  
- Zadbaj o spójność nazw datasetów w `catalog.yml` i pipeline’ach.

---

## 🧭 Linki projektu - linki powinny być zwrócone przez Teamsy (najlepiej podpisane)

- 📦 Repozytorium GitHub: [link tutaj]  
- 🧪 Testy jednostkowe: [link tutaj]  
- 📄 Raport preprocessingowy: [link tutaj]  

---

## 📅 Zadania na kolejne zajęcia (Zajęcia 4)
1. Przygotować pipeline do trenowania modeli (AutoML lub własny).  
2. Przeprowadzić pierwsze eksperymenty z parametrami modelu.  
3. Wygenerować metryki jakości i zapisać je w repozytorium.

---

> 💡 **Wskazówka:**  
> Dobrze przygotowane dane to 80% sukcesu w projekcie ML.  
> Jeśli preprocessing działa automatycznie — Twoje kolejne pipeline’y staną się znacznie prostsze do utrzymania.
