# Pemetaan dan Analisis Spasial Indikasi Luas Kebakaran Hutan & Lahan (Karhutla) di Indonesia (2018–2026)

Proyek ini bertujuan untuk melakukan pemrosesan data spasial, analisis statistik, serta visualisasi lokasi dan luas kebakaran hutan dan lahan (karhutla) di Indonesia menggunakan pustaka **GeoPandas**, **Mapclassify**, **Pandas**, dan **Matplotlib** di lingkungan Python.

Struktur analisis di dalam notebook [`tugas_peta_kebakaran.ipynb`](tugas_peta_kebakaran.ipynb) telah diselaraskan dengan 4 instruksi soal tugas:
1. **Dataset Titik Kebakaran Terkini**: Menggunakan titik-titik panas (*hotspot*) satelit NASA FIRMS VIIRS (NOAA-21) yang memiliki informasi `geometry` (`longitude` dan `latitude`).
2. **Penentuan Sistem CRS**: Menetapkan `EPSG:4326` (WGS 84) untuk data koordinat geografis & pemetaan utama, serta `EPSG:3857` (Pseudo-Mercator) untuk operasi jarak metrik (seperti pembuatan *buffer* 500 meter).
3. **Peta Choropleth**: Menggambarkan peta gradasi warna *sequential* (`YlOrRd` / `OrRd`) berbasis **jumlah titik hotspot (`jumlah_hotspot`)** hasil *spatial join* dengan skema kuantil (`scheme="quantiles"`, `k=5` dari `mapclassify`), disertai peta pembanding berbasis **luas kebakaran (Ha)** tahun 2018–2026.
4. **Kelengkapan Peta (Kartografi) & Auto-Save**: Dilengkapi dengan *legends* berjudul jelas, *north arrow* (arah mata angin), *scale bar* (batang skala jarak km), label sumbu, serta penyimpanan otomatis (*auto-save*) ke folder `output_maps/` dalam format **PNG resolusi tinggi (300 DPI)**.

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
│   ├── tugas3a_choropleth_jumlah_hotspot_nasional_quantiles.png
│   ├── tugas3a_choropleth_jumlah_hotspot_kalimantan_barat.png
│   ├── tugas4_peta_choropleth_nasional_provinsi_dan_kota.png
│   ├── tugas4_peta_choropleth_kalbar_dan_overlay_hotspot.png
│   ├── tugas4_grid_semua_tahun_indonesia_2018_2026.png
│   ├── tugas4_grid_semua_tahun_kalbar_2018_2026.png
│   ├── tugas3_peta_dan_tren_tahunan_kalimantan_barat.png
│   ├── tugas3_tren_bulanan_dan_komposisi_hotspot_kalbar.png
│   ├── tugas3_perbandingan_buffer_500m_titik_dan_garis.png
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

1. **Data Titik Hotspot Satelit (`.csv`)**:
   - Sumber: [NASA FIRMS (Fire Information for Resource Management System)](https://firms.modaps.eosdis.nasa.gov/) — Instrumen **VIIRS (Satelit NOAA-21 / J2V-C2)**.
   - Gabungan *Standard Processing (Archive)* (`66.212` titik, 7 Nov 2025 – 31 Jul 2026) dan *Near Real-Time (NRT)* (`594.926` titik, 1 Agu 2026 – 7 Nov 2026) menghasilkan **661.138 titik hotspot**.
   - Penyaringan `confidence == 'h'` (*High Confidence*) menghasilkan **25.177 titik** (dengan **23.143 titik** berada tepat di dalam poligon daratan 38 Provinsi Indonesia, dan **3.718 titik** di Provinsi Kalimantan Barat: `290` titik *Archive* + `3.428` titik *NRT*).
2. **Data Spasial Batas Administrasi (`.geojson`)**:
   - **38 Provinsi Indonesia**: [denyherianto/indonesia-geojson-topojson-maps-with-38-provinces](https://github.com/denyherianto/indonesia-geojson-topojson-maps-with-38-provinces).
   - **514 Kabupaten/Kota Indonesia**: [ardian28/GeoJson-Indonesia-38-Provinsi](https://github.com/ardian28/GeoJson-Indonesia-38-Provinsi).
3. **Data Luas Kebakaran Hutan dan Lahan (`.xlsx`)**:
   - `luas-karhutla-provinsi.xlsx` (38 Provinsi) & `luas-karhutla-kota.xlsx` (514 Kabupaten/Kota) tahun `2018`–`2026` (satuan Hektare/Ha): https://sipongi.gakkum.kehutanan.go.id/indikasi-luas-kebakaran

---

## 🛠️ Instalasi & Kebutuhan Lingkungan

Pastikan Anda telah menginstal **Python 3.10+** dan paket-paket pustaka berikut:

```bash
pip install geopandas mapclassify pandas matplotlib shapely openpyxl ipywidgets
```

---

## 📈 Alur Kode & Metodologi di Notebook

| Bagian | Topik Sesuai Soal Dosen | Output Visual / Analisis |
| :---: | :--- | :--- |
| **0** | **Persiapan & Fungsi Kartografi** | Fungsi `tambah_north_arrow`, `tambah_scale_bar`, `annotasi_kabupaten_kalbar`, `simpan_peta` (PNG 300 DPI), dan pembersihan data Excel. |
| **1** | **1. Dataset Titik Kebakaran** | Pembacaan GeoJSON Provinsi/Kabupaten & gabungan CSV NASA (`longitude`, `latitude` $\rightarrow$ `Point`), penyaringan `confidence == 'h'`, dan plot sebaran `25.177` titik. |
| **2** | **2. Penentuan Sistem CRS** | Penetapan `EPSG:4326` (WGS 84) dan transformasi metrik `EPSG:3857` beserta catatan distorsi luas/jarak (perbandingan dengan UTM `EPSG:32748`–`32750`). |
| **3 & 4** | **3. Peta Choropleth & 4. Kelengkapan Peta** | - **3A**: *Spatial Join* (`gpd.sjoin`, `predicate="within"`) & Peta Choropleth berbasis `jumlah_hotspot` (`cmap="YlOrRd"`, `scheme="quantiles"`, `k=5`) tingkat Nasional (38 Provinsi & 514 Kab/Kota) serta khusus Kalimantan Barat.<br>- **3B**: Peta Choropleth pembanding berbasis luas kebakaran (Ha) & Grid 3×3 seluruh tahun `2018`–`2026`. |
| **5** | **5. Analisis Tambahan** | - **5A**: Rekapitulasi tabel, grafik tren tahunan (2018–2026), & tren bulanan hotspot (Nov 2025–Nov 2026) di Kalimantan Barat.<br>- **5B**: Analisis `.buffer(500)` (500 meter) pada titik hotspot dan garis batas wilayah (`EPSG:3857`).<br>- **5C**: Fitur pemilih interaktif (`widgets.interact`) untuk indikator `"jumlah_hotspot"` maupun tahun `"2018"`–`"2026"`. |
