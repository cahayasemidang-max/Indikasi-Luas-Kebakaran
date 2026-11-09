# Pemetaan dan Analisis Spasial Indikasi Luas Kebakaran Hutan & Lahan (Karhutla) di Indonesia (2018–2026)

Proyek ini bertujuan untuk melakukan pemrosesan data spasial, analisis statistik, serta visualisasi indikasi kebakaran hutan dan lahan (karhutla) di Indonesia dengan menggunakan pustaka **GeoPandas**, **Pandas**, dan **Matplotlib** di lingkungan Python.

Studi kasus utama difokuskan pada analisis spasiotemporal karhutla di **Provinsi Kalimantan Barat** (wilayah dengan luas karhutla tertinggi di Indonesia pada tahun 2026) dan didukung oleh gabungan dataset *hotspot* penginderaan jauh satelit NASA VIIRS (NOAA-21).

---

## 📌 Fitur Utama & Capaian Tugas
1. **Pembersihan Data Otomatis**:
   - Konversi format angka teks standar Indonesia (`.` ribuan dan `,` desimal) ke numerik `float` pada data statistik karhutla.
   - Perhitungan otomatis metrik agregasi: `Total_2018_2026` dan `Rata_Rata_Tahunan`.
2. **Implementasi 4 Instruksi Tugas GeoPandas**:
   - **Tugas 1**: Pembacaan batas wilayah GeoJSON dan titik hotspot satelit, penggabungan (*attribute merge*), pengecekan `.crs`, `.head()`, dan `.plot()`.
   - **Tugas 2**: Penanganan CRS (`set_crs(epsg=4326)`) dan transformasi proyeksi ke sistem metrik (`to_crs(epsg=3857)`), serta plot perbandingan.
   - **Tugas 3**: Pembuatan buffer 500 meter (`.buffer(500)`) pada titik klaster hotspot dan garis batas wilayah di Kalimantan Barat dengan visualisasi *zoom-in* sebelum vs sesudah buffer.
   - **Tugas 4**: Peta *Choropleth* dengan skema warna *sequential* (`YlOrRd`, `OrRd`), grid komparasi tahun 2018–2026, dan selector interaktif dinamis.
3. **Analisis Spasial Mendalam Kalimantan Barat**:
   - *Spatial Join* (`gpd.sjoin`) untuk memetakan titik hotspot per kabupaten/kota.
   - Grafik tren tahunan luas kebakaran (2018–2026) dan grafik tren bulanan jumlah titik api (Nov 2025 – Nov 2026).
4. **Standar Kartografi Lengkap & Auto-Export**:
   - Setiap peta dilengkapi dengan **North Arrow** (arah mata angin), **Scale Bar** (batang skala jarak km), dan **Legenda**.
   - Otomatis menyimpan (*auto-save*) peta ke folder `output_maps/` dalam format **PNG resolusi tinggi (300 DPI)**.

---

## 🗂️ Struktur Direktori Proyek

```text
Indikasi-Luas-Kebakaran/
├── dataset/
│   ├── indonesia-provinsi.geojson          # Batas administrasi 38 Provinsi Indonesia
│   └── indonesia-kabupaten-kota.geojson    # Batas administrasi 514 Kabupaten/Kota Indonesia
├── output_maps/                            # Direktori penyimpanan otomatis hasil peta (PNG 300 DPI)
│   ├── tugas1_plot_sederhana_geojson_dan_hotspot.png
│   ├── tugas2_perbandingan_crs_epsg4326_vs_epsg3857.png
│   ├── tugas3_peta_dan_tren_tahunan_kalimantan_barat.png
│   ├── tugas3_tren_bulanan_dan_komposisi_hotspot_kalbar.png
│   ├── tugas3_perbandingan_buffer_500m_titik_dan_garis.png
│   ├── tugas4_peta_choropleth_nasional_provinsi_dan_kota.png
│   ├── tugas4_peta_choropleth_kalbar_dan_overlay_hotspot.png
│   ├── tugas4_grid_semua_tahun_indonesia_2018_2026.png
│   ├── tugas4_grid_semua_tahun_kalbar_2018_2026.png
│   └── peta_karhutla_kalimantan_barat_2026.png
├── fire_archive_J2V-C2_840020.csv          # Hotspot VIIRS NOAA-21 Archive (Nov 2025 - Jul 2026)
├── fire_nrt_J2V-C2_840020.csv              # Hotspot VIIRS NOAA-21 NRT (Agu 2026 - Nov 2026)
├── luas-karhutla-provinsi.xlsx             # Data luas karhutla 38 Provinsi (2018-2026)
├── luas-karhutla-kota.xlsx                 # Data luas karhutla 514 Kabupaten/Kota (2018-2026)
├── praktikum_geopandas1.ipynb              # Notebook referensi praktikum awal
├── tugas_peta_kebakaran.ipynb              # Notebook utama penyelesaian tugas & analisis lengkap
└── README.md                               # Dokumentasi proyek
```

---

## 📊 Dataset & Sumber Data

1. **Data Luas Kebakaran Hutan dan Lahan (`.xlsx`)**:
   - `luas-karhutla-provinsi.xlsx`: Luas karhutla per provinsi di Indonesia tahun 2018 s.d. 2026 (satuan Hektare/Ha).
   - `luas-karhutla-kota.xlsx`: Luas karhutla per kabupaten/kota di Indonesia tahun 2018 s.d. 2026 (satuan Hektare/Ha).
2. **Data Spasial Batas Administrasi (`.geojson`)**:
   - **38 Provinsi Indonesia**: [denyherianto/indonesia-geojson-topojson-maps-with-38-provinces](https://github.com/denyherianto/indonesia-geojson-topojson-maps-with-38-provinces) (mencakup pemekaran DOB Papua).
   - **514 Kabupaten/Kota Indonesia**: [ardian28/GeoJson-Indonesia-38-Provinsi](https://github.com/ardian28/GeoJson-Indonesia-38-Provinsi) (diselaraskan dengan kode wilayah BPS/Kemendagri).
3. **Data Titik Hotspot Satelit (`.csv`)**:
   - [NASA FIRMS (Fire Information for Resource Management System)](https://firms.modaps.eosdis.nasa.gov/)
   - Instrumen: **VIIRS (Satelit NOAA-21 / J2V-C2)**
   - Gabungan *Standard Processing (Archive)* dan *Near Real-Time (NRT)* periode 7 November 2025 s.d. 7 November 2026 (**Total 661.138 titik hotspot nasional**, disaring pada titik *High Confidence* untuk visualisasi yang akurat).

---

## 🛠️ Instalasi & Kebutuhan Lingkungan

Pastikan Anda telah menginstal **Python 3.10+** dan paket-paket pustaka berikut:

```bash
pip install geopandas pandas matplotlib shapely openpyxl ipywidgets
```

Jika menggunakan Jupyter Lab / Notebook, aktifkan widget interaktif:
```bash
jupyter nbextension enable --py widgetsnbextension
```

---

## 🚀 Panduan Menjalankan Notebook

1. Buka Jupyter Notebook atau VS Code:
   ```bash
   jupyter notebook tugas_peta_kebakaran.ipynb
   ```
2. Jalankan sel secara berurutan (*Run All* atau jalankan sel per sel dari atas ke bawah).
3. Setiap sel pemetaan akan:
   - Menampilkan visualisasi peta langsung di dalam notebook.
   - Menyimpan salinan gambar peta beresolusi tinggi ke dalam folder `output_maps/` dalam format `.png`.
4. Pada **Bagian 4D**, gunakan kontrol interaktif *Dropdown* untuk memilih tahun (`2018`–`2026`, `Total_2018_2026`, `Rata_Rata_Tahunan`) serta provinsi mana pun untuk menganalisis peta wilayah kabupaten/kota terkait.

---

## 📈 Alur Kode & Metodologi di Notebook

| Bagian | Topik | Output Visual / Analisis |
| :---: | :--- | :--- |
| **0** | **Data Wrangling** | Pembersihan angka teks dan pembentukan agregat statistik dari Excel. |
| **1** | **Tugas 1: Peta Dasar** | Plot poligon provinsi, kabupaten/kota, dan sebaran 20.130 titik hotspot nasional. |
| **2** | **Tugas 2: Transformasi CRS** | Perbandingan CRS `EPSG:4326` (derajat) vs `EPSG:3857` (proyeksi metrik meter). |
| **3** | **Tugas 3: Studi Kasus Kalbar & Buffer 500m** | - Peta 14 Kab/Kota Kalbar & grafik tren luas karhutla 2018–2026.<br>- Grafik lonjakan hotspot bulanan Nov 2025–Nov 2026.<br>- Visualisasi zoom-in perbandingan sebelum vs sesudah `buffer(500)` pada titik hotspot dan garis batas wilayah. |
| **4** | **Tugas 4: Choropleth & Interactivity** | - Peta choropleth nasional & Kalbar skema sequential `YlOrRd`/`OrRd`.<br>- Grid 3x3 perkembangan spasial tahun 2018 s.d. 2026.<br>- Fitur Dropdown Selector (`ipywidgets.interact`) untuk memilih tahun dan provinsi secara dinamis. |

---

## 👥 Kontributor & Lisensi
- Dikembangkan untuk keperluan praktikum & tugas perkuliahan analisis spasial Sistem Informasi Geografis (SIG).
- Dataset dan kode terbuka untuk keperluan studi akademik dan edukasi.

