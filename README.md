<div align="center">

  <!-- Ganti LOGO.png dengan logo/banner proyek Anda -->
  <img src="assets/banner.png" alt="Trash Vision Banner" width="100%"/>

  <h1>🗑️ Trash Vision</h1>

  <p>
    <strong>Drone-Based Real-Time Waste Detection System for Environmental Monitoring</strong>
  </p>

  <p>
    <a href="https://github.com/[username]/trash-vision/stargazers"><img src="https://img.shields.io/github/stars/[username]/trash-vision?style=flat-square&color=yellow" alt="Stars"/></a>
    <a href="https://github.com/[username]/trash-vision/network/members"><img src="https://img.shields.io/github/forks/[username]/trash-vision?style=flat-square&color=blue" alt="Forks"/></a>
    <a href="https://github.com/[username]/trash-vision/commits/main"><img src="https://img.shields.io/github/last-commit/[username]/trash-vision/main?style=flat-square" alt="Last Commit"/></a>
    <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="License"/>
    <img src="https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python&logoColor=white" alt="Python"/>
    <img src="https://img.shields.io/badge/YOLO-Object%20Detection-red?style=flat-square" alt="YOLO"/>
    <img src="https://img.shields.io/badge/Raspberry%20Pi%205-Onboard%20AI-c51a4a?style=flat-square&logo=raspberrypi&logoColor=white" alt="Raspberry Pi"/>
    <img src="https://img.shields.io/badge/Pixhawk%206X-Flight%20Controller-orange?style=flat-square" alt="Pixhawk"/>
  </p>

  <p>
    <a href="README.md"><strong>🇬🇧 English</strong></a> · <a href="README.id.md">🇮🇩 Bahasa Indonesia</a>
  </p>

</div>

---

> **Trash Vision** adalah sistem pendeteksi sampah berbasis drone yang dirancang untuk pemantauan lingkungan secara otomatis dan real-time. Menggunakan arsitektur YOLO yang berjalan di atas Raspberry Pi 5 dengan akselerator AI onboard, sistem ini mampu mendeteksi, melokasikan, dan memetakan sebaran sampah dari udara — khususnya difokuskan untuk monitoring lingkungan perairan seperti Sungai Citarum.

---

## 📋 Quick Navigation

| Bagian | Deskripsi |
|---|---|
| [Tentang Proyek](#-tentang-proyek) | Latar belakang, motivasi, dan tujuan sistem |
| [Arsitektur Sistem](#-arsitektur-sistem) | Komponen hardware dan alur kerja keseluruhan |
| [Tech Stack](#-tech-stack) | Teknologi, framework, dan library yang digunakan |
| [Fitur Utama](#-fitur-utama) | Kemampuan dan fitur yang tersedia |
| [Instalasi & Setup](#-instalasi--setup) | Cara menyiapkan dan menjalankan sistem |
| [Cara Penggunaan](#-cara-penggunaan) | Panduan operasional sistem |
| [Hasil & Evaluasi](#-hasil--evaluasi) | Metrik performa model deteksi |
| [Struktur Repositori](#-struktur-repositori) | Penjelasan direktori dan file |

---

## 🌍 Tentang Proyek

Penumpukan sampah di area terbuka dan perairan — khususnya sungai-sungai besar di Indonesia — merupakan masalah lingkungan yang membutuhkan solusi pemantauan yang **cepat, akurat, dan skalabel**. Inspeksi manual membutuhkan banyak sumber daya manusia, waktu, dan biaya, serta sering kali tidak mampu menjangkau area yang luas secara efisien.

**Trash Vision** hadir sebagai solusi berbasis teknologi drone dan kecerdasan buatan yang dapat:

- 🔍 **Mendeteksi keberadaan sampah** dari ketinggian secara real-time
- 📍 **Mencatat koordinat GPS** titik-titik sampah secara otomatis
- 🗺️ **Memetakan persebaran sampah** dalam satu area misi
- 📊 **Menghasilkan laporan** lengkap setiap akhir sesi penerbangan
- ⚡ **Memproses inferensi secara onboard** tanpa bergantung koneksi internet

> 💡 Proyek ini dikembangkan sebagai Tugas Akhir (Skripsi) dengan fokus integrasi subsistem visi komputer pada drone otonom untuk mendukung program monitoring lingkungan hidup.

---

## 🏗️ Arsitektur Sistem

```
┌─────────────────────────────────────────────────────────────┐
│                        TRASH VISION                         │
│                   Drone-Based AI System                      │
└─────────────────────────────────────────────────────────────┘
          │
          ▼
┌─────────────────┐     ┌──────────────────┐
│   DRONE FRAME   │     │  FLIGHT CONTROL  │
│  Holybro X500V2 │────▶│  Pixhawk 6X      │
└─────────────────┘     └──────────────────┘
                                │
                    ┌───────────▼───────────┐
                    │   ONBOARD COMPUTER    │
                    │   Raspberry Pi 5      │
                    │   + AI Accelerator    │
                    └───────────┬───────────┘
                                │
               ┌────────────────┼────────────────┐
               ▼                ▼                ▼
        ┌──────────┐    ┌──────────────┐  ┌──────────────┐
        │  Camera  │    │  YOLO Model  │  │  GPS Module  │
        │  Input   │───▶│  Detection   │  │  Telemetry   │
        └──────────┘    └──────┬───────┘  └──────┬───────┘
                               │                  │
                               ▼                  ▼
                        ┌─────────────────────────────┐
                        │       BACKEND / API          │
                        │  (Data Fusion & Reporting)   │
                        └─────────────────────────────┘
                                      │
                                      ▼
                        ┌─────────────────────────────┐
                        │      OUTPUT / DASHBOARD      │
                        │  • Peta Sebaran Sampah        │
                        │  • Koordinat GPS Terdeteksi  │
                        │  • Confidence Score          │
                        │  • Laporan Misi              │
                        └─────────────────────────────┘
```

### Alur Kerja Sistem

1. **Perencanaan Misi** — Operator menentukan rute penerbangan dan area target
2. **Penerbangan Drone** — Drone mengikuti rute yang telah diprogram
3. **Akuisisi Data** — Kamera mengambil frame secara real-time dari udara
4. **Preprocessing** — Frame diproses (invert warna, normalisasi, konversi Numpy)
5. **Inferensi AI** — Model YOLO mendeteksi objek sampah pada setiap frame
6. **Fusi Data** — Koordinat GPS dipadukan dengan hasil deteksi
7. **Transmisi** — Data dikirim ke backend untuk disimpan dan divisualisasikan
8. **Pelaporan** — Laporan misi lengkap dihasilkan otomatis

---

## 🛠️ Tech Stack

### Hardware

| Komponen | Spesifikasi |
|---|---|
| **Frame Drone** | Holybro X500 V2 |
| **Flight Controller** | Pixhawk 6X |
| **Onboard Computer** | Raspberry Pi 5 |
| **AI Accelerator** | *(Haiku AI Accelerator / sesuaikan)* |
| **Transmitter** | RadioMaster TX16S |
| **Kamera** | *(Sesuaikan dengan spesifikasi kamera yang dipakai)* |

### Software & Library

| Kategori | Teknologi |
|---|---|
| **Bahasa Pemrograman** | Python 3.10+ |
| **Model Deteksi** | YOLO (YOLOv8 / versi sesuai implementasi) |
| **Computer Vision** | OpenCV |
| **Numerik** | NumPy |
| **Flight Control** | MAVLink / DroneKit *(sesuaikan)* |
| **Backend** | Flask / FastAPI *(sesuaikan)* |
| **Visualisasi** | *(Folium / Leaflet / Sesuaikan)* |

---

## ✨ Fitur Utama

- **🤖 Real-Time Waste Detection** — Inferensi model YOLO langsung onboard Raspberry Pi 5 tanpa memerlukan koneksi cloud
- **📍 GPS-Tagged Detection** — Setiap deteksi sampah secara otomatis ditandai dengan koordinat GPS
- **⚡ Preprocessing Pipeline** — Pipeline preprocessing gambar dengan invert warna dan normalisasi untuk meningkatkan akurasi di kondisi pencahayaan beragam
- **📡 Data Transmission** — Hasil deteksi dikirim langsung ke backend secara real-time
- **🗺️ Waste Mapping** — Peta persebaran sampah yang dihasilkan dari setiap misi penerbangan
- **📊 Mission Report** — Laporan otomatis mencakup jumlah deteksi, koordinat, confidence score, dan cakupan area
- **🎯 Confidence Scoring** — Setiap deteksi disertai persentase kepercayaan model

---

## 🚀 Instalasi & Setup

### Prasyarat

```bash
# Python 3.10 atau lebih baru
python --version

# pip (package manager)
pip --version
```

### Instalasi Dependencies

```bash
# Clone repositori
git clone https://github.com/[username]/trash-vision.git
cd trash-vision

# Buat virtual environment
python -m venv venv
source venv/bin/activate  # Linux/macOS
venv\Scripts\activate     # Windows

# Install dependencies
pip install -r requirements.txt
```

### Setup Model

```bash
# Letakkan file model YOLO (.pt) ke direktori berikut:
mkdir -p models/
# Contoh: models/trash_detection.pt
```

### Konfigurasi

Salin file konfigurasi contoh dan sesuaikan dengan setup Anda:

```bash
cp config/config.example.yaml config/config.yaml
```

Edit `config/config.yaml`:

```yaml
model:
  path: "models/trash_detection.pt"
  confidence_threshold: 0.5

camera:
  source: 0  # 0 untuk webcam, atau path ke video file

gps:
  port: "/dev/ttyUSB0"
  baudrate: 9600

backend:
  url: "http://localhost:5000/api/detection"
```

---

## 🎮 Cara Penggunaan

### Menjalankan Sistem Deteksi

```bash
# Jalankan sistem deteksi utama
python src/main.py

# Dengan mode verbose (tampilkan bounding box di layar)
python src/main.py --display

# Deteksi dari file video (untuk testing)
python src/main.py --source path/to/video.mp4
```

### Menjalankan Backend

```bash
# Jalankan backend server
python backend/app.py

# Server akan berjalan di http://localhost:5000
```

### Testing Model

```bash
# Test inferensi pada gambar statis
python src/detect.py --image path/to/image.jpg --model models/trash_detection.pt

# Evaluasi performa model pada dataset test
python src/evaluate.py --dataset data/test/
```

---

## 📈 Hasil & Evaluasi

Model deteksi sampah dievaluasi menggunakan metrik standar computer vision:

| Metrik | Nilai |
|---|---|
| **Precision** | *(Hasil evaluasi Anda)* |
| **Recall** | *(Hasil evaluasi Anda)* |
| **F1-Score** | *(Hasil evaluasi Anda)* |
| **mAP@0.5** | *(Hasil evaluasi Anda)* |

> 📝 **Catatan**: Evaluasi dilakukan pada dataset yang dikumpulkan khusus untuk pemantauan sampah di lingkungan perairan Indonesia.

<!-- Tambahkan screenshot/gambar hasil deteksi di sini -->
<!--
### Contoh Hasil Deteksi

<div align="center">
  <img src="assets/detection_sample.jpg" alt="Contoh Hasil Deteksi" width="80%"/>
  <br/>
  <em>Contoh deteksi sampah dari perspektif udara dengan bounding box dan confidence score</em>
</div>
-->

---

## 📁 Struktur Repositori

```
trash-vision/
├── assets/                  # Gambar, logo, dan media dokumentasi
├── config/                  # File konfigurasi sistem
│   └── config.example.yaml
├── data/                    # Dataset (tidak disertakan di repo)
│   ├── train/
│   ├── val/
│   └── test/
├── models/                  # Model YOLO yang telah dilatih
├── src/                     # Source code utama
│   ├── main.py              # Entry point sistem deteksi
│   ├── detect.py            # Modul inferensi YOLO
│   ├── preprocess.py        # Pipeline preprocessing gambar
│   ├── gps.py               # Modul komunikasi GPS
│   └── transmitter.py       # Modul pengiriman data ke backend
├── backend/                 # Backend API server
│   └── app.py
├── notebooks/               # Jupyter notebooks (training, analisis)
├── tests/                   # Unit tests
├── requirements.txt         # Python dependencies
└── README.md
```

---

## 📄 Lisensi

Proyek ini dilisensikan di bawah [MIT License](LICENSE).

---

## 🤝 Kontribusi

Proyek ini merupakan bagian dari Tugas Akhir. Saran dan masukan tetap diterima melalui [Issues](https://github.com/[username]/trash-vision/issues).

---

## 📞 Kontak

**Penulis**: [Nama Lengkap Anda]  
**Email**: [email@domain.com]  
**Institusi**: [Nama Universitas / Program Studi]  
**Tahun**: 2025

---

<div align="center">
  <sub>Dibuat dengan ❤️ untuk lingkungan yang lebih bersih · Trash Vision © 2025</sub>
</div>
