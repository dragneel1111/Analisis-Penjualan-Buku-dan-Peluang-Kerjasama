# Analisis Penjualan Buku Gramedia Purwakarta – Kolaborasi UPI Purwakarta

> **Interactive Visualization:** https://analisis-penjualan-buku-gramedia-pu.vercel.app

Dokumentasi proyek website satu-halaman (HTML/CSS/JS murni) untuk mempresentasikan hasil **notebook analisis penjualan** Gramedia Purwakarta dan menerjemahkannya menjadi **kesimpulan otomatis** serta **rekomendasi kerja sama** dengan organisasi/lembaga di **UPI Kampus Purwakarta**.

> **Versi ini menambahkan:** (1) **Upload CSV/JSON** untuk mengisi `DATA` otomatis, (2) **bagian “Pertanyaan Bisnis UPI Purwakarta (PB UPI)”** dengan 4 visual, (3) penutup berurutan: **Kesimpulan Otomatis → Kesimpulan UPI Purwakarta (Final)**, dan (4) navigasi **disembunyikan di mobile** (menyisakan logo + judul).

---

## ✨ Fitur Utama
- **Struktur halaman:** *Latar Belakang → PB Umum (visual+insight) → PB UPI (visual+insight) → Kesimpulan Otomatis → Kesimpulan UPI Purwakarta (Final)*.
- **Upload CSV/JSON** (langsung di browser, tanpa backend):
  - Bisa unggah **satu JSON** yang berisi objek `DATA` lengkap **atau** unggah **array per-dataset**.
  - Bisa unggah **beberapa CSV** sekaligus (masing-masing mewakili dataset berbeda).
  - Tombol **“Gunakan Data Contoh”** untuk demo cepat.
- **11 visual (Chart.js)** dengan **insight otomatis**:
  - **PB Umum (7):** Kategori, Tren Bulanan, Top Judul, Penerbit, Pola Harian, Price Band, Promo vs Non-Promo.
  - **PB UPI (4):** Share Promo/Bulan (%), Affordability (≤100rb vs >100rb), Top 5 Judul untuk event kampus, Top 5 Penerbit prioritas.
- **Bagian UPI Purwakarta (Final):** ringkasan khusus dan **tabel rekomendasi** (Fokus, Mitra, Dasar Data, Program, KPI, Waktu) yang **dibangkitkan otomatis dari data**.
- **Responsif & ringkas:** grid adaptif, tinggi grafik terkendali, dan **navigasi tersembunyi di mobile**.

---

## 📂 Struktur Proyek
```

├─ index.html # Aplikasi satu berkas (HTML + CSS + JS + Chart.js/CDN)  
├─ README.md # Dokumen ini  
└─ (opsional) /assets/ # Logo/gambar tambahan bila diperlukan

````

---

## 🚀 Cara Menjalankan
1. **Buka** `index.html` di browser modern (Chrome/Edge/Firefox/Safari).
2. Di panel **Upload** (pada bagian Hero), pilih salah satu:
   - **Unggah JSON**: satu berkas berisi objek `DATA` penuh **atau** array salah satu dataset.
   - **Unggah CSV**: satu atau beberapa berkas CSV untuk dataset yang berbeda.
   - **Gunakan Data Contoh**: tombol demo untuk melihat tampilan awal.
3. Setelah upload, halaman akan **merender ulang** semua grafik, insight, kesimpulan otomatis, dan tabel UPI.

> Tidak ada build tools. Semuanya berjalan langsung di browser.

---

## 🧩 Skema Data (`DATA`)
Semua visual membaca dari konstanta global `DATA` dengan bentuk berikut (kunci bersifat **case-sensitive**):

```ts
type DataShape = {
  // PB Umum
  kategori: { nama: string; penjualan: number; }[];
  bulanan: { bulan: "YYYY-MM"; penjualan: number; }[];
  topItem: { label: string; penjualan: number; }[];
  topN?: number; // default 10
  penerbit: { nama: string; penjualan: number; }[];
  weekday: { hari: "Sen"|"Sel"|"Rab"|"Kam"|"Jum"|"Sab"|"Min"; penjualan: number; }[];
  priceBands: { band: string; penjualan: number; }[];
  bulananPromo: { bulan: "YYYY-MM"; promo: number; nonpromo: number; }[];
};
````

**Contoh JSON penuh:**

```json
{
  "kategori":[{"nama":"Fiksi","penjualan":1420},{"nama":"Nonfiksi","penjualan":1230}],
  "bulanan":[{"bulan":"2025-01","penjualan":9200},{"bulan":"2025-02","penjualan":9800}],
  "topItem":[{"label":"Judul A","penjualan":820},{"label":"Judul B","penjualan":760}],
  "topN":10,
  "penerbit":[{"nama":"GPU","penjualan":5400},{"nama":"Mizan","penjualan":3100}],
  "weekday":[{"hari":"Sen","penjualan":1500},{"hari":"Sab","penjualan":2600},{"hari":"Min","penjualan":2300}],
  "priceBands":[{"band":"50–99rb","penjualan":4100},{"band":"<50rb","penjualan":2200}],
  "bulananPromo":[{"bulan":"2025-01","promo":1800,"nonpromo":7400}]
}
```

**Contoh JSON per-dataset (array saja):**

```json
[{"nama":"Fiksi","penjualan":1420},{"nama":"Nonfiksi","penjualan":1230}]
```

_(Akan dibaca sebagai `DATA.kategori` karena ada kolom `nama` + `penjualan`.)_

---

## 📑 Format & Penamaan CSV

Setiap CSV wajib memiliki **header** berikut (case-insensitive) dan delimiter koma (`,`).

|Dataset (kunci `DATA`)|Nama berkas CSV yang dikenali|Header wajib|Contoh baris|
|---|---|---|---|
|`kategori`|`kategori.csv`, `category.csv`, `categories.csv`|`nama,penjualan`|`Fiksi,1420`|
|`bulanan`|`bulanan.csv`, `monthly.csv`|`bulan,penjualan` (YYYY-MM)|`2025-01,9200`|
|`topItem`|`topItem.csv`, `top.csv`|`label,penjualan`|`Judul A,820`|
|`penerbit`|`penerbit.csv`, `publisher.csv`, `publishers.csv`|`nama,penjualan`|`Mizan,3100`|
|`weekday`|`weekday.csv`, `harian.csv`, `hari.csv`|`hari,penjualan` (Sen..Min)|`Sab,2600`|
|`priceBands`|`priceBands.csv`, `priceBand.csv`, `band.csv`|`band,penjualan`|`50–99rb,4100`|
|`bulananPromo`|`bulananPromo.csv`, `promoBulanan.csv`, `promo.csv`|`bulan,promo,nonpromo`|`2025-01,1800,7400`|

> **Catatan:** Kolom numerik **tanpa** pemisah ribuan.  
> Untuk `priceBands.band`, label umum: `<50rb`, `50–99rb`, `100–149rb`, `150–199rb`, `≥200rb`. Parser juga mengenali variasi `<=` dan `>=`.

---

## 📊 Visualisasi & Insight yang Dihasilkan

**PB Umum (7 grafik):**

1. **Kategori** (Bar) — kontribusi kategori & kategori teratas
    
2. **Tren Bulanan** (Line/Area) — delta & puncak bulan
    
3. **Top Judul/Penulis** (Bar Horizontal, Top N) — 3 besar bestseller
    
4. **Penerbit** (Donut) — penerbit dominan & pangsa
    
5. **Pola Harian** (Bar Sen–Min) — hari tersibuk & tersantai
    
6. **Price Band** (Bar) — rentang harga terlaris
    
7. **Promo vs Non-Promo** (Grouped Bar) — kontribusi promo & puncak promo
    

**PB UPI (4 grafik):**

- **Share Promo/Bulan (%)** — waktu terbaik aktivasi kampus
    
- **Affordability (≤100rb vs >100rb)** — kecocokan daya beli mahasiswa
    
- **Top 5 Judul untuk Event Kampus** — kandidat bedah buku / author session
    
- **Top 5 Penerbit Prioritas** — mitra kemitraan kampus
    

> Semua insight kartu dirangkum di **“Kesimpulan Otomatis”**, lalu ditutup dengan **“Kesimpulan UPI Purwakarta (Final)”** + tabel rekomendasi kolaborasi.

---

## 🏫 Kesimpulan UPI Purwakarta (Final)

Menampilkan:

- **Ringkasan khusus UPI** (kategori/penerbit/hari/bulan puncak, price band favorit, share promo).
    
- **Tabel rekomendasi kolaborasi otomatis** (Fokus, Mitra, Dasar Data, Program, KPI, Waktu).
    

Logika pembentukan baris dapat disesuaikan di fungsi `generateUPIRecommendations()` pada `index.html`.

---

## 📱 Responsif & Navigasi

- Grid visual: 2 kolom (desktop), 1 kolom (mobile). Tinggi grafik dijaga agar tidak memanjang.
    
- **Mobile (≤640px):** tombol navigasi disembunyikan; hanya **logo + judul** yang tampil.
    
- **Tablet kecil (641–880px):** tombol navigasi diperkecil.
    

---

## ⚙️ Kustomisasi

- **Tema warna**: edit variabel CSS di `:root` (`--bg`, `--brand`, `--accent`, dll.).
    
- **Ambang affordability**: konstanta `AFFORD_MAX = 100_000` pada JS (menentukan batas ≤100rb).
    
- **Top N judul**: atur `DATA.topN` (default 10).
    
- Tambah PB baru: buat `<article>` + `<canvas>` + dataset di `DATA`, instansiasi Chart.js & insight.
    

---

## 🧪 Troubleshooting

- **CSV tidak terbaca** → pastikan **header** sesuai tabel di atas dan delimiter koma `,` (bukan `;`).
    
- **JSON gagal diparse** → cek pesan di status upload atau DevTools Console.
    
- **Angka tidak muncul** → pastikan kolom numerik bukan string bertanda ribuan (`1,200`). Gunakan `1200`.
    
- **Label bulan** → wajib format `YYYY-MM` (contoh: `2025-01`).
    
- **Urutan hari** → gunakan label: `Sen, Sel, Rab, Kam, Jum, Sab, Min` (script akan mengurutkan).
    
- **Price band** → gunakan label konsisten (`<50rb`, `50–99rb`, ...). Parser mencoba deteksi otomatis, format ekstrem bisa salah.
    
- **Grafik tidak update setelah upload** → pastikan file berekstensi `.csv`/`.json` dan status upload menampilkan pesan “dimuat”.
    

---

## 🚢 Deploy Cepat

- **GitHub Pages**: taruh `index.html` di branch `main`, aktifkan Pages → live.
    
- **Netlify/Vercel**: seret-lepas direktori proyek, tanpa build command.
    

---

## 👤 Atribusi

Dikembangkan untuk **Analisis Penjualan Buku Gramedia Purwakarta** dalam perspektif kolaborasi dengan **UPI Kampus Purwakarta**.  
Teknologi: HTML, CSS, JS murni, Chart.js.
