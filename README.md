<div align="center">

# 🌐 CrossGaussian

  <img src="ReadMe/PosterDesign.jpg" width="50%" />

> Exploring the design space of 3DGS-based spatial visualization and interaction for remote collaboration  
> User study: Reconstruction latency × 18 participants, Friedman + Wilcoxon analysis

</div>

<br>

🔮 CrossGaussian is the first study to integrate room-scale 3D Gaussian Splatting (3DGS) with real-time 360° video streaming for remote collaboration. While 360° cameras provide a wide field of view, the lack of depth information limits free spatial exploration; manual 3D modeling enables such exploration but incurs prohibitive production costs. CrossGaussian integrates the explicit scene representation and real-time rendering capabilities of 3DGS into a game engine, exploring the design space of spatial visualization and interaction techniques for remote collaboration environments.

<br>

🔮 CrossGaussian은 룸스케일 3D 가우시안 스플래팅(3DGS)과 실시간 360° 비디오 스트리밍을 원격 협업에 최초로 통합한 연구입니다. 360° 카메라는 넓은 시야각을 제공하지만 깊이 정보가 없어 자유로운 공간 탐색이 제한되고, 수동 3D 모델링은 자유 탐색을 가능케 하지만 제작 비용이 막대합니다. CrossGaussian은 3DGS의 명시적 장면 표현과 실시간 렌더링 특성을 게임 엔진에 통합하여, 원격 협업 환경에서의 공간 시각화 및 상호작용 디자인 스페이스를 탐구합니다.

---

<div align="center">

## 📋 Table of Contents

1. [🎯 Overview](#-overview)
2. [📚 Research Background](#-research-background)
3. [⚙️ System Architecture](#️-system-architecture)
4. [🎨 Design Space](#-design-space)
5. [🔬 User Study](#-user-study)
6. [🏆 Publications](#-publications)

---

</div>

<div align="center">
  
## 🎯 Overview

### 📖 Introduce

**Project**: CrossGaussian  
**Type**: Academic Research (HCI)  
**Duration**: 2024.09 ~ 2025.10  
**Advisors**: Seungjae Oh & Sangkeun Park (KHU)

![C#](https://img.shields.io/badge/C%23-239120?style=flat-square&logo=csharp&logoColor=white) ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=cplusplus&logoColor=white) ![HLSL](https://img.shields.io/badge/HLSL-5E5E5E?style=flat-square&logoColor=white) | ![Unity](https://img.shields.io/badge/Unity-000000?style=flat-square&logo=unity&logoColor=white) ![Meta Quest](https://img.shields.io/badge/Meta_Quest-1C1E20?style=flat-square&logo=meta&logoColor=white) | ![FFmpeg](https://img.shields.io/badge/FFmpeg-007808?style=flat-square&logo=ffmpeg&logoColor=white) ![NVIDIA](https://img.shields.io/badge/NVIDIA_NPP-76B900?style=flat-square&logo=nvidia&logoColor=white) | ![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white) ![Notion](https://img.shields.io/badge/Notion-000000?style=flat-square&logo=notion&logoColor=white)

<br>

### 👥 Team

| Position | Role | Name | Affiliation |
|:--|:--|:--|:--|
| 🎯 Research | First Author<br>Research Design & System Architecture | [Jaehyun Byun](https://github.com/BynPlay) | Kyung Hee Univ.<br>Computer Science |
| 💻 Dev | Co-Author<br>TCP Streaming & Network Protocol | [Byunghoon Kang](https://github.com/dot-mario) | Kyung Hee Univ.<br>Software Convergence |
| 💻 Dev | Co-Author<br>3DGS Rendering & Compute Shader | [Yonghyun Gwon](https://github.com/Noperi0r) | Kyung Hee Univ.<br>Software Convergence |
| 💻 Dev | Co-Author<br>Remote GPU Pipeline | Hongsong Choi | Kyung Hee Univ.<br>Computer Science |
| 📊 Research | Co-Author<br>User Study & Data Analysis | Yunseo Do | Kyung Hee Univ.<br>Computer Science |
| 📊 Research | Co-Author<br>User Study & Data Analysis | Eunho Kim | Kyung Hee Univ.<br>Computer Science |
| 🎓 Advisor | Academic Advisor | [Sangkeun Park](https://uxc.khu.ac.kr/) | Kyung Hee Univ.<br>UXC Lab |
| 🎓 Advisor | Academic Advisor | [Seungjae Oh](https://item.khu.ac.kr/) | Kyung Hee Univ.<br>ITEM Lab |

</div>

---

<br>

## 📚 Research Background

### 📖 Problem Statement

In co-located collaboration, users freely move through physical spaces, but **remote collaboration severely constrains this autonomy**. When remote participants want to examine objects behind the camera or change viewpoints, they must rely on local collaborators—increasing communication burden and limiting meaningful interaction.

동일 공간 협업에서는 물리적 공간을 자유롭게 이동할 수 있지만, **원격 협업에서는 이러한 자율성이 크게 제한**됩니다. 원격 참여자가 카메라 뒤 물체를 살펴보거나 시점을 변경하려면 현장 협업자에게 의존해야 하며, 이는 의사소통 부담을 증가시키고 의미 있는 상호작용을 제한합니다.

### 🔍 Prior Work & Limitations

| Approach | Limitation |
|:--|:--|
| **360° Video Streaming** | Wide FOV but lacks depth—no free spatial exploration<br>넓은 시야각이지만 깊이 정보 부재—자유로운 공간 탐색 불가 |
| **Manual 3D Modeling** | Enables free exploration but labor-intensive and costly<br>자유 탐색 가능하지만 제작에 많은 시간과 비용 소요 |
| **Photogrammetry** | SfM-based surface meshes with limited resolution and responsiveness<br>SfM 기반 표면 메쉬로 해상도와 반응성 제한 |
| **NeRF** | High-fidelity but computationally expensive for large environments<br>고품질이지만 대규모 환경에서 계산량 과다 |

### 💡 Our Approach: 3D Gaussian Splatting

3D Gaussian Splatting (3DGS) represents scenes as explicit 3D Gaussians with position, color, and alpha values—enabling **fast training, real-time rendering, and direct manipulation** unlike implicit NeRF representations.

3D 가우시안 스플래팅(3DGS)은 장면을 위치, 색상, 알파 값을 가진 명시적 3D 가우시안으로 표현하여, 암묵적 NeRF 표현과 달리 **빠른 학습, 실시간 렌더링, 직접 조작**이 가능합니다.

| Property | Benefit for Remote Collaboration |
|:--|:--|
| **Explicit Representation** | Direct access to depth, position, opacity for interaction<br>상호작용을 위한 깊이, 위치, 투명도 직접 접근 |
| **Fast Training** | Minutes instead of hours for room-scale reconstruction<br>룸스케일 재구성이 시간 단위에서 분 단위로 |
| **Real-time Rendering** | GPU-accelerated splatting for responsive exploration<br>반응적 탐색을 위한 GPU 가속 스플래팅 |
| **Alpha Blending** | Natural semi-transparent rendering for see-through effects<br>투시 효과를 위한 자연스러운 반투명 렌더링 |

---

<br>

## ⚙️ System Architecture

### 🔧 Automated End-to-End Pipeline

<div align="center">
  <img src="ReadMe/Pipeline.png" width="80%" />
</div>

```
┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│  Data Collection │    │  Reconstruction  │    │    Streaming     │    │    Rendering     │
│    Insta360      │    │   Remote GPU     │    │   TCP + FFmpeg   │    │  Unity + Shader  │
├──────────────────┤    ├──────────────────┤    ├──────────────────┤    ├──────────────────┤
│ • 360° Dual      │    │ • SSH/SFTP via   │    │ • H.264 Chunked  │    │ • h264_cuvid     │
│   Fisheye        │───→│   Paramiko       │───→│   Transfer       │───→│   GPU Decode     │
│ • H.264 Local    │    │ • SfM + Gaussian │    │ • ACK-based      │    │ • NV12→RGBA      │
│   Capture        │    │   Optimization   │    │   Reliability    │    │ • Fisheye→Sphere │
│ • Real-time      │    │ • Auto Download  │    │ • Low Latency    │    │ • 3DGS Composite │
│   Transmission   │    │   on Complete    │    │   Sync           │    │   Blending       │
└──────────────────┘    └──────────────────┘    └──────────────────┘    └──────────────────┘
```

### 📡 Module Details

| Module | Implementation |
|:--|:--|
| **Data Collection** | Insta360 SDK captures dual fisheye → local folder auto-save → triggers remote pipeline<br>Insta360 SDK 듀얼 피쉬아이 캡처 → 로컬 폴더 자동 저장 → 원격 파이프라인 트리거 |
| **Reconstruction** | Python Paramiko for SSH/SFTP → Remote GPU runs SfM + Gaussian optimization → Auto-download .ply<br>Python Paramiko SSH/SFTP → 원격 GPU에서 SfM + 가우시안 최적화 → .ply 자동 다운로드 |
| **Streaming** | TCP chunked transfer with ACK signals → prevents duplication/loss → near real-time delivery<br>ACK 신호 기반 TCP 청크 전송 → 중복/손실 방지 → 준실시간 전달 |
| **Rendering** | FFmpeg h264_cuvid GPU decode → NVIDIA NPP NV12→RGBA → Fisheye-to-sphere shader → 3DGS composite<br>FFmpeg h264_cuvid GPU 디코딩 → NVIDIA NPP NV12→RGBA → 피쉬아이-구면 셰이더 → 3DGS 합성 |

---

<br>

## 🎨 Design Space

We explore visualization and interaction techniques leveraging 3DGS's **explicit scene representation** and **precise depth rendering** for remote collaboration.

3DGS의 **명시적 장면 표현**과 **정밀한 깊이 렌더링**을 활용하여 원격 협업을 위한 시각화 및 상호작용 기법 디자인 스페이스를 탐색했습니다.

<div align="center">

### 🎛️ Core Design Features

</div>

| Feature | Description | Technical Implementation |
|:--|:--|:--|
| **🔀 Scene Blending** | Gradual transition between 360° stream and 3DGS to reduce motion sickness<br>모션 시크니스 감소를 위한 360° 스트림과 3DGS 간 점진적 전환 | Transparency control + color scaling on overlapping layers<br>중첩 레이어에 투명도 제어 + 색상 스케일링 |
| **👁️ Occlusion Detection** | Auto-detect and highlight regions hidden from 360° camera<br>360° 카메라에서 가려진 영역 자동 감지 및 하이라이트 | Compute shader shadow casting via pseudo-normal estimation<br>의사 법선 추정 기반 컴퓨트 셰이더 그림자 캐스팅 |
| **🔍 See-Through View** | Transparent exploration through occluding objects<br>차폐 물체를 투과하는 투명 탐색 | Alpha blending with 3D Gaussian opacity values<br>3D 가우시안 알파 값 기반 알파 블렌딩 |

<br>

<div align="center">
  <img src="ReadMe/Blending.png" width="45%" />
  <img src="ReadMe/Occlusion.png" width="45%" />
</div>

### 🔀 Blending of Overlapping Scenes

Abrupt transitions between 360° streaming and 3DGS can induce motion sickness and disrupt presence. Our system enables **simultaneous perception of real-time context (360°) while freely exploring alternative viewpoints (3DGS)** through adjustable transparency and color scaling.

360° 스트리밍과 3DGS 간 급격한 전환은 멀미를 유발하고 현존감을 저하시킬 수 있습니다. 본 시스템은 투명도와 색상 스케일링 조절을 통해 **실시간 맥락(360°)을 유지하면서 동시에 자유로운 시점(3DGS)으로 탐색**할 수 있게 합니다.

### 👁️ Occlusion-Aware Exploration

360° cameras cannot reveal areas behind structures. We leverage 3DGS depth information to:

1. **Auto-detect occluded regions** via per-pixel depth and pseudo-normal computation
2. **Highlight hidden areas** to guide remote users toward explorable zones
3. **Enable see-through** by manipulating Gaussian alpha values

360° 카메라는 구조물 뒤 영역을 볼 수 없습니다. 3DGS 깊이 정보를 활용하여:

1. 픽셀별 깊이와 의사 법선 계산을 통한 **차폐 영역 자동 감지**
2. 원격 사용자를 탐색 가능 구역으로 안내하는 **숨겨진 영역 하이라이트**
3. 가우시안 알파 값 조작을 통한 **투시 기능 활성화**

---

<br>

## 🔬 User Study

### 📋 Study Design: Reconstruction Latency & User Perception

Even state-of-the-art 3D reconstruction exceeds real-time thresholds (33ms), yet prior work rarely examines how delays affect user perception. We investigated **how reconstruction latency impacts perceived manipulability and trust in object existence**.

최신 3D 재구성 기술도 실시간 기준(33ms)을 초과하지만, 기존 연구에서는 지연이 사용자 인식에 미치는 영향을 거의 다루지 않았습니다. **재구성 지연이 조작 가능성 인식과 객체 존재 신뢰도에 미치는 영향**을 연구했습니다.

| Variable | Definition |
|:--|:--|
| **Design** | Within-subjects, counterbalanced |
| **Independent** | Reconstruction delay: 0.15s / 1s / 10s / 60s |
| **Dependent** | Perceived manipulability, Trust in existence (7-point Likert) |
| **Participants** | N=18 |

### 📊 Statistical Results

<div align="center">
  <img src="ReadMe/Results.png" width="60%" />
</div>

Non-parametric analysis (Friedman test + Wilcoxon signed-rank with Bonferroni correction):

| Measure | χ² | p-value | Pattern |
|:--|:--:|:--:|:--|
| **Manipulability** | 28.4 | < 0.001*** | 5.8±1.6 (0.15s) → 4.3±1.7 (60s) |
| **Trust** | 31.2 | < 0.001*** | 6.2±1.2 (0.15s) → 4.3±1.8 (60s) |

**Significant pairwise differences**: 0.15s-60s (p<0.001), 1s-60s (p<0.01), 0.15s-10s (p<0.05 for trust)

### 💬 Qualitative Findings

> *"After 10 seconds, I started losing trust in whether the object was really there."*

> *"At 60 seconds, it felt disconnected from reality—I didn't want to interact with it anymore."*

Most participants began losing trust after **10 seconds**, with **60-second delays** making objects feel disconnected and significantly reducing willingness to interact.

---

<br>

## 🏆 Publications

<div align="center">

### 📄 ACM UIST 2025 Poster

**[CrossGaussian: Enhancing Remote Collaboration through 3D Gaussian Splatting and Real-time 360° Streaming](https://doi.org/10.1145/3746058.3758348)**

*ACM Symposium on User Interface Software and Technology (UIST) 2025*  
*September 28–October 01, 2025 | Busan, Republic of Korea*

Jaehyun Byun, Byunghoon Kang, Yonghyun Gwon, Hongsong Choi, Yunseo Do, Eunho Kim, Sangkeun Park, Seungjae Oh

<br>

### 🎪 IEEE ISMAR 2025 Demo

**3-Day Live Demonstration**  
*IEEE International Symposium on Mixed and Augmented Reality 2025*

</div>

---

<br>

## 📚 References

1. Kerbl et al. 3D Gaussian Splatting for Real-Time Radiance Field Rendering. ACM TOG 2023
2. Sakashita et al. SharedNeRF: Leveraging Photorealistic and View Dependent Rendering for Real-time and Remote Collaboration. ACM CHI 2024
3. Huang et al. VirtualNexus: Enhancing 360-Degree Video AR/VR Collaboration with Environment Cutouts and Virtual Replicas. ACM UIST 2024
4. Gruenefeld et al. VRception: Rapid Prototyping of Cross-Reality Systems in Virtual Reality. ACM CHI 2022
5. Teo et al. Mixed reality remote collaboration combining 360 video and 3D reconstruction. ACM CHI 2019

---

<div align="center">

### 📬 Contact

For questions about this research, please contact:  
**Jaehyun Byun** — bjh1750@khu.ac.kr

[![Paper](https://img.shields.io/badge/ACM_Digital_Library-Open_Access-blue?style=for-the-badge&logo=acm&logoColor=white)](https://doi.org/10.1145/3746058.3758348)

</div>
