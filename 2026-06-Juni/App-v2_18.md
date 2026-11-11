# 📝 Daily Work Report - Dedy Putra (2026-06-18)

---

## 📌 Informasi Issue

- **Nomor Issue**: #116
- **Judul Issue**: Implementasi Manajemen Kabel Fiber Optik (Fiber Optic Management System)

## 📅 Laporan Harian - 18 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `backend/src/config/privilege.json`
  - **Deskripsi**: Perbaikan baris baru (newline) di akhir file JSON konfigurasi privilege agar sesuai standar formatting.

- `GIS_DESIGN.md` [DELETED]
  - **Deskripsi**: Penghapusan dokumen desain GIS yang sudah tidak relevan karena implementasi fiber optic management sudah selesai dibangun.

- `GIS_PLAN.md` [DELETED]
  - **Deskripsi**: Penghapusan dokumen perencanaan GIS yang sudah tidak diperlukan karena semua perencanaan sudah terdokumentasi di openspec.

- `temp_original_fibermap.jsx` [DELETED]
  - **Deskripsi**: Penghapusan file temporary backup FiberMap yang sudah tidak diperlukan setelah refactoring final.

### 📅 Rincian Commit

#### [98fbaa2] - save #116 (Issue #116 - Implementasi Manajemen Kabel Fiber Optik)

- **Komponen yang Berubah**:
  - `backend/package-lock.json`
  - `backend/package.json`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/fiberCable.controller.js` [NEW]
  - `backend/src/controllers/fiberSplice.controller.js` [NEW]
  - `backend/src/controllers/fiberTrace.controller.js` [NEW]
  - `backend/src/controllers/locationPoint.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/fiberCable.model.js` [NEW]
  - `backend/src/models/fiberSplice.model.js` [NEW]
  - `backend/src/routes/fiberCable.route.js` [NEW]
  - `backend/src/routes/fiberSplice.route.js` [NEW]
  - `backend/src/routes/fiberTrace.route.js` [NEW]
  - `backend/src/routes/locationPoint.route.js`
  - `backend/src/services/fiberCable.service.js` [NEW]
  - `backend/src/services/fiberSplice.service.js` [NEW]
  - `backend/src/services/fiberTrace.service.js` [NEW]
  - `backend/src/services/locationPoint.service.js`
  - `frontend/jsconfig.json`
  - `frontend/package-lock.json`
  - `frontend/package.json`
  - `frontend/src/app/navigation/networks.js`
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/index.jsx` [NEW]
  - `frontend/src/app/pages/network/sites/create.jsx` [DELETED]
  - `frontend/src/app/pages/network/sites/detail.jsx`
  - `frontend/src/app/pages/network/sites/edit.jsx` [DELETED]
  - `frontend/src/app/pages/network/sites/editBatch.jsx` [DELETED]
  - `frontend/src/app/pages/network/sites/index.jsx`
  - `frontend/src/app/pages/network/sites/schema/SiteCreateDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/sites/schema/SiteDetailDrawer.jsx`
  - `frontend/src/app/pages/network/sites/schema/SiteEditBatchDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/sites/schema/SiteEditDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/sites/schema/columns.jsx`
  - `frontend/src/app/router/network/fiberCableRoute.jsx` [NEW]
  - `frontend/src/app/router/network/sitesRoute.jsx`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/MapComponent.jsx`
  - `frontend/src/components/shared/form/LocationPickerInput.jsx`
  - `frontend/src/features/fiberSlice.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/store.js`
  - `openspec/changes/CONSTITUTION.md` [NEW]
  - `openspec/changes/fiber-optic-management/.openspec.yaml` [NEW]
  - `openspec/changes/fiber-optic-management/design.md` [NEW]
  - `openspec/changes/fiber-optic-management/proposal.md` [NEW]
  - `openspec/changes/fiber-optic-management/tasks.md` [NEW]
  - `openspec/specs/fiber-optic-management/spec.md` [NEW]
  - `temp_original_fibermap.jsx` [NEW]

- **Deskripsi Perubahan & Fungsi**:
  - **Backend - Fiber Cable Management**:
    - Membuat model Mongoose `FiberCable` dengan skema GeoJSON `LineString` untuk menyimpan rute kabel fiber optik beserta metadata seperti arah, slack/kelonggaran, dan indeks geospasial `2dsphere`.
    - Membuat model Mongoose `FiberSplice` untuk menyimpan data sambungan antar core kabel dengan implementasi Optimistic Locking (menggunakan field `__v`) untuk mencegah konflik data saat diedit oleh banyak admin.
    - Mengimplementasikan service `fiberCable.service.js` dengan fungsi CRUD lengkap, dukungan OSRM Waypoint Routing untuk auto-routing rute kabel, dan parser file KML untuk import data dari Google Earth.
    - Mengimplementasikan service `fiberSplice.service.js` dengan validasi duplikasi sambungan core dan penanganan error HTTP 409 Conflict.
    - Membuat algoritma BFS (Breadth-First Search) Tracing pada `fiberTrace.service.js` untuk melakukan tracing jalur kabel dan kalkulasi Redaman Optik (Optical Budget) otomatis berdasarkan panjang kabel dan jumlah sambungan.
    - Membuat controller dan route untuk seluruh endpoint API fiber cable, fiber splice, dan fiber trace, serta mendaftarkannya pada router utama di `app.js`.
    - Menambahkan konfigurasi privilege baru dan lokalisasi i18n untuk semua pesan error dan respons API.

  - **Frontend - Fiber Optic GIS Map**:
    - Membuat Redux slice `fiberSlice.js` untuk state management modul fiber optik dan mendaftarkannya pada global store.
    - Membangun komponen peta utama `FiberMap.jsx` menggunakan Leaflet dengan fitur MarkerCluster untuk clustering node dinamis, Polyline GeoJSON untuk visualisasi rute kabel, dan rendering berbasis bounding box untuk performa optimal.
    - Mengimplementasikan auto-routing OSRM dengan waypoint dragging, simulasi routing aktif, dan koreksi rute manual menggunakan debounce.
    - Membuat antarmuka visual core `SpliceTray.jsx` dengan struktur Accordion/Tab berbasis Tube untuk mendukung kabel >= 24 core dengan standar warna internasional IEC 60304.
    - Membangun `SidebarTools.jsx` sebagai panel alat sisi kiri untuk interaksi pembuatan dan pengeditan kabel di peta.
    - Membuat `NodeInfoDrawer.jsx` untuk menampilkan informasi detail node POP dan hasil kalkulasi redaman optik.
    - Menambahkan penanganan error Optimistic Locking pada antarmuka Splice Tray dengan mapping error backend ke UI Toast menggunakan i18n.
    - Menyiapkan routing dan navigasi menu Fiber Cable di sidebar di bawah menu "Node/Sites".

  - **Frontend - Refactoring Sites (Node/Sites)**:
    - Melakukan konversi halaman dedicated (Create, Edit, EditBatch) menjadi Drawer-based form sesuai standar arsitektur terbaru.
    - Membuat `SiteCreateDrawer.jsx`, `SiteEditDrawer.jsx`, dan `SiteEditBatchDrawer.jsx` sebagai pengganti halaman create.jsx, edit.jsx, dan editBatch.jsx yang dihapus.
    - Memperbarui `SiteDetailDrawer.jsx` dan konfigurasi kolom tabel `columns.jsx` agar kompatibel dengan sistem drawer.
    - Menyesuaikan routing di `sitesRoute.jsx` untuk mendukung navigasi berbasis drawer.

  - **Agent Skills & Workflows**:
    - Menambahkan file SKILL.md untuk agent Openspec (5 skills: apply-change, archive-change, explore, propose, sync-specs).
    - Menambahkan workflow OPSX (5 workflows: apply, archive, explore, propose, sync) untuk standarisasi alur kerja AI Agent.
    - Memperbarui `.clinerules` dengan standar kode dan best practices.

  - **Dokumentasi Openspec**:
    - Membuat proposal, design, tasks, dan spec untuk fitur fiber-optic-management di direktori `openspec/`.
    - Membuat CONSTITUTION.md untuk aturan dasar proyek.

## 📢 Dampak Perubahan & Fungsionalitas Baru

- **Kemampuan Pengguna/Admin**:
  - Admin dapat membuat, mengelola, dan memvisualisasikan rute kabel fiber optik pada peta interaktif Leaflet.
  - Admin dapat melakukan auto-routing rute kabel menggunakan OSRM dengan koreksi manual melalui waypoint dragging.
  - Admin dapat mengelola core kabel (12/24/48/96 core) dengan pengelompokan Tube dan kode warna standar internasional IEC 60304.
  - Admin dapat mencatat sambungan (splice) antar core kabel secara visual dengan drag-and-drop pada Splice Tray.
  - Admin dapat melakukan tracing jalur kabel dan melihat kalkulasi redaman optik (Optical Budget) otomatis.
  - Admin dapat mengimport file KML (Google Earth) untuk mempermudah pembuatan rute kabel.
  - Admin dapat mengelola data Node/Sites sepenuhnya melalui Drawer (tanpa navigasi halaman terpisah).

- **Bug Fix / Solusi Masalah**:
  - Optimistic Locking pada Splice Tray mencegah data corrupt saat dua admin mengedit tray yang sama secara bersamaan (HTTP 409 Conflict).
  - Clustering node dinamis dan rendering berbasis bounding box mencegah pembebanan browser berlebihan pada peta dengan ribuan node.
  - Konversi Sites ke Drawer-based menyelesaikan masalah navigasi bolak-balik antar halaman yang tidak efisien.

- **Menu/Tombol Baru**:
  - Menu "Fiber Optik" baru di sidebar navigasi Networks (di bawah menu "Node/Sites").
  - Tombol "Edit Kabel" dan context menu (klik kanan) pada polyline kabel di peta GIS.
  - Panel SidebarTools untuk interaksi pembuatan dan pengeditan kabel.
  - Drawer untuk Create/Edit/EditBatch Sites yang terintegrasi dengan Datatables.

## 📖 Informasi & Tutorial Singkat Fitur

- **Penjelasan Fitur**: Modul Fiber Optic Management adalah sistem GIS (Geographic Information System) untuk mengelola inventarisasi kabel fiber optik secara visual di peta. Admin dapat membuat rute kabel baru, mengelola core kabel dengan pengelompokan Tube, mencatat sambungan antar kabel, melakukan tracing jalur, dan menghitung redaman optik secara otomatis.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu "Fiber Optik" di sidebar bagian Networks.
  2. Gunakan tombol "+" di sidebar tools untuk memulai pembuatan kabel baru.
  3. Klik dua titik node POP di peta untuk menentukan rute (auto-routing OSRM akan aktif).
  4. Seret waypoint untuk mengkoreksi rute secara manual.
  5. Isi detail kabel (nama, jumlah core, panjang slack) melalui form yang muncul.
  6. Untuk menyambungkan core, klik kabel yang sudah ada lalu buka tab "Splice Tray".
  7. Drag core dari satu kabel ke kabel lain untuk membuat sambungan.
  8. Lihat hasil kalkulasi redaman optik pada panel informasi node.
