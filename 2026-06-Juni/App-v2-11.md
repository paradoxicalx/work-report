# 📝 Daily Work Report - Dedy Putra (11 Juni 2026)

---

## 📌 Informasi Issue
- **Nomor Issue**: #106
- **Judul Issue**: Fitur Hotspot Voucher Massal & Detail Voucher Hotspot

## 📅 Laporan Harian - 11 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `[backend/src/models/hotspotVoucher.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/hotspotVoucher.model.js)` [NEW]
  - **Deskripsi**: Schema Mongoose untuk penyimpanan data voucher hotspot di database MongoDB, mencakup detail seperti username, password, profil, kelompok, masa aktif, harga, dnsname, lock_mac, auto_delete, last_login, expired, dan creator.
- `[backend/src/services/hotspotVoucher.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/hotspotVoucher.service.js)` [NEW]
  - **Deskripsi**: Layanan database murni untuk operasi penyimpanan (bulk), pencarian, dan pengambilan detail voucher hotspot.
- `[backend/src/services/hotspotProfile.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/hotspotProfile.service.js)` [NEW]
  - **Deskripsi**: Layanan khusus untuk menangani relasi pencarian profil hotspot berdasarkan ID numerik mikrotik profile.
- `[backend/src/controllers/hotspotVoucher.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/hotspotVoucher.controller.js)` [NEW]
  - **Deskripsi**: Kontroler yang menangani logika pembuatan voucher massal (bulk generator username/password/prefix), pemetaan data relasi profil, serta pengambilan data detail voucher individual.
- `[backend/src/routes/hotspotVoucher.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/hotspotVoucher.route.js)` [NEW]
  - **Deskripsi**: Berkas routing API untuk hotspot voucher yang mendefinisikan endpoint pembuatan, daftar tabel, opsi lokasi, serta detail voucher.
- `[frontend/src/app/pages/services/hotspotVoucher/index.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/hotspotVoucher/index.jsx)` [NEW]
  - **Deskripsi**: Halaman utama antarmuka pengguna (UI) yang menampilkan tabel data voucher hotspot, tombol penambahan, dan laci form.
- `[frontend/src/app/pages/services/hotspotVoucher/create.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/hotspotVoucher/create.jsx)` [NEW]
  - **Deskripsi**: Komponen drawer form untuk menambahkan voucher hotspot massal secara dinamis, mendukung pengisian lokasi (combobox dengan fitur tambah lokasi kustom), masa aktif (hari), kelompok voucher, profil, dan harga.
- `[frontend/src/app/pages/services/hotspotVoucher/detail.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/hotspotVoucher/detail.jsx)` [NEW]
  - **Deskripsi**: Drawer UI untuk melihat data lengkap dari voucher hotspot secara visual menggunakan badge warna-warni (StringBadge) dan status indikator lock MAC/auto-delete.
- `[frontend/src/app/pages/services/hotspotVoucher/schema/columns.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/hotspotVoucher/schema/columns.jsx)` [NEW]
  - **Deskripsi**: Definisi kolom-kolom TanStack React Table untuk daftar voucher hotspot dengan formatting sel kustom dan interaksi klik detail.
- `[frontend/src/app/pages/services/hotspotVoucher/schema/createSchema.js](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/hotspotVoucher/schema/createSchema.js)` [NEW]
  - **Deskripsi**: Skema validasi form pembuatan voucher hotspot menggunakan library Yup.
- `[frontend/src/app/router/services/hotspotVoucherRoute.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/router/services/hotspotVoucherRoute.jsx)` [NEW]
  - **Deskripsi**: File routing React Router untuk mendaftarkan halaman daftar voucher hotspot.
- `[frontend/src/i18n/locales/id/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)`
  - **Deskripsi**: Menambahkan lokalisasi Bahasa Indonesia untuk menu hotspot voucher, satuan masa aktif (hari), status voucher, prefix, detail voucher, expired, dan last login.
- `[frontend/src/i18n/locales/en/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)`
  - **Deskripsi**: Menambahkan lokalisasi Bahasa Inggris untuk modul hotspot voucher.

### 📅 Rincian Commit
*(Belum ada commit baru yang dibuat hari ini. Seluruh berkas di atas masih berstatus Work in Progress / WIP)*

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)
- **Kemampuan Pengguna/Admin**: Admin sekarang dapat membuat voucher hotspot secara massal (*bulk*) dengan prefix, kelompok voucher, dan jumlah panjang huruf yang dinamis, serta menentukan masa aktif voucher dalam satuan **Hari** dan mengunci MAC address/mengaktifkan fitur hapus otomatis. Admin juga dapat mengklik nama/kode voucher untuk membuka detail laci (*drawer*) guna melihat informasi lengkap status pemakaian voucher tersebut.
- **Bug Fix / Solusi Masalah**: Menyelesaikan masalah pencarian mikrotik profile ID numerik saat penyimpanan data voucher agar tidak menghasilkan error validasi `Cast to ObjectId failed` dengan memisahkan fungsi relasi profile ke service `hotspotProfile`.
- **Menu/Tombol Baru**: Tombol **"Tambah Voucher"** (Add Voucher) di pojok kanan atas halaman daftar voucher, serta tautan interaktif pada kolom **"Kode Voucher"** di tabel untuk membuka drawer detail data voucher.

## 📖 Informasi & Tutorial Singkat Fitur
- **Penjelasan Fitur**: Fitur Hotspot Voucher memungkinkan manajemen terpusat dalam pembuatan voucher hotspot dalam jumlah banyak sekaligus. Kode voucher dan kata sandi digenerate secara acak berdasarkan parameter masukan (prefix, jumlah voucher, panjang karakter). Data ini kemudian disinkronkan untuk kemudian dapat dicetak atau disalurkan kepada pengguna akhir.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Layanan** -> **Hotspot Voucher**.
  2. Klik tombol **"Tambah Voucher"** di pojok kanan atas.
  3. Isi data Kelompok Voucher, Lokasi (bisa memilih lokasi yang ada atau mengetikkan lokasi baru lalu klik enter), Jumlah Voucher yang akan dibuat, Panjang Huruf, Profil Mikrotik yang digunakan, Harga Voucher, dan Masa Aktif (Hari).
  4. Centang opsi "Kunci Alamat MAC" atau "Hapus Otomatis" jika diperlukan.
  5. Klik tombol **"Simpan"**. Tabel voucher hotspot akan dimuat ulang secara otomatis menampilkan voucher baru yang berhasil digenerate.
  6. Klik pada nilai di kolom **"Kode Voucher"** untuk memunculkan detail data lengkap voucher yang telah dibuat.
