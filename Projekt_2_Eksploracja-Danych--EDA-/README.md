# 🔍 Zajęcia 2: Eksploracja Danych (EDA)

## 🎯 Cel zajęć
Celem zajęć jest **zrozumienie danych i ich jakości** oraz przygotowanie wniosków, które pomogą w dalszych etapach projektu (czyszczenie, modelowanie).  
Zespół ma za zadanie przeprowadzić analizę eksploracyjną danych (EDA), zbudować pierwszy pipeline w Kedro oraz udokumentować wyniki w repozytorium.

## 📤 Zasady oddawania

1. **Nazwa repozytorium** musi zawierać numer s studenta lub studentów.
2. **Ten etap** musi mieć **dedykowany branch** z rozwiązaniem (np. `zajecia-2`).
3. **Oddanie** przez Microsoft Teams: **link do brancha** (jako **link**, nie plik tekstowy).
4. **Każda poprawa** = przesłanie **nowego linku**.
5. **Commity** po ostatecznym terminie oddania = **oszustwo**.

---

## 🧩 Zakres pracy

1. **Przygotowanie danych**
   - Umieść surowe dane w folderze `data/raw/`.
   - Skonfiguruj ścieżki do danych w pliku `conf/base/catalog.yml`, np.:
     ```yaml
     raw_data:
       type: pandas.CSVDataSet
       filepath: data/raw/dataset.csv
     ```
   - Stwórz pipeline `eda_pipeline` z pierwszymi `node` do wczytywania i analizy danych.

2. **Eksploracja danych (EDA)**
   - W notebooku (lub node’ach Kedro) wykonaj:
     - Analizę podstawowych statystyk (średnia, odchylenie, rozkład, typy zmiennych).
     - Wykrywanie braków danych i wartości odstających.
     - Wizualizacje:
       - histogramy i boxploty dla zmiennych numerycznych,
       - countploty / barploty dla zmiennych kategorycznych,
       - korelacje (heatmapa),
       - pary zmiennych (pairplot).
   - Zidentyfikuj potencjalne problemy z jakością danych (duplikaty, błędy, skala wartości).

3. **Integracja z Kedro**
   - Utwórz pipeline EDA:
     - `nodes/eda.py` – funkcje analizy i raportowania.
     - `pipelines/eda/pipeline.py` – definicja pipeline’u.
     - Dodaj pipeline do `src/<project_name>/pipeline_registry.py`:
       ```python
       from <project_name>.pipelines import eda as eda_pipeline
       def register_pipelines():
           return {"eda": eda_pipeline.create_pipeline()}
       ```
   - Uruchom pipeline:
     ```bash
     kedro run --pipeline=eda
     ```

4. **Dokumentacja wyników**
   - W folderze `notebooks/` umieść notebook z eksploracją danych, np. `notebooks/EDA_<team_name>.ipynb`.
   - W folderze `docs/` dodaj krótkie podsumowanie w pliku `eda_report.md`:
     - opis danych,
     - główne obserwacje,
     - propozycje dalszych kroków (np. czyszczenie, standaryzacja, feature engineering).

---

## 🧱 Struktura katalogów (po zajęciach 2)

````

ai_project_teamX/
│
├── conf/
│   └── base/
│       └── catalog.yml
│
├── data/
│   ├── raw/
│   │   └── dataset.csv
│   └── intermediate/
│
├── notebooks/
│   └── EDA_teamX.ipynb
│
├── src/
│   └── ai_project_teamX/
│       ├── pipelines/
│       │   └── eda/
│       │       ├── nodes.py
│       │       └── pipeline.py
│       └── pipeline_registry.py
│
├── docs/
│   ├── eda_report.md
│   └── architecture_diagram.png
│
├── .gitignore
├── requirements.txt
├── LICENSE
└── README.md

````

---

## 📊 Ocena — Rezultat końcowy po zajęciach 2 (10 punktów)

| Nr | Element oceny | Opis | Punkty maks. | Punkty uzyskane |
|----|----------------|------|---------------|-----------------|
| 1️⃣ | **Notebook EDA (`notebooks/EDA_teamX.ipynb`)** | Analiza danych, statystyki, wykresy, interpretacja wyników | **4 pkt** | |
| 2️⃣ | **Pipeline EDA w Kedro** | Poprawnie zdefiniowany pipeline i uruchomienie `kedro run --pipeline=eda` | **3 pkt** | |
| 3️⃣ | **Raport z wnioskami (`docs/eda_report.md`)** | Zwięzłe podsumowanie jakości danych i rekomendacje dalszych działań | **2 pkt** | |
| 4️⃣ | **Organizacja repozytorium** | Dane w `data/01_raw`, aktualizacja `catalog.yml`, czysta struktura projektu | **1 pkt** | |
|   | **Łącznie** |  | **10 pkt** | |

---

## 🧠 Wskazówki dla zespołów

- Stosuj `logging` w node’ach Kedro, by śledzić postęp analizy.  
- Dokumentuj obserwacje i decyzje (np. które zmienne usuwasz lub dlaczego).  
- Wizualizacje zapisuj jako pliki `.png` i dołącz do raportu (`docs/eda_report.md`).  
- EDA to **nie tylko wykresy**, ale też **rozumienie kontekstu danych**.

---

## 🧭 Oddanie przez Teams — wyłącznie jako link (nie plik tekstowy)

Prześlij **link do brancha** z rozwiązaniem tego etapu. Po każdej poprawie — **nowy przesłanie rozwiązania**. Commity po ostatecznym terminie = oszustwo.

- 📦 Link do brancha z rozwiązaniem (Zajęcia 2): [link tutaj]  
- 📘 Notebook EDA: [link tutaj]  
- 📄 Raport EDA: [link tutaj]  

---

## 📅 Zadania na kolejne zajęcia (Zajęcia 3)
1. Przygotować dane do modelowania – czyszczenie, standaryzacja, normalizacja.  
2. Zaprojektować pipeline `preprocessing_pipeline`.  
3. Opisać decyzje transformacyjne w dokumentacji.

---

> 💡 **Wskazówka:**  
> Dobrze przeprowadzona EDA pozwala zaoszczędzić godziny pracy przy budowie modelu. Zadbaj o czytelne wykresy i sensowne wnioski!
