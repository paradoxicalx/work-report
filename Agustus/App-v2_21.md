# 📝 Daily Work Report - Dedy S.N Putra (2026-08-21)

---

## 📅 Laporan Harian - 21 Agustus 2026

---

## 🌿 Branch: `issue-119` — Halaman Khusus Mode Developer, Monitoring Sistem Lingkungan, dan Logging Terpusat (API Logs, Error Reporting, Tracing ID, Pengarsipan Log MinIO, serta Notifikasi Telegram Alert)

### 📌 Informasi Issue

- **Nomor Issue**: #119
- **Judul Issue**: Halaman Khusus Mode Developer, Monitoring Sistem Lingkungan, dan Logging Terpusat (API Logs, Error Reporting, Tracing ID, Pengarsipan Log MinIO, serta Notifikasi Telegram Alert)
- **Status Branch**: `Belum di-merge` (Branch aktif: `origin/issue-119`)

### 📅 Rincian Commit / Perubahan Hari Ini

#### [WIP / Uncommitted] - save #119 - 21 Agt 2026 18:05

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/log.controller.js`
  - `backend/src/middlewares/auth.middleware.js`
  - `backend/src/middlewares/logger.middleware.js`
  - `backend/src/models/admin.model.js`
  - `backend/src/models/apiLog.model.js` [NEW]
  - `backend/src/routes/internal.route.js`
  - `backend/src/routes/log.route.js`
  - `backend/src/services/admin.service.js`
  - `backend/src/services/apiLog.service.js` [NEW]
  - `backend/src/services/networkDevice.service.js`
  - `backend/src/services/systemEnvironment.service.js` [NEW]
  - `backend/src/services/waSender.service.js`
  - `backend/src/services/whatsappControl.service.js`
  - `backend/src/utils/correlation.js` [NEW]
  - `backend/src/utils/logger.js`
  - `backend/src/utils/minio.js`
  - `backend/src/utils/telegramAlert.js` [NEW]
  - `backend/test/unit/admin.developer.test.js` [NEW]
  - `backend/test/unit/apiLog.service.test.js` [NEW]
  - `backend/test/unit/correlation.test.js` [NEW]
  - `backend/test/unit/loggerSanitizer.test.js` [NEW]
  - `backend/test/unit/systemEnvironment.test.js` [NEW]
  - `backend/test/unit/telegramAlert.test.js` [NEW]
  - `cron-worker/src/jobs/processors/apiLogArchiveCleanup.js` [NEW]
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `cron-worker/src/utils/logger.js` [NEW]
  - `frontend/src/app/navigation/settings.js`
  - `frontend/src/app/pages/finance/expenses/detail.jsx`
  - `frontend/src/app/pages/finance/expenses/schema/columns.jsx`
  - `frontend/src/app/pages/finance/purchase-requests/detail.jsx`
  - `frontend/src/app/pages/settings/Layout.jsx`
  - `frontend/src/app/pages/settings/Sidebar/SidebarPanel/index.jsx`
  - `frontend/src/app/pages/settings/sections/Developer.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/logs/SystemLogsTab.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/logs/drawers/LogDetailDrawer.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/logs/schema/columns.jsx` [NEW]
  - `frontend/src/app/router/settings/settingsRoute.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/main.jsx`
  - `frontend/src/middleware/AuthGuard.jsx`
  - `frontend/src/utils/axios.js`
  - `frontend/src/utils/devLogger.js` [NEW]
  - `frontend/src/utils/errorReporter.js` [NEW]
  - `network-monitor/src/utils/logger.js`
  - `whatsapp-api/src/utils/logger.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Model Admin & Proteksi Akses Khusus Developer (`admin.model.js` & `auth.middleware.js`)**:
    - Menambahkan atribut `developer: { type: Boolean, default: false }` pada skema `Admin`. Parameter ini bersifat eksklusif tingkat internal dan tidak dapat dimodifikasi melalui form admin standar (hanya dapat disetel langsung di database oleh pengelola sistem).
    - Mengimplementasikan middleware otorisasi `protectedDeveloper` di backend yang memverifikasi sesi JWT dan memastikan nilai `req.user.developer === true`. Jika user bukan developer, sistem secara tegas menolak akses dengan status `403 Forbidden`.
    - Di sisi frontend, rute `/settings/developer` dan navigasi sidebar Pengaturan diproteksi secara kondisional; tombol menu dan halaman hanya ditampilkan kepada pengguna dengan status developer aktif, dan me-redirect pengguna non-developer ke dashboard.
  - **Halaman Pengaturan Developer Terpadu (`Developer.jsx`)**:
    - Merancang antarmuka pengaturan developer dengan 4 tab komprehensif:
      1. **Status & Diagnostik**: Menampilkan kartu status developer aktif, switch preferensi runtime (Console Debug, Network Tracking, Client Error Reporting), informasi koneksi Socket.io (Socket ID, namespace `/admin`, status koneksi, latensi ping-pong real-time), serta tombol pengujian Telegram Alert.
      2. **Log Sistem (API Logs)**: Antarmuka pemantauan log terpusat berbasis TanStack Table (`SystemLogsTab.jsx`) dengan filter dinamis (Level, HTTP Method, HTTP Status Code, Service Name, Rentang Tanggal, dan Global Search Regex), drawer inspeksi detail log interaktif (`LogDetailDrawer.jsx`), ekspor log ke berkas JSON (maksimal 5000 entri), dan tombol manual untuk pengarsipan & pembersihan log.
      3. **Lingkungan Sistem**: Visualisasi metrik kesehatan server secara mendalam (`systemEnvironment.service.js`) mencakup Hostname, Platform OS, arsitektur, uptime, CPU model, kecepatan clock & load average, konsumsi RAM (Total, Used, Free, persentase), antarmuka jaringan & IP primer server, runtime Node.js/V8/OpenSSL, alokasi memori process heap, serta status dan latensi ping layanan pendukung (MongoDB, Redis, MinIO S3, Network Monitor, Cron Worker, Radius Server).
      4. **Klien & Browser**: Menampilkan spesifikasi lingkungan browser pengguna (User Agent, platform OS, resolusi layar, pixel ratio, bahasa, dan status online).
  - **Pelacakan Alur Request (Correlation ID) & Tracing Context (`correlation.js` & `logger.middleware.js`)**:
    - Membangun utilitas `correlation.js` menggunakan Node.js `AsyncLocalStorage` untuk menyimpan dan meneruskan `correlationId` secara transparan di sepanjang alur eksekusi asynchronous backend.
    - Middleware korelasi membaca header `x-correlation-id` / `x-request-id` yang dikirim dari klien atau secara otomatis menghasilkan UUID v4 baru, melampirkannya ke header respons, serta menyertakannya pada setiap entri log Winston.
    - Interceptor Axios di frontend (`axios.js`) disesuaikan untuk secara otomatis melampirkan header `x-correlation-id` pada setiap permintaan HTTP, memungkinkan pelacakan masalah dari UI hingga ke database secara persis.
  - **Penyaringan Data Sensitif & Peringkasan Payload Besar (`logger.middleware.js`)**:
    - Menyempurnakan fungsi `removeSensitive` dengan mekanisme rekursif dan penanganan *circular reference* menggunakan `WeakSet`. Seluruh kunci sensitif (seperti `password`, `token`, `secret`, `apikey`, `pin`, `otp`, `nik`, `npwp`, `card_number`, `signature`, dsb.) otomatis disamarkan menjadi `[REDACTED]`.
    - Meringkas string payload berukuran besar dan data Base64 (gambar / tanda tangan digital) menjadi `[BASE64_DATA: mime, ~N bytes]` serta membatasi teks panjang di atas 1000 karakter (`formatLargeString`) guna mencegah lonjakan ukuran dokumen log MongoDB.
    - Meringkas objek berkas upload (`sanitizeFiles`) sehingga hanya menyimpan nama file, ukuran, dan mimetype tanpa buffer biner mentah.
  - **Pengarsipan Log Otomatis ke MinIO & Pembersihan MongoDB (`apiLog.service.js` & `apiLogArchiveCleanup.js`)**:
    - Merancang skema Mongoose `apiLog.model.js` dengan index majemuk `{ timestamp: -1, level: 1 }` dan TTL index retensi otomatis 30 hari.
    - Mengembangkan fungsi `archiveAndCleanupOldLogs` yang mengelompokkan log lama berdasarkan tanggal (`YYYY-MM-DD`), mengunggah berkas JSON terstruktur ke bucket MinIO `system-logs-archive` (`archives/YYYY-MM/api_logs_YYYY-MM-DD.json`), lalu menghapus record lama dari MongoDB.
    - Mengintegrasikan job processor `apiLogArchiveCleanup.js` di `cron-worker` yang dijadwalkan berjalan harian pada pukul 03:00 dini hari via BullMQ.
  - **Notifikasi Peringatan Kritis Telegram & Deteksi Lonjakan Error (`telegramAlert.js`)**:
    - Membuat transport Winston khusus `TelegramAlertTransport` yang terhubung ke destinasi channel debug Telegram.
    - Format pesan alert HTML yang rapi dengan indikator service, level, Trace ID, endpoint HTTP, IP klien, deskripsi error, dan potongan stack trace.
    - Mekanisme deduplikasi berbasis *fingerprint* unik dengan cooldown 5 menit per jenis error untuk mencegah spam notifikasi.
    - Fitur deteksi lonjakan error (*Error Surge Detection*): mendeteksi jika terjadi 10 atau lebih error dalam rentang sliding window 5 menit, dan otomatis mengirimkan notifikasi rekapitulasi status code serta top endpoint yang mengalami kegagalan.
  - **Ingest Log Lintas Microservice & Sinkronisasi Utilitas Logger**:
    - Menyediakan endpoint internal `POST /internal/logs/ingest` yang memungkinkan layanan satelit (`network-monitor`, `whatsapp-api`, `cron-worker`) mengirimkan log error ke database log pusat backend menggunakan autentikasi `x-api-key`.
    - Memperbarui utilitas logger pada `network-monitor`, `whatsapp-api`, dan `cron-worker` agar mengalirkan error kritis ke backend log collector.
  - **Frontend Utilities & DevTools Control (`devLogger.js`, `errorReporter.js`, `axios.js`)**:
    - Membuat utilitas `devLogger.js` untuk mengontrol output log konsol berdasarkan flag localStorage `dev_console_debug`.
    - Mengimplementasikan `errorReporter.js` untuk menangkap *uncaught exceptions* dan *unhandled promise rejections* pada runtime frontend dan melaporkannya ke backend (`POST /public/client-error`).
    - Menambahkan visualisasi badge status baru pada `rows.jsx` dan `status.js` untuk mendukung ragam level log sistem.
  - **Suite Unit Test Komprehensif (`backend/test/unit/`)**:
    - Menulis 6 berkas test unit dengan 34 test cases di `backend/test/unit/` (`admin.developer.test.js`, `apiLog.service.test.js`, `correlation.test.js`, `loggerSanitizer.test.js`, `systemEnvironment.test.js`, `telegramAlert.test.js`) yang memvalidasi seluruh alur logika bisnis, sanitasi, tracing, arsip log, dan alert notifikasi dengan status lulus 100%.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #119  | Halaman Khusus Mode Developer, Monitoring Sistem Lingkungan, dan Logging Terpusat | Memperkuat visibilitas teknis, pelacakan error, pemantauan kesehatan server real-time, pengarsipan log otomatis, dan mitigasi insiden sistem dengan alert Telegram terpadu. |

### Kemampuan Baru Pengguna/Admin

- **Pengembang Sistem (Developer)**:
  - Memiliki halaman khusus mandiri (`/settings/developer`) untuk memantau performa dan kesehatan server (CPU, RAM, Uptime, Latensi MongoDB/Redis/MinIO, antarmuka jaringan).
  - Mampu mencari, memfilter, dan menginspeksi log HTTP API serta runtime error secara mendalam dengan drawer JSON interaktif dan pelacakan Trace ID korelasi.
  - Dapat mengekspor data log hingga 5000 entri ke dalam format JSON untuk analisis lanjutan.
  - Mampu memicu pengarsipan dan pembersihan log ke MinIO storage secara instan dengan satu klik.
  - Dapat mengaktifkan atau menonaktifkan fitur debugging DevTools, network activity tracking, dan pelaporan error klien secara persis dari antarmuka web.

### Bug Fix / Solusi Masalah

- **Penyembunyian Data Sensitif & Reduksi Beban Log**:
  - Menyelesaikan masalah pencatatan kredensial sensitif pada log HTTP dengan penyaringan rekursif kata kunci rahasia.
  - Meringkas payload Base64 dan string berukuran besar agar koleksi database `api_logs` tetap ramping dan terhindar dari overhead memori.
- **Pencegahan Banjir Notifikasi (Alert Flooding)**:
  - Mengimplementasikan deduplikasi error dengan cooldown 5 menit dan deteksi lonjakan error (*surge detection*) sehingga channel Telegram tidak dibanjiri pesan duplikat saat terjadi insiden server berulang.
- **Isolasi Log Otomatis**:
  - Mengurangi beban database MongoDB jangka panjang dengan pembersihan otomatis retensi 30 hari (TTL index) dan background worker arsip ke MinIO harian.

### Menu/Fitur Baru

- **Menu Pengaturan > Developer** (`/settings/developer`): Halaman diagnostik dan logging komprehensif khusus akun admin berstatus developer.
- **Panel Diagnostik Status & Koneksi WebSocket**: Pemantauan status handshake dan latensi koneksi real-time Socket.io.
- **Panel Diagnostik Lingkungan Server**: Visualisasi detail hardware server, memori heap, jaringan, dan status konektivitas multi-service.
- **Drawer Detail Log JSON (`LogDetailDrawer`)**: Drawer inspeksi payload request, headers, query params, stack trace, dan profil user yang memicu aksi.
- **Endpoint Internal Ingest Log (`/internal/logs/ingest`)**: Saluran agregasi log lintas microservice (`network-monitor`, `whatsapp-api`, `cron-worker`).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **Mode Developer & Sistem Logging Terpusat** adalah modul diagnostik internal yang dirancang untuk pengembang dan DevOps dalam memantau kesehatan seluruh infrastruktur ISPF V2. Modul ini menggabungkan pelacakan korelasi request (Trace ID), penyaringan data sensitif otomatis, pengarsipan log ke MinIO S3, pelaporan error klien, serta notifikasi peringatan Telegram yang cerdas dan anti-spam.

- **Langkah Penggunaan (Tutorial)**:
  1. **Mengaktifkan Akses Developer**:
     - Buka database MongoDB dan setel field `developer: true` pada dokumen admin yang bersangkutan (contoh: `db.admins.updateOne({ username: "nama_admin" }, { $set: { developer: true } })`).
  2. **Mengakses Halaman Developer**:
     - Login ke aplikasi web ISPF, buka menu **Pengaturan** di sidebar, lalu pilih sub-menu **Developer** (terletak di bawah menu Keuangan).
  3. **Mengatur Preferensi Runtime Pengembang**:
     - Pada tab **Status & Diagnostik**, aktifkan opsi *Debug Output di DevTools Console* atau *Pelacakan Aktivitas Jaringan Frontend* sesuai kebutuhan analisis.
     - Klik tombol **Uji Coba Alert Telegram** untuk memastikan integrasi bot notifikasi debug berfungsi dengan normal.
  4. **Memeriksa Log API & Error Trace**:
     - Buka tab **Log Sistem**. Gunakan bilah filter di atas tabel untuk memfilter log berdasarkan Level (INFO, WARN, ERROR, FATAL, CLIENT ERROR), HTTP Status (200, 400, 500, dsb.), atau nama layanan (`backend`, `network-monitor`, `whatsapp-api`, `cron-worker`).
     - Klik tombol ikon mata / baris log untuk membuka **Drawer Detail Log** dan membaca informasi request, headers yang telah disanitasi, serta stack trace error.
  5. **Mengarsipkan & Membersihkan Log**:
     - Klik tombol **Arsipkan & Bersihkan Log** di sudut kanan atas tab Log Sistem, konfirmasikan aksi pada dialog modal untuk memindahkan log ke MinIO storage dan melegakan kapasitas database MongoDB.
  6. **Memantau Kesehatan Infrastruktur**:
     - Buka tab **Lingkungan Sistem** untuk memeriksa penggunaan RAM, beban CPU server, alamat IP antarmuka jaringan, serta latensi koneksi MongoDB, Redis, dan microservice pendukung.
