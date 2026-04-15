# Analisis Korelasi Spasial SPI dan Titik Panas (Hotspot)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

## 📌 Deskripsi Proyek

Repositori ini memuat *master script* untuk analisis puncak penelitian Skripsi mengenai hubungan antara kekeringan meteorologis dengan kejadian kebakaran hutan dan lahan.

Skrip ini mengintegrasikan data spasial **Indeks Kekeringan (SPI 1, 3, 6, 12)** dengan data kejadian **Titik Panas (Hotspot NASA FIRMS MODIS)** di wilayah Provinsi Jambi selama kurun waktu 2010-2020. Tujuannya adalah untuk mengetahui pada skala waktu kekeringan keberapa (jangka pendek atau menengah) titik panas paling responsif muncul.

## 📐 Metodologi

Alur analisis dilakukan secara berurutan:

1.  **Filtering & Cleaning:**
    * Hotspot disaring hanya untuk kejadian dengan *Confidence Level* $\geq$ 80% (High Confidence).
    * Memperbaiki geometri poligon SPI (buffer 0) untuk menghindari *topology error* saat dioverlay.
2.  **Spatial Join (Overlay):**
    * Menggunakan `geopandas.sjoin` (operasi *Intersect*) untuk menampalkan titik koordinat hotspot ke atas poligon kelas SPI pada bulan dan tahun yang sama.
3.  **Agregasi Data:**
    * Menghitung total akumulasi hotspot yang jatuh pada masing-masing kelas SPI (1=Basah/Normal, ..., 5=Sangat Kering).
4.  **Uji Statistik (Korelasi Spearman):**
    * Menghitung Koefisien Korelasi Rank Spearman ($\rho$) antara peningkatan kelas SPI dengan jumlah hotspot.
    * Evaluasi signifikansi (p-value) dengan standar: `*` ($p < 0.05$), `**` ($p < 0.01$), `***` ($p < 0.001$).

## 📂 Struktur Direktori Google Drive

Script dirancang untuk eksekusi di **Google Colab** dan membutuhkan manajemen folder yang rapi di Google Drive:

```text
/My Drive/Colab Notebooks/Skripsi/
├── Batas Adm Jambi/
│   └── Adm_Jambi_Prov.shp
├── NASA-FIRMS Fire (New2)/
│   ├── DL_FIRE_M-C61_587963_2010/
│   └── ... (Data Hotspot per tahun)
├── SPI1-Output/
│   └── Polygon_Class/ ...
├── SPI3-Output/
│   └── Polygon_SPI3_2009-2011/ ...
├── SPI6-Output/
│   └── Polygon_SPI6_2009-2011/ ...
└── SPI12-Output/
    └── Polygon_SPI12_2009-2011/ ...
```

## 💻 Prasyarat Instalasi

```bash
pip install geopandas pandas matplotlib seaborn scipy numpy
```

## 🚀 Cara Penggunaan

1.  Pastikan semua folder hasil pemrosesan SPI (SPI-1 s.d. SPI-12) dan folder Hotspot sudah tersedia di *path* yang sesuai.
2.  Jalankan script utama `analisis_korelasi_spi_hotspot.ipynb`.
3.  Script akan memproses overlay spasial iteratif untuk setiap bulan selama 10 tahun.
4.  Hasil berupa tabel rekapitulasi `.csv` dan grafik komparasi korelasi akan otomatis dihasilkan.

## 📊 Hasil Visualisasi

Visualisasi utama dari script ini adalah **Bar Chart Komparasi Korelasi**, yang menampilkan nilai $\rho$ Spearman untuk setiap indeks SPI, lengkap dengan label signifikansi akademis (*p-value stars*). Bar berwarna merah menandakan korelasi yang signifikan secara statistik.

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Analisis Korelasi Spasial SPI dan Titik Panas (Hotspot)*. GitHub Repository.
