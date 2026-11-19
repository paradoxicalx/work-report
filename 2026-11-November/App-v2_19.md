# 📝 Daily Work Report - Dedy S.N Putra (2026-11-19)

---

## 📅 Laporan Harian - 19 November 2026

---

## 🌿 Branch: `master` / `production` (Integrasi `issue-571`) — Perbaikan Pencarian Pelanggan Reguler & Bisnis (`findOneAnyCustomer`) pada Tiket

### 📌 Informasi Issue

- **Nomor Issue**: #571
- **Judul Issue**: Perbaikan Lookup Pelanggan pada Pembuatan dan Pembaruan Tiket agar Mendukung Pelanggan Reguler maupun Bisnis (`findOneAnyCustomer`)
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` dan `origin/production` melalui commit `e05232a9`)

### 📅 Rincian Commit

#### [e05232a9] & [cfad6931] - resolve #571 - 19 November 2026, 17:25:22 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/ticket.controller.js`

- **Deskripsi Perubahan & Fungsi**:
  - Mengganti pemanggilan `findOneCustomer` menjadi `findOneAnyCustomer` pada fungsi `createTicket` dan `updateTicket` di `backend/src/controllers/ticket.controller.js` (untuk format `CUST-...` maupun string `customer_id` langsung).
  - Memastikan pencarian `customer_id` saat membuat atau memperbarui tiket mencakup seluruh entitas pelanggan yang valid (tidak terbatas pada satu jalur lookup saja) sehingga tiket untuk pelanggan yang bersangkutan dapat ditautkan dengan benar.

---

## 🌿 Branch: `issue-569` — Modul Keuangan & Tagihan (`mobile-frontend`) serta Penyelarasan Data Cetak & Filter Partner API Invoice

### 📌 Informasi Issue

- **Nomor Issue**: #569
- **Judul Issue**: Implementasi Modul E: Keuangan & Tagihan (Tagihan Mitra, Tagihan Pelanggan + Penerbitan/Cetak, Rekening) di `mobile-frontend` serta Penyelarasan Endpoint Partner API Invoice
- **Status Branch**: `Belum di-merge` (aktif di `origin/issue-569`)

### 📅 Rincian Commit

#### [d859ab3d] - resolve #569 - 19 November 2026, 12:28:27 WIB

- **Komponen yang Berubah**:
  - **Backend Partner API Invoice (`backend/*`)**:
    - `backend/src/controllers/partnerApiInvoice.controller.js`
    - `backend/src/routes/partnerApi.route.js`
    - `backend/src/services/financeInvoice.service.js`
    - `backend/test/integration/partnerApiInvoice.test.js`
  - **Halaman & Komponen Keuangan (`mobile-frontend/src/pages/finance/*`)**:
    - `mobile-frontend/src/pages/FinancePage.jsx`
    - `mobile-frontend/src/pages/finance/InvoiceDetailPage.jsx` [NEW]
    - `mobile-frontend/src/pages/finance/InvoiceFormPage.jsx` [NEW]
    - `mobile-frontend/src/pages/finance/components/AccountsTab.jsx` [NEW]
    - `mobile-frontend/src/pages/finance/components/CustomerInvoicesTab.jsx` [NEW]
    - `mobile-frontend/src/pages/finance/components/PartnerInvoicesTab.jsx` [NEW]
    - `mobile-frontend/src/pages/finance/components/InvoiceCard.jsx` [NEW]
    - `mobile-frontend/src/pages/finance/components/InvoicePrintDocument.jsx` [NEW]
    - `mobile-frontend/src/pages/finance/components/PartnerScopeBar.jsx` [NEW]
  - **Fitur, Service, Utilitas, & Terjemahan (`mobile-frontend/src/*`)**:
    - `mobile-frontend/src/features/finance/cacheKeys.js` [NEW], `invoiceDocument.js` [NEW], `invoicePayload.js` [NEW], `invoiceRoutes.js` [NEW], `invoiceSchema.js` [NEW], `invoiceStatus.js` [NEW], `invoiceTotals.js` [NEW], `listSort.js` [NEW], `partnerInvoiceList.js` [NEW], `paymentMethod.js` [NEW], `useFinanceScope.js` [NEW], `finance.test.js` [NEW]
    - `mobile-frontend/src/services/financeApi.js` [NEW], `mobile-frontend/src/services/endpoints.js`
    - `mobile-frontend/src/utils/clipboard.js` [NEW]
    - `mobile-frontend/src/App.jsx`, `mobile-frontend/src/index.css`, `mobile-frontend/src/data/mockData.js`
    - `mobile-frontend/src/components/ConfirmDialog.jsx`, `mobile-frontend/src/components/CustomerPicker.jsx`
    - `mobile-frontend/src/pages/customers/CustomerDetailPage.jsx`
    - `mobile-frontend/src/i18n/locales/en/translations.json`, `mobile-frontend/src/i18n/locales/id/translations.json`
    - `mobile-frontend/PRD.md`

- **Deskripsi Perubahan & Fungsi**:

  1. **Penyelarasan Partner API Invoice untuk Dokumen Cetak & Filter Pelanggan (`backend`)**:
     - Memperbarui `readPartnerAppInvoice` di `partnerApiInvoice.controller.js` agar menyertakan field dokumen cetak (`dpp`, `tax_amount`, `paid_total`, `tax_type`, `tax_inclusive`, `recipient_name`, `recipient_address`, `messages`, serta riwayat `payments` tanpa data kas sensitif via `loadInvoicePayments`).
     - Memperbaiki `listPartnerAppCustomerInvoice` agar parameter query opsional `customer_id` benar-benar menyaring tagihan milik satu pelanggan tertentu milik mitra (dan mengembalikan `400` jika format tidak valid), sehingga tab **Tagihan** di detail pelanggan tidak lagi menampilkan seluruh tagihan mitra.
     - Memperbarui `financeInvoice.service.js`, dokumentasi Swagger di `partnerApi.route.js`, serta integration test `partnerApiInvoice.test.js`.

  2. **Modul Keuangan & Tagihan Lengkap di `mobile-frontend` (`FinancePage`, Detail, Form, & Cetak Faktur)**:
     - Mengimplementasikan `FinancePage.jsx` dengan 3 tab utama: **Tagihan Pelanggan** (`CustomerInvoicesTab`), **Tagihan Mitra** (`PartnerInvoicesTab`), dan **Rekening Pembayaran** (`AccountsTab`), lengkap dengan `PartnerScopeBar` untuk pemilihan scope mitra bagi admin.
     - Membangun `InvoiceDetailPage.jsx`, `InvoiceFormPage.jsx` (pembuatan & pembaruan faktur pelanggan dengan kalkulasi DPP/pajak dan item layanan), serta komponen cetak faktur `InvoicePrintDocument.jsx` dan utilitas salin kode pembayaran `clipboard.js`.
     - Menghubungkan tab **Tagihan** pada `CustomerDetailPage.jsx` ke API nyata, menghapus data mock keuangan di `mockData.js`, serta menambahkan seluruh logika murni (`features/finance/*`) dan unit test `finance.test.js`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #571  | Perbaikan Lookup Pelanggan Tiket (`findOneAnyCustomer`) | Pembuatan dan pembaruan tiket di backend tidak lagi gagal menemukan pelanggan saat memakai `findOneAnyCustomer` |
| #569  | Modul Keuangan & Tagihan (`mobile-frontend`) & Partner API Invoice | Mitra & admin dapat memantau tagihan mitra/pelanggan, menerbitkan/mengubah tagihan pelanggan, melihat rekening, dan mencetak faktur langsung dari `mobile-frontend`; filter `customer_id` pada tab Tagihan detail pelanggan kini akurat |

### Kemampuan Baru Pengguna/Admin

- **Mitra & Admin (`mobile-frontend`)** dapat mengakses modul **Keuangan** secara penuh (daftar tagihan pelanggan & mitra, detail tagihan, cetak faktur, penerbitan/edit tagihan pelanggan, dan daftar rekening), serta melihat daftar tagihan khusus satu pelanggan di halaman detail pelanggan.

### Bug Fix / Solusi Masalah

- **Filter `customer_id` Diabaikan pada `listPartnerAppCustomerInvoice`**: Sebelumnya parameter `customer_id` hanya dihormati di lingkup internal sehingga tab Tagihan pada detail pelanggan menampilkan seluruh tagihan mitra; kini sudah disaring per pelanggan milik mitra dengan validasi string.
- **Lookup Pelanggan di `ticket.controller.js`**: `createTicket` dan `updateTicket` kini menggunakan `findOneAnyCustomer` agar pencarian pelanggan konsisten.

### Menu/Fitur Baru

- Modul **Keuangan & Tagihan** lengkap di `mobile-frontend` (`FinancePage`, `InvoiceDetailPage`, `InvoiceFormPage`, `InvoicePrintDocument`, tab **Tagihan Pelanggan**, **Tagihan Mitra**, dan **Rekening**).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **Modul Keuangan & Tagihan (`mobile-frontend`, `#569`)**: Memungkinkan mitra maupun admin memantau status pembayaran tagihan, membuat atau memperbarui tagihan pelanggan, menyalin kode pembayaran, dan mencetak dokumen faktur langsung dari antarmuka mobile.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka tab **Keuangan** di navigasi bawah `mobile-frontend`, lalu pilih tab **Tagihan Pelanggan**, **Tagihan Mitra**, atau **Rekening** (admin dapat memilih mitra melalui `PartnerScopeBar`).
  2. Ketuk salah satu kartu tagihan untuk melihat rincian pembayaran dan mencetak dokumen faktur, atau ketuk **+** pada tab **Tagihan Pelanggan** untuk menerbitkan tagihan baru.
