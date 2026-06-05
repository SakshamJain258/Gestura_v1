# Gestura v1

Original Gestura implementation using MediaPipe, TensorFlow/Keras, PyQt6, and a virtual camera pipeline.

## Setup

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Run

```powershell
python main_GUI.py
```

## Notes

- Keep `.venv/` local. It is ignored by Git.
- Model files are kept in `models/`.
- Gesture landmark data is kept in `data2/`.
