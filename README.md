# Analisis Penjualan Buku Gramedia Purwakarta – Kolaborasi UPI Purwakarta

Dokumentasi proyek website satu-halaman (HTML/CSS/JS murni) untuk mempresentasikan hasil **notebook analisis penjualan** Gramedia Purwakarta dan menerjemahkannya menjadi **kesimpulan serta rekomendasi kerja sama** dengan organisasi/lembaga di **UPI Kampus Purwakarta**.

> **Penulis:** _Rafli A._

> **Web Statis Siap Deploy:** _https://analisis-penjualan-buku-gramedia-pu.vercel.app_

> **Singkatnya:** buka `index.html` di browser → ganti isi konstanta `DATA` sesuai keluaran notebook → seluruh grafik, insight, kesimpulan otomatis, dan **tabel rekomendasi kolaborasi UPI** akan ter-render.

---

## ✨ Fitur Utama
- **Struktur terarah:** *Latar Belakang → Pertanyaan Bisnis (visual+insight) → Kesimpulan Otomatis → Kesimpulan UPI Purwakarta (Final)*.
- **7 visual utama (Chart.js)** lengkap dengan **insight otomatis**:
  1) Kategori penyumbang penjualan (Bar)  
  2) Tren bulanan (Line/Area)  
  3) Top judul/penulis (Horizontal Bar)  
  4) Kontribusi penerbit (Donut)  
  5) Pola harian Sen–Min (Bar)  
  6) Rentang harga (Bar)  
  7) Promo vs Non‑Promo per bulan (Grouped Bar)
- **Bagian UPI Purwakarta:** ringkasan khusus dan **tabel rekomendasi kerja sama** (Fokus, Mitra, Dasar Data, Program, KPI, Waktu) yang **dibangkitkan otomatis dari data**.
- **Responsif & ringkas:** grid adaptif (2 kolom desktop, 1 kolom mobile), **tinggi grafik terkendali** (tidak memanjang), dan **navigasi disembunyikan di mobile** (menyisakan logo + judul).
- **Tanpa build tools:** cukup satu berkas `index.html` dengan CDN Chart.js.

---

## 📂 Struktur Proyek
```
├─ index.html        # Halaman utama (HTML + CSS + JS dalam satu file)
├─ README.md         # Dokumen ini
└─ (opsional) /assets/  # Jika ingin menambah gambar/logo, dll.
```

---

## 🚀 Cara Menjalankan
1. **Buka langsung** `index.html` di browser (Chrome/Edge/Firefox/Safari).
2. **Sunting data** di dalam `index.html` pada blok:
   ```js
   /* 💾 GANTI DATA DI SINI SESUAI OUTPUT NOTEBOOK ANDA */
   const DATA = { ... }
   ```
3. **Refresh halaman**, seluruh visual & kesimpulan akan menyesuaikan.

> Tip: Simpan satu salinan `index.html` sebagai _template_, lalu buat varian per-periode (mis. `index_2025H1.html`).

---

## 🧩 Skema Data (yang diharapkan `index.html`)
Seluruh grafik membaca dari konstanta global `DATA` dengan struktur berikut. **Gunakan nama kolom persis** agar insight otomatis bekerja.

```ts
type DataShape = {
  // PB#1
  kategori: { nama: string; penjualan: number; }[];

  // PB#2
  bulanan: { bulan: "YYYY-MM"; penjualan: number; }[];

  // PB#3
  topItem: { label: string; penjualan: number; }[];
  topN?: number; // default 10

  // PB#4
  penerbit: { nama: string; penjualan: number; }[];

  // PB#5 (urutan yang valid: Sen, Sel, Rab, Kam, Jum, Sab, Min)
  weekday: { hari: "Sen"|"Sel"|"Rab"|"Kam"|"Jum"|"Sab"|"Min"; penjualan: number; }[];

  // PB#6
  priceBands: { band: string; penjualan: number; }[];

  // PB#7
  bulananPromo: { bulan: "YYYY-MM"; promo: number; nonpromo: number; }[];
};
```

**Contoh minimal:**
```js
const DATA = {
  kategori: [{ nama:"Fiksi", penjualan:1420 }, { nama:"Nonfiksi", penjualan:1230 }],
  bulanan:  [{ bulan:"2025-01", penjualan:9200 }, { bulan:"2025-02", penjualan:9800 }],
  topItem:  [{ label:"Judul A", penjualan:820 }, { label:"Judul B", penjualan:760 }], topN: 10,
  penerbit: [{ nama:"GPU", penjualan:5400 }, { nama:"Mizan", penjualan:3100 }],
  weekday:  [{ hari:"Sen", penjualan:1500 }, { hari:"Sab", penjualan:2600 }, { hari:"Min", penjualan:2300 }],
  priceBands: [{ band:"50–99rb", penjualan:4100 }, { band:"<50rb", penjualan:2200 }],
  bulananPromo: [{ bulan:"2025-01", promo:1800, nonpromo:7400 }]
};
```

---

## 📊 Visualisasi & Insight yang Dihasilkan
| PB | Pertanyaan | Grafik | Sumber Data | Insight Otomatis (contoh) |
|----|------------|--------|-------------|----------------------------|
| 1 | Kategori penyumbang penjualan | Bar | `DATA.kategori` | Kategori X memimpin ~YY% kontribusi |
| 2 | Tren bulanan | Line/Area | `DATA.bulanan` | Tren naik/turun, delta absolut & persentase; bulan puncak |
| 3 | Top judul/penulis | Bar Horizontal (Top N) | `DATA.topItem`, `DATA.topN` | Daftar 3 besar bestseller |
| 4 | Penerbit dominan | Donut | `DATA.penerbit` | Penerbit teratas + pangsa kontribusi |
| 5 | Pola harian (Sen–Min) | Bar | `DATA.weekday` | Hari tersibuk & tersantai |
| 6 | Rentang harga terlaris | Bar | `DATA.priceBands` | Price band paling laku |
| 7 | Dampak promo | Grouped Bar | `DATA.bulananPromo` | Share promo vs total; bulan puncak promo |

> Semua insight ini mengisi elemen teks pada kartu masing‑masing dan dirangkum ulang pada **“Kesimpulan Otomatis”**.

---

## 🏫 Kesimpulan UPI Purwakarta (Final)
Bagian akhir halaman menyajikan:
- **Ringkasan khusus UPI** (dirakit dari data puncak kategori, penerbit, hari/bulan puncak, price band favorit, dan share promo).
- **Tabel rekomendasi kolaborasi** (terisi otomatis) dengan kolom:
  - **Fokus Kerja Sama** — inti program (mis. Kurasi & Display kategori unggul)  
  - **Mitra** — unit Gramedia/UPI yang relevan  
  - **Dasar Data** — justifikasi kuantitatif dari `DATA`  
  - **Bentuk Program** — aktivitas nyata (pojok bacaan, kuliah tamu, bundling, membership, dsb.)  
  - **KPI Awal** — indikator capaian (uplift penjualan, traffic, attach rate, dst.)  
  - **Waktu** — periode/ hari-bulan puncak yang disarankan

Anda dapat menambah/mengurangi baris rekomendasi dengan memodifikasi fungsi pembangkit pada JS (blok *generateUPIRecommendations()*).

---

## 📱 Responsif & Navigasi
- Grid visual otomatis menjadi 1 kolom di layar kecil; tinggi grafik dijaga agar tidak memanjang.
- **Navigasi mobile:** tombol “Latar/PB/Kesimpulan/UPI” **disembunyikan** pada ≤640px — yang tampil hanya **logo + judul**.
- Pada rentang 641–880px, tombol diperkecil agar ringkas.

> Ingin *hamburger menu*? Tambahkan toggle sederhana lalu tampilkan `.nav .row` sebagai drawer; style sudah kompatibel.

---

## 🎨 Kustomisasi Tampilan
Ubah *theme color* lewat variabel CSS di `:root`:
```css
:root{
  --bg:#0b0e14; --card:#11151d; --muted:#8a93a6; --text:#e9edf6;
  --brand:#5aa2ff; --accent:#41d3bd; --border:#1b2230; --ring:#2a3245;
}
```
> Mode terang mengikuti `prefers-color-scheme: light` (sudah disiapkan).

---

## 🧰 Dependensi
- **[Chart.js](https://www.chartjs.org/)** via CDN untuk seluruh grafik.
- **Google Fonts (Inter)** via CDN (opsional).  
Tidak ada _build step_ — cukup browser modern.

---

## 🛠️ Pemetaan Data dari Notebook
- Pastikan kolom & tipe sesuai **Skema Data** di atas.
- Jika notebook mengeluarkan CSV/JSON, Anda bisa:
  - **Copy‑paste nilai agregat** ke konstanta `DATA`, atau
  - (Opsional) tambah tombol *Upload CSV/JSON* dan lakukan parsing untuk mengisi `DATA` secara dinamis.

> Perlu versi dengan tombol **Upload CSV/JSON**? Bisa ditambahkan dengan `FileReader` + parser ringan.

---

## 🧪 Troubleshooting
- **Grafik tidak muncul** → cek konsol browser. Umumnya karena nama kolom salah (`nama` vs `name`, `bulan` bukan `"YYYY-MM"`, dll.).  
- **Angka tidak sesuai** → pastikan tipe `number` (bukan string), tidak ada koma pemisah ribuan dalam nilai numerik.  
- **Urutan hari** → gunakan label persis: `Sen, Sel, Rab, Kam, Jum, Sab, Min` (script akan mengurutkan).
- **Font/warna tidak berubah** → pastikan mengedit variabel CSS di `:root` (bukan style lain).

---

## 🚢 Deploy Cepat
- **GitHub Pages**: taruh `index.html` di branch `main`, aktifkan Pages → `/<repo>` langsung online.  
- **Netlify/Vercel**: seret‑lepas (drag‑and‑drop) direktori proyek, tidak perlu build command.

---

## 🤝 Kontribusi & Pengembangan Lanjut
- Tambah PB baru: buat `<article>` kartu, kanvas `<canvas>`, datanya di `DATA`, lalu instansiasi chart + insight.
- Ekspor gambar grafik (untuk laporan PDF) menggunakan API canvas (`toDataURL()`).
- Integrasi analitik ringan (mis. event klik) untuk evaluasi presentasi.

---

## 👤 Atribusi
Dikembangkan untuk **Analisis Penjualan Buku Gramedia Purwakarta** dalam perspektif kolaborasi dengan **UPI Kampus Purwakarta**.  
Teknologi: HTML, CSS, JS murni, Chart.js.

