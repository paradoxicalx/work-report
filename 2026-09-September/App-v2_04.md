# 📝 Daily Work Report - Dedy (2026-09-04)

---

## 📅 Laporan Harian - 4 September 2026

---

## 🌿 Branch: `issue-268` — Database Tools (DB Tools): Migrasi Antar-Server MongoDB, Backup & Restore MinIO Versioning, Penjadwalan Backup Otomatis, dan Integrasi Google Drive OAuth

### 📌 Informasi Issue

- **Nomor Issue**: #268
- **Judul Issue**: Database Tools (DB Tools): Migrasi Antar-Server MongoDB, Backup & Restore MinIO Versioning, Penjadwalan Backup Otomatis, dan Integrasi Google Drive OAuth
- **Status Branch**: `Belum di-merge` (Branch aktif dalam pengembangan di workspace utama)

---

### ⏳ Pekerjaan Belum Di-commit (Working Tree Changes)

- **Komponen yang Berubah**:
  - **Backend Core — Layanan Enkripsi, Utilitas Google Drive & Jadwal Backup**:
    - [`backend/src/utils/crypto.util.js`](backend/src/utils/crypto.util.js) [NEW] — Enkripsi simetris AES-256-GCM (`encryptSecret` & `decryptSecret`) untuk kredensial sensitif pihak ketiga (`gdrive_refresh_token`) dengan IV 96-bit dan autentikasi tag HMAC, menggunakan kunci 32-byte dari environment `GDRIVE_TOKEN_ENCRYPTION_KEY`.
    - [`backend/src/utils/googleDrive.util.js`](backend/src/utils/googleDrive.util.js) [NEW] — Integrasi Google Drive API v3 berbasis OAuth 2.0 (scope `drive.file`):
      - Pembangun OAuth client (`buildOAuthClient`) dari konfigurasi sistem.
      - Penghasil URL consent Google (`getAuthUrl`) dengan parameter `access_type: 'offline'` dan `prompt: 'consent'` untuk menjamin penerimaan `refresh_token`.
      - Penukar authorization code menjadi token dan penyimpanan refresh token terenkripsi ke database (`exchangeCodeForTokens`).
      - Pengunggah berkas backup lokal ke My Drive (`uploadFileToDrive`) secara streaming.
      - Rotasi retensi berkas backup lama (`deleteFileFromDrive`).
      - Pemutusan koneksi Google Drive dan pencabutan token (`revokeDriveConnection`).
    - [`backend/src/utils/minio.js`](backend/src/utils/minio.js) — Penambahan fungsi `minioRemoveObjectVersion` untuk menghapus versi spesifik objek secara permanen dari bucket ber-versioning, serta refaktor `minioListAllObjectVersions` dan `minioListObjectVersions` untuk mendukung pengelompokan berkas backup dan versi individual.
    - [`backend/src/services/dbTools.service.js`](backend/src/services/dbTools.service.js) — Penambahan penanganan nama berkas backup kustom (`customFilename`), logika rotasi retensi versi lama, integrasi target Google Drive pada backup instan/otomatis, serta parser ekspresi cron jadwal backup (`buildBackupCronPattern`).
    - [`backend/src/services/dbToolsQueue.service.js`](backend/src/services/dbToolsQueue.service.js) — Integrasi BullMQ Job Scheduler API (`upsertBackupSchedule` & `removeBackupSchedule`) untuk mengelola jadwal backup otomatis database tanpa ketergantungan pada repeatable jobs lama.
    - [`backend/src/jobs/dbToolsWorker.js`](backend/src/jobs/dbToolsWorker.js) — Penanganan job `scheduled_minio_backup` dengan dukungan multi-destinasi (MinIO dan Google Drive) dan pelaporan progress real-time ke model job.
    - [`backend/src/services/option.service.js`](backend/src/services/option.service.js) — Pendaftaran key konfigurasi non-sensitif (`db_backup_schedule`, `gdrive_oauth_client_id`, `gdrive_connected_email`) dan sensitif (`gdrive_oauth_client_secret`, `gdrive_refresh_token`).
    - [`backend/src/controllers/dbTools.controller.js`](backend/src/controllers/dbTools.controller.js) & [`backend/src/routes/dbTools.route.js`](backend/src/routes/dbTools.route.js) — Endpoint REST baru:
      - `GET /api/v1/db-tools/objects` & `GET /api/v1/db-tools/objects/:name/versions`: Mengambil daftar objek backup dan riwayat versinya.
      - `DELETE /api/v1/db-tools/versions`: Menghapus satu versi spesifik berkas backup di MinIO.
      - `GET /api/v1/db-tools/schedule` & `PUT /api/v1/db-tools/schedule`: Mengambil dan memperbarui jadwal backup otomatis (harian, mingguan, bulanan).
      - `POST /api/v1/db-tools/gdrive/auth-url`: Menghasilkan URL login consent OAuth Google.
      - `POST /api/v1/db-tools/gdrive/disconnect`: Memutuskan koneksi Google Drive terhubung.
    - [`backend/package.json`](backend/package.json) & [`backend/package-lock.json`](backend/package-lock.json) — Penambahan dependensi resmi `googleapis` (v144+).
    - [`.env.production.example`](.env.production.example) & [`backend/.env.example`](backend/.env.example) — Dokumentasi variabel environment baru `GDRIVE_TOKEN_ENCRYPTION_KEY`.
    - Unit & Integration Tests:
      - [`backend/test/unit/cryptoUtil.test.js`](backend/test/unit/cryptoUtil.test.js) [NEW] — Validasi enkripsi dan dekripsi round-trip AES-256-GCM, verifikasi penolakan key yang ukurannya tidak valid, dan pengecekan deteksi manipulasi ciphertext.
      - [`backend/test/integration/dbTools.service.test.js`](backend/test/integration/dbTools.service.test.js) — Penambahan suite pengujian integrasi untuk jadwal backup otomatis, multi-destinasi Google Drive, dan penghapusan versi spesifik.
  - **Frontend — Antarmuka Penjadwal Backup & Integrasi Google Drive**:
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/BackupNowModal.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/BackupNowModal.jsx) [NEW] — Modal pemicu backup instan dengan opsi input nama berkas kustom atau penamaan otomatis.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/BackupScheduleForm.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/BackupScheduleForm.jsx) [NEW] — Formulir pengaturan jadwal backup otomatis (toggle aktif, pilihan frekuensi harian/mingguan/bulanan, pemilihan hari/tanggal, pengaturan jam/menit, dan destinasi MinIO/Google Drive).
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectModal.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectModal.jsx) [NEW] — Modal konfigurasi OAuth Google Drive (input Client ID & Client Secret, panduan redirect URI, serta tombol otorisasi dengan jendela popup).
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectionCard.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectionCard.jsx) [NEW] — Kartu status koneksi Google Drive pada tab Developer Settings, menampilkan email akun terhubung, status sinkronisasi, dan tombol putuskan koneksi.
    - [`frontend/src/app/pages/settings/sections/developer/OtherTab.jsx`](frontend/src/app/pages/settings/sections/developer/OtherTab.jsx) — Penempatan komponen `GdriveConnectionCard` pada tab Pengaturan Lainnya.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/MinioBackupPanel.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/MinioBackupPanel.jsx) — Pembaruan panel backup MinIO dengan tabel daftar objek backup, ekspansi riwayat versi per berkas, aksi hapus versi permanen, tombol pemicu `BackupNowModal`, dan panel formulir jadwal backup.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/JobProgressPanel.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/JobProgressPanel.jsx) — Dukungan penayangan job terjadwal otomatis dan progress upload Google Drive.
    - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) — Penambahan kamus terjemahan untuk formulir jadwal backup, modal nama backup, status koneksi Google Drive, dan konfirmasi hapus versi.

---

### 📅 Rincian Commit

#### [`bc04f38`](https://github.com/user/repo/commit/bc04f38) - resolve #268 - 4 September 2026, 19:26:53 WIB

- **Komponen yang Berubah**:
  - **Backend Core & Docker Containerization**:
    - [`backend/Dockerfile`](backend/Dockerfile) — Pemasangan paket `mongodb-tools` (`mongodump` & `mongorestore`) pada base image container backend Alpine Linux agar utilitas native database tersedia di runtime production.
    - [`backend/src/utils/mongoTools.util.js`](backend/src/utils/mongoTools.util.js) [NEW] — Utilitas wrapper eksekusi CLI `mongodump` dan `mongorestore` menggunakan child process Node.js:
      - Sanitasi argumen dan pengaburan kredensial connection string pada log eksekusi (*zero secrets exposure*).
      - Dukungan pipeline stream terkompresi gzip (`--gzip` & `--archive`) untuk meminimalkan beban I/O disk lokal.
      - Parsing output stderr untuk mendeteksi error koneksi, autentikasi, dan status dumping/restoring.
    - [`backend/src/models/dbToolsJob.model.js`](backend/src/models/dbToolsJob.model.js) [NEW] — Skema Mongoose pelacak lifecycle pekerjaan database:
      - Field `type`: `migrate`, `minio_backup`, `minio_restore`, `scheduled_minio_backup`.
      - Field `status`: `pending`, `running`, `success`, `failed`.
      - Field detail: `progress` (0-100%), `current_step`, `error_message`, `source_masked`, `destination_masked`, `backup_size`, `duration_ms`, `admin_id`.
    - [`backend/src/services/dbTools.service.js`](backend/src/services/dbTools.service.js) [NEW] — Logika bisnis migrasi database antar server dan backup/restore MinIO:
      - Migrasi Antar-Server: Validasi konektivitas MongoDB sumber dan tujuan, pembersihan data lama saat mode `drop`, dan stream data langsung antar host.
      - Backup MinIO: Dump database aktif ke archive lokal sementara, upload ke bucket MinIO dengan metadata terindeks, dan pembersihan file temporer saat selesai/gagal.
      - Restore MinIO: Streaming download versi backup dari MinIO ke file temporer, verifikasi integritas archive, dan eksekusi `mongorestore` ke database aktif.
    - [`backend/src/services/dbToolsQueue.service.js`](backend/src/services/dbToolsQueue.service.js) [NEW] & [`backend/src/jobs/dbToolsWorker.js`](backend/src/jobs/dbToolsWorker.js) [NEW] — Antrean BullMQ terisolasi `db-tools-queue` dengan kontrol concurrency 1 job pada satu waktu guna mencegah kelebihan beban database dan memory spikes.
    - [`backend/src/controllers/dbTools.controller.js`](backend/src/controllers/dbTools.controller.js) [NEW] & [`backend/src/routes/dbTools.route.js`](backend/src/routes/dbTools.route.js) [NEW] — Rute REST API `/api/v1/db-tools` yang dilindungi privilege superadmin (`settings.read` & `settings.update`):
      - `POST /migrate`: Pendaftaran job migrasi database antar instance.
      - `POST /backup/minio`: Pendaftaran job backup manual ke MinIO.
      - `POST /restore/minio`: Pendaftaran job pemulihan data dari versi backup tertentu.
      - `GET /versions`: Mengambil riwayat versi backup yang tersimpan di bucket MinIO.
      - `GET /jobs/active`: Memantau status dan progress job yang sedang berjalan secara live.
      - `GET /jobs/history`: Mengambil riwayat log pekerjaan backup dan migrasi terdahulu.
    - [`backend/src/app.js`](backend/src/app.js) & [`backend/src/server.js`](backend/src/server.js) — Registrasi routing dan inisialisasi worker `dbToolsWorker` saat startup server.
    - Unit & Integration Tests:
      - [`backend/test/unit/mongoTools.util.test.js`](backend/test/unit/mongoTools.util.test.js) [NEW] — Pengujian parsing URI MongoDB, masking kredensial, dan pembuatan argumen CLI `mongodump`/`mongorestore`.
      - [`backend/test/integration/dbTools.service.test.js`](backend/test/integration/dbTools.service.test.js) [NEW] — Pengujian alur pembuatan job, queuing BullMQ, dan penanganan status error.
  - **Frontend — Antarmuka Pengaturan Database Tools**:
    - [`frontend/src/app/pages/settings/sections/Developer.jsx`](frontend/src/app/pages/settings/sections/Developer.jsx) — Penambahan tab navigasi baru **Database Tools** (`db-tools`) lengkap dengan ikon `ServerStackIcon`.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/DbToolsTab.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/DbToolsTab.jsx) [NEW] — Halaman induk penyedia antarmuka Database Tools yang menyatukan formulir migrasi, panel backup MinIO, dan panel progress job aktif.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/MigrationForm.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/MigrationForm.jsx) [NEW] — Formulir konfigurasi koneksi sumber dan tujuan:
      - Opsi toggle "Gunakan database yang sedang aktif" untuk sisi sumber atau tujuan.
      - Input kredensial host, port, username, password, dan nama database tujuan.
      - Pilihan mode restore: `Drop & Replace` (kosongkan data sebelum restore) atau `Merge` (gabungkan data tanpa menghapus dokumen lama).
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/MigrationConfirmModal.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/MigrationConfirmModal.jsx) [NEW] — Modal proteksi aksi destruktif: mengharuskan admin mengetik ulang nama database tujuan secara persis dan memasukkan kembali password login untuk mencegah kesalahan operasional.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/MinioBackupPanel.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/MinioBackupPanel.jsx) [NEW] — Panel tabel riwayat versi backup MinIO yang menampilkan Version ID, ukuran berkas, tanggal pembuatan, status latest, serta tombol aksi Restore.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/JobProgressPanel.jsx`](frontend/src/app/pages/settings/sections/developer/dbTools/JobProgressPanel.jsx) [NEW] — Panel pemantauan status job real-time (polling interval 2 detik) dengan visualisasi progress bar, tahapan saat ini, badge status, dan catatan error bila terjadi kegagalan.
    - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) — Definisi kamus terjemahan bilingual modul Database Tools.

---

## 🌿 Branch: `issue-270` — Pencegahan Freeze Job Scheduler Cron-Worker & Robust Timeout Koneksi TCP Mesin Absensi ZKTeco

### 📌 Informasi Issue

- **Nomor Issue**: #270
- **Judul Issue**: Pencegahan Freeze Job Scheduler Cron-Worker (Watchdog Healthcheck & Docker Auto-Restart) dan Robust Timeout Koneksi TCP Mesin Absensi ZKTeco
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `66c09e0`)

---

### 📅 Rincian Commit

#### [`619711f`](https://github.com/user/repo/commit/619711f) - resolve #270 - 4 September 2026, 17:49:09 WIB
#### [`66c09e0`](https://github.com/user/repo/commit/66c09e0) - Merge branch 'issue-270' into master - 4 September 2026, 17:49:46 WIB

- **Komponen yang Berubah**:
  - **Backend Core — Robust Timeout Koneksi Perangkat Absensi**:
    - [`backend/src/lib/attendanceDeviceClient.js`](backend/src/lib/attendanceDeviceClient.js) — Implementasi fungsi pembungkus `connectWithTimeout`:
      - Mengatasi bug pada pustaka pihak ketiga `zkteco-js` di mana panggilan `socket.setTimeout()` tidak mendaftarkan event listener `timeout`, sehingga saat mesin absen offline/tidak membalas paket TCP SYN, proses koneksi menggantung tanpa batas waktu (*infinite hang*).
      - Menambahkan timer timeout internal yang secara paksa memutus dan menghancurkan socket mentah (`device.ztcp?.socket?.destroy()`) jika koneksi belum terbentuk dalam batas waktu `DEFAULT_TIMEOUT`.
      - Mencegah kebocoran file descriptor dan memastikan perulangan sinkronisasi absensi sekuensial segera beralih ke mesin berikutnya tanpa membekukan seluruh proses backend.
  - **Cron Worker — Watchdog Monitoring, Health Check, dan Error Handling**:
    - [`cron-worker/src/jobs/watchdog.js`](cron-worker/src/jobs/watchdog.js) [NEW] — Modul pengawas kesehatan job worker BullMQ:
      - Melacak timestamp mulai berjalan (`runningSince`), timestamp terakhir selesai (`lastCompletedAt`), dan status gagal (`lastFailedAt`) per jenis job.
      - Menetapkan batas toleransi job macet `STUCK_THRESHOLD_MS` (10 menit).
      - Timer pencatat log pengawas otomatis (`startWatchdogLogger`) yang mendeteksi dan mencatat error ke Winston jika terdapat job yang berjalan melebihi ambang batas.
    - [`cron-worker/src/jobs/worker.js`](cron-worker/src/jobs/worker.js) — Menghubungkan lifecycle event worker BullMQ (`active`, `completed`, `failed`, `stalled`) ke modul watchdog, serta menambahkan logging terstruktur saat job terdeteksi `stalled`.
    - [`cron-worker/src/app.js`](cron-worker/src/app.js) — Pembaruan endpoint `GET /health` untuk mengembalikan status HTTP 503 Service Unavailable dan daftar nama job yang bermasalah saat watchdog mendeteksi job macet, sehingga orkestrator container dapat melakukan restart otomatis.
    - [`cron-worker/Dockerfile`](cron-worker/Dockerfile) — Penambahan instruksi `HEALTHCHECK` Docker bawaan yang mem-probe `http://localhost:4100/health` setiap 30 detik untuk memicu auto-heal/restart oleh orkestrator (Coolify / Docker Swarm).
    - [`cron-worker/src/jobs/scheduler.js`](cron-worker/src/jobs/scheduler.js) — Penambahan opsi deduplikasi antrean `deduplication: { id: 'attendanceDeviceSync', keepLastIfActive: true }` pada job sinkronisasi absensi agar panggilan jadwal baru tidak menumpuk di antrean Redis saat komunikasi backend sedang mengalami beban tinggi.
    - [`cron-worker/src/server.js`](cron-worker/src/server.js) — Pemasangan handler process-level `unhandledRejection` dan `uncaughtException` yang dicatat ke logger Winston terstruktur untuk mencegah matinya proses Node.js scheduler secara diam-diam.

---

## 🌿 Branch: `issue-213` — Integrasi Multi-Akun WhatsApp Baileys & Routing Percakapan Customer Service Multi-Channel

### 📌 Informasi Issue

- **Nomor Issue**: #213
- **Judul Issue**: Integrasi Multi-Akun WhatsApp Baileys (Microservice `baileys-api`, Manajemen Kredensial Multi-Device WhatsApp, QR Pairing & Anti-Ban Mechanism, Shared Encrypted Mongo Auth State, dan Routing Percakapan Customer Service Multi-Channel)
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `ec5b5c1`)

---

### 📅 Rincian Commit

#### [`4b9e9ae`](https://github.com/user/repo/commit/4b9e9ae) - resolve #213 - 4 September 2026, 16:53:34 WIB
#### [`ec5b5c1`](https://github.com/user/repo/commit/ec5b5c1) - Merge branch 'issue-213' into master - 4 September 2026, 17:12:42 WIB

- **Komponen yang Berubah**:
  - **Microservice Baru — `baileys-api` (Port 3050)**:
    - [`baileys-api/package.json`](baileys-api/package.json) [NEW] & [`baileys-api/Dockerfile`](baileys-api/Dockerfile) [NEW] & [`baileys-api/docker-compose.yml`](baileys-api/docker-compose.yml) [NEW] — Inisialisasi microservice WhatsApp mandiri berbasis Express v4, `@whiskeysockets/baileys` v6.7, Mongoose v8, Winston, dan Swagger.
    - [`baileys-api/src/server.js`](baileys-api/src/server.js) [NEW] — Server Express dengan graceful shutdown, middleware logging Winston, dan auto-reconnect semua akun terhubung saat startup.
    - [`baileys-api/src/services/crypto.service.js`](baileys-api/src/services/crypto.service.js) [NEW] — Enkripsi AES-256-GCM untuk melindungi sesi autentikasi dan Signal Protocol keys WhatsApp di MongoDB.
    - [`baileys-api/src/services/authStateStore.js`](baileys-api/src/services/authStateStore.js) [NEW] — Implementasi MongoDB multi-device auth credentials store yang aman dan atomik.
    - [`baileys-api/src/services/sessionManager.service.js`](baileys-api/src/services/sessionManager.service.js) [NEW] — Manajemen lifecycle multi-socket Baileys: pairing QR code, auto-reconnect backoff, sinkronisasi profil/avatar, dan deteksi un-link device.
    - [`baileys-api/src/services/antiBan.service.js`](baileys-api/src/services/antiBan.service.js) [NEW] & [`baileys-api/src/services/rateLimiter.service.js`](baileys-api/src/services/rateLimiter.service.js) [NEW] — Simulasi human presence (status mengetik `composing`, jeda acak typing delay) dan pembatasan frekuensi pengiriman pesan per akun.
    - [`baileys-api/src/services/backendBridge.service.js`](baileys-api/src/services/backendBridge.service.js) [NEW] — Forwarder pesan masuk, delivery receipts, dan status koneksi ke Backend API.
    - [`baileys-api/src/controllers/internal.controller.js`](baileys-api/src/controllers/internal.controller.js) [NEW] & [`baileys-api/src/controllers/send.controller.js`](baileys-api/src/controllers/send.controller.js) [NEW] — Kontrol sesi REST API dan pengiriman pesan terpadu (teks, gambar, dokumen, audio, video).
  - **Backend Core — Manajemen Akun & Customer Service Multi-Channel**:
    - [`backend/src/models/baileysAccount.model.js`](backend/src/models/baileysAccount.model.js) [NEW] — Model akun Baileys dengan status (`pending_qr`, `connecting`, `connected`, `disconnected`, `logged_out`, `error`), proxy, nomor telepon, dan rate limit.
    - [`backend/src/controllers/baileysAccount.controller.js`](backend/src/controllers/baileysAccount.controller.js) [NEW] & [`backend/src/services/baileysAccount.service.js`](backend/src/services/baileysAccount.service.js) [NEW] — Operasi CRUD akun Baileys dan kontrol otorisasi admin.
    - [`backend/src/controllers/baileysInternal.controller.js`](backend/src/controllers/baileysInternal.controller.js) [NEW] & [`backend/src/services/waChannelRouter.service.js`](backend/src/services/waChannelRouter.service.js) [NEW] — Penerima webhook dari `baileys-api` dan router pesan cerdas untuk menentukan channel tujuan (Meta vs Baileys).
    - [`backend/src/models/waConversation.model.js`](backend/src/models/waConversation.model.js) & [`backend/src/services/waConversation.service.js`](backend/src/services/waConversation.service.js) — Dukungan field `channel` (`meta` | `baileys`) dan relasi `baileys_account`, memisahkan sesi percakapan agar nomor pelanggan yang sama dihubungi lewat akun berbeda tidak saling menimpa.
    - [`backend/src/sockets/baileysAccount.controller.js`](backend/src/sockets/baileysAccount.controller.js) [NEW] & [`backend/src/sockets/waChat.controller.js`](backend/src/sockets/waChat.controller.js) — Event Socket.IO real-time untuk stream QR code pairing, pembaruan status akun live, dan notifikasi percakapan baru.
    - [`backend/src/config/privilege.json`](backend/src/config/privilege.json) — Privilege baru `baileysAccount` (`read`, `create`, `update`, `delete`, `connect`).
    - [`backend/src/data/changelog/releases/issue-213.json`](backend/src/data/changelog/releases/issue-213.json) [NEW] — Dokumentasi rilis v1.63.0.
  - **Frontend — Manajemen Akun & Obrolan Multi-Kanal**:
    - [`frontend/src/app/pages/customerService/baileysAccount/index.jsx`](frontend/src/app/pages/customerService/baileysAccount/index.jsx) [NEW] — Tampilan manajemen kartu akun WhatsApp Baileys dengan informasi foto profil, nomor telepon, dan status live.
    - [`frontend/src/app/pages/customerService/baileysAccount/components/ConnectAccountModal.jsx`](frontend/src/app/pages/customerService/baileysAccount/components/ConnectAccountModal.jsx) [NEW] — Modal pemindaian kode QR WhatsApp dengan stream pembaruan live via Socket.IO.
    - [`frontend/src/app/pages/customerService/baileysAccount/create.jsx`](frontend/src/app/pages/customerService/baileysAccount/create.jsx) [NEW] & [`edit.jsx`](frontend/src/app/pages/customerService/baileysAccount/edit.jsx) [NEW] — Drawer pendaftaran dan konfigurasi akun Baileys beserta izin admin.
    - [`frontend/src/app/pages/customerService/whatsappChat/components/ChannelTabs.jsx`](frontend/src/app/pages/customerService/whatsappChat/components/ChannelTabs.jsx) [NEW] & [`index.jsx`](frontend/src/app/pages/customerService/whatsappChat/index.jsx) — Tab pemisah kanal pada halaman Obrolan (Meta resmi vs multi-akun Baileys) dengan sinkronisasi pesan keluar dari HP secara instan.
    - [`frontend/src/app/router/customerService/baileysAccountRoute.jsx`](frontend/src/app/router/customerService/baileysAccountRoute.jsx) [NEW] — Pendaftaran rute SPA `/customer-service/baileys-account`.
    - [`frontend/src/app/navigation/customerService.js`](frontend/src/app/navigation/customerService.js) — Penambahan menu navigasi **Akun WhatsApp (Baileys)** pada kategori Customer Service.

---

## 🌿 Branch: `issue-267` — Monitoring Latensi Jaringan Real-Time (Smokeping Multi-Protocol Probe ICMP/HTTP/DNS/TCP, RRD Graph Generator & Cron Worker Poller)

### 📌 Informasi Issue

- **Nomor Issue**: #267
- **Judul Issue**: Monitoring Latensi Jaringan Real-Time (Smokeping Multi-Protocol Probe ICMP/HTTP/DNS/TCP, RRD Graph Generator & Cron Worker Poller)
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `7b17d61`)

---

### 📅 Rincian Commit

#### [`61c345a`](https://github.com/user/repo/commit/61c345a) - resolve #267 - 4 September 2026, 16:40:20 WIB
#### [`7b17d61`](https://github.com/user/repo/commit/7b17d61) - Merge branch 'issue-267' into master - 4 September 2026, 16:52:46 WIB

- **Komponen yang Berubah**:
  - **Network Monitor — Probe Latensi Multi-Protokol & Generator RRD (Port 3040)**:
    - [`network-monitor/src/services/ping.service.js`](network-monitor/src/services/ping.service.js) — Pengukuran latensi ICMP ping multi-paket untuk menghitung min, max, avg, dan packet loss percentage.
    - [`network-monitor/src/services/httpProbe.service.js`](network-monitor/src/services/httpProbe.service.js) [NEW] — Probe latensi HTTP/HTTPS dengan rincian durasi DNS lookup, TCP connect, TLS handshake, first byte (TTFB), dan total waktu download.
    - [`network-monitor/src/services/dnsProbe.service.js`](network-monitor/src/services/dnsProbe.service.js) [NEW] — Probe latensi query DNS terhadap record A, AAAA, MX, NS, dan TXT dengan resolver khusus.
    - [`network-monitor/src/services/tcpProbe.service.js`](network-monitor/src/services/tcpProbe.service.js) [NEW] — Probe latensi handshake TCP port spesifik (misal port 80, 443, 8291 Mikrotik).
    - [`network-monitor/src/services/latencyRrd.service.js`](network-monitor/src/services/latencyRrd.service.js) [NEW] — Layanan basis data round-robin (RRDtool) khusus metrik latensi, mengarsipkan data dalam format RRA (1 menit, 5 menit, 1 jam, 1 hari) dan menghasilkan grafik PNG latensi gaya Smokeping.
    - [`network-monitor/src/controllers/latency.controller.js`](network-monitor/src/controllers/latency.controller.js) [NEW] & [`network-monitor/src/routes/latency.route.js`](network-monitor/src/routes/latency.route.js) [NEW] — API probe langsung dan rendering grafik RRD.
  - **Backend Core — Manajemen Target & Rute Latensi**:
    - [`backend/src/models/networkLatencyTarget.model.js`](backend/src/models/networkLatencyTarget.model.js) [NEW] — Skema model target pemantauan: nama, host/IP, protokol (`icmp`, `http`, `dns`, `tcp`), interval polling, status enabled, batas peringatan (warning & critical threshold ms), dan konfigurasi grafik kustom.
    - [`backend/src/services/networkLatency.service.js`](backend/src/services/networkLatency.service.js) [NEW] — Layanan CRUD target latensi, orkestrasi polling berkala, pemanggilan probe ke network-monitor, kalkulasi status target (`normal`, `warning`, `critical`, `down`), dan streaming berkas gambar grafik RRD.
    - [`backend/src/controllers/networkLatency.controller.js`](backend/src/controllers/networkLatency.controller.js) [NEW] & [`backend/src/routes/networkLatency.route.js`](backend/src/routes/networkLatency.route.js) [NEW] — Endpoint API `/api/v1/network-latency` dengan privilege `networkLatency.read`, `create`, `update`, `delete`, dan `probe`.
    - [`backend/src/config/privilege.json`](backend/src/config/privilege.json) — Pendaftaran privilege `networkLatency`.
    - [`backend/src/data/changelog/releases/issue-267.json`](backend/src/data/changelog/releases/issue-267.json) [NEW] — Dokumentasi rilis v1.62.0.
  - **Cron Worker — Poller Berkala**:
    - [`cron-worker/src/jobs/processors/networkLatencyPoll.js`](cron-worker/src/jobs/processors/networkLatencyPoll.js) [NEW] & [`cron-worker/src/jobs/scheduler.js`](cron-worker/src/jobs/scheduler.js) — Job periodik tiap menit untuk memicu probe seluruh target latensi aktif dan meng-update database RRD.
  - **Frontend — Antarmuka Monitoring Latensi Jaringan**:
    - [`frontend/src/app/pages/network/latency/index.jsx`](frontend/src/app/pages/network/latency/index.jsx) [NEW] — Halaman pemantauan latensi interaktif dengan kartu grafik real-time, filter rentang waktu (1 jam, 6 jam, 24 jam, 7 hari, 30 hari), dan tabel daftar target.
    - [`frontend/src/app/pages/network/latency/components/LatencyGraphCard.jsx`](frontend/src/app/pages/network/latency/components/LatencyGraphCard.jsx) [NEW] — Komponen kartu visualisasi grafik RRD dengan badge status kesehatan latensi live.
    - [`frontend/src/app/pages/network/latency/components/LatencyTargetModal.jsx`](frontend/src/app/pages/network/latency/components/LatencyTargetModal.jsx) [NEW] — Modal tambah/edit target dengan form parameter protokol dinamis (`ProtocolParamsFields.jsx`).
    - [`frontend/src/app/pages/network/latency/components/LatencySettingsModal.jsx`](frontend/src/app/pages/network/latency/components/LatencySettingsModal.jsx) [NEW] & [`LatencyGraphStyleFields.jsx`](frontend/src/app/pages/network/latency/components/LatencyGraphStyleFields.jsx) [NEW] — Modal kustomisasi gaya grafik (palet warna line, area fill, ketebalan garis, batas ambang peringatan).
    - [`frontend/src/app/pages/network/latency/components/LatencyGraphZoomModal.jsx`](frontend/src/app/pages/network/latency/components/LatencyGraphZoomModal.jsx) [NEW] — Modal perbesaran grafik beresolusi tinggi untuk inspeksi detail jitter dan spike latensi.
    - [`frontend/src/app/router/network/networkLatencyRoute.jsx`](frontend/src/app/router/network/networkLatencyRoute.jsx) [NEW] — Pendaftaran rute SPA `/network/latency`.
    - [`frontend/src/app/navigation/networks.js`](frontend/src/app/navigation/networks.js) — Penambahan menu navigasi **Monitoring Latensi** pada grup Jaringan.

---

## 🌿 Branch: `issue-265` — Real-Time Badge Counters Navigasi Sidebar & Komprehensif Code Audit Fixes

### 📌 Informasi Issue

- **Nomor Issue**: #265
- **Judul Issue**: Implementasi Real-Time Dynamic Badge Counters pada Navigasi Sidebar dan Komprehensif Code Audit Fixes (Finance, Asset, Developer Logs & Traffic)
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `8813d06`)

---

### 📅 Rincian Commit

#### [`8813d06`](https://github.com/user/repo/commit/8813d06) - resolve #265 - 4 September 2026, 11:37:00 WIB

- **Komponen yang Berubah**:
  - **Backend Core — Penghitung Metrik Real-Time & Audit Fixes**:
    - [`backend/src/controllers/ticket.controller.js`](backend/src/controllers/ticket.controller.js) & [`backend/src/services/ticket.service.js`](backend/src/services/ticket.service.js) — Endpoint `GET /api/v1/tickets/count-open` dengan role-aware filtering (penghitungan tiket perorangan untuk staf teknis vs seluruh tiket untuk admin supervisor).
    - [`backend/src/controllers/waChat.controller.js`](backend/src/controllers/waChat.controller.js) & [`backend/src/services/waConversation.service.js`](backend/src/services/waConversation.service.js) — Endpoint `GET /api/v1/whatsapp-chat/count-unreplied` untuk menghitung percakapan aktif yang menunggu respon staf.
    - [`backend/src/controllers/attendance.controller.js`](backend/src/controllers/attendance.controller.js) & [`backend/src/services/attendance.service.js`](backend/src/services/attendance.service.js) — Endpoint `GET /api/v1/attendance/count-pending` untuk menghitung permohonan absensi pending.
    - [`backend/src/controllers/networkDevice.controller.js`](backend/src/controllers/networkDevice.controller.js) & [`backend/src/services/networkDevice.service.js`](backend/src/services/networkDevice.service.js) — Endpoint `GET /api/v1/network-devices/count-down` untuk menghitung perangkat jaringan yang terputus (*down*).
    - Comprehensive Code Audit Fixes: Perbaikan controller dan service modul keuangan (`financeExpense`, `financeBudgeting`, `financeLedger`, `financeReport`), manajemen aset, log archiving, dan penanganan aktivasi layanan.
  - **Frontend Core — Redux Slices, Hooks, dan Integrasi Menu**:
    - [`frontend/src/features/ticketSlice.js`](frontend/src/features/ticketSlice.js) [NEW] & [`waChatSlice.js`](frontend/src/features/waChatSlice.js) [NEW] & [`attendanceSlice.js`](frontend/src/features/attendanceSlice.js) [NEW] & [`networkDeviceSlice.js`](frontend/src/features/networkDeviceSlice.js) [NEW] — Slice Redux terdedikasi untuk sinkronisasi state counter.
    - [`frontend/src/hooks/useTicketBadge.js`](frontend/src/hooks/useTicketBadge.js) [NEW] — Custom hook pengorkestrasi auto-fetch berkala dan listener event Socket.IO real-time.
    - [`frontend/src/app/layouts/Root.jsx`](frontend/src/app/layouts/Root.jsx) & Layout Sidebar — Injeksi angka badge dinamis ke item menu navigasi bilah sisi utama dan collapsible prime panel.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #268  | Database Tools (DB Tools: Migrasi, MinIO Backup Versioning, Jadwal Otomatis, Google Drive OAuth) | Admin memiliki modul terpusat untuk memigrasikan database MongoDB antar host, membackup/merestore data dari bucket MinIO berbasis riwayat versi, mengatur jadwal backup otomatis, serta mengunggah backup langsung ke Google Drive menggunakan kuota akun pribadi/bisnis via OAuth. |
| #270  | Watchdog Healthcheck Cron-Worker & Robust Timeout Koneksi TCP ZKTeco | Mengeliminasi risiko worker macet/beku tanpa penanganan melalui pengawas internal 10 menit dan Docker Healthcheck auto-restart, serta mencegah freeze sinkronisasi absensi saat mesin ZKTeco offline dengan timeout socket paksa. |
| #213  | Integrasi Multi-Akun WhatsApp Baileys & Routing CS Multi-Channel | Memberikan kemampuan menghubungkan hingga 5 nomor WhatsApp biasa/bisnis via scan QR tanpa biaya Meta Cloud API, dilengkapi sinkronisasi pesan keluar dari HP, anti-ban protection, dan pemisahan sesi percakapan CS. |
| #267  | Monitoring Latensi Jaringan Real-Time (Smokeping Multi-Protocol) | Tim jaringan mendapatkan alat pemantauan kualitas jaringan real-time dengan grafik RRD multi-protokol (ICMP, HTTP, DNS, TCP), deteksi lonjakan latensi (*jitter/spikes*), batas peringatan otomatis, dan modal inspeksi zoom. |
| #265  | Dynamic Real-Time Badge Counters & Audit Fixes | Memberikan indikator angka notifikasi live pada menu sidebar untuk tiket kendala terbuka, pesan WhatsApp belum terbalas, absensi menunggu persetujuan, dan perangkat jaringan down secara instan tanpa reload browser. |

---

### Kemampuan Baru Pengguna/Admin

- **Manajemen & Migrasi Database Mandiri Tanpa Terminal**: Superadmin dapat menyalin seluruh database dari/ke server MongoDB eksternal atau membackup database aktif langsung dari dashboard browser dengan mode *Drop & Replace* atau *Merge*.
- **Pemulihan Berkas Backup Berdasarkan Versi**: Admin dapat meninjau riwayat versi berkas backup di MinIO dan melakukan restore ke versi mana pun di masa lalu dengan perlindungan konfirmasi ganda (ketik ulang nama database dan input password).
- **Otomasi Backup Berkala ke Cloud Multi-Storage**: Mengatur jadwal backup berkala (harian, mingguan, bulanan) pada jam yang diinginkan dan memilih target penyimpanan lokal (MinIO) atau cloud eksternal (Google Drive).
- **Integrasi Akun Google Sekali Klik**: Menghubungkan Google Drive langsung via OAuth resmi dengan kuota akun pengguna asli (bukan service account 0-byte).
- **Pengawasan Latensi Presisi Tinggi**: Staf NOC/Jaringan dapat memantau responsivitas koneksi internet, performa server web, server DNS, dan port router secara kontinyu dengan visualisasi grafik Smokeping.
- **Operasional CS Multi-Nomor Tanpa HP Fisik Selalu di Tangan**: Tim customer service dapat membalas dan memantau chat WhatsApp dari beberapa nomor sekaligus di satu layar obrolan terpadu, lengkap dengan riwayat pesan yang selalu sinkron dengan perangkat fisik.

---

### Bug Fix / Solusi Masalah

- **Pemberantasan Infinite Socket Hang Mesin Absensi**: Memperbaiki koneksi TCP `zkteco-js` yang sebelumnya tidak memiliki penanganan batas waktu saat mesin absen mati, sehingga proses sinkronisasi log absensi tidak lagi membeku tanpa batas waktu.
- **Pencegahan Kematian Diam-Diam Scheduler Cron-Worker**: Menangkap `unhandledRejection` dan `uncaughtException` pada cron worker serta mendeteksi job yang berjalan melebihi 10 menit untuk memicu auto-restart container melalui Docker Healthcheck.
- **Penanganan Versi Hapus Permanen MinIO**: Memperbaiki mekanisme penghapusan berkas backup pada bucket MinIO ber-versioning agar benar-benar menghapus versi tertentu secara permanen dan bukan sekadar menaruh delete marker.
- **Pencegahan Bentrok Sesi Chat Multi-Channel**: Memperbarui compound index dan query model percakapan agar nomor pelanggan yang sama tidak mengalami tumpang tindih pesan saat dihubungi melalui channel resmi Meta maupun akun Baileys yang berbeda.

---

### Menu/Fitur Baru

- **Tab Database Tools**: Tersedia di menu **Pengaturan > Developer > Tab Database Tools** (`/settings` > Developer > `db-tools`).
- **Formulir Jadwal Backup & Modal Backup Instan**: Tersedia pada panel MinIO Backup di dalam tab Database Tools.
- **Kartu Koneksi Google Drive**: Tersedia di menu **Pengaturan > Developer > Tab Lainnya** (`/settings` > Developer > `other`).
- **Halaman Monitoring Latensi**: Tersedia di menu **Jaringan > Monitoring Latensi** (`/network/latency`).
- **Halaman Manajemen Akun WhatsApp (Baileys)**: Tersedia di menu **Customer Service > Akun WhatsApp (Baileys)** (`/customer-service/baileys-account`).
- **Pill Badge Dinamis Sidebar**: Terpasang live pada menu Tiket, Obrolan WA, Absensi, dan Perangkat Jaringan.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Melakukan Backup & Restore Database MongoDB Melalui Database Tools

- **Penjelasan Fitur**:
  - Fitur ini memberikan antarmuka aman bagi Superadmin untuk mengarsipkan database MongoDB aktif ke dalam object storage MinIO sebagai berkas terkompresi `.archive.gz` menggunakan utilitas native `mongodump`.
  - MinIO memanfaatkan bucket ber-versioning sehingga nama berkas backup yang sama dapat menyimpan banyak riwayat versi tanpa khawatir tertimpa secara tidak sengaja.
  - Alur restore dilindungi oleh validasi dua faktor: Superadmin diwajibkan mengetik ulang nama database tujuan secara persis dan memasukkan password akun saat ini.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Pengaturan > Developer**.
  2. Klik tab **Database Tools** (ikon server stack).
  3. **Melakukan Backup Manual**:
     - Pada panel **Backup ke MinIO**, klik tombol **Backup Sekarang**.
     - Masukkan nama berkas backup yang diinginkan (contoh: `dekasimal-prod-backup`), atau kosongkan formulir jika ingin menggunakan nama bawaan otomatis.
     - Klik **Mulai Backup**. Progres dump dan upload akan langsung terpantau pada panel **Progres Job Aktif**.
  4. **Melihat Riwayat Versi & Melakukan Restore**:
     - Cari nama berkas backup pada tabel **Daftar Backup**, lalu klik tombol **Lihat Versi**.
     - Riwayat seluruh versi akan terbuka, lengkap dengan Version ID, ukuran data, dan tanggal pembuatan.
     - Klik tombol **Restore** pada versi backup yang ingin dipulihkan.
     - Pada modal konfirmasi yang muncul, ketik ulang nama database aktif Anda (misal: `dekasimal_v2`) dan masukkan kata sandi akun Anda.
     - Klik tombol konfirmasi **Saya Paham, Lanjutkan**. Sistem akan mengunduh archive versi tersebut dari MinIO dan merestore-nya ke MongoDB.

---

### 2. Menghubungkan Google Drive & Mengonfigurasi Jadwal Backup Otomatis

- **Penjelasan Fitur**:
  - Memungkinkan pengunggahan salinan backup database secara otomatis ke Google Drive pribadi atau perusahaan menggunakan kuota Google asli pemilik akun via OAuth 2.0.
  - Scheduler BullMQ mengeksekusi backup di latar belakang sesuai frekuensi yang dikonfigurasi (Harian, Mingguan, atau Bulanan).

- **Langkah Penggunaan (Tutorial)**:
  1. **Menghubungkan Akun Google Drive**:
     - Buka menu **Pengaturan > Developer**, lalu pilih tab **Lainnya**.
     - Temukan kartu **Koneksi Google Drive** dan klik tombol **Hubungkan Google Drive**.
     - Masukkan **Client ID** dan **Client Secret** yang diperoleh dari Google Cloud Console (pastikan Redirect URI yang tertera di modal telah didaftarkan pada Authorized Redirect URIs di Google Cloud Console).
     - Klik **Simpan & Otorisasi**. Jendela consent Google akan terbuka; pilih akun Google Anda dan setujui izin akses file Drive.
     - Setelah berhasil, kartu akan menampilkan status hijau **Terhubung sebagai: email_anda@gmail.com**.
  2. **Mengatur Jadwal Backup Otomatis**:
     - Kembali ke tab **Database Tools**, scroll ke bawah menuju bagian **Jadwal Backup Otomatis**.
     - Aktifkan toggle **Aktifkan Jadwal Otomatis**.
     - Tentukan **Frekuensi** (contoh: `Harian` untuk setiap malam, `Mingguan` pada hari Minggu, atau `Bulanan` pada tanggal 1).
     - Atur **Jam** dan **Menit** pelaksanaan (misal jam `02:00` dini hari saat trafik rendah).
     - Pada pilihan **Simpan Ke**, pilih **Google Drive** (atau **MinIO**).
     - Klik tombol **Simpan Jadwal**. Job scheduler akan otomatis aktif dan mengeksekusi backup secara teratur.

---

### 3. Menggunakan Monitoring Latensi Jaringan Real-Time (Smokeping Multi-Protocol)

- **Penjelasan Fitur**:
  - Layanan pemantauan latensi berbasis RRDtool yang menguji responsivitas target jaringan secara kontinyu menggunakan berbagai protokol: Ping ICMP, HTTP/HTTPS Web, DNS Lookup, atau Port TCP.
  - Menyajikan visualisasi grafik interaktif bergaya Smokeping untuk mendeteksi deviasi latency, packet loss, dan jitter koneksi.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Jaringan > Monitoring Latensi**.
  2. **Menambahkan Target Pemantauan Baru**:
     - Klik tombol **Tambah Target**.
     - Masukkan nama target (contoh: `Gateway Utama Fiber` atau `DNS Server Google`).
     - Masukkan Host / IP tujuan (contoh: `8.8.8.8` atau `api.dekasimal.id`).
     - Pilih **Protokol**:
       - `ICMP (Ping)`: Masukkan jumlah paket ping dan ukuran paket (bytes).
       - `HTTP`: Masukkan URL lengkap, metode request (`GET`/`HEAD`), dan ekspektasi HTTP status code (`200`).
       - `DNS`: Masukkan record type (`A`, `AAAA`, `MX`) dan resolver khusus bila ada.
       - `TCP`: Masukkan nomor port layanan yang ingin dipantau (contoh: `80`, `443`, `8291`).
     - Atur batas **Warning Threshold** dan **Critical Threshold** dalam satuan milidetik (ms).
     - Klik **Simpan Target**.
  3. **Memantau & Menganalisis Grafik Latensi**:
     - Target baru akan tampil dalam bentuk kartu grafik di dashboard pemantauan.
     - Pilih tab rentang waktu (contoh: `1 Jam`, `24 Jam`, atau `7 Hari`) untuk melihat riwayat performa koneksi.
     - Klik ikon **Zoom** pada kartu untuk membuka modal visualisasi beresolusi tinggi dan memeriksa riwayat spike secara mendetail.
