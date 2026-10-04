# 📝 Daily Work Report - Dedy S.N Putra (2026-10-04)

---

## 📅 Laporan Harian - 4 Oktober 2026

> Laporan ini merangkum seluruh aktivitas pengembangan, refactoring arsitektur, dan integrasi sistem pada monorepo DEKASIMAL V2 per 4 Oktober 2026. Fokus utama hari ini berpusat pada:
> 1. **Perombakan Menyeluruh Manajemen Jaringan Fiber Optik (`issue-365`)**: Migrasi model data ke *Single Source of Truth* sambungan optik (`fiber_connections`) dengan index unik multikey atomik MongoDB, penjaminan invarian server-side ujung kabel presisi ke pusat koordinat node, pemecahan komponen raksasa monolitik frontend menjadi modul-modul terisolasi (*map*, *sidebar*, *drawer*, *hooks*), pemuatan data berbasis *viewport bounding box*, perutean waypoint cerdas, dialog visual sambungan core/port, serta penulisan rangkaian pengujian otomatis lengkap (108 test backend & 50 test frontend, 100% lulus).
> 2. **Penggabungan Modul Arsip Dokumen Administrasi Perusahaan ke Master (`issue-366`)**: Integrasi penuh (*merge*) fitur pengarsipan surat, memo, dan dokumen administrasi internal dengan penomoran romawi dinamis, pratinjau berkas PDF terintegrasi, penyimpanan MinIO Storage, dan proteksi hak akses berbasis peran granular.
> 3. **Pembaruan Berkas Changelog Rilis Sistem Monorepo**: Pemutakhiran dokumentasi riwayat rilis terpusat di `backend/src/data/changelog/` untuk versi rilis `issue-339` (Dashboard Operasional), `issue-346` (Sinkronisasi WhatsApp), `issue-348` (Validasi Alasan Pasif), `issue-358` (Backup Database Cloudflare R2), dan `issue-366` (Arsip Administrasi).

---

## 🌿 Branch: `issue-365` — Perombakan Menyeluruh Manajemen Jaringan Fiber Optik (Overhaul Fiber Optic Management)

### 📌 Informasi Issue

- **Nomor Issue**: #365
- **Judul Issue**: Perombakan Menyeluruh Manajemen Jaringan Fiber Optik (Single Source of Truth Sambungan `fiber_connections`, Geometri Invarian Presisi, Arsitektur UI Modular, Viewport Map Loader, & Otomasi Splice/Merge/Trace)
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-365`, 23 commits terverifikasi, 158 test lulus, siap untuk proses peninjauan & penggabungan ke master)

### 📅 Rincian Commit

#### [[`91379d00`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 fix(fiber): CableCard lewat fiberApi, hook sambungan tahan balapan node, reset pilihan basi - 4 Oktober 2026, 22:42:21 WIB
#### [[`7e91649d`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): sambungan atomik, dialog Sambungkan ke… dan useNodeConnections - 4 Oktober 2026, 22:38:22 WIB
#### [[`6633c7df`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 fix(fiber): pratinjau split node terpilih sesuai server, reset coresToCut, guard double submit - 4 Oktober 2026, 22:32:05 WIB
#### [[`8e2362cb`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): split/merge UI dengan pratinjau terproyeksi dan pesan server - 4 Oktober 2026, 22:28:15 WIB
#### [[`75678ffa`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 fix(fiber): jangkar ikon node di pusat dan offset kabel bertumpuk bertaper di ujung - 4 Oktober 2026, 22:24:37 WIB
#### [[`39db2a1f`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 fix(fiber): rute harus berujung di waypoint pertama/terakhir - 4 Oktober 2026, 22:21:36 WIB
#### [[`2dae43b0`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 fix(fiber): validasi rute vs waypoint, indikator hitung stabil, anchor ber-epsilon - 4 Oktober 2026, 22:18:37 WIB
#### [[`db5fcc66`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): waypoint terjangkar-node dan rute per segmen agar ujung kabel tepat di node - 4 Oktober 2026, 22:13:53 WIB
#### [[`6f2c7254`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 refactor(fiber): pecah NodeInfoDrawer ke drawer/ - 4 Oktober 2026, 22:04:09 WIB
#### [[`051c5d78`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 refactor(fiber): pecah SidebarTools ke hooks/ dan sidebar/ - 4 Oktober 2026, 22:00:27 WIB
#### [[`5d55917e`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 refactor(fiber): pecah FiberMap ke modul map/ - 4 Oktober 2026, 21:57:02 WIB
#### [[`caf5e9e0`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 fix(fiber): race MapDataLoader, form edit stabil saat pan, kunci i18n - 4 Oktober 2026, 21:54:03 WIB
#### [[`ec74b481`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 refactor(fiber): lapisan data tunggal fiberApi dan MapDataLoader per viewport - 4 Oktober 2026, 21:49:50 WIB
#### [[`6781f76b`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): controller/route singular kebab-case dengan alias lama, wrapper status, i18n - 4 Oktober 2026, 21:44:01 WIB
#### [[`e7fe6144`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 fix(fiber): 404 port, update port tanpa menimpa array, normalisasi status - 4 Oktober 2026, 13:42:26 WIB
#### [[`b41a544d`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): peralatan atomik dan adaptor bentuk lama - 4 Oktober 2026, 13:38:37 WIB
#### [[`495d6dd8`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): trace, OTDR, dan topologi berbasis sambungan tanpa N+1 dan tanpa penulisan - 4 Oktober 2026, 13:32:31 WIB
#### [[`12c46ec5`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 fix(fiber): rollback split aman setelah kabel diperkecil, guard merge/split - 4 Oktober 2026, 13:29:20 WIB
#### [[`b566ccbe`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): split/merge kabel dengan proyeksi ke garis, pewarisan atribut, dan pemindahan sambungan - 4 Oktober 2026, 13:24:42 WIB
#### [[`7cfcde1d`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): normalisasi geometri kabel, snap/repin ujung ke node, guard hapus - 4 Oktober 2026, 13:15:03 WIB
#### [[`830a43a6`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 fix(fiber): validasi loss_db/is_cut/ujung sambungan di service - 4 Oktober 2026, 13:08:40 WIB
#### [[`40e1a56c`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): koleksi fiber_connections dengan keunikan atomik per core/port - 4 Oktober 2026, 13:06:35 WIB
#### [[`3f727861`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #365 feat(fiber): util geometri murni untuk pin ujung kabel dan split/merge - 4 Oktober 2026, 13:03:24 WIB

- **Komponen yang Berubah**:
  - **Skema & Model Backend (`backend/src/models/`)**:
    - [`backend/src/models/fiberConnection.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/fiberConnection.model.js) [NEW]:
      - Model independen `fiber_connections` sebagai *Single Source of Truth* seluruh sambungan optik (core-to-core maupun core-to-port).
      - Menyimpan sepasang ujung kanonik `ends` (misal `C:<cableId>:<core>:<nodeId>` dan `P:<nodeId>:<equipmentId>:<port>`), nilai redaman sambungan `loss_db` (nilai `0` sah untuk penyambungan fusi sempurna, `< 0` ditolak), status (`CONNECTED`, `DEGRADED`, `BROKEN`), status putus (`is_cut`), dan catatan teknis.
      - Menerapkan **Multikey Unique Index** pada field `{ ends: 1 }` sehingga MongoDB secara atomik menolak sambungan ganda pada core atau port yang sama (mencegah *race condition* dan *double-splice*).
    - [`backend/src/models/fiberCable.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/fiberCable.model.js):
      - Menghapus field `splices` (Map objek lama yang rawan bentrok dan *last-write-wins*).
      - Menambahkan indeks performa pada field `nodes` untuk mempercepat query relasi kabel yang melalui suatu node.
    - [`backend/src/models/locationPoint.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/locationPoint.model.js):
      - Merestrukturisasi sub-skema port peralatan fiber: menghapus field redundan `connected_to_*`; status port disederhanakan menjadi `AVAILABLE` dan `BROKEN`, sedangkan penentuan status `CONNECTED` dihitung secara komputasi dinamis dari `fiber_connections`.
    - **Pembersihan Skrip Usang (`backend/scripts/`)**:
      - Menghapus berkas migrasi model lama yang tidak lagi dipakai: [`backend/scripts/migrate-fiber-cable-nodes.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/scripts/migrate-fiber-cable-nodes.js), [`backend/scripts/migrate-splices-node.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/scripts/migrate-splices-node.js), dan [`backend/scripts/migrateSplicesToCable.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/scripts/migrateSplicesToCable.js).
  - **Utilitas Geometri Murni & Penanganan Error (`backend/src/utils/`)**:
    - [`backend/src/utils/fiberGeometry.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/fiberGeometry.js) [NEW]:
      - Kumpulan fungsi murni tanpa efek samping berbasis Turf.js geodesik WGS84:
        - `pinEndpoints(coords, startCoord, endCoord, options)`: Memastikan titik koordinat awal `path[0]` dan akhir `path[last]` sama persis (`===`) dengan koordinat node.
        - `repinPathForMovedNode(coords, oldCoord, newCoord)`: Memindahkan ujung kabel saat node digeser tanpa merusak kelengkungan rute tengah.
        - `projectPointOnLine(lineCoords, pt)`: Memproyeksikan klik titik potong secara ortogonal ke segmen kabel terdekat.
        - `splitLineAtPoint(coords, splitPt)`: Memotong LineString kabel menjadi dua jalur tanpa *gap* atau distorsi koordinat.
        - `mergeLines(coordsA, coordsB, commonNodeCoord)`: Menggabungkan dua kabel pada titik node bersama dengan pembalikan orientasi arah yang benar jika diperlukan.
        - `calculateLengthMeters(coords)`: Menghitung panjang rute kabel akurat dalam satuan meter.
    - [`backend/src/utils/fiberError.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/fiberError.js) [NEW]:
      - Definisi kelas `FiberError(statusCode, messageKey)` dan pembantu pemetaan error geometri ke HTTP status code yang tepat (400, 404, 409, 422).
  - **Lapisan Layanan Bisnis Backend (`backend/src/services/`)**:
    - [`backend/src/services/fiberConnection.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/fiberConnection.service.js) [NEW]:
      - Menangani operasi atomik penyambungan (`connect`), pemutusan (`disconnect`), pembaruan redaman/status (`updateConnection`), daftar sambungan per node (`listConnectionsByNode`), daftar sambungan per kabel (`listConnectionsByCable`), dan pembersihan sambungan saat node dihapus (`deleteConnectionsByNode`).
    - [`backend/src/services/fiberCable.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/fiberCable.service.js):
      - Ditulis ulang untuk menjamin invarian geometri server-side, snap otomatis ujung kabel ke titik node, repin ujung kabel ketika lokasi node diperbarui, serta proteksi penghapusan kabel jika masih memiliki sambungan optik aktif.
      - Fitur Split Kabel (`splitCableService`): memproyeksikan koordinat potong, membuat kabel baru yang mewarisi atribut kabel induk (tipe kabel, partner, jumlah core per tube, slack loops, catatan core), dan memindahkan sambungan yang berada di segmen kedua ke ID kabel baru.
      - Fitur Merge Kabel (`mergeCablesService`): menggabungkan dua kabel yang bertemu pada satu node perantara, menyambungkan geometri, memindahkan referensi sambungan, dan menghapus kabel sekunder secara aman.
    - [`backend/src/services/fiberEquipment.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/fiberEquipment.service.js) [NEW]:
      - Operasi posisional atomik Mongoose (`$push`, `$pull`, `$set`) untuk menambah, mengubah, dan menghapus peralatan optik (Splitter, ODP, ODC, Joint Closure) pada suatu node tanpa menimpa konfigurasi peralatan lain.
    - [`backend/src/services/fiberLegacyAdapter.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/fiberLegacyAdapter.js) [NEW]:
      - Mempertahankan kontrak API Partner (`/p-api/v1/map/*`) dan laporan node: mentransformasikan dokumen `fiber_connections` secara dinamis ke bentuk lama (`splices` dan `connected_to_*`) tanpa perlu menyimpan data redundan di database.
    - [`backend/src/services/fiberTrace.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/fiberTrace.service.js):
      - Ditulis ulang menjadi fungsi *read-only* murni (tidak menulis log/trace ke DB).
      - Menghilangkan *query waterfall* N+1 dengan preloading seluruh sambungan dan kabel terkait.
      - Mendukung nilai redaman `loss_db: 0` tanpa fallback keliru ke `0.1`.
    - [`backend/src/services/locationPoint.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/locationPoint.service.js):
      - Mengaitkan repin ujung kabel otomatis saat koordinat node dipindah (`repinCablesForNode`), menolak pembaruan langsung `fiber_equipments` dari endpoint generik node, serta membersihkan sambungan terkait saat node dihapus.
  - **Kontroler & Routing Backend (`backend/src/`)**:
    - [`backend/src/controllers/fiber.handler.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/fiber.handler.js) [NEW]:
      - Wrapper seragam penanganan error controller: meneruskan status code dari `FiberError` ke `res.status()` sebelum melempar error ke middleware global.
    - [`backend/src/controllers/fiberCable.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/fiberCable.controller.js), [`fiberConnection.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/fiberConnection.controller.js) [NEW], [`fiberEquipment.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/fiberEquipment.controller.js) [NEW], dan [`fiberTrace.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/fiberTrace.controller.js):
      - Controller terstandarisasi dengan `asyncHandler`, validasi parameter, sanitasi payload, dan i18n response.
    - [`backend/src/routes/fiberCable.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/fiberCable.route.js), [`fiberConnection.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/fiberConnection.route.js) [NEW], [`fiberEquipment.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/fiberEquipment.route.js) [NEW], dan [`fiberTrace.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/fiberTrace.route.js):
      - Rute RESTful singular kebab-case terstandarisasi (`/api/v1/fiber-cable/*`, `/api/v1/fiber-connection/*`, `/api/v1/fiber-equipment/*`, `/api/v1/fiber-trace/*`) yang tetap menerima alias rute lama untuk backward-compatibility.
    - [`backend/src/app.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/app.js):
      - Pendaftaran router-router fiber baru ke pipeline Express.
  - **Modul Visual & Modularisasi Komponen Frontend (`frontend/src/`)**:
    - **Layanan API Frontend**:
      - [`frontend/src/app/pages/network/fiberCable/services/fiberApi.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/services/fiberApi.js) [NEW]: Satu-satunya sentralisasi pemanggilan HTTP Axios untuk modul fiber kabel, sambungan, peralatan, dan tracing.
    - **Pemecahan Modul Peta (`app/pages/network/fiberCable/map/`)**:
      - [`MapDataLoader.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/map/MapDataLoader.jsx) [NEW]: Komponen pemuat data spasial berbasis *bounding box viewport* Leaflet saat pergeseran (*pan*) atau pembesaran (*zoom*) peta dengan debounce, bereaksi otomatis pada increment `dataVersion`.
      - [`CablePolyline.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/map/CablePolyline.jsx) [NEW]: Rendering garis polyline kabel dengan offset lateral bertaper (`fiberRender.js`) agar kabel bertumpuk tidak saling menutupi di tengah jalur namun tetap menyatu tepat di pusat ikon node.
      - [`DrawingLayer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/map/DrawingLayer.jsx) [NEW]: Lapisan interaktif penggambaran rute kabel, snap otomatis ke node, penambahan titik waypoint, dan indikator perhitungan OSRM yang stabil.
      - [`CableCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/map/CableCard.jsx) [NEW], [`FloatingInfoCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/map/FloatingInfoCard.jsx) [NEW], [`CableCoreMap.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/map/CableCoreMap.jsx) [NEW], [`SplitPreview.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/map/SplitPreview.jsx) [NEW], [`MapEffects.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/map/MapEffects.jsx) [NEW], dan [`mapIcons.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/map/mapIcons.js) [NEW].
    - **Pemecahan Modul Sidebar (`app/pages/network/fiberCable/sidebar/`)**:
      - [`CableDraftForm.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/sidebar/CableDraftForm.jsx) [NEW], [`CablesPanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/sidebar/CablesPanel.jsx) [NEW], [`SplitNodePanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/sidebar/SplitNodePanel.jsx) [NEW], [`MergeSection.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/sidebar/MergeSection.jsx) [NEW], [`TracingPanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/sidebar/TracingPanel.jsx) [NEW].
      - Custom Hooks: [`useCableForm.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/hooks/useCableForm.js) [NEW], [`useSplitMerge.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/hooks/useSplitMerge.js) [NEW], [`useOtdr.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/hooks/useOtdr.js) [NEW], dan [`useNodeConnections.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/hooks/useNodeConnections.js) [NEW].
    - **Pemecahan Modul Drawer Node (`app/pages/network/fiberCable/drawer/`)**:
      - [`DrawerTabs.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/drawer/DrawerTabs.jsx) [NEW], [`NodeInfoTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/drawer/NodeInfoTab.jsx) [NEW], [`NodeEquipmentTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/drawer/NodeEquipmentTab.jsx) [NEW], [`NodeConnectionTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/drawer/NodeConnectionTab.jsx) [NEW], [`NodeDrawerFooter.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/drawer/NodeDrawerFooter.jsx) [NEW], dan [`PendingSpliceBanner.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/drawer/PendingSpliceBanner.jsx) [NEW].
    - **Dialog Sambungan Baru**:
      - [`ConnectDialog.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/fiberCable/components/ConnectDialog.jsx) [NEW]: Dialog interaktif modal untuk memilih target sambungan core-to-core atau core-to-port dengan deteksi port/core kosong, input redaman dB, dan umpan balik kesalahan server. Menggantikan dialog lama `DropCoreModal.jsx` yang telah dihapus.
    - **Utilitas Spasial & Algoritma Murni Frontend**:
      - [`frontend/src/utils/fiberDraw.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/fiberDraw.js) [NEW]: Algoritma snapping waypoint dan perhitungan rute OSRM per segmen.
      - [`frontend/src/utils/fiberRender.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/fiberRender.js) [NEW]: Algoritma tapered offset kabel optik paralel.
      - [`frontend/src/utils/fiberFree.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/fiberFree.js) [NEW]: Utilitas identifikasi core dan port peralatan yang berstatus bebas (belum tersambung).
    - **Redux Slice & Internasionalisasi**:
      - [`frontend/src/features/fiberSlice.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/features/fiberSlice.js): Refactoring state management Redux agar ramping, hanya menangani state antarmuka UI dan increment counter `dataVersion`.
      - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json): Penambahan 60+ kunci terjemahan bilingual untuk seluruh pesan sistem fiber.
  - **Rangkaian Pengujian Otomatis (Test Suite)**:
    - **108 Test Backend Lolos (Vitest + MongoDB In-Memory)**:
      - [`backend/test/unit/fiberGeometry.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/fiberGeometry.test.js) [NEW] (16 test)
      - [`backend/test/integration/fiberCable.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/fiberCable.service.test.js) [NEW] (13 test)
      - [`backend/test/integration/fiberConnection.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/fiberConnection.service.test.js) [NEW] (13 test)
      - [`backend/test/integration/fiberSplitMerge.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/fiberSplitMerge.test.js) [NEW] (21 test)
      - [`backend/test/integration/fiberTrace.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/fiberTrace.service.test.js) [NEW] (11 test)
      - [`backend/test/integration/fiberLegacyAdapter.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/fiberLegacyAdapter.test.js) [NEW] (6 test)
      - [`backend/test/integration/fiberEquipment.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/fiberEquipment.service.test.js) [NEW] (9 test)
      - [`backend/test/integration/fiberController.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/fiberController.test.js) [NEW] (19 test)
      - [`backend/test/helpers/fiberFactories.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/helpers/fiberFactories.js) [NEW]: Pabrik pembuatan data tiruan Node, Kabel, dan Peralatan untuk test.
    - **50 Test Frontend Lolos (Vitest)**:
      - [`frontend/src/utils/fiberDraw.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/fiberDraw.test.js) [NEW] (29 test)
      - [`frontend/src/utils/fiberRender.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/fiberRender.test.js) [NEW] (7 test)
      - [`frontend/src/utils/fiberFree.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/fiberFree.test.js) [NEW] (14 test)
- **Deskripsi Perubahan & Fungsi**:
  - Menyelesaikan seluruh masalah ketidakstabilan data splice fiber optik monorepo dengan menjamin satu sumber kebenaran pada level database.
  - Memastikan ujung kabel selalu melekat secara sempurna pada koordinat node (tidak ada lagi kabel mengambang atau melenceng saat peta digeser).
  - Meningkatkan performa aplikasi secara drastis melalui pemuatan data per *viewport* (tidak lagi memuat ribuan data kabel di seluruh dunia secara bersamaan).
  - Memberikan antarmuka penyambungan core yang intuitif, visualisasi rute kabel paralel yang rapi, dan fitur split/merge kabel yang aman dari kehilangan data sambungan.

---

## 🌿 Branch: `master` / `issue-366` — Modul Pengelolaan Arsip Dokumen Administrasi Perusahaan (Merge Release)

### 📌 Informasi Issue

- **Nomor Issue**: #366
- **Judul Issue**: Pengelolaan Arsip Dokumen Administrasi Perusahaan (Penomoran Romawi Otomatis, Pratinjau PDF, Penyimpanan MinIO, & Proteksi Akses Granular)
- **Status Branch**: `Sudah di-merge` (Digabungkan ke branch `master` melalui merge commit `b5697b85` pada 4 Oktober 2026, 15:18:56 WIB)

### 📅 Rincian Commit

#### [[`b5697b85`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #366 - 4 Oktober 2026, 15:18:56 WIB

- **Komponen yang Berubah**:
  - **Backend**:
    - [`backend/src/models/administrationDocument.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/administrationDocument.model.js): Skema `AdministrationDocument` lengkap dengan penomoran urut otomatis tahunan berformat romawi `{urutan}/{kode}/{bulanRomawi}/{tahun}`.
    - [`backend/src/controllers/administrationDocument.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/administrationDocument.controller.js) & [`backend/src/services/administrationDocument.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/administrationDocument.service.js): Layanan bisnis CRUD dan kontroler terproteksi privilege `administrationDocument`.
    - [`backend/src/routes/administrationDocument.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/administrationDocument.route.js) & [`backend/src/routes/files.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/files.route.js): Pendaftaran rute API RESTful dan endpoint stream berkas dari MinIO.
    - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) & [`privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json): Pendaftaran grup hak akses baru `administrationDocument` (`read`, `create`, `update`, `delete`).
    - [`backend/test/integration/administrationDocument.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/administrationDocument.test.js): Pengujian integrasi Vitest lengkap (349 baris) memvalidasi CRUD dan auto-increment nomor dokumen.
  - **Frontend**:
    - [`frontend/src/app/pages/archive/administration/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/administration/index.jsx): Halaman tabel TanStack Table Arsip Administrasi terintegrasi filter kategori dan pencarian.
    - [`frontend/src/app/pages/archive/administration/AdministrationDocumentDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/administration/AdministrationDocumentDrawer.jsx): Drawer formulir dokumen dengan drag-and-drop unggah PDF.
    - [`frontend/src/app/pages/archive/administration/AdministrationDocumentPreviewModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/administration/AdministrationDocumentPreviewModal.jsx): Modal pratinjau dokumen PDF langsung di dalam browser.
    - [`frontend/src/utils/romanNumeral.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/romanNumeral.js): Utilitas konversi bulan kalender ke angka Romawi.
    - [`frontend/src/app/navigation/archive.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/archive.js) & [`frontend/src/app/router/protected.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/protected.jsx): Navigasi dan rute proteksi menu Arsip Administrasi (`/archive/administration`).
- **Deskripsi Perubahan & Fungsi**:
  - Memfasilitasi digitalisasi dan tata kelola arsip surat resmi, memo dinas, serta berita acara operasional perusahaan ke dalam sistem monorepo terpadu secara aman dan terorganisir.

---

## 🌿 Branch: `master` / `production` — Pembaruan Berkas Changelog Rilis Sistem Monorepo

### 📌 Informasi Issue

- **Nomor Issue**: N/A (Release Maintenance & Documentation)
- **Judul Issue**: Pembaruan Berkas Changelog Rilis Sistem Monorepo DEKASIMAL V2
- **Status Branch**: `Sudah di-merge` (Commit langsung ke `master` & `production` melalui commit `b253888e` pada 4 Oktober 2026, 21:36:47 WIB)

### 📅 Rincian Commit

#### [[`b253888e`](file:///home/dhedhy/Project/Dekasimal-V2)] - update changelog - 4 Oktober 2026, 21:36:47 WIB

- **Komponen yang Berubah**:
  - [`backend/src/data/changelog/index.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/index.json): Menambahkan entri index riwayat rilis untuk 5 modul fitur terbaru.
  - [`backend/src/data/changelog/releases/issue-339.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-339.json) [NEW]: Dokumentasi rilis modul Dashboard Operasional Tiket & Analitik Performa Jaringan.
  - [`backend/src/data/changelog/releases/issue-346.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-346.json) [NEW]: Dokumentasi rilis modul Sinkronisasi Kontak & Pesan Masuk WhatsApp.
  - [`backend/src/data/changelog/releases/issue-348.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-348.json) [NEW]: Dokumentasi rilis dialog validasi alasan penonaktifan pelanggan pasif (*Pasif Reason Modal*).
  - [`backend/src/data/changelog/releases/issue-358.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-358.json) [NEW]: Dokumentasi rilis pencadangan Cloudflare R2, retensi dinamis, notifikasi Telegram, dan pembersihan berkas yatim.
  - [`backend/src/data/changelog/releases/issue-366.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-366.json) [NEW]: Dokumentasi rilis modul Arsip Dokumen Administrasi Perusahaan.
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan modal informasi *What's New / Changelog* pada antarmuka pengguna DEKASIMAL V2 selalu menampilkan catatan rilis terkini secara akurat kepada seluruh administrator.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #365  | Perombakan Manajemen Jaringan Fiber Optik | Menghilangkan bug data sambungan ganda melalui *Single Source of Truth* `fiber_connections`, ujung kabel 100% presisi di pusat koordinat node, performa peta cepat dengan *viewport loader*, pemecahan komponen modular, dan 158 test otomatis. |
| #366  | Pengelolaan Arsip Dokumen Administrasi Perusahaan | Pengarsipan terpusat dokumen resmi/memo/surat tugas perusahaan dengan penomoran romawi dinamis, pratinjau PDF langsung, dan kontrol hak akses peran. |
| N/A   | Pembaruan Changelog Rilis Sistem | Menyelaraskan catatan rilis 5 modul fitur terbaru pada modal changelog aplikasi. |

### Kemampuan Baru Pengguna/Admin

- **Penyambungan Core & Port Presisi**: Admin jaringan kini dapat menyambungkan core kabel ke core kabel lain atau ke port peralatan (Splitter, ODP, ODC, Joint Closure) melalui dialog `ConnectDialog` yang secara otomatis memfilter port/core yang sedang bebas, mencegah penyambungan ganda (*double-splice*).
- **Invarian Jalur Kabel Rapi di Peta**: Ujung kabel optik kini selalu menempel persis di titik tengah ikon node POP/ODP/ODC. Saat node dipindahkan, kabel yang terhubung otomatis bergeser tanpa merusak bentuk rute belokan di bagian tengah.
- **Visualisasi Kabel Bertumpuk (Tapered Offset)**: Pada jalur di mana beberapa kabel optik membentang di rute jalan yang sama, garis kabel otomatis diberi jarak lateral di bagian tengah agar mudah dibedakan, namun tetap mengumpul secara presisi ke titik node di kedua ujungnya.
- **Operasi Potong & Gabung Kabel (Split & Merge)**: Admin dapat memotong kabel di titik klik mana pun di sepanjang rute untuk menyisipkan node baru dengan proyeksi ortogonal otomatis; seluruh sambungan yang ada di segmen lanjutan dipindahkan secara otomatis tanpa merusak data topologi.
- **Pemuatan Peta Cepat (Viewport Bounding Box)**: Peta jaringan fiber tidak lagi mengalami freeze atau *lag* karena data node dan kabel hanya dimuat sesuai area yang sedang dilihat pengguna di layar (*viewport*).

### Bug Fix / Solusi Masalah

- **Eliminasi Masalah Inkonsistensi Sambungan (*Multi-source of Truth*)**: Sambungan tidak lagi disimpan di tiga tempat berbeda (`FiberCable.splices`, `LocationPOP.ports`, dan client snapshot) yang kerap saling menimpa data (*last-write-wins*). Kini seluruh sambungan tersimpan di koleksi tunggal `fiber_connections` dengan *unique index*.
- **Pencegahan Error Ujung Kabel Melenceng**: Memperbaiki bug di mana ujung kabel berada ratusan meter di luar posisi node atau menghasilkan garis spur zigzag akibat kesalahan proyeksi titik koordinat.
- **Pencegahan Redaman Negatif & Fallback Keliru**: Memperbaiki bug pada engine tracing di mana nilai redaman sempurna `0 dB` keliru diubah menjadi `0.1 dB`.
- **Pencegahan Sambungan Yatim (*Orphan Connections*)**: Menolak penghapusan kabel, peralatan, atau node yang masih memiliki sambungan optik aktif dengan HTTP `422 Unprocessable Entity`.

### Menu/Fitur Baru

- **Modul Peta Fiber Optik Terdesentralisasi** di Menu `Jaringan > Kabel Fiber Optik` (`/network/fiber-cable`).
- **Dialog Sambungan Core & Port** (`ConnectDialog`).
- **Pratinjau Split & Merge Terproyeksi** di Sidebar Peta.
- **Modul Arsip Administrasi** di Menu `Arsip > Administrasi` (`/archive/administration`).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: **Alur Penyambungan Core Optik & Manajemen Peralatan Node (Issue #365)**  
  Fitur ini memungkinkan teknisi atau perencana jaringan untuk memetakan jalur kabel serat optik di lapangan secara akurat hingga tingkat core dan port. Melalui arsitektur baru, setiap sambungan (fusi core atau terminasi port) dicatat secara atomik di database dengan validasi bentrok seketika.

- **Langkah Penggunaan (Tutorial)**:
  1. **Melihat Titik Sambungan Node**:
     - Buka menu **Jaringan > Kabel Fiber Optik** (`/network/fiber-cable`).
     - Geser (*pan*) atau perbesar (*zoom*) peta ke area kerja yang diinginkan; sistem otomatis memuat data node dan kabel di area tersebut.
     - Klik pada salah satu ikon node (POP, Joint Closure, ODP, atau ODC) untuk membuka **Node Info Drawer** di sisi kanan.
  2. **Menambahkan Peralatan Fiber di Node**:
     - Pada drawer node yang terbuka, pilih tab **Peralatan** (*Equipment*).
     - Klik tombol **Tambah Peralatan**, pilih jenis peralatan (misal: *Splitter 1:8*, *Patch Panel 24 Port*, dsb.), beri nama label, dan simpan.
  3. **Menyambungkan Core ke Core atau Port**:
     - Pilih tab **Sambungan** (*Connections*) pada drawer node.
     - Klik pada baris core kabel yang ingin disambung, lalu klik tombol **Sambungkan ke…**.
     - Modal dialog `ConnectDialog` akan muncul. Pilih target sambungan:
       - **Ke Core Kabel Lain**: Pilih kabel tujuan dan nomor core tujuan yang masih kosong (*free*).
       - **Ke Port Peralatan**: Pilih peralatan yang ada di node tersebut dan nomor port yang tersedia.
     - Masukkan nilai redaman (*loss dB*, default `0.1` dB atau `0` dB untuk fusi murni) dan catatan tambahan jika ada.
     - Klik **Simpan Sambungan**. Sistem secara instan memperbarui status sambungan pada peta, diagram tray, dan mengunci core tersebut dari penggunaan ganda.
  4. **Melakukan Tracing Sinyal & OTDR**:
     - Buka tab **Tracing** di panel sidebar kiri.
     - Pilih kabel asal dan nomor core awal.
     - Klik tombol **Lacak Jalur Core**. Sistem akan menelusuri seluruh rangkaian sambungan tanpa henti hingga mencapai titik akhir termination/peralatan, menampilkan total panjang jalur, estimasi redaman kumulatif, dan titik sambungan yang dilalui.
