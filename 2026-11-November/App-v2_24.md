# 📝 Daily Work Report - Dedy S.N Putra (2026-11-24)

---

## 📅 Laporan Harian - 24 November 2026

---

## 🌿 Branch: `issue-586` — Endpoint Ringkasan Beranda Partner API & Optimasi Revalidasi Cache Mobile

### 📌 Informasi Issue

- **Nomor Issue**: #586
- **Judul Issue**: Endpoint Ringkasan Beranda Partner API & Optimasi Revalidasi Cache Mobile
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [54359aa4] - resolve #586 - 2026-11-24 18:38:38

- **Komponen yang Berubah**:
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/partnerApiCustomer.controller.js`
  - `backend/src/controllers/partnerApiInvoice.controller.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/customer.service.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/test/integration/partnerApiCustomer.listStatus.test.js`
  - `backend/test/integration/partnerApiInvoice.test.js`
  - `mobile-frontend/PRD.md`
  - `mobile-frontend/src/features/authSlice.js`
  - `mobile-frontend/src/features/authSlice.test.js`
  - `mobile-frontend/src/features/home/homeSummary.js`
  - `mobile-frontend/src/features/home/homeSummary.test.js`
  - `mobile-frontend/src/features/home/revalidation.js` [NEW]
  - `mobile-frontend/src/features/home/revalidation.test.js` [NEW]
  - `mobile-frontend/src/hooks/useHomeSummary.js`
  - `mobile-frontend/src/pages/HomePage.jsx`
  - `mobile-frontend/src/services/customerApi.js`
  - `mobile-frontend/src/services/endpoints.js`
  - `mobile-frontend/src/services/financeApi.js`
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan endpoint ringkasan ringan Partner API `GET /p-api/v1/customer-partners/summary` (`getPartnerAppCustomerPartnerSummary` & `findCustomerSummaryForPartnerApi`) yang menghitung angka per kategori di database melalui `buildPartnerApiStatusFilters` dan hanya mengirim maksimal 4 pelanggan terbaru.
  - Menambahkan endpoint ringkasan faktur Partner API `GET /p-api/v1/invoices/summary` (`summaryPartnerAppInvoice` & `summarizeInvoicesForPartnerApi`) yang mengirim angka agregat dan 4 faktur terbaru tanpa memuat seluruh daftar faktur.
  - Memperbarui `useHomeSummary.js`, `homeSummary.js`, `customerApi.js`, dan `financeApi.js` di `mobile-frontend` agar Beranda mengonsumsi kedua endpoint ringkasan tersebut serta mengekstrak logika kunci cache dan revalidasi ke `revalidation.js` (`buildSummaryCacheKey`, `shouldRevalidate`) agar efek revalidasi hanya berjalan saat status cache berubah.
  - Memperbarui `privilegeDictionary.json`, dokumentasi Swagger di `partnerApi.route.js`, `PRD.md`, serta menambahkan test integrasi dan unit (`partnerApiCustomer.listStatus.test.js`, `partnerApiInvoice.test.js`, `revalidation.test.js`, `homeSummary.test.js`, `authSlice.test.js`).

---

## 🌿 Branch: `issue-582` — Endpoint Daftar Kartu Mitra Partner API & Manajemen Memori Pratinjau Blob

### 📌 Informasi Issue

- **Nomor Issue**: #582
- **Judul Issue**: Endpoint Daftar Kartu Mitra Partner API & Manajemen Memori Pratinjau Blob
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [4d90781e] - resolve #582 - 2026-11-24 19:04:27

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/partnerApiPartner.controller.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/partner.service.js`
  - `backend/test/integration/partnerApiPartner.adminScope.test.js`
  - `backend/test/integration/partnerApiPartner.listCard.test.js` [NEW]
  - `mobile-frontend/PRD.md`
  - `mobile-frontend/src/hooks/useDocumentFiles.js`
  - `mobile-frontend/src/pages/organization/components/OrganizationList.jsx`
  - `mobile-frontend/src/services/endpoints.js`
  - `mobile-frontend/src/services/partnerApi.js`
  - `mobile-frontend/src/utils/blobFile.js`
  - `mobile-frontend/src/utils/blobFile.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan endpoint `POST /p-api/v1/partners/list-card` (`cardPartnerAppPartner` dengan `PARTNER_API_CARD_FIELDS`) untuk daftar kartu mitra yang hanya mengirim field ringkas (`partner_id`, `name`, `status`, `area`, `city`, `regency`, `created_at`) tanpa kontak dan koordinat.
  - Membatasi query dokumen pada `adminProfileOptions` agar hanya dijalankan bila diminta eksplisit (`?include=documents`) dan berhak (`partnerDocument.read`), mewajibkan `partner_id` pada `getPartnerAppDocuments` untuk admin (`400`), serta memisahkan penanganan `CastError` (`warn` -> `partner.notFound`) dari error sistem (`error` -> `global.getDataFailed`) di `findOnePartnerForPartnerApi`.
  - Menambahkan `swapPreview` di `blobFile.js` dan menerapkannya di `useDocumentFiles.js` untuk mencabut Blob URL pratinjau gambar sebelumnya (`URL.revokeObjectURL`) sebelum diganti agar tidak terjadi kebocoran memori, serta menghubungkan `OrganizationList.jsx` ke `partners/list-card` dan `activateOnKey`.
  - Menambahkan unit & integration test (`partnerApiPartner.listCard.test.js`, `partnerApiPartner.adminScope.test.js`, `blobFile.test.js`), Swagger docs, serta memperbarui `AGENTS.md` dan `PRD.md`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                        | Dampak Utama                                                                                                                |
| ----- | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| #586  | Endpoint Ringkasan Beranda Partner API & Optimasi Revalidasi Cache Mobile    | Beranda aplikasi mobile jauh lebih hemat data dan cepat karena hanya mengambil agregat + 4 item terbaru, dengan revalidasi cache terarah. |
| #582  | Endpoint Daftar Kartu Mitra Partner API & Manajemen Memori Pratinjau Blob    | Daftar mitra di modul Organisasi mobile lebih ringan & minim ekspos data kontak, serta pratinjau dokumen bebas kebocoran Blob URL. |

### Kemampuan Baru Pengguna/Admin

- Aplikasi mobile memuat ringkasan pelanggan mitra dan faktur mitra di Beranda melalui endpoint ringkasan khusus (`/customer-partners/summary` dan `/invoices/summary`) tanpa mengunduh seluruh daftar penuh.
- Modul Organisasi pada aplikasi mobile menggunakan endpoint daftar kartu mitra (`/partners/list-card`) yang lebih ringan untuk menampilkan kartu mitra.

### Bug Fix / Solusi Masalah

- Mencegah kebocoran memori Blob URL di `useDocumentFiles.js` ketika membuka pratinjau gambar berulang kali atau berturut-turut.
- Memperbaiki penanganan error di `getPartnerAppDocuments` (mengembalikan `400` jika admin tidak mengirim `partner_id`) dan `findOnePartnerForPartnerApi` (tidak menyamarkan error sistem sebagai `partner.notFound`).
- Menghindari query dokumen yang tidak perlu pada detail profil mitra Partner API kecuali diminta eksplisit dengan `?include=documents`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Partner API kini menyediakan endpoint agregasi ringan (`/p-api/v1/customer-partners/summary`, `/p-api/v1/invoices/summary`, dan `/p-api/v1/partners/list-card`) sehingga halaman Beranda dan Organisasi di `mobile-frontend` tidak perlu menarik seluruh dokumen pelanggan, faktur, maupun detail kontak mitra.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka aplikasi `mobile-frontend` dan masuk sebagai Mitra atau Admin yang memiliki hak akses terkait.
  2. Halaman **Beranda** otomatis memuat ringkasan pelanggan dan faktur melalui endpoint `/summary`, dan melakukan revalidasi latar belakang hanya bila cache telah melewati batas waktu.
  3. Buka menu **Organisasi** untuk melihat daftar mitra ringkas dari `/partners/list-card`, lalu pilih salah satu kartu untuk membuka detail dan pratinjau dokumen dengan manajemen memori Blob otomatis.
