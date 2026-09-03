# 📝 Daily Work Report - Dedy (2026-09-03)

---

## 📅 Laporan Harian - 3 September 2026

---

## 🌿 Branch: `issue-213` — Integrasi Multi-Akun WhatsApp Baileys (Microservice `baileys-api`, Manajemen Akun WhatsApp Multi-Device, QR Pairing, Shared Encrypted Mongo Auth State, dan Routing Percakapan Customer Service)

### 📌 Informasi Issue

- **Nomor Issue**: #213
- **Judul Issue**: Integrasi Multi-Akun WhatsApp Baileys (Microservice `baileys-api`, Manajemen Kredensial Multi-Device WhatsApp, QR Pairing & Anti-Ban Mechanism, Shared Encrypted Mongo Auth State, dan Routing Percakapan Customer Service Multi-Channel)
- **Status Branch**: `Belum di-merge` (Branch aktif dalam pengembangan di workspace utama)

---

### ⏳ Pekerjaan Belum Di-commit (Working Tree Changes)

- **Komponen yang Berubah**:
  - **Microservice Baru — `baileys-api` (Port 3050)**:
    - [`baileys-api/package.json`](baileys-api/package.json) [NEW] — Inisialisasi microservice baru berbasis Express v4, `@whiskeysockets/baileys` v6.7, Mongoose v8, Winston, dan Axios.
    - [`baileys-api/Dockerfile`](baileys-api/Dockerfile) [NEW] — Dockerfile containerisasi microservice `baileys-api` berbasis Node.js Alpine.
    - [`baileys-api/.env.example`](baileys-api/.env.example) [NEW] — Definisi konfigurasi environment (`PORT=3050`, `MONGO_URI`, `BACKEND_INTERNAL_URL`, `INTERNAL_API_KEY`, `SESSION_ENCRYPTION_KEY`).
    - [`baileys-api/src/server.js`](baileys-api/src/server.js) [NEW] — Entry point server Express dengan middleware logger request HTTP, integrasi Swagger UI, graceful shutdown, dan auto-reconnect semua akun aktif saat startup (`initializeAllConnectedAccounts`).
    - [`baileys-api/src/config.js`](baileys-api/src/config.js) [NEW] — Konfigurasi terpusat dan validasi env mandatory.
    - [`baileys-api/src/database.js`](baileys-api/src/database.js) [NEW] — Koneksi Mongoose ke database MongoDB bersama (shared database pattern).
    - [`baileys-api/src/models/baileysAccount.model.js`](baileys-api/src/models/baileysAccount.model.js) [NEW] — Definisi skema data akun Baileys di sisi microservice.
    - [`baileys-api/src/services/crypto.service.js`](baileys-api/src/services/crypto.service.js) [NEW] — Layanan enkripsi/dekripsi AES-256-GCM untuk mengamankan Signal Protocol keys dan auth state sebelum disimpan ke MongoDB.
    - [`baileys-api/src/services/authStateStore.js`](baileys-api/src/services/authStateStore.js) [NEW] — Custom MongoDB multi-device auth credentials store yang mengimplementasikan interface Baileys `AuthenticationState` secara atomic dan aman.
    - [`baileys-api/src/services/sessionManager.service.js`](baileys-api/src/services/sessionManager.service.js) [NEW] — Orkestrasi lifecycle multi-socket Baileys: pembuatan socket, penanganan event QR code (`connection.update`), reconnect otomatis dengan exponential backoff, update status akun, dan sinkronisasi nomor telepon (`creds.me.id`).
    - [`baileys-api/src/services/antiBan.service.js`](baileys-api/src/services/antiBan.service.js) [NEW] — Mekanisme anti-ban WhatsApp: simulasi perilaku manusiawi (*human presence*), pengiriman status mengetik (*composing*), jeda acak (*typing delay* dengan jitter interval), dan sanitasi pesan.
    - [`baileys-api/src/services/rateLimiter.service.js`](baileys-api/src/services/rateLimiter.service.js) [NEW] — Kontrol batas kecepatan pengiriman pesan per akun (per menit/jam/hari) guna melindungi nomor dari pemblokiran massal.
    - [`baileys-api/src/services/backendBridge.service.js`](baileys-api/src/services/backendBridge.service.js) [NEW] — Pengiriman pesan masuk (inbound messages), perubahan status pengiriman (read receipt/ack), dan status koneksi ke Backend API via webhook internal.
    - [`baileys-api/src/controllers/internal.controller.js`](baileys-api/src/controllers/internal.controller.js) & [`baileys-api/src/routes/internal.route.js`](baileys-api/src/routes/internal.route.js) [NEW] — Rute REST internal untuk kontrol sesi (`start`, `stop`, `restart`, `delete`, `qr-code`) yang diamankan dengan `authenticateInternal` (`x-api-key`).
    - [`baileys-api/src/controllers/send.controller.js`](baileys-api/src/controllers/send.controller.js) & [`baileys-api/src/routes/send.route.js`](baileys-api/src/routes/send.route.js) [NEW] — Rute pengiriman pesan terpadu (teks, media gambar, dokumen, video, audio) melalui instance Baileys aktif.
  - **Backend Core — Manajemen Akun & Routing Percakapan Customer Service**:
    - [`backend/src/models/baileysAccount.model.js`](backend/src/models/baileysAccount.model.js) [NEW] — Model Mongoose akun WhatsApp Baileys dengan tracking field: `label`, `phone_number`, `status` (`pending_qr`, `connecting`, `connected`, `disconnected`, `logged_out`, `error`), `status_message`, `connected_at`, `last_disconnected_at`, `proxy`, `rate_limit`, `disabled`, dan `auth_state` (select: false).
    - [`backend/src/controllers/baileysAccount.controller.js`](backend/src/controllers/baileysAccount.controller.js) [NEW] & [`backend/src/services/baileysAccount.service.js`](backend/src/services/baileysAccount.service.js) [NEW] — Handler CRUD akun Baileys, sinkronisasi status, serta fungsi toggle disable/enable.
    - [`backend/src/services/baileysControl.service.js`](backend/src/services/baileysControl.service.js) [NEW] — Komunikasi kontrol HTTP internal ke service `baileys-api` untuk memicu pairing QR, pemutusan sesi, dan pengecekan status runtime.
    - [`backend/src/controllers/baileysInternal.controller.js`](backend/src/controllers/baileysInternal.controller.js) [NEW] — Webhook internal penerima pesan masuk dari `baileys-api` yang mem-forward chat ke modul percakapan customer service (`waConversation.service.js`).
    - [`backend/src/routes/baileysAccount.route.js`](backend/src/routes/baileysAccount.route.js) [NEW] — Rute API `/api/v1/baileys-account` dengan validasi privilege `baileysAccount.read`, `create`, `update`, `delete`, dan `connect`.
    - [`backend/src/sockets/baileysAccount.controller.js`](backend/src/sockets/baileysAccount.controller.js) [NEW] & [`backend/src/sockets/socket-io.js`](backend/src/sockets/socket-io.js) — Event Socket.IO real-time ke admin panel (`baileysAccount:qr`, `baileysAccount:statusChanged`, `baileysAccount:connected`).
    - [`backend/src/models/waConversation.model.js`](backend/src/models/waConversation.model.js) — Pembaruan skema percakapan CS untuk mendukung multi-channel: penambahan field `channel` (`meta` | `baileys`, default `meta`), referensi `baileys_account` (ObjectId), serta compound index `{ wa_id: 1, channel: 1, baileys_account: 1, status: 1 }`.
    - [`backend/src/services/waConversation.service.js`](backend/src/services/waConversation.service.js) — Pemisahan sesi percakapan CS agar pelanggan yang sama dihubungi lewat akun Baileys berbeda tetap memiliki sesi terpisah dan tidak tercampur dengan channel Meta resmi.
    - [`backend/src/utils/waChatSerializer.js`](backend/src/utils/waChatSerializer.js) — Serialisasi metadata pengirim dan label akun Baileys pada payload percakapan chat.
    - [`backend/src/app.js`](backend/src/app.js) — Pendaftaran rute API `baileysAccountRoute`.
    - [`backend/src/config/privilege.json`](backend/src/config/privilege.json) — Pendaftaran privilege baru: `baileysAccount` (`read`, `create`, `update`, `delete`, `connect`).
    - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json) & [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json) — Penambahan translasi pesan validasi dan respon modul Baileys.
  - **Frontend — Antarmuka Manajemen Akun WhatsApp Baileys**:
    - [`frontend/src/app/pages/customerService/baileysAccount/index.jsx`](frontend/src/app/pages/customerService/baileysAccount/index.jsx) [NEW] — Halaman tabel manajemen akun Baileys dengan indikator status live, tombol scan QR pairing, tombol hubungkan/putuskan, serta filter pencarian.
    - [`frontend/src/app/pages/customerService/baileysAccount/create.jsx`](frontend/src/app/pages/customerService/baileysAccount/create.jsx) [NEW] — Drawer form penambahan akun Baileys baru dengan konfigurasi label, pengaturan proxy HTTP/SOCKS5, dan limit rate pengiriman.
    - [`frontend/src/app/pages/customerService/baileysAccount/edit.jsx`](frontend/src/app/pages/customerService/baileysAccount/edit.jsx) [NEW] — Drawer form edit data dan konfigurasi akun.
    - [`frontend/src/app/pages/customerService/baileysAccount/schema/columns.jsx`](frontend/src/app/pages/customerService/baileysAccount/schema/columns.jsx) [NEW] — Definisi kolom TanStack Table dengan status badge khusus akun Baileys.
    - [`frontend/src/app/pages/customerService/baileysAccount/schema/createSchema.js`](frontend/src/app/pages/customerService/baileysAccount/schema/createSchema.js) [NEW] & [`editSchema.js`](frontend/src/app/pages/customerService/baileysAccount/schema/editSchema.js) [NEW] — Validasi skema Yup form input akun Baileys.
    - [`frontend/src/app/router/customerService/baileysAccountRoute.jsx`](frontend/src/app/router/customerService/baileysAccountRoute.jsx) [NEW] & [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx) — Pendaftaran rute SPA `/customer-service/baileys-account`.
    - [`frontend/src/app/navigation/customerService.js`](frontend/src/app/navigation/customerService.js) — Penambahan item menu navigasi **Akun WhatsApp (Baileys)** pada kategori Customer Service.
    - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx) & [`frontend/src/components/shared/table/status.js`](frontend/src/components/shared/table/status.js) — Status badge cell helper untuk status akun Baileys (`pending_qr`, `connecting`, `connected`, `disconnected`, `logged_out`, `error`).
    - [`frontend/src/constants/privilegeDescriptions.en.json`](frontend/src/constants/privilegeDescriptions.en.json) & [`frontend/src/constants/privilegeDescriptions.id.json`](frontend/src/constants/privilegeDescriptions.id.json) — Deskripsi hak akses privilege Baileys bilingual.
    - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) — Kamus terjemahan teks antarmuka Akun Baileys.

---

## 🌿 Branch: `issue-265` — Real-Time Badge Counters Navigasi Sidebar & Komprehensif Code Audit Fixes

### 📌 Informasi Issue

- **Nomor Issue**: #265
- **Judul Issue**: Implementasi Real-Time Dynamic Badge Counters pada Navigasi Sidebar (Tiket Open, Chat WhatsApp Unreplied, Absensi Pending Approval, Network Device Down) dan Komprehensif Code Audit Fixes (Finance, Asset, Developer Logs & Traffic)
- **Status Branch**: `Belum di-merge` (Tersimpan di branch `issue-265` / remote `origin/issue-265`, siap verifikasi & merge)

---

### 📅 Rincian Commit

#### [`c01fda7`](https://github.com/user/repo/commit/c01fda7) - save #265 - 3 September 2026, 23:10:03 WIB

- **Komponen yang Berubah**:
  - **Backend — Endpoint Penghitung Badge Real-Time (Count Services & Routes)**:
    - [`backend/src/services/ticket.service.js`](backend/src/services/ticket.service.js) & [`backend/src/controllers/ticket.controller.js`](backend/src/controllers/ticket.controller.js) & [`backend/src/routes/ticket.route.js`](backend/src/routes/ticket.route.js) — Penambahan service, controller, dan rute `GET /api/v1/tickets/count-open` (privilege: `ticket.read`):
      - Menghitung jumlah tiket aktif berstatus `open` atau `in_progress`.
      - **Role-Aware Filtering**: Jika admin memiliki privilege penuh (`ticket.readAll`), menghitung seluruh tiket yang membutuhkan penanganan; jika hanya memiliki akses tugas pribadi (`ticket.readAssignedOnly`), query otomatis membatasi perhitungan hanya pada tiket yang ditugaskan kepada admin yang sedang login (`assigned_to: req.user.id`).
    - [`backend/src/services/waConversation.service.js`](backend/src/services/waConversation.service.js) & [`backend/src/controllers/waChat.controller.js`](backend/src/controllers/waChat.controller.js) & [`backend/src/routes/waChat.route.js`](backend/src/routes/waChat.route.js) — Penambahan rute `GET /api/v1/whatsapp-chat/count-unreplied` (privilege: `waChat.read`):
      - Menghitung percakapan aktif yang pesan terakhirnya adalah inbound dari pelanggan dan belum dibalas oleh tim Customer Service.
    - [`backend/src/services/attendance.service.js`](backend/src/services/attendance.service.js) & [`backend/src/controllers/attendance.controller.js`](backend/src/controllers/attendance.controller.js) & [`backend/src/routes/attendance.route.js`](backend/src/routes/attendance.route.js) — Penambahan rute `GET /api/v1/attendance/count-pending` (privilege: `attendance.read`):
      - Menghitung total pengajuan absensi, izin, dan cuti karyawan yang berstatus `pending` dan memerlukan persetujuan HR/Admin.
    - [`backend/src/services/networkDevice.service.js`](backend/src/services/networkDevice.service.js) & [`backend/src/controllers/networkDevice.controller.js`](backend/src/controllers/networkDevice.controller.js) & [`backend/src/routes/networkDevice.route.js`](backend/src/routes/networkDevice.route.js) — Penambahan rute `GET /api/v1/network-devices/count-down` (privilege: `networkDevice.read`):
      - Menghitung jumlah perangkat jaringan (router, switch, OLT) aktif yang terdeteksi sedang `down` atau unreachable.
    - [`backend/src/sockets/waChat.controller.js`](backend/src/sockets/waChat.controller.js) — Penambahan broadcast event soket saat pesan pelanggan baru diterima untuk memicu live update counter pada client.
    - Unit Tests Backend [NEW]:
      - [`backend/test/unit/ticketOpenCount.service.test.js`](backend/test/unit/ticketOpenCount.service.test.js) [NEW] — Validasi logika hitung tiket open untuk skenario full privilege dan assigned-only.
      - [`backend/test/unit/waUnrepliedCount.service.test.js`](backend/test/unit/waUnrepliedCount.service.test.js) [NEW] — Pengujian akurasi hitung chat WA yang belum terbalas.
      - [`backend/test/unit/attendancePendingCount.service.test.js`](backend/test/unit/attendancePendingCount.service.test.js) [NEW] — Pengujian hitung absensi berstatus pending.
      - [`backend/test/unit/networkDeviceDownCount.service.test.js`](backend/test/unit/networkDeviceDownCount.service.test.js) [NEW] — Pengujian hitung perangkat jaringan berstatus down.
  - **Frontend — Redux State, Custom Hooks, dan Komponen Sidebar Menu**:
    - [`frontend/src/features/ticketSlice.js`](frontend/src/features/ticketSlice.js) [NEW] — Slice Redux untuk mengelola state `openCount` tiket, status fetching, dan action `fetchOpenTicketCount`.
    - [`frontend/src/features/waChatSlice.js`](frontend/src/features/waChatSlice.js) [NEW] — Slice Redux untuk state `unrepliedCount` chat WhatsApp.
    - [`frontend/src/features/attendanceSlice.js`](frontend/src/features/attendanceSlice.js) [NEW] — Slice Redux untuk state `pendingCount` absensi.
    - [`frontend/src/features/networkDeviceSlice.js`](frontend/src/features/networkDeviceSlice.js) [NEW] — Slice Redux untuk state `downCount` perangkat jaringan.
    - [`frontend/src/store.js`](frontend/src/store.js) — Pendaftaran seluruh slice badge ke dalam Redux root store.
    - [`frontend/src/hooks/useTicketBadge.js`](frontend/src/hooks/useTicketBadge.js) [NEW] & [`frontend/src/hooks/index.js`](frontend/src/hooks/index.js) — Custom hook yang menghubungkan state badge tiket, polling otomatis, dan sinkronisasi socket event.
    - [`frontend/src/app/layouts/Root.jsx`](frontend/src/app/layouts/Root.jsx) — Inisialisasi pengambilan count saat aplikasi pertama kali dimuat serta pemasangan listener Socket.IO terpadu untuk pembaruan badge secara instan tanpa reload halaman.
    - Komponen Menu Navigasi:
      - [`frontend/src/app/layouts/MainLayout/Sidebar/MainPanel/Menu/Item.jsx`](frontend/src/app/layouts/MainLayout/Sidebar/MainPanel/Menu/Item.jsx) — Rendering pill badge angka pada sidebar menu utama.
      - [`frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/MenuItem.jsx`](frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/MenuItem.jsx) — Rendering badge pada panel prime menu sidebar.
      - [`frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/MenuItem.jsx`](frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/MenuItem.jsx) & [`index.jsx`](frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/index.jsx) — Rendering badge pada layout sideblock sidebar.
  - **Audit Bug Fixes & Stabilitas Modul Finance, Log, Asset, dan Traffic**:
    - Penstabilan & perbaikan test suite integrasi finance: [`financeCoa.multiLedger.test.js`](backend/test/integration/financeCoa.multiLedger.test.js), [`financeExpensePayableAging.test.js`](backend/test/integration/financeExpensePayableAging.test.js), [`financeInvoice.create.test.js`](backend/test/integration/financeInvoice.create.test.js), [`financeInvoiceReceivableAging.test.js`](backend/test/integration/financeInvoiceReceivableAging.test.js), [`financeInvoiceReport.test.js`](backend/test/integration/financeInvoiceReport.test.js), [`financePayment.crashRecovery.test.js`](backend/test/integration/financePayment.crashRecovery.test.js), [`financePeriod.test.js`](backend/test/integration/financePeriod.test.js), [`financeReport.cashFlow.test.js`](backend/test/integration/financeReport.cashFlow.test.js), [`financeTransactionDraft.recurring.test.js`](backend/test/integration/financeTransactionDraft.recurring.test.js).
    - Perbaikan model [`financeRegulatorySettings.model.js`](backend/src/models/financeRegulatorySettings.model.js) dan controller finance [`financeExpense.controller.js`](backend/src/controllers/financeExpense.controller.js), [`financeLedger.service.js`](backend/src/services/financeLedger.service.js), [`financeRecurring.service.js`](backend/src/services/financeRecurring.service.js), [`financeReport.service.js`](backend/src/services/financeReport.service.js).
    - Pembenahan UI form & drawer finance: [`AssetFormDrawer.jsx`](frontend/src/app/pages/finance/assets/AssetFormDrawer.jsx), [`DetailDrawer.jsx`](frontend/src/app/pages/finance/assets/DetailDrawer.jsx), [`DisposeModal.jsx`](frontend/src/app/pages/finance/assets/DisposeModal.jsx), [`payroll/runs/index.jsx`](frontend/src/app/pages/finance/payroll/runs/index.jsx), [`periods/ReopenModal.jsx`](frontend/src/app/pages/finance/periods/ReopenModal.jsx), [`regulatory/DetailDrawer.jsx`](frontend/src/app/pages/finance/regulatory/DetailDrawer.jsx), [`FixedAssetReport.jsx`](frontend/src/app/pages/finance/reports/FixedAssetReport.jsx), [`ReportCard.jsx`](frontend/src/app/pages/finance/reports/ReportCard.jsx).
    - Refinement developer logs & traffic: [`AiLogAnalysisModal.jsx`](frontend/src/app/pages/settings/sections/developer/logs/AiLogAnalysisModal.jsx), [`LogDetailDrawer.jsx`](frontend/src/app/pages/settings/sections/developer/logs/components/LogDetailDrawer.jsx), [`FakeTrafficTab.jsx`](frontend/src/app/pages/settings/sections/developer/fakeTraffic/FakeTrafficTab.jsx), [`TrafficSettingsModal.jsx`](frontend/src/app/pages/network/traffic/components/TrafficSettingsModal.jsx), [`RrdZoomableGraph.jsx`](frontend/src/app/pages/network/traffic/components/RrdZoomableGraph.jsx), [`GraphStyleFields.jsx`](frontend/src/app/pages/network/traffic/components/GraphStyleFields.jsx).
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan visibilitas instan kepada staf dan manajemen atas beban operasional yang membutuhkan tindakan langsung (tiket kendala pelanggan, pesan CS masuk, persetujuan absensi, dan gangguan link perangkat) melalui badge angka animasi di sidebar, sekaligus mengeliminasi kelemahan/flaky test pada suite modul Finance dan Developer Logs pasca-audit menyeluruh.

---

## 🌿 Branch: `issue-175` — Integrasi OLT Multi-Vendor (C-Data & V-SOL CLI Crawler / Driver), Manajemen Perangkat OLT, Auto-Discovery ONU, dan Rekap Proyek TR-069 / ACS

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: Integrasi OLT Multi-Vendor (C-Data & V-SOL CLI Crawler / Driver), Manajemen Perangkat OLT, Auto-Discovery & Zero-Touch Provisioning ONU, dan Rekapitulasi Proyek TR-069 / ACS
- **Status Branch**: `Belum di-merge` (Tersimpan di branch `origin/issue-175`; status pengerjaan ditunda sementara dan dirangkum dalam dokumentasi rekap proyek untuk dilanjutkan pada sesi berikutnya)

---

### 📅 Rincian Commit

#### [`dd5c4ce`](https://github.com/user/repo/commit/dd5c4ce) - save #175 - 3 September 2026, 20:48:38 WIB

- **Komponen yang Berubah**:
  - **Arsitektur Model & Driver OLT Multi-Vendor**:
    - [`backend/src/models/oltDevice.model.js`](backend/src/models/oltDevice.model.js) [NEW] — Skema Mongoose master perangkat OLT: penamaan, merk/vendor (`cdata`, `vsol`, `huawei`, `zte`), tipe model, konfigurasi IP address & port Telnet/SNMP, kredensial login (username, password, enable password, SNMP community), status konektivitas, kapasitas port uplink/PON, serta total ONU terhubung.
    - [`backend/src/services/oltDrivers/registry.js`](backend/src/services/oltDrivers/registry.js) [NEW] — Driver registry pattern untuk memuat driver vendor OLT yang sesuai secara dinamis berdasarkan brand perangkat yang dipilih.
    - [`backend/src/services/oltDrivers/cdata.driver.js`](backend/src/services/oltDrivers/cdata.driver.js) [NEW] — Driver interaksi Telnet/CLI untuk OLT C-Data: parsing respons command CLI, navigasi mode privilege/config, pembacaan daftar ONU belum teregistrasi (*unregistered ONU auto-discovery*), penarikan status redaman sinyal optik RX/TX power (dBm), serta perintah konfigurasi profil PPPoE.
    - [`backend/src/services/oltDrivers/vsol.driver.js`](backend/src/services/oltDrivers/vsol.driver.js) [NEW] — Driver dasar untuk OLT V-SOL (V1600 series).
    - [`backend/src/services/oltDevice.service.js`](backend/src/services/oltDevice.service.js) [NEW] & [`backend/src/controllers/oltDevice.controller.js`](backend/src/controllers/oltDevice.controller.js) [NEW] & [`backend/src/routes/oltDevice.route.js`](backend/src/routes/oltDevice.route.js) [NEW] — Endpoint dan logika CRUD perangkat OLT, pengujian konektivitas Telnet/SNMP, sinkronisasi status port PON, dan pembacaan ONU aktif.
    - [`backend/src/models/radiusAuthentication.model.js`](backend/src/models/radiusAuthentication.model.js) & [`backend/src/controllers/radiusAuthentication.controller.js`](backend/src/controllers/radiusAuthentication.controller.js) — Integrasi data autentikasi PPPoE dengan referensi OLT Device, nomor port PON, dan identitas ONU (ID/Serial Number) untuk mendukung provisioning terpadu.
    - [`backend/src/app.js`](backend/src/app.js), [`backend/src/config/privilege.json`](backend/src/config/privilege.json), [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json), [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json) — Pendaftaran rute API, privilege `oltDevice`, dan translasi i18n.
  - **Tooling CLI Crawler & Dokumentasi Teknis OLT C-Data**:
    - [`backend/scripts/oltCliCrawler.js`](backend/scripts/oltCliCrawler.js) [NEW] — Script crawler interaktif Telnet otomatis untuk menjelajah seluruh pohon perintah CLI OLT C-Data secara rekursif melalui bantuan prompt `?` / `help`.
    - [`documentations/olt-cdata-command-tree.json`](documentations/olt-cdata-command-tree.json) [NEW] & [`documentations/olt-cdata-command-tree.md`](documentations/olt-cdata-command-tree.md) [NEW] — Katalog lengkap ribuan perintah CLI OLT C-Data dari mode pengguna dasar hingga mode konfigurasi mendalam.
    - [`documentations/olt-cdata-reference.md`](documentations/olt-cdata-reference.md) [NEW] — Lembar panduan referensi cepat perintah Telnet C-Data untuk konfigurasi ONU, VLAN, DBA profile, line profile, dan service-port.
    - [`documentations/olt-cdata-crawl-debug.log`](documentations/olt-cdata-crawl-debug.log) [NEW] — Log rekaman eksekusi crawling CLI pada perangkat OLT nyata.
    - [`documentations/olt-acs-project-recap.md`](documentations/olt-acs-project-recap.md) [NEW] — Dokumen rekapitulasi arsitektur OLT multi-vendor dan rencana integrasi ACS (TR-069 via GenieACS) yang merangkum hasil riset lapangan, keputusan arsitektur (rujukan SmartOLT tanpa engine pihak ketiga, GenieACS standalone service), serta panduan langkah sebelum melanjutkan pengerjaan berikutnya.
- **Deskripsi Perubahan & Fungsi**:
  - Membuka kapabilitas V2 untuk mengenali dan mengontrol perangkat transmisi fisik optik (OLT), memungkinkan deteksi ONU baru di lapangan, serta mendokumentasikan pohon perintah perangkat OLT C-Data secara menyeluruh sebagai fondasi zero-touch provisioning pelanggan.

---

## 🌿 Branch: `master` / `issue-264` — Manajemen Pengarsipan Log Sistem MinIO S3 (Archive Preview, Export GZ/JSON, Purge Expired Logs, dan Tab MinIO Archive)

### 📌 Informasi Issue

- **Nomor Issue**: #264
- **Judul Issue**: Manajemen Pengarsipan Log Sistem MinIO S3 (Archive Preview, Export GZ/JSON, Purge Expired Logs, Cron Archiving, dan UI Tab MinIO Archive)
- **Status Branch**: `Sudah di-merge` (Merge commit [`5477907`](https://github.com/user/repo/commit/5477907) ke `master` pada 3 September 2026, 20:41:15 WIB)

---

### 📅 Rincian Commit

#### [`5477907`](https://github.com/user/repo/commit/5477907) - resolve #264 - 3 September 2026, 20:41:15 WIB
#### [`210e73c`](https://github.com/user/repo/commit/210e73c) - resolve #264 - 3 September 2026, 12:16:23 WIB

- **Komponen yang Berubah**:
  - **Backend — MinIO S3 Client Utility & Log Archive Engine**:
    - [`backend/src/utils/minio.js`](backend/src/utils/minio.js) [NEW] — Utilitas integrasi storage MinIO S3: inisialisasi bucket otomatis, streaming upload berkas, pembuatan presigned URL untuk unduh/lihat file secara aman, streaming dekompresi berkas `.gz`, serta fungsi penghapusan batch object.
    - [`backend/src/services/logArchive.service.js`](backend/src/services/logArchive.service.js) [NEW] — Engine pengarsipan log terpadu:
      - **Kompresi Gzip Ringan**: Mengompresi record log API historis dari MongoDB (`ApiLog`) ke dalam arsip format JSON terkompresi Gzip (`.json.gz`) berdasarkan rentang tanggal.
      - **Upload ke MinIO**: Menyimpan arsip ke dalam bucket khusus `system-logs-archive`.
      - **Pratinjau Tanpa Unduh Penuh (`previewArchive`)**: Membaca dan mengurai isi arsip log langsung dari MinIO secara streaming dengan batasan record (`limit`) sehingga admin dapat meninjau log lama tanpa perlu mengunduh seluruh file secara manual.
      - **Pembersihan Log Usang (`purgeExpiredLogs`)**: Menghapus data log lama dari MongoDB yang telah sukses diarsipkan ke MinIO untuk menghemat kapasitas storage database utama.
    - [`backend/src/services/apiLog.service.js`](backend/src/services/apiLog.service.js) & [`backend/src/controllers/log.controller.js`](backend/src/controllers/log.controller.js) & [`backend/src/routes/log.route.js`](backend/src/routes/log.route.js) — Penambahan rute API pengarsipan log (privilege: `log.read`, `log.manage`):
      - `GET /api/v1/logs/archives`: Mengambil daftar berkas arsip MinIO beserta ukuran file dan rentang tanggal.
      - `POST /api/v1/logs/archives/run`: Memicu proses pengarsipan log manual untuk rentang tanggal tertentu.
      - `GET /api/v1/logs/archives/preview`: Mengambil sampel isi rekaman log di dalam berkas arsip terkompresi.
      - `GET /api/v1/logs/archives/download-url`: Mendapatkan presigned URL untuk pengunduhan aman berkas arsip.
      - `DELETE /api/v1/logs/archives`: Menghapus berkas arsip tertentu dari storage MinIO.
    - [`backend/test/unit/apiLog.service.test.js`](backend/test/unit/apiLog.service.test.js) — Penambahan unit test untuk skenario pengarsipan, preview arsip, dan penghapusan data log.
  - **Frontend — UI Tab MinIO Archive & Modal Pratinjau**:
    - [`frontend/src/app/pages/settings/sections/developer/logs/components/MinioArchiveTab.jsx`](frontend/src/app/pages/settings/sections/developer/logs/components/MinioArchiveTab.jsx) [NEW] — Tab antarmuka "Arsip MinIO" pada menu Developer Logs:
      - Menampilkan tabel berkas arsip dengan indikator ukuran berkas terkompresi, tanggal pembuatan, dan rentang tanggal isi log.
      - Tombol aksi: **Pratinjau Isi**, **Unduh File**, dan **Hapus Arsip**.
      - Tombol pemicu pengarsipan manual dengan pemilih rentang tanggal (*date range picker*).
    - [`frontend/src/app/pages/settings/sections/developer/logs/components/ArchivePreviewModal.jsx`](frontend/src/app/pages/settings/sections/developer/logs/components/ArchivePreviewModal.jsx) [NEW] — Modal interaktif pratinjau isi arsip log dengan tabel terstruktur, fitur pencarian instan, dan filter level log (info/warn/error).
    - [`frontend/src/app/pages/settings/sections/developer/logs/SystemLogsTab.jsx`](frontend/src/app/pages/settings/sections/developer/logs/SystemLogsTab.jsx) — Penataan layout tab MinIO Archive berdampingan dengan System Logs dan Audit Trail.
    - [`frontend/src/app/pages/settings/sections/developer/logs/components/AiAnalysisModal.jsx`](frontend/src/app/pages/settings/sections/developer/logs/components/AiAnalysisModal.jsx) — Penyesuaian modal analisis AI log dengan dukungan rendering markdown yang lebih rapi.
    - [`frontend/src/components/ui/Modal.jsx`](frontend/src/components/ui/Modal.jsx) — Penambahan varian ukuran modal `max-w-6xl` dan `max-w-7xl` untuk tabel pratinjau data log yang padat.
    - [`backend/src/locales/`](backend/src/locales/) & [`frontend/src/i18n/locales/`](frontend/src/i18n/locales/) — Terjemahan lengkap bilingual untuk seluruh terminologi modul MinIO Archive.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan solusi pengelolaan kapasitas database jangka panjang dengan memindahkan record log lawas ke object storage MinIO S3 dalam format terkompresi `.json.gz`, sekaligus memberikan kenyamanan kepada developer untuk meninjau, mengunduh, atau menghapus arsip langsung dari browser.

---

## 🌿 Branch: `master` / `issue-254` — Pemantauan Trafik Antarmuka RRDtool pada Layanan Dedicated Internet, Data Access & Broadband

### 📌 Informasi Issue

- **Nomor Issue**: #254
- **Judul Issue**: Pemantauan Trafik Antarmuka Berbasis RRDtool pada Layanan Dedicated Internet, Data Access & Broadband (Direct Host SNMP Discovery, Service Traffic Target Linking, Mutex Lock Serialisasi RRD & Dynamic Timestamp Bumping)
- **Status Branch**: `Sudah di-merge` (Merge commit [`74a1904`](https://github.com/user/repo/commit/74a1904) ke `master` pada 3 September 2026, 11:20:15 WIB)

---

### 📅 Rincian Commit

#### [`74a1904`](https://github.com/user/repo/commit/74a1904) - resolve #254 - 3 September 2026, 11:20:15 WIB
#### [`174b9e6`](https://github.com/user/repo/commit/174b9e6) - resolve #254 - 3 September 2026, 10:42:10 WIB

- **Komponen yang Berubah**:
  - **Backend & Script Migrasi Data**:
    - [`backend/scripts/backfillBroadbandTrafficTargets.js`](backend/scripts/backfillBroadbandTrafficTargets.js) [NEW] — Script migrasi data historis untuk mengaitkan target trafik RRD antarmuka pelanggan broadband eksisting secara otomatis.
    - [`backend/src/services/broadbandTraffic.service.js`](backend/src/services/broadbandTraffic.service.js) [NEW] — Layanan khusus penanganan pemantauan trafik antarmuka pelanggan broadband (PPPoE/IPoE).
    - [`backend/src/services/networkTraffic.service.js`](backend/src/services/networkTraffic.service.js) — Penyempurnaan menyeluruh fungsi polling multi-sumber, discovery antarmuka direct host, dan pelepasan target layanan secara aman (*safe deletion*).
    - [`backend/src/models/networkTrafficTarget.model.js`](backend/src/models/networkTrafficTarget.model.js) — Relasi skema layanan (`service`), direct SNMP host credentials, dan indeks unik parsial compound.
    - [`backend/src/routes/networkTraffic.route.js`](backend/src/routes/networkTraffic.route.js) & [`backend/src/controllers/networkTraffic.controller.js`](backend/src/controllers/networkTraffic.controller.js) — Endpoint discovery host kustom dan query target per layanan.
    - [`backend/src/routes/internal.route.js`](backend/src/routes/internal.route.js) & [`backend/src/controllers/internal.controller.js`](backend/src/controllers/internal.controller.js) — Endpoint internal trigger polling broadband traffic untuk cron worker.
    - [`backend/src/data/changelog/releases/issue-254.json`](backend/src/data/changelog/releases/issue-254.json) [NEW] & [`backend/src/data/changelog/index.json`](backend/src/data/changelog/index.json) — Dokumentasi rilis resmi changelog v1.57.2.
  - **Cron Worker — Penjadwalan Polling Trafik Berkala**:
    - [`cron-worker/src/jobs/processors/broadbandTrafficPoll.js`](cron-worker/src/jobs/processors/broadbandTrafficPoll.js) [NEW] — Worker BullMQ untuk eksekusi polling trafik antarmuka layanan secara teratur.
    - [`cron-worker/src/jobs/scheduler.js`](cron-worker/src/jobs/scheduler.js), [`worker.js`](cron-worker/src/jobs/worker.js), [`api.service.js`](cron-worker/src/services/api.service.js) — Pendaftaran job scheduler periodik dengan proteksi concurrency.
  - **Network Monitor Service — Pengamanan File RRD**:
    - [`network-monitor/src/services/rrd.service.js`](network-monitor/src/services/rrd.service.js) & [`network-monitor/src/controllers/rrd.controller.js`](network-monitor/src/controllers/rrd.controller.js) — Penerapan mutex lock serialisasi (`withRrdLock`) dan dynamic timestamp bumping (`MAX_RRD_TIMESTAMP_BUMP = 5`) untuk memitigasi kesalahan *“illegal attempt to update using time”*.
    - [`network-monitor/src/routes/rrd.route.js`](network-monitor/src/routes/rrd.route.js) — Alias endpoint hapus file RRD.
  - **Frontend — Komponen Monitoring Modern pada Halaman Layanan**:
    - [`frontend/src/app/pages/services/components/AddServiceMonitoringDrawer.jsx`](frontend/src/app/pages/services/components/AddServiceMonitoringDrawer.jsx) [NEW] — Drawer form penambahan monitoring trafik layanan dengan opsi input IP & SNMP manual atau memilih router master.
    - [`frontend/src/app/pages/services/components/ServiceTrafficMonitoring.jsx`](frontend/src/app/pages/services/components/ServiceTrafficMonitoring.jsx) [NEW] — Panel monitoring grafik trafik RRD modern pada halaman detail layanan.
    - [`frontend/src/app/pages/services/broadband/detail.jsx`](frontend/src/app/pages/services/broadband/detail.jsx), [`dataAccess/detail.jsx`](frontend/src/app/pages/services/dataAccess/detail.jsx), [`dedicatedInternet/detail.jsx`](frontend/src/app/pages/services/dedicatedInternet/detail.jsx) — Integrasi panel `ServiceTrafficMonitoring` ke halaman detail layanan.
    - Komponen Grafis: [`TrafficGraphCard.jsx`](frontend/src/app/pages/network/traffic/components/TrafficGraphCard.jsx), [`TrafficTargetEditModal.jsx`](frontend/src/app/pages/network/traffic/components/TrafficTargetEditModal.jsx), [`TrafficTimeFilterBar.jsx`](frontend/src/app/pages/network/traffic/components/TrafficTimeFilterBar.jsx).
- **Deskripsi Perubahan & Fungsi**:
  - Menyelesaikan seluruh implementasi visualisasi trafik RRD langsung di halaman layanan internet pelanggan (Broadband, Dedicated, Data Access), menyediakan crawler SNMP langsung ke CPE pelanggan tanpa ketergantungan inventori router master, serta mengamankan stabilitas time-series RRD dengan mutex lock dan penanganan tabrakan waktu.

---

## 🌿 Branch: `master` / `issue-260` — Pengaturan Visibilitas Mitra Portal & Isolasi Resource Partner API

### 📌 Informasi Issue

- **Nomor Issue**: #260
- **Judul Issue**: Pengaturan Visibilitas Mitra pada Portal Mitra (`show_in_portal`), Toggle Status Portal Visibility, Filter Reseller Portal, dan Isolasi Resource Partner API (Perangkat Jaringan, Radius, Broadband, Business)
- **Status Branch**: `Sudah di-merge` (Merge commit [`69c54dc`](https://github.com/user/repo/commit/69c54dc) ke `master` pada 3 September 2026, 10:09:01 WIB)

---

### 📅 Rincian Commit

#### [`69c54dc`](https://github.com/user/repo/commit/69c54dc) - resolve #260 - 3 September 2026, 10:09:01 WIB

- **Komponen yang Berubah**:
  - **Backend — Kontrol Visibilitas Portal Mitra & Isolasi Resource**:
    - [`backend/src/models/partner.model.js`](backend/src/models/partner.model.js) — Penambahan field `show_in_portal` (boolean, default: `false`, indexed) serta compound index `{ reseller: 1, show_in_portal: 1 }`.
    - [`backend/src/routes/partner.route.js`](backend/src/routes/partner.route.js) & [`backend/src/controllers/partner.controller.js`](backend/src/controllers/partner.controller.js) & [`backend/src/services/partner.service.js`](backend/src/services/partner.service.js) — Penambahan rute `PATCH /api/v1/partner/change-portal-visibility` (privilege: `partner.changeSensitive`) untuk mengubah hak tampil mitra di portal reseller secara instan.
    - Penyaringan Resource Partner API: [`partnerApiNetworkDevice.controller.js`](backend/src/controllers/partnerApiNetworkDevice.controller.js), [`partnerApiRadius.controller.js`](backend/src/controllers/partnerApiRadius.controller.js), [`partnerApiProductBroadband.controller.js`](backend/src/controllers/partnerApiProductBroadband.controller.js), [`partnerApiBusiness.controller.js`](backend/src/controllers/partnerApiBusiness.controller.js), [`partnerApiCustomer.controller.js`](backend/src/controllers/partnerApiCustomer.controller.js), [`partnerApiMap.controller.js`](backend/src/controllers/partnerApiMap.controller.js):
      - Pengetatan isolasi data: memastikan kredensial API mitra hanya dapat mengakses data pelanggan, profil RADIUS, perangkat, dan peta coverage yang dimiliki secara sah oleh mitra tersebut.
    - Integration Tests Backend [NEW]:
      - [`backend/test/integration/partnerApiNetworkDevice.test.js`](backend/test/integration/partnerApiNetworkDevice.test.js) [NEW] — Validasi isolasi data perangkat jaringan pada Partner API.
      - [`backend/test/integration/partnerApiRadius.test.js`](backend/test/integration/partnerApiRadius.test.js) [NEW] — Validasi isolasi profil RADIUS pada Partner API.
  - **Frontend — Pengaturan Tampilan & Kolom Portal Mitra**:
    - [`frontend/src/app/pages/users/partner/schema/columns.jsx`](frontend/src/app/pages/users/partner/schema/columns.jsx) — Penambahan kolom **Portal Mitra** dengan badge interaktif status tampil/tersembunyi.
    - [`frontend/src/app/pages/users/partner/profile.jsx`](frontend/src/app/pages/users/partner/profile.jsx) — Tombol aksi toggle visibilitas portal pada halaman profil mitra.
    - [`frontend/src/components/shared/Badge.jsx`](frontend/src/components/shared/Badge.jsx) & [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx) — Status badge helper untuk status visibilitas portal.
    - [`backend/src/locales/`](backend/src/locales/) & [`frontend/src/i18n/locales/`](frontend/src/i18n/locales/) — Key terjemahan teks pengaturan portal mitra.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan kewenangan penuh kepada admin untuk mengatur apakah suatu mitra bisnis diizinkan tampil pada katalog portal reseller mitra publik, serta memperketat keamanan isolasi data Partner API agar tidak terjadi kebocoran informasi lintas mitra.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #213  | Integrasi Multi-Akun WhatsApp Baileys & Microservice `baileys-api` | Menyediakan microservice khusus untuk menghubungkan nomor WhatsApp multi-device tanpa biaya Meta Cloud API, dilengkapi QR pairing instan, auth state terenkripsi di MongoDB, rate limiter, anti-ban simulator, dan pemisahan kanal percakapan CS terpadu. |
| #265  | Dynamic Badge Counters Navigasi Sidebar & Code Audit Fixes | Menampilkan badge notifikasi angka real-time pada menu sidebar untuk tiket aktif, chat CS yang belum dijawab, persetujuan absensi pending, dan router down, serta menstabilkan suite test modul Finance dan Developer Logs. |
| #175  | Integrasi OLT Multi-Vendor (C-Data/V-SOL) & Rencana TR-069 ACS | Membangun fondasi manajemen OLT optik dan penemuan ONU belum terkonfigurasi (*unregistered ONU*), merilis script Telnet CLI crawler, mendokumentasikan pohon perintah C-Data secara lengkap, dan menyusun rekap arsitektur integrasi ACS. |
| #264  | Manajemen Pengarsipan Log Sistem MinIO S3 & Tab UI Archive | Menyediakan kemampuan pengarsipan data log MongoDB ke object storage MinIO S3 dalam format `.json.gz`, fitur pratinjau isi log langsung dari browser tanpa unduh manual, tombol purge log kedaluwarsa, dan tab UI MinIO Archive. |
| #254  | Monitoring Trafik RRD Layanan Dedicated, Data Access & Broadband | Mengintegrasikan grafik pemantauan bandwidth RRDtool ke seluruh halaman detail layanan pelanggan, SNMP auto-discovery langsung ke IP pelanggan, script backfill data broadband, worker polling, dan serialisasi mutex file RRD. |
| #260  | Visibilitas Mitra Portal Reseller & Isolasi Resource Partner API | Menyediakan fitur kontrol penayangan mitra pada portal mitra (`show_in_portal`), tombol toggle status portal, serta memperketat isolasi multi-tenant data pada seluruh endpoint Partner API. |

---

### 🚀 Kemampuan Baru Pengguna/Admin

- **Indikator Beban Kerja Real-Time di Sidebar**: Admin kini dapat langsung mengetahui adanya tiket kendala yang belum selesai, chat pelanggan yang butuh respon, permohonan izin karyawan yang menunggu persetujuan, atau router yang down tanpa harus membuka menu satu per satu.
- **Dukungan WhatsApp Multi-Nomor (Baileys)**: Admin dapat mendaftarkan nomor WhatsApp operasional tambahan via scan QR code langsung di web, memanfaatkan nomor lokal untuk customer service atau bot dengan kontrol anti-banned.
- **Pemisahan Jalur Komunikasi Pelanggan**: Sesi percakapan CS kini mengenali channel asal (Meta Cloud API vs Baileys), memungkinkan admin merespon pelanggan dari nomor WhatsApp bisnis yang tepat.
- **Eksplorasi & Pratinjau Arsip Log MinIO**: Developer dan admin sistem dapat memeriksa riwayat aktivitas sistem berbulan-bulan lalu yang sudah diarsipkan ke MinIO tanpa perlu menyedot kapasitas MongoDB utama.
- **Monitoring Trafik Terpadu pada Seluruh Layanan Pelanggan**: Layanan Broadband, Dedicated Internet, dan Data Access kini seragam memiliki visualisasi pemakaian bandwidth berbasis grafik RRDtool interaktif.
- **Manajemen Inventori & Konfigurasi OLT**: Admin jaringan dapat mendaftarkan perangkat OLT C-Data atau V-SOL dan memantau status port PON serta ONU yang terhubung.
- **Kontrol Penayangan Mitra Reseller**: Admin bisnis dapat mengaktifkan atau menyembunyikan profil mitra tertentu dari portal mitra hanya dengan sekali klik pada tabel mitra.

---

### 🛠️ Bug Fix / Solusi Masalah

- **Penyelesaian Tes Integrasi Finance Flaky**: Memperbaiki urutan pembukuan ledger multi-mata uang, penanganan crash recovery pada pembayaran faktur, dan skema validasi pengaturan regulasi pajak sehingga seluruh rangkaian unit & integration test berjalan lulus 100%.
- **Penanganan Illegal Timestamp Update pada RRDtool**: Penggunaan mutex lock per target ID dan *Dynamic Timestamp Bumping* pada microservice `network-monitor` mencegah penolakan pembaruan data time-series saat dua proses polling berjalan bersamaan.
- **Pencegahan Akses Lintas Mitra pada Partner API**: Menambahkan klausa filter identitas mitra yang ketat pada endpoint perangkat jaringan, pelanggan, dan profil RADIUS agar mitra tidak dapat mengintip aset mitra lain.
- **Pembersihan Log MongoDB yang Efisien**: Mekanisme purge log otomatis pasca-arsip ke MinIO memastikan ukuran database MongoDB tetap optimal dan tidak mengalami penurunan performa query.
- **Penyesuaian Ukuran Modal Pratinjau Lebar**: Komponen `Modal.jsx` kini mendukung ukuran `max-w-6xl` dan `max-w-7xl` sehingga modal pratinjau arsip log dan analisis AI tidak terpotong pada layar desktop standar.

---

### 📦 Menu/Fitur Baru

- **Menu Akun WhatsApp (Baileys)**: Tersedia di **Customer Service > Akun WhatsApp (Baileys)** (`/customer-service/baileys-account`).
- **Tab Arsip MinIO**: Tersedia pada menu **Pengaturan > Developer > Log Sistem > Tab Arsip MinIO**.
- **Badge Counter Navigasi Sidebar**: Pill counter dinamis pada menu **Bantuan & Dukungan > Tiket**, **Customer Service > Obrolan WA**, **Manajemen Staf > Absensi**, dan **Jaringan > Perangkat Jaringan**.
- **Panel Monitoring Trafik Broadband**: Tersedia pada halaman **Layanan > Broadband > Detail**.
- **Kolom Portal Mitra**: Tersedia pada tabel **Pengguna > Mitra Bisnis**.
- **Dokumentasi Command Tree OLT C-Data**: Berkas referensi pohon perintah CLI OLT di `documentations/olt-cdata-command-tree.md` dan rekap proyek TR-069 di `documentations/olt-acs-project-recap.md`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Memanfaatkan Dynamic Real-Time Badge Counters pada Menu Navigasi Sidebar

- **Penjelasan Fitur**:
  - Fitur ini memberikan indikator visual berupa angka (badge) di samping label menu sidebar untuk menunjukkan jumlah item yang memerlukan perhatian segera.
  - Badge diperbarui secara otomatis menggunakan Socket.IO saat ada perubahan data di sistem tanpa perlu memuat ulang (*refresh*) halaman browser.

- **Daftar Menu & Kriteria Perhitungan**:
  1. **Tiket Kendala** (`/support/ticket`):
     - Menampilkan jumlah tiket yang berstatus `open` atau `in_progress`.
     - Jika pengguna adalah staf teknis/lapangan, badge hanya menghitung tiket yang ditugaskan kepada dirinya sendiri.
  2. **Obrolan WA** (`/customer-service/chat`):
     - Menampilkan jumlah percakapan yang pesan terakhirnya berasal dari pelanggan dan belum mendapatkan balasan dari staf.
  3. **Absensi & Cuti** (`/staff/attendance`):
     - Menampilkan jumlah permohonan absensi/izin karyawan yang berstatus `pending` dan menunggu persetujuan.
  4. **Perangkat Jaringan** (`/network/devices`):
     - Menampilkan jumlah router/switch/OLT yang status polling-nya terdeteksi `down` (indikator berwarna merah).

---

### 2. Mengelola & Memeriksa Arsip Log Sistem MinIO S3

- **Penjelasan Fitur**:
  - Fitur ini memungkinkan admin untuk mengarsipkan log transaksi API dari MongoDB ke object storage MinIO S3 dalam format terkompresi `.json.gz` guna menjaga performa database MongoDB.
  - Admin dapat langsung meninjau cuplikan isi log di dalam berkas terkompresi tanpa harus mengunduh file secara utuh.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Pengaturan > Developer > Log Sistem**.
  2. Pilih tab **Arsip MinIO**.
  3. Untuk melakukan pengarsipan manual:
     - Klik tombol **Arsipkan Log**.
     - Tentukan rentang tanggal log yang ingin diarsipkan (misal: 1 bulan yang lalu).
     - Klik **Mulai Pengarsipan**. Sistem akan mengekstrak data dari MongoDB, mengompresi ke `.json.gz`, dan mengunggahnya ke MinIO bucket `system-logs-archive`.
  4. Untuk memeriksa isi log pada arsip:
     - Cari berkas arsip yang diinginkan pada tabel.
     - Klik tombol **Pratinjau** (ikon mata).
     - Modal pratinjau akan menampilkan daftar entri log di dalam arsip lengkap dengan level log, metode HTTP, endpoint URL, dan pesan response.
  5. Untuk menyimpan salinan lokal, klik tombol **Unduh** untuk mendapatkan berkas `.json.gz`.

---

### 3. Menghubungkan Akun WhatsApp Baileys (Multi-Device Pairing)

- **Penjelasan Fitur**:
  - Modul Baileys memungkinkan penggunaan nomor WhatsApp biasa atau WhatsApp Business tanpa ketergantungan pada Meta Cloud API resmi.
  - Sesi koneksi disimpan secara aman dan terenkripsi AES-256 di MongoDB dan diproses oleh microservice `baileys-api`.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Customer Service > Akun WhatsApp (Baileys)**.
  2. Klik tombol **Tambah Akun**.
  3. Masukkan **Label Akun** (contoh: `CS Support Regional 1`), atur konfigurasi proxy bila diperlukan, dan tentukan limit pengiriman pesan harian.
  4. Klik **Simpan**. Akun baru akan muncul pada tabel dengan status `pending_qr`.
  5. Klik tombol **Hubungkan / Scan QR** pada baris akun tersebut.
  6. Buka aplikasi WhatsApp pada ponsel Anda, masuk ke menu **Perangkat Tertaut (Linked Devices)**, lalu arahkan kamera ke QR Code yang muncul di layar.
  7. Setelah pairing berhasil, status akun akan otomatis berubah menjadi `connected` berwarna hijau, dan nomor telepon WhatsApp akan terisi secara otomatis. Nomor ini sekarang siap menerima dan mengirim chat pelanggan.
