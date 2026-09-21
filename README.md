# NeuroScan: Lightweight Explainable Brain Tumor MRI Classifier

NeuroScan is a **lightweight and explainable deep learning system** for 4-class brain MRI classification. Using **knowledge distillation from a ShuffleNetV2 teacher**, the final CNN student achieves **93.38% test accuracy** while being **2.60× smaller** in model size (**1.91 MB vs 4.97 MB**) and achieving **~9.65× faster inference** than the ShuffleNetV2 teacher. The system also provides **Grad-CAM++ and LIME explanations** through a FastAPI-based web application.

> **Research and educational use only. This system is not a clinical diagnostic tool.**

## 🚀 Live Demo

**Web App:** https://mri-brain-tumor-classifier.onrender.com/

Upload an MRI image and receive:

* Brain tumor classification
* Prediction result
* **Grad-CAM++** visualization
* **LIME** explanation

## 🧠 Supported Classes

| Class        | Prediction       |
| ------------ | ---------------- |
| `glioma`     | Glioma tumor     |
| `meningioma` | Meningioma tumor |
| `pituitary`  | Pituitary tumor  |
| `notumor`    | No tumor         |

## 📊 Key Results

### Teacher vs Final Student

| Metric         | ShuffleNetV2 |          S-CNN5 |
| -------------- | -----------: | --------------: |
| Role           |      Teacher |          Student |
| Test Accuracy  |   **95.69%** |       **93.38%** |
| Macro F1       |       0.9561 |       **0.9326** |
| Macro AUC (OvR)|       0.9910 |       **0.9851** |
| Parameters     |    **1.26M** |       **0.50M** |
| Model Size     | **4.97 MB**  |      **1.91 MB** |
| GFLOPs         |        0.181 |       **0.079** |
| GPU Latency    |     6.39 ms  |      **0.66 ms** |
| GPU Throughput |   156.58 FPS | **1509.48 FPS** |

### Per-Class Performance (S-CNN5, final student)

| Class        | Precision | Recall |     F1 |
| ------------ | --------: | -----: | -----: |
| Glioma       |    0.9701 | 0.8125 | 0.8844 |
| Meningioma   |    0.9195 | 0.9425 | 0.9309 |
| No Tumor     |    0.8862 | 0.9925 | 0.9363 |
| Pituitary    |    0.9705 | 0.9875 | 0.9789 |

### Efficiency Gain

* **2.60× smaller model**
* **~9.65× faster inference**
* **61.6% reduction in model size**
* **56.4% reduction in GFLOPs**
* Only **2.31 percentage points** lower test accuracy than the teacher

> Benchmark results were measured using 100 test samples after 10 warm-up samples on the research environment.

## 🔬 Knowledge Distillation

The student model is a custom **5-block CNN (S-CNN5)** trained using knowledge transferred from pretrained teacher networks.

Final configuration:

```text
Teacher       : ShuffleNetV2
Student       : CNN5
Temperature   : 6.0
Alpha         : 0.8
Input Size    : 240 × 240
Parameters    : 500,116
```

The distillation objective combines teacher soft predictions with ground-truth supervision:

```text
L = α × Lsoft + (1 − α) × Lhard
```

## 🛠️ Preprocessing

The MRI images undergo:

```text
MRI Image
   ↓
Grayscale Conversion
   ↓
Gaussian Blur
   ↓
Thresholding
   ↓
Morphological Processing
   ↓
Brain Region Cropping
   ↓
Resize to 240×240
   ↓
ImageNet Normalization
```

Training images additionally use data augmentation including rotation, translation, and horizontal flipping.

## 🔍 Explainable AI

NeuroScan provides two complementary explanation methods:

**Grad-CAM++**

* Highlights important spatial regions contributing to the prediction.

**LIME**

* Explains individual predictions using image superpixels.

This allows users to inspect not only **what the model predicted**, but also **which image regions influenced the prediction**.

## 🌐 API

**Base URL:**

```text
https://mri-brain-tumor-classifier.onrender.com
```

| Endpoint    | Method | Description                        |
| ----------- | ------ | ---------------------------------- |
| `/health`   | GET    | Check service/model readiness      |
| `/predict`  | POST   | Upload MRI and generate prediction |
| `/api/docs` | GET    | Swagger API documentation          |

### Example

```bash
curl -X POST \
  "https://mri-brain-tumor-classifier.onrender.com/predict" \
  -H "accept: application/json" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@brain_mri.jpg"
```

## 💻 Run Locally

```bash
git clone https://github.com/imtiazdeepto/MRI-Brain-Tumor-Classifier.git
cd MRI-Brain-Tumor-Classifier

python -m venv venv
source venv/bin/activate

pip install -r requirements.txt

uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

Open:

```text
App:       http://localhost:8000/
API Docs:  http://localhost:8000/api/docs
```

## 📁 Project Structure

```text
MRI-Brain-Tumor-Classifier/
│
├── main.py
├── inference.py
├── KD_T6.0_a0.8_latest.pth
├── requirements.txt
├── render.yaml
└── static/
```

## 📚 Dataset

**Brain Tumor MRI Dataset**

https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset

The dataset contains MRI images from four classes: Glioma, Meningioma, Pituitary, and No Tumor.

## 🧰 Tech Stack

* **Python**
* **PyTorch**
* **OpenCV**
* **FastAPI**
* **LIME**
* **Grad-CAM++**
* **Scikit-learn**
* **Render**

## ⚠️ Disclaimer

NeuroScan is developed strictly for **research and educational purposes**. It is **not a medical device** and should not be used for clinical diagnosis or medical decision-making. Model predictions should always be reviewed by qualified medical professionals.

## 👨‍💻 Author

Developed as a research project exploring **knowledge distillation, lightweight CNNs, explainable AI, and efficient deployment for brain MRI classification**.

## 📖 Citation

If you use this work, please cite:

```bibtex
@misc{neuroscan2026,
  title  = {NeuroScan: Lightweight Explainable Brain Tumor MRI Classification via Knowledge Distillation},
  author = {Hasib ur Rahman},
  year   = {2026},
  url    = {https://github.com/imtiazdeepto/MRI-Brain-Tumor-Classifier}
}
```

## 📄 License

Released under the [MIT License](LICENSE).

## 🔗 Links

* **Live App:** https://mri-brain-tumor-classifier.onrender.com/
* **API Docs:** https://mri-brain-tumor-classifier.onrender.com/api/docs
* **Dataset:** https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset
