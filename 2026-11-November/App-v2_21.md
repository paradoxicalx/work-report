# 📝 Daily Work Report - Dedy S.N Putra (2026-11-21)

---

## 📅 Laporan Harian - 21 November 2026

---

## 🌿 Branch: `issue-578` — Filter Autocomplete Kolom Tabel & Perbaikan Pemilihan Pelanggan/Paket Perubahan Layanan

### 📌 Informasi Issue

- **Nomor Issue**: #578
- **Judul Issue**: Filter Autocomplete Kolom Tabel & Perbaikan Pemilihan Pelanggan/Paket Perubahan Layanan
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [409b62aa] - resolve #578 - 2026-11-21 19:21:57

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/mobileServiceChange.controller.js`
  - `backend/src/controllers/productBroadband.controller.js`
  - `backend/src/controllers/vendor.controller.js`
  - `backend/src/routes/productBroadband.route.js`
  - `backend/src/services/admin.service.js`
  - `backend/src/services/customer.service.js`
  - `backend/src/services/financeExpense.service.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/src/services/mobileServiceChange.service.js`
  - `backend/src/services/productBroadband.service.js`
  - `backend/src/services/serviceChangeDocument.service.js`
  - `backend/src/services/serviceChangeRequest.service.js`
  - `backend/src/services/vendor.service.js`
  - `backend/src/utils/data-table.js`
  - `backend/src/utils/party-search.js`
  - `backend/test/integration/financeGateway.tableFilter.test.js`
  - `backend/test/integration/financePayment.tableFilter.test.js`
  - `backend/test/integration/mobileNotification.service.test.js`
  - `backend/test/integration/partyFilterEndpoints.test.js` [NEW]
  - `backend/test/integration/serviceChangeDocument.test.js`
  - `backend/test/integration/serviceChangeRequest.admin.test.js`
  - `backend/test/unit/dataTableAutocompleteFilter.test.js`
  - `backend/test/unit/partySearch.test.js`
  - `frontend/src/app/pages/activities/attendanceDevice/schema/mappingColumns.jsx`
  - `frontend/src/app/pages/mobileApp/serviceChange/components/CreateWebRequestModal.jsx`
  - `frontend/src/app/pages/mobileApp/serviceChange/components/WebRequestDrawer.jsx`
  - `frontend/src/app/pages/network/acsDevices/schema/columns.jsx`
  - `frontend/src/app/pages/users/document/serviceChange/CreateDrawer.jsx`
  - `frontend/src/app/pages/users/document/serviceChange/schema/columns.jsx`
  - `frontend/src/components/shared/table/TextAutocompleteFilter.jsx`
  - `frontend/src/components/shared/table/TextAutocompleteFilter.test.jsx`
  - `frontend/src/components/shared/table/allAutocompleteColumns.test.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - Memperketat utilitas `data-table.js` agar nilai teks filter autocomplete dipotong maksimal 100 karakter sebelum diubah menjadi regex, serta mendukung objek filter `{ text, id }` pada kolom bertipe non-string (`$toString` + `$regexMatch`).
  - Memperluas dukungan pencarian dan pemilihan pelanggan mitra serta paket broadband turunan mitra (`findOneProductBroadbandAnyOwner`, `findMultipleProductBroadbandAnyOwner`) pada alur perubahan layanan admin (`mobileServiceChange`, `serviceChangeRequest`, `productBroadband/select`, dan `CreateWebRequestModal.jsx`).
  - Memperbarui endpoint `/customer/user-select` pada modal pembuatan permintaan perubahan layanan agar admin dapat memilih pelanggan reguler maupun pelanggan mitra (dengan fallback pembacaan `/customer/read` dan `/customer-partner/read` sesuai hak akses).
  - Menambahkan dan memperbarui test integrasi serta unit (`partyFilterEndpoints.test.js`, `serviceChangeRequest.admin.test.js`, `financeGateway.tableFilter.test.js`, `financePayment.tableFilter.test.js`, dan test komponen frontend) serta memperbarui dokumentasi aturan di `AGENTS.md`.

#### [8a079719] - resolve #578 - 2026-11-21 16:18:50

- **Komponen yang Berubah**:
  - `backend/src/controllers/customerPKS.controller.js`
  - `backend/src/controllers/financeInvoice.controller.js`
  - `backend/src/controllers/financeJournal.controller.js`
  - `backend/src/controllers/financeLedger.controller.js`
  - `backend/src/controllers/financeTransaction.controller.js`
  - `backend/src/controllers/mobileServiceChange.controller.js`
  - `backend/src/controllers/productBroadband.controller.js`
  - `backend/src/controllers/radiusSession.controller.js`
  - `backend/src/routes/productBroadband.route.js`
  - `backend/src/services/acs.service.js`
  - `backend/src/services/admin.service.js`
  - `backend/src/services/customer.service.js`
  - `backend/src/services/financeCoa.service.js`
  - `backend/src/services/financeExpense.service.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/src/services/hotspotUser.service.js`
  - `backend/src/services/mobileNotification.service.js`
  - `backend/src/services/vendor.service.js`
  - `backend/src/utils/data-table.js`
  - `backend/src/utils/party-search.js`
  - `backend/test/unit/dataTableAutocompleteFilter.test.js` [NEW]
  - `backend/test/unit/partySearch.test.js` [NEW]
  - `frontend/src/app/pages/activities/attendanceDevice/schema/mappingColumns.jsx`
  - `frontend/src/app/pages/customerService/whatsappReport/components/ReportAdminTable.jsx`
  - `frontend/src/app/pages/dashboards/radius/schema/sessionColumns.jsx`
  - `frontend/src/app/pages/finance/accounts/schema/ledgerColumns.jsx`
  - `frontend/src/app/pages/finance/coa/detail/schema/columns.jsx`
  - `frontend/src/app/pages/finance/expenses/schema/columns.jsx`
  - `frontend/src/app/pages/finance/gateway/schema/columns.jsx`
  - `frontend/src/app/pages/finance/invoices/payments/schema/columns.jsx`
  - `frontend/src/app/pages/finance/invoices/schema/columns.jsx`
  - `frontend/src/app/pages/finance/journals/schema/columns.jsx`
  - `frontend/src/app/pages/finance/ledger/schema/columns.jsx`
  - `frontend/src/app/pages/finance/reports/InputTaxRecap.jsx`
  - `frontend/src/app/pages/finance/reports/TaxRecap.jsx`
  - `frontend/src/app/pages/finance/transactions/schema/columns.jsx`
  - `frontend/src/app/pages/finance/transactions/schema/draftColumns.jsx`
  - `frontend/src/app/pages/finance/wallet/schema/columns.jsx`
  - `frontend/src/app/pages/mobileApp/notification/schema/columns.jsx`
  - `frontend/src/app/pages/mobileApp/serviceChange/schema/columns.jsx`
  - `frontend/src/app/pages/mobileApp/serviceChange/schema/webColumns.jsx`
  - `frontend/src/app/pages/network/acsDevices/schema/columns.jsx`
  - `frontend/src/app/pages/network/oltOnt/components/config/schema/auditColumns.jsx`
  - `frontend/src/app/pages/network/radiusSession/schema/columns.jsx`
  - `frontend/src/app/pages/services/hotspotUser/schema/columns.jsx`
  - `frontend/src/app/pages/users/document/pks/schema/columns.jsx`
  - `frontend/src/app/pages/users/document/serviceChange/schema/columns.jsx`
  - `frontend/src/components/shared/table/ColumnFilter.jsx`
  - `frontend/src/components/shared/table/TextAutocompleteFilter.jsx` [NEW]
  - `frontend/src/components/shared/table/TextAutocompleteFilter.test.jsx` [NEW]
  - `frontend/src/components/shared/table/allAutocompleteColumns.test.jsx` [NEW]
  - `frontend/src/components/shared/table/autocompleteUtils.js` [NEW]
  - `frontend/src/utils/react-table/fuzzyFilter.js`
- **Deskripsi Perubahan & Fungsi**:
  - Membangun komponen filter kolom tabel baru `TextAutocompleteFilter` beserta `autocompleteUtils.js` dan mengintegrasikannya ke `ColumnFilter.jsx` serta `fuzzyFilter.js`.
  - Mengganti filter teks biasa menjadi `text-autocomplete` di seluruh schema tabel terkait (Finance, Radius, ACS/OLT, Hotspot, PKS, Service Change, WhatsApp Report, Mobile Notification, dan Attendance Device).
  - Menambahkan dukungan parsing nilai filter autocomplete `{ text, id, partyType }` pada `party-search.js` (`parsePartyFilterValue`, `hasPartyFilterValue`, `resolveExactPartyObjectId`, `resolveCustomerFilterIds`) dan `data-table.js` agar pemilihan item dari dropdown melakukan pencocokan eksak (`^...$` atau `_id`) sehingga tidak menarik record lain yang hanya mirip secara substring.
  - Menambahkan unit test backend (`dataTableAutocompleteFilter.test.js`, `partySearch.test.js`) dan test frontend (`TextAutocompleteFilter.test.jsx`, `allAutocompleteColumns.test.jsx`).

---

## 🌿 Branch: `issue-576` — Perbaikan Validasi Tanggal Mutasi, Unggah Dokumen Mitra & Penampil PDF Mobile

### 📌 Informasi Issue

- **Nomor Issue**: #576
- **Judul Issue**: Perbaikan Validasi Tanggal Mutasi, Unggah Dokumen Mitra & Penampil PDF Mobile
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [45c8bacb] - resolve #576 - 2026-11-21 15:21:10

- **Komponen yang Berubah**:
  - `backend/src/controllers/partnerApiPartner.controller.js`
  - `backend/src/controllers/partnerDocument.controller.js`
  - `backend/src/services/financeTransfer.service.js`
  - `backend/src/services/partnerDocument.service.js`
  - `backend/test/integration/financeTransfer.approval.test.js`
  - `backend/test/integration/partnerApiPartner.uploadDocuments.test.js`
  - `mobile-frontend/src/components/PdfViewer.jsx`
  - `mobile-frontend/src/pages/ProfilePage.jsx`
  - `mobile-frontend/src/pages/profile/DocumentsPage.jsx`
  - `mobile-frontend/src/services/accountApi.js`
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan validasi tanggal mutasi (`assertJournalDateAllowed`) langsung saat pembuatan transfer (`createNewTransfer`) di `financeTransfer.service.js` sehingga transfer `pending_approval` bertanggal tidak valid (masa depan/periode tertutup) langsung ditolak di awal dan tidak gagal diam-diam menjadi `canceled` saat disetujui.
  - Memperbaiki pembersihan berkas temporer (`cleanupTempFile`) pada seluruh jalur gagal di `setPartnerAppDocument`, menambahkan pengecekan `file.truncated` pada `partnerDocument.controller.js`, serta membatasi proyeksi `.select('name label file type mime created_at')` pada `findPartnerDocumentsLegacyShape`.
  - Memperbaiki manajemen state di `mobile-frontend` (`DocumentsPage.jsx` mendukung pembatalan unduhan via `AbortController`, melacak status buka/unduh per-file dengan `Set`, menjaga referensi `pendingUploadTypeRef` saat ganti dokumen; `PdfViewer.jsx` mereset `pageCount` dan `failed` saat `file` berubah; `ProfilePage.jsx` membungkus `logout()` dalam `try-finally` agar state `loggingOut` selalu pulih).
  - Menambahkan test integrasi untuk validasi tanggal transfer dan unggah dokumen mitra.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                              | Dampak Utama                                                                                                                |
| ----- | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| #578  | Filter Autocomplete Kolom Tabel & Pemilihan Pelanggan/Paket Perubahan Layanan      | Filter kolom tabel mendukung autocomplete dengan pencocokan eksak, serta alur perubahan layanan mendukung pelanggan & paket mitra. |
| #576  | Validasi Tanggal Mutasi, Unggah Dokumen Mitra & Penampil PDF Mobile                | Pengajuan mutasi bertanggal salah ditolak sejak awal, unggah dokumen mitra bebas file yatim, dan UI dokumen/PDF mobile lebih stabil. |

### Kemampuan Baru Pengguna/Admin

- Admin dapat memfilter kolom tabel (pihak terkait, pelanggan, admin, paket, dll.) menggunakan dropdown autocomplete dengan pencocokan eksak sehingga hasil tidak tercampur oleh nama yang mirip secara substring.
- Admin dapat memilih pelanggan mitra beserta paket broadband turunan mitra saat membuat atau memperbarui permintaan perubahan layanan.

### Bug Fix / Solusi Masalah

- Mencegah transfer kas/bank berstatus `pending_approval` yang bertanggal tidak valid lolos saat pengajuan lalu otomatis menjadi `canceled` saat disetujui.
- Mencegah kebocoran berkas temporer di disk ketika validasi unggah dokumen mitra di Partner API gagal, serta memastikan error `file.truncated` ditolak dengan status `400`.
- Memperbaiki state loading/abort unduhan dokumen, reset state `PdfViewer` saat berganti berkas, dan pemulihan tombol logout di `mobile-frontend`.

### Menu/Fitur Baru

- Komponen filter tabel `TextAutocompleteFilter` pada berbagai halaman tabel utama di dashboard admin (`frontend`).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Filter kolom tabel pada dashboard admin kini dilengkapi fitur `TextAutocompleteFilter` yang menampilkan saran pilihan saat pengguna mengetik. Jika pengguna memilih item dari dropdown, sistem mengirimkan identitas eksak (`id` & `partyType`) sehingga backend maupun filter client-side hanya menampilkan data milik entitas tersebut.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka salah satu halaman tabel di dashboard admin (misalnya Transaksi Keuangan, Jurnal, Faktur, Sesi RADIUS, atau Permintaan Perubahan Layanan).
  2. Klik ikon/input filter pada kolom yang mendukung autocomplete (seperti Pihak Terkait, Pelanggan, atau Admin).
  3. Ketik beberapa huruf untuk memunculkan daftar saran, lalu klik salah satu item untuk menyaring data secara tepat (atau tekan Enter untuk mencari berdasarkan teks bebas).
