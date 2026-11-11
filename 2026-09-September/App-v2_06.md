# 📝 Daily Work Report - Dedy (2026-09-06)

---

## 📅 Laporan Harian - 6 September 2026

---

## 🌿 Branch: `master` / `issue-275` — Penguatan Keamanan, Stabilitas, & Audit Menyeluruh Sistem (Audit Hardening Modul Keuangan, Sesi Autentikasi Access Token, Pencegahan Balapan Concurrency & Path Traversal Database Tools, Sanitasi Data Table, dan Resiliensi Telegram Alert)

### 📌 Informasi Issue

- **Nomor Issue**: #275
- **Judul Issue**: Penguatan Keamanan, Stabilitas, & Audit Menyeluruh Sistem (Audit Hardening Modul Keuangan, Sesi Autentikasi Access Token, Pencegahan Balapan Concurrency & Path Traversal Database Tools, Sanitasi Data Table, dan Resiliensi Telegram Alert)
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `cf95140`)

---

### 📅 Rincian Commit

#### [`cf95140`](https://github.com/user/repo/commit/cf95140) - resolve #275 - 6 September 2026, 17:29:11 WIB
#### [`d9ba6c3`](https://github.com/user/repo/commit/d9ba6c3) - resolve #275 - 6 September 2026, 17:28:37 WIB

- **Komponen yang Berubah**:
  - **Backend Core — Autentikasi & Keamanan Sesi**:
    - [`backend/src/middlewares/auth.middleware.js`](backend/src/middlewares/auth.middleware.js) — Implementasi fungsi `assertSessionActive` pada seluruh middleware proteksi (`protectedAdmin`, `protectedCustomer`, `protectedPartnerApp`, `protectedDeveloper`):
      - Memvalidasi status keaktifan sesi (`jti`) secara real-time melalui cache Redis dan store sesi MongoDB (`findActiveSession`).
      - Menolak access token seketika dengan status HTTP `401 Unauthorized` (`AUTH_ERROR.SESSION_REVOKED`) jika sesi telah dicabut (misal aksi *logout* atau *logout all devices*), tanpa harus menunggu masa kedaluwarsa alami token (`exp`).
      - Menangani kegagalan ketersediaan store sesi secara elegan (`SessionStoreUnavailableError`) dengan HTTP 503.
    - [`backend/src/controllers/auth.controller.js`](backend/src/controllers/auth.controller.js) — Penambahan guard `hasUnsafeCredentialTypes(body)` pada `partnerGetToken` dan `customerGetToken` untuk memvalidasi tipe data username/password, mencegah potensi manipulasi tipe payload dan NoSQL injection.
  - **Backend Core — Pencegahan Concurrency Race & Path Traversal DB Tools**:
    - [`backend/src/models/dbToolsJob.model.js`](backend/src/models/dbToolsJob.model.js) — Penambahan field `active_lock` bertipe Boolean dengan partial unique index (`{ active_lock: 1 }, { unique: true, partialFilterExpression: { active_lock: { $exists: true } } }`).
    - [`backend/src/services/dbToolsQueue.service.js`](backend/src/services/dbToolsQueue.service.js) — Penggantian pola pemeriksaan lama non-atomik (*check-then-write*) dengan fungsi atomik `createLockedDbToolsJob`:
      - Mengeliminasi *race condition* saat beberapa permintaan migrasi/backup masuk bersamaan. MongoDB secara atomik menolak duplikasi slot aktif melalui error constraint `E11000`, yang diterjemahkan menjadi HTTP `409 Conflict` (`jobAlreadyRunning`).
    - [`backend/src/jobs/dbToolsWorker.js`](backend/src/jobs/dbToolsWorker.js) — Pelepasan kunci atomik (`$unset: { active_lock: '' }`) saat pekerjaan database selesai dieksekusi, baik dengan status `success` maupun `failed`.
    - [`backend/src/services/dbTools.service.js`](backend/src/services/dbTools.service.js):
      - Penambahan fungsi `assertValidObjectName` dengan regex ketat `BACKUP_FILENAME_REGEX` pada rute pembacaan dan penghapusan versi (`listMinioBackupObjectVersions`, `streamMinioBackupVersion`, `deleteMinioBackupVersion`) guna menutup celah *path traversal* (`../../`) pada MinIO.
      - Penambahan validasi `mongoose.Types.ObjectId.isValid` pada `getJobById` untuk mencegah *unhandled* `CastError` Mongoose (500) saat menerima ID tidak valid.
  - **Backend Core — Sanitasi Data Table & Ketahanan Telegram Alert**:
    - [`backend/src/utils/data-table.js`](backend/src/utils/data-table.js):
      - Penambahan pemeriksaan whitelist ketat `keys.includes(fl.id)` pada filter kolom bertipe select, range, dan ObjectId untuk mencegah pembocoran informasi keberadaan field rahasia model (*data oracle leakage*).
      - Penambahan saringan tipe Boolean (`[true, false, 1, 0, '1', '0', 'true', 'false', 'yes', 'no']`) untuk mencegah `CastError` 500 saat klien mengirimkan filter nilai boolean non-standar.
    - [`backend/src/utils/telegramAlert.js`](backend/src/utils/telegramAlert.js) — Pembungkusan pengiriman pesan per-destinasi dengan blok `try-catch` terisolasi, memastikan kegagalan atau *rate limit* satu grup/topik Telegram tidak menggagalkan pengiriman peringatan ke grup lainnya.
  - **Backend Core — Audit & Integritas Modul Keuangan (Finance)**:
    - [`backend/src/services/financeExpense.service.js`](backend/src/services/financeExpense.service.js) — Penambahan penanganan lampiran faktur (`attachmentNotFound`), validasi ketat status pembatalan dan pembayaran pengeluaran, penguncian status kas/akrual, serta audit trail jurnal buku besar.
    - [`backend/src/services/financeInvoice.service.js`](backend/src/services/financeInvoice.service.js) — Hardening pembaruan dan pembatalan faktur piutang, sinkronisasi status pembayaran parsial, dan proteksi integritas saldo pelanggan.
    - [`backend/src/services/financeCoa.service.js`](backend/src/services/financeCoa.service.js) — Perbaikan penjaluran hierarki akun (*path cascade*), pemutakhiran relasi parent-child Bagan Akun Standar (COA), dan pencegahan siklus relasi tak valid.
    - [`backend/src/services/financeReport.service.js`](backend/src/services/financeReport.service.js), [`backend/src/services/financeBudgeting.service.js`](backend/src/services/financeBudgeting.service.js), [`backend/src/services/financeGateway.service.js`](backend/src/services/financeGateway.service.js), [`backend/src/services/financeRecurring.service.js`](backend/src/services/financeRecurring.service.js), [`backend/src/services/financeRegulatoryObligation.service.js`](backend/src/services/financeRegulatoryObligation.service.js), [`backend/src/services/financeFixedAsset.service.js`](backend/src/services/financeFixedAsset.service.js) — Penyelarasan mutasi saldo, kalkulasi pajak, rekonsiliasi gateway, dan depresiasi aset tetap.
  - **Backend Core — Partner API & Perangkat Jaringan**:
    - [`backend/src/controllers/partnerApiCustomer.controller.js`](backend/src/controllers/partnerApiCustomer.controller.js), [`backend/src/controllers/partnerApiPartner.controller.js`](backend/src/controllers/partnerApiPartner.controller.js), [`backend/src/controllers/partnerApiNetworkDevice.controller.js`](backend/src/controllers/partnerApiNetworkDevice.controller.js) — Validasi input, pembersihan tipe data, dan perbaikan penanganan berkas/dokumen mitra.
    - [`backend/src/services/networkDevice.service.js`](backend/src/services/networkDevice.service.js) — Penguatan query relasi dan penanganan status perangkat jaringan offline.
  - **Pengujian Otomatis Komprehensif (Unit & Integration Tests)**:
    - Penambahan dan penyempurnaan suite pengujian Vitest dengan in-memory MongoDB sungguhan (131 berkas berubah, +17.660 baris tes):
      - Pengujian idempotensi dan transaksi outbox ledger keuangan.
      - Pengujian penanganan balapan (*race condition*) pada pembayaran faktur dan pengeluaran (`financePayment.race.test.js`, `financeExpense.payment.race.test.js`).
      - Pengujian pemulihan dari crash (*crash recovery*) pada gateway pembayaran dan ledger ops.
      - Pengujian sanitasi Data Table (`dataTable.test.js`) dan kunci atomik DB Tools (`dbTools.service.test.js`).
      - Pengujian lengkap endpoint Partner API (login, logout, customer CRUD, documents, radius profile, network device).

---

## 🌿 Branch: `issue-175` — Integrasi TR-069 GenieACS & Manajemen Perangkat ONT / OLT (Halaman ACS Devices, Drawer Diagnostik & Parameter ONT, Tautan Pelanggan Broadband, dan Emulator Mock ONT TR-069)

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: Integrasi TR-069 GenieACS & Manajemen Perangkat ONT / OLT (Halaman ACS Devices, Drawer Diagnostik & Parameter ONT, Tautan Pelanggan Broadband, dan Emulator Mock ONT TR-069)
- **Status Branch**: `Belum di-merge` (Branch aktif dalam pengembangan di workspace lokal)

---

### 📅 Rincian Commit

#### [`9a8a28b`](https://github.com/user/repo/commit/9a8a28b) - save #175 - 6 September 2026, 01:22:36 WIB

- **Komponen yang Berubah**:
  - **ACS Module & Tooling Simulasi CPE**:
    - [`acs/scripts/mockOnt.js`](acs/scripts/mockOnt.js) [NEW] — Emulator / simulator komprehensif ONT TR-069 CPE berbasis HTTP CWMP:
      - Mensimulasikan siklus hidup perangkat ONT fisik lengkap (Huawei, ZTE, VSOL) yang berkomunikasi dua arah dengan server GenieACS NBI/CWMP (:7547).
      - Mengirimkan pesan `Inform` berkala (interval 60 detik) dengan parameter TR-098 / TR-181 (Serial Number, Manufacturer, ProductClass, OUI, Hardware/Software Version, WAN IP, PPPoE credentials, WiFi SSID & Password).
      - Mensimulasikan pembacaan telemetry optik PON dinamis: RX Optical Power (-19.5 dBm), TX Optical Power (2.4 dBm), Voltage (3.3V), dan Temperature (42.0°C).
      - Menyediakan server Connection Request lokal (:7549) dan merespons RPC standar GenieACS: `GetParameterValues`, `SetParameterValues`, `Reboot`, dan `FactoryReset`.
    - [`acs/package.json`](acs/package.json) & [`package.json`](package.json) — Penambahan script npm `npm run mock:ont` untuk mempermudah eksekusi emulator perangkat ONT saat pengembangan dan pengujian lokal.

#### [`90bc1aa`](https://github.com/user/repo/commit/90bc1aa) - save #175 - 6 September 2026, 00:53:37 WIB

- **Komponen yang Berubah**:
  - **Backend Core — Resiliensi Komunikasi GenieACS**:
    - [`backend/src/services/acs.service.js`](backend/src/services/acs.service.js) — Penanganan graceful fallback pada `findAcsDevicesForTable`:
      - Mendeteksi error kegagalan koneksi jaringan ke GenieACS NBI (`ECONNREFUSED`, `ENOTFOUND`, `ECONNRESET`).
      - Menghindari pelemparan uncaught error 500 saat daemon GenieACS sedang offline/belum berjalan, dengan mencatat log peringatan Winston dan mengembalikan metadata `{ list: [], totalDocs: 0, acs_offline: true }` agar UI tetap dapat merender halaman secara elegan.

#### [`7812cf2`](https://github.com/user/repo/commit/7812cf2) - save #175 - 6 September 2026, 00:51:01 WIB

- **Komponen yang Berubah**:
  - **Frontend — Optimasi Reaktivitas & Standarisasi UI**:
    - [`frontend/src/app/pages/network/acsDevices/index.jsx`](frontend/src/app/pages/network/acsDevices/index.jsx) — Penyesuaian tombol muat ulang menggunakan key terjemahan standar `global.refresh`.
    - [`frontend/src/app/pages/services/broadband/detail.jsx`](frontend/src/app/pages/services/broadband/detail.jsx) — Penggantian key dinamis tidak stabil (`randomId()`) pada baris tabel dan item linimasa riwayat dengan key deterministik (`_id` / `index`), serta penambahan guard pengecekan fungsi `toggleSeries` pada grafik traffic ApexCharts saat inisialisasi.
    - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) — Pendaftaran kamus terjemahan untuk tombol refresh global.

#### [`e0516f9`](https://github.com/user/repo/commit/e0516f9) - save #175 - 6 September 2026, 00:45:31 WIB

- **Komponen yang Berubah**:
  - **Backend Core — API Perangkat TR-069 ACS**:
    - [`backend/src/controllers/acs.controller.js`](backend/src/controllers/acs.controller.js) & [`backend/src/routes/acs.route.js`](backend/src/routes/acs.route.js) — Penyediaan endpoint REST baru untuk manajemen perangkat ACS:
      - `GET /api/v1/acs/devices/table`: Mengambil data tabel paginasi perangkat ONT dari GenieACS dengan metadata sinyal optik dan tautan pelanggan.
      - `POST /api/v1/acs/devices/:deviceId/link-customer`: Menautkan perangkat ONT CPE ke akun langganan broadband pelanggan.
      - `POST /api/v1/acs/devices/:deviceId/unlink-customer`: Memutuskan relasi perangkat ONT dari akun pelanggan.
    - [`backend/src/services/acs.service.js`](backend/src/services/acs.service.js) — Implementasi fungsi `findAcsDevicesForTable`, `linkDeviceToCustomer`, dan `unlinkDeviceFromCustomer` dengan validasi kepemilikan dan penyimpanan referensi silang ke database MongoDB.
  - **Frontend — Halaman Manajemen ACS & Drawer Diagnostik ONT**:
    - [`frontend/src/app/navigation/networks.js`](frontend/src/app/navigation/networks.js) — Pendaftaran menu baru **ACS Devices** (`/networks/acs-devices`) di bawah grup navigasi Jaringan, dilindungi hak akses `acsDevice.read`.
    - [`frontend/src/app/router/network/acsDeviceRoute.jsx`](frontend/src/app/router/network/acsDeviceRoute.jsx) [NEW] & [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx) — Konfigurasi lazy-loading rute proteksi untuk halaman ACS Devices.
    - [`frontend/src/app/pages/network/acsDevices/index.jsx`](frontend/src/app/pages/network/acsDevices/index.jsx) [NEW] — Halaman utama pengelolaan perangkat ONT ACS:
      - Ringkasan KPI perangkat (Total CPE, Online, Offline, Sinyal Kritis).
      - Tabel TanStack dengan filter pencarian Serial Number, Model, Vendor, dan status sinkronisasi.
      - Tombol aksi cepat untuk membuka detail diagnostik, menautkan pelanggan, serta restart ONT.
    - [`frontend/src/app/pages/network/acsDevices/components/OntAcsDetailDrawer.jsx`](frontend/src/app/pages/network/acsDevices/components/OntAcsDetailDrawer.jsx) [NEW] — Drawer diagnostik komprehensif perangkat ONT:
      - Menampilkan metrik optik PON live (RX Power, TX Power, Voltase, Suhu).
      - Menampilkan konfigurasi WAN & koneksi PPPoE aktif.
      - Menampilkan konfigurasi WLAN/WiFi (SSID, status radio, enkripsi, dan jumlah client terhubung).
      - Menampilkan daftar perangkat lokal (LAN Host Table) yang terhubung ke port Ethernet & WiFi.
      - Aksi manajemen jarak jauh: Reboot ONT, Factory Reset, dan Re-fetch parameter GenieACS.
    - [`frontend/src/app/pages/network/acsDevices/components/LinkCustomerModal.jsx`](frontend/src/app/pages/network/acsDevices/components/LinkCustomerModal.jsx) [NEW] — Modal interaktif penautan perangkat ONT ke pelanggan broadband dengan fitur pencarian live pelanggan.
    - [`frontend/src/app/pages/network/acsDevices/components/AcsRowActions.jsx`](frontend/src/app/pages/network/acsDevices/components/AcsRowActions.jsx) [NEW] — Dropdown menu aksi pada baris tabel (Detail, Tautkan Pelanggan, Lepas Tautan, Reboot).
    - [`frontend/src/app/pages/network/acsDevices/schema/columns.jsx`](frontend/src/app/pages/network/acsDevices/schema/columns.jsx) [NEW] — Definisi kolom TanStack Table dengan formatting badge status online/offline, badge kekuatan sinyal optik (dBm), dan tautan profil pelanggan.
    - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx) — Penambahan komponen cell renderer pembantu untuk data tabel ACS.
    - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) — Definisi kamus terjemahan bilingual modul ACS Devices.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #275  | Penguatan Keamanan, Stabilitas, & Audit Menyeluruh Sistem | Penutupan celah keamanan sesi access token revokasi seketika, proteksi *path traversal* dan balapan atomik DB Tools, sanitasi *data table*, serta audit stabilitas menyeluruh modul keuangan dengan 131 berkas uji otomatis. |
| #175  | Integrasi TR-069 GenieACS & Manajemen Perangkat ONT / OLT | Penyediaan antarmuka terpadu pemantauan dan diagnostik perangkat ONT pelanggan secara *real-time* via protokol TR-069 GenieACS, penautan ONT ke data broadband, dan tool simulasi CPE mock. |

### Kemampuan Baru Pengguna/Admin

- **Pencabutan Sesi Seketika (Instant Session Revocation)**: Admin atau pengguna yang melakukan *logout* atau aksi "keluar dari semua perangkat" langsung tidak dapat lagi menggunakan access token yang sudah beredar; sistem menolak token seketika lewat verifikasi `assertSessionActive`.
- **Manajemen Jarak Jauh ONT Pelanggan**: Admin dan teknisi jaringan kini dapat melihat status operasional ONT pelanggan langsung dari dashboard (status online/offline, redaman optik RX/TX, voltase, suhu, WAN IP, PPPoE status, serta client WiFi yang tersambung).
- **Diagnostik & Tindakan Remote CPE**: Admin dapat memicu pembacaan ulang parameter, melakukan *reboot* ONT secara remote dari aplikasi tanpa harus datang ke lokasi pelanggan atau masuk ke web admin ONT secara manual.
- **Penautan Mandiri ONT & Pelanggan**: Admin dapat menghubungkan nomor seri ONT TR-069 yang terdeteksi di jaringan ke akun layanan broadband pelanggan secara interaktif.

### Bug Fix / Solusi Masalah

- **Eliminasi Celah Balapan DB Tools**: Mengatasi celah balapan (*race condition*) pada pemicu pekerjaan database tools (migrasi/backup) dengan partial unique index `active_lock` di MongoDB, sehingga mustahil terjadi eksekusi backup ganda yang dapat membebani server.
- **Penutupan Path Traversal MinIO**: Menutup potensi celah *path traversal* pada nama objek backup MinIO dengan validasi regex ketat `assertValidObjectName`.
- **Pencegahan Kebocoran Data Oracle Data Table**: Menambahkan validasi whitelist `keys.includes(fl.id)` pada filter select dan range di utilitas Data Table sehingga field tersembunyi tidak dapat difilter atau ditebak nilainya.
- **Pencegahan Error 500 Saat GenieACS Offline**: Menambahkan *graceful error handling* pada endpoint tabel perangkat ACS, sehingga jika microservice GenieACS sedang mati atau belum menyala, antarmuka Dekasimal tetap menampilkan tabel kosong dengan indikator ramah tanpa *crash*.
- **Ketahanan Notifikasi Telegram Alert**: Mengisolasi eksekusi pengiriman alert per destinasi agar kegagalan/rate limit di satu chat ID Telegram tidak membatalkan pengiriman ke grup/channel lainnya.

### Menu/Fitur Baru

- **Menu Jaringan > ACS Devices** (`/networks/acs-devices`): Menu baru untuk monitoring seluruh perangkat modem/ONT TR-069 yang terhubung ke sistem GenieACS.
- **Drawer Diagnostik ONT CPE**: Panel rincian lengkap telemetri optik, antarmuka jaringan, port LAN, WLAN, dan perangkat pengguna yang terhubung.
- **Modal Link Customer ONT**: Dialog pencarian dan penautan perangkat ke data pelanggan broadband.
- **Simulator Mock ONT CLI** (`npm run mock:ont`): Utilitas developer untuk mensimulasikan komunikasi ONT TR-069 vendor Huawei/ZTE/VSOL ke GenieACS saat pengujian lokal.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **Modul ACS Devices**: Modul pemantauan perangkat pelanggan (*Customer Premises Equipment* / CPE) berbasis protokol standar TR-069 (CWMP) yang terintegrasi dengan GenieACS NBI. Memungkinkan tim NOC dan teknisi melihat kondisi redaman kabel fiber optik pelanggan, status koneksi PPPoE, nama SSID WiFi, hingga melakukan restart modem secara terpusat.
  - **Pengujian dengan Mock ONT CPE**: Skrip simulator yang dapat dijalankan secara lokal untuk menghasilkan perangkat ONT tiruan lengkap dengan nilai telemetri optik dan respons terhadap perintah reboot / perubahan SSID dari GenieACS.

- **Langkah Penggunaan (Tutorial)**:
  1. **Mengakses Halaman ACS Devices**:
     - Buka menu navigasi utama di sisi kiri, klik grup **Jaringan** lalu pilih menu **ACS Devices**.
     - Halaman akan memuat daftar perangkat ONT yang saat ini terdaftar di GenieACS lengkap dengan Serial Number, vendor, status koneksi, dan level daya optik (RX Power).
  2. **Melihat Diagnostik & Telemetri Optik**:
     - Klik tombol aksi **Detail** (ikon mata) pada salah satu baris perangkat.
     - Drawer kanan akan terbuka menampilkan:
       - **Optical Info**: Nilai RX Power (misal: `-19.5 dBm` dengan indikator warna status sinyal), TX Power, Voltase, dan Temperatur.
       - **WAN Info**: Mode koneksi (PPPoE/DHCP), status koneksi internet, IP WAN, dan durasi aktif (*uptime*).
       - **WLAN Info**: Nama SSID WiFi, status proteksi sandi, dan frekuensi.
       - **Connected Devices**: Daftar smartphone/laptop yang sedang terhubung ke WiFi/LAN modem tersebut.
  3. **Menautkan Perangkat ke Pelanggan Broadband**:
     - Pada baris perangkat yang belum bertuan, klik tombol **Tautkan Pelanggan**.
     - Ketikkan nama pelanggan atau nomor layanan broadband pada kolom pencarian.
     - Pilih pelanggan yang sesuai lalu klik tombol **Simpan Penautan**. Perangkat kini resmi terasosiasi dengan data langganan pelanggan tersebut.
  4. **Melakukan Reboot ONT Jarak Jauh**:
     - Dari drawer detail atau menu dropdown aksi baris tabel, klik opsi **Reboot Perangkat**.
     - Konfirmasi dialog peringatan. Server akan mengirimkan instruksi RPC `Reboot` via TR-069 ke modem pelanggan secara otomatis.
  5. **Menjalankan Simulator Mock ONT (Khusus Pengembang)**:
     - Jalankan perintah berikut di terminal:
       ```bash
       npm run mock:ont
       ```
     - Skrip simulator akan memicu registrasi perangkat ONT virtual ke GenieACS lokal (`http://127.0.0.1:7547/`) dan mengirimkan sinyal Inform secara periodik setiap 60 detik.
