# 🏋️‍♂️ RepLica — Real-Time AI Workout Coach

[![Streamlit App](https://img.shields.io/badge/Live_App-Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://ai-realtime-gym-coach.streamlit.app/)
[![Landing Page](https://img.shields.io/badge/Landing_Page-Netlify-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)](https://replica-ai-gym-coach.netlify.app)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![LLM Powered](https://img.shields.io/badge/LLM-Groq_%2F_Llama_3.3-f34e3a?style=for-the-badge)](https://groq.com/)

> **RepLica** turns your webcam into a personal fitness trainer. It watches your exercise form in real time and provides instant, spoken feedback — powered by pose detection, an LLM coaching engine, and text-to-speech.

---

## 🎯 What RepLica Does

Start a workout session, pick an exercise, and let RepLica handle the rest:

* **🧘 Real-Time Body Tracking:** Tracks 33 body landmarks using MediaPipe pose detection through your webcam.
* **📐 Precision Form Analysis:** Evaluates joint angles, depth, alignment, and balance per rep tailored to specific exercises (*Squats, Push-ups, Bicep Curls, Shoulder Press, Lunges*).
* **🗣️ Hands-Free Voice Coaching:** Groq (Llama 3.3 70B) generates contextual feedback converted to speech in real time, so you never need to interrupt your set to look at a screen.
* **📊 Workout History Logging:** Tracks reps, sets, and duration per session, saved locally in SQLite and presented in a clean summary dashboard.

---

## 🔄 How It Works

```text
Webcam Feed 📷 ──> MediaPipe Pose Detection 🧘 ──> Per-Exercise Metrics 📐
                                                             │
SQLite History 📊 <── Session Progress Synced 🏁 <── Live Audio Feedback 🔊 <── LLM Coaching (Groq) 🤖


```

## 🏗️ System Architecture

```text
RepLica/
├── main.py                    # Streamlit UI tying vision, state, and audio together
├── detectors/                 # Per-exercise form analysis (angles, thresholds, flags)
└── services/
    ├── vision/                # WebRTC video capture & MediaPipe pose processing
    ├── tracking/              # Syncs live metrics to app state & detects reps/sets
    ├── coaching/              # LLM prompt engineering, TTS, & debounced voice pipeline
    ├── persistence/           # SQLite-backed workout history database
    ├── auth/                  # Lightweight session/user handling
    └── state/                 # Streamlit session state management


```


## 🛠️ Tech Stack

| Domain | Technology / Library |
| :--- | :--- |
| **Frontend & UI** | [Streamlit](https://streamlit.io/) |
| **Real-Time Video** | `streamlit-webrtc`, OpenCV |
| **Pose Detection** | [MediaPipe](https://google.github.io/mediapipe/) |
| **LLM Coaching Engine** | [Groq API](https://groq.com/) *(Llama 3.3 70B)* |
| **Text-to-Speech (TTS)** | `gTTS` |
| **Database** | SQLite |
| **Package Manager** | `uv` |

---

## ⚡ Notable Engineering Highlights

* **Debounced Voice Feedback:** The AI coach uses a priority queue with rate-limiting so it never speaks over itself or spams minor form corrections during a rep.
* **Per-Exercise Metric Schemas:** Centralized exercise configurations isolate math rules and thresholds, avoiding messy conditional logic across the codebase.
* **Frame-Rate Optimized Pipeline:** Model complexity and frame resolution are tuned specifically to maintain high FPS and low latency (sub-100ms) on standard webcams.

# Create virtual environment with uv
uv venv --python 3.12

# Activate virtual environment
# On Windows PowerShell:
.venv\Scripts\activate
# On macOS / Linux:
source .venv/bin/activate

# Install dependencies
uv pip install -r requirements.txt
### 3. Configure API Keys
Create a `.env` file in the project root:
```env
GROQ_API_KEY=your_groq_api_key_here
```
### 4. Run the Application
```bash
uv run streamlit run main.py

```
## 📌 Status & Roadmap

> 🟢 **Actively Developed**

- [x] Real-time pose analysis & rep counting
- [x] LLM + TTS debounced voice feedback pipeline
- [x] Local SQLite workout history logging
- [ ] Automated testing suite
- [ ] Expanded exercise library
- [ ] Cloud deployment

---

## 👤 Author

**Sumit Sana**
* **GitHub:** [@sumitstat07](https://github.com/sumitstat07)
* **Email:** [sumitsana2002@gmail.com](mailto:sumitsana2002@gmail.com)
* **Landing Page:** [replica-ai-gym-coach.netlify.app](https://replica-ai-gym-coach.netlify.app)

