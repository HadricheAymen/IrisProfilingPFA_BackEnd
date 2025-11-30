# 🔮 Iris Profiling Backend API

A powerful Flask-based REST API for iris image extraction, personality profiling, and prediction using deep learning models. This backend service processes facial images to extract iris patterns and predicts personality types using advanced computer vision and machine learning techniques.

## 📋 Table of Contents

- [Features](#-features)
- [Technologies](#-technologies)
- [Architecture](#-architecture)
- [API Endpoints](#-api-endpoints)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Deployment](#-deployment)
- [Project Structure](#-project-structure)

## ✨ Features

### Core Functionality
- **Iris Extraction**: Automatic extraction of left and right iris from facial images using dlib's 68-point facial landmark detection
- **Personality Prediction**: Multiple deep learning models for iris-based personality profiling
- **Image Enhancement**: Automatic quality improvement (sharpness, contrast, brightness) using Pillow
- **Firebase Integration**: Store predictions and images in Firestore with full CRUD operations

### Machine Learning Models
- **MobileNet Model** (`mobileNet.h5`): Lightweight and fast predictions
- **EfficientNet Model** (`Efficient_10unfrozelayers.keras`): High-accuracy predictions with fine-tuned layers
- **Face Landmark Predictor** (`shape_predictor_68_face_landmarks.dat`): Precise facial feature detection

### Prediction Categories
The models classify iris patterns into personality types: Flower, Jewel, Stream, Shaker, and other categories.

## 🛠 Technologies

### Backend Framework
| Technology | Version | Purpose |
|------------|---------|---------|
| **Flask** | 3.1.1 | Web framework |
| **Flask-CORS** | 6.0.0 | Cross-origin resource sharing |
| **Waitress** | 3.0.2 | Production WSGI server |

### Machine Learning & Computer Vision
| Technology | Purpose |
|------------|---------|
| **TensorFlow** (CPU) | Deep learning model inference |
| **OpenCV** | Image processing and manipulation |
| **dlib** | Facial landmark detection |
| **NumPy** | Numerical computations |
| **Pillow** | Image enhancement |
| **scikit-learn** | ML utilities |

### Cloud Services
| Technology | Purpose |
|------------|---------|
| **Firebase Admin SDK** | Firestore database & Storage |
| **Railway** | Cloud deployment platform |

### Development Tools
| Technology | Purpose |
|------------|---------|
| **python-dotenv** | Environment variable management |
| **psutil** | System resource monitoring |
| **requests** | HTTP client for model downloads |

## 🏗 Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   Client App    │────▶│   Flask API      │────▶│   Firebase      │
│   (Frontend)    │◀────│   (Backend)      │◀────│   Firestore     │
└─────────────────┘     └──────────────────┘     └─────────────────┘
                               │
                               ▼
                        ┌──────────────────┐
                        │   ML Models      │
                        │  - MobileNet     │
                        │  - EfficientNet  │
                        │  - dlib          │
                        └──────────────────┘
```

## 🔌 API Endpoints

### Health & Status
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | API information and available endpoints |
| GET | `/health` | Health check with system metrics |
| GET | `/api/check-env` | Check environment variables status |
| GET | `/api/debug-models` | Debug model loading status |
| GET | `/api/test-firebase` | Test Firebase connectivity |

### Iris Extraction
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/extract-iris` | Extract left and right iris from facial image |

### Predictions
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/predict` | Basic prediction from single image |
| POST | `/api/predict-mobilenet` | MobileNet prediction (requires 2 iris images) |
| POST | `/api/predict-efficient` | EfficientNet prediction (requires 2 iris images) |

### Data Management
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/list-predictions/<user_id>` | List all predictions for a user |
| GET | `/api/get-image/<user_id>/<image_id>` | Retrieve stored image |

### Model Management
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/force-download` | Force redownload all models |
| POST | `/api/force-download-mobilenet` | Force redownload MobileNet model |

## 🚀 Installation

### Prerequisites
- Python 3.11+
- pip package manager

### Local Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd IrisProfilingPFA_BackEnd
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   pip install dlib tensorflow-cpu opencv-python-headless
   ```

4. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your Firebase credentials
   ```

5. **Run the application**
   ```bash
   python run.py
   ```

The server will start at `http://localhost:5000`

## ⚙️ Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `PORT` | Server port (default: 5000) | No |
| `FLASK_ENV` | Environment mode (development/production) | No |
| `API_VERSION` | API version string | No |
| `ALLOWED_ORIGINS` | CORS allowed origins | No |
| `FIREBASE_PROJECT_ID` | Firebase project ID | Yes |
| `FIREBASE_PRIVATE_KEY` | Firebase private key | Yes |
| `FIREBASE_CLIENT_EMAIL` | Firebase service account email | Yes |
| `FIREBASE_CREDENTIALS` | Path to Firebase credentials JSON | Optional |

### Firebase Setup
1. Create a Firebase project at [Firebase Console](https://console.firebase.google.com)
2. Generate a service account key (Project Settings > Service Accounts)
3. Set environment variables or place credentials JSON in project root

## 🌐 Deployment

### Railway Deployment

This project is configured for Railway deployment with:
- `railway.json` - Railway configuration
- `nixpacks.toml` - Build configuration with system dependencies
- `Procfile` - Process definition
- `build.sh` - Custom build script

**Quick Deploy:**
1. Push code to GitHub
2. Connect repository to Railway
3. Set environment variables in Railway dashboard
4. Deploy!

See [DEPLOYMENT.md](DEPLOYMENT.md) for detailed instructions.

## 📁 Project Structure

```
IrisProfilingPFA_BackEnd/
├── api/
│   ├── __init__.py
│   ├── iris_extraction.py    # Iris extraction endpoints
│   └── prediction.py         # Prediction endpoints & Firebase
├── models/
│   ├── __init__.py
│   ├── download_models.py    # Runtime model downloader
│   ├── model_loader.py       # Model loading utilities
│   ├── mobileNet.h5          # MobileNet model (downloaded)
│   ├── Efficient_*.keras     # EfficientNet model (downloaded)
│   └── shape_predictor_*.dat # dlib face landmarks (downloaded)
├── utils/
│   └── image_processing.py   # Image enhancement utilities
├── app.py                    # Flask application factory
├── run.py                    # Production entry point
├── requirements.txt          # Python dependencies
├── railway.json              # Railway configuration
├── nixpacks.toml             # Build configuration
└── README.md                 # This file
```

## 📝 License

This project is part of a PFA (Projet de Fin d'Année) academic project.

## 👥 Contributors

Developed for iris-based personality profiling research.

---

**API Version:** 1.0.0
**Python Version:** 3.11+

