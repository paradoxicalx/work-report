# 📝 Daily Work Report - Dedy S.N Putra (2 November 2026)

---

## 📅 Laporan Harian - 2 November 2026

---

## 🌿 Branch: `issue-516` — Arsitektur Job Latar Cron Worker, Riwayat Eksekusi Permanen Dashboard, Pengayaan Relasi NAS Router pada Sesi & Log RADIUS, serta Otomasi Callback URL iPaymu

### 📌 Informasi Issue

- **Nomor Issue**: #516
- **Judul Issue**: Arsitektur Job Latar Cron Worker (Long-running Background Jobs), Riwayat Eksekusi Permanen Dashboard Cron, Pengayaan Relasi NAS Router pada Sesi & Log RADIUS, serta Otomasi Callback URL iPaymu
- **Status Branch**: `Belum di-merge` (Perubahan aktif lokal / *Work In Progress* di branch `issue-516`)

### 📅 Rincian Perubahan (Work In Progress)

#### [WIP] - Penyempurnaan Job Latar, Riwayat Eksekusi Cron, dan Pengayaan Relasi NAS Router

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/radiusControl.controller.js`
  - `backend/src/controllers/radiusNas.controller.js`
  - `backend/src/controllers/radiusSession.controller.js`
  - `backend/src/models/radiusNas.model.js`
  - `backend/src/routes/internal.route.js`
  - `backend/src/routes/radiusNas.route.js`
  - `backend/src/services/cronJob.service.js` [NEW]
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/src/services/radiusNas.service.js`
  - `backend/src/services/radiusSession.service.js`
  - `backend/src/utils/ipayCallbackUrl.js` [NEW]
  - `backend/test/integration/financeSettings.ipayCallback.test.js` [NEW]
  - `backend/test/integration/internalCronJob.controller.test.js` [NEW]
  - `backend/test/integration/radiusNas.create.test.js` [NEW]
  - `backend/test/integration/radiusSession.nasEnrichment.test.js` [NEW]
  - `backend/test/unit/cronJob.service.test.js` [NEW]
  - `backend/test/unit/ipayCallbackUrl.test.js` [NEW]
  - `cron-worker/src/controllers/cron.controller.js`
  - `cron-worker/src/jobs/lastRun.js` [NEW]
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `cron-worker/src/services/cronJobPoller.js` [NEW]
  - `cron-worker/test/cronJobPoller.test.js` [NEW]
  - `cron-worker/test/lastRun.test.js` [NEW]
  - `frontend/src/app/pages/dashboards/radius/schema/sessionColumns.jsx`
  - `frontend/src/app/pages/network/radiusSession/schema/columns.jsx`
  - `frontend/src/app/pages/services/broadband/detail.jsx`
  - `frontend/src/app/pages/services/broadband/schema/columnsLogs.jsx`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/IpaymuGatewaySettings.jsx`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/providerLogic.js`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/providerLogic.test.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `radius-server/DOCUMENTATION.md`
  - `radius-server/internal/repository/mongo/hotspot_session_repo.go`
  - `radius-server/internal/repository/mongo/session_repo.go`
  - `radius-server/test/integration/session_nasip_test.go`
  - `radius-server/test/testutil/fake_repos.go`
- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Job Latar Cron Worker (Pencegahan 504 Gateway Timeout)**:
    - Membangun utilitas `runCronJob` dan `respondWithCronJob` (`cronJob.service.js`) berbasis status Redis terdistribusi. Menangani pekerjaan periodik backend yang memakan waktu lebih dari 60 detik (seperti `invoice-freeze`, `finance-gateway-sweep`, `finance-gateway-history-reconcile`, `auto-invoice-generate`, `syslog-hourly-digest`, dan `syslog-daily-report`).
    - Menyediakan mekanisme tunggu sinkron hingga 20 detik; apabila belum selesai, backend langsung mengembalikan respons `202 Accepted` bersama `job_id`, sementara eksekusi terus berlanjut di latar belakang.
    - Menghadirkan `cronJobPoller.js` dan metode `postLongRunning` pada `api.service.js` di `cron-worker` untuk memolling status pekerjaan secara periodik hingga selesai (dengan batas waktu hingga 30 menit dan toleransi kesalahan jaringan sesaat). Hal ini mengeliminasi pemutusan sepihak (*504 Gateway Timeout*) dari reverse proxy (Nginx/Traefik/Cloudflare).
  - **Persistensi Riwayat Eksekusi Terakhir Cron Worker**:
    - Mengatasi masalah tampilan dashboard jadwal cron yang keliru menampilkan status "Gagal" atau "Eksekusi Terakhir: -" akibat pembersihan otomatis queue BullMQ (`removeOnComplete: 20`).
    - Menambahkan modul `lastRun.js` yang secara andal mencatat hasil eksekusi final tiap job ke Redis hash `cron-worker:last-runs`, sehingga endpoint `getSchedulesHandler` di `cron.controller.js` selalu menyajikan data riwayat eksekusi dan durasi yang presisi.
  - **Pengayaan & Penyaringan Relasi Router NAS pada Sesi & Log RADIUS**:
    - Mengimplementasikan `getRadiusNasMap()` dan `enrichWithNasInfo()` pada `radiusSession.service.js` untuk memetakan alamat IP router ke entitas `RadiusNas` (`nas_id`, `name`, `ip_address`) secara in-memory dan efisien.
    - Menambahkan logika filter multi-kolom di `radiusSession.controller.js` dan `radiusControl.controller.js` agar administrator dapat menyaring data sesi aktif, riwayat sesi, dan log autentikasi broadband langsung berdasarkan nama atau IP router NAS.
    - Memperbarui repository MongoDB `radius-server` (`session_repo.go`) dan skema frontend (`sessionColumns.jsx`, `columns.jsx`, `columnsLogs.jsx`) untuk menampilkan chip nama router NAS berserta tautan pintasan ke detail perangkat jaringan.
  - **Otomasi Callback URL Notifikasi Payment Gateway iPaymu**:
    - Menghadirkan modul `ipayCallbackUrl.js` untuk menyusun URL webhook callback pembayaran iPaymu secara dinamis dan presisi.
    - Memperbarui antarmuka `IpaymuGatewaySettings.jsx` dan `providerLogic.js` pada pengaturan keuangan untuk memvalidasi dan menampilkan format URL callback yang siap digunakan di dashboard merchant iPaymu.

---

## 🌿 Branch: `issue-510` — Pengelolaan Penarikan Dana Payment Gateway (Withdrawal Ingestion & Recording), Rekonsiliasi Saldo Awal Gateway, dan Pemulihan Non-Payment

### 📌 Informasi Issue

- **Nomor Issue**: #510
- **Judul Issue**: Pengelolaan Penarikan Dana Payment Gateway (Withdrawal Ingestion & Recording), Rekonsiliasi Saldo Awal Gateway, dan Pemulihan Transaksi Non-Payment
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Senin, 2 November 2026, 17:49:54 WIB via commit `9becfd36`, disinkronkan ke branch `production` via commit `9152639a`)

### 📅 Rincian Commit

#### [9152639a] - Merge branch 'master' into production - Senin, 2 November 2026, 17:51:38 WIB

_(Merge commit menyinkronkan seluruh pembaruan issue #510 dari branch `master` ke branch `production`)_

---

#### [9becfd36] - resolve #510 - Senin, 2 November 2026, 17:49:54 WIB

- **Komponen yang Berubah**:
  - `frontend/src/app/pages/finance/gateway/GatewayWithdrawalsPanel.jsx`
  - `frontend/src/app/pages/finance/gateway/schema/withdrawalColumns.jsx`
  - `frontend/src/app/pages/finance/gateway/schema/withdrawalColumns.test.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Penyempurnaan Antarmuka Penarikan Dana Gateway**:
    - Menambahkan penanganan status mutasi penarikan dana pada panel penarikan (`GatewayWithdrawalsPanel.jsx`) dan tabel kolom mutasi (`withdrawalColumns.jsx`).
    - Menyempurnakan pengujian unit komponen kolom penarikan (`withdrawalColumns.test.jsx`) guna memastikan validasi status, rendering biaya transaksi, dan aksi drawer pencatatan berjalan tanpa regresi.

---

#### [2e0fa288] - Merge branch 'master' into production - Senin, 2 November 2026, 17:23:45 WIB

_(Merge commit menyinkronkan rilis awal issue #510 ke branch `production`)_

---

#### [0a2bbf53] - resolve #510 - Senin, 2 November 2026, 17:23:15 WIB

_(Merge commit menggabungkan branch `issue-510` ke branch `master`)_

---

#### [1c1e9eee] - resolve #510 - Senin, 2 November 2026, 17:18:03 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/financeGateway.controller.js`
  - `backend/src/controllers/financeGatewayWithdrawal.controller.js` [NEW]
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeGatewayWithdrawal.model.js` [NEW]
  - `backend/src/routes/financeGateway.route.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeGatewayOpening.service.js` [NEW]
  - `backend/src/services/financeGatewayWithdrawal.service.js` [NEW]
  - `backend/src/services/financeGatewayWithdrawalRecord.service.js` [NEW]
  - `backend/src/services/financeTransfer.service.js`
  - `backend/src/services/paymentGateways/ipaymu.gateway.js`
  - `backend/src/services/paymentGateways/registry.js`
  - `backend/src/services/paymentGateways/xendit.gateway.js`
  - `backend/src/services/recovery.service.js`
  - `backend/src/utils/finance-error.js`
  - `backend/test/integration/financeGateway.historyWithdrawal.test.js` [NEW]
  - `backend/test/integration/financeGateway.sweep.test.js`
  - `backend/test/integration/gatewayNonPaymentRecovery.test.js` [NEW]
  - `backend/test/integration/gatewayOpening.test.js` [NEW]
  - `backend/test/integration/gatewayWithdrawal.controller.test.js` [NEW]
  - `backend/test/integration/gatewayWithdrawal.fetchIngest.test.js` [NEW]
  - `backend/test/integration/gatewayWithdrawal.ingest.test.js` [NEW]
  - `backend/test/integration/gatewayWithdrawal.list.test.js` [NEW]
  - `backend/test/integration/gatewayWithdrawal.record.test.js` [NEW]
  - `backend/test/integration/xenditGateway.createPayment.test.js`
  - `backend/test/integration/xenditGateway.withdrawals.test.js` [NEW]
  - `backend/test/unit/gatewayWithdrawalContract.test.js` [NEW]
  - `backend/test/unit/ipaymuGateway.normalize.test.js`
  - `backend/test/unit/xenditWithdrawal.normalize.test.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/GatewayWithdrawalsPanel.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/OpeningBalancePanel.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/WithdrawalIgnoreModal.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/WithdrawalRecordDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/index.jsx`
  - `frontend/src/app/pages/finance/gateway/schema/withdrawalColumns.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/schema/withdrawalColumns.test.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/withdrawals.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/withdrawals.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.test.js`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Model & Ingesti Penarikan Dana Payment Gateway**:
    - Membuat model `FinanceGatewayWithdrawal` (`financeGatewayWithdrawal.model.js`) untuk menyimpan riwayat transaksi penarikan saldo payment gateway (payout/disbursement) ke rekening bank riil perusahaan.
    - Mengintegrasikan penarikan data mutasi otomatis pada gateway adapter Xendit (`xendit.gateway.js`) dan iPaymu (`ipaymu.gateway.js`) melalui service `financeGatewayWithdrawal.service.js`.
  - **Pencatatan Buku Besar Penarikan Dana (Withdrawal Recording)**:
    - Menghadirkan service `financeGatewayWithdrawalRecord.service.js` untuk membukukan transaksi penarikan dana ke kas/bank perusahaan secara atomik, memperbarui saldo buku besar, mencatat biaya penarikan, serta mencegah pencatatan ganda.
    - Menyediakan opsi pengabaian (*ignore*) melalui `WithdrawalIgnoreModal.jsx` untuk transaksi penarikan yang telah dibukukan secara manual di luar sistem.
  - **Rekonsiliasi Saldo Awal Gateway (Opening Balance Management)**:
    - Mengembangkan `financeGatewayOpening.service.js` dan panel `OpeningBalancePanel.jsx` untuk mengatur dan mengunci saldo awal akun payment gateway pada periode tertentu, menjamin angka rekonsiliasi kas tidak bergeser (*drift*).
  - **Pemulihan Transaksi Non-Payment Gateway**:
    - Menambahkan modul `gatewayNonPaymentRecovery.test.js` dan fungsi pada `recovery.service.js` untuk mengidentifikasi serta memulihkan transaksi gateway non-pembayaran (misal biaya administrasi, transfer balik, atau penyesuaian saldo manual).
  - **Antarmuka & Pengujian Lengkap**:
    - Merancang tab Penarikan Dana dan Saldo Awal pada antarmuka Keuangan Payment Gateway (`/finance/gateway`).
    - Menyusun 12 rangkaian tes integrasi dan unit baru untuk menguji kontrak API withdrawal, normalisasi respons Xendit/iPaymu, dan alur pencatatan akuntansi.

---

## 🌿 Branch: `issue-511` — Otomasi Freeze Faktur Terlambat, Pemulihan Sesi Terisolir, Format Cetak Faktur, dan Build Info Radius Server

### 📌 Informasi Issue

- **Nomor Issue**: #511
- **Judul Issue**: Otomasi Freeze Faktur Terlambat (Invoice Freeze Policy), Pemulihan Sesi Terisolir (Frozen Session Recovery), Format Cetak Faktur, dan Sinkronisasi Build Info Radius Server
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Senin, 2 November 2026, 12:53:04 WIB via commit `dc4d38d4`, disinkronkan ke branch `production` via commit `6ab813c4`)

### 📅 Rincian Commit

#### [6ab813c4] - Merge branch 'master' into production - Senin, 2 November 2026, 17:00:30 WIB

_(Merge commit menyinkronkan pembaruan lanjutan pemulihan sesi terisolir ke branch `production`)_

---

#### [135f11fc] - resolve #511 - Senin, 2 November 2026, 16:59:12 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/radiusAuthentication.model.js`
  - `backend/src/routes/radiusAuthentication.route.js`
  - `backend/src/services/recovery.service.js`
  - `backend/test/integration/frozenSessionRecovery.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.test.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `radius-server/DOCUMENTATION.md`
  - `radius-server/internal/domain/accounting/handlers.go`
  - `radius-server/internal/domain/accounting/handlers_test.go`
  - `radius-server/internal/domain/ports/types.go`
  - `radius-server/internal/repository/mongo/authentication_repo.go`
  - `radius-server/internal/repository/mongo/model/authentication.go`
  - `radius-server/internal/transport/pppoe/acct_listener.go`
  - `radius-server/test/integration/usage_aggregate_test.go`
  - `radius-server/test/testutil/fake_repos.go`
- **Deskripsi Perubahan & Fungsi**:
  - **Alat Pemulihan Sesi Terisolir (Frozen Session Recovery Tool)**:
    - Membangun service pemulihan `frozenSessionRecovery` pada `recovery.service.js` dan endpoint API `radiusAuthentication.route.js` untuk mendeteksi dan memperbaiki perbedaan (*drift*) status isolir antara pelanggan bertagihan jatuh tempo dengan kredensial RADIUS broadband/hotspot.
    - Menambahkan rute dan registrasi alat pemulihan pada antarmuka Pengaturan Pengembang (`RecoveryToolsTab.jsx`), memungkinkan eksekusi simulasi (*dry-run*) maupun eksekusi perbaikan langsung oleh developer.
  - **Sinkronisasi Status Freeze pada RADIUS Accounting**:
    - Memperbarui listener accounting PPPoE (`acct_listener.go`) dan handler domain Go (`handlers.go`) di `radius-server` agar mengenali sesi terisolir serta memperbarui status isolir secara konsisten di database MongoDB.

---

#### [262a55c5] - Merge branch 'master' into production - Senin, 2 November 2026, 13:40:12 WIB

_(Merge commit menyinkronkan rilis awal issue #511 ke branch `production`)_

---

#### [dc4d38d4] - resolve #511 - Senin, 2 November 2026, 12:53:04 WIB

_(Merge commit menggabungkan branch `issue-511` ke branch `master`)_

---

#### [30931418] - resolve #511 - Senin, 2 November 2026, 10:52:11 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/financeInvoice.controller.js`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/publicInvoice.controller.js`
  - `backend/src/routes/internal.route.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/invoiceFreeze.service.js`
  - `backend/src/services/recovery.service.js`
  - `backend/src/utils/invoiceStatusFilter.js` [NEW]
  - `backend/test/integration/financeAutoInvoice.simulation.test.js`
  - `backend/test/integration/financeInvoice.overdueFilter.test.js` [NEW]
  - `backend/test/integration/financeInvoice.update.test.js`
  - `backend/test/integration/googleDriveScope.util.test.js`
  - `backend/test/integration/invoiceFreeze.staleOnline.test.js` [NEW]
  - `backend/test/integration/invoiceFreeze.test.js`
  - `backend/test/integration/publicInvoice.dto.test.js` [NEW]
  - `cron-worker/src/jobs/repeatableRefresh.js` [NEW]
  - `cron-worker/src/jobs/retryPolicy.js` [NEW]
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/services/api.service.js`
  - `cron-worker/test/invoiceFreezePolicy.test.js` [NEW]
  - `cron-worker/test/repeatableRefresh.test.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`
  - `frontend/src/app/pages/finance/invoices/InvoiceDocument.test.jsx` [NEW]
  - `frontend/src/app/pages/finance/invoices/detail.jsx`
  - `frontend/src/app/pages/finance/invoices/utils/printableInvoice.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/utils/printableInvoice.test.js` [NEW]
  - `frontend/src/app/pages/public/registration.jsx`
  - `frontend/src/app/pages/services/broadband/create.jsx`
  - `frontend/src/app/pages/services/broadband/edit.jsx`
  - `frontend/src/app/pages/services/broadband/editBatch.jsx`
  - `frontend/src/app/pages/services/broadbandPackage/create.jsx`
  - `frontend/src/app/pages/services/broadbandPackage/edit.jsx`
  - `frontend/src/app/pages/services/broadbandPackage/editBatch.jsx`
  - `frontend/src/app/pages/services/broadbandProfile/create.jsx`
  - `frontend/src/app/pages/services/broadbandProfile/edit.jsx`
  - `frontend/src/app/pages/services/broadbandProfile/editBatch.jsx`
  - `frontend/src/app/pages/services/dataAccess/create.jsx`
  - `frontend/src/app/pages/users/admin/create.jsx`
  - `frontend/src/app/pages/users/admin/edit.jsx`
  - `frontend/src/app/pages/users/admin/editBatch.jsx`
  - `frontend/src/app/pages/users/business/edit.jsx`
  - `frontend/src/app/pages/users/business/editBatch.jsx`
  - `frontend/src/app/pages/users/customer/create.jsx`
  - `frontend/src/app/pages/users/customer/edit.jsx`
  - `frontend/src/app/pages/users/customer/editBatch.jsx`
  - `frontend/src/app/pages/users/employee/create.jsx`
  - `frontend/src/app/pages/users/employee/edit.jsx`
  - `frontend/src/app/pages/users/employee/editBatch.jsx`
  - `frontend/src/app/pages/users/partner/edit.jsx`
  - `frontend/src/app/pages/users/partner/editBatch.jsx`
  - `frontend/src/components/shared/form/LocationPickerInput.jsx`
  - `frontend/src/components/shared/table/invoiceCells.test.jsx` [NEW]
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/invoiceOverdue.js` [NEW]
  - `frontend/src/utils/invoiceOverdue.test.js` [NEW]
  - `frontend/src/utils/scheduleMapResize.js` [NEW]
  - `frontend/src/utils/scheduleMapResize.test.js` [NEW]
  - `frontend/src/utils/serverFieldErrors.js` [NEW]
  - `frontend/src/utils/serverFieldErrors.test.js` [NEW]
  - `partner-frontend/src/components/shared/form/LocationPickerInput.jsx`
  - `partner-frontend/src/utils/scheduleMapResize.js` [NEW]
  - `partner-frontend/src/utils/scheduleMapResize.test.js` [NEW]
  - `radius-server/TUTORIAL_DEPLOY_PM2.md`
  - `radius-server/cmd/radiusd/buildinfo.go` [NEW]
  - `radius-server/cmd/radiusd/buildinfo_test.go` [NEW]
  - `radius-server/cmd/radiusd/main.go`
  - `radius-server/internal/transport/hotspot/auth_listener.go`
  - `radius-server/internal/transport/hotspot/auth_listener_test.go`
  - `radius-server/test/testutil/fake_repos.go`
- **Deskripsi Perubahan & Fungsi**:
  - **Kebijakan Isolasi Otomatis Faktur Jatuh Tempo (Invoice Freeze Policy)**:
    - Mengembangkan `invoiceFreeze.service.js` pada backend dan penjadwal BullMQ `scheduler.js` pada cron-worker untuk mengisolasi layanan pelanggan broadband yang melewati batas waktu toleransi pembayaran secara otomatis.
    - Menangani kasus sesi online usang (*stale online sessions*) agar status pemutusan koneksi pada router sinkron dengan database.
  - **Standarisasi Dokumen Cetak Faktur (Printable Invoice)**:
    - Merancang ulang modul `InvoiceDocument.jsx` dan utilitas `printableInvoice.js` untuk menghasilkan tata letak faktur cetak profesional yang siap dicetak langsung (*browser print/PDF*), memuat rincian Dasar Pengenaan Pajak (DPP), rincian potongan, dan informasi nomor rekening tujuan.
  - **Optimalisasi Antarmuka & Penanganan Kesalahan**:
    - Menambahkan `serverFieldErrors.js` pada frontend untuk memetakan pesan kegagalan validasi backend secara otomatis ke setiap field input formulir secara presisi.
    - Menambahkan `scheduleMapResize.js` pada `LocationPickerInput.jsx` untuk memperbaiki bug peta Leaflet yang tidak ter-render sempurna (*grey tiles*) saat pertama kali modal lokasi dibuka.
  - **Injeksi Informasi Build pada Radius Server**:
    - Membuat modul `buildinfo.go` pada daemon RADIUS Go untuk menyajikan informasi nomor versi, commit hash, dan tanggal kompilasi secara transparan via parameter CLI atau log awal (*startup log*).
    - Menyusun dokumentasi deployment `TUTORIAL_DEPLOY_PM2.md` untuk menjalankan service RADIUS server di lingkungan produksi.

---

## 🌿 Branch: `issue-509` — Monitoring NAS Accounting Log, Panel Riwayat PPPoE, Origin NAS pada Dashboard RADIUS, dan Alat Pemulihan Penggunaan Profil

### 📌 Informasi Issue

- **Nomor Issue**: #509
- **Judul Issue**: Monitoring NAS Accounting Log, Panel Riwayat PPPoE, Origin NAS pada Dashboard RADIUS, dan Alat Pemulihan Penggunaan Profil Radius (Profile Usage Recovery)
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Senin, 2 November 2026, 00:07:56 WIB via commit `01cfd9f2`, disinkronkan ke branch `production` via commit `df7f4292`)

### 📅 Rincian Commit

#### [df7f4292] - Merge branch 'master' into production - Senin, 2 November 2026, 00:08:35 WIB

_(Merge commit menyinkronkan seluruh hasil issue #509 ke branch `production`)_

---

#### [01cfd9f2] - resolve #509 - Senin, 2 November 2026, 00:07:56 WIB

_(Merge commit menggabungkan branch `issue-509` ke branch `master`)_

---

#### [cee6e412] - resolve #509 - Senin, 2 November 2026, 00:06:57 WIB

- **Komponen yang Berubah**:
  - `backend/proto/radius/v1/radius.proto`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/radiusControl.controller.js`
  - `backend/src/controllers/radiusNas.controller.js`
  - `backend/src/controllers/radiusProfileUsage.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/radiusSession.model.js`
  - `backend/src/routes/radiusControl.route.js`
  - `backend/src/routes/radiusNas.route.js`
  - `backend/src/routes/radiusProfile.route.js`
  - `backend/src/services/radiusEvent.service.js`
  - `backend/src/services/radiusNasAccountingLog.service.js` [NEW]
  - `backend/src/services/radiusProfileUsage.service.js` [NEW]
  - `backend/src/services/radiusServerRegistry.service.js`
  - `backend/src/services/radiusStats.service.js`
  - `backend/test/integration/radiusDashboardServers.test.js` [NEW]
  - `backend/test/integration/radiusNasAccountingLog.test.js` [NEW]
  - `backend/test/integration/radiusProfileUsage.test.js` [NEW]
  - `backend/test/setup/redis.stub.js`
  - `frontend/src/app/pages/dashboards/radius/components/CustomerCell.jsx` [NEW]
  - `frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/NasCell.jsx` [NEW]
  - `frontend/src/app/pages/dashboards/radius/components/NasOriginPanel.jsx` [NEW]
  - `frontend/src/app/pages/dashboards/radius/components/PppoeDisconnectPanel.jsx` [DELETED]
  - `frontend/src/app/pages/dashboards/radius/components/PppoeHistoryPanel.jsx` [NEW]
  - `frontend/src/app/pages/dashboards/radius/components/ServerList.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/dashboardPanels.test.jsx` [NEW]
  - `frontend/src/app/pages/dashboards/radius/hooks/useRadiusStream.js`
  - `frontend/src/app/pages/dashboards/radius/index.jsx`
  - `frontend/src/app/pages/dashboards/radius/utils/nasAttempts.js` [NEW]
  - `frontend/src/app/pages/dashboards/radius/utils/nasAttempts.test.js` [NEW]
  - `frontend/src/app/pages/network/radiusNas/components/AccountingLogSection.jsx` [NEW]
  - `frontend/src/app/pages/network/radiusNas/components/AccountingLogSection.test.jsx` [NEW]
  - `frontend/src/app/pages/network/radiusNas/detail.jsx`
  - `frontend/src/app/pages/network/radiusNas/utils/accountingLog.js` [NEW]
  - `frontend/src/app/pages/network/radiusNas/utils/accountingLog.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/ProfileUsageModal.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/ProfileUsageRow.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/ProfileUsageRow.test.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/RecoveryToolsTab.jsx`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/profileUsage.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/profileUsage.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `radius-server/DOCUMENTATION.md`
  - `radius-server/cmd/radiusd/main.go`
  - `radius-server/cmd/radiusd/wire.go`
  - `radius-server/cmd/radiusd/wire_test.go`
  - `radius-server/gen/radius/v1/radius.pb.go`
  - `radius-server/internal/domain/ports/eventpublisher.go`
  - `radius-server/internal/domain/ports/repositories.go`
  - `radius-server/internal/domain/ports/types.go`
  - `radius-server/internal/repository/mongo/hotspot_session_repo.go`
  - `radius-server/internal/repository/mongo/model/hotspot_session.go`
  - `radius-server/internal/repository/mongo/model/session.go`
  - `radius-server/internal/repository/mongo/session_repo.go`
  - `radius-server/internal/transport/grpcclient/client_test.go`
  - `radius-server/internal/transport/grpcclient/publisher.go`
  - `radius-server/internal/transport/hotspot/auth_listener.go`
  - `radius-server/internal/transport/hotspot/auth_listener_test.go`
  - `radius-server/internal/transport/pppoe/acct_listener.go`
  - `radius-server/internal/transport/pppoe/acct_listener_log_test.go` [NEW]
  - `radius-server/internal/transport/pppoe/acct_listener_test.go`
  - `radius-server/proto/radius/v1/radius.proto`
  - `radius-server/test/integration/session_server_test.go` [NEW]
  - `radius-server/test/testutil/fake_repos.go`
- **Deskripsi Perubahan & Fungsi**:
  - **Monitoring Log Accounting Router NAS**:
    - Mengembangkan `radiusNasAccountingLog.service.js` dan antarmuka `AccountingLogSection.jsx` di halaman detail NAS (`/network/nas/:id`) untuk menampilkan log autentikasi/accounting secara real-time dan terperinci per perangkat router MikroTik.
  - **Panel Asal NAS & Riwayat PPPoE pada Dashboard RADIUS**:
    - Menghadirkan komponen `NasOriginPanel.jsx` untuk memantau dari router mana saja koneksi PPPoE dan Hotspot berasal.
    - Mengganti panel diskoneksi lama dengan `PppoeHistoryPanel.jsx` yang memberikan visualisasi lengkap kapan sesi terputus, durasi terhubung, serta penyebab diskoneksi (*terminate cause*).
  - **Alat Pemulihan Penggunaan Profil RADIUS (Profile Usage Recovery)**:
    - Membangun service `radiusProfileUsage.service.js` dan modal dialog `ProfileUsageModal.jsx` di menu Pengaturan Pengembang untuk mendeteksi profil paket broadband yang tidak sinkron antara master paket dengan akun langganan pelanggan, serta menyediakan audit fix secara atomik.
  - **Ekstensi Komunikasi gRPC RADIUS Server**:
    - Memperbarui skema Protobuf `radius.proto` pada backend dan server RADIUS Go guna mendukung streaming event accounting, pelaporan metadata server, dan verifikasi status node cluster.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #516  | Arsitektur Job Latar Cron Worker, Riwayat Eksekusi Permanen Dashboard, Pengayaan Relasi NAS Router, & Otomasi Callback iPaymu | Mencegah error 504 timeout proxy pada eksekusi batch cron worker (>60s) lewat arsitektur job latar Redis; menyajikan riwayat eksekusi cron yang akurat di dashboard; memperkaya tabel sesi & log RADIUS dengan identitas nama/IP router NAS; serta menstandarisasi callback URL payment gateway iPaymu. |
| #510  | Pengelolaan Penarikan Dana Payment Gateway, Saldo Awal, & Pemulihan Non-Payment | Menghadirkan otomatisasi pengambilan (*ingestion*) dan pembukuan (*recording*) mutasi penarikan dana (disbursement) gateway Xendit & iPaymu ke buku besar keuangan perusahaan; menyediakan penguncian saldo awal gateway (*opening balance*); dan pemulihan transaksi non-pembayaran. |
| #511  | Otomasi Freeze Faktur Terlambat, Pemulihan Sesi Terisolir, Format Cetak Faktur, & Build Info Radius | Menerapkan kebijakan isolasi otomatis pelanggan berfaktur jatuh tempo lewat BullMQ; menyediakan alat audit pemulihan sesi terisolir di tab Developer Recovery Tools; mempercantik layout cetak faktur standar; dan menyinkronkan metadata build info pada daemon RADIUS Go. |
| #509  | Monitoring NAS Accounting Log, Panel Riwayat PPPoE, Origin NAS, & Pemulihan Profil Radius | Meningkatkan visibilitas monitoring jaringan dengan panel Asal Router NAS & Riwayat PPPoE di Dashboard RADIUS; tab Log Accounting di detail router NAS; serta alat pemulihan ketidaksinkronan profil paket broadband di tab Developer. |

---

### Kemampuan Baru Pengguna/Admin

- **Visibilitas Router NAS pada Sesi & Log Koneksi**: Administrator jaringan kini dapat langsung melihat nama router NAS asal koneksi pada tabel Sesi Aktif, Riwayat Sesi, dan Log Autentikasi Broadband, serta dapat menyaring data langsung berdasarkan nama router (bukan hanya IP mentah).
- **Pencatatan Penarikan Dana Payment Gateway**: Bagian keuangan kini memiliki tab khusus "Penarikan Dana" pada menu Keuangan Payment Gateway untuk melihat riwayat mutasi pencairan dana dari Xendit/iPaymu dan langsung membukukannya ke rekening bank/kas perusahaan dengan satu klik melalui formulir laci (*drawer*).
- **Penguncian Saldo Awal Gateway**: Tim akunting dapat menetapkan dan mengunci saldo awal payment gateway per tanggal tertentu untuk memastikan rekonsiliasi kas tidak mengalami pergeseran (*drift*).
- **Alat Pemulihan Sesi Terisolir & Profil RADIUS**: Tim pengembang (Developer) kini dapat menjalankan pemulihan mandiri (*dry-run* dan eksekusi perbaikan) untuk menyelaraskan status isolir tagihan dengan status RADIUS, serta memperbaiki relasi paket broadband yang usang.
- **Pemantauan Log Accounting Per Router**: Administrator dapat membuka halaman detail perangkat NAS router dan melihat riwayat log accounting khusus untuk router tersebut secara terisolasi.

---

### Bug Fix / Solusi Masalah

1. **Error 504 Gateway Timeout pada Cron Worker**: Berhasil diatasi dengan mengubah alur pemanggilan cron berat menjadi asinkron berbasis polling Redis (`respondWithCronJob` & `postLongRunning`), sehingga proxy tidak lagi memutus koneksi di detik ke-60 saat proses berjalan hingga puluhan menit.
2. **Status Eksekusi Palsu pada Dashboard Cron**: Diperbaiki dengan mencatat status eksekusi final ke Redis hash permanen `cron-worker:last-runs`, mencegah status job tampil "-" atau "Gagal" palsu saat antrean BullMQ dipangkas.
3. **Peta Grey Tiles pada Pemilihan Lokasi**: Berhasil diatasi dengan utilitas `scheduleMapResize.js` yang memaksa peta Leaflet melakukan kalkulasi ulang dimensi (*invalidateSize*) sesaat setelah modal dibuka.
4. **Sesi Zombie / Stale Sesi Online pada Pelanggan Terisolir**: Diselesaikan pada `invoiceFreeze.service.js` dan domain accounting RADIUS server yang memastikan sesi lama dibersihkan secara tuntas saat pelanggan mengalami isolasi.
5. **Kesalahan Tampilan Kolom No. Faktur & Filter Datatable**: Diperbaiki dengan penerjemahan query kustom di service backend dan pelarian regex aman (`escapeRegex`).

---

### Menu/Fitur Baru

- **Tab "Penarikan Dana" (Withdrawals)** di menu Keuangan > Payment Gateway (`/finance/gateway`).
- **Tab "Saldo Awal" (Opening Balance)** di menu Keuangan > Payment Gateway (`/finance/gateway`).
- **Tab "Log Accounting"** di halaman Detail Router NAS (`/network/nas/:id`).
- **Panel "Asal NAS" & "Riwayat PPPoE"** pada Dashboard RADIUS (`/dashboards/radius`).
- **Alat Pemulihan "Frozen Session Recovery"** di Pengaturan Pengembang → Recovery Tools.
- **Alat Pemulihan "Profile Usage Recovery"** di Pengaturan Pengembang → Recovery Tools.
- **Kolom "Router NAS" & Filter Multi-Kolom NAS** pada Sesi RADIUS dan Log Autentikasi Broadband.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Pencatatan Penarikan Dana Payment Gateway (Withdrawal Ingestion & Recording)
- **Penjelasan Fitur**: Sistem secara otomatis mengambil (*ingest*) riwayat mutasi penarikan dana dari penyedia gateway (seperti Xendit dan iPaymu). Tim keuangan dapat meninjau mutasi yang berstatus berhasil (*settled*) dan membukukannya ke akun kas/bank tujuan perusahaan. Sistem akan mencatat jurnal debet pada akun kas bank penerima dan kredit pada akun payment gateway, dipotong biaya penarikan (*disbursement fee*) secara akurat.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Keuangan** → **Payment Gateway** (`/finance/gateway`).
  2. Beralih ke tab **Penarikan Dana**.
  3. Klik tombol **Ambil Data Terbaru** untuk menyinkronkan mutasi penarikan dari API Xendit atau iPaymu.
  4. Pada baris mutasi yang belum dibukukan, klik ikon **Catat ke Pembukuan**.
  5. Pada laci formulir yang muncul:
     - Pilih **Akun Kas/Bank Tujuan** penerima dana pencairan.
     - Periksa tanggal mutasi dan rincian biaya transfer/penarikan.
     - Tambahkan catatan jika diperlukan, lalu klik **Simpan Pencatatan**.
  6. Status mutasi akan berubah menjadi **Tercatat** (*Recorded*) dan jurnal keuangan akan otomatis terbentuk.

---

### 2. Monitoring Asal Router NAS & Log Accounting Sesi RADIUS
- **Penjelasan Fitur**: Dashboard RADIUS dan detail router NAS kini dilengkapi pengayaan data router MikroTik. Sesi koneksi pelanggan broadband tidak lagi hanya menampilkan IP NAS mentah, melainkan chip nama router lengkap berserta tautan detail router.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Dashboard** → **Dashboard RADIUS** (`/dashboards/radius`).
  2. Perhatikan panel baru **Asal NAS** di bagian atas untuk melihat distribusi jumlah sesi per router aktif.
  3. Buka tab **Sesi Berjalan**: kolom **Router NAS** kini menampilkan nama router (misal `Router-Utama-Core`).
  4. Untuk menyaring koneksi hanya dari router tertentu, ketikkan nama router pada kolom filter di atas tabel.
  5. Untuk melihat log accounting spesifik per router, kunjungi menu **Jaringan** → **Radius NAS**, klik salah satu router, lalu buka tab **Log Accounting**.

