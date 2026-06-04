# ⚡ LoadOptimizer

> An AI-driven transformer load optimization system with real-time demand simulation, predictive modeling, and intelligent load-shedding algorithms.

![Python](https://img.shields.io/badge/Python-3.13+-3776AB?style=flat&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.95+-009688?style=flat&logo=fastapi&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3+-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Running the Application](#running-the-application)
- [API Reference](#-api-reference)
  - [Simulation Endpoint](#simulation-endpoint)
  - [Query Parameters](#query-parameters)
  - [Example Requests](#example-requests)
- [Frontend](#-frontend)
- [Tech Stack](#-tech-stack)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🔍 Overview

**LoadOptimizer** is an AI-powered system designed to simulate, predict, and optimize transformer load distribution. Given a configurable demand scenario, the system forecasts expected load using a trained regression model and applies a load-shedding algorithm to prevent transformer overload — all served through a clean REST API with an interactive browser-based frontend.

Originally built for a hackathon and later refactored into a production-style application with a **FastAPI** backend and a **Bootstrap + Chart.js** frontend.

---

## ✨ Features

- 🔁 **Demand Simulation** — Generates realistic load profiles across multiple scenarios
- 🤖 **Load Prediction** — Linear Regression model forecasts transformer demand
- ⚖️ **Dual Optimization Algorithms** — Choose between `greedy` or `proportional` load-shedding strategies
- 🌐 **REST API** — FastAPI backend with full Swagger/OpenAPI documentation
- 📊 **Interactive Frontend** — Real-time charts and scenario controls via Chart.js
- 🧪 **Scenario Support** — Simulate `normal`, `heatwave`, `high_ev`, and `emergency` conditions

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend (HTML/JS)                    │
│             Bootstrap UI  +  Chart.js Visualizations         │
└────────────────────────────┬────────────────────────────────┘
                             │ HTTP (REST)
┌────────────────────────────▼────────────────────────────────┐
│                     FastAPI Backend                          │
│  ┌──────────────┐  ┌───────────────┐  ┌──────────────────┐  │
│  │  main.py     │  │ controller.py │  │  simulator.py    │  │
│  │  (Endpoints) │→ │ (Orchestrator)│→ │  (Demand Gen)    │  │
│  └──────────────┘  └───────┬───────┘  └──────────────────┘  │
│                            │                                 │
│                   ┌────────┴────────┐                        │
│                   │                 │                        │
│           ┌───────▼──────┐  ┌───────▼──────┐                │
│           │   model.py   │  │ optimizer.py │                │
│           │  (Predictor) │  │  (Shedding)  │                │
│           └──────────────┘  └──────────────┘                │
└─────────────────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
load_optimizer/
│
├── backend/
│   ├── main.py          # FastAPI app — route definitions & request handling
│   ├── controller.py    # SystemController — orchestrates simulation pipeline
│   ├── simulator.py     # Demand simulator — generates load profiles per scenario
│   ├── model.py         # Prediction model — Linear Regression on simulated data
│   └── optimizer.py     # Load-shedding optimizer — greedy & proportional strategies
│
├── frontend/
│   ├── index.html       # Main UI page
│   ├── script.js        # API calls & Chart.js rendering
│   └── style.css        # Custom styles
│
├── app.py               # Legacy entry point (pre-refactor)
├── model.py             # Legacy model (pre-refactor)
├── simulator.py         # Legacy simulator (pre-refactor)
├── requirements.txt     # Python dependencies
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python **3.13+**
- `pip` package manager
- A modern web browser

### Installation

**1. Clone the repository:**

```bash
git clone https://github.com/Meenu55/load_optimizer.git
cd load_optimizer
```

**2. Create and activate a virtual environment:**

```bash
python -m venv venv

# On macOS/Linux:
source venv/bin/activate

# On Windows:
venv\Scripts\activate
```

**3. Install dependencies:**

```bash
pip install -r requirements.txt
```

### Running the Application

**1. Start the backend server:**

```bash
uvicorn backend.main:app --reload
```

The API will be live at: `http://127.0.0.1:8000`

**2. Open the frontend** (in a separate terminal):

```bash
cd frontend
python -m http.server 8080
```

Then open your browser and navigate to: `http://localhost:8080`

**3. View interactive API docs:**

Visit `http://127.0.0.1:8000/docs` for the Swagger UI with all available endpoints and parameters.

---

## 📡 API Reference

### Simulation Endpoint

```
GET /simulate
```

Runs a full simulation pipeline: generates demand → predicts load → applies optimization.

### Query Parameters

| Parameter   | Type     | Default        | Options                                          | Description                          |
|-------------|----------|----------------|--------------------------------------------------|--------------------------------------|
| `scenario`  | `string` | `normal`       | `normal`, `heatwave`, `high_ev`, `emergency`     | Demand scenario to simulate          |
| `algorithm` | `string` | `proportional` | `proportional`, `greedy`                         | Load-shedding algorithm to apply     |

### Example Requests

**Normal scenario with proportional shedding (default):**
```bash
curl "http://127.0.0.1:8000/simulate"
```

**Heatwave scenario with proportional algorithm:**
```bash
curl "http://127.0.0.1:8000/simulate?scenario=heatwave&algorithm=proportional"
```

**Emergency scenario with greedy algorithm:**
```bash
curl "http://127.0.0.1:8000/simulate?scenario=emergency&algorithm=greedy"
```

**High EV adoption scenario:**
```bash
curl "http://127.0.0.1:8000/simulate?scenario=high_ev&algorithm=greedy"
```

---

## 🖥️ Frontend

The frontend is a lightweight static page served via Python's built-in HTTP server. It communicates with the FastAPI backend to:

- Select a **scenario** and **algorithm** from dropdown controls
- Trigger a simulation run on button click
- Render **real-time load charts** using Chart.js
- Display optimization results and load-shedding metrics

> No build step required — pure HTML, CSS, and vanilla JavaScript.

---

## 🛠️ Tech Stack

| Layer       | Technology                        |
|-------------|-----------------------------------|
| Backend     | Python 3.13, FastAPI, Uvicorn     |
| ML / Data   | scikit-learn, pandas, numpy       |
| Visualization (legacy) | Streamlit, Plotly       |
| Frontend    | HTML5, Bootstrap, Chart.js        |
| API Docs    | Swagger UI (built into FastAPI)   |

---

## 🗺️ Roadmap

- [ ] Comprehensive unit tests for `SystemController` and edge cases
- [ ] Integrate a more realistic forecasting model (e.g., LSTM or XGBoost)
- [ ] Support larger and real-world datasets
- [ ] Improve frontend UX — loading states, error handling, responsive design
- [ ] Add Docker support for containerized deployment
- [ ] CI/CD pipeline with GitHub Actions

---

## 🤝 Contributing

Contributions are welcome! To get started:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "feat: add your feature"`
4. Push to your branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

Please follow [conventional commits](https://www.conventionalcommits.org/) for commit messages.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

<p align="center">Built with ⚡ for smarter energy systems</p>
- 🌐 **REST API** — FastAPI backend with full Swagger/OpenAPI documentation
- 📊 **Interactive Frontend** — Real-time charts and scenario controls via Chart.js
- 🧪 **Scenario Support** — Simulate `normal`, `heatwave`, `high_ev`, and `emergency` conditions

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend (HTML/JS)                    │
│             Bootstrap UI  +  Chart.js Visualizations         │
└────────────────────────────┬────────────────────────────────┘
                             │ HTTP (REST)
┌────────────────────────────▼────────────────────────────────┐
│                     FastAPI Backend                          │
│  ┌──────────────┐  ┌───────────────┐  ┌──────────────────┐  │
│  │  main.py     │  │ controller.py │  │  simulator.py    │  │
│  │  (Endpoints) │→ │ (Orchestrator)│→ │  (Demand Gen)    │  │
│  └──────────────┘  └───────┬───────┘  └──────────────────┘  │
│                            │                                 │
│                   ┌────────┴────────┐                        │
│                   │                 │                        │
│           ┌───────▼──────┐  ┌───────▼──────┐                │
│           │   model.py   │  │ optimizer.py │                │
│           │  (Predictor) │  │  (Shedding)  │                │
│           └──────────────┘  └──────────────┘                │
└─────────────────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
load_optimizer/
│
├── backend/
│   ├── main.py          # FastAPI app — route definitions & request handling
│   ├── controller.py    # SystemController — orchestrates simulation pipeline
│   ├── simulator.py     # Demand simulator — generates load profiles per scenario
│   ├── model.py         # Prediction model — Linear Regression on simulated data
│   └── optimizer.py     # Load-shedding optimizer — greedy & proportional strategies
│
├── frontend/
│   ├── index.html       # Main UI page
│   ├── script.js        # API calls & Chart.js rendering
│   └── style.css        # Custom styles
│
├── app.py               # Legacy entry point (pre-refactor)
├── model.py             # Legacy model (pre-refactor)
├── simulator.py         # Legacy simulator (pre-refactor)
├── requirements.txt     # Python dependencies
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python **3.13+**
- `pip` package manager
- A modern web browser

### Installation

**1. Clone the repository:**

```bash
git clone https://github.com/Meenu55/load_optimizer.git
cd load_optimizer
```

**2. Create and activate a virtual environment:**

```bash
python -m venv venv

# On macOS/Linux:
source venv/bin/activate

# On Windows:
venv\Scripts\activate
```

**3. Install dependencies:**

```bash
pip install -r requirements.txt
```

### Running the Application

**1. Start the backend server:**

```bash
uvicorn backend.main:app --reload
```

The API will be live at: `http://127.0.0.1:8000`

**2. Open the frontend** (in a separate terminal):

```bash
cd frontend
python -m http.server 8080
```

Then open your browser and navigate to: `http://localhost:8080`

**3. View interactive API docs:**

Visit `http://127.0.0.1:8000/docs` for the Swagger UI with all available endpoints and parameters.

---

## 📡 API Reference

### Simulation Endpoint

```
GET /simulate
```

Runs a full simulation pipeline: generates demand → predicts load → applies optimization.

### Query Parameters

| Parameter   | Type     | Default        | Options                                          | Description                          |
|-------------|----------|----------------|--------------------------------------------------|--------------------------------------|
| `scenario`  | `string` | `normal`       | `normal`, `heatwave`, `high_ev`, `emergency`     | Demand scenario to simulate          |
| `algorithm` | `string` | `proportional` | `proportional`, `greedy`                         | Load-shedding algorithm to apply     |

### Example Requests

**Normal scenario with proportional shedding (default):**
```bash
curl "http://127.0.0.1:8000/simulate"
```

**Heatwave scenario with proportional algorithm:**
```bash
curl "http://127.0.0.1:8000/simulate?scenario=heatwave&algorithm=proportional"
```

**Emergency scenario with greedy algorithm:**
```bash
curl "http://127.0.0.1:8000/simulate?scenario=emergency&algorithm=greedy"
```

**High EV adoption scenario:**
```bash
curl "http://127.0.0.1:8000/simulate?scenario=high_ev&algorithm=greedy"
```

---

## 🖥️ Frontend

The frontend is a lightweight static page served via Python's built-in HTTP server. It communicates with the FastAPI backend to:

- Select a **scenario** and **algorithm** from dropdown controls
- Trigger a simulation run on button click
- Render **real-time load charts** using Chart.js
- Display optimization results and load-shedding metrics

> No build step required — pure HTML, CSS, and vanilla JavaScript.

---

## 🛠️ Tech Stack

| Layer       | Technology                        |
|-------------|-----------------------------------|
| Backend     | Python 3.13, FastAPI, Uvicorn     |
| ML / Data   | scikit-learn, pandas, numpy       |
| Visualization (legacy) | Streamlit, Plotly       |
| Frontend    | HTML5, Bootstrap, Chart.js        |
| API Docs    | Swagger UI (built into FastAPI)   |

---

## 🗺️ Roadmap

- [ ] Comprehensive unit tests for `SystemController` and edge cases
- [ ] Integrate a more realistic forecasting model (e.g., LSTM or XGBoost)
- [ ] Support larger and real-world datasets
- [ ] Improve frontend UX — loading states, error handling, responsive design
- [ ] Add Docker support for containerized deployment
- [ ] CI/CD pipeline with GitHub Actions

 
 
 
