# Contactless Heart Health Monitoring System

A web-based health monitoring system that uses remote photoplethysmography (rPPG) technology to extract pulse waveform data from facial videos captured via device camera, enabling contactless heart rate measurement and cardiovascular risk assessment.

## Project Overview

This application provides a non-invasive way to monitor heart health by analyzing subtle color changes in facial skin to extract blood volume pulse (BVP) signals. The system processes recorded facial videos to generate heart rate time-series data and predicts cardiovascular risk using machine learning models.

### Key Features

- **Contactless Heart Rate Measurement**: Extract pulse waveform data from facial video without wearables
- **Real-time Video Recording**: Record high-quality facial video through device camera
- **Heart Rate Visualization**: Interactive charts displaying heart rate trends over time
- **Risk Assessment**: ML-powered cardiovascular risk prediction with personalized insights
- **Secure Data Storage**: All data encrypted and stored securely with Row Level Security (RLS) policies
- **User Authentication**: Email/password authentication with session management
- **Responsive Design**: Works on desktop and mobile devices

---

## Technology Stack

### Frontend
| Technology | Purpose |
|-----------|---------|
| **React 18.3** | UI framework and component management |
| **TypeScript** | Type-safe JavaScript development |
| **Vite 7.1** | Fast build tool and dev server |
| **Tailwind CSS 3.4** | Utility-first CSS for responsive design |
| **Lucide React** | Icon library for UI components |
| **WebRTC API** | Browser-native video recording from camera |

### Backend & Database
| Technology | Purpose |
|-----------|---------|
| **Supabase** | PostgreSQL database + authentication + real-time updates |
| **Row Level Security (RLS)** | Database-level access control for data privacy |
| **Supabase Auth** | Email/password authentication system |
| **Supabase Storage** | Secure video file storage |

### Edge Computing
| Technology | Purpose |
|-----------|---------|
| **Supabase Edge Functions** | Serverless TypeScript functions for video processing orchestration |
| **Deno Runtime** | Secure JavaScript runtime for edge functions |

### ML/AI Backend (Python)
| Technology | Purpose |
|-----------|---------|
| **FastAPI** | High-performance Python web framework for ML API |
| **PyVHR 2.0** | Video-based heart rate extraction using rPPG algorithms |
| **LSTM Neural Network** | Deep learning for cardiovascular risk classification (planned) |
| **scikit-learn** | Machine learning utilities for feature extraction |
| **NumPy & SciPy** | Numerical computing and signal processing |
| **OpenCV** | Computer vision for video frame processing |
| **TensorFlow/PyTorch** | Deep learning frameworks for LSTM models (optional) |

### Deployment Ready
| Technology | Purpose |
|-----------|---------|
| **Docker** | Containerization for Python backend |
| **Google Cloud Run** | Serverless deployment option |
| **AWS Lambda** | Serverless deployment option |
| **Heroku** | Simple deployment option |

---

## How PyVHR Works (rPPG Technology)

**Remote Photoplethysmography (rPPG)** extracts heart rate from video by detecting:

1. **Blood Volume Pulse Detection**: Analyzes subtle color changes in facial skin
2. **Signal Extraction Methods**:
   - **POS** (Plane-Orthogonal-to-Skin): Motion-robust, recommended
   - **CHROM** (Chrominance): Good in controlled lighting
   - **GREEN**: Fast, simple green channel analysis
   - **ICA** (Independent Component Analysis): Multiple signal separation
   - **LGI** (Local Group Invariance): Robust to illumination changes

3. **Output**: Heart rate time-series with timestamps and confidence scores

## How LSTM Risk Prediction Works

**LSTM (Long Short-Term Memory)** analyzes heart rate patterns to predict cardiovascular risk:

1. **Input Features**:
   - Heart rate time-series (60 seconds, 4 samples/second = 240 data points)
   - Heart Rate Variability (HRV) metrics (RMSSD, SDNN, pNN50)
   - Statistical features (mean, std, skewness, kurtosis)
   - Frequency domain features (LF/HF ratio from FFT)

2. **Output**: Risk classification (Low/Medium/High) with confidence score

3. **Processing**: Sequential analysis of heart rate patterns to identify abnormal rhythms

---

## Folder Structure

```
.
├── index.html                          # HTML entry point
├── package.json                        # Node.js dependencies
├── tsconfig.json                       # TypeScript configuration
├── vite.config.ts                      # Vite build configuration
├── tailwind.config.js                  # Tailwind CSS configuration
├── postcss.config.js                   # PostCSS configuration
│
├── src/                               # Frontend source code
│   ├── main.tsx                       # React app entry point
│   ├── App.tsx                        # Root app component
│   ├── index.css                      # Global styles
│   │
│   ├── components/                    # Reusable React components
│   │   ├── AuthForm.tsx              # Login/registration form
│   │   ├── VideoRecorder.tsx         # Video recording UI
│   │   ├── HeartRateChart.tsx        # Interactive heart rate chart
│   │   └── RiskAssessment.tsx        # Risk prediction display
│   │
│   ├── pages/                        # Page components (routes)
│   │   ├── Dashboard.tsx             # Main dashboard with recordings list
│   │   ├── RecordingPage.tsx         # Video recording interface
│   │   └── AnalysisPage.tsx          # Results and analysis display
│   │
│   ├── contexts/                     # React Context for state management
│   │   └── AuthContext.tsx           # Authentication state provider
│   │
│   ├── services/                     # API and business logic
│   │   └── api.ts                    # Supabase and edge function calls
│   │
│   └── lib/                          # Utilities and configs
│       └── supabase.ts               # Supabase client initialization
│
├── supabase/                          # Supabase backend configuration
│   ├── migrations/                   # Database schema migrations
│   │   └── 20251008214204_create_health_monitoring_schema.sql
│   │
│   └── functions/                    # Edge Functions (serverless)
│       └── process-heart-rate/
│           └── index.ts              # Main edge function for video processing
│
├── python-backend/                    # ML/AI backend (separate deployment)
│   ├── main.py                       # FastAPI server
│   ├── requirements.txt               # Python dependencies
│   ├── README.md                     # Backend documentation
│   └── Dockerfile                    # Container configuration (future)
│
├── .env                              # Environment variables (not in repo)
├── .gitignore                        # Git ignore rules
├── README.md                         # This file
└── ML_INTEGRATION_GUIDE.md           # Detailed ML setup guide
```

---

## Database Schema

### Core Tables

**users**
- User accounts and authentication metadata

**video_recordings**
- Recording metadata, video URLs, processing status
- Linked to users table via user_id

**heart_rate_data**
- Time-series heart rate measurements (240+ points per recording)
- Linked to video_recordings via recording_id

**risk_predictions**
- ML model predictions with risk level, score, and insights
- Linked to video_recordings via recording_id

All tables have Row Level Security (RLS) enabled to ensure users can only access their own data.

---

## Installation & Setup

### Prerequisites

- Node.js 18+ and npm/yarn
- Python 3.10+ (for ML backend, optional for initial testing)
- A Supabase account (free tier available)

### Step 1: Set Up Supabase Project

1. Go to [supabase.com](https://supabase.com) and create a new project
2. In the Supabase dashboard, note these values:
   - **Project URL**: `https://[project-id].supabase.co`
   - **Anon Key**: Found in Settings → API → `anon` key
   - **Service Role Key**: Found in Settings → API → `service_role` key

### Step 2: Clone & Install Dependencies

```bash
git clone <repository-url>
cd project
npm install
```

### Step 3: Configure Environment Variables

Create `.env` file in the project root:

```env
# Supabase Configuration
VITE_SUPABASE_URL=https://[your-project-id].supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

# Python ML Backend URL (optional, for real ML processing)
VITE_PYTHON_BACKEND_URL=http://localhost:8000
# Or for production:
# VITE_PYTHON_BACKEND_URL=https://your-ml-backend.com
```

**Getting Your Keys:**

1. Open your Supabase project dashboard
2. Go to **Settings** → **API**
3. Copy:
   - **Project URL** (appears at top of page)
   - **anon (public)** key - for frontend, can be exposed
   - **service_role** key - SECRET, only for backend/edge functions

### Step 4: Set Up Database Schema

The database is automatically set up through Supabase migrations:

```bash
# Migrations are already in supabase/migrations/
# They will be applied automatically when you connect your Supabase project
```

### Step 5: Run the Development Server

```bash
npm run dev
```

Access the app at `http://localhost:5173`

### Step 6: (Optional) Set Up Python ML Backend

For real heart rate extraction with PyVHR and risk prediction:

```bash
cd python-backend
pip install -r requirements.txt
pip install pyVHR opencv-python scipy scikit-learn
python main.py
```

The Python backend will run on `http://localhost:8000`

Set `VITE_PYTHON_BACKEND_URL=http://localhost:8000` in your `.env` to use it.

---

## API Keys & Configuration

### Required Keys

| Key | Location | Purpose | Visibility |
|-----|----------|---------|-----------|
| `VITE_SUPABASE_URL` | Supabase Dashboard → Settings → API | Database & auth endpoint | Public |
| `VITE_SUPABASE_ANON_KEY` | Supabase Dashboard → Settings → API → `anon` key | Frontend authentication | Public |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase Dashboard → Settings → API → `service_role` key | Backend/Edge Functions (SECRET) | Private only |
| `PYTHON_BACKEND_URL` | Your deployed ML backend | ML processing endpoint | Private |

### Optional Keys

| Key | Purpose | Default |
|-----|---------|---------|
| `VITE_PYTHON_BACKEND_URL` | Custom ML backend URL | `http://localhost:8000` |

### Where to Get Keys

**Supabase**:
1. Create account at [supabase.com](https://supabase.com)
2. Create new project
3. Go to Settings → API
4. Copy Project URL and keys

**Python Backend** (if using ML):
1. Deploy to Google Cloud Run, AWS Lambda, or local server
2. Get the URL and add to `.env`

---

## How to Run

### Development Mode

```bash
# Start frontend development server
npm run dev

# In another terminal, start ML backend (optional)
cd python-backend
python main.py
```

Access at `http://localhost:5173`

### Build for Production

```bash
# Build optimized bundle
npm run build

# Preview production build
npm run preview
```

### Run Type Checking

```bash
npm run typecheck
```

### Run Linting

```bash
npm run lint
```

---

## Usage Workflow

### 1. Sign Up / Login

- Create account with email and password
- Authentication handled by Supabase Auth

### 2. Record Video

- Click "Record Video"
- Allow camera access
- Record 60-second facial video following on-screen instructions
- Face should be centered and well-lit

### 3. Process Video

- Click "Analyze Video"
- System processes video through:
  - **Supabase Edge Function** → forwards to Python backend
  - **PyVHR** → extracts heart rate from facial color changes
  - **LSTM Model** → predicts cardiovascular risk
- Results saved to database

### 4. View Results

- Interactive heart rate chart showing pulse waveform
- Risk assessment card with level (Low/Medium/High) and confidence score
- Health insights and personalized recommendations
- Historical data available on dashboard

---

## Current Implementation Status

### ✅ Completed & Working

- Full frontend UI with responsive design
- Video recording via WebRTC
- Database schema with RLS policies
- Supabase authentication
- Edge Function infrastructure
- Real-time results display
- Historical data tracking

### ⚠️ Partially Implemented (Using Simulations)

- **Heart Rate Data**: Currently simulated with realistic patterns
- **Risk Prediction**: Currently rule-based (will use LSTM when ML model is trained)

### ❌ Requires Implementation

1. **PyVHR Integration** - Replace simulation with actual rPPG extraction
2. **LSTM Model Training** - Collect data and train risk prediction model
3. **Python Backend Deployment** - Deploy to cloud or local server
4. **Video Storage** - Connect Supabase Storage for video persistence

---

## Next Steps

### Phase 1: Enable Real Heart Rate Extraction

```bash
cd python-backend
pip install pyVHR opencv-python scipy
# Implement extract_heart_rate_from_video() in main.py
# Deploy backend
```

### Phase 2: Train Risk Prediction Model

- Collect training data (heart rate recordings with clinical labels)
- Extract HRV features from heart rate time-series
- Train LSTM or Random Forest model
- Deploy model to Python backend

### Phase 3: Optimize & Scale

- Add GPU acceleration for PyVHR
- Implement batch processing
- Add monitoring and logging
- Deploy to production infrastructure

---

## Architecture Diagram

```
┌─────────────────┐
│   Web Browser   │
│   (React App)   │
└────────┬────────┘
         │ HTTPS
         ▼
┌─────────────────────────────────┐
│    Supabase (PostgreSQL DB)     │
│  - Authentication               │
│  - Data Storage (RLS secured)   │
│  - Edge Functions Runtime       │
└────────┬────────────────────────┘
         │
         ├──────────────────┐
         │                  │
         ▼                  ▼
    ┌──────────┐     ┌──────────────────┐
    │  Storage │     │  Edge Function   │
    │(Videos)  │     │(process-heart-   │
    └──────────┘     │  rate)           │
                     └────────┬─────────┘
                              │
                              ▼
                     ┌──────────────────┐
                     │  Python Backend  │
                     │  (FastAPI)       │
                     │  - PyVHR (rPPG)  │
                     │  - LSTM Model    │
                     │  - ML Processing │
                     └──────────────────┘
```

---

## Troubleshooting

### "Camera Access Denied"
- Check browser permissions for camera
- On mobile, ensure app has camera permission in OS settings

### "Failed to connect to backend"
- Verify Python backend is running on configured URL
- Check `VITE_PYTHON_BACKEND_URL` in `.env`
- Edge Function will fall back to simulation if backend unavailable

### "Supabase connection error"
- Verify `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in `.env`
- Check internet connection
- Ensure Supabase project is active

### "Video processing timeout"
- Videos are processing asynchronously
- Wait longer for results (typically 30-120 seconds depending on backend)
- Check browser console for detailed error messages

---

## Security & Privacy

- **Database Encryption**: All data encrypted at rest in Supabase
- **Row Level Security**: Users can only access their own data
- **Authentication**: Email/password with session tokens
- **HTTPS Only**: All communication encrypted in transit
- **No Face Storage**: Raw facial videos are not permanently stored
- **Medical Disclaimer**: System is for informational purposes only, not medical diagnosis

---

## Resources

- **PyVHR Documentation**: https://github.com/phuselab/pyVHR
- **rPPG Research**: https://ieeexplore.ieee.org/document/7565547
- **Heart Rate Variability**: https://www.frontiersin.org/articles/10.3389/fpubh.2017.00258/full
- **FastAPI Docs**: https://fastapi.tiangolo.com
- **Supabase Docs**: https://supabase.com/docs
- **React Docs**: https://react.dev
- **Vite Docs**: https://vitejs.dev

---

## License

[Add your license here]

## Contributing

[Add contribution guidelines here]

## Support

For issues and questions, please open an issue in the repository.

---

**Last Updated**: January 2026

**Project Status**: Active Development (Simulation Mode)
