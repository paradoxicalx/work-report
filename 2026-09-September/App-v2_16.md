# 📝 Daily Work Report - Dedy S.N Putra (16 September 2026)

---

## 📅 Laporan Harian - 16 September 2026

---

## 🌿 Branch: `master` — Issue #297: Autentikasi Radius Non-Customer

### 📌 Informasi Issue

- **Nomor Issue**: #297
- **Judul Issue**: Implementasi Modul Autentikasi Radius Non-Customer
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via squash merge pada commit `610b661`)

### 📅 Rincian Commit

#### [610b661] - resolve #297 - Rabu, 16 September 2026, 18:20:51 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/radiusAuthenticationNonCustomer.controller.js` [NEW]
  - `backend/src/controllers/radiusSession.controller.js`
  - `backend/src/routes/radiusAuthenticationNonCustomer.route.js` [NEW]
  - `backend/src/routes/radiusSession.route.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/src/services/radiusIdentity.service.js`
  - `backend/src/services/radiusProfile.service.js`
  - `backend/src/models/radiusAuthentication.model.js`
  - `backend/src/models/radiusProfile.js`
  - `backend/src/config/privilege.json`
  - `backend/src/app.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/test/helpers/factories.js`
  - `backend/test/integration/radiusAuthenticationNonCustomer.controller.test.js` [NEW]
  - `backend/test/integration/radiusAuthenticationNonCustomer.model.test.js` [NEW]
  - `backend/test/integration/radiusAuthenticationNonCustomer.service.test.js` [NEW]
  - `backend/test/integration/radiusIdentity.service.test.js` [NEW]
  - `backend/test/integration/radiusProfile.service.test.js` [NEW]
  - `frontend/src/app/navigation/networks.js`
  - `frontend/src/app/pages/network/radiusNonCustomer/create.jsx` [NEW]
  - `frontend/src/app/pages/network/radiusNonCustomer/detail.jsx` [NEW]
  - `frontend/src/app/pages/network/radiusNonCustomer/edit.jsx` [NEW]
  - `frontend/src/app/pages/network/radiusNonCustomer/index.jsx` [NEW]
  - `frontend/src/app/pages/network/radiusNonCustomer/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/network/radiusNonCustomer/schema/createSchema.js` [NEW]
  - `frontend/src/app/router/network/radiusNonCustomerRoute.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `documentations/2026-09-15-radius-internal-auth-design.md` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Implementasi penuh modul autentikasi radius untuk koneksi non-pelanggan (hotspot guest, uji coba, dll.) dengan fungsionalitas CRUD lengkap (create, read, update, delete).
  - Membuat controller, service, model, dan route baru untuk `radiusAuthenticationNonCustomer` dengan endpoint REST API yang didokumentasikan Swagger.
  - Menambahkan halaman frontend lengkap: halaman daftar (`index.jsx`), formulir buat (`create.jsx`), detail (`detail.jsx`), dan edit (`edit.jsx`) dengan validasi form via React Hook Form + Yup.
  - Membuat schema kolom datatable (`columns.jsx`) dan schema validasi (`createSchema.js`) untuk modul non-customer.
  - Menambahkan privilege akses `radiusNonCustomer.create`, `radiusNonCustomer.read`, `radiusNonCustomer.update`, `radiusNonCustomer.delete`, `radiusNonCustomer.list` ke sistem hak akses dan navigasi sidebar.
  - Menulis unit test dan integration test menyeluruh untuk controller, service, dan model.
  - Mendokumentasikan desain arsitektur autentikasi radius internal dalam dokumen desain khusus.
  - Menambahkan translation key i18n (EN & ID) untuk seluruh modul.

---

## 🌿 Branch: `master` — Issue #311: Peningkatan Radius Server (Go)

### 📌 Informasi Issue

- **Nomor Issue**: #311
- **Judul Issue**: Peningkatan Radius Server — Caching Profile, Rate Limiting Auth, dan Perluasan Konfigurasi
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via squash merge pada commit `cc9e096`)

### 📅 Rincian Commit

#### [cc9e096] - resolve #311 - Rabu, 16 September 2026, 20:55:22 WIB

- **Komponen yang Berubah**:
  - `radius-server/internal/repository/cache/cached_profile_repo.go` [NEW]
  - `radius-server/internal/repository/cache/cached_profile_repo_test.go` [NEW]
  - `radius-server/internal/transport/shared/auth_limiter.go` [NEW]
  - `radius-server/internal/transport/shared/auth_limiter_test.go` [NEW]
  - `radius-server/internal/transport/shared/bound_socket.go` [NEW]
  - `radius-server/internal/transport/shared/auth_limiter_test.go` [NEW]
  - `radius-server/internal/transport/shared/bound_socket.go` [NEW]
  - `radius-server/internal/domain/ports/repositories.go`
  - `radius-server/internal/domain/auth/authenticate.go`
  - `radius-server/internal/repository/mongo/authentication_repo.go`
  - `radius-server/internal/repository/mongo/connection.go`
  - `radius-server/internal/repository/mongo/metrics.go`
  - `radius-server/internal/repository/mongo/nas_repo.go`
  - `radius-server/internal/repository/mongo/profile_repo.go`
  - `radius-server/internal/transport/pppoe/auth_listener.go`
  - `radius-server/internal/transport/shared/side_effects.go`
  - `radius-server/pkg/config/env.go`
  - `radius-server/pkg/config/config_test.go`
  - `radius-server/pkg/metrics/metrics.go`
  - `radius-server/cmd/radiusd/wire.go`
  - `radius-server/test/integration/mongo_repo_test.go`
  - `radius-server/test/integration/usage_aggregate_test.go`
  - `radius-server/test/integration/voucher_total_test.go`
  - `radius-server/test/integration/wal_aggregate_test.go`
  - `radius-server/test/testutil/fake_repos.go`
  - `radius-server/DOCUMENTATION.md`
  - `.env.production.example`
  - `radius-server/.env.example`
- **Deskripsi Perubahan & Fungsi**:
  - Menerapkan lapisan caching pada repository profile radius (`cached_profile_repo.go`) untuk mengurangi frekuensi akses database MongoDB saat autentikasi — meningkatkan performa response time secara signifikan.
  - Membuat modul rate limiting otoritatif (`auth_limiter.go`) untuk mengontrol jumlah percobaan autentikasi per IP/source, mencegah brute-force attack dan abuse.
  - Menambahkan bound socket helper (`bound_socket.go`) untuk manajemen koneksi socket yang lebih terkontrol.
  - Memperluas repository port interface dengan method tambahan untuk mendukung caching dan metrics collection.
  - Menambahkan konfigurasi environment variable baru (rate limit threshold, cache TTL, dsb.) ke `config/env.go`.
  - Menambahkan metrics collection pada repository layer untuk observabilitas performa.
  - Memperbarui integration test dan menambah unit test untuk modul caching dan rate limiting.
  - Mendokumentasikan konfigurasi dan arsitektur baru di `DOCUMENTATION.md`.

---

## 🌿 Branch: `master` — Issue #312: Fix Attendance Device Client (Timezone & Dockerfile)

### 📌 Informasi Issue

- **Nomor Issue**: #312
- **Judul Issue**: Perbaikan Attendance Device Client — Penanganan Timezone & Infrastructure Docker
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via squash merge pada commit `8ef3eb6`)

### 📅 Rincian Commit

#### [8ef3eb6] - resolve #312 - Rabu, 16 September 2026, 16:54:57 WIB

- **Komponen yang Berubah**:
  - `backend/Dockerfile`
  - `backend/src/lib/attendanceDeviceClient.js`
  - `backend/test/unit/attendanceDeviceClient.test.js` [NEW]
  - `cron-worker/Dockerfile`
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan `ENV TZ=Asia/Jakarta` ke Dockerfile backend dan cron-worker untuk memastikan seluruh proses berjalan dengan timezone Indonesia (WIB), mengatasi inkonsistensi waktu pada perangkat absensi.
  - Memperbarui client perangkat absensi (`attendanceDeviceClient.js`) untuk menangani konversi timezone yang benar pada data clock-in/clock-out dari mesin ZKTeco.
  - Menulis unit test untuk memvalidasi logika konversi timezone pada attendance device client.

---

## 🌿 Branch: `origin/production` — Issue #312: Fix Attendance Device Client (Timezone)

### 📌 Informasi Issue

- **Nomor Issue**: #312
- **Judul Issue**: Perbaikan Attendance Device Client — Penanganan Timezone
- **Status Branch**: `Belum di-merge` (Branch `origin/production` belum di-merge ke `master`)

### 📅 Rincian Commit

#### [edeb9de] - resolve #312 - Rabu, 16 September 2026, 23:21:42 WIB

- **Komponen yang Berubah**:
  - `backend/src/lib/attendanceDeviceClient.js`
  - `backend/test/unit/attendanceDeviceClient.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Revisi lebih lanjut pada modul `attendanceDeviceClient.js` untuk perbaikan处理 penanganan data timestamp dari mesin absensi — menyesuaikan parsing timezone agar sinkron dengan format waktu lokal server (WIB).
  - Memperbarui unit test untuk menutupi skenario edge-case pada konversi waktu absensi.

---

## 🌿 Branch: `master` — Issue #313: Setup Admin & Refactor Hak Akses

### 📌 Informasi Issue

- **Nomor Issue**: #313
- **Judul Issue**: Fitur Setup Admin, Refactor Modul Privilege, dan Standardisasi Integration Test
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via squash merge pada commit `c1a283e`)

### 📅 Rincian Commit

#### [c1a283e] - resolve #313 - Rabu, 16 September 2026, 15:50:26 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/setup.controller.js` [NEW]
  - `backend/src/routes/setup.route.js` [NEW]
  - `backend/src/services/admin.service.js`
  - `backend/src/app.js`
  - `backend/src/middlewares/auth.middleware.js`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/auth.controller.js`
  - `backend/src/controllers/direkturDocument.controller.js`
  - `backend/src/controllers/privilege.controller.js`
  - `backend/src/controllers/waChat.controller.js`
  - `backend/src/services/baileysControl.service.js`
  - `backend/src/services/baileysAccount.service.js`
  - `backend/src/services/privilege.service.js`
  - `backend/src/services/privilegeAi.service.js`
  - `backend/src/services/privilegeDictionary.service.js`
  - `backend/src/services/v1PrivilegeMapper.js`
  - `backend/src/services/waChannelRouter.service.js`
  - `backend/src/services/waChatSweep.service.js`
  - `backend/src/services/networkLatency.service.js`
  - `backend/src/services/financeExpense.service.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/dashboard.service.js`
  - `backend/src/services/attendance.service.js`
  - `backend/src/lib/attendanceDeviceClient.js`
  - `backend/src/utils/data-table.js`
  - `backend/src/utils/minio.js`
  - `backend/src/utils/crypto.util.js`
  - `backend/src/sockets/baileysAccount.controller.js`
  - `backend/src/sockets/waChat.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/test/helpers/factories.js`
  - `backend/test/integration/setupInitialAdmin.test.js` [NEW]
  - `backend/test/unit/cryptoUtil.test.js`
  - `backend/test/unit/loggerSanitizer.test.js`
  - `backend/test/unit/mongoTools.util.test.js`
  - `backend/test/unit/telegramAlert.test.js`
  - `backend/test/unit/ticketTelegramClose.test.js`
  - `backend/test/unit/waUnrepliedCount.service.test.js`
  - `backend/test/unit/financeInvoiceTotal.test.js`
  - `backend/test/unit/payrollCalculation.test.js`
  - `backend/test/integration/dbTools.service.test.js`
  - `backend/test/integration/financeAutoInvoice.prorata.test.js`
  - `backend/test/integration/financeBudgeting.approve.test.js`
  - `backend/test/integration/financeBudgeting.create.test.js`
  - `backend/test/integration/financeBudgeting.rejectRevise.test.js`
  - `backend/test/integration/financeCoa.pathCascade.test.js`
  - `backend/test/integration/financeExpense.bulkPay.test.js`
  - `backend/test/integration/financeExpense.create.test.js`
  - `backend/test/integration/financeExpense.payment.test.js`
  - `backend/test/integration/financeExpense.paymentVoid.test.js`
  - `backend/test/integration/financeExpenseAP.journal.test.js`
  - `backend/test/integration/financeExpenseAttachments.test.js`
  - `backend/test/integration/financeExpenseFromVendorPO.test.js`
  - `backend/test/integration/financeExpenseOrder.listDetail.test.js`
  - `backend/test/integration/financeExpensePayableAging.test.js`
  - `backend/test/integration/financeGateway.refundReversal.test.js`
  - `backend/test/integration/financeInvoice.reactivate.test.js`
  - `backend/test/integration/financeInvoiceClassifier.test.js`
  - `backend/test/integration/financeInvoiceReceivableAging.test.js`
  - `backend/test/integration/financeLedger.postEntries.test.js`
  - `backend/test/integration/financePaymentSummary.test.js`
  - `backend/test/integration/financeRegulatoryObligation.test.js`
  - `backend/test/integration/financeReport.cashFlow.test.js`
  - `backend/test/integration/financeTransactionDraft.recurring.test.js`
  - `backend/test/integration/mobileServiceChange.service.test.js`
  - `backend/test/integration/networkDevice.service.test.js`
  - `backend/test/integration/partnerApiAccount.test.js`
  - `backend/test/integration/partnerApiBusiness.test.js`
  - `backend/test/integration/partnerApiCustomer.changeStatus.test.js`
  - `backend/test/integration/partnerApiCustomer.create.test.js`
  - `backend/test/integration/partnerApiCustomer.update.test.js`
  - `backend/test/integration/partnerApiInvoice.test.js`
  - `backend/test/integration/partnerApiPartner.documents.test.js`
  - `backend/test/integration/partnerApiPartner.uploadDocuments.test.js`
  - `backend/test/integration/partnerApiProductBroadband.test.js`
  - `backend/test/integration/partnerApiRadius.test.js`
  - `backend/test/integration/partnerApiRadiusProfile.test.js`
  - `backend/test/integration/partnerAppLogout.test.js`
  - `backend/test/integration/payrollRun.notification.test.js`
  - `backend/test/integration/payrollRun.tableFilter.test.js`
  - `backend/test/integration/payrollRun.workflow.test.js`
  - `backend/test/integration/payrollSlip.paymentReport.test.js`
  - `frontend/src/app/pages/Auth/SetupAdmin.jsx` [NEW]
  - `frontend/src/app/pages/Auth/index.jsx`
  - `frontend/src/app/navigation/mobileApp.js`
  - `frontend/src/app/pages/archive/decree/direktur/DirekturGeneratedDocumentPreview.jsx`
  - `frontend/src/app/pages/archive/decree/direktur/ReviewDrawer.jsx`
  - `frontend/src/app/pages/archive/decree/direktur/SignPositionStep.jsx`
  - `frontend/src/app/pages/archive/decree/direktur/create.jsx`
  - `frontend/src/app/pages/archive/decree/direktur/edit.jsx`
  - `frontend/src/app/pages/archive/decree/direktur/schema/columns.jsx`
  - `frontend/src/app/pages/customerService/baileysAccount/edit.jsx`
  - `frontend/src/app/pages/dashboards/connectivity/components/ConnectivityQuickActions.jsx`
  - `frontend/src/app/pages/dashboards/connectivity/components/IPv4PoolCard.jsx`
  - `frontend/src/app/pages/dashboards/connectivity/components/NetworkAlertSummaryCard.jsx`
  - `frontend/src/app/pages/dashboards/connectivity/components/OltHealthCard.jsx`
  - `frontend/src/app/pages/dashboards/connectivity/components/TrafficThroughputCard.jsx`
  - `frontend/src/app/pages/dashboards/finance/components/FinanceQuickActions.jsx`
  - `frontend/src/app/pages/dashboards/home/components/HomeQuickActions.jsx`
  - `frontend/src/app/pages/dashboards/home/components/SystemHealthCard.jsx`
  - `frontend/src/app/pages/dashboards/hotspot/components/HotspotKpiStrip.jsx`
  - `frontend/src/app/pages/dashboards/hotspot/components/HotspotQuickActions.jsx`
  - `frontend/src/app/pages/dashboards/hotspot/components/HotspotSalesTrendCard.jsx`
  - `frontend/src/app/pages/dashboards/hotspot/components/HotspotDashboardSkeleton.jsx`
  - `frontend/src/app/pages/dashboards/problem/components/CategoryChart.jsx`
  - `frontend/src/app/pages/dashboards/problem/components/IncidentSeverityCard.jsx`
  - `frontend/src/app/pages/dashboards/problem/components/PipelineCard.jsx`
  - `frontend/src/app/pages/dashboards/problem/components/ProblemKpiStrip.jsx`
  - `frontend/src/app/pages/dashboards/problem/components/ProblemQuickActions.jsx`
  - `frontend/src/app/pages/dashboards/problem/components/ResolutionTimeCard.jsx`
  - `frontend/src/app/pages/dashboards/problem/components/TechnicianWorkloadTable.jsx`
  - `frontend/src/app/pages/dashboards/problem/components/WorkOrderCard.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/EventsChart.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/RadiusDashboardSkeleton.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/RadiusQuickActions.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/ServerList.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/StatsRow.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx`
  - `frontend/src/app/pages/dashboards/sales/components/SalesDashboardSkeleton.jsx`
  - `frontend/src/app/pages/dashboards/sales/components/SalesQuickActions.jsx`
  - `frontend/src/app/pages/dashboards/sales/hooks/useSalesStats.js`
  - `frontend/src/app/pages/dashboards/warehouse/components/WarehouseDashboardSkeleton.jsx`
  - `frontend/src/app/pages/dashboards/warehouse/components/WarehouseKpiStrip.jsx`
  - `frontend/src/app/pages/dashboards/warehouse/components/WarehouseQuickActions.jsx`
  - `frontend/src/app/pages/dashboards/whatsapp/components/WhatsappDashboardSkeleton.jsx`
  - `frontend/src/app/pages/dashboards/whatsapp/components/WhatsappQuickActions.jsx`
  - `frontend/src/app/pages/mobileApp/news/create.jsx`
  - `frontend/src/app/pages/mobileApp/news/edit.jsx`
  - `frontend/src/app/pages/mobileApp/news/index.jsx`
  - `frontend/src/app/pages/mobileApp/news/schema/GridCard.jsx`
  - `frontend/src/app/pages/mobileApp/news/schema/columns.jsx`
  - `frontend/src/app/pages/mobileApp/serviceChange/index.jsx`
  - `frontend/src/app/pages/network/latency/components/LatencyGraphCard.jsx`
  - `frontend/src/app/pages/network/latency/components/LatencyGraphStyleFields.jsx`
  - `frontend/src/app/pages/network/latency/components/LatencyTargetModal.jsx`
  - `frontend/src/app/pages/network/latency/index.jsx`
  - `frontend/src/app/pages/network/latency/utils/latencyGraphStyle.js`
  - `frontend/src/app/pages/services/activation/index.jsx`
  - `frontend/src/app/pages/services/activation/schema/poColumns.jsx`
  - `frontend/src/app/pages/services/activation/schema/soColumns.jsx`
  - `frontend/src/app/pages/developer/dbTools/BackupScheduleForm.jsx`
  - `frontend/src/app/pages/developer/recoveryTools/RecoveryDryRunModal.jsx`
  - `frontend/src/app/pages/developer/recoveryTools/RecoveryRouteRow.jsx`
  - `frontend/src/app/pages/developer/recoveryTools/RecoveryToolsTab.jsx`
  - `frontend/src/app/pages/users/document/index.jsx`
  - `frontend/src/app/pages/users/privilege/components/NotificationPrivilegeSection.jsx`
  - `frontend/src/app/pages/users/privilege/components/PrivilegeAiDrawer.jsx`
  - `frontend/src/app/pages/users/privilege/components/PrivilegeDetailCategories.jsx`
  - `frontend/src/app/pages/users/privilege/components/PrivilegeDetailStats.jsx`
  - `frontend/src/app/pages/users/privilege/components/PrivilegeDetailUsers.jsx`
  - `frontend/src/app/pages/users/privilege/components/PrivilegeDiffModal.jsx`
  - `frontend/src/app/pages/users/privilege/components/PrivilegeMatrixTable.jsx`
  - `frontend/src/app/pages/users/privilege/create.jsx`
  - `frontend/src/app/pages/users/privilege/detail.jsx`
  - `frontend/src/app/pages/users/privilege/edit.jsx`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/DocumentPreviewModal.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `network-monitor/src/controllers/fakeTraffic.controller.js`
  - `network-monitor/src/controllers/latency.controller.js`
  - `network-monitor/src/services/dnsProbe.service.js`
  - `network-monitor/src/services/fakeTraffic.service.js`
  - `network-monitor/src/services/httpProbe.service.js`
  - `network-monitor/src/services/latencyPoller.service.js`
  - `network-monitor/src/services/latencyRrd.service.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Fitur Setup Admin**: Membuat endpoint `/setup/initial-admin` dan halaman frontend `SetupAdmin.jsx` untuk inisialisasi admin pertama kali saat sistem fresh deploy — memungkinkan setup admin tanpa akses database manual.
  - **Refactor Privilege System**: Melakukan refactoring besar-besaran pada modul privilege di seluruh stack (backend service, frontend komponen). Memperbarui v1PrivilegeMapper untuk kompatibilitas, memperbarui PrivilegeAiDrawer, PrivilegeMatrixTable, dan PrivilegeDiffModal untuk tampilan yang lebih baik.
  - **Cleanup Dashboard Components**: Melakukan standarisasi dan pembersihan kode pada seluruh komponen dashboard (connectivity, finance, home, hotspot, problem, radius, sales, warehouse, whatsapp) — menghapus unused imports, menyeragamkan struktur komponen, dan memperbaiki konsistensi styling.
  - **Test Infrastructure Overhaul**: Melakukan standarisasi seluruh integration test dan unit test (80+ berkas) — menambahkan proper setup/teardown, memperbaiki factory pattern, menambah `beforeAll`/`afterAll` lifecycle hooks, dan memastikan tes berjalan stabil di CI/CD.
  - **Network Monitor Fixes**: Memperbaiki latency polling, DNS probe, fake traffic, dan HTTP probe services dengan logging yang lebih baik dan error handling yang lebih robust.
  - Menambahkan translation key i18n baru di kedua sisi (EN & ID) untuk modul setup admin dan privilege.

---

## 🌿 Branch: `master` — Issue #313: Fix Infra — Timezone Docker & Attendance Client

### 📌 Informasi Issue

- **Nomor Issue**: #313
- **Judul Issue**: Infra: Penanganan Timezone di Dockerfile & Attendance Device Client
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via squash merge pada commit `a3bfc70`)

### 📅 Rincian Commit

#### [a3bfc70] - fix(infra): set ENV TZ=Asia/Jakarta di backend & cron-worker Dockerfile - Rabu, 16 September 2026, 16:42:23 WIB

- **Komponen yang Berubah**:
  - `backend/Dockerfile`
  - `cron-worker/Dockerfile`
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan environment variable `TZ=Asia/Jakarta` ke Dockerfile backend dan cron-worker untuk memastikan semua proses Node.js berjalan dengan timezone WIB secara konsisten, mengatasi perbedaan waktu antara server Docker dan host.

---

## 🌿 Branch: `master` — Issue #305: CI/CD — Hapus GitHub Actions Deploy Workflow

### 📌 Informasi Issue

- **Nomor Issue**: #305
- **Judul Issue**: CI/CD: Migrasi dari GitHub Actions Deploy Workflow ke GitHub App
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via squash merge pada commit `73d370b`)

### 📅 Rincian Commit

#### [73d370b] - resolve #305 - Rabu, 16 September 2026, 01:34:47 WIB

- **Komponen yang Berubah**:
  - `.github/workflows/deploy-production.yml` [DELETE]
- **Deskripsi Perubahan & Fungsi**:
  - Menghapus workflow GitHub Actions deploy production (`deploy-production.yml`) karena pengelolaan deploy production dialihkan ke GitHub App native yang terintegrasi langsung ke server deploy.

---

## 🌿 Branch: `origin/remove-deploy-production-workflow` — CI/CD Workflow Cleanup

### 📌 Informasi Issue

- **Nomor Issue**: — (Branch maintenance/CI cleanup)
- **Judul Issue**: Hapus GitHub Actions Deploy Workflow, Gunakan GitHub App
- **Status Branch**: `Belum di-merge` (Branch `origin/remove-deploy-production-workflow` belum di-merge ke `master`)

### 📅 Rincian Commit

#### [8a2bfba] - ci: remove GitHub Actions deploy workflow, use native GitHub App instead - Rabu, 16 September 2026, 00:47:27 WIB

- **Komponen yang Berubah**:
  - `.github/workflows/deploy-production.yml` [DELETE]
- **Deskripsi Perubahan & Fungsi**:
  - Menghapus seluruh isi workflow deploy production karena pengelolaan deploy production telah dialihkan ke GitHub App native, mengurangi kompleksitas CI/CD pipeline dan menghilangkan dependensi terhadap GitHub Actions runner untuk deployment.

---

## 🌿 Branch: `master` — Changelog Updates

### 📌 Informasi Issue

- **Nomor Issue**: — (Maintenance)
- **Judul Issue**: Pembaruan Changelog untuk Rilis Issue #297, #311, #312, #313

### 📅 Rincian Commit

#### [686fa0b] - update changelog - Rabu, 16 September 2026, 21:21:16 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-297.json` [NEW]
  - `backend/src/data/changelog/releases/issue-311.json` [NEW]
  - `backend/src/data/changelog/releases/issue-312.json` [NEW]
  - `backend/src/data/changelog/releases/issue-313.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan entri changelog untuk 4 issue yang baru diselesaikan: #297 (autentikasi radius non-customer), #311 (peningkatan radius server), #312 (perbaikan attendance device), dan #313 (setup admin & refactor privilege) ke dalam sistem changelog terpusat.

#### [4fd9eb0] - update changelog - Rabu, 16 September 2026, 01:57:50 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-289.json` [NEW]
  - `backend/src/data/changelog/releases/issue-293.json` [NEW]
  - `backend/src/data/changelog/releases/issue-294.json` [NEW]
  - `backend/src/data/changelog/releases/issue-298.json` [NEW]
  - `backend/src/data/changelog/releases/issue-300.json` [NEW]
  - `backend/src/data/changelog/releases/issue-305.json` [NEW]
  - `.github/workflows/deploy-production.yml` [DELETE]
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan entri changelog untuk 6 issue rilis sebelumnya (#289, #293, #294, #298, #300, #305) ke sistem changelog terpusat.
  - Menghapus workflow GitHub Actions deploy production.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                            | Dampak Utama                                                      |
| ----- | -------------------------------- | ----------------------------------------------------------------- |
| #297  | Autentikasi Radius Non-Customer  | Modul CRUD lengkap untuk koneksi hotspot/tamu non-pelanggan       |
| #311  | Peningkatan Radius Server        | Caching profile, rate limiting autentikasi, metrics observability |
| #312  | Fix Attendance Device Client     | Perbaikan timezone WIB untuk data absensi                         |
| #313  | Setup Admin & Refactor Privilege | Fitur setup admin + pembersihan kode komprehensif                 |
| #305  | CI/CD: Hapus Deploy Workflow     | Migrasi deploy production ke GitHub App native                    |

### Kemampuan Baru Pengguna/Admin

- Admin dapat membuat dan mengelola autentikasi radius untuk koneksi non-pelanggan (hotspot guest, akun uji coba) melalui halaman administrasi web.
- Sistem autentikasi radius kini lebih cepat berkat caching profile di lapisan repository, mengurangi beban database MongoDB.
- Percobaan autentikasi brute-force dapat dideteksi dan dibatasi oleh rate limiter otoritatif yang baru.
- Pengguna dapat melakukan setup admin pertama kali melalui halaman web tanpa perlu akses database langsung.
- Data absensi dari mesin ZKTeco kini ditampilkan dengan timezone yang benar (WIB).

### Bug Fix / Solusi Masalah

- Perbaikan inkonsistensi timezone pada Docker container backend dan cron-worker yang menyebabkan waktu absensi salah.
- Perbaikan pipeline CI/CD dengan menghapus workflow deploy production yang sudah tidak relevan.

### Menu/Fitur Baru

- Menu **Radius Non-Customer** di sidebar navigasi (Network) — halaman daftar, buat, detail, dan edit untuk autentikasi radius non-pelanggan.
- Halaman **Setup Admin** di `/auth/setup` — formulir inisialisasi admin pertama kali.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### Modul Autentikasi Radius Non-Customer

- **Penjelasan Fitur**: Modul ini memungkinkan admin membuat, melihat, mengubah, dan menghapus entri autentikasi radius untuk koneksi non-pelanggan (misal: hotspot tamu, akun uji coba). Setiap entri berisi data identitas (nama, MAC address, username, password) dan parameter koneksi (profil, masa aktif, quota). Modul ini terintegrasi dengan radius server Go yang menangani autentikasi PPPoE/Hotspot secara real-time.

- **Langkah Penggunaan**:
  1. Buka menu **Network → Radius Non-Customer** di sidebar navigasi.
  2. Klik tombol **Tambah Baru** untuk membuat entri autentikasi baru.
  3. Isi formulir: nama, username, password, MAC address, pilih profil radius, dan tentukan masa aktif.
  4. Klik **Simpan** — entri akan langsung aktif dan dapat digunakan untuk autentikasi koneksi.
  5. Untuk mengubah data, klik baris pada tabel → pilih **Edit** → ubah field yang diperlukan → **Simpan**.
  6. Untuk menghapus, klik baris pada tabel → pilih **Hapus** → konfirmasi pada modal.

### Fitur Setup Admin

- **Penjelasan Fitur**: Halaman setup admin memungkinkan inisialisasi akun admin pertama kali saat sistem baru di-deploy. Fitur ini hanya aktif jika belum ada admin yang terdaftar di database.

- **Langkah Penggunaan**:
  1. Buka halaman login di `/auth`.
  2. Jika belum ada admin, sistem akan menampilkan link **"Setup Admin Pertama Kali"**.
  3. Isi nama lengkap, username, email, dan password admin pertama.
  4. Klik **Setup** — admin akan dibuat dan dapat langsung login.
