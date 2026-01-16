# VeraLux Suite for PixInsight

![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)
![Platform](https://img.shields.io/badge/Platform-PixInsight-orange)
![Version](https://img.shields.io/badge/Version-2.1.0-green)

**The Unified Photometric Engine: HyperMetric Stretch & StarComposer.**

> [!IMPORTANT]
> **Original Authorship & Credits**
>
> This script is a port of the original **VeraLux** tools developed for Siril/Python by **Riccardo Paterniti**.
> All credit for the underlying mathematics, the "True Color" methodology, and the sensor weighting logic belongs to him.
>
> *   **Original Project:** [VeraLux.space](https://veralux.space)
> *   **Original Author:** [Riccardo Paterniti](https://github.com/RikyPate)
> *   **License:** GPL-3.0-or-later
>
> This repository is maintained by **lucasssvaz** (GitHub: [lucasssvaz](https://github.com/lucasssvaz)).
> The original JavaScript/PJSR port was created by **killerciao** (GitHub: [killerciao](https://github.com/killerciao)).
> Thanks to them for their contributions to the community.

---

## 📖 About This Port

This repository houses a **PixInsight JavaScript Runtime (PJSR)** implementation of the VeraLux ecosystem. It unifies two powerful engines into a single interface:

1.  **HyperMetric Stretch (HMS):** A physics-based linear-to-nonlinear stretcher (Ported from Python v1.3.1).
2.  **StarComposer:** A high-fidelity star reconstruction and composition engine (Ported from Python v1.0.2).

---

## ✨ Key Features

### 🖼️ Interactive Preview Window (New in v2.1.0)
Real-time parameter visualization for both modules.

*   **Live Preview:** See your stretch or composition results before processing with fast downsampled preview.
*   **Tab-Aware:** Automatically adapts to show HyperMetric Stretch or StarComposer preview based on active tab.
*   **Multiple Scale Options:** Choose from 1:1, 1:2, 1:4, 1:8, or Fit-to-Window for optimal preview speed.
*   **Interactive Navigation:** Pan with mouse drag, zoom with mouse wheel, inspect pixel values on hover.
*   **Position Memory:** Preview maintains your scroll position when refreshing parameters.
*   **Fast Iteration:** Optimized downsampling means instant parameter adjustments at lower scales.
*   **Reset Buttons:** Quick parameter reset for both HyperMetric Stretch and StarComposer modules.

### Module 1: HyperMetric Stretch
Operates on the axiom that standard histogram transformations destroy photometric color ratios (hue shifts).

*   **Smart Iterative Solver:** The "Auto-Calc" now simulates the entire post-stretch scaling pipeline. It detects black clipping ("Floating Sky Check") and iteratively adjusts the target to find the safest maximum contrast.
*   **Unified Color Strategy:** A single, intuitive slider to balance the equation.
    *   **Left (<0):** Cleans noise by increasing Shadow Convergence.
    *   **Right (>0):** Softens highlights by relaxing Color Grip.
*   **Sensor-Aware:** Expanded database including **Seestar S50/S30**, **IMX585**, and **Narrowband (HOO/SHO)** profiles.
*   **In-Place Editing:** Processes directly on the active image without creating duplicates.

### Module 2: StarComposer
Decouples the star field from the main object to prevent bloating and bleaching.

*   **Star Surgery:** Includes native tools for **Large Structure Rejection (LSR)** (removing galaxy cores from star masks) and **Optical Healing** (fixing chromatic aberration/halos).
*   **Vector Preservation:** Stretches stars without losing their core color temperature.
*   **Auto-Stretch Stars:** A specialized solver that ignores black backgrounds to find the optimal visibility for linear star masks.
*   **Composition Modes:** Choose between **Linear Add** (Physical accuracy) or **Screen** (Safe blending to prevent core saturation).

---

## ⏩ Automatic Installation (Recommended)

1.  Open **PixInsight**.
2.  Go to **Resources > Updates > Manage Repositories**.
3.  Click **Add** and paste the URL: `https://raw.githubusercontent.com/lucasssvaz/VeraLuxPorting/main/dist/`
4.  Click **OK**.
5.  Go to **Resources > Updates > Check for Updates**.
6.  Install the package and **Restart PixInsight**.

---

## 🛠️ Manual Installation

1.  Download the `.zip` file from the latest Release.
2.  Extract `VeraLuxSuite.js`.
3.  Open **PixInsight**.
4.  Go to **Script > Execute Script File...**
5.  Select `VeraLuxSuite.js`.

---

## 🚀 Usage Guide

### Preview Window Workflow (New in v2.1.0)
The right side of the interface shows a live preview of your processing:

*   **Scale Selection:** Choose preview resolution (1:1, 1:2, 1:4, 1:8, or Fit) for speed vs quality.
*   **Navigation:** 
    *   Mouse wheel to zoom in/out
    *   Click and drag to pan around the image
    *   Hover over pixels to see RGB values
*   **Preview Refresh:** Click to update the preview with current parameters (very fast at lower scales).
*   **Iterative Workflow:** Adjust parameters → Preview Refresh → Tweak → Preview Refresh → Process when satisfied.

### [Tab 1] HyperMetric Stretch
*Use this for your main image (Linear).*

1.  **Prerequisites:** Image must be **Linear**, **Background Extracted**, and **Color Calibrated (SPCC)**.
2.  **Mode:** Select **Ready-to-Use** for the new Unified Strategy workflow.
3.  **Sensor:** Select your camera profile (or Rec.709).
4.  **Solve:** Click **⚡ Auto-Calc Log D**. The solver will find the perfect stretch intensity.
5.  **Preview & Refine:** 
    *   Click **Preview Refresh** to see the result.
    *   Use the **Color Strategy** slider to adjust.
    *   Background too noisy? Slide **Left**.
    *   Stars too hard/white? Slide **Right**.
    *   Click **Preview Refresh** after each adjustment.
6.  **Process:** Click **PROCESS STRETCH** when satisfied (modifies active image in-place).
7.  **Reset:** Click **⟲ Reset** to restore default parameters.

### [Tab 2] StarComposer
*Use this to recombine a Linear Starmask with a Stretched Starless image.*

1.  **Input:** Select your **Starmask (Linear)** and **Starless Base (Stretched)** from the dropdowns.
2.  **Calibrate:** Click **⚡ Auto-Stretch Stars**. This calculates the intensity needed to make linear stars visible.
3.  **Preview & Surgery (Optional):**
    *   Click **Preview Refresh** to see the initial composition.
    *   **LSR:** Increase if your star mask contains pieces of nebulosity or galaxy cores.
    *   **Healing:** Increase if your stars have purple/green halos.
    *   Click **Preview Refresh** after each adjustment.
4.  **Process:** Click **PROCESS STAR COMPOSITION** when satisfied (creates new image).
5.  **Reset:** Click **⟲ Reset** to restore default StarComposer parameters.

---

## 📄 License

This project is licensed under the **GNU General Public License v3.0**.

You are free to use, modify, and distribute this software under the terms of the GPLv3. Any derivative works must also be open-source and preserve the original copyright notices and attribution to the original authors.

Copyright (c) 2026 lucasssvaz (Maintained JavaScript/PJSR port)
Copyright (c) 2025 killerciao (Original JavaScript/PJSR port)
Copyright (c) 2025 Riccardo Paterniti (Original Python implementation)
