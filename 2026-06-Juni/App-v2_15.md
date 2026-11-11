# 📝 Daily Work Report - Dedy Putra (2026-06-15)

---

## 📌 Informasi Issue
- **Nomor Issue**: #112 & #116
- **Judul Issue**: Manajemen Alokasi IPv4 dan Pemetaan Jaringan Fiber Optik (GIS)

## 📅 Laporan Harian - 15 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `[backend/src/models/fiberNode.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/fiberNode.model.js)` [NEW]
  - **Deskripsi**: Model data untuk node jaringan fiber optik (seperti POP, OLT, ODC, ODP, JC) dengan 2dsphere index geospasial untuk longitude dan latitude.
- `[backend/src/models/fiberCable.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/fiberCable.model.js)` [NEW]
  - **Deskripsi**: Model data untuk kabel fiber optik fisik yang menghubungkan antar node menggunakan tipe data GeoJSON LineString path.
- `[backend/src/models/fiberSplice.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/fiberSplice.model.js)` [NEW]
  - **Deskripsi**: Model data untuk mencatat konfigurasi sambungan logis core-to-core di dalam Joint Closure/ODC.
- `[backend/src/services/fiberNode.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberNode.service.js)` [NEW]
  - **Deskripsi**: Layanan backend untuk CRUD node jaringan, pencarian geospasial berdasarkan bounding box, dan pemasangan/pelepasan perangkat dari inventaris gudang (WarehouseItem).
- `[backend/src/services/fiberCable.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberCable.service.js)` [NEW]
  - **Deskripsi**: Layanan backend untuk CRUD kabel fiber optik, proxy permintaan routing ke OSRM Public API, rumus kalkulasi jarak Haversine, serta penanganan putus/sambung kabel.
- `[backend/src/services/fiberSplice.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberSplice.service.js)` [NEW]
  - **Deskripsi**: Layanan backend untuk mengelola konfigurasi splicing core dengan validasi keunikan sambungan 1:1.
- `[backend/src/services/fiberTrace.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberTrace.service.js)` [NEW]
  - **Deskripsi**: Layanan backend untuk melacak jalur distribusi fiber optik dari node tertentu ke POP/OLT terdekat menggunakan algoritma pencarian BFS.
- `[backend/src/controllers/fiberNode.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberNode.controller.js)` [NEW]
  - **Deskripsi**: Controller untuk memproses request/response HTTP terkait operasi node jaringan.
- `[backend/src/controllers/fiberCable.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberCable.controller.js)` [NEW]
  - **Deskripsi**: Controller untuk memproses request/response HTTP terkait operasi lintasan kabel.
- `[backend/src/controllers/fiberSplice.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberSplice.controller.js)` [NEW]
  - **Deskripsi**: Controller untuk memproses request/response HTTP terkait konfigurasi splicing core.
- `[backend/src/controllers/fiberTrace.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberTrace.controller.js)` [NEW]
  - **Deskripsi**: Controller untuk memproses request/response HTTP terkait penelusuran optik (tracing).
- `[backend/src/routes/fiberNode.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/fiberNode.route.js)` [NEW]
  - **Deskripsi**: Definisi endpoint API untuk fiber-nodes yang dilindungi hak akses `fiberCable.*`.
- `[backend/src/routes/fiberCable.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/fiberCable.route.js)` [NEW]
  - **Deskripsi**: Definisi endpoint API untuk fiber-cables yang dilindungi hak akses `fiberCable.*`.
- `[backend/src/routes/fiberSplice.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/fiberSplice.route.js)` [NEW]
  - **Deskripsi**: Definisi endpoint API untuk fiber-splices dan fiber-trace yang dilindungi hak akses `fiberCable.*`.
- `[backend/src/app.js](file:///d:/Project/DEKASIMAL_V2/backend/src/app.js)`
  - **Deskripsi**: Mengintegrasikan router `fiberNode`, `fiberCable`, dan `fiberSplice` di bawah jalur `/api/v1`.
- `[backend/src/config/privilege.json](file:///d:/Project/DEKASIMAL_V2/backend/src/config/privilege.json)`
  - **Deskripsi**: Mendaftarkan hak akses privilege baru untuk `fiberCable` (`list`, `create`, `read`, `update`, `delete`).
- `[backend/src/locales/id/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/id/translation.json)`
  - **Deskripsi**: Menambahkan lokalisasi Bahasa Indonesia untuk label kesalahan/status pada modul `fiberCable`.
- `[backend/src/locales/en/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/en/translation.json)`
  - **Deskripsi**: Menambahkan lokalisasi Bahasa Inggris untuk label kesalahan/status pada modul `fiberCable`.
- `[frontend/src/app/navigation/networks.js](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/navigation/networks.js)`
  - **Deskripsi**: Menambahkan menu "Peta Fiber" di bawah menu "Sites" pada panel navigasi Jaringan.
- `[frontend/src/features/fiberSlice.js](file:///d:/Project/DEKASIMAL_V2/frontend/src/features/fiberSlice.js)` [NEW]
  - **Deskripsi**: Redux slice untuk mengelola state data GIS (nodes, cables, viewport, mapMode, activeModal, dll.).
- `[frontend/src/store.js](file:///d:/Project/DEKASIMAL_V2/frontend/src/store.js)`
  - **Deskripsi**: Meregistrasikan `fiberReducer` ke dalam global store Redux.
- `[frontend/src/app/router/network/fiberMapRoute.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/router/network/fiberMapRoute.jsx)` [NEW]
  - **Deskripsi**: Definisi rute lazy loading untuk halaman GIS `/networks/gis`.
- `[frontend/src/app/router/protected.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/router/protected.jsx)`
  - **Deskripsi**: Mengimpor `fiberMapRoute` dan meregistrasikannya ke dalam protected routing system.
- `[frontend/src/i18n/locales/id/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)`
  - **Deskripsi**: Menambahkan lokalisasi menu Bahasa Indonesia untuk `"gis": "Peta Fiber"`.
- `[frontend/src/i18n/locales/en/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)`
  - **Deskripsi**: Menambahkan lokalisasi menu Bahasa Inggris untuk `"gis": "Fiber Map"`.
- `[frontend/src/app/pages/gis/FiberMap.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/gis/FiberMap.jsx)` [NEW]
  - **Deskripsi**: Komponen halaman utama GIS split-pane dashboard dengan sidebar drawer informasi.
- `[frontend/src/app/pages/gis/NodeForm.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/gis/NodeForm.jsx)` [NEW]
  - **Deskripsi**: Modal form untuk menambahkan node baru berdasarkan koordinat klik peta.
- `[frontend/src/app/pages/gis/CableForm.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/gis/CableForm.jsx)` [NEW]
  - **Deskripsi**: Modal form untuk menambahkan kabel optik baru antara dua node dengan pratinjau OSRM.
- `[frontend/src/components/shared/gis/FiberMapContainer.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/components/shared/gis/FiberMapContainer.jsx)` [NEW]
  - **Deskripsi**: Komponen pembungkus peta Leaflet yang merender marker kustom SVG dan polyline warna kabel sesuai keaktifan status.

### 📅 Rincian Commit

#### [fe8e6d5] - resolve #112 (#112 - Manajemen Alokasi IPv4)

- **Komponen yang Berubah**:
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/networkIPv4.controller.js`
  - `backend/src/lib/redisClient.js`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/networkIPv4.route.js`
  - `backend/src/services/networkIPv4.service.js`
  - `backend/src/services/networkIPv4Used.service.js`
  - `backend/src/utils/ip4.js`
  - `frontend/src/app/navigation/networks.js`
  - `frontend/src/app/pages/network/ipv4Management/create.jsx` [NEW]
  - `frontend/src/app/pages/network/ipv4Management/edit.jsx` [NEW]
  - `frontend/src/app/pages/network/ipv4Management/index.jsx` [NEW]
  - `frontend/src/app/pages/network/ipv4Management/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/network/ipv4Management/schema/createSchema.js` [NEW]
  - `frontend/src/app/router/network/ipv4Management.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - Menyelesaikan implementasi penuh modul manajemen blok IPv4 dan alokasi IP statis/dinamis yang dikhususkan pada subnetting presisi.
  - Penambahan backend service untuk menghitung sisa IP tersedia, mendaftarkan IP yang sedang digunakan secara otomatis, serta membersihkan cache alokasi di Redis.
  - Implementasi CRUD di frontend UI lengkap dengan tabel dinamis TanStack Table, custom cell status badge, dan form validation memakai Yup.

---

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)
- **Kemampuan Pengguna/Admin**: 
  - Admin sekarang dapat membagi, mendistribusikan, dan memantau penggunaan blok alamat IPv4 (subnetting & IP allocation) dari dasbor.
  - Admin dengan hak akses `fiberCable` sekarang dapat membuka modul Peta Fiber, melihat persebaran node optik, menempatkan node baru dengan mengklik peta secara presisi, serta menarik lintasan kabel jalan raya otomatis yang ditenagai oleh OSRM routing.
- **Bug Fix / Solusi Masalah**: Masalah linter terkait variabel yang tidak digunakan (seperti `nextNodeDoc` dan `loading`) serta parameter `adminId` di backend telah berhasil diatasi.
- **Menu/Tombol Baru**: 
  - Menu sidebar baru "IP Manager" di bawah navigasi Jaringan.
  - Menu sidebar baru "Peta Fiber" di bawah sub-navigasi "Network -> Sites" serta tombol-tombol mode peta ("Lihat Peta", "Tambah Node", "Tarik Kabel", "Lacak").

---

## 📖 Informasi & Tutorial Singkat Fitur
- **Penjelasan Fitur**: 
  - **IPv4 Management**: Layanan untuk mendaftarkan dan memantau status alokasi alamat IP pada perangkat site.
  - **GIS Fiber Optik**: Layanan pemetaan spasial Leaflet dengan database MongoDB (2dsphere index) yang mendukung visualisasi marker node, auto-routing kabel jalan raya, manajemen splicing core, dan BFS tracing.
- **Langkah Penggunaan (Tutorial)**:
  - **IPv4**: Masuk ke **Network** -> **IP Manager**, klik **Tambah Subnet/Blok**, masukkan parameter CIDR, lalu simpan.
  - **Peta Fiber**: 
    1. Masuk ke halaman **Network** -> **Peta Fiber** dari sidebar.
    2. Untuk menambah node baru, klik tombol **Tambah Node**, lalu klik lokasi mana saja pada peta. Isi formulir lalu simpan.
    3. Untuk menarik kabel, klik tombol **Tarik Kabel**, lalu klik **Node A** diikuti dengan mengklik **Node B**. Formulir penarikan kabel beserta pratinjau OSRM akan otomatis tampil.
