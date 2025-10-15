# Enhancing threat intelligence for internet of things (IoT) systems

A full‑stack app to explore IoT devices, link them to critical infrastructure sectors, and surface known vulnerabilities with AI‑assisted insights.

-   Browse and manage IoT devices
-   Assign devices to one or more sectors
-   Auto‑enrich devices with NIST CVE data and generate AI summaries via Ollama
-   Color‑coded risk indicators and clean UI (SvelteKit + Tailwind + DaisyUI)

## 🐳 Quick Start ­(Docker)

Prerequisites: Docker Desktop with Compose. For GPU, install appropriate drivers and Docker toolkit.

<i>Using the `frontend-and-backend-images.tar` you can skip building the images local entirely:</i>

First run this command to load the images:

`docker load -i frontend-and-backend-images.tar`

[you can download the `frontend-and-backend-images.tar` file here](https://drive.google.com/file/d/1_Kn03Pox7IW2C7wbpI9gT5pL-MwlSWoh/view?usp=sharing)

Then run one of the following commands

-   CPU:
    -   `docker compose up -d --build`
-   NVIDIA GPU:
    -   `docker compose -f Compose.yaml -f Compose.nvidia.yaml up -d`
-   AMD GPU:
    -   `docker compose -f Compose.yaml -f Compose.amd.yaml up -d`

### 🛑 Stop the services

-   Remove the image and stop services

    -   `docker compose down -v` <br>
        <em>Note: `-v` will delete the database along with recently added IoT device(s)</em>

-   Stop Services
    -   `docker-compose stop`

## 📂 Accessible links

-   Frontend : http://localhost:4173/
-   Backend : http://localhost:8080/
-   Ollama : http://localhost:11434/

## 💾 Backend/Django links:

-   Admin: http://localhost:8080/admin/
    -   Username: `admin`
    -   Password: `admin`
-   API: http://localhost:8080/api/
-   Swagger UI: http://localhost:8080/docs/
-   ReDoc: http://localhost:8080/redoc/redoc/

# 🧑‍💻Dev Notes

## 🌳 Architecture

-   Frontend (SvelteKit): [Frontend](Frontend/README.md)
-   Backend (Django + SQLite): [BackEnd/iot_backend](BackEnd/iot_backend)
-   Local LLM (Ollama): HTTP API at :11434 (see [BackEnd/iot_backend/api.md](BackEnd/iot_backend/api.md))

### 📜 Key files:

-   Root compose files: [Compose.yaml](Compose.yaml), [Compose.nvidia.yaml](Compose.nvidia.yaml), [Compose.amd.yaml](Compose.amd.yaml)
-   Frontend files: [Frontend/src/routes/+page.svelte](Frontend/src/routes/+page.svelte)
-   Backend DB: [BackEnd/iot_backend/db.sqlite3](BackEnd/iot_backend/db.sqlite3)

## ⌨️ Development (without Docker)

### 📱 Frontend (SvelteKit):

-   `cd Frontend`
-   `yarn install`
-   `yarn run dev`
-   Build: `yarn run build`

### 🗃️Backend (Django):

-   `cd BackEnd`
-   💻 Create venv and install:
    -   `python -m venv venv-backend && source venv-backend/bin/activate`
    -   `pip install -r iot_backend/requirements.txt`
-   🐛 Turn on debug mode

    -   Create an .env file with the debug flag:
        -   macOS/Linux: `printf 'DJANGO_DEBUG="True"\n' > iot_backend/.env`
        -   Windows (PowerShell): `Set-Content -Path iot_backend/.env -Value 'DJANGO_DEBUG="True"'`

-   🚀 Migrate and run:
    -   `python iot_backend/manage.py migrate`
    -   `python iot_backend/manage.py runserver 0.0.0.0:8080`

## 🤖 Data enrichment and AI summaries

-   NIST CVE enrichment and AI summarization are triggered on device creation or via management utilities.
-   Batch update (management command):
    -   `python BackEnd/iot_backend/manage.py update_threats`
-   Utility script (optional): [BackEnd/update_all_device_threats.py](BackEnd/update_all_device_threats.py)
-   Ollama endpoints and examples (generate/chat/embed, model ops): see [BackEnd/iot_backend/api.md](BackEnd/iot_backend/api.md)
