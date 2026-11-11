# 📝 Daily Work Report - Dedy Putra (2026-06-25)

---

## 📌 Informasi Issue
- **Nomor Issue**: #113
- **Judul Issue**: hotspot-template-management (Manajemen Template Hotspot)

## 📅 Laporan Harian - 25 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `[backend/src/controllers/hotspotTemplate.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/hotspotTemplate.controller.js)`
  - **Deskripsi**: Menambahkan handler `countVoucherByFilter` dan `printVoucherByFilter` untuk menghitung jumlah voucher dan mengambil data voucher yang akan dicetak berdasarkan filter yang dipilih pengguna.
- `[backend/src/controllers/hotspotVoucher.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/hotspotVoucher.controller.js)`
  - **Deskripsi**: Menambahkan handler `selectHotspotVoucherGroup` dan `selectHotspotVoucherPrefix` untuk mendukung dynamic combobox/select option saat memilih kelompok voucher dan prefix voucher.
- `[backend/src/locales/en/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/en/translation.json)`
  - **Deskripsi**: Menambahkan string lokalisasi bahasa Inggris untuk modul backend template hotspot.
- `[backend/src/locales/id/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/id/translation.json)`
  - **Deskripsi**: Menambahkan string lokalisasi bahasa Indonesia untuk modul backend template hotspot.
- `[backend/src/models/hotspotTemplate.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/hotspotTemplate.model.js)`
  - **Deskripsi**: Memperbarui skema model template hotspot dengan menambahkan field `margin` dan menghapus field `countpage` guna mendukung margin dinamis dan presisi saat pencetakan.
- `[backend/src/routes/hotspotTemplate.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/hotspotTemplate.route.js)`
  - **Deskripsi**: Mendaftarkan endpoint API baru untuk menghitung dan mencetak voucher dengan pemeriksaan hak akses administrator (`hotspotTemplate.read`).
- `[backend/src/routes/hotspotVoucher.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/hotspotVoucher.route.js)`
  - **Deskripsi**: Mendaftarkan rute select option kelompok dan prefix voucher ke dalam backend routing.
- `[backend/src/services/hotspotTemplate.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/hotspotTemplate.service.js)`
  - **Deskripsi**: Mengimplementasikan logika business query database untuk menghitung voucher (`countHotspotVouchers`) dan mencari voucher (`getPrintHotspotVouchers`) berdasarkan parameter filter seperti status, profile, prefix, kelompok, dan tanggal pembuatan.
- `[backend/src/services/hotspotVoucher.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/hotspotVoucher.service.js)`
  - **Deskripsi**: Mengimplementasikan distinct query ke database untuk mendapatkan daftar kelompok voucher (`selectUniqueHotspotVoucherGroups`) dan prefix voucher (`selectUniqueHotspotVoucherPrefixes`) yang terdaftar secara unik.
- `[frontend/src/app/pages/services/hotspotTemplate/index.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/hotspotTemplate/index.jsx)`
  - **Deskripsi**: Memperbarui antarmuka pengguna untuk menambahkan komponen cetak menggunakan React Portal (`createPortal`) ke `document.body` dengan gaya print media CSS (`@media print`), visual crop marks (tanda potong voucher), margin yang dapat diatur, serta tombol dan state handler untuk print.
- `[frontend/src/app/pages/services/hotspotTemplate/schema/formSchema.js](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/hotspotTemplate/schema/formSchema.js)`
  - **Deskripsi**: Memperbarui validasi Yup schema di frontend dengan mengganti validasi `countpage` menjadi `margin`.
- `[frontend/src/i18n/locales/en/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)`
  - **Deskripsi**: Menambahkan lokalisasi teks frontend (bahasa Inggris) terkait filter dialog cetak voucher, status, opsi pencetakan, dll.
- `[frontend/src/i18n/locales/id/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)`
  - **Deskripsi**: Menambahkan lokalisasi teks frontend (bahasa Indonesia) terkait filter dialog cetak voucher, status, opsi pencetakan, dll.
- `[frontend/src/app/pages/services/hotspotTemplate/components/PrintVoucherDrawer.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/services/hotspotTemplate/components/PrintVoucherDrawer.jsx)` [NEW]
  - **Deskripsi**: Membuat komponen laci/drawer modern untuk memilih filter cetak voucher hotspot (kelompok voucher, awalan/prefix, tanggal pembuatan, profile hotspot, status voucher) serta menampilkan live-counter jumlah voucher yang cocok secara real-time.

### 📅 Rincian Commit

#### [88a5613] - save #113 (#113 - hotspot-template-management)

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/hotspotTemplate.controller.js`
  - `backend/src/models/hotspotTemplate.model.js`
  - `backend/src/routes/hotspotTemplate.route.js`
  - `backend/src/services/hotspotTemplate.service.js`
  - `frontend/package-lock.json`
  - `frontend/package.json`
  - `frontend/src/app/navigation/services.js`
  - `frontend/src/app/pages/services/hotspotTemplate/index.jsx`
  - `frontend/src/app/pages/services/hotspotTemplate/schema/formSchema.js`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/router/services/hotspotTemplateRoute.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `openspec/changes/hotspot-template/.openspec.yaml` [NEW]
  - `openspec/changes/hotspot-template/design.md` [NEW]
  - `openspec/changes/hotspot-template/proposal.md` [NEW]
  - `openspec/specs/hotspot-template-management/spec.md` [NEW]
  - `openspec/changes/hotspot-template/tasks.md` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx`
  - `frontend/src/app/pages/network/sites/schema/SiteCreateDrawer.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - Inisialisasi awal fitur Manajemen Template Hotspot (Hotspot Template Management) untuk mendukung pencetakan voucher berdesain kustom.
  - **Backend**: Membuat database schema Mongoose `HotspotTemplate`, mengimplementasikan CRUD service layer, controller dengan async error handler, mendaftarkan endpoint API `/api/v1/hotspot-template` pada backend app, dan memperbarui hak akses privilege admin.
  - **Frontend**: Mengonfigurasi modul router baru, mendaftarkan menu navigasi baru di sidebar ("Layanan" > "Template Hotspot"), menginisialisasi form schema validasi dengan Yup, membuat halaman dashboard template (`index.jsx`) terintegrasi dengan Live Preview HTML renderer dan Code Editor dasar.
  - **OpenSpec & Dokumentasi**: Membuat proposal, spesifikasi fungsional, dokumen desain arsitektur, dan pelacak tugas (tasks tracking) untuk fitur hotspot template.
  - **Lain-lain**: Penyesuaian kecil di antarmuka FiberMap, SidebarTools, dan SiteCreateDrawer untuk mendukung perubahan layout peta dan drawer data sites.

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)
- **Kemampuan Pengguna/Admin**: Admin/User sekarang memiliki kemampuan penuh untuk membuat, memperbarui, dan menghapus template desain voucher hotspot menggunakan editor HTML bawaan, mengatur ukuran dimensi voucher (lebar/tinggi canvas), margin potong (crop margin), dan ukuran QR Code. Pengguna juga dapat memfilter voucher hotspot secara terperinci (berdasarkan kelompok, awalan/prefix, profil, tanggal pembuatan, status voucher) dan langsung mencetaknya ke format PDF/kertas printer dengan tata letak yang presisi dan rapi.
- **Bug Fix / Solusi Masalah**: Mengganti implementasi cetak voucher berbasis EJS dan jQuery yang kaku dengan integrasi SPA React modern yang modular, responsif, dan dinamis, serta meningkatkan performa dan sanitasi keamanan data untuk mencegah XSS saat me-render template HTML.
- **Menu/Tombol Baru**:
  - Menu navigasi baru: **Layanan > Template Hotspot** di sidebar panel admin.
  - Tombol **"Cetak Voucher"** di dashboard Template Hotspot yang membuka drawer filter cetak voucher.
  - Tombol **"Mulai Cetak"** di dalam dialog filter untuk memulai proses print media print preview.

## 📖 Informasi & Tutorial Singkat Fitur
- **Penjelasan Fitur**: Fitur Manajemen Template Hotspot memungkinkan admin mengatur template tata letak visual voucher hotspot. Admin menulis HTML/CSS kustom yang menyisipkan tag khusus seperti `%code%` (username voucher), `%price%` (harga), `%valid%` (masa aktif), dan `%qrcode%` (link gambar QR login). Sistem secara otomatis memetakan variabel ini dengan data voucher sebenarnya saat proses cetak berjalan.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke halaman admin DEKASIMAL V2.
  2. Buka menu **Layanan** > **Template Hotspot** pada sidebar.
  3. Pilih template yang ada atau klik untuk membuat template baru.
  4. Atur dimensi voucher (Width, Height, Margin, QR Size) dan tulis kode HTML/CSS desain voucher pada editor yang tersedia. Klik **Simpan Template**.
  5. Klik tombol **Cetak Voucher** pada template terpilih.
  6. Pada drawer yang muncul di sebelah kanan, pilih filter voucher yang ingin dicetak (misal: kelompok voucher tertentu atau status 'unused').
  7. Perhatikan jumlah voucher yang ditemukan di bagian bawah drawer, lalu klik **Mulai Cetak**. Dialog print browser akan otomatis terbuka.
