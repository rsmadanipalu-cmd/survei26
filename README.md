# Survei Kepuasan Masyarakat RSUD Madani Palu

Kuesioner online berbasis GitHub Pages. Jawaban tersimpan di Google Sheets dan nilai IKM dihitung otomatis sesuai PermenPANRB No. 14 Tahun 2017.

Isi folder:

| File | Fungsi |
|---|---|
| `index.html` | Halaman kuesioner yang diisi pasien |
| `Code.gs` | Kode Google Apps Script untuk menyimpan jawaban dan merekap IKM |
| `README.md` | Panduan ini |

## Langkah 1: Siapkan Google Sheets

1. Masuk ke Google Drive dengan akun resmi rumah sakit (bukan akun pribadi), lalu buat **Google Sheet** baru, misalnya "SKM RSUD Madani 2026".
2. Klik **Ekstensi → Apps Script**. Hapus isi bawaan, lalu tempel seluruh isi `Code.gs`. Klik ikon simpan.
3. Di bagian atas, pilih fungsi **setup**, lalu klik **Jalankan**. Izinkan akses yang diminta. Setelah selesai, Sheet akan memiliki tab **Respon** dan **Rekap IKM**.
4. Klik **Deploy → Deployment baru**. Pilih jenis **Aplikasi web**, lalu atur:
   - Jalankan sebagai: **Saya**
   - Yang memiliki akses: **Siapa saja**
5. Klik **Deploy**, lalu salin **URL aplikasi web** yang berakhiran `/exec`.

Untuk mengecek, buka URL tersebut di browser. Jika muncul tulisan "Layanan survei RSUD Madani Palu aktif.", berarti sudah berhasil.

## Langkah 2: Hubungkan halaman kuesioner

Buka `index.html` dengan editor teks (Notepad pun bisa). Cari bagian `CONFIG` di dekat bawah, lalu tempel URL tadi:

```js
SCRIPT_URL: "https://script.google.com/macros/s/XXXXXXXX/exec",
```

Selama `SCRIPT_URL` masih kosong, halaman berjalan dalam **mode uji**. Semua layar bisa dicoba, tetapi jawaban tidak disimpan.

## Langkah 3: Unggah ke GitHub Pages

1. Buat akun di github.com (sebaiknya akun instansi), lalu buat repositori baru, misalnya `survei`.
2. Klik **Add file → Upload files**, lalu unggah `index.html` dan `README.md`. **Jangan unggah `Code.gs`**, karena file itu cukup ada di Apps Script.
3. Buka **Settings → Pages**. Pada *Branch*, pilih `main` dan folder `/ (root)`, lalu klik **Save**.
4. Tunggu 1–2 menit. Halaman akan aktif di `https://NAMA-AKUN.github.io/survei/`.

## Langkah 4: Buat QR code per unit

Tambahkan `?unit=` di akhir alamat agar unit layanan langsung terpilih:

| Unit | Akhiran tautan |
|---|---|
| IGD | `?unit=igd` |
| Rawat Jalan / Poliklinik Umum | `?unit=rawat-jalan` |
| Poliklinik Jiwa | `?unit=poli-jiwa` |
| Rawat Inap | `?unit=rawat-inap` |
| Farmasi / Apotek | `?unit=farmasi` |
| Laboratorium | `?unit=laboratorium` |
| Radiologi | `?unit=radiologi` |
| Rehabilitasi Medik | `?unit=rehab-medik` |
| Pendaftaran / Rekam Medis | `?unit=pendaftaran` |
| Kasir | `?unit=kasir` |

Contoh: `https://NAMA-AKUN.github.io/survei/?unit=farmasi`

Ubah setiap tautan menjadi QR code dengan pembuat QR gratis, cetak, lalu tempel di ruang tunggu masing-masing unit.

**Mode kios untuk tablet di loket.** Tambahkan `kios=1`, contohnya `?unit=igd&kios=1`. Setelah pasien selesai mengisi, halaman kembali ke awal secara otomatis dalam 15 detik.

## Membaca hasil

Buka tab **Rekap IKM** di Google Sheets. Semua angka di tab ini dihitung otomatis setiap kali ada jawaban baru:

- **NRR per unsur**, yaitu nilai rata-rata tiap unsur pada skala 1–4. Unsur dengan nilai terendah menjadi prioritas perbaikan.
- **Nilai IKM** pada skala 25–100 beserta **mutu pelayanan** (A, B, C, atau D).
- **IKM per unit layanan**, untuk membandingkan unit satu dengan yang lain.

Untuk laporan per periode (triwulan atau semester), salin tab Respon ke Sheet baru, atau saring berdasarkan kolom Waktu.

## Menyesuaikan isi

Semua pengaturan ada di bagian atas `<script>` dalam `index.html`:

- **`UNIT`** berisi daftar unit layanan. Tambah, ubah, atau hapus baris sesuai unit yang ada di RSUD.
- **`PROFIL`** berisi pertanyaan data responden.
- **`TANYA`** berisi 9 pertanyaan SKM. Kalimatnya boleh diperjelas, tetapi **urutan dan jumlahnya jangan diubah** karena rekap IKM bergantung pada urutan itu.

Untuk memakai logo resmi, unggah file logo (misalnya `logo.png`) ke repositori, lalu ganti `<svg class="tanda" ...>...</svg>` di bagian header dengan:

```html
<img src="logo.png" class="tanda" alt="Logo RSUD Madani">
```

**Penting:** setiap kali `Code.gs` diubah, buka **Deploy → Kelola deployment → ikon pensil → Versi: Versi baru → Deploy**. Jika langkah ini dilewati, perubahan tidak berlaku.

## Privasi dan keamanan

- Kuesioner ini tidak meminta nama, NIK, nomor HP, atau nomor rekam medis, sejalan dengan UU No. 27 Tahun 2022 tentang Pelindungan Data Pribadi.
- Repositori GitHub boleh publik karena hanya berisi tampilan dan tidak memuat data jawaban.
- **Jangan bagikan Google Sheet secara publik.** Bagikan hanya ke tim mutu atau humas melalui tombol Bagikan.
- Halaman sudah dilengkapi penyaring isian bot sederhana. Jika spam tetap muncul, hapus baris yang mencurigakan di tab Respon dan pertimbangkan menambahkan reCAPTCHA.

## Jika ada masalah

| Masalah | Penyebab dan solusi |
|---|---|
| Halaman GitHub menampilkan 404 | Tunggu beberapa menit setelah mengaktifkan Pages. Pastikan nama file tepat `index.html`. |
| Halaman terima kasih menyebut "Mode uji" | `SCRIPT_URL` di `index.html` belum diisi. |
| Jawaban tidak masuk ke Sheet | Pastikan akses Web App diatur ke **Siapa saja** dan URL berakhiran `/exec`, bukan `/dev`. |
| Rekap IKM kosong atau error | Jalankan ulang fungsi `setup`. Pastikan nama tab tetap **Respon**. |
| Perubahan `Code.gs` tidak berlaku | Deploy ulang dengan versi baru (lihat bagian Menyesuaikan isi). |
