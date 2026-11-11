# 📝 Daily Work Report - Dedy S.N Putra (2026-08-10)

---

## 📅 Laporan Harian - 10 Agustus 2026

---

## 📌 Catatan Ringkas

Pada hari ini, tanggal 10 Agustus 2026, telah diselesaikan dan di-merge **dua issue utama** beserta pembaruan rilis changelog aplikasi:
1. **Issue #211**: Pengembangan Lanjutan Modul Finance & Accounting (Party Tracking, Journal Reversal, Settings Consolidation, Payment Gateway Config, dan Integration Test Suite).
2. **Issue #212**: Integrasi & Manajemen Notifikasi Telegram (Telegram Groups Manager), Pengaturan Aplikasi, Sync Presensi Absensi Perangkat, dan Notifikasi Laporan Gudang.
3. **Pembaruan Rilis**: Update `CHANGELOG.md` dan `backend/src/data/changelog.json` untuk mencatat riwayat perubahan versi terbaru.

---

## 🌿 Branch: `issue-211` — Pengembangan Lanjutan Modul Finance & Accounting

### 📌 Informasi Issue

- **Nomor Issue**: #211
- **Judul Issue**: Pengembangan Lanjutan Modul Finance & Accounting (Party Tracking, Journal Reversal, Settings Consolidation, Payment Gateway Config, dan Integration Test Suite)
- **Status Branch**: `Sudah di-merge` ke master

### 📅 Rincian Commit

#### [`a48d5e0`] & [`5211259`] - `resolve #211` - 10 Agustus 2026, 19:06 - 19:07 WIB

- **Komponen yang Berubah**:
  - `FINANCE_AUDIT.md` [NEW]
  - `V1_COMPAT_DEBT.md` [NEW]
  - `backend/test/README.md` [NEW]
  - `backend/test/helpers/db.js` [NEW]
  - `backend/test/helpers/factories.js` [NEW]
  - `backend/test/integration/financeLedger.postEntries.test.js` [NEW]
  - `backend/test/setup/global-setup.js` [NEW]
  - `backend/test/setup/logger.stub.js` [NEW]
  - `backend/test/setup/redis.stub.js` [NEW]
  - `backend/test/setup/setup-file.js` [NEW]
  - `backend/test/unit/financeRecurring.schedule.test.js` [NEW]
  - `backend/vitest.config.js` [NEW]
  - `backend/src/utils/party-search.js` [NEW]
  - `backend/src/utils/resolve-party.js` [NEW]
  - `frontend/src/components/shared/form/PartyPicker.jsx` [NEW]
  - `frontend/src/app/pages/finance/coa/detail/index.jsx` [NEW]
  - `frontend/src/app/pages/finance/coa/detail/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/finance/ledger/detail.jsx` [NEW]
  - `frontend/src/app/pages/finance/recurring/detail.jsx` [NEW]
  - `frontend/src/app/pages/finance/reports/TrialBalance.jsx` [NEW]
  - `backend/src/models/financeTransaction.model.js`
  - `backend/src/models/financeTransactionDraft.model.js`
  - `backend/src/models/financeLogs.model.js`
  - `backend/src/models/financeCoa.model.js`
  - `backend/src/models/financeJournal.model.js`
  - `backend/src/models/financeRecurring.model.js`
  - `backend/src/services/financeCoa.service.js`
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/financeJournal.service.js`
  - `backend/src/services/financeTransaction.service.js`
  - `backend/src/services/paymentGateway.service.js`
  - `backend/src/services/financeAccount.service.js`
  - `backend/src/services/option.service.js`
  - `backend/src/controllers/financeSettings.controller.js`
  - `backend/src/controllers/financeJournal.controller.js`
  - `backend/src/controllers/financeTransaction.controller.js`
  - `backend/src/controllers/financeCoa.controller.js`
  - `backend/src/routes/financeJournal.route.js`
  - `backend/src/routes/financeCoa.route.js`
  - `backend/src/routes/financeRecurring.route.js`
  - `backend/src/routes/financeSettings.route.js`
  - `frontend/src/app/pages/finance/transactions/TransactionDrawer.jsx`
  - `frontend/src/app/pages/finance/journals/index.jsx`
  - `frontend/src/app/pages/finance/journals/detail.jsx`
  - `frontend/src/app/pages/finance/journals/schema/columns.jsx`
  - `frontend/src/app/pages/finance/coa/index.jsx`
  - `frontend/src/app/pages/finance/coa/schema/columns.jsx`
  - `frontend/src/app/pages/settings/sections/Finance.jsx`
  - `frontend/src/app/pages/settings/sections/Application.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/locales/en/translation.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Pelacakan Pihak Terstruktur (Party Tracking)**: Menambahkan penautan terstruktur ke entitas `customer`, `partner`, `employee`, dan `vendor` pada model transaksi, draf transaksi, dan log mutasi kas/bank. Menyediakan komponen frontend [`PartyPicker.jsx`](frontend/src/components/shared/form/PartyPicker.jsx) dan utility backend [`party-search.js`](backend/src/utils/party-search.js) (`buildPartySearchOr`) yang memungkinkan pencarian pihak terstruktur secara terpusat di seluruh tabel keuangan.
  - **Pembalikan Jurnal (Journal Reversal)**: Menambahkan layanan dan endpoint backend `POST /finance/journal/:journal_id/reverse` serta dialog konfirmasi frontend untuk membalik jurnal saldo awal (`opening`) atau koreksi manual (`manual`) dengan alasan wajib.
  - **Konsolidasi Pengaturan Keuangan**: Memindahkan konfigurasi Tagihan (catatan faktur, jatuh tempo, pajak) dan iPay (payment gateway iPaymu) dari tab Settings > Application ke Settings > Finance. Kredensial gateway iPaymu kini dibaca langsung dari database dengan otomasional penautan akun Kas & Bank gateway.
  - **Halaman & Detail Baru**: Membuat halaman detail COA (`/finance/coa/view/:coa_id`), detail mutasi kas/bank (`/finance/ledger/view/:id`), detail transaksi berulang, serta memindahkan Neraca Saldo (Trial Balance) ke komponen terpisah di modul Reports.
  - **Infrastruktur Unit & Integration Test (Vitest)**: Membangun test runner Vitest berbasis MongoDB In-Memory untuk menguji atomisitas pos jurnal dan eksekusi transaksi berulang secara otomatis.
  - **Dokumentasi Audit & Kompatibilitas**: Menulis dokumen audit komprehensif [`FINANCE_AUDIT.md`](FINANCE_AUDIT.md) dan [`V1_COMPAT_DEBT.md`](V1_COMPAT_DEBT.md) untuk mendokumentasikan penanganan utang teknis kompatibilitas V1.

---

## 🌿 Branch: `issue-212` — Integrasi & Manajemen Notifikasi Telegram (Telegram Groups Manager)

### 📌 Informasi Issue

- **Nomor Issue**: #212
- **Judul Issue**: Integrasi & Manajemen Notifikasi Telegram (Telegram Groups Manager), Pengaturan Aplikasi, Sync Presensi Absensi, dan Laporan Gudang
- **Status Branch**: `Sudah di-merge` ke master (merged via `b700473`)

### 📅 Rincian Commit

#### [`a621ed4`], [`80062fd`] & [`b700473`] - `resolve #212` - 10 Agustus 2026, 19:08 - 19:24 WIB

- **Komponen yang Berubah**:
  - `frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx` [NEW]
  - `backend/src/utils/telegram.js`
  - `backend/src/routes/settings.route.js`
  - `backend/src/routes/public.route.js`
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/controllers/attendance.controller.js`
  - `backend/src/services/option.service.js`
  - `backend/src/services/attendanceDeviceSync.service.js`
  - `backend/src/services/warehouseReport.service.js`
  - `frontend/src/app/pages/settings/sections/Application.jsx`
  - `frontend/src/app/pages/settings/schema/applicationSchema.js`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/locales/en/translation.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Telegram Groups Manager**: Mengembangkan komponen manajemen grup Telegram [`TelegramGroupsManager.jsx`](frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx) di Pengaturan Aplikasi. Memungkinkan admin mendaftarkan beberapa grup Telegram (multi-group), menentukan kategori notifikasi per grup (Presensi/Absensi, Keuangan, Gudang, Tiket Layanan), mengaktifkan/menonaktifkan grup, dan melakukan pengujian kirim notifikasi (Test Notification).
  - **Refactoring Utilitas Telegram Backend**: Memperbarui [`backend/src/utils/telegram.js`](backend/src/utils/telegram.js) agar pengiriman notifikasi disalurkan secara dinamis ke grup Telegram yang aktif sesuai dengan topik notifikasi masing-masing.
  - **Peningkatan Sinkronisasi Absensi Perangkat**: Menyempurnakan pencatatan presensi dari mesin absen (`attendanceDeviceSync.service.js`) dan otomatisasi pengiriman ringkasan kehadiran ke grup Telegram terkait.
  - **Integrasi Notifikasi Laporan Gudang**: Menyambungkan laporan mutasi dan stok barang gudang (`warehouseReport.service.js`) agar dapat mengirim notifikasi peringatan stok ke grup Telegram yang terkonfigurasi.

---

## 🌿 Maintenance: Rilis Changelog Aplikasi

### 📌 Informasi

- **Komponen**: `CHANGELOG.md` & `backend/src/data/changelog.json`
- **Status**: `Sudah di-merge` ke master

### 📅 Rincian Commit

#### [`563420d`] - `update changelog` - 10 Agustus 2026, 19:43 WIB

- **Komponen yang Berubah**:
  - `CHANGELOG.md`
  - `backend/src/data/changelog.json`
- **Deskripsi Perubahan & Fungsi**:
  - Memperbarui dokumentasi catatan perubahan rilis aplikasi (Changelog) agar riwayat pembaruan sistem dan modul keuangan/notifikasi tercatat rapi untuk konsumsi pengguna dan admin.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #211 | Modul Finance Advanced & Test Suite | Pelacakan pihak terstruktur, pembalikan jurnal, pengaturan payment gateway dinamis dari DB, halaman detail COA/Ledger, dan pengujian otomatis Vitest. |
| #212 | Integrasi Telegram Groups Manager | Pengelolaan grup Telegram multi-topik (Absensi, Keuangan, Gudang, Tiket) langsung dari UI Pengaturan Aplikasi dengan pengujian kirim notifikasi. |
| Maintenance | Update Changelog | Pencatatan riwayat rilis otomatis di `CHANGELOG.md` dan `changelog.json`. |

### Kemampuan Baru Pengguna/Admin

- Admin dapat memilih **pihak lawan terstruktur** (Karyawan/Pelanggan/Mitra/Vendor) pada form transaksi keuangan menggunakan komponen `PartyPicker`.
- Admin dapat **membalik jurnal saldo awal dan koreksi manual** yang bermasalah langsung dari halaman Jurnal dengan menyertakan alasan pembalikan wajib.
- Admin dapat **mengelola kredensial payment gateway iPaymu** (VA, API Key, Sandbox/Live switch) dari halaman Pengaturan Keuangan tanpa restart server.
- Admin dapat **mengatur daftar grup Telegram notifikasi** (Telegram Groups Manager) per kategori notifikasi (Absensi, Keuangan, Gudang, Tiket) dan menguji pengirimannya.
- Admin dapat melihat **buku besar transaksional di halaman Detail COA** dan **Detail Mutasi Kas/Bank**.

### Bug Fix / Solusi Masalah

- **Penanganan Nilai `false` dan `0` pada Form Settings**: Mencegah terbuangnya nilai boolean nonaktif dan angka 0 (seperti diskon 0% atau threshold 0) akibat pembersihan otomatis `cleanFormData`.
- **Kontinuitas Pengaturan Legacy V1**: Kredensial gateway dan pengaturan tagihan tetap membaca `application_settings` lama sebagai fallback untuk mencegah matinya integrasi pada server produksi.
- **Penyempurnaan Notifikasi Telegram Multi-Grup**: Mencegah kegagalan pengiriman notifikasi ke grup Telegram yang tidak valid dengan validasi format Chat ID dan penanganan error terisolasi.

### Menu/Fitur Baru

- **Telegram Groups Manager** (`Settings > Application > Telegram Groups`).
- **Halaman Detail COA** (`/finance/coa/view/:coa_id`).
- **Halaman Detail Mutasi Kas & Bank** (`/finance/ledger/view/:id`).
- **Dialog Pembalikan Jurnal** pada tabel Jurnal Keuangan.
- **Tab Tagihan & Payment Gateway iPaymu** pada `Settings > Finance`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Manajemen Grup Telegram Notifikasi (Telegram Groups Manager)

- **Penjelasan Fitur**: Fitur ini memungkinkan admin mengatur grup-grup Telegram yang akan menerima notifikasi otomatis dari sistem. Setiap grup dapat dikonfigurasi untuk menerima notifikasi spesifik, seperti notifikasi presensi karyawan, mutasi/stok gudang, laporan keuangan, atau tiket layanan.
- **Langkah Penggunaan**:
  1. Buka halaman **Settings > Application**, lalu pilih tab/seksi **Telegram Groups**.
  2. Klik tombol **Tambah Grup Telegram**.
  3. Masukkan **Nama Grup**, **Chat ID Telegram** (contoh: `-100123456789`), dan centang jenis notifikasi yang ingin dialirkan ke grup tersebut (Absensi, Keuangan, Gudang, Tiket).
  4. Klik tombol **Uji Notifikasi (Test Notification)** pada baris grup untuk memastikan bot Telegram sudah dimasukkan ke grup dan memiliki hak kirim pesan.
  5. Simpan pengaturan.

### 2. Pelacakan Pihak Terstruktur (Party Tracking) pada Transaksi Keuangan

- **Penjelasan Fitur**: Saat mencatat transaksi kas/bank atau faktur, admin kini dapat menautkan transaksi secara langsung ke pihak lawan terstruktur (Karyawan, Pelanggan, Mitra, atau Vendor). Pihak ini dapat dicari dari satu kotak pencarian lintas entitas dan akan muncul sebagai link yang dapat diklik menuju profil entitas terkait pada tabel transaksi dan buku besar.
- **Langkah Penggunaan**:
  1. Buka menu **Finance > Transactions**, lalu klik **Tambah Transaksi**.
  2. Pada field **Pihak Terkait**, ketik nama pihak (karyawan/pelanggan/mitra/vendor).
  3. Komponen `PartyPicker` akan menampilkan pilihan yang sesuai lengkap dengan lencana jenis entitasnya.
  4. Pilih entitas yang sesuai, lengkapi rincian nominal dan akun kas/bank, lalu klik **Simpan**.
  5. Di tabel transaksi, nama pihak akan tampil dengan link interaktif menuju detail profil pihak tersebut.
