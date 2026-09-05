# 📝 Daily Work Report - Dedy S.N Putra (2026-09-05)

---

## 📅 Laporan Harian - 5 September 2026

---

## 🌿 Branch: `issue-175` — Integrasi OLT Multi-Vendor & ACS TR-069

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: Integrasi OLT Multi-Vendor & ACS TR-069 (GenieACS Auto Configuration Server)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [9ca6e7b] - save #175 - Sabtu, 5 September 2026, 15:27:23 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `acs/.env.example` [NEW]
  - `acs/config/provisions/inform.js` [NEW]
  - `acs/ext/informWebhook.js` [NEW]
  - `acs/ext/package.json` [NEW]
  - `acs/index.js` [NEW]
  - `acs/package.json` [NEW]
  - `acs/package-lock.json` [NEW]
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/acs.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/acs.route.js` [NEW]
  - `backend/src/services/acs.service.js` [NEW]
  - `backend/src/services/acsProfiles/huawei.profile.js` [NEW]
  - `backend/src/services/acsProfiles/registry.js` [NEW]
  - `backend/src/services/acsProfiles/standard.profile.js` [NEW]
  - `backend/src/services/acsProfiles/vsol.profile.js` [NEW]
  - `backend/src/services/acsProfiles/zte.profile.js` [NEW]
  - `frontend/src/app/pages/services/broadband/components/OntAcsCard.jsx` [NEW]
  - `frontend/src/app/pages/services/broadband/detail.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `package.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Inisialisasi Modul ACS (Auto Configuration Server)**: Membangun microservice baru `acs/` di dalam monorepo DEKASIMAL V2 menggunakan engine GenieACS v1.2.x untuk standarisasi protokol TR-069 (CWMP). Modul ini menyediakan layanan CWMP (port 7547), Northbound Interface REST API (port 7557), File Server (port 7567), dan Web GUI (port 7580) terpisah dengan database internal khusus.
  - **Ekstensi Inform Webhook GenieACS**: Membuat script webhook ekstensi (`acs/ext/informWebhook.js`) dan provisi preset TR-069 (`acs/config/provisions/inform.js`) yang secara otomatis meneruskan event `1 BOOT`, `2 PERIODIC`, dan `4 VALUE CHANGE` dari ONT pelanggan ke Backend API via REST internal (`INTERNAL_API_KEY`).
  - **Backend ACS Service (`acs.service.js`)**: Mengembangkan layer servis backend yang terhubung langsung ke GenieACS NBI REST API untuk:
    - Melakukan pemantauan dan query parameter perangkat ONT secara real-time via TR-069 _Connection Request_.
    - Melakukan _Remote Reboot_ perangkat ONT dari jarak jauh tanpa kontak fisik teknisi.
    - Mengelola parameter konfigurasi Wi-Fi pelanggan (SSID, pre-shared key/password WPA2/WPA3, status radio on/off).
    - Melakukan _Zero-Touch Provisioning_ PPPoE push: secara otomatis mengirimkan konfigurasi WAN PPP Connection (username, password, dan VLAN) dari data akun RADIUS pelanggan langsung ke memori ONT.
    - Membaca metrik kualitas sinyal optik PON (RX Power, TX Power, suhu modul PON, voltase, dan status link optik).
  - **Vendor Profile Registry (`acsProfiles/`)**: Mengabstraksi perbedaan path data model TR-069 antar produsen ONT:
    - `standard.profile.js`: Standard Data Model TR-098 (`InternetGatewayDevice.*`) & TR-181 (`Device.*`).
    - `huawei.profile.js`: Pemetaan parameter khusus perangkat ONT seri Huawei EchoLife.
    - `zte.profile.js`: Pemetaan parameter khusus perangkat ONT seri ZTE ZXHN.
    - `vsol.profile.js`: Pemetaan parameter khusus perangkat ONT seri VSOL (V2801, V2804, dsb).
  - **Controller & Routing Backend (`acs.controller.js`, `acs.route.js`)**: Menyediakan REST API lengkap untuk mengambil data perangkat, memperbarui parameter, reboot perangkat, push PPPoE & Wi-Fi, dan penerimaan webhook inform internal, diamankan dengan proteksi middleware privilege (`acs.view`, `acs.edit`, `acs.reboot`, `acs.wifi`).
  - **Komponen UI `OntAcsCard.jsx` pada Detail Pelanggan Broadband**: Mengintegrasikan kartu informasi dan manajemen perangkat ONT di halaman detail layanan pelanggan broadband (`frontend/src/app/pages/services/broadband/detail.jsx`):
    - Status konektivitas CWMP real-time, waktu inform terakhir, manufaktur, model, nomor seri, dan versi firmware ONT.
    - Indikator visual pengukur kualitas sinyal optik (dBm) lengkap dengan badge status evaluasi kualitas redaman.
    - Formulir pengelolaan nama Wi-Fi (SSID) dan kata sandi dengan toggle visibilitas keamanan.
    - Tombol aksi: Reboot ONT, Muat Ulang Parameter (Refresh), dan Sinkronkan Konfigurasi PPPoE dari RADIUS.
  - **Pembaruan Panduan Monorepo**: Mendokumentasikan arsitektur modul ACS, batas port, dan alur komunikasi TR-069 pada panduan baku `AGENTS.md`.

#### [dd5c4ce] - save #175 - Kamis, 3 September 2026, 20:48:38 WIB

- **Komponen yang Berubah**:
  - `backend/package.json`
  - `backend/package-lock.json`
  - `backend/scripts/oltCliCrawler.js` [NEW]
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/oltDevice.controller.js` [NEW]
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/oltDevice.model.js` [NEW]
  - `backend/src/models/radiusAuthentication.model.js`
  - `backend/src/routes/oltDevice.route.js` [NEW]
  - `backend/src/services/oltDevice.service.js` [NEW]
  - `backend/src/services/oltDrivers/cdata.driver.js` [NEW]
  - `backend/src/services/oltDrivers/registry.js` [NEW]
  - `backend/src/services/oltDrivers/vsol.driver.js` [NEW]
  - `documentations/olt-acs-project-recap.md` [NEW]
  - `documentations/olt-cdata-command-tree.json` [NEW]
  - `documentations/olt-cdata-command-tree.md` [NEW]
  - `documentations/olt-cdata-crawl-debug.log` [NEW]
  - `documentations/olt-cdata-reference.md` [NEW]

- **Deskripsi Perubahan & Fungsi**:
  - Fondasi awal manajemen perangkat OLT multi-vendor dengan model database `OltDevice` (menyimpan IP, port, vendor, tipe protokol SSH/Telnet, status koneksi, dan kredensial terenkripsi).
  - Implementasi driver CLI OLT (`cdata.driver.js` dan `vsol.driver.js`) berbasis SSH (`ssh2`) dengan registry driver dinamis.
  - Skrip crawler interaktif command tree OLT CDATA (`oltCliCrawler.js`) beserta dokumentasi referensi pohon perintah CLI OLT CDATA.
  - Penambahan relasi identitas perangkat ONT (`acs.ont_serial`, `acs.ont_vendor`, `acs.ont_model`) pada schema dan kontroler data autentikasi pelanggan RADIUS.

---

## 🌿 Branch: `issue-275` — Penguatan Keamanan (Security Hardening), Audit Perbaikan Celah Sistem, dan Peningkatan Masif Cakupan Pengujian

### 📌 Informasi Issue

- **Nomor Issue**: #275
- **Judul Issue**: Penguatan Keamanan (Security Hardening), Audit Perbaikan Celah Sistem, dan Peningkatan Masif Cakupan Pengujian
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [5c983cf] - save #275 - Sabtu, 5 September 2026, 15:27:09 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/auth.controller.js`
  - `backend/src/controllers/partnerApiCustomer.controller.js`
  - `backend/src/middlewares/auth.middleware.js`
  - `backend/src/models/financeInvoice.model.js`
  - `backend/src/services/dbTools.service.js`
  - `backend/src/services/financeAccount.service.js`
  - `backend/src/services/financeExpense.service.js`
  - `backend/src/services/financeFixedAsset.service.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/radiusProfile.service.js`
  - `backend/src/utils/has-privilege.js`
  - 100+ berkas unit test dan integration test (`backend/test/integration/*`, `backend/test/unit/*`)

- **Deskripsi Perubahan & Fungsi**:
  - **Mekanisme Pencabutan Sesi Seketika (Instant Session Revocation)**:
    - Menambahkan identifikasi sesi `jti` unik ke dalam payload JWT access token yang diterbitkan bersamaan dengan refresh token pada `auth.controller.js`.
    - Mengintegrasikan fungsi validasi sesi aktif `assertSessionActive` ke dalam middleware autentikasi seluruh peran (`protectedAdmin`, `protectedCustomer`, `protectedPartnerApp`, `protectedDeveloper`) pada `auth.middleware.js`.
    - Memastikan access token langsung ditolak dengan status HTTP 401 (`AUTH_ERROR.SESSION_REVOKED`) jika pengguna telah keluar (logout) atau sesinya dicabut oleh administrator, tanpa menunggu waktu kedaluwarsa alami token (`exp`) habis.
  - **Mitigasi Celah NoSQL Injection & Crash Tipe Kredensial Login (CWE-943)**:
    - Menerapkan fungsi filter tipe data `hasUnsafeCredentialTypes` pada endpoint login admin, customer, dan partner app.
    - Menolak input kredensial bertipe object/array (seperti operator query Mongo `{ $gt: '' }`) dan memastikan password bertipe string murni sebelum diproses oleh `bcrypt.compareSync`, mencegah bypass autentikasi maupun runtime unhandled error.
  - **Perlindungan Whitelist & Integritas Data Partner API**:
    - Membuang field `wallet` dan `customer_id` dari payload pendaftaran pelanggan via Partner API (`partnerApiCustomer.controller.js`) untuk menutup celah manipulasi saldo dompet awal secara ilegal atau pembajakan nomor identitas pelanggan.
    - Mengeliminasi field `documents` dari endpoint update JSON biasa, mewajibkan setiap pembaruan dokumen identitas melewati jalur upload multipart resmi dengan validasi berkas fisik yang ketat.
  - **Pencegahan Eskalasi Hak Akses Superadmin**:
    - Memperbaiki logika evaluasi privilege di `has-privilege.js` dengan perbandingan nilai boolean ketat (`req.user?.super === true || req.user?.isSuper === true`). Mencegah nilai string non-kosong (seperti `'0'`) hasil anomali migrasi data lama disalahartikan sebagai kondisi truthy yang memberikan hak superadmin penuh.
  - **Proteksi Celah Akses Faktur Publik Tanpa Autentikasi**:
    - Menambahkan indeks unik `unique: true, sparse: true` pada kolom `payment_code` di schema `FinanceInvoiceSchema`. Memastikan kode pembayaran faktur publik tidak pernah terduplikasi, mencegah kebocoran data faktur milik pelanggan lain saat diakses lewat kode pembayaran publik.
  - **Validasi Anti Path Traversal & Penanganan CastError**:
    - Menerapkan fungsi sanitasi nama berkas `assertValidObjectName` dengan regex ketat pada layanan MinIO di `dbTools.service.js`, mencegah manipulasi path traversal `../../` pada aksi pencatatan, pengunduhan, dan penghapusan arsip backup database.
    - Membungkus pembacaan dokumen berbasis ID/angka non-valid pada Mongoose di berbagai controller/service (`dbTools`, `financeExpense`, `financeInvoice`, `radiusProfile`) agar menghasilkan respons HTTP 400/404 yang bersih alih-alih melempar error 500 internal server error.
  - **Peningkatan Masif Cakupan Pengujian (Test Coverage Suite)**:
    - Menambahkan dan memperbarui lebih dari 100 berkas pengujian otomatis (unit test dan integration test) yang mencakup 17.000+ baris assertions untuk modul keuangan (transaksi berulang, crash recovery pembayaran, race conditions pelunasan, depresiasi aset tetap, rekonsiliasi gateway, aging piutang/hutang), Partner API (CRUD pelanggan, dokumen, profil produk, RADIUS), alur kerja penggajian (payroll), logging, dan utilitas enkripsi.

- **Status Pengerjaan Lanjutan (Working Tree Aktif)**:
  - Validasi tipe data ekstrem pada pembuatan/pengubahan pelanggan Partner API (mencegah error 500 saat `name` atau `phone` dikirim dalam tipe non-string).
  - Penanganan error per-destinasi pada pengiriman alert Telegram (`telegramAlert.js`) agar kegagalan satu grup/chat tidak membungkam pengiriman alert ke destinasi lainnya.
  - Penyesuaian debounce alert rekonsiliasi gateway pembayaran (`financeGateway.service.js`) agar penanda hanya diperbarui jika notifikasi benar-benar berhasil terkirim.

---

## 🌿 Branch: `master` (Issue #268) — Database Tools (DbTools) & Pemulihan Sistem (Recovery Tools): Backup MinIO & Google Drive, Migrasi Database, dan Pemulihan Data

### 📌 Informasi Issue

- **Nomor Issue**: #268
- **Judul Issue**: Database Tools (DbTools) & Pemulihan Sistem (Recovery Tools): Backup MinIO & Google Drive, Migrasi Database, dan Pemulihan Data
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [2dcf11f] - resolve #268 - Sabtu, 5 September 2026, 00:28:49 WIB (Merge Commit)

#### [3c3fbe3] - resolve #268 - Sabtu, 5 September 2026, 00:28:05 WIB

- **Komponen yang Berubah**:
  - `.env.production.example`
  - `backend/.env.example`
  - `backend/Dockerfile`
  - `backend/package.json` & `backend/package-lock.json`
  - `backend/src/app.js`
  - `backend/src/server.js`
  - `backend/src/jobs/dbToolsWorker.js` [NEW]
  - `backend/src/models/dbToolsJob.model.js` [NEW]
  - `backend/src/controllers/dbTools.controller.js` [NEW]
  - `backend/src/routes/dbTools.route.js` [NEW]
  - `backend/src/services/dbTools.service.js` [NEW]
  - `backend/src/services/dbToolsQueue.service.js` [NEW]
  - `backend/src/services/recovery.service.js`
  - `backend/src/utils/crypto.util.js` [NEW]
  - `backend/src/utils/googleDrive.util.js` [NEW]
  - `backend/src/utils/minio.js`
  - `backend/src/utils/mongoTools.util.js` [NEW]
  - 14 berkas controller & route modul backend (admin, business, customer, financeAccount, financeWallet, networkIPv4, payrollRun, productBroadband, productDataAccess, radiusAuthentication, radiusProfile, registration, warehouseRequest, warehouseType)
  - `backend/test/integration/dbTools.service.test.js` [NEW]
  - `backend/test/unit/cryptoUtil.test.js` [NEW]
  - `backend/test/unit/mongoTools.util.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/Developer.jsx`
  - `frontend/src/app/pages/settings/sections/developer/OtherTab.jsx`
  - `frontend/src/app/pages/settings/sections/developer/dbTools/DbToolsTab.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/dbTools/BackupNowModal.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/dbTools/BackupScheduleForm.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectModal.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectionCard.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/dbTools/JobProgressPanel.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/dbTools/MigrationConfirmModal.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/dbTools/MigrationForm.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/dbTools/MinioBackupPanel.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/RecoveryToolsTab.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/RecoveryRouteRow.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/RecoveryDryRunModal.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json` & `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Sistem Backup Database Asinkron & Terjadwal (`dbToolsWorker.js`)**:
    - Mengembangkan sistem backup database berbasis queue (BullMQ + Redis) dan streaming native MongoDB (`mongodump`/`mongorestore`).
    - Mendukung dua destinasi penyimpanan: MinIO Object Storage terenkripsi (dengan fitur versioning, unduhan berkas arsip, dan penghapusan versi) dan Google Drive via API v3 (dengan integrasi OAuth2 PKCE / Service Account).
    - Fitur penjadwalan backup otomatis (harian, mingguan, bulanan) dengan pengaturan batas retensi dokumen lama.
  - **Live Database Migration**:
    - Fasilitas kloning dan migrasi data langsung antar server MongoDB (misal migrasi staging ke production atau upgrade server) langsung melalui antarmuka web.
    - Dilengkapi modal konfirmasi keamanan ganda (`MigrationConfirmModal.jsx`) dan penyiapan koneksi terisolasi untuk menjamin integritas data.
    - Pelacakan progres pekerjaan migrasi dan backup secara real-time melalui Socket.io (`JobProgressPanel.jsx`), menampilkan persentase progres, kecepatan transfer, dan log aktivitas langsung.
  - **Tab Alat Pemulihan (Recovery Tools) & Mode Simulasi (Dry Run)**:
    - Menambahkan tab baru `RecoveryToolsTab` di menu Pengaturan Pengembang untuk melakukan perbaikan otomatis data bermasalah, menyinkronkan counter yang melenceng, dan membersihkan inkonsistensi relasi dokumen pada 14 modul sistem.
    - Setiap proses pemulihan dilengkapi **Mode Dry-Run** (`RecoveryDryRunModal.jsx`) untuk mempratinjau entitas apa saja yang akan dimodifikasi sebelum perubahan permanen diterapkan ke basis data produksi.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                                                          | Dampak Utama                                                                                                                                                                                                                                |
| ----- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #175  | Integrasi OLT Multi-Vendor & ACS TR-069 (GenieACS Auto Configuration Server)                                   | Mengaktifkan remote monitoring dan konfigurasi CPE/ONT pelanggan (Wi-Fi, sinyal optik, reboot) langsung dari detail layanan broadband via TR-069 GenieACS, serta fondasi zero-touch PPPoE push.                                             |
| #275  | Penguatan Keamanan (Security Hardening), Audit Perbaikan Celah Sistem, dan Peningkatan Masif Cakupan Pengujian | Menutup celah NoSQL injection, menegakkan pencabutan instan access token saat logout, melindungi integritas API Mitra, memperbaiki penanganan CastError menjadi respons 400/404 yang bersih, serta menambahkan 17.000+ baris test coverage. |
| #268  | Database Tools (DbTools) & Pemulihan Sistem (Recovery Tools)                                                   | Menyediakan fitur backup otomatis/manual ke MinIO dan Google Drive, migrasi database online dengan pemantauan progres real-time, serta panel pemulihan data sistem dengan mode Dry-Run.                                                     |

### Kemampuan Baru Pengguna/Admin

- **Manajemen Jarak Jauh ONT Pelanggan**: Admin dan tim teknis dapat memantau kekuatan sinyal optik (RX/TX Power) pelanggan secara langsung, mengubah nama Wi-Fi (SSID) dan kata sandi tanpa datang ke lokasi fisik, merestart ONT pelanggan secara remote, dan menyinkronkan kredensial PPPoE langsung dari halaman detail layanan broadband.
- **Pencadangan & Pemulihan Database Mandiri**: Developer dan superadmin dapat memicu backup database kapan saja, mengatur jadwal backup rutin ke MinIO atau Google Drive, mengunduh arsip snapshot database, serta memindahkan data database antar server secara langsung dengan progress bar real-time.
- **Simulasi Perbaikan Data (Dry Run)**: Admin dapat menjalankan alat pemulihan data bermasalah dengan rasa aman berkat mode Dry Run yang menampilkan daftar data terdampak sebelum aksi permanen dijalankan.
- **Keamanan Sesi Lebih Ketat**: Sesi pengguna yang sudah keluar (logout) atau dinonaktifkan administrator akan langsung terputus seketika pada seluruh permintaan API tanpa menunggu token kedaluwarsa.

### Bug Fix / Solusi Masalah

- **Celah Sesi Zombie JWT**: Memperbaiki kelemahan JWT access token lama yang tetap bisa dipakai melakukan request selama masa berlaku token masih aktif meski pengguna sudah menekan tombol logout.
- **Celah NoSQL Injection & Crash Login**: Mencegah serangan bypass login via objek MongoDB dan menghilangkan error 500 tak terduga saat parameter username/password dikirim dalam tipe non-string.
- **Pencegahan Kebocoran Faktur Publik**: Memperbaiki risiko duplikasi kode pembayaran pada endpoint publik faktur melalui penegakan indeks unik `payment_code`.
- **Celah Path Traversal MinIO**: Menutup potensi eksploitasi traversal `../../` pada fungsi manajemen berkas backup MinIO dengan validasi regex nama objek yang ketat.
- **Penanganan CastError Mongoose**: Menghilangkan respons error 500 mentah saat parameter ID dikirim dengan format tidak valid, digantikan dengan status HTTP 400/404 yang informatif sesuai standar REST API.
- **Isolasi Alert Telegram**: Memperbaiki pengiriman alert Telegram agar kegagalan pada satu grup obrolan (misal akibat limitasi rate) tidak menghentikan pengiriman alert ke grup atau saluran lainnya.

### Menu/Fitur Baru

- **Kartu Kontrol ONT ACS TR-069**: Terletak di menu _Layanan Internet Broadband_ > _Detail Pelanggan_ > _Tab Perangkat ONT_.
- **Tab Database Tools (DbTools)**: Terletak di menu _Pengaturan_ > _Pengembang_ > _Tab Database Tools_.
- **Tab Alat Pemulihan (Recovery Tools)**: Terletak di menu _Pengaturan_ > _Pengembang_ > _Tab Alat Pemulihan_.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Modul Kontrol ONT ACS TR-069 pada Layanan Broadband

- **Penjelasan Fitur**:
  Modul ini menghubungkan sistem DEKASIMAL V2 dengan server GenieACS TR-069 via Northbound Interface REST API. Melalui protokol TR-069 CWMP, sistem dapat berkomunikasi dua arah dengan ONT di rumah pelanggan (mendukung vendor Huawei, ZTE, VSOL, dan ONT TR-098/TR-181 standar) tanpa memerlukan alamat IP publik di sisi ONT pelanggan.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Layanan** > **Broadband** pada panel admin.
  2. Pilih salah satu pelanggan yang sudah memiliki nomor seri ONT terdaftar, lalu buka halaman **Detail Layanan**.
  3. Gulir ke bagian kartu **Kontrol & Status ONT (ACS TR-069)**:
     - Periksa **Status Koneksi CWMP** dan **Waktu Kontak Terakhir**.
     - Perhatikan meteran **Kekuatan Sinyal Optik (RX Power)**: indikator hijau menunjukkan sinyal ideal (-15 dBm s/d -24 dBm), kuning sinyal pas-pasan, dan merah sinyal drop/kritis.
  4. Untuk mengubah Wi-Fi pelanggan:
     - Masukkan nama Wi-Fi baru pada kolom **Nama Wi-Fi (SSID)**.
     - Masukkan password baru pada kolom **Kata Sandi Wi-Fi**.
     - Klik tombol **Simpan Konfigurasi Wi-Fi**. Perangkat ONT pelanggan akan menerima konfigurasi baru secara otomatis dalam beberapa detik.
  5. Untuk merestart perangkat ONT pelanggan saat terjadi gangguan koneksi, klik tombol **Reboot ONT** dan konfirmasikan aksi.

### 2. Database Tools & Pemulihan Sistem (DbTools & Recovery Tools)

- **Penjelasan Fitur**:
  Menyediakan utilitas terpadu bagi administrator untuk mencadangkan database secara asinkron ke MinIO Object Storage atau Google Drive, menjalankan migrasi database real-time antar server, serta memulihkan relasi dan anomali data sistem dengan fitur simulasi Dry Run.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Pengaturan** > **Pengembang**.
  2. Pilih tab **Database Tools**:
     - Untuk melakukan backup instan, klik tombol **Backup Sekarang**, pilih target penyimpanan (MinIO atau Google Drive), dan klik **Mulai Backup**. Progres backup dapat dipantau langsung melalui panel progres real-time.
     - Untuk mengatur jadwal rutin, buka kartu **Jadwal Backup Otomatis**, tentukan interval (misal: Setiap Hari pukul 02:00 WIB), lalu klik **Simpan Jadwal**.
     - Untuk melihat dan mengunduh berkas backup, gunakan kartu **Daftar Arsip Backup MinIO**.
  3. Pilih tab **Alat Pemulihan (Recovery Tools)**:
     - Cari modul data yang ingin diperbaiki atau dinormalkan (misal: _Sinkronisasi Saldo Dompet & Jurnal Faktur_).
     - Klik tombol **Simulasi (Dry Run)** terlebih dahulu untuk melihat daftar dokumen yang akan diperbaiki.
     - Setelah memeriksa ringkasan simulasi dan memastikan data sesuai, klik tombol **Jalankan Pemulihan** untuk menerapkan perubahan data secara permanen.
