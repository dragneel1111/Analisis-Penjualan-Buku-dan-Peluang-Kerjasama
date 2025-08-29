
# Analisis Penjualan Buku Purwakarta — Bundle

## Cara Pakai
1. Buka `Analisis_Penjualan_Buku_Purwakarta_FINAL.ipynb` di VS Code.
2. Cek sel `CONFIG`:
   - Set `USE_SYNTHETIC_SALES=False` jika punya data transaksi riil dan isi `LOCAL_PATH_SALES_CSV`.
   - Jika pakai open data, jalankan sel **Auto-Fetch URL CSV (CKAN)** untuk mengisi URL otomatis.
3. Jalankan sel **OSM/Overpass (Robust)** untuk peta toko buku. Output disimpan ke folder `outputs/`.
4. Jalankan bagian **Integrasi Open Data** lalu **Join & Visualisasi**.
5. Lihat file hasil di `outputs/yearly_with_context.csv` dan `outputs/monthly_with_mom_yoy.csv`.

## Output
- `outputs/peta_toko_buku_purwakarta.html` — Peta interaktif Folium
- `outputs/osm_toko_buku_purwakarta.csv` — POI OSM
- `outputs/yearly_with_context.csv` — Ringkasan tahunan + konteks
- `outputs/monthly_with_mom_yoy.csv` — Ringkasan bulanan + MoM/YoY

