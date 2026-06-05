# Arsitektur Data Warehouse dan Visualisasi Atoti pada Data Hospital Staffing (2009-2013)

Proyek Data Warehouse Sains Data UNESA yang mengintegrasikan proses ETL (Extract, Transform, Load), optimasi database cloud menggunakan PostgreSQL, serta analisis multidimensi (OLAP) menggunakan Atoti untuk menganalisis data produktivitas staf rumah sakit.

Anggota Kelompok 8:
Ananda Keissa Az Zahra (24031554051), Laili Nurrohmatul Fadhila Z. (24031554093), Eka Putri Maharani (24031554121)

Dataset: https://catalog.data.gov/dataset/hospital-staffing-2009-2013

---

## Latar Belakang
Sektor pelayanan kesehatan, khususnya operasional rumah sakit, menghasilkan volume data historis yang sangat masif terkait alokasi jam kerja produktif (*productive hours*) staf medis. Evaluasi data ini sangat penting untuk memastikan keseimbangan beban kerja dan kualitas layanan pasien. 

Namun, pengolahan dataset operasional seperti **Hospital Staffing (2009–2013)** secara langsung seringkali menghadirkan tantangan teknis. Data rentan terhadap anomali, *missing value*, dan format yang tidak seragam. Selain itu, sistem basis data transaksional tradisional tidak dirancang untuk menangani kueri agregasi multidimensi berskala besar. 

Oleh karena itu, proyek ini mengimplementasikan **Data Warehouse** sebagai solusi sentralisasi data historis. Dengan mengkombinasikan pipeline ETL berbasis Python, repositori PostgreSQL di *cloud*, dan platform OLAP Atoti, proyek ini bertujuan untuk mengekstraksi wawasan berharga terkait efisiensi jam kerja staf di berbagai fasilitas kesehatan.

## Tujuan Proyek
1. **Membangun Repositori Sentral:** Mengimplementasikan Data Warehouse berbasis PostgreSQL Cloud (Neon).
2. **Standardisasi Kualitas Data:** Mengaplikasikan proses ETL pada data mentah, termasuk penanganan *missing value* dan deteksi anomali menggunakan metode *Interquartile Range* (IQR).
3. **Optimasi Performa:** Mengoptimalkan kueri analisis (OLAP) dengan menerapkan *Table Partitioning*, *Indexing*, dan *Materialized Views* pada PostgreSQL.
4. **Membangun OLAP Cube:** Mengembangkan DataMart dan OLAP cube interaktif menggunakan Atoti.
5. **Ekstraksi Wawasan (Insight):** Menyajikan visualisasi *dashboard* untuk memantau distribusi beban kerja berdasarkan fasilitas, jenis profesi, dan periode waktu.

---

## Arsitektur & Teknologi
* **Pemrosesan Data (ETL):** Python (Pandas)
* **Cloud Data Warehouse:** PostgreSQL (Neon Serverless)
* **Visualisasi & OLAP:** Atoti
* **Environment:** Jupyter Notebook 

---

## Insight & Temuan Utama (Business Value)
*(Bagian ini akan di-update)*

1. **Distribusi Beban Kerja:** ...
2. **Efisiensi Berdasarkan Tipe Kepemilikan:** ...
3. **Isu Kualitas Data (Data Quality):** ...

### Dashboard Screenshots
- `[Screenshot Tren Tahunan]`
- `[Screenshot Komposisi Tenaga Medis]`

---
