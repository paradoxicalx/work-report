# 📝 Daily Work Report - Dedy (2026-07-10)

---

## 📅 Laporan Harian - 10 Juli 2026

> ✅ **Status**: Semua commit pada laporan ini sudah **di-merge ke `master`** dan **di-push ke `origin/master`** (production). Tidak ada branch aktif yang tertunda. Working tree dalam keadaan bersih (*clean*).

---

## 🌿 Branch: `issue-118` — Pemisahan Modul Purchase Order & Sales Order dari Vendor

### 📌 Informasi Issue
- **Nomor Issue**: #118
- **Judul Issue**: Pemisahan Modul Purchase Order & Sales Order dari Vendor + Public Document Review + PDF Sign
- **Status Branch**: `Sudah di-merge` (merge commit `47d045f` ke master)

### 📅 Rincian Commit

#### `47d045f` - resolve #118 - 10 Juli 2026, 19:03 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/purchaseOrder.controller.js` [NEW]
  - `backend/src/controllers/salesOrder.controller.js` [NEW]
  - `backend/src/controllers/publicSO.controller.js`
  - `backend/src/controllers/ticket.controller.js`
  - `backend/src/controllers/files.controller.js`
  - `backend/src/controllers/vendor.controller.js` [DELETE]
  - `backend/src/services/purchaseOrder.service.js` [NEW]
  - `backend/src/services/salesOrder.service.js` [NEW]
  - `backend/src/services/productDataAccess.service.js` [NEW]
  - `backend/src/services/vendor.service.js` [DELETE]
  - `backend/src/models/vendorSO.model.js` [NEW]
  - `backend/src/routes/purchaseOrder.route.js` [NEW]
  - `backend/src/routes/salesOrder.route.js` [NEW]
  - `backend/src/routes/vendor.route.js` [DELETE]
  - `backend/src/routes/files.route.js`
  - `backend/src/routes/public.route.js`
  - `backend/src/utils/pdf-sign.js` [NEW]
  - `backend/src/utils/telegram.js`
  - `backend/src/config/privilege.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/app.js`
  - `frontend/src/app/pages/public/PublicSODocument.jsx` [NEW]
  - `frontend/src/app/pages/public/ReviewSOPage.jsx` [NEW]
  - `frontend/src/app/pages/public/PublicBAADocument.jsx`
  - `frontend/src/app/pages/public/PublicBADDocument.jsx`
  - `frontend/src/app/pages/public/publicBAPDocument.jsx`
  - `frontend/src/app/pages/public/ReviewPOPage.jsx`
  - `frontend/src/app/pages/services/salesOrder/SODocumentPreview.jsx` [NEW]
  - `frontend/src/app/pages/services/salesOrder/create.jsx` [NEW]
  - `frontend/src/app/pages/services/salesOrder/edit.jsx` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/detail.jsx`
  - `frontend/src/app/pages/services/vendorManagement/schema/vendorSOSchema.js` [NEW]
  - `frontend/src/app/pages/services/activation/components/SOReviewDrawer.jsx` [NEW]
  - `frontend/src/app/pages/services/activation/index.jsx`
  - `frontend/src/app/pages/services/activation/schema/poColumns.jsx`
  - `frontend/src/app/pages/services/activation/schema/soColumns.jsx` [NEW]
  - `frontend/src/components/shared/DocumentPreviewModal.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/configs/pdf.config.js` [NEW]
  - `frontend/src/utils/formatFileSize.js` [NEW]
  - `frontend/src/utils/axios.js`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/router/public.jsx`
  - `frontend/src/app/router/services/vendorRoute.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Refactor Besar Vendor Module**: Modul `vendor` dipecah menjadi dua modul independen: **Purchase Order (PO)** dan **Sales Order (SO)**. Controller, service, dan route `vendor` dihapus (`vendor.controller.js`, `vendor.service.js`, `vendor.route.js`) dan digantikan oleh `purchaseOrder.*` dan `salesOrder.*`.
  - **Public SO Document**: Menambahkan halaman publik `PublicSODocument.jsx` untuk menampilkan dokumen Sales Order yang dapat diakses tanpa login, mirip dengan halaman BA/BAD/BAP yang sudah ada.
  - **Review SO Page**: Halaman `ReviewSOPage.jsx` untuk proses review dan approval Sales Order oleh pihak terkait via link publik.
  - **SO Review Drawer**: Komponen `SOReviewDrawer.jsx` di halaman Activation untuk meninjau Sales Order dalam proses aktivasi layanan.
  - **PDF Sign Utility**: Menambahkan `pdf-sign.js` di backend untuk melakukan digital signing pada dokumen PDF yang di-generate, memberikan validitas hukum pada dokumen PO/SO.
  - **Product Data Access Service**: Service baru `productDataAccess.service.js` untuk mengelola akses data produk secara terpusat, termasuk pengecekan hak akses berdasarkan tiket/langganan.
  - **Telegram Notifikasi**: Menambahkan fungsi notifikasi Telegram (`telegram.js`) untuk mengirim pemberitahuan saat PO/SO dibuat, di-review, atau disetujui.
  - **Vendor SO Model**: Model baru `vendorSO.model.js` untuk menyimpan data Sales Order yang terkait dengan vendor.
  - **Format File Size Utility**: Utility frontend baru `formatFileSize.js` untuk menampilkan ukuran file dengan format human-readable (KB, MB, GB).
  - **PDF Config**: Konfigurasi `pdf.config.js` untuk pengaturan generate dan sign dokumen PDF.
  - **Privilege Update**: Menyesuaikan hak akses di `privilege.json` untuk modul PO dan SO yang baru.
  - **i18n**: Menambahkan 198+ key translasi untuk Bahasa Inggris dan Bahasa Indonesia.
  - **Total**: 52 files changed, +5,996 insertions, -1,315 deletions.

---

## 🌿 Branch: `issue-129` — Peningkatan Manajemen Fiber Cable & Topologi Jaringan

### 📌 Informasi Issue
- **Nomor Issue**: #129
- **Judul Issue**: Peningkatan Manajemen Fiber Cable, Core Topology Visualization, Node Equipment & Splice Management
- **Status Branch**: `Sudah di-merge` (merge commit `c72ab02` ke master)

### 📅 Rincian Commit

#### `ec50399` - Merge branch 'master' into issue-129 - 10 Juli 2026, 19:08 WIB

- **Deskripsi**: Merge sinkronisasi dari master ke branch `issue-129` sebelum final merge untuk menyelesaikan konflik dengan perubahan issue #118 yang sudah di-merge terlebih dahulu.

#### `c72ab02` - resolve #129 - 10 Juli 2026, 19:14 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/fiberCable.controller.js`
  - `backend/src/controllers/locationPoint.controller.js`
  - `backend/src/services/fiberCable.service.js`
  - `backend/src/services/fiberTrace.service.js`
  - `backend/src/services/locationPoint.service.js`
  - `backend/src/services/nodeReport.service.js` [NEW]
  - `backend/src/models/fiberCable.model.js`
  - `backend/src/models/locationPoint.model.js`
  - `backend/src/routes/locationPoint.route.js`
  - `backend/scripts/migrate-splices-node.js` [NEW]
  - `backend/src/config/privilege.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `frontend/src/app/pages/network/fiberCable/components/CoreTopologyCustomEdge.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/CoreTopologyCustomNode.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/CoreTopologyModal.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/DropCoreModal.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/NodeEquipment.jsx` [NEW]
  - `frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx`
  - `frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx`
  - `frontend/src/app/pages/network/fiberCable/index.jsx`
  - `frontend/src/constants/fiberColors.constant.js` [NEW]
  - `frontend/src/styles/index.css`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `.gitignore`

- **Deskripsi Perubahan & Fungsi**:
  - **Node Equipment Component (`NodeEquipment.jsx`)**: Komponen baru sepanjang 1,061 baris yang menampilkan daftar peralatan yang terpasang di setiap node/lokasi dalam jaringan fiber. Menampilkan informasi tipe perangkat, port, status, dan konfigurasi.
  - **Node Report Service (`nodeReport.service.js`)**: Service backend baru untuk menghasilkan laporan per node yang mencakup data perangkat, koneksi fiber, dan status operasional.
  - **Drop Core Modal (`DropCoreModal.jsx`)**: Modal baru untuk mengelola proses "drop core" — yaitu menghubungkan core fiber dari kabel utama ke perangkat pelanggan. Mendukung pemilihan core, pencatatan loss, dan konfigurasi koneksi.
  - **Core Topology Visualization**: Peningkatan signifikan pada `CoreTopologyCustomNode.jsx` (+512 lines) dan `CoreTopologyCustomEdge.jsx` untuk visualisasi topologi fiber yang lebih interaktif — termasuk warna kabel berdasarkan tipe, label core, dan indikator status koneksi.
  - **Fiber Colors Constant (`fiberColors.constant.js`)**: Konstanta baru yang mendefinisikan palet warna standar untuk berbagai tipe fiber (single-mode, multi-mode, dll) dan status koneksi.
  - **Splice Tray Enhancement**: Peningkatan komponen `SpliceTray.jsx` (+190 lines) untuk manajemen splicing yang lebih detail — termasuk tracking loss per splice, dokumentasi foto, dan status verifikasi.
  - **Node Info Drawer Overhaul**: Refactor besar pada `NodeInfoDrawer.jsx` (+1,389 lines) untuk menampilkan informasi node yang lebih komprehensif: ringkasan perangkat, daftar koneksi fiber, splice history, dan equipment status.
  - **Fiber Map Enhancement**: Peningkatan `FiberMap.jsx` untuk mendukung visualisasi rute fiber yang lebih akurat dengan koordinat geografis.
  - **Location Point Management**: Penambahan field baru di model `locationPoint.model.js` (+51 lines) untuk mendukung data node fiber — termasuk tipe lokasi (POP, ODP, Pelanggan), koordinat, dan kapasitas.
  - **Fiber Trace Service**: Peningkatan `fiberTrace.service.js` (+96 lines) untuk tracing rute fiber end-to-end — dari OLT ke ONT pelanggan — lengkap dengan perhitungan total loss dan jumlah splice point.
  - **Migrate Splices Node Script**: Script migrasi `migrate-splices-node.js` untuk memigrasikan data splicing dari struktur lama ke struktur node-based yang baru.
  - **Sidebar Tools Enhancement**: Peningkatan tools sidebar untuk filter dan pencarian node berdasarkan tipe, status, dan kapasitas.
  - **Total**: 30 files changed, +3,948 insertions, -707 deletions.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
|-------|-------|--------------|
| #118 | Pemisahan Modul PO & SO dari Vendor | Modul Vendor dipecah menjadi Purchase Order dan Sales Order independen; tambahan public document review, PDF digital signing, dan notifikasi Telegram |
| #129 | Peningkatan Fiber Cable & Topologi | Visualisasi topologi core fiber interaktif, manajemen perangkat node, drop core modal, splice management detail, dan laporan node |

### Kemampuan Baru Pengguna/Admin
- Admin sekarang dapat membuat dan mengelola **Purchase Order** dan **Sales Order** secara terpisah melalui modul yang dedicated (sebelumnya tergabung dalam satu modul Vendor)
- Pihak ketiga (vendor/customer) dapat me-review dan menyetujui **Sales Order** melalui halaman publik (`ReviewSOPage`) tanpa perlu login ke sistem
- Dokumen PO/SO kini dilengkapi **digital signature (PDF Sign)** yang memberikan validitas hukum
- Notifikasi Telegram otomatis dikirim saat PO/SO dibuat, di-review, atau disetujui
- Teknisi lapangan dapat melihat **topologi core fiber** secara visual dan interaktif — termasuk warna kabel, status koneksi, dan rute end-to-end
- Teknisi dapat melakukan **drop core** (menghubungkan core ke pelanggan) melalui modal khusus dengan pencatatan loss dan konfigurasi
- Admin jaringan dapat melihat **daftar perangkat (equipment)** yang terpasang di setiap node/lokasi
- Teknisi dapat melakukan **fiber trace** end-to-end dari OLT ke ONT dengan perhitungan total loss otomatis
- Sistem splice management kini mendukung tracking **loss per splice**, dokumentasi foto, dan status verifikasi

### Bug Fix / Solusi Masalah
- **Issue #118**: Memperbaiki masalah akses data produk yang sebelumnya tidak mempertimbangkan hak akses berdasarkan tiket/langganan (via `productDataAccess.service.js`)
- **Issue #118**: Memperbaiki inkonsistensi routing — route vendor yang lama dihapus dan diganti dengan route PO/SO yang dedicated
- **Issue #129**: Memperbaiki struktur data splicing dari format lama ke format node-based yang lebih scalable (via script migrasi)

### Menu/Fitur Baru
- **Purchase Order** (`/services/purchase-order`) — Menu untuk membuat dan mengelola PO ke vendor
- **Sales Order** (`/services/sales-order`) — Menu untuk membuat dan mengelola SO dari customer
- **Sales Order Create** (`/services/sales-order/create`) — Form pembuatan SO baru
- **Sales Order Edit** (`/services/sales-order/edit/:id`) — Form edit SO
- **Public SO Document** (`/public/so/:id`) — Halaman publik untuk melihat dokumen SO
- **Review SO Page** (`/public/review-so/:id`) — Halaman review dan approval SO
- **SO Review Drawer** (di halaman Activation) — Drawer untuk meninjau SO saat aktivasi layanan
- **Node Equipment** (di halaman Fiber Cable) — Tab/layer yang menampilkan perangkat di setiap node
- **Drop Core Modal** (di halaman Fiber Cable) — Modal untuk proses drop core fiber ke pelanggan

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Modul Purchase Order & Sales Order (Issue #118)

**Penjelasan Fitur**: Modul Vendor yang lama telah dipecah menjadi dua modul terpisah: **Purchase Order** (pembelian ke vendor) dan **Sales Order** (penjualan ke customer). Setiap modul memiliki form create, edit, dan document preview yang dedicated. Dokumen PO/SO kini dilengkapi digital signature (PDF Sign) dan notifikasi Telegram otomatis. Pihak ketiga dapat me-review dan menyetujui SO melalui halaman publik tanpa login.

**Langkah Penggunaan — Membuat Sales Order**:
1. Buka menu **Services → Sales Order**
2. Klik tombol **Create** untuk membuat SO baru
3. Isi data customer, produk/layanan, jumlah, dan harga
4. Klik **Save** — sistem akan meng-generate dokumen PDF dengan digital signature
5. Notifikasi Telegram dikirim ke pihak terkait
6. Customer dapat me-review SO melalui link publik yang dikirimkan

**Langkah Penggunaan — Review SO (Customer)**:
1. Buka link publik yang diterima (contoh: `/public/review-so/<id>`)
2. Lihat detail dokumen SO
3. Klik **Approve** atau **Reject**
4. Status review tersimpan dan notifikasi dikirim ke admin

### 2. Fiber Cable Core Topology & Node Equipment (Issue #129)

**Penjelasan Fitur**: Halaman Fiber Cable kini dilengkapi visualisasi topologi core fiber yang interaktif. Setiap node menampilkan daftar perangkat (equipment) yang terpasang. Teknisi dapat melakukan drop core (menghubungkan core ke pelanggan) melalui modal khusus, melakukan fiber trace end-to-end dengan perhitungan loss otomatis, dan mengelola splicing dengan tracking loss per splice.

**Langkah Penggunaan — Melihat Topologi Core Fiber**:
1. Buka menu **Network → Fiber Cable**
2. Pilih kabel fiber yang ingin dilihat topologinya
3. Klik tombol **Core Topology** — sistem menampilkan visualisasi interaktif dengan warna kabel berdasarkan tipe fiber
4. Klik node mana saja untuk melihat **Node Info Drawer** — menampilkan detail perangkat, koneksi, dan splice history
5. Gunakan tools sidebar untuk filter node berdasarkan tipe (POP, ODP, Pelanggan)

**Langkah Penggunaan — Drop Core ke Pelanggan**:
1. Di halaman Fiber Cable, pilih node sumber (misal: ODP)
2. Klik tombol **Drop Core**
3. Pilih core fiber yang tersedia dari daftar
4. Masukkan data pelanggan dan nilai loss (dB)
5. Klik **Confirm** — koneksi fiber dari ODP ke pelanggan tercatat

**Langkah Penggunaan — Fiber Trace End-to-End**:
1. Di Node Info Drawer, klik tab **Trace**
2. Pilih titik awal (OLT) dan titik akhir (ONT Pelanggan)
3. Sistem menampilkan rute fiber lengkap dengan:
   - Jumlah splice point
   - Total loss kumulatif (dB)
   - Status setiap segmen koneksi
4. Data trace dapat di-export sebagai laporan
