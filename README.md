# SignBridge AI – Sign Language to Text Translator

![Python](https://img.shields.io/badge/Python-3.x-blue)
![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688)
![JavaScript](https://img.shields.io/badge/Frontend-Vanilla%20JavaScript-yellow)
![AI](https://img.shields.io/badge/AI-MediaPipe%20%7C%20PyTorch-orange)
![Languages](https://img.shields.io/badge/Sign%20Languages-ISL%20%7C%20ASL-green)

**SignBridge AI** is a web-based sign language translation project designed to help bridge communication between sign language users and people who may not understand sign language. It supports **Indian Sign Language (ISL)** and **American Sign Language (ASL)** through image, video, and live webcam input.

The application combines a browser-based frontend, a Python FastAPI backend, and computer-vision / machine-learning models to recognize gestures and present the results as text. Speech playback is also available through the browser's Web Speech API.

---

## Features

- **Image-to-text translation:** Upload an image containing a sign and view the predicted sign, confidence information, and visual landmark output.
- **Video-to-text translation:** Upload a video for background processing and view translation progress and transcript results.
- **Live webcam translation:** Use a camera stream for real-time gesture recognition through a WebSocket connection.
- **ISL and ASL support:** Select Indian Sign Language or American Sign Language.
- **Text-to-speech:** Listen to translated text using browser speech synthesis.
- **User accounts:** Register and log in using the authentication endpoints.
- **Translation history:** View previous translation records.
- **Dashboard:** Review translation and application analytics.
- **Emergency assistance interface:** Access a dedicated screen for communicating urgent assistance messages.

> **Note:** This is an academic software project and a prototype. Recognition quality depends on the supported vocabulary, model training data, camera quality, lighting, and signing conditions. It should not be relied on as a certified interpreter or as the only channel for emergency communication.

---

## Technology Stack

| Layer | Technologies |
|---|---|
| Frontend | HTML5, CSS3, JavaScript (ES6+) |
| Backend | Python, FastAPI, Uvicorn, Pydantic |
| Computer vision | OpenCV, Google MediaPipe Tasks |
| Machine learning | Scikit-learn, PyTorch |
| Database | MongoDB with an in-memory fallback |
| Authentication | JWT, bcrypt |
| Real-time communication | WebSockets |
| Testing | Pytest |

---

## Project Structure

```text
sign Language to text translator02/
├── ai_engine/
│   ├── datasets/              # ISL and ASL vocabulary files
│   ├── evaluation/            # Model evaluation reports
│   ├── inference/             # Gesture prediction engine
│   ├── preprocessing/         # Landmark extraction and normalization
│   ├── saved_models/          # Trained models and MediaPipe task files
│   └── training/              # Model training utilities
├── backend/
│   ├── app/
│   │   ├── core/              # Configuration and security
│   │   ├── database/          # Database connection and collections
│   │   ├── routes/            # API and WebSocket routes
│   │   ├── schemas/           # Request and response schemas
│   │   ├── services/          # Vision, sentence, and video services
│   │   └── main.py            # FastAPI application entry point
│   └── requirements.txt
├── docs/
│   ├── api_documentation.md
│   ├── architecture.md
│   ├── database_design.md
│   ├── team_responsibilities.md
│   └── viva_preparation.md
├── frontend/
│   ├── css/                   # Stylesheets
│   ├── js/                    # Frontend JavaScript
│   └── *.html                 # Application pages
├── storage/
│   ├── temporary/
│   ├── transcripts/
│   └── uploads/
├── tests/                     # Automated tests
├── .env.example               # Example environment configuration
├── .gitignore
├── conftest.py
└── README.md
```

---

## Getting Started

### 1. Prerequisites

Install the following before running the project:

- Python (Python 3.11 is a recommended starting point; use a version supported by the installed PyTorch and MediaPipe packages).
- pip (Python package installer).
- Git.
- MongoDB (optional; the application includes an in-memory fallback when MongoDB is unavailable).

A webcam is required for the live translation feature.

### 2. Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd "<YOUR_PROJECT_FOLDER>"
```

Replace the placeholders with your GitHub repository URL and the folder created by Git.

### 3. Create and activate a virtual environment

**Windows (PowerShell):**

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**Windows (Command Prompt):**

```bat
python -m venv venv
venv\Scripts\activate
```

**macOS / Linux:**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 4. Install dependencies

Run this command from the project root directory:

```bash
python -m pip install --upgrade pip
pip install -r backend/requirements.txt
```

### 5. Configure environment variables

Create a `.env` file by copying the example:

**Windows (PowerShell):**

```powershell
Copy-Item .env.example .env
```

**macOS / Linux:**

```bash
cp .env.example .env
```

Open `.env` and set a unique, private `SECRET_KEY` before using the application beyond local development. Configure `MONGODB_URI` and `MONGODB_DB_NAME` if you want to use MongoDB.

**Important:** Never commit your real `.env` file, passwords, tokens, or secret keys to GitHub. The `.env` file is listed in `.gitignore`.

### 6. Run the application

From the project root directory, start the FastAPI server:

```bash
python -m uvicorn backend.app.main:app --host 127.0.0.1 --port 8000 --reload
```

Open the application in your browser:

```text
http://127.0.0.1:8000/
```

Interactive API documentation is available at:

```text
http://127.0.0.1:8000/docs
```

Stop the server with `Ctrl + C`.

---

## How to Use

1. Start the application using the steps above.
2. Open the home page in your browser.
3. Register or log in if you want to use the account features.
4. Choose the translation mode:
   - **Image Translator** – upload a sign language image.
   - **Video Translator** – upload a video and follow its processing progress.
   - **Live Translator** – allow camera access and sign in front of the webcam.
5. Select **ISL** or **ASL**.
6. View the recognized sign or translated text. Use speech playback where available.
7. Visit the dashboard or history page to review available records.

---

## API Overview

The backend exposes REST API endpoints and a WebSocket endpoint.

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/api/auth/register` | Register a user |
| `POST` | `/api/auth/login` | Log in |
| `GET` | `/api/auth/me` | Get the current user profile |
| `GET` | `/api/translation/languages` | List supported sign languages |
| `POST` | `/api/translation/image` | Translate an image |
| `POST` | `/api/translation/video` | Submit a video for translation |
| `POST` | `/api/translation/emergency` | Submit an emergency message |
| `GET` | `/api/history` | Retrieve translation history |
| `GET` | `/api/dashboard/analytics` | Retrieve dashboard analytics |
| WebSocket | `/ws/live-translate` | Live webcam translation |

For request formats, response examples, job progress, transcript downloads, and WebSocket message details, see [`docs/api_documentation.md`](docs/api_documentation.md).

---

## Run Tests

From the project root, run:

```bash
python -m pytest tests -v
```

This runs the automated test suite for authentication, prediction, translation APIs, and WebSocket behavior.

---

## Documentation

Additional project documentation is available in the [`docs/`](docs/) directory:

- [System Architecture](docs/architecture.md)
- [API Documentation](docs/api_documentation.md)
- [Database Design](docs/database_design.md)
- [Team Responsibilities](docs/team_responsibilities.md)
- [Viva Preparation](docs/viva_preparation.md)

---

## Future Improvements

- Expand and diversify the ISL and ASL training datasets.
- Improve recognition of continuous signing and sentence-level context.
- Evaluate model accuracy with larger, independently collected datasets.
- Improve accessibility, multilingual output, and deployment support.
- Add further testing with different cameras, lighting conditions, and signing styles.

---

## Contributors

Add the project contributors and their roles here.

| Name | Contribution |
|---|---|
| Contributor 1 | Backend / AI-ML |
| Contributor 2 | Frontend / UI |

---

## License

No license has been specified for this repository yet. If you want others to reuse, modify, or distribute this project, choose and add an appropriate open-source license.
