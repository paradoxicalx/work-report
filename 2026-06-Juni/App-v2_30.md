# 📝 Daily Work Report — Dedy Putra (30 Juni 2026)

---

## 📌 Informasi Issue

- **Nomor Issue**: #104
- **Judul Issue**: Vendor Management & Purchase Order System

## 📅 Laporan Harian — 30 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- [`backend/src/config/privilege.json`](file:///d:/Project/DEKASIMAL_V2/backend/src/config/privilege.json)
  - **Deskripsi**: Perbaikan final newline di akhir berkas (trailing newline).

- [`report.txt`](file:///d:/Project/DEKASIMAL_V2/report.txt)
  - **Deskripsi**: Hasil audit kode menyeluruh pada branch issue-104 — berisi 24 temuan bug, kerentanan keamanan, dan pelanggaran standar kode (format Before vs. After).

- [`task.md`](file:///d:/Project/DEKASIMAL_V2/task.md)
  - **Deskripsi**: Checklist actionable perbaikan hasil audit, dikelompokkan berdasarkan prioritas (Kritis, Tinggi, Sedang, Rendah) dengan markdown checkbox.

### 📅 Rincian Commit

#### [879730d] - resolve #104 (#104 — Vendor Management & Purchase Order System)

- **Komponen yang Berubah** (90 file, +6,085 / -3,685 baris):
  - `backend/src/models/vendor.model.js` [NEW]
  - `backend/src/models/vendorPO.model.js` [NEW]
  - `backend/src/models/vendorService.model.js` [NEW]
  - `backend/src/controllers/vendor.controller.js` [NEW]
  - `backend/src/controllers/publicPO.controller.js` [NEW]
  - `backend/src/services/vendor.service.js` [NEW]
  - `backend/src/routes/vendor.route.js` [NEW]
  - `backend/src/utils/is-object-id.js` [NEW]
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/document.controller.js`
  - `backend/src/controllers/files.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/public.route.js`
  - `backend/src/utils/data-table.js`
  - `backend/src/utils/telegram.js`
  - `backend/src/utils/validation-data.js`
  - `frontend/src/app/navigation/services.js`
  - `frontend/src/app/pages/services/purchaseOrder/create.jsx` [NEW]
  - `frontend/src/app/pages/services/purchaseOrder/edit.jsx` [NEW]
  - `frontend/src/app/pages/services/purchaseOrder/PODocumentPreview.jsx` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/index.jsx` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/create.jsx` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/detail.jsx` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/edit.jsx` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/VendorItemDetailDrawer.jsx` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/schema/vendorSchema.js` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/schema/vendorItemSchema.js` [NEW]
  - `frontend/src/app/pages/services/vendorManagement/schema/vendorPOSchema.js` [NEW]
  - `frontend/src/app/router/services/vendorRoute.jsx` [NEW]
  - `frontend/src/components/shared/VendorItemModal.jsx` [NEW]
  - `frontend/src/components/ui/Modal.jsx` [NEW]
  - `frontend/src/constants/vendor.constant.js` [NEW]
  - `frontend/src/utils/fetchCompanyInfo.js` [NEW]
  - `frontend/src/app/pages/public/PublicPODocument.jsx` [NEW]
  - `frontend/src/app/pages/public/ReviewPOPage.jsx` [NEW]
  - `frontend/src/app/pages/services/activation/components/POReviewDrawer.jsx` [NEW]
  - `frontend/src/app/pages/services/activation/schema/poColumns.jsx` [NEW]
  - `frontend/src/app/pages/services/activation/schema/POApprovalStatusCell.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/router/public.jsx`
  - `frontend/src/components/shared/ConfirmModal.jsx`
  - `frontend/src/components/shared/DocumentPreviewModal.jsx`
  - `frontend/src/components/shared/GlobalConfirmModal.jsx`
  - `frontend/src/components/shared/MapRouteDrawer.jsx`
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/shared/table/RowActions.jsx`
  - `frontend/src/components/shared/table/Table.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/ui/index.js`
  - `frontend/src/constants/app.constant.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/middleware/AuthGuard.jsx`
  - `frontend/.env.example`
  - `backend/.env.example`
  - `telegram-api/.env.example`
  - Dan file-file pendukung lainnya (ipv4Management, hotspotVoucher, tickets, warehouse, fiberCable, printStation)

- **Deskripsi Perubahan & Fungsi**:
  - **Backend — Modul Vendor Management**: Menambahkan 3 model baru (`Vendor`, `VendorPO`, `VendorService`) dengan fitur soft-delete dan auto-increment. Controller `vendor.controller.js` meng-handle seluruh operasi CRUD untuk vendor, vendor item (katalog layanan/barang), dan Purchase Order (PO). Service layer `vendor.service.js` menyediakan DAL dengan field projection yang terdefinisi. Rute `vendor.route.js` melindungi seluruh endpoint dengan middleware `protectedAdmin` + `checkPrivilege` untuk RBAC.
  - **Backend — Public PO Endpoint**: Controller `publicPO.controller.js` menyediakan endpoint publik untuk vendor melihat dan menandatangani PO via token unik (`share_token`).
  - **Backend — Telegram Notifikasi PO**: Fungsi `TelegramNotifPOSubmit` di `utils/telegram.js` mengirim notifikasi Telegram ke admin approval ketika PO disubmit, lengkap dengan informasi vendor, jumlah item, total, dan link persetujuan.
  - **Backend — Privilege**: Menambahkan privilege `vendor.*` (list, read, create, update, delete, changeStatus) di `privilege.json`.
  - **Backend — Data Table Utility**: Peningkatan di `data-table.js` untuk mendukung force-select kolom tertentu (approval, complete) agar selalu tersedia meskipun disembunyikan di UI.
  - **Frontend — Vendor Management Pages**: Halaman indeks, detail, create, dan edit vendor dengan drawer system. Komponen `VendorItemDetailDrawer` untuk manajemen item/layanan vendor. Schema validasi Yup untuk vendor, vendor item, dan PO.
  - **Frontend — Purchase Order Drawer**: `CreatePO` dan `EditPODrawer` sebagai drawer full-width (900px) dengan fitur: katalog item dari vendor, line items dinamis (qty, unit, harga, subtotal), kalkulasi pajak (none/percentage/fixed), ringkasan grand total, pengaturan kontak PDF, dan unggah cap/stempel perusahaan.
  - **Frontend — Public PO Document & Review**: Halaman `PublicPODocument.jsx` untuk vendor melihat dan menandatangani PO melalui tautan publik. Halaman `ReviewPOPage.jsx` untuk admin memberikan persetujuan PO. Komponen `POReviewDrawer.jsx` di modul activation untuk proses approval terintegrasi.
  - **Frontend — Refactoring**: Modul vendor dipindahkan dari `services/vendor/` ke `services/vendorManagement/` untuk konsistensi penamaan. File lama `VendorFormDrawer.jsx`, `VendorPODetailDrawer.jsx`, `CreatePODrawer.jsx` dihapus dan diganti dengan implementasi baru yang lebih modular.
  - **Frontend — Shared Components**: `VendorItemModal.jsx` untuk memilih item vendor, `Modal.jsx` komponen UI baru, `DocumentPreviewModal.jsx` diperbarui untuk mendukung preview PO, `MapRouteDrawer.jsx` diperluas, `FormInput.jsx` menambahkan `InputAppend` dan `InputMoney`.
  - **Frontend — i18n**: Menambahkan 74+ key translasi baru (id & en) untuk seluruh UI vendor, PO, pajak, kontak, approval, dan dokumen.

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**:
  - Admin dapat mengelola data vendor (nama, kode, kontak AM/NOC, alamat, status aktif/non-aktif) melalui halaman Vendor Management.
  - Admin dapat mengelola katalog item/layanan vendor (tipe service/goods/contractor, harga OTC/MRC, kapasitas, SLA, kontrak).
  - Admin dapat membuat Purchase Order dengan memilih item dari katalog vendor, menentukan kuantitas, menghitung pajak otomatis, dan mengatur visibilitas kontak pada dokumen PDF.
  - Admin dapat mengajukan PO untuk persetujuan, mengirim permintaan preview ke admin lain via Telegram, menyetujui/menolak PO.
  - Admin dapat mengunggah lampiran dokumen (PDF, gambar, Word, Excel) pada PO.
  - Vendor (pihak eksternal) dapat melihat dan menandatangani PO melalui tautan publik yang aman.
- **Bug Fix / Solusi Masalah**:
  - Perbaikan UI fiberCable (NodeInfo, SidebarTools, SpliceTray) untuk kompatibilitas dengan komponen standar.
  - Perbaikan form ipv4Management create/edit untuk validasi yang lebih ketat.
  - Perbaikan DocumentPreviewModal untuk mendukung multiple document types (BAA, BAD, BAP, PO).
  - Perbaikan ConfirmModal dan GlobalConfirmModal untuk konsistensi state management.
- **Menu/Tombol Baru**:
  - Menu **Vendor** di navigasi Services → menuju halaman daftar vendor.
  - Tombol **Create PO** di halaman detail vendor → membuka drawer Purchase Order.
  - Tombol **Edit PO** di tabel aktivasi → membuka drawer edit PO.
  - Tombol **Preview PO** → membuka modal DocumentPreview dengan template PO.
  - Tombol **Approve/Reject** di halaman review PO → untuk proses persetujuan.

## 📖 Informasi & Tutorial Singkat Fitur

### Vendor Management & Purchase Order

**Penjelasan Fitur**:
Modul Vendor Management memungkinkan admin mengelola data vendor/rekanan beserta katalog layanan dan barang yang mereka sediakan. Fitur Purchase Order digunakan untuk membuat dokumen pemesanan resmi ke vendor dengan kalkulasi otomatis (subtotal, pajak, grand total) dan alur persetujuan (approval workflow). PO yang sudah disetujui dapat dibagikan ke vendor melalui tautan publik untuk ditandatangani secara digital.

**Langkah Penggunaan (Tutorial)**:

1. **Menambah Vendor**: Buka menu Services → Vendor → klik tombol tambah → isi nama, kode, kontak AM/NOC, alamat → simpan.
2. **Menambah Item Vendor**: Buka detail vendor → tab Katalog → tambah item (nama, tipe service/goods/contractor, harga, satuan) → simpan.
3. **Membuat Purchase Order**: Dari halaman detail vendor → klik "Buat PO" → pilih item dari katalog → atur qty dan harga → pilih tipe pajak (none/percentage/fixed) → cek ringkasan grand total → simpan.
4. **Mengajukan Persetujuan**: Buka PO yang sudah dibuat → klik "Ajukan" → notifikasi Telegram terkirim ke admin approval.
5. **Menyetujui PO**: Admin approval membuka link dari Telegram → klik "Setujui" atau "Tolak".
6. **Tanda Tangan Vendor**: Vendor membuka link publik PO → mengisi tanda tangan digital → PO berstatus "Selesai".

---

_Laporan ini dibuat otomatis oleh AI berdasarkan data Git dan perubahan kode pada branch `issue-104`._
