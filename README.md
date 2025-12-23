# Urban Mirage: Multimodal Perception System for Ghost City Detection

**"This place looks like a city, but it is not a city at all."**

## 1. Installation and Requirements

Before running the system, please ensure the firmware is correctly uploaded to the microcontrollers and the host software environment is set up.

### Hardware Configuration (ESP32-CAM & STM32)

1. **Arduino IDE Setup**
* Open Arduino IDE, go to **File -> Preferences**.
* Add the following URL to "Additional Boards Manager URLs":
`https://dl.espressif.com/dl/package_esp32_index.json`


2. **Library Installation**
* Go to **Tools -> Board -> Boards Manager**.
* Search for and install `esp32` (Version 2.0.0 or higher is recommended).


3. **Firmware Upload**
* Use the code provided in the `firmware/` directory to flash your **ESP32-CAM** (Visual) and **STM32/Microphone** (Audio) modules respectively.



### Software Configuration (Python Host)

This project requires **Python 3.8+**. Install the core dependencies by running the following command in your terminal:

```bash
# Deep Learning & Numerical Computation
pip install tensorflow numpy matplotlib scikit-learn

# Computer Vision & Image Processing
pip install opencv-python pillow

# Audio Signal Processing
pip install librosa soundfile

# Hardware Communication & Network
pip install pyserial requests

# Utilities
pip install tqdm

```

**Key Dependencies:**

* `tensorflow`: For loading and running `.keras` deep learning models.
* `librosa` & `soundfile`: For audio resampling and Mel-Spectrogram feature extraction.
* `opencv-python` (cv2) & `pillow`: For image stream capture and preprocessing.
* `pyserial`: For serial communication between the host PC and microcontrollers.
* `requests`: For retrieving HTTP video streams from the ESP32-CAM.

---

## 2. Project Purpose

Urban Mirage is a multimodal perception system designed to detect "Pseudo-urbanization" phenomena, such as Ghost Cities and Sleeper Towns, which often deceive visual-only sensors with their high-density architectural forms. By integrating an STM32 auditory sensor and an ESP32 visual sensor, the system captures environmental soundscapes and streetscapes simultaneously. It utilizes a Dual-Path Convolutional Neural Network (CNN) for real-time inference and applies a Weighted Decision Fusion Algorithm () to correct visual misclassifications using acoustic features like drone noise or silence. This approach allows for the precise categorization of urban areas into Urban Void (Edge), Residential Area (Sleeper), and Urban Core (Vitality), providing a robust computational tool for urban sociological analysis.

---

## 3. Project Structure

```text
project_root/
├── classification/
│   └── predictor              # Main System: Real-time acquisition, inference & visualization
├── data_processing/
│   ├── simulate_audio         # Audio Processing: Maps TAU dataset to the 6-class taxonomy
│   └── simulate_image         # Image Processing: Calculates vitality scores from Cityscapes
├── evaluation/
│   └── evaluate_fusion        # Evaluation: Generates confusion matrices & fusion reports
├── firmware/
│   ├── esp32cam               # Firmware for ESP32-CAM module
│   ├── microphone             # Firmware for STM32 audio acquisition
│   └── servo                  # Firmware for Servo control or Display drivers
├── models/
│   ├── 1                      # Saved model artifacts or checkpoints
│   ├── best_audio_model.keras      # Pre-trained optimal Audio CNN model
│   └── best_image_model_v2.keras   # Pre-trained optimal Image CNN model (V2)
└── training/
    ├── training_audio         # Training script for the Audio model
    └── training_image         # Training script for the Image model

```

*Note: The Python scripts in the structure above (e.g., predictor, simulate_audio) are expected to be executed as Python modules or scripts (e.g., `predictor.py`). Ensure they have the correct extension or run them accordingly.*

---

## 4. Usage Guide

### Step 1: Data Preparation

Due to the scarcity of field data, we use a Proxy Data Strategy. Ensure you have downloaded the **TAU Urban Acoustic Scenes** and **Cityscapes** datasets and configured the paths in the scripts.

```bash
# Generate audio training samples
python data_processing/simulate_audio.py

# Generate image training samples
python data_processing/simulate_image.py

```

### Step 2: Model Training

Train the auditory and visual neural networks independently. Training logs will be automatically saved as CSV files.

```bash
# Train the Audio Model
python training/training_audio.py

# Train the Image Model
python training/training_image.py

```

### Step 3: System Evaluation

Load the pre-trained models from the `models/` directory and verify the accuracy of the multimodal fusion algorithm on the test set.

```bash
python evaluation/evaluate_fusion.py

```

### Step 4: Real-time Deployment

Connect your hardware devices to the PC via USB/WiFi. Start the main program to perform real-time urban region detection.

```bash
python classification/predictor.py

```

---

## Hardware List

* **Microcontroller:** STM32F103C8T6 (Blue Pill)
* **Visual Module:** ESP32-CAM (OV2640)
* **Microphone:** INMP441 I2S MEMS Microphone
* **Display:** 1.44' TFT LCD (ST7735)
* **Interface:** Arduino Uno (Bridge for Display/Servo)

---

## License

This project is licensed under the MIT License - see the [LICENSE](https://www.google.com/search?q=LICENSE) file for details.
