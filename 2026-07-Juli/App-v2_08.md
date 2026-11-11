# 📝 Daily Work Report - Dedy Putra (2026-07-08)

---

## 📅 Laporan Harian - 8 Juli 2026

> Hari ini dikerjakan **5 issue** lintas branch: `issue-132`, `issue-134`, `issue-118`, `issue-123`, dan `issue-129` (WIP).

---

## 📌 Ringkasan Issue yang Dikerjakan

| # | Nomor Issue | Branch | Status |
|---|-------------|--------|--------|
| 1 | #132 | `issue-132` | ✅ Resolve (Pushed) |
| 2 | #134 | `issue-134` | ✅ Resolve (Pushed) |
| 3 | #118 | `issue-118` | ✅ Resolve (Pushed) |
| 4 | #123 | `issue-123` | 🔄 Save / WIP (Pushed) |
| 5 | #129 | `issue-129` | 🔄 Save / WIP |

---

## 🔖 Issue #132 — Peningkatan Sistem Tiket & Standarisasi Tabel

### 📅 Rincian Commit

#### [[e78ef74]] - resolve #132 (#132 - Peningkatan Sistem Tiket & Standarisasi Tabel)

- **Komponen yang Berubah (Backend)**:
  - `backend/src/controllers/ticket.controller.js` — Tambahan logika controller tiket (133 baris baru)
  - `backend/src/services/ticket.service.js` — Refaktor service tiket
  - `backend/src/models/ticket.model.js` — Penambahan field baru pada model tiket
  - `backend/src/routes/ticket.route.js` — Tambahan 46 baris definisi route tiket
  - `backend/src/locales/en/translation.json` & `id/translation.json` — Tambahan string i18n baru
  - `backend/src/utils/data-table.js` — Update utilitas data table
  - `backend/src/utils/missing-i18n-logger.js` — Update logger i18n

- **Komponen yang Berubah (Frontend)**:
  - `frontend/src/app/pages/tickets/TicketDetailDrawer.jsx` — Perombakan besar drawer detail tiket (238 baris baru)
  - `frontend/src/app/pages/tickets/GeneralInformation.jsx` — Update informasi umum tiket
  - `frontend/src/app/pages/tickets/MessageUpdate.jsx` — Update tampilan pesan tiket
  - `frontend/src/app/pages/tickets/backbone/BackboneReport.jsx` & `columns.jsx` — Update laporan & kolom backbone
  - `frontend/src/app/pages/tickets/customer/CustomerReport.jsx` & `columns.jsx`
  - `frontend/src/app/pages/tickets/dismantle/DismantleReport.jsx` & `columns.jsx`
  - `frontend/src/app/pages/tickets/installation/InstallationReport.jsx` & `columns.jsx`
  - `frontend/src/app/pages/tickets/other/OtherReport.jsx` & `columns.jsx`
  - `frontend/src/app/pages/tickets/partner/PartnerReport.jsx` & `columns.jsx` — Perombakan signifikan
  - `frontend/src/app/pages/tickets/payment/PaymentReport.jsx` & `columns.jsx`
  - `frontend/src/app/pages/tickets/survey/SurveyReport.jsx` & `columns.jsx`
  - `frontend/src/components/shared/table/Table.jsx` — Simplifikasi komponen tabel (57 baris dikurangi)
  - `frontend/src/i18n/locales/en/translations.json` & `id/translations.json` — Tambahan terjemahan

- **Deskripsi Perubahan & Fungsi**:
  - Penambahan endpoint dan logika backend baru untuk fitur tiket
  - Refaktor `TicketDetailDrawer` secara signifikan untuk menampilkan informasi tiket yang lebih lengkap dan terstruktur
  - Standarisasi skema kolom (`columns.jsx`) pada semua jenis laporan tiket (backbone, customer, dismantle, installation, other, partner, payment, survey)
  - Simplifikasi komponen `Table.jsx` agar lebih modular dan ringkas
  - Penambahan terjemahan i18n untuk fitur baru

---

## 🔖 Issue #134 — Perbaikan Menyeluruh & Tambahan Fitur Vendor/Tiket

### 📅 Rincian Commit

#### [[87d7e19]] - resolve #134 (#134 - Perbaikan Menyeluruh & Tambahan Fitur Vendor/Tiket)

- **Komponen yang Berubah (Backend)**:
  - `backend/src/controllers/document.controller.js` — Perbaikan controller dokumen
  - `backend/src/controllers/files.controller.js` — Perbaikan controller file
  - `backend/src/controllers/internal.controller.js` — Refaktor controller internal (24 baris berubah)
  - `backend/src/controllers/log.controller.js` — Perbaikan controller log
  - `backend/src/controllers/publicPO.controller.js` — Perbaikan controller public PO
  - `backend/src/controllers/ticket.controller.js` — Tambahan logika tiket (20 baris baru)
  - `backend/src/controllers/vendor.controller.js` — Update signifikan controller vendor (44 baris)
  - `backend/src/controllers/warehouseItem.controller.js` — Refaktor controller item gudang
  - `backend/src/controllers/warehouseMutation.controller.js` — Update mutasi gudang
  - `backend/src/controllers/warehouseRequest.controller.js` — Update permintaan gudang
  - `backend/src/locales/config.js` — Update konfigurasi bahasa
  - `backend/src/middlewares/auth.middleware.js` — Update middleware autentikasi
  - `backend/src/models/ticket.model.js` — Tambahan field pada model tiket
  - `backend/src/routes/internal.route.js` & `vendor.route.js` — Update route
  - `backend/src/services/ticket.service.js` & `vendor.service.js` — Update service
  - `backend/src/services/warehouseReport.service.js` & `warehouseType.service.js` — Perbaikan
  - `backend/src/utils/data-table.js` — Update utilitas
  - `backend/src/utils/missing-i18n-logger.js` — Perbaikan logger
  - `backend/src/utils/telegram.js` — Refaktor utilitas Telegram (15 baris berubah)

- **Komponen yang Berubah (Frontend)**:
  - `frontend/src/app/navigation/services.js` — Update navigasi layanan
  - `frontend/src/app/pages/services/activation/components/EditBAADrawer.jsx` — Perbaikan drawer BAA
  - `frontend/src/app/pages/services/activation/components/ReviewDrawer.jsx` — Update drawer review
  - `frontend/src/app/pages/services/purchaseOrder/PODocumentPreview.jsx` — Perbaikan preview PO
  - `frontend/src/app/pages/services/purchaseOrder/edit.jsx` — Update form edit PO
  - `frontend/src/app/pages/services/vendorManagement/detail.jsx` — Update halaman detail vendor (40 baris baru)
  - `frontend/src/app/pages/services/vendorManagement/schema/ticketColumns.jsx` [NEW] — Kolom tiket baru di halaman vendor
  - `frontend/src/app/pages/tickets/GeneralInformation.jsx` — Tambahan informasi umum tiket
  - `frontend/src/app/pages/tickets/MessageUpdate.jsx` — Update tampilan pesan
  - `frontend/src/app/pages/tickets/TicketDetailDrawer.jsx` — Update detail drawer tiket
  - `frontend/src/app/pages/tickets/*/[jenis]Report.jsx` — Update semua jenis laporan tiket
  - `frontend/src/app/pages/tickets/partner/create.jsx` — Tambahan form buat tiket partner (24 baris baru)
  - `frontend/src/app/pages/tickets/partner/edit.jsx` — Tambahan form edit tiket partner (33 baris baru)
  - `frontend/src/app/pages/tickets/partner/schema/createSchema.js` — Update schema validasi
  - `frontend/src/components/shared/DrawerSign.jsx` — Update komponen tanda tangan
  - `frontend/src/components/shared/table/Table.jsx` — Update komponen tabel
  - `frontend/src/i18n/config.js` — Update konfigurasi i18n (32 baris berubah)
  - `frontend/src/i18n/locales/id/translations.json` — Update terjemahan Indonesia

- **Deskripsi Perubahan & Fungsi**:
  - Perbaikan menyeluruh (bug fix) pada berbagai controller backend untuk kekonsistenan dan keandalan
  - Penambahan skema kolom tiket pada halaman detail vendor (`ticketColumns.jsx`) sehingga tiket terkait vendor bisa ditampilkan langsung dari halaman vendor
  - Penguatan fitur buat dan edit tiket partner dengan validasi schema yang lebih lengkap
  - Refaktor konfigurasi i18n frontend untuk mendukung namespace yang lebih terstruktur

---

## 🔖 Issue #118 — Hak Akses Produk & Perbaikan Tiket/Vendor

### 📅 Rincian Commit

#### [[e8f7416]] - resolve #118 (#118 - Hak Akses Produk & Perbaikan Tiket/Vendor)

- **Komponen yang Berubah (Backend)**:
  - `backend/src/config/privilege.json` — Update konfigurasi hak akses (6 perubahan)
  - `backend/src/controllers/ticket.controller.js` — Refaktor controller tiket (44 baris berubah)
  - `backend/src/services/productDataAccess.service.js` [NEW] — Service baru untuk kontrol akses data produk (42 baris)

- **Komponen yang Berubah (Frontend)**:
  - `frontend/src/app/pages/services/salesOrder/SODocumentPreview.jsx` — Perbaikan preview dokumen SO
  - `frontend/src/app/pages/services/vendorManagement/detail.jsx` — Update detail vendor (30 baris berubah)
  - `frontend/src/i18n/locales/en/translations.json` & `id/translations.json` — Tambahan 7 kunci terjemahan baru
  - `frontend/src/utils/axios.js` — Penambahan 4 baris konfigurasi Axios

- **Deskripsi Perubahan & Fungsi**:
  - Pembuatan service baru `productDataAccess.service.js` untuk mengelola kontrol akses data produk secara terpusat dan terstandar
  - Penyesuaian konfigurasi privilege untuk mendukung role permission yang lebih granular pada fitur produk
  - Perbaikan tampilan preview dokumen Sales Order
  - Peningkatan konfigurasi Axios di frontend untuk menangani skenario autentikasi/request tertentu

---

## 🔖 Issue #129 — Manajemen Jaringan Fiber Optic (WIP)

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `backend/src/controllers/locationPoint.controller.js` — Update controller titik lokasi (fiber/node)
- `backend/src/models/fiberCable.model.js` — Update model fiber cable
- `backend/src/models/locationPoint.model.js` — Update model titik lokasi
- `frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx` — Update drawer info node
- `frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx` — Update komponen splice tray
- `frontend/src/i18n/locales/en/translations.json` & `id/translations.json` — Tambahan terjemahan
- `frontend/src/app/pages/network/fiberCable/components/DropCoreModal.jsx` [NEW] — Modal manajemen drop core *(belum di-stage)*
- `frontend/src/app/pages/network/fiberCable/components/NodeEquipment.jsx` [NEW] — Tampilan perangkat di node *(belum di-stage)*

### 📅 Rincian Commit (Save WIP)

#### [[6f0120c]] - save #129 (#129 - Manajemen Jaringan Fiber Optic)

- **Komponen yang Berubah**:
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx` — Integrasi data node ke peta
  - `frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx` — Pengembangan signifikan drawer info node (190 baris baru)
  - `frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx` — Update manajemen splice tray
  - `frontend/src/app/pages/network/fiberCable/index.jsx` — Update halaman utama fiber cable
  - `frontend/src/i18n/locales/en/translations.json` & `id/translations.json` — Tambahan terjemahan fiber

- **Deskripsi Perubahan & Fungsi**:
  - Pengembangan signifikan `NodeInfoDrawer` untuk menampilkan detail komprehensif dari setiap titik node jaringan fiber optik
  - Peningkatan `SpliceTray` untuk manajemen splicing core fiber pada node
  - Integrasi informasi node ke dalam peta interaktif `FiberMap`
  - Pengembangan dua komponen baru: `DropCoreModal` (modal manajemen drop core ke pelanggan) dan `NodeEquipment` (tampilan inventaris perangkat di node) — *masih aktif dikerjakan*

---

## 📢 Dampak Perubahan & Fungsionalitas Baru

### Issue #132 — Sistem Tiket
- **Kemampuan Pengguna/Admin**: Admin dapat melihat detail tiket yang jauh lebih lengkap dan terstruktur melalui `TicketDetailDrawer` yang telah diperbaru. Tabel laporan tiket (semua 8 jenis) kini menggunakan kolom yang terstandardisasi untuk konsistensi UX.
- **Bug Fix / Solusi Masalah**: Perbaikan standarisasi skema kolom tabel pada seluruh jenis laporan tiket agar konsisten dengan aturan arsitektur proyek (tidak menggunakan `Badge` mentah di columns.jsx).
- **Menu/Tombol Baru**: Endpoint API tiket baru yang mendukung operasi tambahan pada ekosistem sistem tiket.

### Issue #134 — Vendor & Integrasi Tiket
- **Kemampuan Pengguna/Admin**: Halaman detail vendor kini menampilkan daftar tiket yang terkait langsung dengan vendor tersebut dalam tabel yang terintegrasi. Admin dapat membuat dan mengedit tiket partner langsung dari modul tiket.
- **Bug Fix / Solusi Masalah**: Perbaikan berbagai bug pada controller backend (dokumen, file, gudang, vendor) yang meningkatkan stabilitas dan keandalan sistem secara keseluruhan.
- **Menu/Tombol Baru**: Aksi buat/edit tiket partner yang diperkuat dengan schema validasi form baru.

### Issue #118 — Kontrol Akses Produk
- **Kemampuan Pengguna/Admin**: Sistem kini mendukung kontrol akses data produk yang lebih terperinci melalui service terpusat `productDataAccess`. Role dan privilege dapat dikonfigurasi per fitur produk secara lebih spesifik.
- **Bug Fix / Solusi Masalah**: Perbaikan tampilan preview dokumen Sales Order dan penyesuaian konfigurasi privilege untuk keakuratan hak akses pengguna.

### Issue #129 — Jaringan Fiber Optic (Ongoing)
- **Kemampuan Pengguna/Admin**: Technician/admin dapat melihat informasi detail node jaringan fiber optik secara langsung dari peta interaktif melalui `NodeInfoDrawer` yang diperkaya dengan informasi splice tray dan koneksi.
- **Bug Fix / Solusi Masalah**: Peningkatan akurasi data titik lokasi dan model kabel fiber untuk representasi jaringan yang lebih tepat.
- **Menu/Tombol Baru**: Komponen baru `DropCoreModal` (segera hadir) untuk mengelola koneksi drop core dari panel fiber, dan `NodeEquipment` untuk inventaris perangkat di setiap node.

---

## 📖 Informasi & Tutorial Singkat Fitur

### Fiber Optic Network Management (Issue #129)
- **Penjelasan Fitur**: Modul manajemen jaringan fiber optik memungkinkan admin/teknisi untuk memetakan, memonitor, dan mengelola infrastruktur jaringan berbasis peta interaktif. Setiap titik lokasi (node) dapat diklik untuk melihat detail perangkat, splice tray, dan koneksi drop core.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Network → Fiber Cable**
  2. Peta jaringan akan tampil dengan penanda node (titik lokasi)
  3. Klik pada salah satu node/titik untuk membuka **Node Info Drawer** di sisi kanan layar
  4. Di dalam drawer, tersedia informasi splice tray: core mana saja yang sudah tersplice dan kemana terhubung
  5. *(Segera hadir)* Tab **Drop Core** untuk mengelola koneksi ke pelanggan, dan **Equipment** untuk melihat inventaris perangkat yang terpasang di node tersebut

### Sistem Tiket — Fitur Terbaru (Issue #132 & #134)
- **Penjelasan Fitur**: Sistem tiket mendukung berbagai jenis pekerjaan lapangan. Setiap tiket memiliki drawer detail komprehensif yang menampilkan semua informasi yang relevan. Halaman vendor kini terintegrasi dengan tiket terkait vendor.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Tickets** dan pilih jenis tiket (Instalasi, Backbone, Dismantle, dll.)
  2. Klik baris tiket pada tabel untuk membuka **Ticket Detail Drawer**
  3. Drawer menampilkan: Informasi Umum, Log Pesan, dan Laporan spesifik jenis tiket
  4. Untuk melihat tiket terkait vendor: buka **Vendor Management → Detail Vendor → Tab Tiket**
  5. Untuk membuat tiket partner baru: buka menu **Tickets → Partner → Buat Tiket**
