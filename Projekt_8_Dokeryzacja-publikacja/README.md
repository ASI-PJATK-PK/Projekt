# 🐳 Zajęcia 8: Dokeryzacja i publikacja

## 🎯 Cel zajęć
Celem zajęć jest **wdrożenie aplikacji ML w kontenerze Docker**, dzięki czemu można ją uruchomić w dowolnym środowisku.  
Studenci nauczą się tworzyć obrazy Dockera, łączyć backend i frontend w jednej konfiguracji, testować lokalnie aplikację oraz publikować obraz w **DockerHub**.  
Dodatkowo, zespoły mogą uzyskać **punkty extra za wdrożenie rozwiązania w chmurze** (Render, Azure, GCP, AWS, HuggingFace Spaces itp.).

**Rezultat:** Opublikowany obraz Dockera i działająca aplikacja w kontenerze.

**WAŻNA PRZYPOMNIENIE!!!**

**Grupy w których jest więcej niż 1 osoba muszą mieć już meta-model lub inne równoważne rozwiązanie !!! 
BEZ TEGO ZADANIE BĘDZIE OCENIONE NA 0 DLA WSZYSTKICH W GRUPIE !!!**

---

## 🧩 Zakres pracy

### 1️⃣ Wprowadzenie do Docker
1. Sprawdź, czy masz zainstalowanego Dockera:
   ```bash
   docker --version
```

2. Uruchom Dockera Desktop lub usługę `dockerd`.
3. Zaloguj się do swojego konta DockerHub:

   ```bash
   docker login
   ```

---

### 2️⃣ Stworzenie pliku Dockerfile dla backendu

W folderze `app/` utwórz plik `Dockerfile`:

```dockerfile
# Etap 1 — obraz bazowy
FROM python:3.10-slim

# Ustaw katalog roboczy
WORKDIR /app

# Skopiuj pliki projektu
COPY ./app /app
COPY ./data/reporting/best_model.pkl /app/data/reporting/best_model.pkl
COPY ./requirements.txt /app/requirements.txt

# Instalacja zależności
RUN pip install --no-cache-dir -r requirements.txt

# Port API
EXPOSE 8000

# Komenda startowa
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Uruchom testowo:

```bash
docker build -t ai_project_backend .
docker run -p 8000:8000 ai_project_backend
```

Sprawdź działanie aplikacji pod adresem 👉 [http://localhost:8000/docs](http://localhost:8000/docs)

---

### 3️⃣ (Opcjonalnie) Dockerfile dla frontendu (Streamlit)

Jeśli używasz Streamlit / React, utwórz w folderze `frontend/` plik `Dockerfile`:

```dockerfile
FROM python:3.10-slim

WORKDIR /frontend
COPY ./frontend /frontend
RUN pip install streamlit requests

EXPOSE 8501
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

Uruchom testowo:

```bash
docker build -t ai_project_frontend -f frontend/Dockerfile .
docker run -p 8501:8501 ai_project_frontend
```

Frontend powinien działać pod adresem 👉 [http://localhost:8501](http://localhost:8501)

---

### 4️⃣ Połączenie usług – docker-compose

Utwórz w katalogu głównym plik `docker-compose.yml`:

```yaml
version: '3.8'
services:
  backend:
    build: 
      context: .
      dockerfile: app/Dockerfile
    ports:
      - "8000:8000"
    container_name: ai_backend
    networks:
      - ai_network

  frontend:
    build:
      context: .
      dockerfile: frontend/Dockerfile
    ports:
      - "8501:8501"
    container_name: ai_frontend
    depends_on:
      - backend
    networks:
      - ai_network

networks:
  ai_network:
    driver: bridge
```

Uruchom cały system:

```bash
docker-compose up --build
```

---

### 5️⃣ Publikacja w DockerHub

1. Nadaj nazwę obrazowi zgodną z kontem DockerHub:

   ```bash
   docker tag ai_project_backend <twoj_login>/ai_project_backend:latest
   ```
2. Wypchnij obraz:

   ```bash
   docker push <twoj_login>/ai_project_backend:latest
   ```
3. Potwierdź, że obraz jest dostępny publicznie na stronie DockerHub.
   Przykład:
   👉 [https://hub.docker.com/r/twoj_login/ai_project_backend](https://hub.docker.com/)

---

### 6️⃣ (EXTRA) Wdrożenie w chmurze 🌩️

Za dodatkowe 10 punktów możesz wdrożyć konteneryzowaną aplikację w jednym z rozwiązań chmurowych:

#### 🔹 Render.com

* Utwórz darmowe konto i nowy **Web Service**.
* Połącz z repozytorium GitHub (Render sam wykryje plik `Dockerfile`).
* Po wdrożeniu otrzymasz publiczny URL aplikacji.

#### 🔹 HuggingFace Spaces

* Stwórz nowy Space → wybierz **Docker** jako typ aplikacji.
* Wgraj pliki `Dockerfile` i `requirements.txt`.
* Po chwili aplikacja będzie działać online.

#### 🔹 Azure / AWS / GCP

* Skorzystaj z usług typu *App Service*, *ECS*, *Cloud Run* lub *App Engine*.
* Użyj `docker push` do wysłania obrazu do odpowiedniego registry (Azure Container Registry, ECR, GCR).

---

### 7️⃣ Dokumentacja projektu

Utwórz plik `docs/docker_report.md`, w którym umieścisz:

* listę kontenerów i opis ich funkcji,
* komendy użyte do budowy i uruchomienia,
* link do obrazu w DockerHub,
* (opcjonalnie) link do wdrożonej aplikacji w chmurze,
* zrzuty ekranu z działania aplikacji lokalnie i w chmurze.

---

## 🧱 Struktura projektu po zajęciach 8

```
ai_project_teamX/
│
├── app/
│   ├── main.py
│   └── Dockerfile
│
├── frontend/
│   ├── app.py
│   └── Dockerfile
│
├── data/
│   └── reporting/
│       └── best_model.pkl
│
├── docs/
│   ├── docker_report.md
│   └── screenshots/
│       ├── docker_run.png
│       ├── dockerhub.png
│       └── cloud_deploy.png
│
├── docker-compose.yml
├── requirements.txt
└── README.md
```

---

## 📊 Ocena — Rezultat końcowy po zajęciach 8 (do 20 punktów extra)

| Nr  | Element oceny                                      | Opis                                                                              | Punkty maks.               | Punkty uzyskane |
| --- | -------------------------------------------------- | --------------------------------------------------------------------------------- | -------------------------- | --------------- |
| 1️⃣ | **Dokeryzacja aplikacji**                          | Utworzenie Dockerfile dla backendu i (opcjonalnie) frontendu                      | **5 pkt**                  |                 |
| 2️⃣ | **Test lokalny (`docker run` / `docker-compose`)** | Poprawne uruchomienie kontenerów lokalnie                                         | **5 pkt**                  |                 |
| 3️⃣ | **Publikacja w DockerHub**                         | Publiczny obraz Dockera dostępny online                                           | **5 pkt**                  |                 |
| 4️⃣ | **(EXTRA)** **Wdrożenie w chmurze**                | Działająca aplikacja w chmurze (Render, Azure, GCP, AWS, HuggingFace Spaces itp.) | **+10 pkt**                |                 |
|     | **Łącznie**                                        |                                                                                   | **20 pkt (10 + 10 extra)** |                 |

---

## 🧠 Wskazówki dla zespołów

* Zadbaj o mały rozmiar obrazu (używaj `python:3.10-slim`, `--no-cache-dir`).
* Sprawdź, czy modele i dane są kopiowane do obrazu Dockera (częsty błąd).
* Upewnij się, że aplikacja działa lokalnie przed publikacją.
* Jeśli wdrażasz w chmurze — dołącz publiczny link w raporcie.

---

## 🧭 Linki projektu - linki powinny być zwrócone przez Teamsy (najlepiej podpisane)

* 🧱 **Backend:** [http://localhost:8000/docs](http://localhost:8000/docs)
* 💻 **Frontend:** [http://localhost:8501](http://localhost:8501)
* 🐋 **DockerHub:** [link do obrazu Dockera]
* ☁️ **Aplikacja w chmurze:** [link do wdrożenia]
* 📄 **Raport:** `docs/docker_report.md`

---

## 🏁 Zakończenie projektu

To ostatnie zajęcia projektu **„Od surowych danych do wdrożonego modelu AI”** 🎓
Na tym etapie każdy zespół ma pełen ekosystem:

* przetwarzanie danych (Kedro),
* automatyzację (Airflow),
* model ML,
* API i UI,
* oraz wdrożenie w kontenerze lub chmurze.

> 💡 **Wskazówka:**
> Upewnij się, że wszystkie artefakty projektu (repozytorium, raporty, obrazy, linki) są gotowe do prezentacji podczas retrospektywy (Zajęcia 9).

