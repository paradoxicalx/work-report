# 📝 Daily Work Report - Dedy S.N Putra (11 November 2026)

---

## 📅 Laporan Harian - 11 November 2026

> **Catatan Status Kerja:** Tidak ada perubahan yang belum di-*commit* pada working tree aktif (`master`), dan tidak ada commit baru pada branch remote `issue-523`, `issue-477`, `issue-440`, maupun `issue-242` hari ini. Seluruh pekerjaan hari ini telah di-*merge* ke `master` dan tersinkron ke `production`.

---

## 🌿 Branch: `issue-536` — Integrasi Payment Gateway Midtrans (Core API), Koreksi Fee Manual, dan Penjaga Mode Sandbox/Live

### 📌 Informasi Issue

- **Nomor Issue**: #536
- **Judul Issue**: Integrasi Payment Gateway Midtrans (Core API), Koreksi Fee Manual Per Transaksi, dan Penjaga Mode Sandbox/Live
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` & `production` pada Rabu, 11 November 2026, 16:03:51 WIB via commit `a6a86030`)

### 📅 Rincian Commit

#### [69420e4d] (merge: [a6a86030]) - resolve #536 - 11 November 2026, 15:53:30 WIB (merge 16:03:51 WIB)

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/midtransChannels.json` [NEW]
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/financeGateway.controller.js`
  - `backend/src/controllers/financeSettings.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/routes/financeGateway.route.js`
  - `backend/src/routes/financeSettings.route.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeGatewayFee.service.js`
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/paymentGateways/midtrans.client.js` [NEW]
  - `backend/src/services/paymentGateways/midtrans.gateway.js` [NEW]
  - `backend/src/services/paymentGateways/midtransCatalog.js` [NEW]
  - `backend/src/services/paymentGateways/registry.js`
  - `backend/src/services/paymentGateways/xendit.gateway.js`
  - `backend/src/services/settingsBackup.registry.js`
  - `backend/src/utils/finance-error.js`
  - `backend/src/utils/midtransFee.js` [NEW]
  - `backend/test/integration/financeGateway.callbackEvents.test.js`
  - `backend/test/integration/financeGateway.reuseAcrossMode.test.js` [NEW]
  - `backend/test/integration/financeGateway.verifyModeMismatch.test.js` [NEW]
  - `backend/test/integration/financeSettings.midtrans.test.js` [NEW]
  - `backend/test/integration/midtransClient.test.js` [NEW]
  - `backend/test/integration/midtransConfig.test.js` [NEW]
  - `backend/test/integration/midtransGateway.adapter.test.js` [NEW]
  - `backend/test/integration/midtransGateway.e2e.test.js` [NEW]
  - `backend/test/integration/midtransGateway.failover.test.js` [NEW]
  - `backend/test/integration/midtransGateway.feeCorrection.test.js` [NEW]
  - `backend/test/unit/midtransCatalog.test.js` [NEW]
  - `backend/test/unit/midtransFee.test.js` [NEW]
  - `backend/test/unit/midtransGateway.normalize.test.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/FeeCorrectionModal.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/detail.jsx`
  - `frontend/src/app/pages/finance/gateway/utils/channelPanel.js`
  - `frontend/src/app/pages/finance/gateway/utils/feeCorrection.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/feeCorrection.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/Finance.jsx`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/MidtransGatewaySettings.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/gatewayProviders/MidtransGatewaySettings.test.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/gatewayProviders/index.js`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/providerLogic.js`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/providerLogic.test.js`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Adaptor Midtrans Core API (`midtrans.gateway.js`, `midtrans.client.js`, `midtransCatalog.js`, `midtransChannels.json`)**:
    - Mengimplementasikan adaptor payment gateway Midtrans berbasis Core API (`POST /v2/charge`) untuk kanal VA (`bca`, `bni`, `bri`, `permata`), `qris`, gerai ritel (`alfamart`, `indomaret`), dan e-wallet (`gopay`, `shopeepay`).
    - Menangani format gambar QR (`qr_image`) untuk QRIS/GoPay, mendeteksi transaksi asing (`is_foreign`) di akun Midtrans yang sama lewat pola `order_id` dan `custom_field1: 'dekasimal'`, serta mengonversi error HTTP 200 ber-`status_code >= 400` di body atau `error_messages[]` dari Midtrans menjadi error terstruktur.
    - Mengintegrasikan endpoint saldo Merchant Balance Mutation (`getBalance`) dengan penanganan khusus status 401/403 ke pesan ramah agar tidak memicu alert kritis palsu.
  - **Webhook & Rotasi Signature Secret Key (`POST /api/v1/finance/gateway/midtrans/callback`)**:
    - Memverifikasi keaslian notifikasi Midtrans via `signature_key` (`SHA512(order_id + status_code + gross_amount + key)`) dengan dukungan `midtrans_signature_secret_key` (kunci terpisah dari Server Key sejak kebijakan Midtrans 28 Oktober 2026) serta `midtrans_signature_secret_key_previous` untuk masa transisi rotasi kunci dan flag hapus eksplisit `midtrans_signature_secret_key_previous_clear`.
  - **Perkiraan Tarif Fee & Koreksi Fee Manual (`midtransFee.js`, `financeGatewayFee.service.js`, `FeeCorrectionModal.jsx`)**:
    - Karena API Midtrans tidak mengirim nominal fee per transaksi, sistem menghitung perkiraan fee otomatis dari tarif per kanal (`midtrans_fee_rates`) dan kebijakan akun PPN (`midtrans_fee_vat_account`: `bank_fee` atau `input_vat`).
    - Menambahkan endpoint `PATCH /api/v1/finance/gateway/transaction/:transaction_id/fee-correction` (`financeGateway.changeSensitive`) dan modal **Koreksi Fee** di drawer detail transaksi gateway untuk mengganti fee perkiraan dengan fee aktual dari laporan Midtrans (maksimal sekali per transaksi), membukukan selisihnya secara atomik bertanggal hari ini, dan ikut dibalik bila transaksi di-refund.
  - **Penjaga Mode Sandbox/Live (`getAccountMode`, `reuseAcrossMode`, `verifyModeMismatch`)**:
    - Mencegah cek ulang transaksi lintas mode (`GATEWAY_MODE_MISMATCH`, HTTP 409) ketika transaksi dibuat di mode sandbox/live yang berbeda dengan mode aktif saat ini, menutup otomatis transaksi sandbox yang kedaluwarsa saat gateway sudah live (`closeExpiredSandboxTransaction`), dan membatasi pemakaian ulang transaksi pending sesuai akun merchant aktif (`midtrans` vs `midtrans-sandbox`, `xendit` vs `xendit-sandbox`).
  - **UI Pengaturan & Backup**:
    - Menambahkan kartu konfigurasi `MidtransGatewaySettings.jsx` di **Pengaturan > Keuangan > Gerbang Pembayaran** (Server Key, Signature Secret Key & kunci lama, Mode Sandbox + deteksi ketidakcocokan awalan kunci, URL notifikasi, tarif fee perkiraan, pilihan akun PPN fee, dan status akun Kas & Bank otomatis), serta memperbarui `settingsBackup.registry.js`, terjemahan ID/EN, `privilegeDictionary.json`, dan suite unit/integration test.

---

## 🌿 Branch: `issue-539` — Modal Detail Pembayaran Tagihan, Perbaikan Urutan Cron Isolir & Tagihan Otomatis, serta Penataan Tabel Riwayat Pembayaran

### 📌 Informasi Issue

- **Nomor Issue**: #539
- **Judul Issue**: Modal Detail Pembayaran Tagihan, Perbaikan Urutan Cron Isolir & Tagihan Otomatis, serta Penataan Tabel Riwayat Pembayaran
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` & `production` pada Rabu, 11 November 2026, 16:02:48 WIB via commit `c60cce2e`)

### 📅 Rincian Commit

#### [1f19dad8] (merge: [c60cce2e]) - resolve #539 - 11 November 2026, 15:44:34 WIB (merge 16:02:48 WIB)

- **Komponen yang Berubah**:
  - `backend/src/controllers/financePayment.controller.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/src/services/invoiceFreeze.service.js`
  - `backend/src/utils/invoicePaymentEntries.js`
  - `backend/test/integration/financePayment.correction.test.js`
  - `backend/test/unit/invoicePaymentEntries.test.js`
  - `cron-worker/src/jobs/scheduler.js`
  - `frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`
  - `frontend/src/app/pages/finance/invoices/PaymentDetailModal.jsx` [NEW]
  - `frontend/src/app/pages/finance/invoices/PaymentReceiptDocument.jsx`
  - `frontend/src/app/pages/finance/invoices/detail.jsx`
  - `frontend/src/app/pages/finance/invoices/payments/schema/columns.jsx`
  - `frontend/src/app/pages/finance/invoices/payments/schema/columns.test.jsx`
  - `frontend/src/app/pages/finance/invoices/utils/paymentEntries.js`
  - `frontend/src/app/pages/finance/invoices/utils/paymentParty.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Endpoint & Modal Detail Pembayaran (`findOnePaymentFullDetail`, `PaymentDetailModal.jsx`)**:
    - Menambahkan fungsi `findOnePaymentFullDetail` pada `financePayment.service.js` dengan proyeksi field eksplisit (`PAYMENT_FULL_DETAIL_SELECT` — menghilangkan field internal seperti `_id`, `pid`, `deleted`, `client_ref`, `correction_plan`) dan `.populate()` terstruktur untuk faktur, pelanggan/mitra, akun kas, transaksi gateway, jurnal, info refund, serta riwayat cicilan.
    - Memperbarui `financePaymentDetail` di `financePayment.controller.js` agar memvalidasi `payment_id` (400 jika kosong) dan memanggil `findOnePaymentFullDetail`.
    - Membuat komponen `PaymentDetailModal.jsx` dan helper `paymentParty.js` agar admin dengan hak akses `financePayment.read` dapat membuka rincian lengkap pembayaran (termasuk ringkasan pembayaran, daftar cicilan/koreksi/void, info gateway, dan informasi refund) langsung dari ID pembayaran.
  - **Integrasi Tabel Riwayat Pembayaran & Drawer Detail Faktur (`columns.jsx`, `detail.jsx`, `InvoiceDocument.jsx`, `PaymentReceiptDocument.jsx`)**:
    - Mengubah kolom pertama pada tabel **Riwayat Pembayaran** menjadi `payment_id` yang memicu `PaymentDetailModal`, sementara kolom nama tagihan (`name`) memicu `InvoiceDetailDrawer`.
    - Menambahkan tombol klik ID pembayaran pada tabel riwayat pembayaran di `InvoiceDetailDrawer` (`detail.jsx`), menyertakan `payment_id` pada `buildInvoicePaymentEntries` (`invoicePaymentEntries.js`), dan memperbaiki tipe avatar pembuat/pembatal cicilan (`admin` vs `employee`).
  - **Perbaikan Urutan Jadwal Cron Isolir & Tagihan Otomatis (`cron-worker/src/jobs/scheduler.js`)**:
    - Menukar jadwal `invoiceFreeze` menjadi pukul **01:00** dan `financeAutoInvoiceGenerate` menjadi pukul **01:30** (serta mendaftarkan keduanya di `CURRENT_REPEAT_PATTERNS` dan `REFRESH_REPEATABLE_ON_BOOT`), sehingga akun RADIUS yang jatuh temponya baru lewat diisolir lebih dulu sebelum generator tagihan otomatis menyeleksi kandidat `status: true`.

---

## 🌿 Branch: `master` — Pembaruan Changelog & Basis Pengetahuan AI

### 📌 Informasi Commit

- **Branch**: `master`
- **Judul**: Pembaruan Changelog Rilis (`issue-514` s.d. `issue-535`) dan Basis Pengetahuan AI (`features.json`)
- **Status Branch**: `Sudah di-merge` (Langsung di `master` & `production` pada Rabu, 11 November 2026, 11:19:03 WIB via commit `0e87ec16`)

### 📅 Rincian Commit

#### [0e87ec16] - update changelog - 11 November 2026, 11:19:03 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-514.json` [NEW]
  - `backend/src/data/changelog/releases/issue-518.json` [NEW]
  - `backend/src/data/changelog/releases/issue-520.json` [NEW]
  - `backend/src/data/changelog/releases/issue-524.json` [NEW]
  - `backend/src/data/changelog/releases/issue-526.json` [NEW]
  - `backend/src/data/changelog/releases/issue-527.json` [NEW]
  - `backend/src/data/changelog/releases/issue-531.json` [NEW]
  - `backend/src/data/changelog/releases/issue-534.json` [NEW]
  - `backend/src/data/changelog/releases/issue-535.json` [NEW]

- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan 9 berkas catatan rilis baru (`issue-514` hingga `issue-535`, versi `v1.99.x` sampai `v1.100.2`) ke `backend/src/data/changelog/releases/` serta memperbarui indeks pencarian `changelog/index.json`.
  - Memperbarui `backend/src/data/aiKnowledge/features.json` agar asisten AI memiliki informasi fitur dan pembaruan aplikasi terkini.

---

## 🌿 Branch: `issue-534` — Alat Pemulihan Reservasi IP & Penghitung ID Radius NAS, Pengamanan API Key AI, serta Validasi Konfigurasi OLT

### 📌 Informasi Issue

- **Nomor Issue**: #534
- **Judul Issue**: Alat Pemulihan IP & NAS, Pengamanan API Key AI Agent, dan Pemeriksaan Konfigurasi OLT
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` & `production` pada Rabu, 11 November 2026, 11:14:56 WIB via commit `76951317`)

### 📅 Rincian Commit

#### [247eff85] (merge: [76951317]) - resolve #534 - 11 November 2026, 11:14:22 WIB (merge 11:14:56 WIB)

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/aiAgent.controller.js`
  - `backend/src/controllers/networkIPv4.controller.js`
  - `backend/src/controllers/radiusNas.controller.js`
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/radiusNas.model.js`
  - `backend/src/routes/networkIPv4.route.js`
  - `backend/src/routes/radiusNas.route.js`
  - `backend/src/routes/settings.route.js`
  - `backend/src/server.js`
  - `backend/src/services/olt.service.js`
  - `backend/src/services/radiusNas.service.js`
  - `backend/src/services/recovery.service.js`
  - `backend/src/utils/maskSecret.js` [NEW]
  - `backend/src/utils/oltConfigCheck.js` [NEW]
  - `backend/test/integration/aiAgentApiKeyMask.test.js` [NEW]
  - `backend/test/integration/olt.service.test.js`
  - `backend/test/integration/orphanIpReservationRecovery.test.js` [NEW]
  - `backend/test/integration/radiusNasCounterRecovery.test.js` [NEW]
  - `backend/test/unit/maskSecret.test.js` [NEW]
  - `backend/test/unit/oltConfigCheck.test.js` [NEW]
  - `backend/test/unit/radiusNas.model.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/AiAgent.jsx`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `olt-gateway/src/config.js`
  - `olt-gateway/src/server.js`
  - `olt-gateway/test/config.validate.test.js` [NEW]
  - `radius-server/internal/repository/mongo/model/nas.go`

- **Deskripsi Perubahan & Fungsi**:
  - **Alat Pemulihan "Pelepasan IP Tersangkut dari Layanan Terhapus" (`POST /api/v1/network/ipv4/recovery-orphan-reservation`)**:
    - Menambahkan alat pemulihan developer-only untuk melepas reservasi IP (`use: "radius"`) milik akun RADIUS yang sudah di-soft-delete agar alamat IP kembali muncul di picker IP tersedia. Mendukung mode simulasi (`dryRun: true`) serta konfirmasi wajib `confirm: "RECOVERY"`.
  - **Perbaikan Bug ID Ganda & Alat Pemulihan Penghitung ID Radius NAS (`POST /api/v1/radius-nas/recovery-counter`)**:
    - Menambahkan sinkronisasi atomik penghitung `identitycounters` sebelum membuat Radius NAS (`syncRadiusNasCounter` di `radiusNas.service.js`) beserta retry saat terjadi bentrok duplicate key `nas_id_1`, dan memperbarui model `nas.go` di `radius-server`.
    - Menyediakan alat pemulihan developer-only untuk menaikkan penghitung `nas_id` ke nilai maksimum data yang ada, melaporkan `nas_id` duplikat, dan membangun ulang indeks unik `nas_id_1` jika hilang.
  - **Pengamanan API Key Asisten AI (`maskSecret.js`, `settings.controller.js`, `AiAgent.jsx`)**:
    - Mengubah endpoint `POST /api/v1/settings/read/sensitive` dan `aiAgent.controller.js` agar tidak pernah mengirim API key AI asli ke klien; hanya mengirim versi termasker (`ai_api_key_masked`), sementara form Pengaturan AI Agent membiarkan kolom kosong untuk mempertahankan kunci lama.
  - **Validasi Konfigurasi Koneksi OLT (`oltConfigCheck.js`, `olt-gateway/src/config.js`)**:
    - Backend kini mencatat peringatan terstruktur saat startup bila `OLT_CREDENTIAL_ENCRYPTION_KEY` tidak valid (harus 64 karakter hex tanpa spasi/CR) atau `OLT_GATEWAY_URL` kosong, sedangkan service `olt-gateway` menolak start jika kunci enkripsi tidak valid.

---

## 🌿 Branch: `issue-535` — Penyempurnaan Pemilihan Baris Tabel & Ekspor Data Terpilih

### 📌 Informasi Issue

- **Nomor Issue**: #535
- **Judul Issue**: Penyempurnaan Pemilihan Baris Tabel dan Ekspor Data Terpilih
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` & `production` pada Rabu, 11 November 2026, 11:13:47 WIB via commit `8bcf1a13`)

### 📅 Rincian Commit

#### [8b815048] (merge: [8bcf1a13]) - resolve #535 - 11 November 2026, 11:12:47 WIB (merge 11:13:47 WIB)

- **Komponen yang Berubah**:
  - `frontend/src/app/pages/archive/partnerCard/PrintBatchDrawer.jsx`
  - `frontend/src/components/shared/table/SelectCheckbox.jsx`
  - `frontend/src/components/shared/table/SelectedRowsActions.jsx`
  - `frontend/src/components/shared/table/Table.jsx`
  - `frontend/src/components/shared/table/Table.test.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/selectionUtils.js` [NEW]
  - `frontend/src/components/shared/table/selectionUtils.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/jsonExporter.js`

- **Deskripsi Perubahan & Fungsi**:
  - **Persistensi Pilihan Lintas Halaman & Filter (`selectionUtils.js`, `Table.jsx`, `SelectCheckbox.jsx`, `rows.jsx`)**:
    - Menambahkan utilitas pemilihan baris (`selectionUtils.js`) dan integrasi di `Table.jsx` agar daftar baris terpilih beserta jumlah total pilihan tetap terjaga saat berpindah halaman tabel, mengubah ukuran halaman, atau melakukan pencarian.
  - **Kontrol Toolbar Aksi Baris Terpilih (`SelectedRowsActions.jsx`)**:
    - Menambahkan tombol **"Hapus Pilihan"** (`clearSelection`) dan toggle **"Tampilkan hanya daftar terpilih"** / **"Tampilkan semua data"** (`showSelectedOnly` / `showAllRows`), serta merapikan layout tombol aksi agar tidak terpotong di layar sempit.
  - **Perbaikan Ekspor & Cetak Data Terpilih (`jsonExporter.js`)**:
    - Menambahkan `resolveExportRows` agar fungsi `exportTableToExcel`, `exporTabletToCSV`, dan `printTable` mengekspor seluruh baris yang dipilih (termasuk yang berada di halaman lain menggunakan `meta.getSelectedRows()`), bukan hanya baris pada halaman aktif saat ini.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #536  | Integrasi Midtrans Core API & Koreksi Fee | Menambah provider Midtrans (VA, QRIS, retail, e-wallet), rotasi Signature Secret Key, koreksi fee manual 1x per transaksi, dan penjaga mode sandbox/live |
| #539  | Detail Pembayaran & Urutan Cron Finance | Menambah modal & endpoint detail pembayaran ber-minimisasi field, merapikan tabel Riwayat Pembayaran, dan memastikan isolir (01:00) berjalan sebelum auto-invoice (01:30) |
| (master) | Pembaruan Changelog & AI Knowledge | Memperbarui daftar rilis changelog hingga `v1.100.2` dan basis pengetahuan fitur AI |
| #534  | Alat Pemulihan IP/NAS & Keamanan API Key AI | Menambah alat pemulihan IP tersangkut & counter NAS ID, mencegah kegagalan NAS ID ganda, menyembunyikan API key AI asli, dan memperketat validasi kunci OLT |
| #535  | Pemilihan Baris Tabel & Ekspor Terpilih | Menjaga pilihan baris tabel lintas halaman/pencarian, menambah filter "tampilkan terpilih saja" & "hapus pilihan", serta memperbaiki ekspor Excel/CSV/cetak data terpilih |

### Kemampuan Baru Pengguna/Admin

- **Konfigurasi & Penerimaan Pembayaran via Midtrans**: Admin keuangan dapat mengonfigurasi Midtrans (Server Key, Signature Secret Key & rotasinya, Mode Sandbox, pilihan kanal VA/QRIS/Retail/E-Wallet, tarif perkiraan fee, dan kebijakan PPN fee) serta memantau transaksi dan saldo pembukuan Midtrans di menu Gerbang Pembayaran.
- **Koreksi Fee Transaksi Gateway**: Admin dengan hak akses `financeGateway.changeSensitive` dapat mengoreksi fee perkiraan transaksi Midtrans menjadi fee aktual dari laporan dashboard provider melalui modal **Koreksi Fee** di detail transaksi.
- **Inspeksi Detail Pembayaran Terpusat**: Admin (`financePayment.read`) dapat mengklik `payment_id` di tabel Riwayat Pembayaran atau di dalam drawer Detail Tagihan untuk melihat modal rincian pembayaran lengkap beserta riwayat cicilan, koreksi, transaksi gateway, dan refund.
- **Alat Pemulihan Data Developer**: Developer kini memiliki dua alat pemulihan baru dengan mode simulasi (`dryRun`) di Pengaturan Developer > Perbaikan Data untuk melepas reservasi IP yatim (`orphanIpReservation`) dan menyinkronkan penghitung ID Radius NAS (`radiusNasCounter`).
- **Manajemen Seleksi Baris Tabel**: Admin dapat memilih baris di beberapa halaman tabel sekaligus, memfilter tampilan hanya untuk baris terpilih, menghapus seluruh pilihan, dan mengekspor hanya data terpilih ke Excel/CSV/Cetak.

### Bug Fix / Solusi Masalah

- **Penjaga Mode Sandbox/Live Payment Gateway**: Mencegah error 500 saat verifikasi ulang transaksi dari mode berbeda (kini ditolak terstruktur HTTP 409) serta mencegah instruksi pembayaran mode lama dipakai ulang setelah berpindah mode sandbox ↔ live.
- **Urutan Cron Isolir vs Tagihan Otomatis**: Mengubah jadwal `invoiceFreeze` ke `01:00` sebelum `financeAutoInvoiceGenerate` (`01:30`) agar akun yang baru melewati jatuh tempo diisolir terlebih dahulu sebelum tagihan otomatis baru diterbitkan.
- **Duplikasi ID Radius NAS**: Memperbaiki kegagalan pembuatan Radius NAS akibat nilai penghitung `identitycounters` tertinggal dari `nas_id` terbesar di koleksi.
- **Kebocoran API Key AI & Minimisasi Payload Detail Pembayaran**: API key asisten AI tidak lagi dikembalikan dalam teks terang ke frontend, dan endpoint detail pembayaran membatasi field internal MongoDB melalui proyeksi eksplisit.
- **Ekspor Data Terpilih Lintas Halaman**: Memperbaiki ekspor Excel/CSV/cetak pada tabel agar benar-benar memuat seluruh item terpilih meskipun pengguna berpindah halaman atau melakukan pencarian.

### Menu/Fitur Baru

- **Pengaturan > Keuangan > Gerbang Pembayaran — Kartu Midtrans**: Panel pengaturan lengkap untuk provider Midtrans beserta tombol Koreksi Fee pada drawer detail transaksi di menu **Keuangan > Payment Gateway**.
- **Modal Detail Pembayaran (`PaymentDetailModal`)**: Tampilan detail pembayaran yang dapat diakses langsung dari kolom ID Pembayaran pada menu **Keuangan > Tagihan > Riwayat Pembayaran** maupun drawer Detail Tagihan.
- **Toolbar Seleksi Tabel**: Tombol **Hapus Pilihan** dan toggle **Tampilkan hanya daftar terpilih / Tampilkan semua data** pada komponen tabel bersama.
- **Pengaturan Developer > Perbaikan Data**: Dua aksi pemulihan baru — **Pelepasan IP Tersangkut dari Layanan Terhapus** dan **Perbaikan Penghitung ID Radius NAS**.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  Integrasi **Payment Gateway Midtrans** dan **Koreksi Fee Manual** memungkinkan aplikasi menerbitkan pembayaran digital melalui Core API Midtrans (VA BCA/BNI/BRI/Permata, QRIS, Alfamart/Indomaret, GoPay, ShopeePay) dengan verifikasi webhook berbasis `Signature Secret Key` yang mendukung rotasi kunci tanpa downtime. Karena Midtrans tidak mengirimkan potongan fee pada respons API per transaksi, sistem membukukan fee berdasarkan tabel tarif perkiraan yang dapat disesuaikan admin; apabila ada selisih terhadap laporan settlement Midtrans, admin dapat melakukan penyesuaian sekali per transaksi melalui fitur Koreksi Fee yang otomatis menjurnal selisihnya ke akun Kas & Bank gateway dan Beban Administrasi Bank / PPN Masukan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka **Pengaturan > Keuangan > Gerbang Pembayaran**, isi **Server Key** dan **Signature Secret Key** Midtrans (salin juga **URL Notifikasi** ke dashboard Midtrans pada *Settings > Configuration > Payment Notification URL*), aktifkan kanal pembayaran yang diinginkan, dan sesuaikan **Tarif Fee (Perkiraan)** sesuai kontrak merchant lalu klik simpan.
  2. Jika ingin langsung memakai Midtrans untuk transaksi baru, pilih **Midtrans** pada opsi **Gerbang Pembayaran Aktif** (sistem otomatis menautkan/membuat akun Kas & Bank `midtrans` atau `midtrans-sandbox`).
  3. Untuk menyesuaikan selisih fee setelah melihat laporan Midtrans, buka **Keuangan > Payment Gateway**, klik nomor transaksi Midtrans yang sudah berstatus terbukukan, lalu klik tombol **Koreksi Fee**, masukkan fee aktual dan porsi PPN, periksa pratinjau selisih, dan klik **Simpan Koreksi**.
