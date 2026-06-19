# 📝 Daily Work Report - Dedy Putra (2026-06-19)

---

## 📌 Informasi Issue

- **Nomor Issue**: #116
- **Judul Issue**: Refactor Fiber Splice — Pindahkan `fiber_splices` ke `fiberCable.splices`

## 📅 Laporan Harian - 19 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

> Perubahan belum di-commit (local workspace), hasil pengembangan pasca commit `b5ab6d5`.

- `[backend/src/models/fiberCable.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/fiberCable.model.js)`
  - **Deskripsi**: Menambahkan field `splices` (Map of arrays, key: nomor core, value: array koneksi) dan `slack_loops` (array dengan `node_id`) ke schema FiberCable. Field `splices` menggantikan koleksi `fiber_splices` yang terpisah.

- `[backend/src/models/fiberSplice.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/fiberSplice.model.js)` **[DELETED]**
  - **Deskripsi**: Model fiberSplice dihapus karena data sambungan sekarang melekat langsung di dokumen fiberCable.

- `[backend/src/services/fiberCable.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberCable.service.js)`
  - **Deskripsi**: Menambahkan 4 fungsi service baru untuk pengelolaan splice: `getSpliceByCable` (query via `cable_id`), `getSplicesByNode` (agregasi splice dari semua kabel di node), `saveSplice` (simpan/update splice per kabel dengan optimistic locking), `unspliceCore` (hapus satu koneksi spesifik). Memperbarui `splitCableService` agar tidak lagi mengimpor koleksi `fiber_splices` — sekarang mencari dan memperbarui splice langsung di dokumen `fiber_cables`.

- `[backend/src/services/fiberSplice.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberSplice.service.js)` **[DELETED]**
  - **Deskripsi**: Logic dipindahkan ke `fiberCable.service.js`.

- `[backend/src/services/fiberTrace.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberTrace.service.js)`
  - **Deskripsi**: Rewrite tracing dari 192 → 130 baris (-32%). Mengganti mekanisme yang sebelumnya scan koleksi `fiber_splices` (O(n×m)) menjadi O(1) lookup via `cable.splices.get(core)`. Parameter `startNodeId` dihapus — tracing sekarang hanya butuh `startCableId` + `startCoreIndex`.

- `[backend/src/controllers/fiberCable.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberCable.controller.js)`
  - **Deskripsi**: Menambahkan 4 handler splice: `getSpliceByCable`, `getSplicesByNode`, `saveSplice`, `unspliceCore`. Parameter path menggunakan `cable_id` (string custom) bukan `_id` (ObjectId MongoDB).

- `[backend/src/controllers/fiberSplice.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberSplice.controller.js)` **[DELETED]**
  - **Deskripsi**: Logic dipindahkan ke `fiberCable.controller.js`.

- `[backend/src/controllers/fiberTrace.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberTrace.controller.js)`
  - **Deskripsi**: Menyederhanakan validasi — menghapus `startNodeId`, hanya memvalidasi `startCableId` + `startCoreIndex`.

- `[backend/src/routes/fiberCable.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/fiberCable.route.js)`
  - **Deskripsi**: Menambahkan 4 endpoint splice: `GET/PUT/DELETE /:cable_id/splices`, `GET /splices-by-node/:nodeId`. Route spesifik (`splices-by-node`) diletakkan di atas route parametrik (`:cable_id/splices`) untuk menghindari konflik pencocokan Express. Memperbaiki prefix Swagger JSDoc dari `/api/...` ke `/api/v1/...`. Parameter path menggunakan `cable_id` (string).

- `[backend/src/routes/fiberSplice.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/fiberSplice.route.js)` **[DELETED]**
  - **Deskripsi**: Route dipindahkan ke `fiberCable.route.js`.

- `[backend/src/routes/fiberTrace.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/fiberTrace.route.js)`
  - **Deskripsi**: Memperbarui Swagger JSDoc — menghapus `startNodeId` dari request body schema.

- `[backend/src/app.js](file:///d:/Project/DEKASIMAL_V2/backend/src/app.js)`
  - **Deskripsi**: Menghapus import dan `app.use` untuk `FiberSpliceRoute`.

- `[backend/scripts/migrateSplicesToCable.js](file:///d:/Project/DEKASIMAL_V2/backend/scripts/migrateSplicesToCable.js)` **[NEW]**
  - **Deskripsi**: Script migrasi untuk memindahkan data dari koleksi `fiber_splices` ke field `splices` di `fiber_cables`. Membaca langsung via native MongoDB driver (tidak mengimpor model FiberSplice yang sudah dihapus). Support flag `--dry-run` (preview) dan `--drop` (hapus koleksi lama setelah migrasi).

- `[backend/scripts/cleanFiberSpliceNullNodes.js](file:///d:/Project/DEKASIMAL_V2/backend/scripts/cleanFiberSpliceNullNodes.js)` **[DELETED]**
- `[backend/scripts/resetSplitCables.js](file:///d:/Project/DEKASIMAL_V2/backend/scripts/resetSplitCables.js)` **[DELETED]**
- `[backend/scripts/dropFiberSpliceLegacyIndex.js](file:///d:/Project/DEKASIMAL_V2/backend/scripts/dropFiberSpliceLegacyIndex.js)` **[DELETED]**
  - **Deskripsi**: Script usang yang berkaitan dengan koleksi `fiber_splices` — tidak diperlukan lagi setelah arsitektur baru.

- `[frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx)`
  - **Deskripsi**: Mengubah struktur input dari array flat menjadi objek berindeks `{"0": [...], "5": [...]}`. Menambahkan logika konversi `flatSplices` untuk backward compatibility dengan fungsi pencarian existing.

- `[frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx)`
  - **Deskripsi**: Mengganti endpoint dari `/fiber-splices/node/:nodeId` ke `/fiber-cables/splices-by-node/:nodeId`. Mengubah manajemen state dari `spliceDoc` + `splices` (array) menjadi `spliceDataByNode` (objek per kabel). Matching data menggunakan `cable.cable_id` (bukan `_id`). `handleSpliceChange` dan `handleUnSplice` dikonversi untuk mengirim `cable_id` ke API.

- `[frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx)`
  - **Deskripsi**: Menghapus `startNodeId` dari payload `POST /fiber-trace`. Menyederhanakan `handleTraceCore` — tidak perlu lagi mencari source node sebelum tracing.

### 📅 Rincian Commit

#### [b5ab6d5] - save #116 (#116 - Refactor Fiber Splice)

- **Komponen yang Berubah**:
  - `backend/src/services/fiberCable.service.js`
- **Deskripsi Perubahan & Fungsi**:
  - Perbaikan lanjutan pada service `splitCableService` — mengganti referensi ke `splices` dari format lama (koleksi fiber_splices) ke format baru (field `splices` di fiberCable). Memperbaiki logika update referensi kabel setelah split.

#### [bd7414b] - save #116 (#116 - Refactor Fiber Splice)

- **Komponen yang Berubah**:
  - `backend/scripts/cleanFiberSpliceNullNodes.js` [NEW] (kemudian dihapus di WIP)
  - `backend/scripts/dropFiberSpliceLegacyIndex.js` [NEW] (kemudian dihapus di WIP)
  - `backend/scripts/resetSplitCables.js` [NEW] (kemudian dihapus di WIP)
  - `backend/src/controllers/fiberSplice.controller.js`
  - `backend/src/controllers/locationPoint.controller.js`
  - `backend/src/locales/id/translation.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/services/fiberCable.service.js`
  - `backend/src/services/fiberSplice.service.js`
  - `backend/src/services/fiberTrace.service.js`
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - Pekerjaan persiapan refactor: menambahkan i18n keys untuk splice, menyiapkan script migrasi dan cleanup, memulai modifikasi tracing dan split cable service.

#### [eb5d4fe] - save #116 (#116 - Refactor Fiber Splice)

- **Komponen yang Berubah**:
  - `.clinerules`
  - `.gitignore`
  - `backend/src/locales/id/translation.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/services/fiberCable.service.js`
  - `backend/src/services/fiberSplice.service.js`
  - `backend/src/services/fiberTrace.service.js`
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - Tahap awal refactor: memperbarui .clinerules, menambahkan i18n keys fiber splice, memodifikasi service fiberCable dan fiberSplice untuk persiapan migrasi ke arsitektur baru.

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**:
  - Admin dapat mengelola sambungan (splice) langsung per kabel melalui endpoint `PUT /fiber-cables/:cable_id/splices`
  - Admin dapat melihat semua sambungan di suatu node melalui `GET /fiber-cables/splices-by-node/:nodeId`
  - Admin dapat melepas sambungan spesifik melalui `DELETE /fiber-cables/:cable_id/splices`
  - Tracing core kabel dari FiberMap sekarang lebih cepat karena hanya 1 query per hop (sebelumnya 3 query + scan koleksi)
- **Bug Fix / Solusi Masalah**:
  - **Fix Route 404**: Route spesifik `splices-by-node` kini diletakkan di atas route parametrik `:cable_id/splices` agar Express tidak salah menangkap
  - **Fix `_id` vs `cable_id`**: Seluruh stack (route, controller, service, frontend) kini konsisten menggunakan `cable_id` (string custom) untuk identifikasi kabel, bukan `_id` (ObjectId)
  - **Fix Script Migrasi Crash**: Script tidak lagi mengimpor model `FiberSplice` yang sudah dihapus — membaca langsung dari koleksi MongoDB via native driver
  - **Fix Swagger JSDoc Prefix**: URL dokumentasi diperbaiki dari `/api/...` ke `/api/v1/...`
- **Performa Tracing**: Tracing core kabel berkurang dari 3 query DB + COLLSCAN menjadi 1 query O(1) lookup — peningkatan signifikan untuk topology fiber besar

## 📖 Informasi & Tutorial Singkat Fitur

- **Penjelasan Fitur**: Arsitektur baru menyimpan data sambungan (splice) sebagai field `splices` di dalam dokumen `fiber_cables`, bukan di koleksi terpisah. Setiap core kabel memiliki array koneksi yang mendukung 1:N (splitter). Data slack_loops juga pindah ke fiberCable dengan tambahan field `node_id` untuk tracking lokasi.
- **Langkah Penggunaan (Tutorial)**:
  1. Jalankan migrasi: `cd backend && node scripts/migrateSplicesToCable.js --dry-run` (preview) → `node scripts/migrateSplicesToCable.js` (eksekusi)
  2. Setelah migrasi sukses, hapus koleksi lama: `node scripts/migrateSplicesToCable.js --drop`
  3. Restart backend & frontend dev server
  4. Buka halaman Fiber Cable Management → klik node → drawer NodeInfo akan menampilkan splice tray per kabel
  5. Klik core kabel di CableCoreMap untuk tracing — visualisasi jalur fiber akan muncul di peta
