# Military Object Detection — YOLOv8

A deep learning-based military object detection system built with YOLOv8, capable of detecting 12 military-related object classes from images, videos, and live camera footage.

---

## Dataset

| Split | Images | Labels |
|-------|--------|--------|
| Train | 6,039  | 5,885  |
| Val   | 2,938  | 2,937  |
| Test  | 1,396  | 1,396  |
| **Total** | **10,373** | **10,218** |

📦 **Download Dataset:** [Google Drive](https://drive.google.com/drive/folders/1C6tLdAsGsz1stXxNVB_VqDNOk0YrB87p?usp=sharing)

---

## Classes

| ID | Class |
|----|-------|
| 0  | camouflage_soldier |
| 1  | weapon |
| 2  | military_tank |
| 3  | military_truck |
| 4  | military_vehicle |
| 5  | civilian |
| 6  | soldier |
| 7  | civilian_vehicle |
| 8  | military_artillery |
| 9  | trench |
| 10 | military_aircraft |
| 11 | military_warship |

---

## Model Architecture

- **Model:** YOLOv8n (nano)
- **Input Size:** 640 × 640
- **Framework:** Ultralytics YOLOv8
- **Epochs:** 50
- **Optimizer:** SGD (default)

---

## Hardware & Environment

| Component | Details |
|-----------|---------|
| GPU | NVIDIA RTX A4000 16GB |
| CUDA | 12.4 |
| Python | 3.13 |
| PyTorch | 2.6.0+cu124 |
| Ultralytics | 8.4.46 |
| OS | Windows 11 |

---

## Installation

```bash
# Clone the repository
git clone https://github.com/WajahatGul/military-object-detection.git
cd military-object-detection

# Create conda environment
conda create -n military python=3.13
conda activate military

# Install dependencies
pip install ultralytics
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu124
pip install opencv-python matplotlib
```

---

## Dataset Setup

1. Download the dataset from [Google Drive](https://drive.google.com/drive/folders/1C6tLdAsGsz1stXxNVB_VqDNOk0YrB87p?usp=sharing)
2. Extract it to `C:\Military_object_Detection\military_object_dataset`
3. Run the YAML path update cell in `Code.ipynb`

---

## Training

```bash
yolo task=detect mode=train model=yolov8n.pt \
data="C:\Military_object_Detection\military_object_dataset\military_dataset.yaml" \
epochs=50 imgsz=640 project=YOLO_FYP name=train_backup exist_ok=True device=0
```

### Resume Training

```bash
yolo task=detect mode=train \
model="C:\Military_object_Detection\military_object_dataset\runs\detect\YOLO_FYP\train_backup\weights\last.pt" \
data="C:\Military_object_Detection\military_object_dataset\military_dataset.yaml" \
epochs=100 imgsz=640 project=YOLO_FYP name=train_backup exist_ok=True resume=True device=0
```

---

## Inference

Run detection on any image, video, or live webcam using the file picker in `Code.ipynb`:

```python
import tkinter as tk
from tkinter import filedialog
from ultralytics import YOLO

root = tk.Tk()
root.withdraw()

SOURCE = filedialog.askopenfilename(
    title="Select Image or Video",
    filetypes=[
        ("All supported", "*.jpg *.jpeg *.png *.bmp *.mp4 *.avi *.mov *.mkv"),
        ("Images", "*.jpg *.jpeg *.png *.bmp"),
        ("Videos", "*.mp4 *.avi *.mov *.mkv"),
    ]
)

model = YOLO("YOLO_FYP/train_backup/weights/best.pt")
model.predict(source=SOURCE, conf=0.4, imgsz=640, save=True, show=True, device=0)
```

### Supported Sources

| Source | Value |
|--------|-------|
| Image | `"path/to/image.jpg"` |
| Video | `"path/to/video.mp4"` |
| Folder | `"path/to/folder"` |
| Webcam | `0` |
| IP Camera | `"rtsp://your-stream-url"` |

---

## Project Structure

```
military-object-detection/
│
├── military_object_dataset/
│   ├── train/
│   │   ├── images/
│   │   └── labels/
│   ├── val/
│   │   ├── images/
│   │   └── labels/
│   ├── test/
│   │   ├── images/
│   │   └── labels/
│   ├── Code.ipynb
│   ├── military_dataset.yaml
│   └── dataset.md
│
├── YOLO_FYP/
│   └── train_backup/
│       └── weights/
│           ├── best.pt
│           └── last.pt
│
└── README.md
```

---

## Results

| Metric | Value |
|--------|-------|
| mAP50 | 0.114 (epoch 8, training in progress) |
| Precision | 0.131 |
| Recall | 0.102 |

> Training is ongoing — results will be updated after full 50-epoch run.

---

## Author

**Wajahat Gul**
Bahria University, Islamabad
BS Computer Science
