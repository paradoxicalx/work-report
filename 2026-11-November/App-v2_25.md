# 📝 Daily Work Report - Dedy S.N Putra (2026-11-25)

---

## 📅 Laporan Harian - 25 November 2026

---

## 🌿 Branch: `issue-588` — Optimasi Query & Keamanan DTO Faktur Partner API

### 📌 Informasi Issue

- **Nomor Issue**: #588
- **Judul Issue**: Optimasi Query & Keamanan DTO Faktur Partner API
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [0ea722da] - resolve #588 - 2026-11-25 19:36:02

- **Komponen yang Berubah**:
  - `backend/src/controllers/partnerApiInvoice.controller.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/utils/partnerApiInvoiceInput.js`
  - `backend/test/integration/partnerApiInvoice.reporting.test.js`
  - `backend/test/integration/partnerApiInvoice.write.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Menggabungkan pengambilan ringkasan `payment` dan daftar `payments` faktur menjadi satu query lewat `loadInvoicePaymentDetails` serta menjalankan pemuatan detail pembayaran dan `issueDatePolicy` secara paralel (`Promise.all`) pada `readPartnerAppInvoice` dan `readPartnerAppCustomerInvoice`.
  - Membatasi proyeksi field pada `findScopedCustomerInvoice` di `listPartnerAppCustomerInvoiceUpdates` hanya mengambil `['_id', 'customer']`, serta menghapus `_id` internal pada `shapeCustomerInvoiceListItem`.
  - Memperketat validasi input di `partnerApiInvoiceInput.js` (`isNumberLike` memeriksa `Number.isFinite` dan menolak string kosong/NaN; `pickPartnerApiInvoiceInput` menolak body non-objek) dan memperbaiki logging/error handling di `findListCustomerInvoiceForPartnerApi` serta `findOneInternalCustomerInvoiceForPartnerApi`.
  - Menambahkan test integrasi di `partnerApiInvoice.reporting.test.js` dan `partnerApiInvoice.write.test.js`.

---

## 🌿 Branch: `issue-589` — Pembersihan & Penyelarasan Partner Portal (`partner-frontend`) serta Konfigurasi Deploy `mobile-frontend`

### 📌 Informasi Issue

- **Nomor Issue**: #589
- **Judul Issue**: Pembersihan & Penyelarasan Partner Portal (`partner-frontend`) serta Konfigurasi Deploy `mobile-frontend`
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [621d9cc8] - resolve #589 - 2026-11-25 16:38:58

- **Komponen yang Berubah**:
  - `mobile-frontend/.dockerignore` [NEW]
  - `mobile-frontend/Dockerfile` [NEW]
  - `mobile-frontend/nginx.conf` [NEW]
  - `mobile-frontend/vercel.json` [NEW]
  - `mobile-frontend/README.md`
  - `partner-frontend/src/app/navigation/` (`account.js`, `customers.js`, `dashboards.js`, `finance.js`, `index.js`, `organization.js` [NEW], `services.js`)
  - `partner-frontend/src/app/pages/customers/regular/` (`index.jsx` [NEW], `create.jsx` [NEW], `edit.jsx` [NEW], `view.jsx` [NEW]) & `customers/customer/customerKind.js` [NEW] (+ test)
  - `partner-frontend/src/app/pages/finance/customerInvoice/` (`InvoiceForm.jsx` [NEW], `create.jsx` [NEW], `edit.jsx` [NEW], `schema/invoiceSchema.js` [NEW], `index.jsx`, `detail.jsx`) & `finance/paymentAccount/index.jsx` [NEW]
  - `partner-frontend/src/app/pages/services/radiusProfile/` (`ProfileForm.jsx` [NEW], `index.jsx` [NEW], `create.jsx` [NEW], `edit.jsx` [NEW], `view.jsx` [NEW], `schema/columns.jsx` [NEW], `schema/profileSchema.js` [NEW]) & `broadbandPackage/PackageSubscribers.jsx` [NEW]
  - `partner-frontend/src/app/pages/organization/` (`partner/index.jsx` [NEW], `partner/view.jsx` [NEW], `staff/index.jsx` [NEW], `staff/admin.jsx` [NEW], `staff/employee.jsx` [NEW], `privilege/index.jsx` [NEW])
  - `partner-frontend/src/app/pages/statistic/index.jsx` [NEW]
  - `partner-frontend/src/app/pages/network/fiberCable/` & `hooks/useFiberEditable.js` [NEW] (+ test)
  - `partner-frontend/src/app/router/` (`regularRoute.jsx` [NEW], `customerInvoiceFormRoute.jsx` [NEW], `paymentAccountRoute.jsx` [NEW], `organizationRoute.jsx` [NEW], `radiusProfileRoute.jsx` [NEW], `protected.jsx`)
  - `partner-frontend/src/components/shared/` (`InfoDrawer.jsx` [NEW], `PartnerScopeBar.jsx` [NEW], `PartnerSelect.jsx` [NEW], `SiteMap.jsx`, `table/rows.jsx`)
  - `partner-frontend/src/hooks/` (`useFinanceScope.js` [NEW], `useHomeSummaries.js` [NEW], `useUserType.js` [NEW], `useMenuTree.js`)
  - `partner-frontend/src/utils/` (`customerInvoice.js` [NEW], `homeSummary.js` [NEW], `ipPool.js` [NEW], `organization.js` [NEW], `partnerApiOnly.guard.test.js` [NEW], `partnerSelect.js` [NEW], `radiusProfile.js` [NEW], `siteAccess.js` [NEW], `userType.js` [NEW], beserta file test masing-masing)
  - `partner-frontend/src/i18n/locales/en/translations.json`, `partner-frontend/src/i18n/locales/id/translations.json`
  - Pembersihan 1.070 berkas modul internal yang tidak digunakan di `partner-frontend` (modul `activities`, `archive`, `hr`, dsb.)
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan konfigurasi deployment produksi untuk `mobile-frontend` (`Dockerfile`, `.dockerignore`, `nginx.conf`, `vercel.json`) beserta pembaruan panduan di `mobile-frontend/README.md`.
  - Melakukan pembersihan menyeluruh pada `partner-frontend` dengan menghapus 1.070 berkas modul internal/admin non-partner serta memastikan seluruh halaman Portal Mitra hanya memanggil Partner API (`/p-api/v1`) yang dikunci melalui `partnerApiOnly.guard.test.js`.
  - Menambahkan dan menyelaraskan modul-modul inti Portal Mitra di `partner-frontend`: halaman Statistik (`statistic/index.jsx`), Beranda (`useHomeSummaries.js`, `homeSummary.js`), Pelanggan Reguler (`customers/regular/*`), Form CRUD Faktur Pelanggan & Akun Pembayaran (`finance/customerInvoice/*`, `finance/paymentAccount`), Profil RADIUS & Daftar Pelanggan Paket (`services/radiusProfile/*`, `PackageSubscribers.jsx`), serta modul Organisasi (`organization/partner/*`, `staff/*`, `privilege/*`).
  - Menambahkan unit test untuk seluruh utilitas baru (`customerInvoice.test.js`, `homeSummary.test.js`, `ipPool.test.js`, `organization.test.js`, `partnerSelect.test.js`, `radiusProfile.test.js`, `siteAccess.test.js`, `userType.test.js`, `customerKind.test.js`, `useFiberEditable.test.js`).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                            | Dampak Utama                                                                                                                |
| ----- | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| #588  | Optimasi Query & Keamanan DTO Faktur Partner API                                 | Pembacaan detail faktur Partner API lebih cepat (1 query payment + paralel) dengan validasi input dan DTO lebih bersih.     |
| #589  | Pembersihan & Penyelarasan `partner-frontend` serta Deploy `mobile-frontend`     | `mobile-frontend` siap dideploy via Docker/Nginx/Vercel, dan `partner-frontend` bersih serta terfokus pada fitur Portal Mitra. |

### Kemampuan Baru Pengguna/Admin

- Mitra dan Admin dapat mengelola Profil RADIUS, Faktur Pelanggan (buat/ubah), Akun Pembayaran, Pelanggan Reguler, Statistik, serta melihat data Organisasi (Mitra/Staf/Hak Akses) langsung melalui `partner-frontend`.
- Tim DevOps dapat men-deploy `mobile-frontend` menggunakan `Dockerfile` + `nginx.conf` atau `vercel.json`.

### Bug Fix / Solusi Masalah

- Mengurangi query duplikat ke koleksi `Payment` pada endpoint baca detail faktur Partner API dan menghilangkan field `_id` internal pada daftar faktur pelanggan.
- Memperketat validasi angka (`Number.isFinite`) dan payload objek pada input pembuatan/pembaruan faktur Partner API agar input tidak valid langsung ditolak.

### Menu/Fitur Baru

- Menu & halaman baru di `partner-frontend`: **Statistik**, **Pelanggan Reguler**, **Akun Pembayaran**, **Profil RADIUS**, serta grup menu **Organisasi** (Mitra, Staf, Hak Akses).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: `partner-frontend` kini disederhanakan dan diselaraskan khusus untuk Portal Mitra/POP yang berkomunikasi eksklusif dengan `/p-api/v1`, lengkap dengan manajemen Profil RADIUS, Faktur Pelanggan, Organisasi, dan Statistik.
- **Langkah Penggunaan (Tutorial)**:
  1. Untuk menjalankan/deploy `mobile-frontend` dengan Docker, bangun image menggunakan `mobile-frontend/Dockerfile` (menggunakan Nginx dengan konfigurasi SPA di `nginx.conf`), atau deploy langsung ke Vercel via `mobile-frontend/vercel.json`.
  2. Untuk mengakses fitur baru di `partner-frontend`, masuk ke Portal Mitra, lalu gunakan menu **Statistik**, **Layanan > Profil RADIUS**, **Keuangan > Akun Pembayaran / Faktur Pelanggan**, atau **Organisasi**.
