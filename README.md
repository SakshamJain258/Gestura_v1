# Gestura v1

Gestura v1 is the original real-time ASL recognition prototype. It captures webcam frames, extracts MediaPipe pose and hand landmarks, builds a rolling 60-frame sequence, and classifies the gesture with a TensorFlow/Keras temporal model.

## What This Version Does

- Reads live webcam video with OpenCV.
- Mirrors the frame for natural interaction.
- Uses MediaPipe Holistic to extract pose, left-hand, and right-hand landmarks.
- Converts every frame into a 258-value feature vector:
  - Pose: 33 landmarks x 4 values = 132
  - Left hand: 21 landmarks x 3 values = 63
  - Right hand: 21 landmarks x 3 values = 63
- Classifies 60-frame gesture sequences with a Conv1D + BiLSTM + soft-attention Keras model.
- Displays recognized signs as subtitles in a PyQt6 desktop window.
- Includes optional virtual camera support through `pyvirtualcam`.

## Supported Signs

The current v1 code is a word-level recognizer with 17 classes:

```text
goodbye, hello, yes, no, thank you, please, sorry, stop, help,
what, how, where, when, eat, drink, sleep, IDLE_STATE
```

## Repository Structure

```text
.
|-- ASL.py              # Landmark extraction, class list, dataset helpers, model architecture
|-- main_GUI.py         # PyQt6 live recognition app
|-- model_training.py   # Training script for the Keras model
|-- requirements.txt    # Python dependencies
|-- data2/              # Self-collected landmark dataset
|-- models/             # Saved trained model files
|-- .gitignore          # Keeps local env/cache files out of Git
```

## Required Files

For live recognition, keep these files/folders:

- `ASL.py`
- `main_GUI.py`
- `requirements.txt`
- `models/model3.keras`

For retraining, also keep:

- `model_training.py`
- `data2/`
- the other saved models in `models/`

The current live app loads:

```python
models/model3.keras
```

## Setup

Create a local environment inside this repo:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

## Run Live Recognition

```powershell
.\.venv\Scripts\Activate.ps1
python main_GUI.py
```

The app currently uses:

```python
self.camera_index = 2
```

If your webcam does not open, change that value in `main_GUI.py` to `0` or `1`, depending on your camera.

## Train The Model

```powershell
.\.venv\Scripts\Activate.ps1
python model_training.py
```

Training reads landmark arrays from `data2/`, builds `(60, 258)` gesture sequences, trains the Keras model, and saves a new model file.

## Model Summary

The v1 model uses:

- `Conv1D` for short motion features
- `BatchNormalization` and `Dropout` for stability
- two bidirectional LSTM layers for temporal sequence learning
- soft attention to weight important frames
- a dense softmax classifier over the 17 sign classes


## Current Validation

The source files compile successfully with Python syntax checks:

```powershell
python -m py_compile ASL.py main_GUI.py model_training.py
```

Full live validation requires the Python dependencies, a working webcam, and the `models/model3.keras` file.
