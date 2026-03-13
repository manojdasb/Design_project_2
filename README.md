# Design_project_2
# Predator Watch - a Hugging Face Space by manojdas23

[![Hugging Face Spaces](https://img.shields.io/badge/Hugging%20Face-Spaces-orange?logo=huggingface)](https://huggingface.co/spaces/manojdas23/predator_watch)
[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)](https://python.org)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-EE4C2C?logo=pytorch)](https://pytorch.org)
[![Gradio](https://img.shields.io/badge/Gradio-4.0+-green?logo=gradio)](https://gradio.app)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)
[![Model](https://img.shields.io/badge/Model-EfficientNet--B3-purple)](https://pytorch.org/vision/stable/models/efficientnet.html)
[![Accuracy](https://img.shields.io/badge/Val%20Accuracy-93%25-brightgreen)]()

> Real-time big cat detection and threat assessment powered by EfficientNet-B3 + GradCAM, with instant Ntfy push notifications and Twilio SMS alerts.

---

## Try it Live

**[Open in Hugging Face Spaces](https://huggingface.co/spaces/manojdas23/predator_watch)**

```
https://huggingface.co/spaces/manojdas23/predator_watch
```

---

## What It Does

Predator Watch is an AI-powered wildlife threat detection system that:

- Identifies big cat species (cheetah, leopard, lion, tiger) from photos or webcam
- Generates **GradCAM heatmaps** showing exactly where the model is looking
- Returns a **species threat card** with danger level, recommended action, and species profile
- Fires **real-time alerts** via Ntfy push notification and Twilio SMS when a high-threat animal is detected

---

## Architecture

![Architecture](architecture.png)

| Stage | Description |
|-------|-------------|
| Input | Image upload / Webcam / Clipboard |
| Model | EfficientNet-B3 fine-tuned on Kaggle Big Cats dataset |
| Explainability | GradCAM — gradient-weighted class activation mapping |
| Assessment | Per-species danger profiles with threat index |
| Alerts | Ntfy.sh (push) + Twilio (SMS) fired in parallel threads |
| UI | Gradio — dark-themed, real-time, mobile-friendly |

---

## Threat Levels

| Level | Threat % | Species | Recommended Action |
|-------|----------|---------|-------------------|
| 🔴 CRITICAL | 85 – 100% | Tiger (95%), Lion (90%) | Evacuate immediately |
| 🟠 HIGH | 65 – 84% | Leopard (78%), Jaguar (72%) | Retreat slowly |
| 🟡 MODERATE | 40 – 64% | Cheetah (38%), Cougar (50%) | Maintain distance |
| 🟢 LOW | 0 – 39% | Snow Leopard (22%) | Observe safely |

---

## Project Structure

```
predator_watch/
│
├── src/
│   ├── big_cats_gradio_app.py     # Main Gradio app (GradCAM + alerts + UI)
│   ├── big_cats_classifier.py     # EfficientNet-B3 training script
│   └── app.py                     # Hugging Face Spaces entry point
│
├── docs/
│   ├── system_design.md           # Component breakdown and data flow
│   └── api_reference.md           # Function signatures and config reference
│
├── README.md                      # This file
├── requirements.txt               # Python dependencies
├── architecture.png               # System architecture diagram
├── demo_video_link.txt            # Demo video link
└── setup_instructions.md         # How to run locally or deploy
```

---

## Model Details

| Property | Value |
|----------|-------|
| Architecture | EfficientNet-B3 |
| Pretrained | ImageNet (backbone frozen) |
| Classifier | Dropout(0.4) + Linear(1536 → 4) |
| Dataset | [Big Cats Image Classification](https://www.kaggle.com/datasets/patriciabrezeanu/big-cats-image-classification-dataset) |
| Classes | cheetah, leopard, lion, tiger |
| Val Accuracy | ~93% |
| Input Size | 224 × 224 |
| Trained On | Kaggle Notebooks (CPU) |

---

## Alert System

Both channels fire simultaneously in parallel threads whenever the threat index exceeds the threshold (default: 65%).

**Ntfy.sh** — Free push notifications to any phone. Subscribe to topic `predatorwatch123` in the Ntfy app.

**Twilio SMS** — SMS alerts to verified recipient numbers via Twilio trial account.

A 60-second cooldown per species prevents alert spam.

---

## Run It Yourself

### Hugging Face Spaces (no setup needed)
```
https://huggingface.co/spaces/manojdas23/predator_watch
```

### Google Colab
```python
import os
os.environ["KAGGLE_USERNAME"] = "your_username"
os.environ["KAGGLE_KEY"]      = "your_api_key"
exec(open("src/big_cats_gradio_app.py").read())
```

### Kaggle Notebook
```python
exec(open("/kaggle/working/big_cats_gradio_app.py").read())
app = build_app()
app.launch(share=True)
```

### Local
```bash
git clone https://github.com/manojdas23/predator_watch.git
cd predator_watch
pip install -r requirements.txt
python src/app.py
```
Open: [http://localhost:7860](http://localhost:7860)

---

## Train from Scratch

```bash
export KAGGLE_USERNAME=your_username
export KAGGLE_KEY=your_api_key
python src/big_cats_classifier.py
# Output: best_big_cats.pth
```

---

## Requirements

```
gradio>=4.0
torch>=2.0
torchvision>=0.15
kagglehub>=0.1
Pillow>=9.0
requests>=2.28
twilio>=8.0
matplotlib>=3.7
numpy>=1.24
```

---

## Environment Variables

| Variable | Description |
|----------|-------------|
| `NTFY_TOPIC` | Ntfy topic name (default: `predatorwatch123`) |
| `TWILIO_SID` | Twilio Account SID |
| `TWILIO_TOKEN` | Twilio Auth Token |
| `TWILIO_FROM` | Twilio sender phone number |

---

## Links

- **Live App:** [huggingface.co/spaces/manojdas23/predator_watch](https://huggingface.co/spaces/manojdas23/predator_watch)
- **Dataset:** [Kaggle — Big Cats Image Classification](https://www.kaggle.com/datasets/patriciabrezeanu/big-cats-image-classification-dataset)
- **Ntfy:** [ntfy.sh](https://ntfy.sh)
  

---

## Author

**Manoj Das**

- Kaggle: [ManojDas245](https://www.kaggle.com/ManojDas245)
- Hugging Face: [manojdas23](https://huggingface.co/manojdas23)

---

## License

This project is licensed under the [MIT License](LICENSE).
