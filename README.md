# RepLica

**An AI-powered real-time workout coach** that watches your exercise form through your webcam and gives live, spoken feedback — powered by pose detection, an LLM coaching layer, and text-to-speech.

## What it does

RepLica turns your webcam into a personal trainer. Start a workout session, pick an exercise, and RepLica:

- **Tracks your body in real time** using MediaPipe pose landmarks
- **Analyzes your form** — joint angles, depth, alignment, balance — per rep, tailored to the exercise (squats, push-ups, bicep curls, shoulder press, lunges)
- **Coaches you out loud** — an LLM (via Groq) generates contextual feedback based on your form, converted to speech and played back live, without you needing to look at a screen
- **Logs your workout history** — reps, sets, and time per session, persisted locally and viewable as a summary table

## How it works

Webcam feed → MediaPipe pose detection → per-exercise metrics extraction
→ LLM coaching (Groq / Llama 3.3) → text-to-speech → live audio feedback
→ session progress synced to UI → workout logged to SQLite

**Architecture:**
- `services/vision/` — WebRTC video capture + MediaPipe pose processing
- `detectors/` — per-exercise form analysis (angle calculations, thresholds, status flags)
- `services/tracking/` — syncs live metrics into app state, detects completed reps/sets
- `services/coaching/` — LLM prompt logic, TTS generation, and a voice pipeline that debounces feedback so the coach doesn't talk over itself
- `services/persistence/` — SQLite-backed workout history
- `services/auth/`, `services/state/` — lightweight session/user handling
- `main.py` — Streamlit UI tying it all together

## Tech stack

- **Frontend/UI:** Streamlit
- **Real-time video:** `streamlit-webrtc`
- **Pose detection:** MediaPipe
- **LLM coaching:** Groq API (Llama 3.3 70B)
- **Text-to-speech:** gTTS
- **Persistence:** SQLite
- **Language:** Python

## Notable engineering decisions

- **Debounced voice feedback** — the coach doesn't repeat itself on every frame; feedback is throttled and prioritized (major events like completing a set always speak, minor form corrections are rate-limited)
- **Per-exercise metric schemas** — each exercise defines its own relevant angles/statuses, kept in a single config rather than scattered conditionals
- **Frame-rate-conscious pose pipeline** — resolution and model complexity tuned to keep the live feed responsive without sacrificing detection accuracy

## Running it locally

```bash
uv venv --python 3.12
.venv\Scripts\activate      # or source .venv/bin/activate on macOS/Linux
uv pip install -r requirements.txt
```

Create a `.env` file in the project root:

GROQ_API_KEY=your_key_here


Then run:

```bash
uv run streamlit run main.py
```

## Status

Actively developed. Known next steps: automated tests, deployment, and expanding the exercise library.