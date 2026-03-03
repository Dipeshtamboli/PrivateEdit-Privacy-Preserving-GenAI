

# PrivateEdit: Privacy-Preserving Pipeline for Generative Image Editing

**PrivateEdit** is a modular, on-device pipeline designed to protect biometric privacy during generative image editing. It allows users to leverage powerful third-party cloud APIs (like Midjourney, DALL-E, or Gemini) for tasks such as professional headshot generation without ever uploading their actual facial identity.

## 🛡️ The Core Problem: The "Biometric Disclosure" Tradeoff

Current generative AI workflows require users to upload high-fidelity facial images to untrusted cloud servers. As shown in the **Baseline (A)** section of the figure below, this exposes users to:

*  **Biometric Profiling:** Unauthorized inference of sex, eye color, age, and other traits.


* **Unauthorized Data Sale:** Redistribution of sensitive biometric data.


* **Malicious Generative Editing:** Misuse of the original likeness for unintended purposes.



## 🏗️ The PrivateEdit Pipeline

Our approach enforces **Privacy-by-Design** by separating the image into two streams: **Editable Context** (sent to the cloud) and **Identity Core** (kept on-device).

![PrivateEdit Pipeline Overview](figs/main_fig.png)

### System Architecture & Privacy Analysis

The **PrivateEdit** pipeline introduces a robust defense against the privacy risks inherent in cloud-based generative AI by decoupling a user's biometric identity from the editable image context. As illustrated in the figure above **(B)**, our system utilizes lightweight on-device masking to neutralize sensitive facial features before they reach any "Privacy-Risk 3rd Party" (PR3P) AI model. This architecture prevents high-stakes threats such as biometric profiling and malicious identity reconstruction while maintaining the utility of professional-grade generative editing. Quantitative evaluations **(C)** across multiple foundation models show that our masking mechanism collapses the accuracy of sensitive attribute inference—such as eye color and presence of a mustache—to near-random levels, effectively putting data sovereignty back into the hands of the user.

---

### 1. On-Device Masking (Upstream)

Before any data leaves the device, a lightweight neural network performs facial landmark detection and segmentation.

* **Selective Occlusion:** The system generates a binary mask covering the inner facial region.


* **Privacy Injection:** Facial pixels are replaced with a constant value or noise.


* **Result:** The transmitted image contains clothing and background, but **zero** biometric facial data.



### 2. Cloud-Based Generative Editing

The masked image is sent to a third-party API with a prompt (e.g., *"Make a professional headshot out of this picture"*).

* **Model Agnostic:** Works with any "black-box" API without requiring retraining.


* **Contextual Synthesis:** The model fills in the background and attire while "hallucinating" a generic face in the masked area.



### 3. Local Reintegration (Downstream)

Once the edited image is returned, the final composite is generated locally on the user's device:

* **Geometric Alignment:** The system aligns the original facial pixels with the new edited context.


* **Seamless Blending:** The original identity is restored into the professionally edited environment using local reconstruction.



---

## 📊 Key Features

* **Zero-Trust Architecture:** The cloud provider never sees your actual biometric identity.


* **Tunable Privacy:** Adjustable "Mask Ratio" allows users to balance between privacy and output fidelity.


* **Inference Hardening:** Proven to reduce attribute inference (age, eye color, mustache) by over **50%** against state-of-the-art models like Gemini, Grok, and LLaMA.



## 📜 Citation

If you use this work in your research, please cite our paper:

```bibtex
@article{tamboli2025privateedit,
  title={PrivateEdit: A Privacy-Preserving Pipeline for Face-Centric Generative Image Editing},
  author={Tamboli, Dipesh and Punyamoorty, Vineet and Pawar, Atharv and Aggarwal, Vaneet},
  journal={IEEE Transactions on Artificial Intelligence},
  year={2026}
}

```

---
