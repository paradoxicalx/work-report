# 📝 Daily Work Report - Dedy Putra (2026-06-17)

---

## 📌 Informasi Issue
- **Nomor Issue**: #116
- **Judul Issue**: Manajemen Kabel Fiber Optik

## 📅 Laporan Harian - 17 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `[frontend/src/features/fiberSlice.js](file:///d:/Project/DEKASIMAL_V2/frontend/src/features/fiberSlice.js)`
  - **Deskripsi**: Menambahkan properti state baru `isAutoRouting` (tipe data boolean untuk menentukan apakah rute digambar otomatis menggunakan jalan/OSRM atau garis lurus) dan memperbarui `newCableWaypoints` agar menampung objek koordinat waypoint beserta tipe routingnya, serta mendaftarkan action `setAutoRouting`.
- `[frontend/src/app/pages/network/sites/schema/SiteCreateDrawer.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/sites/schema/SiteCreateDrawer.jsx)`
  - **Deskripsi**: Menambahkan prop `initialCoordinate` ke `SiteCreateDrawer` dan memanfaatkan hook `useEffect` untuk otomatis mengisi field formulir `coordinate` ketika drawer dibuka dengan nilai koordinat awal tersebut.
- `[frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx)`
  - **Deskripsi**: Mengintegrasikan switch untuk memilih opsi routing otomatis (OSRM/jalan) atau manual (garis lurus), menambahkan shortcut keyboard menggunakan tombol Shift untuk mengubah tipe routing secara sementara saat menggambar, mengganti select input dan input teks lama dengan terjemahan i18n murni (`t('key')` tanpa fallback string default), serta mengintegrasikan FilePond untuk antarmuka unggah berkas KML.
- `[frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx)`
  - **Deskripsi**: Mengganti modal/sidebar detail node statis dengan komponen `Transition` dan `Dialog` dari Headless UI untuk tampilan laci drawer interaktif modern yang meluncur mulus dari kanan. Ditambahkan pula tombol "Start Add Cable" untuk memulai proses pembuatan kabel baru dari node aktif tersebut yang otomatis mengisi koordinat awal di draft waypoint.
- `[frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx)`
  - **Deskripsi**: Menambahkan styling kustom untuk kursor peta (leaflet) serta tombol close popup (warna lingkaran merah solid beranimasi transisi saat hover), memperluas popup informasi Marker Node dengan detail koordinat latitude/longitude dan tombol "Start Add Cable" yang otomatis mengisi koordinat awal, serta mengintegrasikan context menu / right click popup untuk membuat node baru (`SiteCreateDrawer`) langsung di lokasi koordinat klik peta.
- `[frontend/src/i18n/locales/en/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)`
  - **Deskripsi**: Menambahkan key terjemahan bahasa Inggris untuk fitur routing kabel baru, switch auto-routing, petunjuk gambar waypoint, placeholder input nama kabel, label node awal/akhir, dan tombol interaksi pada peta.
- `[frontend/src/i18n/locales/id/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)`
  - **Deskripsi**: Menambahkan key terjemahan bahasa Indonesia untuk fitur routing kabel baru, switch auto-routing, petunjuk gambar waypoint, placeholder input nama kabel, label node awal/akhir, dan tombol interaksi pada peta.

### 📅 Rincian Commit

#### [31f821d] - save #116 (#116 - Manajemen Kabel Fiber Optik)

- **Komponen yang Berubah**:
  - `backend/src/models/fiberCable.model.js` [NEW]
  - `backend/src/models/fiberSplice.model.js` [NEW]
  - `backend/src/services/fiberCable.service.js` [NEW]
  - `backend/src/services/fiberSplice.service.js` [NEW]
  - `backend/src/services/fiberTrace.service.js` [NEW]
  - `backend/src/controllers/fiberCable.controller.js` [NEW]
  - `backend/src/controllers/fiberSplice.controller.js` [NEW]
  - `backend/src/controllers/fiberTrace.controller.js` [NEW]
  - `backend/src/routes/fiberCable.route.js` [NEW]
  - `backend/src/routes/fiberSplice.route.js` [NEW]
  - `backend/src/routes/fiberTrace.route.js` [NEW]
  - `backend/src/routes/locationPoint.route.js`
  - `backend/src/controllers/locationPoint.controller.js`
  - `backend/src/services/locationPoint.service.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `frontend/src/app/pages/network/fiberCable/index.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx` [NEW]
  - `frontend/src/features/fiberSlice.js` [NEW]
  - `frontend/src/app/router/network/fiberCableRoute.jsx` [NEW]
  - `frontend/src/app/router/network/sitesRoute.jsx`
  - `frontend/src/app/pages/network/sites/index.jsx`
  - `frontend/src/app/pages/network/sites/schema/SiteCreateDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/sites/schema/SiteEditDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/sites/schema/SiteEditBatchDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/sites/schema/SiteDetailDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/sites/create.jsx` [DELETE]
  - `frontend/src/app/pages/network/sites/edit.jsx` [DELETE]
  - `frontend/src/app/pages/network/sites/editBatch.jsx` [DELETE]
  - `frontend/src/app/pages/network/sites/detail.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Backend**: Mengimplementasikan database model Mongoose `FiberCable` dengan skema GeoJSON `LineString` dan indeks spasial `2dsphere` serta model `FiberSplice` lengkap dengan optimasi *Optimistic Locking* (`__v`) untuk menjaga integritas data core. Mengembangkan business logic services untuk CRUD kabel, routing proxy OSRM, BFS Tracing algoritma untuk tracing core fiber, serta kalkulasi redaman optik otomatis (*Optical Budget*). Seluruh response error dan notifikasi backend diintegrasikan dengan modul i18n (`req.t()`).
  - **Frontend**: Mengembangkan modul spasial GIS Fiber Optik lengkap dengan visualisasi peta interaktif Leaflet, marker clustering, serta visualisasi tray penyambungan core (`SpliceTray.jsx`) berbasis tube standar warna IEC 60304. Mengimplementasikan fitur drag-and-drop antar core dengan status loader/spinner penyambungan dan indikasi visual glow/koneksi link (🔗).
  - **Refactoring**: Melakukan refaktorisasi terhadap modul manajemen Site/POP lama. Mengubah halaman halaman terpisah (create, edit, editBatch, detail) menjadi laci drawer modern (`SiteCreateDrawer`, `SiteEditDrawer`, dll.) demi konsistensi antarmuka pengguna (UX).

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)
- **Kemampuan Pengguna/Admin**:
  - Admin dapat memetakan rute kabel fiber optik secara spasial di peta GIS secara presisi.
  - Admin dapat melakukan penyambungan (*splicing*) core antar kabel secara visual menggunakan metode drag-and-drop pada panel Splice Tray.
  - Admin dapat melacak rute core (*tracing*) dari titik awal hingga titik akhir serta melihat kalkulasi otomatis redaman optik (*Optical Budget*) yang terjadi sepanjang rute.
  - Admin dapat menggambar kabel dengan opsi routing otomatis mengikuti jalan raya (via OSRM) atau menggunakan garis lurus, serta dapat menggeser/menyeret waypoint pin di peta secara fleksibel.
  - Admin dapat mengimpor lintasan rute kabel secara instan melalui file `.kml` Google Earth.
- **Bug Fix / Solusi Masalah**:
  - Menyelesaikan masalah bentrokan data (race condition) saat beberapa admin mengedit tray yang sama secara bersamaan menggunakan optimasi *Optimistic Locking* di backend (HTTP 409 Conflict) dan menampilkan toast i18n interaktif di frontend.
  - Memperbaiki kompatibilitas drag-and-drop HTML5 di berbagai peramban pada Splice Tray, mengatasi isu kursor terhenti / dilarang.
  - Menghilangkan struktur DataTables pada respons `/fiber-cables/list` untuk memastikan data relasi entitas dapat dikonsumsi secara utuh dan optimal oleh client React.
- **Menu/Tombol Baru**:
  - Menu navigasi baru **Kabel Fiber Optik** di sidebar di bawah menu "Node/Sites".
  - Switch pilihan **Routing Otomatis (Jalan)** pada Sidebar panel gambar kabel.
  - Tombol **Mulai Tambah Kabel** pada Drawer Detail Node dan Popup marker peta.
  - Context menu klik kanan pada peta untuk langsung memicu drawer pembuatan POP/Site baru (`SiteCreateDrawer`).
  - Dropzone unggah file untuk fitur **Import KML** di sidebar.

## 📖 Informasi & Tutorial Singkat Fitur
- **Penjelasan Fitur**:
  Fitur Manajemen Kabel Fiber Optik mengintegrasikan GIS (Geographic Information System) untuk memetakan infrastruktur kabel fisik secara digital. Peta interaktif menampilkan node (POP/Site) dan link (kabel) dengan rendering cepat (clustering & bounding box query). Di sisi detail node, admin dapat membuka Splice Tray yang membagi core kabel menjadi grup Tube standar warna IEC 60304 untuk dihubungkan satu sama lain menggunakan sistem drag-and-drop. Sistem secara cerdas akan menghitung redaman optik total sepanjang rute kabel tersebut.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Kabel Fiber Optik** pada sidebar (di bawah menu "Node/Sites").
  2. **Membuat Kabel**: Klik tombol **Mulai Tambah Kabel** di panel detail salah satu POP/Site, atau pilih mode gambar kabel baru di Sidebar. Klik peta untuk menentukan titik-titik (waypoint) lintasan kabel. Isi formulir nama kabel, tipe core, warna rute, lalu klik **Simpan**.
  3. **Penyambungan Core (Splice)**: Klik pada Marker Node di peta untuk membuka **Node Info Drawer**. Pilih kabel yang ingin disambung, dan drag/tarik pin core dari satu kabel ke core kabel tujuan pada panel **Splice Tray**.
  4. **Import KML**: Klik/drop file pada area dropzone **Import KML** di sidebar kiri, pilih file `.kml` rute Anda, dan sistem akan merender jalur kabel secara otomatis pada peta.
