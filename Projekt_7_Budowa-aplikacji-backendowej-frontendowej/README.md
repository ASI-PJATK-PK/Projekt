# 🌐 Zajęcia 7: Budowa aplikacji backendowej i frontendowej

## 🎯 Cel zajęć
Celem zajęć jest **umożliwienie interakcji z wytrenowanym modelem ML** poprzez stworzenie aplikacji webowej — składającej się z prostego **backendu (API)** oraz **frontendu (UI)**.  
Dzięki temu użytkownik będzie mógł wprowadzać dane wejściowe i otrzymywać predykcję modelu w czasie rzeczywistym.

**Rezultat:** Działająca aplikacja webowa lokalnie (backend + frontend).

**WAŻNA PRZYPOMNIENIE!!!**

**Grupy w których jest więcej niż 1 osoba muszą mieć już meta-model lub inne równoważne rozwiązanie !!! 
BEZ TEGO ZADANIE BĘDZIE OCENIONE NA 0 DLA WSZYSTKICH W GRUPIE !!!**

---

## 🧩 Zakres pracy

### 1️⃣ Backend — API do predykcji (FastAPI / Flask)

1. Utwórz folder `app/` i plik `main.py`.
2. Zaimplementuj prosty serwer API, który:
   - wczytuje wytrenowany model (`best_model.pkl`),
   - przyjmuje dane wejściowe w formacie JSON,
   - zwraca wynik predykcji.

#### Przykład (FastAPI):
```python
from fastapi import FastAPI
from pydantic import BaseModel
import pickle
import pandas as pd

app = FastAPI(title="AI Model API", version="1.0")

# Wczytanie modelu
with open("data/reporting/best_model.pkl", "rb") as f:
    model = pickle.load(f)

# Definicja danych wejściowych
class InputData(BaseModel):
    feature1: float
    feature2: float
    feature3: float

@app.get("/")
def home():
    return {"message": "Model API działa poprawnie 🚀"}

@app.post("/predict")
def predict(data: InputData):
    df = pd.DataFrame([data.dict()])
    prediction = model.predict(df)[0]
    return {"prediction": float(prediction)}


3. Uruchom serwer:

   ```bash
   uvicorn app.main:app --reload
   ```
4. Otwórz przeglądarkę:
   👉 [http://localhost:8000/docs](http://localhost:8000/docs)
   (automatycznie generowana dokumentacja **Swagger / OpenAPI**)

---

### 2️⃣ Frontend — prosty interfejs użytkownika (Streamlit / React / Gradio)

#### 🔹 Opcja 1: Streamlit

1. Utwórz plik `frontend/app.py`:

   ```python
   import streamlit as st
   import requests

   st.title("🌐 Predykcja modelu ML")
   st.write("Wprowadź dane wejściowe, aby uzyskać predykcję:")

   feature1 = st.number_input("Feature 1")
   feature2 = st.number_input("Feature 2")
   feature3 = st.number_input("Feature 3")

   if st.button("Wyślij do modelu"):
       response = requests.post(
           "http://127.0.0.1:8000/predict",
           json={"feature1": feature1, "feature2": feature2, "feature3": feature3}
       )
       st.success(f"Predykcja modelu: {response.json()['prediction']}")
   ```

2. Uruchom:

   ```bash
   streamlit run frontend/app.py
   ```

3. Otwórz przeglądarkę:
   👉 [http://localhost:8501](http://localhost:8501)

#### 🔹 Opcja 2: React lub Gradio

* **React** – utwórz prosty formularz HTML/JS i komunikację z API za pomocą `fetch()`.
* **Gradio** – szybkie GUI bez kodu frontendu:

  ```python
  import gradio as gr
  import requests

  def predict(feature1, feature2, feature3):
      response = requests.post("http://127.0.0.1:8000/predict",
                               json={"feature1": feature1, "feature2": feature2, "feature3": feature3})
      return response.json()["prediction"]

  demo = gr.Interface(fn=predict,
                      inputs=["number", "number", "number"],
                      outputs="number",
                      title="ML Model Prediction")
  demo.launch()
  ```

---

### 3️⃣ Testy integracyjne

* Dodaj plik `tests/test_api.py`:

  ```python
  from fastapi.testclient import TestClient
  from app.main import app

  client = TestClient(app)

  def test_home():
      response = client.get("/")
      assert response.status_code == 200
      assert "Model API" in response.text or "działa" in response.text

  def test_predict():
      payload = {"feature1": 1.2, "feature2": 3.4, "feature3": 5.6}
      response = client.post("/predict", json=payload)
      assert response.status_code == 200
      assert "prediction" in response.json()
  ```

* Uruchom testy:

  ```bash
  pytest tests/test_api.py
  ```

---

### 4️⃣ Dokumentacja projektu

* Utwórz plik `docs/webapp_report.md`, w którym zawrzesz:

  * opis architektury aplikacji (API + frontend),
  * instrukcję uruchomienia aplikacji lokalnie,
  * zrzuty ekranu działania interfejsu,
  * przykładowe zapytania i odpowiedzi z API.

---

## 🧱 Struktura projektu po zajęciach 7

```
ai_project_teamX/
│
├── app/
│   └── main.py
│
├── frontend/
│   └── app.py
│
├── data/
│   └── reporting/
│       └── best_model.pkl
│
├── tests/
│   └── test_api.py
│
├── docs/
│   └── webapp_report.md
│
└── README.md
```

---

## 📊 Ocena — Rezultat końcowy po zajęciach 7 (20 punktów)

| Nr  | Element oceny                              | Opis                                                   | Punkty maks. | Punkty uzyskane |
| --- | ------------------------------------------ | ------------------------------------------------------ | ------------ | --------------- |
| 1️⃣ | **Backend (API)**                          | Działające API w FastAPI/Flask, poprawne endpointy     | **5 pkt**    |                 |
| 2️⃣ | **Integracja z modelem**                   | Wczytywanie modelu i poprawna obsługa predykcji        | **5 pkt**    |                 |
| 3️⃣ | **Frontend (Streamlit / React / Gradio)**  | Interfejs użytkownika do komunikacji z API             | **5 pkt**    |                 |
| 4️⃣ | **Testy integracyjne**                     | Sprawdzenie poprawności działania API                  | **2 pkt**    |                 |
| 5️⃣ | **Dokumentacja (`docs/webapp_report.md`)** | Opis działania, instrukcja uruchomienia, zrzuty ekranu | **3 pkt**    |                 |
|     | **Łącznie**                                |                                                        | **20 pkt**   |                 |

---

## 🧠 Wskazówki dla zespołów

* Upewnij się, że **ścieżki i porty** między backendem a frontendem są zgodne.
* API powinno działać niezależnie od frontend’u — testuj je osobno przez `/docs`.
* Dodaj walidację danych wejściowych (`pydantic.BaseModel` lub formularz w UI).
* Przygotuj **zrzuty ekranu** pokazujące komunikację modelu z interfejsem.

---

## 🧭 Linki projektu - linki powinny być zwrócone przez Teamsy (najlepiej podpisane)

* 🌐 **Backend (API):** Screnshot do [http://localhost:8000/docs](http://localhost:8000/docs) <- tylko jeśli działa !!!
* 💻 **Frontend (Streamlit):** Screnshot do  [http://localhost:8501](http://localhost:8501) <- tylko jeśli działa !!!
* 📄 **Raport webowy:** `docs/webapp_report.md`
* 📦 **Repozytorium projektu:** [link do GitHub]

---

## 📅 Zadania na kolejne zajęcia (Zajęcia 8)

1. Dokeryzacja aplikacji (backend + frontend).
2. Budowa pliku `Dockerfile` i `docker-compose.yml`.
3. Publikacja obrazu w **DockerHub** lub wdrożenie w chmurze.

---

> 💡 **Wskazówka:**
> Po tym etapie Wasz projekt zyskuje „twarz” — można z niego korzystać jak z prawdziwej aplikacji AI!
> To doskonały moment na dopracowanie UX i przygotowanie do wdrożenia w Dockerze.
