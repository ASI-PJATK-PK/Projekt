# 🧠 Projekt AI — Zajęcia 1: Planowanie i definicja projektu

## 🎯 Cel zajęć
Celem pierwszych zajęć jest rozpoczęcie pracy nad projektem zespołowym z zakresu **architektury systemów sztucznej inteligencji**.  
Zespół ma zdefiniować temat projektu, zaplanować jego strukturę, stworzyć architekturę systemu oraz przygotować repozytorium z bazowym szkieletem projektu w **Kedro**.

Studenci rozpoczynają zespół projektowy (1–4 osoby), który przez kolejne tygodnie zrealizuje kompletny system AI.
Pierwsze zajęcia to moment na ustalenie kierunku, odpowiedzialności i podstaw technicznych.

## UWAGA:
W przypadku grup większych niż 1 osoba należy zbudować **Meta Model** składający się z tyłu mniejszych modeli ile jest osób  w grupie i każdy z nich ma mieć swój oddzielny dataset.

---

## 🧩 Zakres pracy

1. **Wybór tematu projektu**  
   - Określ problem, który chcesz rozwiązać (np. klasyfikacja, predykcja, rekomendacja, wykrywanie anomalii).  
   - Zdefiniuj wartość użytkową lub biznesową projektu.  
   - Wybierz dane, które będą wykorzystane (lub poszukaj otwartych źródeł).  

2. **Projekt architektury systemu**  
   - Zidentyfikuj główne moduły:
     - ETL / przetwarzanie danych (Kedro)
     - Trening modelu (AutoML lub własny)
     - API backend (FastAPI)
     - UI frontend (Streamlit/React)
     - Automatyzacja (Airflow)
     - Wdrożenie (Docker / Cloud)
   - Przygotuj diagram architektury i zapisz go w folderze `docs/`.

3. **Utworzenie repozytorium projektu**
   - Stwórz repozytorium w organizacji [ASI-PJATK-PK](https://github.com/ASI-PJATK-PK):  
     `ai_project_<team_name>`
   - Dodaj wszystkich członków zespołu.
   - Skonfiguruj:
     - `.gitignore`
     - `requirements.txt`
     - `LICENSE`
     - ten plik `README.md`
   - Zainicjuj projekt **Kedro**:
     ```bash
     kedro new
     kedro run
     ```

4. **Stworzenie backlogu projektu (GitHub Projects)**
   - W GitHub → zakładka **Projects → New Project (Board)**.  
   - Utwórz kolumny: `To Do`, `In Progress`, `Done`.  
   - Dodaj **minimum 10 zadań (issues)** odpowiadających etapom projektu:
     - Inicjalizacja Kedro  
     - EDA  
     - Preprocessing  
     - Modelowanie  
     - Ewaluacja  
     - Airflow  
     - API  
     - Frontend  
     - Docker  
     - Retrospektywa  

---

## 🧱 Struktura katalogów (propozycja)

````

ai_project_teamX/
│
├── docs/
│   └── architecture_diagram.png
│
├── src/
│   └── ai_project_teamX/   ← kod Kedro
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── models/
│
├── notebooks/
│
├── .gitignore
├── requirements.txt
├── LICENSE
└── README.md

````

---

## 📊 Ocena — Rezultat końcowy po zajęciach 1

| Nr | Element oceny | Opis | Punkty maks. | Punkty uzyskane |
|----|----------------|------|---------------|-----------------|
| 1️⃣ | **Repozytorium GitHub** | Utworzony projekt Kedro, README, `.gitignore`, `requirements.txt`, `LICENSE` | **10 pkt** | |
| 2️⃣ | **Backlog projektu w GitHub Projects** | Minimum 10 zadań w tablicy z przypisanymi osobami i statusem | **5 pkt** | |
| 3️⃣ | **Diagram architektury systemu (w folderze `docs/`)** | Diagram z modułami (ETL, ML, API, UI, Airflow, Docker) | **5 pkt** | |
|   | **Łącznie** |  | **20 pkt** | |

---

## 🧑‍💻 Członkowie zespołu

| Imię i nazwisko | Rola w projekcie | GitHub login |
|------------------|------------------|---------------|
|                  |                  |               |
|                  |                  |               |
|                  |                  |               |

---

## 🧭 Linki projektu - linki powinny być zwrócone przez Teamsy (najlepiej podpisane)

- 📦 Repozytorium GitHub: [link tutaj]  
- 🗂️ GitHub Project Board: [link tutaj]  
- 📜 Dokumentacja / diagram architektury: [link tutaj]  

---

## 📅 Zadania na kolejne zajęcia
1. Uzupełnić dokumentację w README o szczegóły projektu.  
2. Dokończyć diagram architektury.  
3. Przygotować dane do eksploracji (EDA) na zajęcia 2.  

---

> 💡 **Wskazówka:**  
> Zachowaj spójność nazw i struktur – dzięki temu kolejne zajęcia (Airflow, AutoML, Docker) będą prostsze do zintegrowania z Twoim kodem.
