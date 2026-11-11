# 📝 Daily Work Report - Dedy Putra (2026-06-20)

---

## 📌 Informasi Issue

- **Nomor Issue**: #116
- **Judul Issue**: Fiber Optic Management — Manajemen Kabel Fiber Optik & Visualisasi Peta GIS

## 📅 Laporan Harian - 20 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- [`backend/src/models/fiberCable.model.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/models/fiberCable.model.js)
  - **Deskripsi**: Menambahkan field `line_weight` (Number, default: 3) pada schema `FiberCable` agar ketebalan garis kabel di peta dapat dikustomisasi per kabel.

- [`backend/src/services/fiberCable.service.js`](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberCable.service.js)
  - **Deskripsi**: Menambahkan validasi pada `updateFiberCable` — ketika `core_capacity` dikurangi, sistem akan memeriksa apakah ada splice pada core yang akan dihapus. Jika ada, akan menolak perubahan dengan pesan error `reduceCoreConflict`. Logika pengecekan keberadaan kabel dipindahkan ke awal fungsi sebelum update.

- [`backend/src/locales/en/translation.json`](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/en/translation.json)
  - **Deskripsi**: Menambahkan key i18n `fiber.error.reduceCoreConflict` untuk pesan error validasi pengurangan kapasitas core.

- [`backend/src/locales/id/translation.json`](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/id/translation.json)
  - **Deskripsi**: Menambahkan key i18n `fiber.error.reduceCoreConflict` terjemahan Bahasa Indonesia.

- [`frontend/src/i18n/locales/en/translations.json`](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)
  - **Deskripsi**: Menambahkan key `fiber.sidebar.lineWeight` = "Line Weight".

- [`frontend/src/i18n/locales/id/translations.json`](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)
  - **Deskripsi**: Menambahkan key `fiber.sidebar.lineWeight` = "Tebal Garis".

- [`frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx`](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx)
  - **Deskripsi**: Mengubah logika `activeWeight` dan `baseWeight` untuk membaca nilai `line_weight` dari data kabel (default 3). Saat kabel aktif, bobot menjadi `baseWeight + 4`. Saat hover, bobot bertambah 3 dari `activeWeight`.

- [`frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx`](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx)
  - **Deskripsi**: Menambahkan input `line_weight` (type number, min 1, max 10) pada sidebar form menggunakan komponen `InputDefault`. Nilai `line_weight` juga disertakan saat inisialisasi state, load kabel untuk edit, reset form, dan payload kabel baru/update.

- [`frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx`](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx)
  - **Deskripsi**: Menambahkan `mapsId` sebagai dependency pada `useEffect` fetch splices agar data splice diperbarui saat peta berganti.

### 📅 Rincian Commit

#### [ff262aa] - save #116 (#116 - Fiber Optic Management)

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/fiberCable.controller.js`
  - `backend/src/controllers/fiberTrace.controller.js`
  - `backend/src/services/fiberCable.service.js`
  - `backend/src/services/locationPoint.service.js`
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx`
  - `frontend/src/app/pages/network/sites/schema/SiteCreateDrawer.jsx`
  - `frontend/src/app/pages/network/sites/schema/SiteEditDrawer.jsx`
  - `frontend/src/features/fiberSlice.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Pembersihan `AGENTS.md`**: Reduksi besar-besaran dari 1196 baris agar lebih ringkas dan relevan.
  - **Backend Controller & Service**: Penyempurnaan logika pada `fiberCable.controller` (+59/- baris) dan `fiberCable.service` (+139/- baris) — termasuk optimasi query, validasi tambahan, dan perbaikan flow bisnis.
  - **Frontend FiberMap**: Refaktor signifikan (+909/- baris) pada komponen peta untuk performa rendering yang lebih baik dan integrasi fitur baru.
  - **SidebarTools**: Restrukturisasi form (+637/-) untuk mendukung mode create/edit kabel fiber dengan field tambahan.
  - **NodeInfoDrawer & SpliceTray**: Perbaikan bug dan penyempurnaan UI untuk manajemen splice.
  - **Redux Slice**: Penambahan state `mapsId` di `fiberSlice` untuk pelacakan instans peta aktif.
  - **i18n**: Penambahan 36+ key baru (EN) dan 14+ key baru (ID) untuk label dan pesan fiber management.
  - **14 file berubah, +1082/-2006 baris**.

#### [a1fea96] - save #116 (#116 - Fiber Optic Management)

- **Komponen yang Berubah**:
  - `backend/scripts/cleanFiberSpliceNullNodes.js` [DELETED]
  - `backend/scripts/dropFiberSpliceLegacyIndex.js` [DELETED]
  - `backend/scripts/migrate-fiber-cable-nodes.js` [NEW]
  - `backend/scripts/migrateSplicesToCable.js` [NEW]
  - `backend/scripts/resetSplitCables.js` [DELETED]
  - `backend/src/controllers/fiberCable.controller.js`
  - `backend/src/controllers/fiberSplice.controller.js` [DELETED]
  - `backend/src/controllers/fiberTrace.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/fiberCable.model.js`
  - `backend/src/models/fiberSplice.model.js` [DELETED]
  - `backend/src/routes/fiberCable.route.js`
  - `backend/src/routes/fiberSplice.route.js` [DELETED]
  - `backend/src/routes/fiberTrace.route.js`
  - `backend/src/services/fiberCable.service.js`
  - `backend/src/services/fiberSplice.service.js` [DELETED]
  - `backend/src/services/fiberTrace.service.js`
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx`
  - `frontend/src/components/shared/ConfirmModal.jsx`
  - `frontend/src/features/fiberSlice.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Migrasi Arsitektur Splice**: Model `FiberSplice` dihapus sepenuhnya; data splice sekarang di-embed langsung dalam dokumen `FiberCable` (field `splices` bertipe Map). Ini menyederhanakan query dan meningkatkan performa.
  - **Script Migrasi**: Dua script baru (`migrate-fiber-cable-nodes.js`, `migrateSplicesToCable.js`) untuk memindahkan data dari struktur lama ke baru. Tiga script usang dihapus.
  - **Konsolidasi Route & Service**: Route dan service `FiberSplice` dihapus; endpoint splice kini terintegrasi dalam `fiberCable.route.js` dan `fiberCable.service.js` (+424/- baris).
  - **Model FiberCable**: Penambahan field `splices` (Map), `split_from`, dan field pendukung lainnya untuk menyimpan data splice langsung di kabel.
  - **Frontend SpliceTray**: Refaktor total (+456/-) untuk bekerja dengan struktur data splice baru yang embedded di kabel.
  - **FiberMap, SidebarTools, NodeInfoDrawer**: Penyesuaian untuk integrasi dengan struktur splice dan kabel yang baru.
  - **28 file berubah, +1881/-1263 baris**.

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**:
  - Admin dapat mengatur **ketebalan garis** (line weight) setiap kabel fiber optik di peta (1–10), meningkatkan visibilitas rute utama.
  - Sistem secara otomatis **menolak pengurangan kapasitas core** jika terdapat sambungan aktif pada core yang akan dihapus — mencegah kehilangan data sambungan.
  - Data splice kini terintegrasi langsung dalam dokumen kabel, mempercepat loading dan menyederhanakan query backend.
  - Performa peta lebih responsif berkat optimasi rendering (clustering node, bounding-box loading).

- **Bug Fix / Solusi Masalah**:
  - Perbaikan dependency `mapsId` pada `NodeInfoDrawer` — sebelumnya data splice tidak diperbarui saat pengguna berpindah instans peta.
  - Perbaikan bug pada `ConfirmModal` untuk handling state yang lebih andal.
  - Validasi `core_capacity` mencegah inkonsistensi data saat admin mencoba mengurangi kapasitas kabel yang masih memiliki splice.

- **Menu/Tombol Baru**:
  - Input **"Tebal Garis" (Line Weight)** baru tersedia di sidebar form kabel fiber, memungkinkan admin mengatur ketebalan visual rute kabel langsung dari panel editing.

## 📖 Informasi & Tutorial Singkat Fitur

- **Penjelasan Fitur**: Fitur _Fiber Optic Management_ adalah modul GIS untuk memetakan, mengelola, dan memvisualisasikan infrastruktur kabel fiber optik. Fitur ini mencakup pembuatan rute kabel antar POP, manajemen core & tube dengan kode warna standar IEC 60304, pencatatan sambungan (splice) dan potongan (cut), kalkulasi redaman optik, serta visualisasi interaktif pada peta Leaflet dengan optimasi performa tinggi.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Network > Fiber Cable** pada sidebar aplikasi.
  2. Peta akan menampilkan semua kabel fiber yang sudah terdaftar dengan clustering otomatis.
  3. Untuk membuat kabel baru, klik tombol **"Draw Cable"** di sidebar tools:
     - Pilih dua node (POP) sebagai titik awal dan akhir rute.
     - Sistem akan melakukan _auto-routing_ via OSRM; rute dapat dikoreksi manual dengan drag waypoint.
     - Atur properti: Kapasitas Core, Warna Rute, Jenis Garis (solid/dashed), dan **Tebal Garis**.
     - Klik **Save** untuk menyimpan kabel.
  4. Untuk mengelola splice, klik node kabel di peta → drawer **Node Info** akan terbuka menampilkan Splice Tray.
  5. Drag & drop core dari kabel masuk ke kabel keluar untuk membuat sambungan baru.
  6. Sistem akan otomatis menghitung redaman optik berdasarkan panjang kabel dan jumlah sambungan.
