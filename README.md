# ⚡ Custom Wheels for RTX 6000 (Blackwell) - ComfyUI Trellis2

[![Hugging Face](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Download%20Files-yellow)](https://huggingface.co/PixWizardry/AI_Trellis2-WHLs-RTX-PRO-6000/tree/main)
[![Python](https://img.shields.io/badge/Python-3.12.10-blue.svg)](https://www.python.org/)
[![CUDA](https://img.shields.io/badge/CUDA-13.0-green.svg)](https://developer.nvidia.com/cuda-toolkit)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.10-red.svg)](https://pytorch.org/)
[![Platform](https://img.shields.io/badge/OS-Windows_11-blue)](https://www.microsoft.com/windows)
[![Architecture](https://img.shields.io/badge/Arch-Blackwell_sm120-purple)]()

This repository documents pre-compiled Windows Wheels (`.whl`) optimized specifically for the **NVIDIA RTX PRO 6000 Blackwell Workstation Edition**. These wheels satisfy the hard-to-build dependencies required for **Trellis2** implementation in ComfyUI.

These binaries are built to accelerate 3D generation and rendering pipelines, specifically targeting Compute Capability 12.0.

## 📥 Download

All `.whl` files are hosted on Hugging Face:
**[👉 Click here to download files from Hugging Face](https://huggingface.co/PixWizardry/AI_Trellis2-WHLs-RTX-PRO-6000/tree/main)**

## ⚠️ Hardware & Software Requirements

**Do not install these wheels if you do not match the environment below.** These are built for next-generation architecture and will likely fail on Ada Lovelace (4090) or Ampere (3090) cards due to the specific `sm120` compilation.

| Component | Requirement |
| :--- | :--- |
| **GPU** | NVIDIA RTX PRO 6000 (Blackwell) |
| **Compute Capability** | 12.0 (sm120) |
| **OS** | Windows 11 |
| **Python** | 3.12.10 |
| **PyTorch** | 2.10 |
| **CUDA** | 13.0 |

## 📦 Included Wheels

The following libraries have been compiled:

1.  **Flash Attention** (`flash_attn-2.8.3`)
2.  **Nvdiffrast** (`nvdiffrast-0.4.0`)
3.  **FlexGEMM** (`flex_gemm-0.0.1`)
4.  **CuMesh** (`cumesh-0.0.1`)
5.  **O-Voxel** (`o_voxel-0.0.1`)
6.  **Nvdiffrec** (`nvdiffrec-0.1.0`)

## 🚀 ComfyUI Compatibility

These wheels are essential for running the Z-Trellis2 workflow. They have been tested with the following custom nodes:

*   **Recommended (Faster):** [visualbruno/ComfyUI-Trellis2](https://github.com/visualbruno/ComfyUI-Trellis2)
*   **Compatible:** [PozzettiAndrea/ComfyUI-TRELLIS2](https://github.com/PozzettiAndrea/ComfyUI-TRELLIS2)

> **Note:** Initial testing indicates better performance/speed when using the **visualbruno** implementation with these specific binaries.

## 🛠️ Installation

This guide assumes you are using the standard **ComfyUI GitHub version** (cloned via git) on Windows 11.

> **Recommendation:** It is highly recommended to install these wheels inside a **Python Virtual Environment** (`venv`) to ensure these specific CUDA-compiled wheels do not conflict with your system packages or other projects.

### Step 1: Install Wheels
1.  Download the `.whl` files from the [Hugging Face Repository](https://huggingface.co/PixWizardry/AI_Trellis2-WHLs-RTX-PRO-6000/tree/main) to a local folder (e.g., `C:\wheels`).
2.  Open your terminal and ensure your virtual environment is **active**.
3.  Run the following commands:

```powershell
pip install "C:\wheels\flash_attn-2.8.3+cu130torch2.10.0cxx11abiTRUE-cp312-cp312-win_amd64.whl"
pip install "C:\wheels\nvdiffrast-0.4.0-cp312-cp312-win_amd64.whl"
pip install "C:\wheels\flex_gemm-0.0.1-cp312-cp312-win_amd64.whl"
pip install "C:\wheels\cumesh-0.0.1-cp312-cp312-win_amd64.whl"
pip install "C:\wheels\o_voxel-0.0.1-cp312-cp312-win_amd64.whl"
pip install "C:\wheels\nvdiffrec-0.1.0-cp312-cp312-win_amd64.whl"
```

### Step 2: Apply Name Conversion Shim (Crucial)
The `nvdiffrec` wheel requires a specific module name (`nvdiffrec_render`) that differs from the package name. To ensure the Trellis nodes can find the module, run this Python command immediately after installation:

```powershell
python -c "import site; import os; path = site.getsitepackages()[1]; f = open(os.path.join(path, 'nvdiffrec_render.py'), 'w'); f.write('from render import renderutils as _ru\nimport sys\nsys.modules[\"nvdiffrec_render\"] = _ru\nfrom render.renderutils import *'); f.close(); print('Shim created at: ' + path)"
```
*This script creates a redirection file in your site-packages so `import nvdiffrec_render` works correctly.*

## 🔗 Credits & Source Code

These wheels represent compiled versions of open-source libraries. Full credit goes to the original authors and researchers:

*   **Flash Attention:** [Tri Dao & wildminder](https://github.com/wildminder/AI-windows-whl)
*   **Nvdiffrast / Nvdiffrec:** [NVLabs](https://github.com/NVlabs/nvdiffrast)
*   **FlexGEMM:** [JeffreyXiang](https://github.com/JeffreyXiang/FlexGEMM)
*   **CuMesh:** [JeffreyXiang](https://github.com/JeffreyXiang/CuMesh)
*   **TRELLIS 2 / O-Voxel:** [Microsoft](https://github.com/microsoft/TRELLIS.2/tree/main/o-voxel)

---
*Disclaimer: These files are provided "as is" for experimental builds on Blackwell architecture. Please check the original repositories for licensing information regarding commercial use.*
