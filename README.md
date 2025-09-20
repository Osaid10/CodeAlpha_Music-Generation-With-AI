# 🎵 SATB Chorale Generator with LSTM

This project generates Bach-style SATB (Soprano, Alto, Tenor, Bass) chorales using a **deep learning LSTM model** trained on the **JSB Chorales dataset**. The generated chorales are saved as MIDI files and can be played or converted to WAV for audio playback.  

---

## Features

- Train an LSTM model on real Bach chorales.  
- Generates **SATB chorales** with separate S-A-T-B voices.  
- Converts MIDI to WAV for direct playback in Colab or locally.  
- Colab-friendly with fast audio rendering using **Timidity**.  
- Handles rests and ensures MIDI notes are within valid pitch ranges.  

---

## Dataset

- [JSB Chorales dataset](https://github.com/czhuang/JSB-Chorales-dataset)  
- Contains four-part chorales encoded as 16th-note sequences.  
- Preprocessed into training sequences for LSTM input.  

---

## Installation

1. Clone this repository:

```bash
git clone https://github.com/yourusername/satb-chorale-generator.git
cd satb-chorale-generator
pip install numpy tensorflow pretty_midi midi2audio
# For Colab playback
apt-get install -y timidity
# Load dataset
import numpy as np
data = np.load("chorales/JSB-Chorales-dataset-master/Jsb16thSeparated.npz", allow_pickle=True)
sequences = data["train"]

# Flatten sequences, encode notes, and prepare training sequences
# ... (see training code)
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import LSTM, Dense, Embedding

model = Sequential([
    Embedding(input_dim=vocab_size, output_dim=128),
    LSTM(256, return_sequences=True),
    LSTM(256),
    Dense(vocab_size, activation='softmax')
])
model.compile(loss='sparse_categorical_crossentropy', optimizer='adam')
from generate_chorale import generate_chorale
audio = generate_chorale(model, X, int_to_note, length=100)
timidity generated_chorale.mid -Ow -o generated_chorale.wav
from IPython.display import Audio
Audio("generated_chorale.wav", rate=44100)
