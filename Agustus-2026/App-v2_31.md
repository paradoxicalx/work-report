# 📝 Daily Work Report - Dedy (2026-08-31)

---

## 📅 Laporan Harian - 31 Agustus 2026

---

## 🌿 Branch: `issue-257` — Docker Production Deployment, Finance Recurring Transaksi & Payment Crash Recovery

### 📌 Informasi Issue

- **Nomor Issue**: #257
- **Judul Issue**: Docker Production Deployment Infrastructure, Modul Finance Recurring Transaction (Pembayaran Berulang Otomatis), Payment Crash Recovery & Audit Fixes, serta Restrukturisasi Sidebar Frontend
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`79a147a`](https://github.com/user/repo/commit/79a147a) - resolve #257 - 31 Agustus 2026, 23:21:57 WIB

- **Komponen yang Berubah**:
  - [`.env.production.example`](.env.production.example) [NEW] — Template environment variable untuk seluruh stack produksi (Backend, Frontend, Cron Worker, Network Monitor, Telegram Apps, WhatsApp API, Radius Server, MongoDB, Redis, MinIO) dengan dokumentasi komentar tiap variabel.
  - [`FINANCE_AUDIT.md`](FINANCE_AUDIT.md) — Pembaruan dokumen audit keuangan: checklist temuan akuntansi dan status penyelesaian modul.
  - [`docker-compose.prod.yml`](docker-compose.prod.yml) [NEW] — Orkestrasi Docker Compose produksi lengkap untuk seluruh service monorepo: Backend, Frontend (Nginx), Cron Worker, Network Monitor, Telegram Apps, Telegram API, WhatsApp API, Radius Server, MongoDB, Redis, MinIO — termasuk health check, restart policy, network isolasi, dan volume persisten.
  - **Backend Dockerfile & `.dockerignore`**:
    - [`backend/Dockerfile`](backend/Dockerfile) [NEW] — Build image Node.js ESM multi-stage untuk Backend API.
    - [`backend/.dockerignore`](backend/.dockerignore) [NEW] — Pengecualian file tidak perlu dari Docker context backend.
  - **Frontend Dockerfile & Nginx**:
    - [`frontend/Dockerfile`](frontend/Dockerfile) [NEW] — Build image multi-stage Vite + Nginx untuk SPA frontend.
    - [`frontend/.dockerignore`](frontend/.dockerignore) [NEW]
    - [`frontend/nginx.conf`](frontend/nginx.conf) [NEW] — Konfigurasi Nginx untuk SPA routing (try_files) dan proxy API ke Backend.
  - **Cron Worker Dockerfile**:
    - [`cron-worker/Dockerfile`](cron-worker/Dockerfile) [NEW] — Build image Node.js untuk service penjadwal BullMQ.
    - [`cron-worker/.dockerignore`](cron-worker/.dockerignore) [NEW]
  - **Network Monitor Dockerfile**:
    - [`network-monitor/Dockerfile`](network-monitor/Dockerfile) [NEW] — Build image untuk microservice probe jaringan.
    - [`network-monitor/.dockerignore`](network-monitor/.dockerignore) [NEW]
  - **Telegram Apps Dockerfile & Nginx**:
    - [`telegram-apps/Dockerfile`](telegram-apps/Dockerfile) [NEW] — Build image untuk Telegram Mini App.
    - [`telegram-apps/.dockerignore`](telegram-apps/.dockerignore) [NEW]
    - [`telegram-apps/nginx.conf`](telegram-apps/nginx.conf) [NEW] — Konfigurasi Nginx untuk TWA.
  - **Backend — Finance Recurring Transaction (Pembayaran Berulang)**:
    - [`backend/src/controllers/financeRecurring.controller.js`](backend/src/controllers/financeRecurring.controller.js) — Controller endpoint CRUD dan eksekusi pembayaran berulang (list, detail, create, update, delete, execute).
    - [`backend/src/models/financeRecurring.model.js`](backend/src/models/financeRecurring.model.js) [NEW] — Model Mongoose `FinanceRecurring` dengan field: `description`, `amount`, `wallet_id`, `destination_wallet_id`, `coa_debit_id`, `coa_credit_id`, `schedule` (type: `daily`/`weekly`/`monthly`/`yearly`, interval, day_of_week, day_of_month, month), `start_date`, `end_date`, `max_executions`, `executed_count`, `last_executed_at`, `next_execution_at`, `status` (`active`/`paused`/`completed`/`failed`), `last_error`.
    - [`backend/src/routes/financeRecurring.route.js`](backend/src/routes/financeRecurring.route.js) [NEW] — Route `api/finance/recurring` dengan privilege `financeRecurring.*` (list, read, create, update, delete, changeStatus).
    - [`backend/src/services/financeRecurring.service.js`](backend/src/services/financeRecurring.service.js) — Layanan bisnis: kalkulasi `next_execution_at` berdasarkan tipe jadwal (daily/weekly/monthly/yearly dengan `day_of_week`, `day_of_month`, `month`), eksekusi transaksi berulang secara atomik (cek saldo wallet sumber, buat jurnal, mutasi wallet debit/kredit, update counter), validasi coa_debit & coa_credit.
  - **Backend — Payment Crash Recovery & Audit Fixes**:
    - [`backend/src/services/financePayment.service.js`](backend/src/services/financePayment.service.js) — Perubahan besar (+510 baris): mekanisme crash recovery pembayaran invoice, idempotensi mutasi wallet berbasis `ref_key` unik, penanganan transaksi parsial yang terputus di tengah (partial payment yang belum selesai diproses jurnal/wallet), rollback/forward recovery untuk pembayaran yang crash sebelum jurnal tercatat.
    - [`backend/src/models/financePayment.model.js`](backend/src/models/financePayment.model.js) — Penambahan field `idempotency_key` dan `crash_recovery_status` pada model pembayaran untuk melacak status pemulihan.
    - [`backend/src/controllers/financePayment.controller.js`](backend/src/controllers/financePayment.controller.js) — Perbaikan kecil pada validasi payload pembayaran.
  - **Backend — Finance Service Enhancements**:
    - [`backend/src/services/financeWallet.service.js`](backend/src/services/financeWallet.service.js) — Perubahan signifikan: normalisasi query wallet, penambahan validasi saldo sebelum mutasi, perbaikan `findOneAndUpdate` atomik dengan kondisi `balance: { $gte: amount }`.
    - [`backend/src/services/financeExpense.service.js`](backend/src/services/financeExpense.service.js) — Perubahan besar (+160 baris): penyesuaian alur persetujuan expense, validasi relasi akun COA, perbaikan kalkulasi saldo pembayaran parsial.
    - [`backend/src/services/financeFixedAsset.service.js`](backend/src/services/financeFixedAsset.service.js) — Perubahan (+67 baris): penyesuaian kalkulasi depresiasi dan dispose aset tetap.
    - [`backend/src/services/financeCoa.service.js`](backend/src/services/financeCoa.service.js) — Penambahan validasi kaskade path/hierarchy dan integrasi dengan modul recurring.
    - [`backend/src/services/financeAccount.service.js`](backend/src/services/financeAccount.service.js) — Perubahan pada logika pencarian dan filtering akun.
    - [`backend/src/services/financeTransaction.service.js`](backend/src/services/financeTransaction.service.js) — Penambahan fungsi utility untuk transaksi berulang.
    - [`backend/src/controllers/financeRegulatoryObligation.controller.js`](backend/src/controllers/financeRegulatoryObligation.controller.js) — Perubahan pada controller kewajiban regulasi.
    - [`backend/src/services/financeRegulatoryObligation.service.js`](backend/src/services/financeRegulatoryObligation.service.js) — Perubahan pada layanan kewajiban regulasi.
    - [`backend/src/controllers/internal.controller.js`](backend/src/controllers/internal.controller.js) — Penyesuaian endpoint internal untuk mendukung cron worker.
    - [`backend/src/data/financeCoaSeed.json`](backend/src/data/financeCoaSeed.json) — Penyesuaian data seed COA.
  - **Backend — Error Handling & i18n**:
    - [`backend/src/utils/finance-error.js`](backend/src/utils/finance-error.js) — Penambahan kode error baru untuk recurring transaction dan crash recovery.
    - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json) & [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json) — Penambahan kunci i18n untuk pesan error dan notifikasi modul berulang.
  - **Backend — Test Suite**:
    - [`backend/test/integration/financePayment.crashRecovery.test.js`](backend/test/integration/financePayment.crashRecovery.test.js) [NEW] — Pengujian integrasi mekanisme crash recovery pembayaran: simulasi crash sebelum/ sesudah jurnal, verifikasi idempotensi, pemulihan transaksi parsial.
    - [`backend/test/integration/financeTransactionDraft.recurring.test.js`](backend/test/integration/financeTransactionDraft.recurring.test.js) [NEW] — Pengujian integrasi transaksi draf berulang: pembuatan draf otomatis, eksekusi jadwal, pembatalan.
    - [`backend/test/integration/financeExpense.auditFixes.test.js`](backend/test/integration/financeExpense.auditFixes.test.js) — Pembaruan pengujian audit expense (penambahan kasus edge-case).
    - [`backend/test/integration/financeExpenseAP.journal.test.js`](backend/test/integration/financeExpenseAP.journal.test.js) — Penyesuaian pengujian jurnal AP.
    - [`backend/test/integration/financeFixedAsset.test.js`](backend/test/integration/financeFixedAsset.test.js) — Pembaruan pengujian aset tetap.
    - [`backend/test/integration/financeRegulatoryObligation.test.js`](backend/test/integration/financeRegulatoryObligation.test.js) — Pembaruan pengujian kewajiban regulasi.
    - [`backend/test/unit/financeRecurring.schedule.test.js`](backend/test/unit/financeRecurring.schedule.test.js) — Pembaruan pengujian unit kalkulasi jadwal berulang.
  - **Frontend — Restrukturisasi Sidebar & Navigasi**:
    - [`frontend/src/app/layouts/AppLayout.jsx`](frontend/src/app/layouts/AppLayout.jsx) — Penghapusan komponen AppLayout (tidak lagi digunakan).
    - [`frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/index.jsx`](frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/index.jsx) — Penyesuaian menu sidebar utama.
    - [`frontend/src/app/layouts/MainLayout/Sidebar/index.jsx`](frontend/src/app/layouts/MainLayout/Sidebar/index.jsx) — Restrukturisasi komponen sidebar utama.
    - [`frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/index.jsx`](frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/index.jsx) & [`Menu/index.jsx`](frontend/src/app/layouts/Sideblock/Sidebar/Menu/index.jsx) — Penyesuaian menu sidebar sideblock.
    - [`frontend/src/app/navigation/baseNavigation.js`](frontend/src/app/navigation/baseNavigation.js) — Penghapusan file navigasi dasar.
    - [`frontend/src/app/navigation/index.js`](frontend/src/app/navigation/index.js) & [`settings.js`](frontend/src/app/navigation/settings.js) — Penyesuaian konfigurasi navigasi.
    - [`frontend/src/app/pages/settings/Layout.jsx`](frontend/src/app/pages/settings/Layout.jsx) — Penyesuaian layout halaman settings.
    - [`frontend/src/app/pages/settings/Sidebar/SidebarPanel/Footer.jsx`](frontend/src/app/pages/settings/Sidebar/SidebarPanel/Footer.jsx) — Penghapusan footer sidebar settings.
    - [`frontend/src/app/pages/settings/Sidebar/SidebarPanel/Header.jsx`](frontend/src/app/pages/settings/Sidebar/SidebarPanel/Header.jsx) — Penghapusan header sidebar settings.
    - [`frontend/src/app/pages/settings/Sidebar/SidebarPanel/index.jsx`](frontend/src/app/pages/settings/Sidebar/SidebarPanel/index.jsx) — Penghapusan panel sidebar settings.
    - [`frontend/src/app/pages/settings/Sidebar/index.jsx`](frontend/src/app/pages/settings/Sidebar/index.jsx) — Penghapusan komponen sidebar settings.
    - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx) — Penyesuaian route guard untuk mendukung struktur sidebar baru.
- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan infrastruktur Docker Compose produksi lengkap untuk seluruh ekosistem monorepo DEKASIMAL V2, memungkinkan deployment satu perintah (`docker-compose -f docker-compose.prod.yml up -d`) dengan konfigurasi health check, restart policy, network isolasi, dan volume persisten untuk MongoDB, Redis, dan MinIO.
  - Membangun modul Finance Recurring Transaction yang memungkinkan admin mengatur jadwal pembayaran berulang otomatis (harian, mingguan, bulanan, tahunan) dengan validasi saldo wallet sumber, kalkulasi jadwal presisi, dan pelacakan status eksekusi termasuk penanganan error.
  - Mengimplementasikan mekanisme Crash Recovery pada modul pembayaran invoice untuk menangani skenario crash di tengah pemrosesan (setelah wallet terpotong tetapi sebelum jurnal tercatat, atau sebaliknya), memastikan integritas data keuangan tetap terjaga meskipun terjadi kegagalan sistem.
  - Melakukan restrukturisasi sidebar dan navigasi frontend untuk menyederhanakan arsitektur komponen, menghapus lapisan komponen yang redundan (AppLayout, SidebarPanel), dan menyempurnakan alur navigasi.

---

## 🌿 Branch: `issue-183` — Comprehensive Financial Module Enhancement & G3 Accounting Standards Compliance

### 📌 Informasi Issue

- **Nomor Issue**: #183
- **Judul Issue**: Financial Module Audit & G3 Accounting Standards Compliance — Laporan Arus Kas, Umur Piutang/Hutang, Buku Besar Multi-Akun, Manajemen Aset Tetap, Manajemen Periode Akuntansi, Kewajiban Regulasi, Pembayaran Gaji, dan Audit Temuan Akuntansi
- **Status Branch**: `Sudah di-merge` (Merge commit: `0552f78` ke `master`)

### 📅 Rincian Commit

#### [`f30e80b`](https://github.com/user/repo/commit/f30e80b) - resolve #183 - 31 Agustus 2026, 18:14:04 WIB

- **Komponen yang Berubah**:
  - **Backend — Model Baru**:
    - [`backend/src/models/financeFixedAsset.model.js`](backend/src/models/financeFixedAsset.model.js) [NEW] — Model Mongoose `FinanceFixedAsset` untuk aset tetap: nama, kode aset, kategori (tanah, bangunan, kendaraan, elektronik, furniture, mesin, lainnya), tanggal perolehan, harga beli, nilai residu, umur manfaat (bulan), metode depresiasi (straight-line), status (active, disposed, fully-depreciated).
    - [`backend/src/models/financeDepreciationEntry.model.js`](backend/src/models/financeDepreciationEntry.model.js) [NEW] — Model `FinanceDepreciationEntry` untuk pencatatan depresiasi bulanan per aset: period, amount, accumulated, journal_id.
    - [`backend/src/models/financePeriod.model.js`](backend/src/models/financePeriod.model.js) [NEW] — Model `FinancePeriod` untuk manajemen periode akuntansi: year, month, status (open, closed, locked), closed_by, closed_at, open_balance_verified.
    - [`backend/src/models/financeRegulatoryObligation.model.js`](backend/src/models/financeRegulatoryObligation.model.js) [NEW] — Model `FinanceRegulatoryObligation` untuk kewajiban regulasi: PPN, PPh, BPJS, dll — termasuk jatuh tempo, status pembayaran, lampiran dokumen.
    - [`backend/src/models/financeRegulatorySettings.model.js`](backend/src/models/financeRegulatorySettings.model.js) [NEW] — Model pengaturan regulasi: tipe kewajiban, coa_kredit, coa_debit, jadwal pengingat.
    - [`backend/src/models/financeCoa.model.js`](backend/src/models/financeCoa.model.js) — Penambahan field `cash_flow_category` (operating, investing, financing) pada skema COA.
    - [`backend/src/models/financeJournal.model.js`](backend/src/models/financeJournal.model.js) — Penambahan field `leg_key` untuk pengecekan duplikasi jurnal.
    - [`backend/src/models/financeLogs.model.js`](backend/src/models/financeLogs.model.js) — Penambahan field `activeLogsMatch` untuk konsistensi filter.
    - [`backend/src/models/financePeriod.model.js`](backend/src/models/financePeriod.model.js) — Model periode akuntansi.
  - **Backend — Controller Baru**:
    - [`backend/src/controllers/financeFixedAsset.controller.js`](backend/src/controllers/financeFixedAsset.controller.js) [NEW] — Endpoint CRUD aset tetap, generate depresiasi, dispose aset.
    - [`backend/src/controllers/financePeriod.controller.js`](backend/src/controllers/financePeriod.controller.js) [NEW] — Endpoint manajemen periode akuntansi: list, open, close, reopen.
    - [`backend/src/controllers/financeRegulatoryObligation.controller.js`](backend/src/controllers/financeRegulatoryObligation.controller.js) [NEW] — Endpoint CRUD kewajiban regulasi, generate otomatis, pembayaran, void.
    - [`backend/src/controllers/financeInvoice.controller.js`](backend/src/controllers/financeInvoice.controller.js) — Penambahan endpoint receivable aging (summary & table).
    - [`backend/src/controllers/financeCoa.controller.js`](backend/src/controllers/financeCoa.controller.js) — Penambahan endpoint multi-ledger dan opening balance conflicts.
    - [`backend/src/controllers/financeReport.controller.js`](backend/src/controllers/financeReport.controller.js) — Penambahan endpoint cash flow statement.
    - [`backend/src/controllers/payrollSlip.controller.js`](backend/src/controllers/payrollSlip.controller.js) — Endpoint cetak slip gaji.
    - [`backend/src/controllers/settings.controller.js`](backend/src/controllers/settings.controller.js) — Endpoint pengaturan sistem.
  - **Backend — Service Baru & Diperbarui**:
    - [`backend/src/services/financeFixedAsset.service.js`](backend/src/services/financeFixedAsset.service.js) [NEW] — Layanan lengkap aset tetap: CRUD, kalkulasi depresiasi straight-line (bulanan), generate jurnal depresiasi, dispose aset tetap dengan jurnal pembalik.
    - [`backend/src/services/financePeriod.service.js`](backend/src/services/financePeriod.service.js) [NEW] — Layanan periode akuntansi: pengecekan transaksi sebelum close, verifikasi saldo awal, close/lock periode.
    - [`backend/src/services/financeRegulatoryObligation.service.js`](backend/src/services/financeRegulatoryObligation.service.js) [NEW] — Layanan kewajiban regulasi: generate otomatis berdasarkan pengaturan, pembayaran, void, lampiran dokumen.
    - [`backend/src/services/financeRegulatorySettings.service.js`](backend/src/services/financeRegulatorySettings.service.js) [NEW] — Layanan pengaturan kewajiban regulasi.
    - [`backend/src/services/payrollSlip.service.js`](backend/src/services/payrollSlip.service.js) [NEW] — Layanan cetak slip gaji dengan kalkulasi pendapatan dan potongan.
    - [`backend/src/services/financeInvoice.service.js`](backend/src/services/financeInvoice.service.js) — Implementasi receivable aging (AR Aging): agregasi pipeline MongoDB untuk pengelompokan umur piutang (0-30, 31-60, 61-90, >90 hari) dan tabel berpaginasi.
    - [`backend/src/services/financeCoa.service.js`](backend/src/services/financeCoa.service.js) — Perubahan signifikan: buku besar multi-akun (`buildMultiCoaLedger`), kaskade hierarki path/level, deteksi konflik saldo awal ganda, validasi relasi sebelum hapus akun.
    - [`backend/src/services/financeReport.service.js`](backend/src/services/financeReport.service.js) — Laporan Arus Kas metode tidak langsung (`buildCashFlowStatement`), rekonsiliasi kas mandiri, perbaikan struktur Laba-Rugi.
    - [`backend/src/services/financeJournal.service.js`](backend/src/services/financeJournal.service.js) — Penegakan penutupan periode, validasi tanggal masa depan.
    - [`backend/src/services/financeLedger.service.js`](backend/src/services/financeLedger.service.js) — Filter `deleted: { $ne: true }` di seluruh fungsi kalkulasi saldo.
    - [`backend/src/services/financeWallet.service.js`](backend/src/services/financeWallet.service.js) — Penyesuaian kalkulasi.
    - [`backend/src/services/financeExpense.service.js`](backend/src/services/financeExpense.service.js) — Refactoring AP Aging ke agregasi MongoDB.
    - [`backend/src/services/financePayment.service.js`](backend/src/services/financePayment.service.js) — Penghapusan kode legacy.
    - [`backend/src/services/financeAccount.service.js`](backend/src/services/financeAccount.service.js) — Penyesuaian pencarian akun.
    - [`backend/src/services/financeTransaction.service.js`](backend/src/services/financeTransaction.service.js) — Penyesuaian filter transaksi.
    - [`backend/src/services/option.service.js`](backend/src/services/option.service.js) — Penambahan opsi untuk modul aset.
    - [`backend/src/services/waBroadcast.service.js`](backend/src/services/waBroadcast.service.js) — Penyesuaian notifikasi WA.
  - **Backend — Route Baru**:
    - [`backend/src/routes/financeFixedAsset.route.js`](backend/src/routes/financeFixedAsset.route.js) [NEW] — Route CRUD aset tetap dengan privilege `financeFixedAsset.*`.
    - [`backend/src/routes/financePeriod.route.js`](backend/src/routes/financePeriod.route.js) [NEW] — Route manajemen periode akuntansi.
    - [`backend/src/routes/financeRegulatoryObligation.route.js`](backend/src/routes/financeRegulatoryObligation.route.js) [NEW] — Route kewajiban regulasi.
    - [`backend/src/routes/financeInvoice.route.js`](backend/src/routes/financeInvoice.route.js) — Penambahan route receivable aging.
    - [`backend/src/routes/financeCoa.route.js`](backend/src/routes/financeCoa.route.js) — Penambahan route multi-ledger dan conflicts.
    - [`backend/src/routes/financeReport.route.js`](backend/src/routes/financeReport.route.js) — Penambahan route cash flow.
    - [`backend/src/routes/payrollSlip.route.js`](backend/src/routes/payrollSlip.route.js) [NEW] — Route cetak slip gaji.
    - [`backend/src/routes/financeExpense.route.js`](backend/src/routes/financeExpense.route.js) — Penambahan route payable aging.
    - [`backend/src/routes/financeSettings.route.js`](backend/src/routes/financeSettings.route.js) — Penghapusan route lama.
  - **Backend — Utility & Config**:
    - [`backend/src/utils/aging-bucket.js`](backend/src/utils/aging-bucket.js) [NEW] — Utilitas terpusat untuk klasifikasi ember umur piutang/hutang.
    - [`backend/src/utils/finance-error.js`](backend/src/utils/finance-error.js) [NEW] — Kode error terstruktur untuk modul keuangan (22 kode error).
    - [`backend/src/data/financeCoaSeed.json`](backend/src/data/financeCoaSeed.json`) — Pembaruan data seed COA standar.
    - [`backend/src/config/privilege.json`](backend/src/config/privilege.json`) — Penambahan privilege untuk modul aset, periode, regulasi, slip gaji.
    - [`backend/src/app.js`](backend/src/app.js`) — Pendaftaran route baru.
    - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json`) & [`id/translation.json`](backend/src/locales/id/translation.json`) — 67+ kunci i18n baru (id & en).
  - **Backend — Test Suite**:
    - [`backend/test/integration/financeFixedAsset.test.js`](backend/test/integration/financeFixedAsset.test.js) [NEW] — Pengujian integrasi aset tetap: CRUD, depresiasi, dispose.
    - [`backend/test/integration/financePeriod.test.js`](backend/test/integration/financePeriod.test.js) [NEW] — Pengujian integrasi periode akuntansi: open, close, reopen, validasi transaksi.
    - [`backend/test/integration/financeRegulatoryObligation.test.js`](backend/test/integration/financeRegulatoryObligation.test.js) [NEW] — Pengujian integrasi kewajiban regulasi: generate, bayar, void.
    - [`backend/test/integration/financeReport.cashFlow.test.js`](backend/test/integration/financeReport.cashFlow.test.js) [NEW] — Pengujian laporan arus kas dan rekonsiliasi.
    - [`backend/test/integration/financeCoa.multiLedger.test.js`](backend/test/integration/financeCoa.multiLedger.test.js) [NEW] — Pengujian buku besar multi-akun.
    - [`backend/test/integration/financeCoa.pathCascade.test.js`](backend/src/services/../integration/financeCoa.pathCascade.test.js) [NEW] — Pengujian kaskade hierarki COA.
    - [`backend/test/integration/financeInvoiceReceivableAging.test.js`](backend/test/integration/financeInvoiceReceivableAging.test.js) [NEW] — Pengujian AR Aging.
    - [`backend/test/integration/financeReport.cashFlow.test.js`](backend/test/integration/financeReport.cashFlow.test.js) [NEW] — Pengujian Cash Flow Statement.
    - [`backend/test/integration/payrollSlip.paymentReport.test.js`](backend/test/integration/payrollSlip.paymentReport.test.js) [NEW] — Pengujian cetak slip gaji.
    - [`backend/test/integration/financeInvoiceReport.test.js`](backend/test/integration/financeInvoiceReport.test.js`) — Pembaruan pengujian laporan invoice.
    - [`backend/test/integration/financeLedger.postEntries.test.js`](backend/test/integration/financeLedger.postEntries.test.js`) — Penyesuaian pengujian posting jurnal.
    - [`backend/test/integration/financeExpensePayableAging.test.js`](backend/test/integration/financeExpensePayableAging.test.js`) — Penyesuaian pengujian AP Aging.
    - [`backend/test/integration/financeInvoice.create.test.js`](backend/test/integration/financeInvoice.create.test.js`) — Penyesuaian pengujian create invoice.
  - **Frontend — Halaman Baru: Aset Tetap**:
    - [`frontend/src/app/pages/finance/assets/index.jsx`](frontend/src/app/pages/finance/assets/index.jsx) [NEW] — Halaman daftar aset tetap dengan datatable, filter kategori/status, dan tombol aksi.
    - [`frontend/src/app/pages/finance/assets/AssetFormDrawer.jsx`](frontend/src/app/pages/finance/assets/AssetFormDrawer.jsx) [NEW] — Drawer formulir tambah/edit aset tetap (nama, kode, kategori, tanggal perolehan, harga beli, nilai residu, umur manfaat, metode depresiasi).
    - [`frontend/src/app/pages/finance/assets/DetailDrawer.jsx`](frontend/src/app/pages/finance/assets/DetailDrawer.jsx) [NEW] — Drawer detail aset tetap: info utama, grafik depresiasi, jurnal depresiasi, status aset.
    - [`frontend/src/app/pages/finance/assets/DisposeModal.jsx`](frontend/src/app/pages/finance/assets/DisposeModal.jsx) [NEW] — Modal dispose aset tetap: harga jual, tanggal dispose, hitung untung/rugi, generate jurnal pembalik.
    - [`frontend/src/app/pages/finance/assets/GenerateDepreciationDrawer.jsx`](frontend/src/app/pages/finance/assets/GenerateDepreciationDrawer.jsx) [NEW] — Drawer generate depresiasi bulanan untuk seluruh aset aktif.
    - [`frontend/src/app/pages/finance/assets/schema/columns.jsx`](frontend/src/app/pages/finance/assets/schema/columns.jsx) [NEW] — Definisi kolom datatable aset tetap.
  - **Frontend — Halaman Baru: Manajemen Periode Akuntansi**:
    - [`frontend/src/app/pages/finance/periods/index.jsx`](frontend/src/app/pages/finance/periods/index.jsx) [NEW] — Halaman daftar periode akuntansi dengan status (open/closed/locked) dan aksi close/reopen.
    - [`frontend/src/app/pages/finance/periods/CloseDrawer.jsx`](frontend/src/app/pages/finance/periods/CloseDrawer.jsx) [NEW] — Drawer konfirmasi penutupan periode dengan checklist validasi.
    - [`frontend/src/app/pages/finance/periods/ReopenModal.jsx`](frontend/src/app/pages/finance/periods/ReopenModal.jsx) [NEW] — Modal pembukaan kembali periode yang sudah ditutup.
    - [`frontend/src/app/pages/finance/periods/schema/columns.jsx`](frontend/src/app/pages/finance/periods/schema/columns.jsx) [NEW] — Definisi kolom datatable periode.
  - **Frontend — Halaman Baru: Kewajiban Regulasi**:
    - [`frontend/src/app/pages/finance/regulatory/index.jsx`](frontend/src/app/pages/finance/regulatory/index.jsx) [NEW] — Halaman daftar kewajiban regulasi (PPN, PPh, BPJS, dll).
    - [`frontend/src/app/pages/finance/regulatory/DetailDrawer.jsx`](frontend/src/app/pages/finance/regulatory/DetailDrawer.jsx) [NEW] — Drawer detail kewajiban regulasi.
    - [`frontend/src/app/pages/finance/regulatory/GenerateDrawer.jsx`](frontend/src/app/pages/finance/regulatory/GenerateDrawer.jsx) [NEW] — Drawer generate kewajiban regulasi otomatis.
    - [`frontend/src/app/pages/finance/regulatory/PaymentDrawer.jsx`](frontend/src/app/pages/finance/regulatory/PaymentDrawer.jsx) [NEW] — Drawer pembayaran kewajiban regulasi.
    - [`frontend/src/app/pages/finance/regulatory/VoidModal.jsx`](frontend/src/app/pages/finance/regulatory/VoidModal.jsx) [NEW] — Modal pembatalan (void) kewajiban regulasi.
    - [`frontend/src/app/pages/finance/regulatory/SettingsModal.jsx`](frontend/src/app/pages/finance/regulatory/SettingsModal.jsx) [NEW] — Modal pengaturan tipe kewajiban regulasi.
    - [`frontend/src/app/pages/finance/regulatory/schema/columns.jsx`](frontend/src/app/pages/finance/regulatory/schema/columns.jsx) [NEW] — Definisi kolom datatable regulasi.
    - [`frontend/src/app/pages/finance/regulatory/schema/settingsSchema.js`](frontend/src/app/pages/finance/regulatory/schema/settingsSchema.js) [NEW] — Skema form pengaturan regulasi.
  - **Frontend — Laporan Baru & Diperbarui**:
    - [`frontend/src/app/pages/finance/reports/CashFlowStatement.jsx`](frontend/src/app/pages/finance/reports/CashFlowStatement.jsx) [NEW] — Laporan Arus Kas interaktif: breakdown Operating/Investing/Financing, kartu total, status rekonsiliasi.
    - [`frontend/src/app/pages/finance/reports/FixedAssetReport.jsx`](frontend/src/app/pages/finance/reports/FixedAssetReport.jsx) [NEW] — Laporan daftar aset tetap dengan nilai buku, akumulasi depresiasi.
    - [`frontend/src/app/pages/finance/reports/ReceivableAging.jsx`](frontend/src/app/pages/finance/reports/ReceivableAging.jsx) [NEW] — Laporan Umur Piutang (AR Aging) dengan kartu metrik ringkasan.
    - [`frontend/src/app/pages/finance/reports/SalesByAreaReport.jsx`](frontend/src/app/pages/finance/reports/SalesByAreaReport.jsx) [NEW] — Laporan penjualan berdasarkan area/wilayah.
    - [`frontend/src/app/pages/finance/reports/SalesByProductReport.jsx`](frontend/src/app/pages/finance/reports/SalesByProductReport.jsx) [NEW] — Laporan penjualan berdasarkan produk/layanan.
    - [`frontend/src/app/pages/finance/reports/LedgerPrint.jsx`](frontend/src/app/pages/finance/reports/LedgerPrint.jsx) [NEW] — Fitur cetak Buku Besar multi-akun dengan `@media print`.
    - [`frontend/src/app/pages/finance/reports/ReportCard.jsx`](frontend/src/app/pages/finance/reports/ReportCard.jsx) [NEW] — Komponen kartu ringkasan laporan reusable.
    - [`frontend/src/app/pages/finance/reports/BalanceSheet.jsx`](frontend/src/app/pages/finance/reports/BalanceSheet.jsx) — Pembaruan Neraca: banner deteksi saldo awal ganda.
    - [`frontend/src/app/pages/finance/reports/IncomeStatement.jsx`](frontend/src/app/pages/finance/reports/IncomeStatement.jsx) — Pembaruan Laba-Rugi: pemisahan Pendapatan Lain-lain.
    - [`frontend/src/app/pages/finance/reports/PayableAging.jsx`](frontend/src/app/pages/finance/reports/PayableAging.jsx) — Pembaruan AP Aging: kartu ringkasan berbasis backend.
    - [`frontend/src/app/pages/finance/reports/TrialBalance.jsx`](frontend/src/app/pages/finance/reports/TrialBalance.jsx) — Penyesuaian Neraca Saldo.
    - [`frontend/src/app/pages/finance/reports/IssuedReport.jsx`](frontend/src/app/pages/finance/reports/IssuedReport.jsx) — Penyesuaian laporan terbit.
    - [`frontend/src/app/pages/finance/reports/index.jsx`](frontend/src/app/pages/finance/reports/index.jsx) — Pembaruan indeks laporan keuangan dengan akses ke seluruh laporan baru.
  - **Frontend — Draf Transaksi & Transaksi**:
    - [`frontend/src/app/pages/finance/transactions/TransactionDraftDetailDrawer.jsx`](frontend/src/app/pages/finance/transactions/TransactionDraftDetailDrawer.jsx) [NEW] — Drawer detail draf transaksi dengan baris akun debit/kredit dan tombol Approve/Reject.
    - [`frontend/src/app/pages/finance/transactions/TransactionDraftTable.jsx`](frontend/src/app/pages/finance/transactions/TransactionDraftTable.jsx) [NEW] — Tabel draf transaksi pending terisolasi.
    - [`frontend/src/app/pages/finance/transactions/DraftAccountCell.jsx`](frontend/src/app/pages/finance/transactions/DraftAccountCell.jsx) [NEW] — Komponen cell untuk menampilkan akun pada tabel draf.
    - [`frontend/src/app/pages/finance/transactions/schema/draftColumns.jsx`](frontend/src/app/pages/finance/transactions/schema/draftColumns.jsx) [NEW] — Definisi kolom tabel draf transaksi.
    - [`frontend/src/app/pages/finance/transactions/index.jsx`](frontend/src/app/pages/finance/transactions/index.jsx) — Pembaruan halaman transaksi: tab terpisah untuk Transaksi dan Draf Tertunda dengan badge counter.
  - **Frontend — COA & Payroll**:
    - [`frontend/src/app/pages/finance/coa/CoaDrawer.jsx`](frontend/src/app/pages/finance/coa/CoaDrawer.jsx) — Penambahan dropdown `cash_flow_category`.
    - [`frontend/src/app/pages/finance/coa/schema/coaSchema.js`](frontend/src/app/pages/finance/coa/schema/coaSchema.js) — Penambahan validasi `cash_flow_category`.
    - [`frontend/src/app/pages/finance/payroll/runs/SettingsModal.jsx`](frontend/src/app/pages/finance/payroll/runs/SettingsModal.jsx) [NEW] — Modal pengaturan payroll.
    - [`frontend/src/app/pages/finance/payroll/runs/PaymentReport.jsx`](frontend/src/app/pages/finance/payroll/runs/PaymentReport.jsx) [NEW] — Laporan pembayaran gaji.
    - [`frontend/src/app/pages/finance/payroll/runs/index.jsx`](frontend/src/app/pages/finance/payroll/runs/index.jsx) — Pembaruan halaman payroll runs.
    - [`frontend/src/app/pages/finance/payroll/settings/index.jsx`](frontend/src/app/pages/finance/payroll/settings/index.jsx) — Penghapusan halaman pengaturan payroll lama (digantikan SettingsModal).
    - [`frontend/src/app/pages/finance/payroll/slips/PayrollSlipDocument.jsx`](frontend/src/app/pages/finance/payroll/slips/PayrollSlipDocument.jsx) — Penyesuaian template slip gaji.
  - **Frontend — Pengaturan Sistem & Developer Logs**:
    - [`frontend/src/app/pages/settings/sections/System.jsx`](frontend/src/app/pages/settings/sections/System.jsx) — Penambahan fitur pengaturan sistem (179 baris).
    - [`frontend/src/app/pages/settings/sections/Finance.jsx`](frontend/src/app/pages/settings/sections/Finance.jsx) — Pembaruan pengaturan keuangan.
    - [`frontend/src/app/pages/settings/schema/systemSchema.js`](frontend/src/app/pages/settings/schema/systemSchema.js) — Penambahan validasi skema sistem.
    - [`frontend/src/app/pages/settings/sections/developer/logs/AiLogAnalysisModal.jsx`](frontend/src/app/pages/settings/sections/developer/logs/AiLogAnalysisModal.jsx) [NEW] — Modal analisis log menggunakan AI Gemini: upload/log stream, analisis otomatis, rekomendasi perbaikan.
    - [`frontend/src/app/pages/settings/sections/developer/logs/drawers/LogDetailDrawer.jsx`](frontend/src/app/pages/settings/sections/developer/logs/drawers/LogDetailDrawer.jsx) [NEW] — Drawer detail log: metadata, stack trace, konteks request, informasi user.
  - **Frontend — Customer PKS & Privilege**:
    - [`frontend/src/app/pages/users/customerPKS/CustomerPKSDocumentPreview.jsx`](frontend/src/app/pages/users/customerPKS/CustomerPKSDocumentPreview.jsx) — Pembaruan signifikan (+303 baris) komponen pratinjau dokumen PKS.
    - [`frontend/src/app/pages/users/customerPKS/CustomerPKSReviewDrawer.jsx`](frontend/src/app/pages/users/customerPKS/CustomerPKSReviewDrawer.jsx`) — Penyesuaian drawer review.
    - [`frontend/src/app/pages/users/customerPKS/create.jsx`](frontend/src/app/pages/users/customerPKS/create.jsx) — Pembaruan form buat Customer PKS.
    - [`frontend/src/app/pages/users/customerPKS/edit.jsx`](frontend/src/app/pages/users/customerPKS/edit.jsx) — Pembaruan form edit Customer PKS.
    - [`frontend/src/app/pages/users/customerPKS/schema/customerPKSSchema.js`](frontend/src/app/pages/users/customerPKS/schema/customerPKSSchema.js) — Penyesuaian skema validasi.
    - [`frontend/src/app/pages/users/document/pks/schema/columns.jsx`](frontend/src/app/pages/users/document/pks/schema/columns.jsx) — Penyesuaian kolom dokumen PKS.
    - [`frontend/src/app/pages/users/document/shared/useDocumentApproval.js`](frontend/src/app/pages/users/document/shared/useDocumentApproval.js) — Penyesuaian hook approval dokumen.
    - [`frontend/src/app/pages/users/privilege/create.jsx`](frontend/src/app/pages/users/privilege/create.jsx) — Pembaruan form privilege.
    - [`frontend/src/app/pages/users/privilege/detail.jsx`](frontend/src/app/pages/users/privilege/detail.jsx) — Pembaruan detail privilege.
    - [`frontend/src/app/pages/users/privilege/edit.jsx`](frontend/src/app/pages/users/privilege/edit.jsx) — Pembaruan edit privilege.
  - **Frontend — Komponen Shared & UI**:
    - [`frontend/src/components/shared/PageHeader.jsx`](frontend/src/components/shared/PageHeader.jsx) [NEW] — Komponen header halaman reusable dengan judul, deskripsi, tombol aksi, dan breadcrumbs.
    - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx) — Penambahan cell wrapper untuk badge status.
    - [`frontend/src/components/shared/table/status.js`](frontend/src/components/shared/table/status.js) [NEW] — Utilitas konstanta dan helper untuk status badge (warna, label).
    - [`frontend/src/components/shared/table/Table.jsx`](frontend/src/components/shared/table/Table.jsx) — Penyesuaian komponen tabel.
    - [`frontend/src/components/shared/DocumentPreviewModal.jsx`](frontend/src/components/shared/DocumentPreviewModal.jsx) — Penyesuaian modal pratinjau dokumen.
    - [`frontend/src/components/ui/Modal.jsx`](frontend/src/components/ui/Modal.jsx) — Penyesuaian komponen modal.
  - **Frontend — Routing & Middleware**:
    - [`frontend/src/app/router/finance/assets.jsx`](frontend/src/app/router/finance/assets.jsx) [NEW] — Route modul aset tetap.
    - [`frontend/src/app/router/finance/regulatory.jsx`](frontend/src/app/router/finance/regulatory.jsx) [NEW] — Route modul kewajiban regulasi.
    - [`frontend/src/app/router/finance/payroll.jsx`](frontend/src/app/router/finance/payroll.jsx) — Penghapusan route payroll settings lama.
    - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx) — Penambahan route aset dan regulasi.
    - [`frontend/src/app/router/public.jsx`](frontend/src/app/router/public.jsx) — Penyesuaian route publik.
    - [`frontend/src/middleware/AuthGuard.jsx`](frontend/src/middleware/AuthGuard.jsx) — Penyesuaian guard autentikasi.
  - **Frontend — Navigasi & i18n**:
    - [`frontend/src/app/navigation/finance.js`](frontend/src/app/navigation/finance.js) — Pembaruan menu navigasi keuangan: tambah akses Aset Tetap dan Kewajiban Regulasi.
    - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](frontend/src/i18n/locales/id/translations.json) — Penambahan 318+ kunci i18n baru untuk seluruh modul baru.
    - [`frontend/src/styles/layouts.css`](frontend/src/styles/layouts.css) — Penambahan style untuk komponen layout baru.
  - **Frontend — Update Lintas Modul**:
    - Pembaruan `import PageHeader` pada ~80+ halaman modul (tickets, services, users, settings, warehouse, network) dari komponen header statik ke komponen `PageHeader` reusable.
- **Deskripsi Perubahan & Fungsi**:
  - Mengintegrasikan seluruh modul keuangan baru (Aset Tetap, Manajemen Periode, Kewajiban Regulasi, Slip Gaji) ke dalam kode produksi master setelah melalui pengujian komprehensif.
  - Melengkapi seluruh laporan keuangan standar akuntansi: Neraca, Laba-Rugi, Arus Kas, Neraca Saldo, Umur Piutang, Umur Hutang, Buku Besar Multi-Akun, Laporan Aset Tetap, Laporan Penjualan per Area/Produk.
  - Mengimplementasikan sistem manajemen periode akuntansi untuk memastikan integritas data transaksi melalui mekanisme close/open/lock periode.
  - Menambahkan fitur Developer System Logs dengan analisis AI Gemini untuk debugging dan monitoring produksi.
  - Memperluas sistem privilege RBAC dengan 20+ hak akses baru untuk modul-modul keuangan terbaru.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                    | Dampak Utama                                                                                                          |
| ----- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| #257  | Docker Production Deployment, Finance Recurring & Payment Crash Recovery | Infrastruktur deploy produksi siap pakai, pembayaran berulang otomatis, ketahanan sistem pembayaran terhadap crash    |
| #183  | Financial Module Enhancement & G3 Accounting Standards Compliance        | Modul keuangan lengkap: aset tetap, periode akuntansi, kewajiban regulasi, slip gaji, laporan standar akuntansi penuh |

### Kemampuan Baru Pengguna/Admin

- **Pembayaran Berulang Otomatis**: Admin dapat mengatur jadwal pembayaran berulang (harian/mingguan/bulanan/tahunan) untuk transaksi rutin seperti sewa, langganan, atau cicilan. Sistem akan otomatis mengeksekusi pembayaran sesuai jadwal dengan validasi saldo wallet sumber.
- **Crash Recovery Pembayaran**: Sistem pembayaran invoice kini memiliki mekanisme pemulihan otomatis jika terjadi crash di tengah pemrosesan, memastikan tidak ada data yang hilang atau tidak konsisten.
- **Manajemen Aset Tetap**: Admin dapat mencatat, melacak, menghitung depresiasi (metode straight-line), dan melakukan disposal aset tetap perusahaan. Laporan nilai buku dan akumulasi depresiasi tersedia.
- **Manajemen Periode Akuntansi**: Admin dapat membuka/menutup/mengunci periode akuntansi bulanan untuk mencegah posting transaksi backdate atau ke periode yang sudah ditutup.
- **Kewajiban Regulasi**: Sistem dapat mengelola kewajiban pajak dan iuran (PPN, PPh, BPJS) termasuk generate otomatis, pencatatan pembayaran, dan pelacakan jatuh tempo.
- **Slip Gaji Digital**: Admin dapat mencetak slip gaji karyawan dengan detail pendapatan dan potongan.
- **Docker One-Command Deploy**: Seluruh stack aplikasi dapat di-deploy dengan satu perintah Docker Compose.
- **Analisis Log AI**: Developer dapat menganalisis log error menggunakan AI Gemini langsung dari panel Developer Logs.

### Bug Fix / Solusi Masalah

- **Konsistensi Saldo Buku Besar**: Penerapan filter `deleted: { $ne: true }` di seluruh fungsi kalkulasi saldo kas untuk mencegah inkonsistensi data (Temuan B1).
- **Pembalikan Jurnal Deterministik**: Pemetaan pembatalan jurnal `reversed_by` berbasis `leg_key` deterministik untuk mencegah inkonsistensi penunjukan baris jurnal (Temuan A6).
- **Kaskade Hierarki COA**: Perhitungan ulang otomatis field `path` dan `level` ke seluruh akun turunan saat kode akun atau induk diperbarui, serta validasi pencegahan struktur hierarki melingkar (Temuan B4 & B5).
- **Larangan Posting Periode Terkunci**: Penegakan larangan posting ke periode akuntansi yang sudah ditutup/dikunci (Temuan B6).
- **Operasi `$unset` yang Valid**: Penggantian syntax `$set: { last_error: undefined }` menjadi `$unset: { last_error: '' }` yang valid di Mongoose/MongoDB (Temuan B10).
- **Saldo Awal Akun Nonaktif**: Penghapusan filter `status: 'active'` agar akun nonaktif yang masih memiliki sisa saldo tetap tercermin akurat di Neraca (Temuan C7).
- **Deteksi Saldo Awal Ganda**: Pendeteksi konflik antara field `opening_balance` pada master COA dan jurnal saldo awal (Temuan C8).
- **Validasi Relasi Hapus COA**: Pengecekan relasi terhadap transaksi belum dijurnal, jadwal transaksi berulang, dan draf transaksi pending sebelum menghapus atau menonaktifkan akun COA (Temuan C10).

### Menu/Fitur Baru

- **Finance > Aset Tetap** — Manajemen aset tetap perusahaan (CRUD, depresiasi, dispose)
- **Finance > Kewajiban Regulasi** — Manajemen kewajiban pajak dan iuran (PPN, PPh, BPJS)
- **Finance > Laporan > Arus Kas** — Laporan Arus Kas metode tidak langsung
- **Finance > Laporan > Umur Piutang** — Laporan AR Aging dengan kartu metrik
- **Finance > Laporan > Aset Tetap** — Laporan daftar aset dan nilai buku
- **Finance > Laporan > Penjualan per Area** — Laporan penjualan berdasarkan wilayah
- **Finance > Laporan > Penjualan per Produk** — Laporan penjualan berdasarkan produk/layanan
- **Finance > Laporan > Cetak Buku Besar** — Fitur cetak Buku Besar multi-akun
- **Finance > Transaksi > Draf Tertunda** — Tab terpisah untuk draf transaksi yang menunggu persetujuan
- **Finance > Pengaturan Gaji > Settings Modal** — Pengaturan payroll terintegrasi
- **Finance > Pengaturan Gaji > Laporan Pembayaran** — Laporan pembayaran gaji
- **Settings > System** — Pengaturan sistem yang diperluas
- **Settings > Developer Logs > Analisis AI** — Modal analisis log error dengan AI

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Modul **Finance Recurring Transaction** memungkinkan admin mengotomatiskan pencatatan transaksi keuangan berulang (misal: pembayaran sewa bulanan, cicilan pinjaman, iuran rutin). Admin cukup mengatur sekali dengan parameter: jumlah, wallet sumber & tujuan, akun debit & kredit, serta jadwal eksekusi. Sistem akan menjalankan transaksi secara otomatis sesuai jadwal yang ditentukan, dengan validasi saldo sebelum eksekusi dan pencatatan status setiap kali dijalankan.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Finance > Transaksi Berulang** (akan tersedia setelah branch di-merge).
  2. Klik tombol **Tambah Baru** untuk membuat jadwal transaksi berulang baru.
  3. Isi deskripsi, jumlah transaksi, pilih wallet sumber (debit) dan wallet tujuan (kredit).
  4. Pilih akun perkiraan debit dan kredit yang sesuai.
  5. Atur jadwal: pilih tipe (Harian/Mingguan/Bulanan/Tahunan), interval, dan parameter jadwal spesifik (hari dalam minggu, tanggal dalam bulan, atau bulan dalam tahun).
  6. Tentukan tanggal mulai, tanggal akhir (opsional), dan batas jumlah eksekusi (opsional).
  7. Simpan — status awal adalah **Aktif**. Sistem akan menghitung waktu eksekusi berikutnya secara otomatis.
  8. Untuk menjeda sementara, ubah status ke **Paused**. Untuk membatalkan permanen, hapus jadwal tersebut.
