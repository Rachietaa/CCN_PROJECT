# CCN_PROJECT — 3D & 4D Human Body Reconstruction

A collection of three Google Colab notebooks for 3D/4D human body estimation, segmentation, and mesh reconstruction using state-of-the-art models.

---

## 📁 Notebooks

| Notebook | Model | Task |
|---|---|---|
| `sam3dbody.ipynb` | SAM-3D-Body (Meta) | 3D body shape & pose from a single image |
| `sambody4d.ipynb` | SAM-Body4D | 4D body tracking & segmentation from video |
| `GEM_X.ipynb` | GEM-X (NVIDIA) | Human pose estimation from video + SOMA→MHR conversion |

---

## 🔬 Project Overview

### 1. SAM-3D-Body (`sam3dbody.ipynb`)
Uses Meta's **SAM-3D-Body** model with a DINOv3 backbone to reconstruct 3D human body meshes from a single RGB image.

- Clones `facebookresearch/sam-3d-body`
- Downloads `facebook/sam-3d-body-dinov3` checkpoint from HuggingFace
- Runs single-image 3D body estimation
- Visualises and saves the output mesh render

**Requirements:** HuggingFace token, GPU runtime (T4 or better)

---

### 2. SAM-Body4D (`sambody4d.ipynb`)
Uses **SAM-Body4D** — a Gradio-based interactive app for 4D (video + 3D) human body segmentation and reconstruction.

- Clones `gaomingqi/sam-body4d`
- Installs SAM3 and detectron2
- Downloads model checkpoints via `scripts/setup.py`
- Patches `app.py` to run in Colab with a public Gradio share link
- Launches an interactive UI to annotate frames, generate masks, and produce 4D body reconstructions

**Requirements:** HuggingFace token, GPU runtime (A100 recommended)

---

### 3. GEM-X (`GEM_X.ipynb`)
Uses NVIDIA's **GEM-X** pipeline for video-based human pose estimation, followed by conversion from SOMA body parameters to MHR (Multi-Human Representation) format.

- Clones `NVlabs/GEM-X` with `third_party/soma` and `third_party/sam-3d-body` submodules
- Runs `demo_soma.py` on an input video to extract pose parameters
- Loads `hpe_results.pt` and converts body pose from `(B, 231)` → `(B, 77, 3)`
- Feeds through `SOMALayer` to get MHR vertices and joints
- Saves output to `mhr_params.pt` and renders per-frame visualisations

**Requirements:** HuggingFace token, GPU runtime (A100 recommended), Git LFS

---

## 🚀 How to Run

Each notebook is self-contained and runs on **Google Colab**.

1. Open the notebook in Colab
2. Set your runtime to **A100 GPU or higher** (Runtime → Change runtime type → GPU)
3. Add your HuggingFace token where prompted (`login(token="YOUR_TOKEN")`)
4. Run all cells in order

---

## 🗂 Repository Structure

```
CCN_PROJECT/
├── sam3dbody.ipynb       # 3D body from single image
├── sambody4d.ipynb       # 4D body tracking from video
├── GEM_X.ipynb           # GEM-X pose estimation + MHR conversion
└── README.md
```

---

## 📽 Results

### SAM-3D-Body

<img width="1192" height="376" alt="Image" src="https://github.com/user-attachments/assets/479e7581-acb9-49fd-a145-16bff2b6ab11" />

---

### SAM-Body4D

https://github.com/user-attachments/assets/076943fd-f223-4d75-b731-1a4534eb88b4

https://github.com/user-attachments/assets/41629462-307e-4380-83b3-4ae6e64a12e7


---

### GEM-X

https://github.com/user-attachments/assets/302f35ae-8971-4cf6-8807-3a841212edd5

https://github.com/user-attachments/assets/520d44ee-9790-4096-84dd-dac964921f08

---

## 🔗 References

- [SAM-3D-Body — Meta Research](https://github.com/facebookresearch/sam-3d-body)
- [SAM-Body4D](https://github.com/gaomingqi/sam-body4d)
- [GEM-X — NVIDIA Labs](https://github.com/NVlabs/GEM-X)
- [SOMA — Body Model Layer](https://github.com/nghorbani/soma)

---

## 📋 Notes

- All notebooks were developed and tested on Google Colab with A100/T4 GPUs
- `GEM_X.ipynb`: the `soma` system package conflicts with GEM-X's bundled SOMA — the notebook patches `sys.path` to load the correct version from `third_party/soma`
- `sambody4d.ipynb`: `app.py` is auto-patched each session to set the correct `ROOT` path and enable Gradio's public share link
