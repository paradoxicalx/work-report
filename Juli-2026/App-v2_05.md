# 📝 Daily Work Report - Dedy Putra (2026-07-05)

---

## 📌 Informasi Issue

- **Nomor Issue**: #118
- **Judul Issue**: Restrukturisasi Modul Vendor → Purchase Order & Sales Order

## 📅 Laporan Harian - 5 Juli 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

Tidak ada. Working tree dalam keadaan bersih (`nothing to commit, working tree clean`).

### 📅 Rincian Commit

#### [8a4bfde] - resolve #118 (Restrukturisasi Modul Vendor → Purchase Order & Sales Order)

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/files.controller.js`
  - `backend/src/controllers/publicPO.controller.js`
  - `backend/src/controllers/purchaseOrder.controller.js` [NEW]
  - `backend/src/controllers/salesOrder.controller.js` [NEW]
  - `backend/src/controllers/vendor.controller.js` [DELETED]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/purchaseOrder.route.js` [NEW]
  - `backend/src/routes/salesOrder.route.js` [NEW]
  - `backend/src/routes/vendor.route.js` [DELETED]
  - `backend/src/services/purchaseOrder.service.js` [NEW]
  - `backend/src/services/salesOrder.service.js` [NEW]
  - `backend/src/services/vendor.service.js` [DELETED]
  - `backend/src/utils/telegram.js`

- **Deskripsi Perubahan & Fungsi**:
  - **Restrukturisasi besar**: Modul Vendor yang semula monolitik dipecah menjadi dua modul mandiri: **Purchase Order (PO)** dan **Sales Order (SO)**.
  - **Backend — Controller Baru**:
    - `purchaseOrder.controller.js` (483 baris): Meng-handle seluruh logika bisnis Purchase Order (pembelian ke vendor). Mencakup CRUD PO, manajemen item, approval workflow, dan integrasi notifikasi Telegram.
    - `salesOrder.controller.js` (284 baris): Meng-handle seluruh logika bisnis Sales Order (penjualan ke customer). Mencakup CRUD SO, tracking status, dan integrasi dokumen.
  - **Backend — Service (DAL) Baru**:
    - `purchaseOrder.service.js` (276 baris): Data access layer khusus untuk koleksi Purchase Order.
    - `salesOrder.service.js` (233 baris): Data access layer khusus untuk koleksi Sales Order.
  - **Backend — Routes Baru**:
    - `purchaseOrder.route.js` (492 baris): Mendefinisikan semua endpoint REST API untuk modul Purchase Order.
    - `salesOrder.route.js` (320 baris): Mendefinisikan semua endpoint REST API untuk modul Sales Order.
  - **Backend — Penghapusan**: `vendor.controller.js` (751 baris), `vendor.route.js` (802 baris), dan `vendor.service.js` (479 baris) dihapus sepenuhnya karena logikanya sudah dipindahkan ke modul PO dan SO yang baru.
  - **Backend — app.js**: Registrasi route baru untuk PO dan SO, serta penghapusan registrasi route vendor lama.
  - **Backend — privilege.json**: Pembaruan daftar privilege untuk mencerminkan modul PO dan SO yang baru.
  - **Backend — files.controller.js & publicPO.controller.js**: Penyesuaian minor untuk kompatibilitas dengan struktur baru.
  - **Backend — telegram.js**: Penyesuaian notifikasi Telegram untuk mendukung workflow PO dan SO.
  - **Backend — i18n**: Pembaruan besar pada file terjemahan (en/id) — menambah 84+ key baru untuk modul PO dan SO, menghapus key vendor yang sudah tidak digunakan.
  - **Statistik**: +2.191 baris ditambahkan, -2.121 baris dihapus (selisih bersih +70 baris).

---

#### [09fcd74] - resolve #118 (Pembaruan Frontend Sales Order & Dokumen Publik)

- **Komponen yang Berubah**:
  - `backend/package-lock.json`
  - `backend/src/controllers/files.controller.js`
  - `backend/src/controllers/publicSO.controller.js`
  - `backend/src/controllers/vendor.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/services/vendor.service.js`
  - `backend/src/utils/telegram.js`
  - `frontend/package-lock.json`
  - `frontend/src/app/pages/public/PublicBAADocument.jsx`
  - `frontend/src/app/pages/public/PublicBADDocument.jsx`
  - `frontend/src/app/pages/public/PublicSODocument.jsx`
  - `frontend/src/app/pages/public/ReviewSOPage.jsx`
  - `frontend/src/app/pages/public/publicBAPDocument.jsx`
  - `frontend/src/app/pages/services/salesOrder/SODocumentPreview.jsx`
  - `frontend/src/app/pages/services/salesOrder/create.jsx`
  - `frontend/src/app/pages/services/salesOrder/edit.jsx`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/router/public.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Frontend — Public SO Document**: Perombakan besar pada `PublicSODocument.jsx` (127 perubahan) untuk menampilkan dokumen Sales Order publik dengan format baru yang lebih informatif.
  - **Frontend — Review SO Page & Document Preview**: Pembaruan tampilan review dan pratinjau dokumen SO untuk konsistensi dengan struktur data baru.
  - **Frontend — Form SO (Create & Edit)**: Penyesuaian form pembuatan dan pengeditan Sales Order (`create.jsx` 54 perubahan, `edit.jsx` 46 perubahan) mengikuti skema data PO/SO yang baru.
  - **Frontend — Public Document Pages**: Pembaruan pada `PublicBAADocument.jsx`, `PublicBADDocument.jsx`, dan `publicBAPDocument.jsx` untuk kompatibilitas dengan struktur SO baru.
  - **Frontend — Routing**: Penambahan rute baru di `protected.jsx` dan `public.jsx` untuk mengakomodasi halaman PO dan SO yang telah direstrukturisasi.
  - **Frontend — i18n**: Penambahan 3 key terjemahan baru (en & id) untuk label UI SO.
  - **Backend — publicSO.controller.js**: Penambahan 21 baris untuk endpoint publik Sales Order yang menangani tampilan dokumen SO oleh pihak eksternal.
  - **Backend — vendor.controller.js & vendor.service.js**: Penyesuaian minor (9 dan 35 perubahan) sebagai bagian transisi sebelum dihapus total di commit berikutnya.
  - **Backend — telegram.js**: Perbaikan notifikasi Telegram (+11 perubahan) untuk workflow SO.
  - **Backend — i18n**: Penambahan 14 key baru untuk pesan notifikasi dan response SO.
  - **Statistik**: +254 baris ditambahkan, -202 baris dihapus (selisih bersih +52 baris).

---

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**:
  - Admin kini dapat mengelola **Purchase Order** dan **Sales Order** sebagai modul terpisah yang mandiri, tidak lagi bergantung pada modul Vendor yang monolitik.
  - Setiap modul (PO & SO) memiliki controller, service, dan route sendiri sehingga lebih mudah di-maintain dan dikembangkan ke depannya.
  - Halaman dokumen publik (BA A, BA D, BAP, SO) kini menampilkan data dengan format yang lebih jelas dan terstruktur.
  - Form pembuatan dan pengeditan Sales Order telah disesuaikan dengan skema data baru yang lebih ringkas.

- **Bug Fix / Solusi Masalah**:
  - Memisahkan logika Vendor yang terlalu besar (monolitik) menjadi dua modul PO dan SO menghilangkan ketergantungan yang tidak perlu dan memudahkan debugging.
  - Notifikasi Telegram kini terpisah dengan jelas antara workflow PO (pembelian) dan SO (penjualan).

- **Menu/Tombol Baru**:
  - Rute protected baru untuk **Purchase Order** dan **Sales Order** telah didaftarkan di router, sehingga admin dapat mengakses menu PO dan SO secara langsung dari sidebar/navigasi.

## 📖 Informasi & Tutorial Singkat Fitur

- **Penjelasan Fitur**:
  - **Purchase Order (PO)** adalah modul untuk mengelola proses pembelian barang/jasa dari vendor. Mencakup pembuatan order, tracking status (pending/dikirim/diterima), dan notifikasi via Telegram.
  - **Sales Order (SO)** adalah modul untuk mengelola proses penjualan ke customer. Mencakup pembuatan order penjualan, tracking status, dan pembuatan dokumen pendukung (BA A, BA D, BAP).
  - Kedua modul ini sebelumnya tergabung dalam satu modul Vendor yang besar. Restrukturisasi ini memisahkan mereka agar lebih modular dan mudah dikelola.

- **Langkah Penggunaan (Tutorial)**:
  1. **Akses Purchase Order**: Buka menu "Purchase Order" di sidebar → Admin dapat membuat PO baru, melihat daftar PO, dan mengubah status PO.
  2. **Akses Sales Order**: Buka menu "Sales Order" di sidebar → Admin dapat membuat SO baru, mengedit SO yang ada, dan melihat pratinjau dokumen SO.
  3. **Dokumen Publik**: Customer dapat mengakses dokumen SO melalui link publik yang dibagikan, menampilkan detail order dalam format yang mudah dibaca.
  4. **Notifikasi**: Setiap perubahan status PO/SO akan mengirim notifikasi otomatis ke Telegram untuk memberi tahu pihak terkait.

---

## 🔍 Hasil Remediasi Audit Kode (Branch issue-118)

> **Total Item**: 21 | **Selesai**: 21 | **Tersisa**: 0

### 🔴 Prioritas KRITIS — Security & Data Leak (3/4 selesai)

| #   | Status     | Deskripsi                                                                                    | File                     |
| --- | ---------- | -------------------------------------------------------------------------------------------- | ------------------------ |
| #3  | ✅ Selesai | Batasi ukuran gambar tanda tangan (MAX 2 MB) untuk DoS prevention                            | `publicSO.controller.js` |
| #4  | ✅ Selesai | Validasi path tanda tangan — gunakan `startsWith('/admin-sign/')` untuk cegah path traversal | `publicSO.controller.js` |
| #28 | ✅ Selesai | Tambahkan privilege check pada route `/review/so/:id`                                        | `protected.jsx`          |

### 🟠 Prioritas TINGGI — Data Minimization & Payload (3/3 selesai)

| #   | Status     | Deskripsi                                                                                            | File                     |
| --- | ---------- | ---------------------------------------------------------------------------------------------------- | ------------------------ |
| #2  | ✅ Selesai | Batasi field vendor pada DTO endpoint publik — hanya `name` dan `code`                               | `publicSO.controller.js` |
| #6  | ✅ Selesai | Pisahkan field selection list vs detail — buat `VENDOR_SO_LIST_FIELDS` dan `VENDOR_SO_DETAIL_FIELDS` | `vendor.service.js`      |
| #7  | ✅ Selesai | Tambahkan `.lean()` pada `findSOsByVendor` untuk performa dan hindari overhead Mongoose              | `vendor.service.js`      |

### 🟡 Prioritas SEDANG — Error Handling & Logging (9/9 selesai)

| #   | Status     | Deskripsi                                                                                    | File                    |
| --- | ---------- | -------------------------------------------------------------------------------------------- | ----------------------- |
| #1  | ✅ Selesai | Ganti `console.log` dengan `logger.error` di `getVendorSOFile()`                             | `files.controller.js`   |
| #8  | ✅ Selesai | Throw error alih-alih return objek `{ error, message }` di `createNewVendorSO()`             | `vendor.service.js`     |
| #9  | ✅ Selesai | Ganti `console.warn`/`console.error` dengan `logger` di `TelegramNotifSOSubmit()`            | `telegram.js`           |
| #10 | ✅ Selesai | Sesuaikan controller `createSO` — hapus pengecekan `result.error`, gunakan try-catch standar | `vendor.controller.js`  |
| #16 | ✅ Selesai | Perbaiki error handling — gunakan `err.response?.data?.message \|\| t('global.anError')`     | `PublicSODocument.jsx`  |
| #18 | ✅ Selesai | Perbaiki error handling di `fetchDocument`, `executeApprove`, `executeReject`                | `ReviewSOPage.jsx`      |
| #19 | ✅ Selesai | Perbaiki error handling di `handleApprove`, `handleReject`                                   | `SOReviewDrawer.jsx`    |
| #22 | ✅ Selesai | Perbaiki error handling di `onSubmit()`                                                      | `salesOrder/create.jsx` |
| #24 | ✅ Selesai | Perbaiki error handling di `onSubmit()`                                                      | `salesOrder/edit.jsx`   |

### 🟢 Prioritas RENDAH — Standards & Performance (7/7 selesai)

| #   | Status     | Deskripsi                                                                                           | File                    |
| --- | ---------- | --------------------------------------------------------------------------------------------------- | ----------------------- |
| #13 | ✅ Selesai | Ganti `<button>` dan `<input type="file">` mentah dengan komponen UI (`Button`, `Upload`)           | `PublicSODocument.jsx`  |
| #17 | ✅ Selesai | Hapus fallback teks hardcoded `\|\| 'Unauthorized'`, pastikan key `global.unauthorized` ada di i18n | `ReviewSOPage.jsx`      |
| #21 | ✅ Selesai | Ganti `<button>` dan `<input type="file">` mentah dengan komponen UI standar                        | `salesOrder/create.jsx` |
| #23 | ✅ Selesai | Ganti `<input type="file">` dan tombol ganti dokumen dengan komponen UI standar                     | `salesOrder/edit.jsx`   |
| #14 | ✅ Selesai | Tambahkan useEffect cleanup untuk menghapus event listener window (`pointermove`/`pointerup`)       | `PublicSODocument.jsx`  |
| #15 | ✅ Selesai | Gunakan `useState` + event listener `resize` untuk `renderedWidth` yang reaktif                     | `PublicSODocument.jsx`  |
| #20 | ✅ Selesai | Tambahkan `AbortController` pada axios request + cleanup `controller.abort()`                       | `SODocumentPreview.jsx` |

### 📋 Catatan Tambahan

| #   | Status   | Keterangan                                                                                                |
| --- | -------- | --------------------------------------------------------------------------------------------------------- |
| #11 | ⚠️ Minor | `cleanFormData` menghapus `notes=''` — disengaja karena notes kosong tidak perlu disimpan                 |
| #12 | 💡 Saran | Submit SO menggunakan privilege `vendor.update` — evaluasi perlu privilege `vendor.submit` terpisah       |
| #25 | ✅ OK    | `soColumns.jsx` sudah mengikuti standar TanStack Table (accessor callback, visible: true, label = header) |
| #26 | ✅ OK    | RowActions di `soColumns.jsx` sudah menggunakan component Drawer, bukan url                               |
| #27 | ✅ OK    | Refaktor `VendorApprovalStatusCell` sudah baik dan reuse existing pattern                                 |
