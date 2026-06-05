# Arsitektur Data Warehouse dan Visualisasi Atoti pada Data Hospital Staffing (2009-2013)

Proyek Ujian Akhir Semester Data Warehouse Sains Data UNESA 2024E yang mengintegrasikan proses ETL (Extract, Transform, Load), optimasi database cloud menggunakan PostgreSQL, serta analisis multidimensi (OLAP) menggunakan Atoti untuk menganalisis data produktivitas staf rumah sakit.

Anggota Kelompok 8:
Ananda Keissa Az Zahra (24031554051), Laili Nurrohmatul Fadhila Z. (24031554093), Eka Putri Maharani (24031554121)

Dataset: https://catalog.data.gov/dataset/hospital-staffing-2009-2013

---

<img width="1600" height="900" alt="Image" src="https://github.com/user-attachments/assets/4fbc14e8-02df-4c95-8dcd-b151a7922f0c" />

## Arsitektur & Pipeline Sistem 

### 1. Data Source & Periodic Extraction
* **Dataset:** Menggunakan data publik resmi dari [California Health and Human Services (CHHS) / Data.gov](https://catalog.data.gov/dataset/hospital-staffing-2009-2013) dengan rentang waktu historis 5 tahun (2009–2013).
* **Automated Pipeline:** Menggunakan library `APScheduler` (`BackgroundScheduler`) untuk menyimulasikan proses ekstraksi data (*Data Acquisition*) secara periodik dan asinkron (interval 60 detik) tanpa memblokir *thread* utama sistem.

### 2. Proses ETL (Extract & Transform)
* **Extract:** Data mentah ditarik langsung melalui URL Raw CSV menggunakan `pandas` dengan mekanisme penanganan *bad lines* untuk mencegah *parser error*.
* **Transform & Data Quality:** * **Deteksi & Penanganan Anomali:** Mengimplementasikan metode statistik *Interquartile Range* (IQR) dan *Winsorization* untuk memfilter *outlier* pada metrik jam kerja.
  * **Granularitas Waktu:** Pemecahan dimensi waktu menjadi hierarki yang lebih spesifik (Tahun, Kuartal, Bulan).
  * **Standardisasi & Integritas:** Mengadopsi pemodelan *Star Schema* (Teori Vaisman/Inmon), mengonversi *Natural Keys* menjadi *Surrogate Keys*, dan mengkarantina data yang tidak memiliki referensi ke dalam tabel *Bad Records*.

### 3. Cloud Data Warehouse (Load)
* **Repositori:** Menggunakan layanan cloud database **Neon (PostgreSQL Serverless)**.
* **Optimasi OLAP (Extensions):**
  * **Table Partitioning:** Memecah tabel fakta berdasarkan rentang waktu (*Range Partitioning by Year*) untuk mempercepat pemindaian data.
  * **Indexing:** Mengimplementasikan arsitektur indeks `pg_trgm` dan `btree_gin` untuk akselerasi pencarian dimensi teks.
  * **Materialized Views:** Menggunakan tabel fisik pra-komputasi (`mv_hospital_summary`) untuk memangkas *response time* kueri analitik kompleks.

### 4. Multidimensional Analysis (Atoti OLAP)
* **DataMart & Cube:** Menginisiasi mesin *in-memory* Atoti untuk menyatukan dimensi kualitatif (Fasilitas, Lokasi) dan metrik kuantitatif (Total Hours).
* **Interactive Widgets:** Menyajikan *dashboard* visual untuk pelaporan *insight*.

---

<img width="1600" height="841" alt="Image" src="https://github.com/user-attachments/assets/bc369a77-55de-4295-a834-a9cf939a1bb7" />

## Insight & Analisis Bisnis (OLAP Results)
<img width="1066" height="735" alt="Image" src="https://github.com/user-attachments/assets/268c089e-ea9c-4f97-abe3-9249fbab3405" />

Berdasarkan *dashboard* Atoti yang telah dibangun, ditemukan beberapa *insight* strategis:
1. **Kinerja Kepemilikan & Anomali Data (Pivot Table):**
   * **Cost-Efficiency:** Fasilitas *For-Profit* (Swasta) mengelola jadwal staf secara lebih efisien dibandingkan *Non-Profit* (Yayasan), terlihat dari rasio jam kerja per pasien yang lebih terukur.
   * **Data Quality Issue:** Analisis berhasil mengisolasi anomali pada fasilitas berstatus *State* (Negara Bagian) yang selalu menunjukkan nilai nol mutlak (0), memvalidasi adanya *missing reporting* dari instansi pemerintah tanpa merusak metrik wilayah lain.
2. **Peta Efisiensi Layanan (Treemap):** Distribusi metrik Avg Hours per Patient Day pada tingkat County (Kabupaten) memperlihatkan ketimpangan standar pelayanan. Area dengan rasio sangat tinggi mengindikasikan pemborosan anggaran (overstaffing), sementara rasio yang sangat rendah berisiko pada kelelahan staf medis (understaffing).
3. **Beban Kerja Berdasarkan Granularitas Waktu (Line Chart):** 
   Visualisasi *Time Series* (Tahun × Kuartal) menunjukkan adanya fluktuasi beban kerja musiman. Wawasan ini krusial bagi manajemen rumah sakit untuk melakukan *capacity planning*, seperti penyesuaian jadwal *shift* atau rekrutmen staf *part-time* pada kuartal yang sibuk.
