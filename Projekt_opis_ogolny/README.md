# 🧠 Projekt: Od surowych danych do wdrożonego modelu AI

## 🎯 Cel projektu
Studenci tworzą kompletny pipeline ML — od **eksploracji danych** po **wdrożenie modelu** jako usługi webowej (Docker + opcjonalny deploy w chmurze).  
Każde zajęcia kończą się działającym artefaktem: kodem, pipeline’em lub komponentem systemu.

---

## 📚 Plan zajęć

### 🏗️ Zajęcia 1: Planowanie i definicja projektu
**Cele:** zrozumienie procesu, wybór tematu, podział ról, projektowanie architektury  

**Zakres:**
- Omówienie celu projektu i przykładowych tematów (np. klasyfikacja, rekomendacja, predykcja)  
- Tworzenie backlogu projektu (user stories, kamienie milowe)  
- Projektowanie architektury systemu (moduły: ETL, ML, API, UI)  
- Wprowadzenie do **Kedro** i zasad zarządzania projektem ML  
- Utworzenie repozytorium (GitHub/GitLab) i inicjalizacja projektu Kedro  

**Rezultat:** Repozytorium z utworzonym szkieletem Kedro i backlogiem zadań  

---

### 🔍 Zajęcia 2: Eksploracja danych (EDA)
**Cele:** zrozumienie danych i ich jakości  

**Zakres:**
- Wczytanie i analiza danych w Kedro (DataCatalog, pipelines)  
- Eksploracja: statystyki opisowe, wizualizacje, wykrywanie braków i anomalii  
- Wprowadzenie do **Jupyter Notebook** w kontekście Kedro  
- Dokumentacja wniosków z EDA w repozytorium  

**Rezultat:** Notebook z eksploracją danych, pipeline EDA w Kedro  

---

### 🧹 Zajęcia 3: Przygotowanie danych
**Cele:** stworzenie pipeline’u przetwarzania danych  

**Zakres:**
- Czyszczenie danych (braki, outliery, typy danych)  
- Standaryzacja i normalizacja (np. `StandardScaler`, `MinMaxScaler`)  
- Podział na zbiory treningowy / walidacyjny / testowy  
- Testy jednostkowe dla modułów przetwarzania danych  

**Rezultat:** Pipeline preprocessingowy w Kedro + gotowe dane wejściowe do modelu  

---

### 🤖 Zajęcia 4: Budowa modelu ML i AutoML
**Cele:** automatyzacja procesu budowy modeli i wybór najlepszego  

**Zakres:**
- Krótkie wprowadzenie do **AutoML** (np. PyCaret, Auto-sklearn, H2O AutoML)  
- Integracja z Kedro (node’y: trening, zapis modelu, ewaluacja)  
- Porównanie wyników kilku modeli (baseline, AutoML, własny model)  
- Omówienie metryk jakości (np. accuracy, F1, RMSE)  

**Rezultat:** Wytrenowany model + pipeline ML w Kedro  

---

### 📈 Zajęcia 5: Ocena jakości i wersjonowanie
**Cele:** ocena, rejestracja i monitoring modeli  

**Zakres:**
- Walidacja krzyżowa, testy końcowe  
- Wprowadzenie **MLflow** (opcjonalnie) lub prosty logging wyników  
- Analiza błędów modelu (confusion matrix, SHAP / feature importance)  
- Wersjonowanie danych i modeli (DVC lub Kedro datasets)  

**Rezultat:** Raport z jakości modelu + zapisana wersja modelu w repozytorium  

---

### ⚙️ Zajęcia 6: Automatyzacja procesu (Airflow)
**Cele:** zautomatyzować cały pipeline ETL + ML  

**Zakres:**
- Wprowadzenie do **Apache Airflow**  
- Integracja z Kedro (uruchamianie pipeline’ów)  
- Definiowanie **DAG-ów**: EDA, preprocessing, trening, ewaluacja  
- Monitoring i harmonogramowanie zadań  

**Rezultat:** Działający DAG w Airflow uruchamiający pipeline Kedro  

---

### 🌐 Zajęcia 7: Budowa aplikacji backendowej i frontendowej
**Cele:** umożliwić interakcję z modelem  

**Zakres:**
- Budowa prostego **API (FastAPI / Flask)** do predykcji  
- Wczytywanie wytrenowanego modelu i przetwarzanie zapytań  
- Prosty **frontend (React / Streamlit / Gradio)** do komunikacji z API  
- Testy integracyjne i dokumentacja (OpenAPI, Swagger)  

**Rezultat:** Działająca aplikacja webowa lokalnie  

---

### 🐳 Zajęcia 8: Dokeryzacja i publikacja (Opcjonalnie dla grup 1-osobowych)
**Cele:** wdrożenie aplikacji w kontenerze  

**Zakres:**
- Wprowadzenie do **Docker**: Dockerfile, docker-compose  
- Konteneryzacja aplikacji backend + frontend + model  
- Test lokalny (`docker run`)  
- Publikacja obrazu w **DockerHub**  
- (Opcjonalnie dla grup 2-osobowych) deploy na chmurze (np. Render, Azure, GCP, AWS, HuggingFace Spaces)
- Grupy 4 osobowe muszą zastosować **IaC** w rozwiązaniu chmurowym

**Rezultat:** Opublikowany obraz Dockera i działająca aplikacja w kontenerze  

---

### 💬 Zajęcia 9 (online): Retrospektywa i prezentacje
**Cele:** refleksja, prezentacja rezultatów i dyskusja nad ulepszeniami  

**Zakres:**
- Prezentacje zespołów: demo aplikacji + pipeline’u  
- Dyskusja o problemach i najlepszych praktykach  
- Feedback 360° (techniczny i projektowy)  
- Wnioski na przyszłość – jak można projekt rozwinąć  

**Rezultat:** 

| Nr zajęć | Temat zajęć | Kluczowe elementy | Maks. liczba punktów |
|-----------|-------------|------------------|----------------------|
| **1️⃣** | **Planowanie i definicja projektu** | Repozytorium z projektem Kedro, backlog GitHub Projects, diagram architektury systemu | **20 pkt** |
| **2️⃣** | **Eksploracja danych (EDA)** | Notebook z EDA, pipeline EDA w Kedro, raport z wnioskami | **10 pkt** |
| **3️⃣** | **Przygotowanie danych (Preprocessing)** | Pipeline czyszczenia i standaryzacji danych, podział zbiorów, testy jednostkowe | **10 pkt** |
| **4️⃣** | **Budowa modelu ML i AutoML** | Baseline, AutoML, własny model, integracja z Kedro, raport z wyników | **20 pkt** |
| **5️⃣** | **Ocena jakości i wersjonowanie** | Walidacja, analiza błędów, metryki, wersjonowanie modeli, raport | **5 pkt** |
| **6️⃣** | **Automatyzacja procesu (Airflow)** | Konfiguracja Airflow, definicja DAG-ów, integracja z Kedro, monitoring, raport | **30 pkt** |
| **7️⃣** | **Budowa aplikacji backendowej i frontendowej** | API (FastAPI / Flask), frontend (Streamlit / React / Gradio), testy integracyjne, dokumentacja | **20 pkt** |
| **8️⃣** | **Dokeryzacja i publikacja** | Dockerfile, docker-compose, test lokalny, publikacja w DockerHub, (EXTRA: wdrożenie w chmurze) | **10 pkt + 10 pkt extra** |
| **1️⃣-8️⃣** | **Suma** | Suma za cały projekt | **125 pkt + 20 pkt extra** |

---

> 💡 **Wskazówka dla studentów:**  
> Traktujcie projekt jak prawdziwy system AI — każdy etap to komponent większej architektury.  
> Dobrze zaprojektowane pipeline’y i dokumentacja ułatwią integrację kolejnych części projektu.
