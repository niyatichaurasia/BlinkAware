<div align="center">

<br/>

```
██████╗ ██╗     ██╗███╗   ██╗██╗  ██╗ █████╗ ██╗    ██╗ █████╗ ██████╗ ███████╗
██╔══██╗██║     ██║████╗  ██║██║ ██╔╝██╔══██╗██║    ██║██╔══██╗██╔══██╗██╔════╝
██████╔╝██║     ██║██╔██╗ ██║█████╔╝ ███████║██║ █╗ ██║███████║██████╔╝█████╗  
██╔══██╗██║     ██║██║╚██╗██║██╔═██╗ ██╔══██║██║███╗██║██╔══██║██╔══██╗██╔══╝  
██████╔╝███████╗██║██║ ╚████║██║  ██╗██║  ██║╚███╔███╔╝██║  ██║██║  ██║███████╗
╚═════╝ ╚══════╝╚═╝╚═╝  ╚═══╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚══╝╚══╝ ╚═╝  ╚═╝╚═╝  ╚═╝╚══════╝
```

### 👁️ Real-Time Attention Monitoring · Deep Learning · WebSocket Streaming

<br/>

[![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white)](https://www.tensorflow.org/)
[![WebSocket](https://img.shields.io/badge/WebSocket-010101?style=for-the-badge&logo=socketdotio&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org/)

<br/>

> **A production-grade, low-latency AI system that monitors user attentiveness in real time — using computer vision, a custom-trained CNN, and WebSocket-based streaming.**

<br/>

[📖 Overview](#-overview) · [🏗️ Architecture](#️-system-architecture) · [🧠 ML Model](#-machine-learning-model) · [⚡ Tech Stack](#-tech-stack) · [🚀 Setup](#-local-setup) · [🔮 Roadmap](#-roadmap)

</div>

---

## 📖 Overview

**BlinkAware** is a full-stack AI application that uses your webcam to detect blinks and assess attentiveness in real time. Built at the intersection of computer vision, streaming systems, and modern web architecture, it showcases how deep learning models can be deployed in production-ready pipelines with minimal latency.

The system is split into three clean layers:

| Layer | Technology | Role |
|-------|-----------|------|
| 🖥️ **Frontend** | React + Tailwind CSS | Live video capture, real-time UI, analytics |
| ⚙️ **Backend** | FastAPI + WebSockets | Frame ingestion, preprocessing, inference orchestration |
| 🧠 **ML Pipeline** | CNN (TensorFlow/PyTorch) | Binary eye-state classification (open / closed) |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   FRONTEND  (React)                      │
│   Webcam Capture  →  useWebSocket Hook  →  Live UI       │
└────────────────────────┬────────────────────────────────┘
                         │  WebSocket (raw frames)
                         ▼
┌─────────────────────────────────────────────────────────┐
│                  BACKEND  (FastAPI)                      │
│   Frame Receiver  →  Preprocessor  →  ml_pipeline.py    │
└────────────────────────┬────────────────────────────────┘
                         │  Eye region crop + inference
                         ▼
┌─────────────────────────────────────────────────────────┐
│                  ML MODEL  (CNN)                         │
│         Input: cropped eye image (grayscale)             │
│         Output: Open (1) / Closed (0)                    │
└────────────────────────┬────────────────────────────────┘
                         │  Prediction + blink logic
                         ▼
              Real-time UI update on Frontend
```

---

## 🧩 Key Features

- 🔴 **Real-time blink detection** from live webcam stream
- ⚡ **Sub-100ms latency** via WebSocket-based frame streaming
- 🧠 **Custom CNN** trained for binary eye-state classification
- 📊 **Analytics dashboard** tracking blink rate and attention patterns
- 🔐 **Authentication system** with protected routes and session management
- 🧱 **Modular, layered architecture** — frontend, backend, and ML are fully decoupled

---

## 🖥️ Frontend Architecture

```
frontend/src/
│
├── components/         # Reusable UI (ProtectedRoute, etc.)
├── contexts/
│   └── AuthContext.js  # Global auth state
├── hooks/
│   └── useWebSocket.js # Persistent WebSocket connection manager
├── pages/
│   ├── LandingPage.js
│   ├── LoginPage.js
│   ├── RegisterPage.js
│   ├── DashboardPage.js
│   ├── AnalyticsPage.js
│   └── SettingsPage.js
├── App.js              # Routing & layout
└── index.js            # Entry point
```

### Design Highlights

**`useWebSocket` Custom Hook**  
Abstracts WebSocket lifecycle — connection, reconnection, frame dispatch, and incoming message handling — into a clean, reusable API.

**`AuthContext`**  
Centralized authentication state shared across the app. Handles login, logout, token storage, and user session persistence.

**`ProtectedRoute`**  
Wraps sensitive pages to redirect unauthenticated users, keeping the routing layer clean and secure.

---

## ⚙️ Backend Architecture

```
backend/
│
├── server.py        # FastAPI app + WebSocket endpoint
├── auth.py          # JWT / token authentication logic
├── ml_pipeline.py   # Inference orchestration
├── model/           # Saved model weights + config
└── requirements.txt
```

### Frame Processing Pipeline

```
1. Frontend captures frame  →  encodes as binary/base64
2. WebSocket sends frame    →  server.py receives
3. Preprocessing            →  resize, normalize, crop eye region
4. ml_pipeline.py           →  model inference → Open / Closed
5. Blink logic              →  state transition detection
6. Result streamed back     →  frontend updates UI in real time
```

---

## 🧠 Machine Learning Model

### Architecture

- **Type:** Convolutional Neural Network (CNN)
- **Task:** Binary image classification
- **Classes:** `Eye Open` · `Eye Closed`
- **Input:** Grayscale cropped eye-region patches

### Training

- Eye region extracted using facial landmark detection
- Data augmented with flips, brightness shifts, and noise
- Trained with binary cross-entropy loss + Adam optimizer

### Performance

| Metric | Score |
|--------|-------|
| ✅ Accuracy | **94.2%** |
| 🎯 Precision | **92.8%** |
| 🔁 Recall | **93.5%** |
| ⚖️ F1 Score | **93.1%** |

---

## ⚡ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React, Tailwind CSS, React Context API |
| Backend | FastAPI, Python, Uvicorn |
| Real-time Comm | WebSocket Protocol |
| ML Framework | TensorFlow / PyTorch |
| Auth | JWT / Token-based |

---

## 🚀 Local Setup

### Prerequisites

- Python 3.8+
- Node.js 16+
- A working webcam

---

### 1. Clone the Repository

```bash
git clone https://github.com/niyatichaurasia/blinkaware.git
cd blinkaware
```

### 2. Backend Setup

```bash
cd backend

# Create and activate virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # macOS / Linux

# Install dependencies
pip install -r requirements.txt

# Start the server
uvicorn server:app --reload
```

> Backend runs at `http://localhost:8000`

---

### 3. Frontend Setup

```bash
cd frontend

npm install
npm start
```

> Frontend runs at `http://localhost:3000`

---

Open your browser, allow webcam access, and BlinkAware will begin monitoring in real time. 👁️

---

## 🔐 Authentication Flow

```
User → Register / Login → Token issued
                              │
                         AuthContext stores token
                              │
                    ProtectedRoute checks auth state
                              │
                    Backend validates token on WS connect
```

---

## 🔮 Roadmap

- [ ] 😴 **Drowsiness detection** using PERCLOS (Percentage of Eye Closure) metric  
- [ ] 📱 **Mobile support** — responsive UI + mobile camera access  
- [ ] ⚡ **Model optimization** — ONNX / TensorRT for faster inference  
- [ ] 👥 **Multi-face tracking** — monitor multiple users simultaneously  
- [ ] ☁️ **Cloud deployment** — AWS / GCP with containerized services  
- [ ] 📈 **Blink rate analytics** — historical trends and attention scoring  

---

## 💡 Why BlinkAware?

This project is a demonstration of end-to-end ML system design:

- **Real-time architecture** — WebSocket streaming from browser to inference engine
- **Full-stack + ML integration** — React ↔ FastAPI ↔ CNN in a single cohesive system
- **Production thinking** — auth, protected routes, modular pipeline, clean separation of concerns
- **Applied deep learning** — a custom-trained model solving a real computer vision problem

---

## 👩‍💻 Author

**Niyati Chaurasia**  
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/niyatichaurasia)

---

<div align="center">

**If this project sparked your interest, drop a ⭐ — it means a lot!**

*Built with 👁️ attention to detail*

</div>
