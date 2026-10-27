# ⚙️ Zajęcia 6: Automatyzacja procesu (Airflow)

## 🎯 Cel zajęć
Celem zajęć jest **zautomatyzowanie całego procesu ETL + ML** przy użyciu **Apache Airflow**.  
Studenci nauczą się tworzyć i konfigurować przepływy zadań (DAG-i), które uruchamiają pipeline’y Kedro w ustalonej kolejności, monitorują ich przebieg oraz umożliwiają harmonogramowanie automatycznego działania systemu.

**Rezultat:** Działający DAG w Airflow uruchamiający pipeline’y Kedro.

**WAŻNA PRZYPOMNIENIE!!!**

**Grupy w których jest więcej niż 1 osoba muszą mieć już meta-model lub inne równoważne rozwiązanie !!! 
BEZ TEGO ZADANIE BĘDZIE OCENIONE NA 0 DLA WSZYSTKICH W GRUPIE !!!**

## 📤 Zasady oddawania

1. **Nazwa repozytorium** musi zawierać numer s studenta lub studentów.
2. **Ten etap** musi mieć **dedykowany branch** z rozwiązaniem (np. `zajecia-6`).
3. **Oddanie** przez Microsoft Teams: **link do brancha** (jako **link**, nie plik tekstowy).
4. **Każda poprawa** = przesłanie **nowego linku**.
5. **Commity** po ostatecznym terminie oddania = **oszustwo**.

---

## 🧩 Zakres pracy

### 1️⃣ Instalacja i konfiguracja Apache Airflow
1. Zainstaluj Airflow w środowisku projektu (lokalnie lub przez Docker):
   ```bash
   pip install apache-airflow
   airflow db init
   airflow users create \
     --username admin \
     --firstname admin \
     --lastname user \
     --role Admin \
     --email admin@example.com
   airflow webserver --port 8080
   airflow scheduler

2. Otwórz interfejs webowy Airflow:
   👉 [http://localhost:8080](http://localhost:8080)

3. Alternatywnie możesz uruchomić Airflow w kontenerze:

   ```bash
   docker run -d -p 8080:8080 apache/airflow:2.9.0
   ```

---

### 2️⃣ Integracja Airflow z Kedro

1. Utwórz folder `dags/` (jeśli nie istnieje).
2. W nim stwórz plik `kedro_dag.py` zawierający definicję przepływu zadań:

   ```python
   from airflow import DAG
   from airflow.operators.bash import BashOperator
   from datetime import datetime

   default_args = {
       "owner": "airflow",
       "depends_on_past": False,
       "start_date": datetime(2025, 10, 21),
       "retries": 1,
   }

   with DAG(
       dag_id="kedro_project_pipeline",
       default_args=default_args,
       schedule_interval=None,  # manualne uruchomienie
       catchup=False,
       tags=["kedro", "mlops"],
   ) as dag:

       eda = BashOperator(
           task_id="eda_pipeline",
           bash_command="cd /path/to/project && kedro run --pipeline=eda"
       )

       preprocessing = BashOperator(
           task_id="preprocessing_pipeline",
           bash_command="cd /path/to/project && kedro run --pipeline=preprocessing"
       )

       modeling = BashOperator(
           task_id="modeling_pipeline",
           bash_command="cd /path/to/project && kedro run --pipeline=modeling"
       )

       evaluation = BashOperator(
           task_id="evaluation_pipeline",
           bash_command="cd /path/to/project && kedro run --pipeline=evaluation"
       )

       eda >> preprocessing >> modeling >> evaluation
   ```
3. Skopiuj plik do katalogu Airflow:

   ```
   ~/airflow/dags/kedro_dag.py
   ```

---

### 3️⃣ Definiowanie i uruchamianie DAG-a

* Każdy pipeline Kedro (EDA, preprocessing, modeling, evaluation) jest osobnym **taskiem** w DAG-u.
* Zależności (`>>`) określają kolejność wykonywania zadań.
* Możesz dodać możliwość uruchamiania konkretnego pipeline’u:

  ```bash
  kedro run --pipeline={{ dag_run.conf['pipeline_name'] }}
  ```
* W Airflow UI:

  1. Znajdź swój DAG `kedro_project_pipeline`.
  2. Kliknij **Trigger DAG**.
  3. Monitoruj postęp zadań (statusy Success / Failed).
  4. Przejrzyj logi poszczególnych tasków.

---

### 4️⃣ Monitoring i harmonogramowanie

* Włącz **scheduler** i ustaw harmonogram uruchamiania DAG-u, np. codziennie o 8:00:

  ```python
  schedule_interval="0 8 * * *"
  ```
* Airflow zapewnia:

  * automatyczne ponawianie błędnych tasków (`retries`),
  * logowanie statusów,
  * możliwość wysyłania alertów (np. e-mail lub Slack webhook).

---

### 5️⃣ Dokumentacja i raport

1. Przygotuj plik `docs/airflow_report.md`, w którym umieścisz:

   * opis struktury DAG-u i zależności między pipeline’ami,
   * zrzuty ekranu z interfejsu Airflow (`DAG graph`, `Tree view`),
   * opis logów lub błędów napotkanych podczas uruchamiania,
   * wnioski i pomysły na usprawnienia (np. automatyczny retraining, notyfikacje).

2. Umieść zrzuty ekranu w `docs/screenshots/`.

---

## 🧱 Struktura projektu po zajęciach 6

```
ai_project_teamX/
│
├── dags/
│   └── kedro_dag.py
│
├── docs/
│   ├── airflow_report.md
│   └── screenshots/
│       ├── dag_graph.png
│       ├── task_success.png
│       └── tree_view.png
│
├── data/
│   ├── raw/
│   ├── intermediate/
│   ├── primary/
│   ├── model_input/
│   └── reporting/
│
├── src/
│   └── ai_project_teamX/
│       ├── pipelines/
│       │   ├── eda/
│       │   ├── preprocessing/
│       │   ├── modeling/
│       │   └── evaluation/
│       └── pipeline_registry.py
│
└── README.md
```

---

## 📊 Ocena — Rezultat końcowy po zajęciach 6 (30 punktów)

| Nr  | Element oceny                                        | Opis                                                              | Punkty maks. | Punkty uzyskane |
| --- | ---------------------------------------------------- | ----------------------------------------------------------------- | ------------ | --------------- |
| 1️⃣ | **Instalacja i konfiguracja Airflow**                | Poprawne środowisko, uruchomiony webserver i scheduler            | **5 pkt**    |                 |
| 2️⃣ | **Definicja DAG-a (`kedro_dag.py`)**                 | Zdefiniowany przepływ EDA → Preprocessing → Modeling → Evaluation | **10 pkt**   |                 |
| 3️⃣ | **Integracja z Kedro**                               | Pipeline’y Kedro uruchamiają się poprawnie z poziomu Airflow      | **5 pkt**    |                 |
| 4️⃣ | **Monitoring i harmonogramowanie**                   | Działający DAG, poprawne logi, sukces wykonania tasków            | **5 pkt**    |                 |
| 5️⃣ | **Raport i dokumentacja (`docs/airflow_report.md`)** | Zrzuty ekranu, opis DAG-u, analiza wyników i wnioski              | **5 pkt**    |                 |
|     | **Łącznie**                                          |                                                                   | **30 pkt**   |                 |

---

## 🧠 Wskazówki dla zespołów

* Upewnij się, że ścieżki w `bash_command` prowadzą do właściwego katalogu projektu.
* Testuj każdy pipeline osobno przed pełnym uruchomieniem DAG-u.
* Możesz dodać warunki logiczne (np. zatrzymanie procesu przy błędzie).
* Airflow pozwala na monitorowanie wszystkich zadań w czasie rzeczywistym — korzystaj z logów.

---

## 🧭 Oddanie przez Teams — wyłącznie jako link (nie plik tekstowy)

Prześlij **link do brancha** z rozwiązaniem tego etapu. Po każdej poprawie — **nowy link**. Commity po ostatecznym terminie = oszustwo.

* 📦 **Link do brancha z rozwiązaniem (Zajęcia 6):** [link tutaj]
* 🌐 **Airflow UI:** [http://localhost:8080](http://localhost:8080) (zrzut / opis w raporcie)
* 📄 **Raport Airflow:** `docs/airflow_report.md`

---

## 📅 Zadania na kolejne zajęcia (Zajęcia 7)

1. Stworzyć **API (FastAPI / Flask)** do serwowania predykcji modelu.
2. Przygotować **frontend (Streamlit / React)** do interakcji z API.
3. Połączyć oba komponenty w działającą aplikację webową.

---

> 💡 **Wskazówka:**
> Airflow to kluczowe narzędzie MLOps – dzięki niemu Twój projekt ML staje się automatyczny, skalowalny i gotowy do produkcji.

