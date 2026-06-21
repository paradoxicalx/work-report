# 📝 Daily Work Report - Dedy Putra (2026-06-21)

---

## 📌 Informasi Issue
- **Nomor Issue**: #116
- **Judul Issue**: Manajemen Fiber Optic (Fiber Optic Management)

## 📅 Laporan Harian - 21 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `[backend/src/controllers/locationPoint.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/locationPoint.controller.js)`
  - **Deskripsi**: Menambahkan logic penanganan filter `types` pada endpoint pengumpulan data titik map (nodes) guna mendukung optimasi lazy loading dari UI.
- `[backend/src/services/locationPoint.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/locationPoint.service.js)`
  - **Deskripsi**: Implementasi service logic pada database untuk menyaring data marker node berdasarkan array tipe node yang diizinkan (dari query).
- `[backend/src/routes/locationPoint.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/locationPoint.route.js)`
  - **Deskripsi**: Mendaftarkan endpoint spesifik untuk mengambil list tipe node yang tersedia beserta implementasi parameter query `types`.
- `[backend/src/controllers/fiberCable.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberCable.controller.js)`
  - **Deskripsi**: Menambahkan controller endpoint untuk mengambil daftar kapasitas core yang unik di database serta menambahkan dukungan filter `cores` pada daftar fiber.
- `[backend/src/services/fiberCable.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberCable.service.js)`
  - **Deskripsi**: Implementasi service untuk mendapatkan nilai distinct dari kolom core capacity, serta update filter mongoose untuk menyaring berdasarkan core saat melisting fiber optik.
- `[backend/src/routes/fiberCable.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/fiberCable.route.js)`
  - **Deskripsi**: Registrasi endpoint GET kapasitas core kabel fiber (`core-capacities`).
- `[backend/src/locales/id/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/id/translation.json)` & `[en/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/en/translation.json)`
  - **Deskripsi**: Menambahkan i18n localization keys untuk backend (pesan kegagalan fetch daftar kabel & konfirmasi).
- `[frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx)`
  - **Deskripsi**: Menerapkan integrasi endpoint lazy loading untuk node dan kabel berdasarkan opsi tercentang di sidebar. Mengoptimalkan state render pada mode Trace agar kabel yang tidak tercentang tetapi terlibat jalur Trace tetap ditampikan sementara secara dinamis (menggunakan `displayedCables` dan array merge). Menyesuaikan Tooltip kabel dengan gaya UI multi-line yang memuat informasi core terpisah menggunakan representasi icon SVG kustom berupa penampang fiber optik.
- `[frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx)`
  - **Deskripsi**: Menambahkan komponen fungsional filter Checkbox untuk daftar Tipe Node (misal: ODC, ODP) dan Kapasitas Core (misal: 12 Core, 24 Core), di mana state ini terikat langsung ke global state (Redux).
- `[frontend/src/app/pages/network/fiberCable/index.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/index.jsx)`
  - **Deskripsi**: Melakukan dispatch pengambilan data referensi master tipe node dan kapasitas core pada saat komponen dashboard Fiber Optic pertama kali di-mount.
- `[frontend/src/features/fiberSlice.js](file:///d:/Project/DEKASIMAL_V2/frontend/src/features/fiberSlice.js)`
  - **Deskripsi**: Penambahan reducer state dan action untuk manajemen filtering (`visibleCableCores`, `availableCableCores`, `availableNodeTypes`, `visibleNodeTypes`).
- `[frontend/src/i18n/locales/id/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)` & `[en/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)`
  - **Deskripsi**: Menambah terjemahan komponen SidebarTools untuk tombol dan filter ("Filter Tipe Node", "Gabung Kabel").
- `[AGENTS.md](file:///d:/Project/DEKASIMAL_V2/AGENTS.md)`
  - **Deskripsi**: Dokumentasi konfigurasi dan regulasi standar yang mengatur sistem terupdate dengan instruksi arsitektur ekosistem DEKASIMAL V2 terbaru.

### 📅 Rincian Commit

#### [5ffe648] - save #116 (#116 - Manajemen Fiber Optic)

- **Komponen yang Berubah**:
  - `[backend/src/controllers/files.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/files.controller.js)`
  - `[backend/src/app.js](file:///d:/Project/DEKASIMAL_V2/backend/src/app.js)`
  - `[frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx)`
- **Deskripsi Perubahan & Fungsi**:
  - Melakukan *refactoring* endpoint controller untuk aksi hapus lampiran tiket. Implementasi ditarik keluar dari referensi Model secara langsung untuk didelegasikan ke arsitektur pola Model-Service-Controller standar, sehingga code logic controller menjadi lebih bersih dan mengikuti standarisasi yang ada. Melakukan sedikit perbaikan render peta.

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)
- **Kemampuan Pengguna/Admin**: Pengguna kini dapat memfilter tampilan kabel dan node pada peta secara langsung dengan mengklik checkbox tipe/kapasitas di bilah Sidebar (Lazy Loading). Hal ini sangat mengurangi beban memory browser karena membatasi dom element pada map untuk rute optik skala besar.
- **Bug Fix / Solusi Masalah**: Memperbaiki issue di mana jalur Trace terpotong (tidak merender kabel 24-core pada Trace 12-core) akibat mode isolasi checkbox. Sekarang saat dalam proses trace, rute yang ditempuh dipaksa ditampilkan walau diluar filter.
- **Menu/Tombol Baru**: Penambahan seksi Filter Tipe Node dan Filter Kapasitas Core di dalam panel kiri peta Fiber Optic. Tooltip informasi hover untuk setiap jalur serat optik dirombak menjadi rapi.

## 📖 Informasi & Tutorial Singkat Fitur
- **Penjelasan Fitur**: Fitur Lazy Filter pada peta geografis Fiber Optic akan mempercepat render dan menghemat performa. Alih-alih meload keseluruhan ratusan kabel dan titik tiang (node), peta kini memunculkan node dan line kabel berdasarkan status aktif checkbox. Terdapat juga mekanisme bypass display di saat melakukan "Trace Route".
- **Langkah Penggunaan (Tutorial)**:
  1. Buka halaman Peta Jaringan Fiber.
  2. Pada panel Sidebar di sebelah kiri, *uncheck* kotak "12 Core" dan "24 Core" untuk menyembunyikan semua kabel dari layar.
  3. Centang "12 Core" kembali, maka hanya kabel 12 core saja yang terunduh & muncul di peta.
  4. Untuk melakukan penelusuran (*trace*), klik pada salah satu node yang terhubung. Peta otomatis akan memunculkan secara paksa rute tracing walau berlanjut ke kabel "24 Core" (seperti bypass visibilitas sementara).
