# 📝 Daily Work Report - Dedy S.N Putra (26 Oktober 2026)

---

## 📅 Laporan Harian - 26 Oktober 2026

---

## 🌿 Branch: `master` & `production` — Pembaruan Catatan Rilis (Changelog v1.98.29 – v1.98.31) & Sinkronisasi Rilis Produksi

### 📌 Informasi Pembaruan

- **Nomor Versi**: `v1.98.29`, `v1.98.30`, `v1.98.31` (Total 3 rilis tercakup)
- **Judul Rilis**: Sinkronisasi Catatan Rilis Dekasimal V2 & Deploy Produksi
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `production` pada Senin, 26 Oktober 2026, 20:54:23 WIB via commit `231e8eb0`)

### 📅 Rincian Commit

#### [231e8eb0] - Merge branch 'master' into production - Senin, 26 Oktober 2026, 20:54:23 WIB

_(Merge commit menyinkronkan seluruh pembaruan kode, fitur, dan perbaikan dari branch `master` ke branch `production`)_

---

#### [7703686c] - update changelog - Senin, 26 Oktober 2026, 20:54:04 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-479.json` [NEW]
  - `backend/src/data/changelog/releases/issue-480.json` [NEW]
  - `backend/src/data/changelog/releases/issue-487.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Generasi Berkas Detail Rilis Changelog**:
    - Menghasilkan 3 berkas rilis terstruktur berbasis JSON di direktori `backend/src/data/changelog/releases/` untuk issue #480 (`v1.98.29`), issue #479 (`v1.98.30`), dan issue #487 (`v1.98.31`).
    - Menyusun dokumentasi rilis yang ramah pengguna, merinci fitur baru, bug fix, dan peningkatan stabilitas sistem tanpa jargon teknis internal.
  - **Pembaruan Indeks Metadata Rilis (`index.json`)**:
    - Memperbarui berkas `backend/src/data/changelog/index.json` dengan menambahkan 3 entri rilis terbaru di posisi teratas secara kronologis mundur.
    - Menyediakan teks pencarian komprehensif (`searchText`) pada setiap entri rilis untuk mendukung fitur penelusuran cepat pada antarmuka Catatan Rilis admin.

---

## 🌿 Branch: `issue-487` — Issue #487: Diagnostik Koneksi Google Drive dan Peningkatan Keandalan Aplikasi

### 📌 Informasi Issue

- **Nomor Issue**: #487
- **Judul Issue**: Diagnostik Koneksi Google Drive dan Peningkatan Keandalan Aplikasi
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Senin, 26 Oktober 2026, 20:46:02 WIB via commit `8eb1f8b5`)

### 📅 Rincian Commit

#### [8eb1f8b5] - resolve #487 - Senin, 26 Oktober 2026, 20:46:02 WIB

_(Merge commit menggabungkan branch `issue-487` ke branch `master`)_

---

#### [b4384a2c] - resolve #487 - Senin, 26 Oktober 2026, 14:56:16 WIB

- **Komponen yang Berubah**:
  - **Backend & Database Tools**:
    - `backend/.env.example`
    - `backend/scripts/releaseOrphanIpReservations.js` [NEW]
    - `backend/src/config/privilege.json`
    - `backend/src/config/privilegeDictionary.json`
    - `backend/src/controllers/dbTools.controller.js`
    - `backend/src/controllers/networkIPv4.controller.js`
    - `backend/src/controllers/radiusAuthentication.controller.js`
    - `backend/src/controllers/radiusAuthenticationNonCustomer.controller.js`
    - `backend/src/grpc/server.js`
    - `backend/src/jobs/dbToolsWorker.js`
    - `backend/src/locales/en/translation.json`
    - `backend/src/locales/id/translation.json`
    - `backend/src/models/dbToolsJob.model.js`
    - `backend/src/routes/dbTools.route.js`
    - `backend/src/routes/radiusAuthentication.route.js`
    - `backend/src/routes/radiusAuthenticationNonCustomer.route.js`
    - `backend/src/server.js`
    - `backend/src/services/dbTools.service.js`
    - `backend/src/services/dbToolsGdrive.service.js` [NEW]
    - `backend/src/services/dbToolsQueue.service.js`
    - `backend/src/services/dbToolsReconcile.service.js` [NEW]
    - `backend/src/services/networkIPv4Used.service.js`
    - `backend/src/services/networkPing.service.js`
    - `backend/src/services/partner.service.js`
    - `backend/src/services/radiusAuthentication.service.js`
    - `backend/src/services/recovery.service.js`
    - `backend/src/utils/googleDrive.util.js`
    - `backend/src/utils/minio.js`
    - `backend/src/utils/startupStep.core.js` [NEW]
    - `backend/src/utils/startupStep.js` [NEW]
    - `backend/test/integration/dbToolsGdriveController.test.js` [NEW]
    - `backend/test/integration/dbToolsGdriveDestination.test.js` [NEW]
    - `backend/test/integration/dbToolsGdriveDiagnostics.test.js` [NEW]
    - `backend/test/integration/dbToolsReconcile.test.js` [NEW]
    - `backend/test/integration/googleDriveScope.util.test.js` [NEW]
    - `backend/test/integration/networkIPv4.pingIPv4.test.js` [NEW]
    - `backend/test/integration/networkIPv4Used.service.test.js`
    - `backend/test/integration/partner.listFields.test.js` [NEW]
    - `backend/test/integration/radiusAuthentication.deleteDisconnect.test.js` [NEW]
    - `backend/test/integration/radiusAuthentication.readDeleted.test.js` [NEW]
    - `backend/test/integration/radiusAuthentication.readSensitive.test.js` [NEW]
    - `backend/test/integration/radiusAuthenticationNonCustomer.delete.test.js` [NEW]
    - `backend/test/integration/radiusAuthenticationNonCustomer.readSensitive.test.js` [NEW]
    - `backend/test/integration/releaseOrphanIpReservations.test.js` [NEW]
    - `backend/test/integration/staleQuotaLimitRecovery.test.js` [NEW]
    - `backend/test/unit/startupStep.test.js` [NEW]
  - **Cron Worker & Network Monitor & Microservices**:
    - `cron-worker/src/jobs/processors/backendHealthCheck.js`
    - `cron-worker/src/utils/consoleFormat.js` [NEW]
    - `cron-worker/src/utils/jobFailureReporter.js` [NEW]
    - `cron-worker/src/utils/jobReporter.js` [NEW]
    - `cron-worker/src/utils/startupStep.core.js` [NEW]
    - `cron-worker/src/utils/startupStep.js` [NEW]
    - `cron-worker/test/consoleFormat.test.js` [NEW]
    - `cron-worker/test/jobFailureReporter.test.js` [NEW]
    - `network-monitor/src/server.js`
    - `network-monitor/src/services/rrd.service.js`
    - `network-monitor/src/utils/startupChecks.js` [NEW]
    - `network-monitor/src/utils/startupStep.core.js` [NEW]
    - `network-monitor/src/utils/startupStep.js` [NEW]
    - `network-monitor/test/startupChecks.test.js` [NEW]
    - `telegram-api/src/botManager.js`
    - `telegram-api/src/server.js`
    - `telegram-api/src/utils/startupStep.core.js` [NEW]
    - `telegram-api/src/utils/startupStep.js` [NEW]
    - `radius-server/DOCUMENTATION.md`
    - `radius-server/internal/domain/auth/authenticate.go`
    - `radius-server/internal/domain/auth/authenticate_test.go`
  - **Frontend UI & Components**:
    - `frontend/src/app/pages/errors/ChunkLoadFallback.test.jsx` [NEW]
    - `frontend/src/app/pages/errors/RootErrorBoundary.jsx`
    - `frontend/src/app/pages/settings/sections/developer/dbTools/BackupScheduleForm.jsx`
    - `frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectModal.jsx`
    - `frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectionCard.jsx`
    - `frontend/src/app/pages/settings/sections/developer/dbTools/GdriveDiagnosticsModal.jsx` [NEW]
    - `frontend/src/app/pages/settings/sections/developer/dbTools/MigrationForm.jsx`
    - `frontend/src/app/pages/settings/sections/developer/dbTools/MinioBackupPanel.jsx`
    - `frontend/src/app/pages/settings/sections/developer/dbTools/gdriveDiagnostics.js` [NEW]
    - `frontend/src/app/pages/settings/sections/developer/dbTools/gdriveDiagnostics.test.js` [NEW]
    - `frontend/src/app/pages/settings/sections/developer/dbTools/gdriveResult.js` [NEW]
    - `frontend/src/app/pages/settings/sections/developer/dbTools/gdriveResult.test.js` [NEW]
    - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
    - `frontend/src/components/shared/DeletedAccountNotice.jsx` [NEW]
    - `frontend/src/components/shared/DeletedAccountNotice.test.jsx` [NEW]
    - `frontend/src/components/shared/SensitivePassword.jsx` [NEW]
    - `frontend/src/components/template/PwaUpdatePrompt.jsx`
    - `frontend/src/constants/privilegeDescriptions.en.json`
    - `frontend/src/constants/privilegeDescriptions.id.json`
    - `frontend/src/i18n/locales/en/translations.json`
    - `frontend/src/i18n/locales/id/translations.json`
    - `frontend/src/pwa/assetCache.js` [NEW]
    - `frontend/src/pwa/assetCache.test.js` [NEW]
    - `frontend/src/pwa/chunkDiagnostics.js` [NEW]
    - `frontend/src/pwa/chunkDiagnostics.test.js` [NEW]
    - `frontend/src/pwa/sw.js`
    - `frontend/src/pwa/updateLoopDiagnostics.js` [NEW]
    - `frontend/src/pwa/updateLoopDiagnostics.test.js` [NEW]
    - `frontend/src/utils/customerLabel.js` [NEW]
    - `frontend/src/utils/customerLabel.test.js` [NEW]
    - `frontend/src/utils/networkLogRedaction.js`
    - `frontend/src/utils/networkLogRedaction.test.js`
    - `frontend/vite.config.js`
    - Pembaruan berkas-berkas dashboard, network, services, dan tabel pengguna untuk integrasi komponen password sensitif dan penanganan akun terhapus.
  - **Alat Pengembang Monorepo & Standarisasi Startup**:
    - `package.json`
    - `tools/dev/dev.mjs` [NEW]
    - `tools/dev/lib.mjs` [NEW]
    - `tools/dev/lib.test.mjs` [NEW]
    - `tools/dev/orchestrator.mjs` [NEW]
    - `tools/dev/orchestrator.test.mjs` [NEW]
    - `tools/dev/services.config.mjs` [NEW]
    - `tools/startup-step/package.json` [NEW]
    - `tools/startup-step/startupStep.core.js` [NEW]
    - `tools/startup-step/sync.mjs` [NEW]
    - `tools/startup-step/sync.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Alat Diagnostik & Pengujian Koneksi Google Drive**:
    - Menyediakan antarmuka dan layanan API `POST /db-tools/destinations/gdrive/test` dan `POST /db-tools/destinations/gdrive/diagnose` untuk memeriksa status otorisasi, validitas token OAuth, izin folder target, serta ruang kuota penyimpanan Google Drive.
    - Menghadirkan modal diagnostik interaktif `GdriveDiagnosticsModal` pada menu Pengaturan Pengembang > Cadangan Database dengan rincian status per tahap pengujian, pesan error yang jelas, dan rekomendasi langkah perbaikan jika terjadi kendala otorisasi.
    - Menangani cakupan otorisasi file drive (`drive.file`) dan memberikan peringatan jika pengguna belum memberikan centang izin akses berkas pada consent screen Google.
  - **Alat Pemulihan Akun Terkunci Kuota Kedaluwarsa (Stale Quota Limit Recovery)**:
    - Menambahkan fitur pemulihan akun jaringan pada menu Alat Pemulihan Sistem untuk mendeteksi dan membuka kunci akun Radius yang terkena batas kuota basi/kedaluwarsa akibat inkonsistensi sinkronisasi masa aktif.
    - Mengintegrasikan rute pemulihan baru di `recoveryRoutes.config.js` dan layanan backend `recovery.service.js`.
  - **Pemberitahuan Informatif Akun Terhapus (`DeletedAccountNotice`)**:
    - Membuat komponen `DeletedAccountNotice` yang menampilkan peringatan visual ramah saat staf admin membuka halaman atau referensi yang terhubung ke akun yang sudah dihapus/dinonaktifkan dari sistem.
  - **Komponen Sandi Sensitif Terproteksi (`SensitivePassword`)**:
    - Mengimplementasikan komponen `SensitivePassword` dengan fitur sensor bawaan (masked), tombol buka-tutup intip mata (*reveal/hide*), serta tombol salin instan (*copy to clipboard*) untuk menjaga keamanan informasi kredensial di layar dari pandangan pihak yang tidak berkepentingan.
  - **Ketahanan PWA & Mitigasi Loop Pembaruan Aplikasi (Chunk Load & Cache Resilience)**:
    - Membangun modul `assetCache.js`, `chunkDiagnostics.js`, dan `updateLoopDiagnostics.js` pada layer PWA service worker dan root error boundary untuk mendeteksi kegagalan unduh berkas chunk (versi baru rilis) secara otomatis.
    - Mencegah perulangan refresh tanpa henti (*infinite reload loop*) saat aplikasi diperbarui ke versi baru, dengan mekanisme pembersihan cache lama dan pemberitahuan update yang mulus.
  - **Standarisasi Pengecekan Kesiapan Layanan (Startup Step Checks)**:
    - Membuat pustaka inti `startupStep.core.js` yang disinkronkan ke seluruh microservice (`backend`, `cron-worker`, `network-monitor`, `telegram-api`) untuk melakukan pengecekan kesehatan mandiri (koneksi database, Redis, port ketersediaan) sebelum server dinyatakan siap melayani lalu lintas request.
  - **Alat Orkestrasi Pengembang Monorepo (`tools/dev`)**:
    - Menghadirkan skrip runner `orchestrator.mjs` untuk mempermudah menjalankan dan mengelola seluruh rangkaian service Dekasimal V2 pada lingkungan lokal pengembang.

---

## 🌿 Branch: `issue-479` — Issue #479: Pantauan Langsung Tagihan dan Pembaruan Halaman Pembayaran Online

### 📌 Informasi Issue

- **Nomor Issue**: #479
- **Judul Issue**: Pantauan Langsung Tagihan dan Pembaruan Halaman Pembayaran Online
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Senin, 26 Oktober 2026, 19:55:41 WIB via commit `966244c0`)

### 📅 Rincian Commit

#### [966244c0] - resolve #479 - Senin, 26 Oktober 2026, 19:55:41 WIB

_(Merge commit menggabungkan branch `issue-479` ke branch `master`)_

---

#### [58223d94] - resolve #479 - Senin, 26 Oktober 2026, 19:54:14 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/financePayment.controller.js`
  - `backend/src/controllers/publicPayment.controller.js`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/financeInvoice.route.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/liveInvoice.service.js`
  - `backend/src/sockets/liveInvoice.controller.js`
  - `backend/src/utils/liveInvoicePresence.js`
  - `backend/test/integration/liveInvoice.test.js`
  - `backend/test/setup/redis.stub.js`
  - `backend/test/unit/liveInvoicePresence.test.js`
  - `frontend/src/app/pages/finance/invoices/InvoiceWhatsappHistory.jsx`
  - `frontend/src/app/pages/finance/invoices/detail.jsx`
  - `frontend/src/app/pages/finance/invoices/live/LiveInvoicePanel.jsx`
  - `frontend/src/app/pages/finance/invoices/live/liveInvoiceStage.js`
  - `frontend/src/app/pages/finance/invoices/live/liveInvoiceStage.test.js`
  - `frontend/src/app/pages/finance/invoices/live/useLiveInvoice.js`
  - `frontend/src/components/shared/payment/PaymentInstructionView.jsx`
  - `frontend/src/components/shared/payment/paymentDisplay.js`
  - `frontend/src/components/shared/payment/paymentDisplay.test.js`
  - `frontend/src/components/ui/Timeline/TimelineItem.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Panel Pantauan Langsung Status Aktivitas Tagihan (`LiveInvoicePanel`)**:
    - Mengimplementasikan pemantauan kehadiran pelanggan secara realtime saat mengakses tautan pembayaran faktur via Socket.io dan Redis presence.
    - Menampilkan tahapan pelanggan secara langsung: apakah pelanggan sedang melihat tagihan (*viewing*), beralih ke aplikasi lain/latar belakang (*away*), sedang memilih saluran pembayaran (*choosing*), atau sedang membuka instruksi pembayaran (*awaiting payment*).
    - Menyediakan log aktivitas interaksi pelanggan pada faktur dalam 24 jam terakhir (kapan tautan dibuka, metode apa yang dipilih, apakah instruksi pembayaran diterbitkan atau dibatalkan).
  - **Penyempurnaan Halaman Pembayaran Online Publik**:
    - Memperbarui antarmuka `PaymentInstructionView` dan tata letak pemilihan metode pembayaran online publik agar lebih responsif, bersih, dan mudah digunakan di perangkat seluler.
    - Menyediakan tombol salin praktis untuk Nomor Virtual Account dan Kode Pembayaran dengan umpan balik pesan tersalin (*toast*) otomatis.
    - Menambahkan sanitasi keamanan terhadap tautan pembayaran eksternal untuk mencegah skema URL berisiko (`javascript:`).
  - **Peningkatan Fleksibilitas Komponen Timeline**:
    - Memperbarui `TimelineItem.jsx` untuk mendukung format waktu yang lebih fleksibel (berupa teks langsung, angka timestamp, maupun objek Date) serta perbaikan perataan teks dan layout visual.

---

## 🌿 Branch: `issue-480` — Issue #480: Penguatan Validasi dan Keamanan Integrasi Pelanggan Mitra

### 📌 Informasi Issue

- **Nomor Issue**: #480
- **Judul Issue**: Penguatan Validasi dan Keamanan Integrasi Pelanggan Mitra
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Senin, 26 Oktober 2026, 15:21:28 WIB via commit `e476c7cf`)

### 📅 Rincian Commit

#### [e476c7cf] - resolve #480 - Senin, 26 Oktober 2026, 15:21:28 WIB

_(Merge commit menggabungkan branch `issue-480` ke branch `master`)_

---

#### [92fc4d8d] - resolve #480 - Senin, 26 Oktober 2026, 15:20:59 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/partnerApiCustomer.controller.js`
  - `backend/test/integration/partnerApiCustomer.hardening.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Penguatan Penanganan Berkas & Pembersihan File Sementara**:
    - Memperbaiki handler unggah dokumen pelanggan mitra `setDocument` agar secara ketat menolak permintaan jika berkas dikirim dalam bentuk array pada endpoint tunggal dengan respons HTTP 400 yang terstruktur, bukan error server 500.
    - Menggunakan helper validasi berkas `isAllowedFile` untuk memeriksa ekstensi berkas yang diizinkan (`ALLOWED_DOCUMENT_EXTENSIONS`) dan MIME type (`ALLOWED_DOCUMENT_MIME_TYPES`).
    - Memastikan seluruh berkas sementara (`tempFilePath`) selalu dibersihkan secara aman melalui blok `finally` (`cleanTempFiles`), mencegah kebocoran ruang penyimpanan disk pada server.
  - **Pengujian Integrasi Hardening Partner API**:
    - Menambahkan pengujian integrasi otomatis pada `partnerApiCustomer.hardening.test.js` untuk memastikan proteksi input, penolakan payload tidak valid, dan pembersihan file sementara berjalan sesuai spesifikasi.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #487  | Diagnostik Koneksi Google Drive dan Peningkatan Keandalan Aplikasi | Admin dapat menguji dan mendiagnosa izin serta kapasitas Google Drive secara langsung, pemulihan akun terkunci kuota basi, peningkatan keamanan sandi, dan proteksi dari siklus reload saat pembaruan aplikasi. |
| #479  | Pantauan Langsung Tagihan dan Pembaruan Halaman Pembayaran Online | Staf penagihan dapat memantau secara langsung kehadiran pelanggan di link invoice, mengetahui metode yang sedang dipilih, serta tampilan halaman pembayaran publik yang lebih nyaman dan praktis. |
| #480  | Penguatan Validasi dan Keamanan Integrasi Pelanggan Mitra | Mengamankan jalur integrasi data pelanggan mitra dari input berkas yang salah, menjamin kebersihan file temporer, serta melindungi integritas data pelanggan antar sistem. |

### Kemampuan Baru Pengguna/Admin

- **Uji Koneksi & Diagnosa Google Drive**: Administrator dapat menekan tombol "Uji Koneksi" dan "Diagnosa" di pengaturan cadangan data untuk memverifikasi apakah akun Google Drive sudah memiliki izin file yang memadai, token aktif, dan sisa kapasitas penyimpanan yang cukup sebelum jadwal pencadangan berjalan.
- **Pemulihan Akun Terkunci Kuota Basi**: Staf teknis dapat menggunakan alat pemulihan di menu Pengembang untuk melepaskan status kunci kuota lama yang tertinggal pada akun jaringan.
- **Sensor Sandi Sensitif**: Seluruh tampilan informasi sandi pada antarmuka admin kini terlindungi oleh sensor bawaan dengan tombol intip dan salin, menjaga kerahasiaan dari pihak lain di sekitar layar kerja.
- **Pantauan Langsung Aktivitas Tagihan**: Staf penagihan dapat melihat apakah pelanggan sedang membuka link tagihan, sedang membuka aplikasi bank/e-wallet, atau sedang melihat petunjuk transfer, lengkap dengan log aktivitas 24 jam terakhir.
- **Salin Kode Bayar / VA Praktis**: Pelanggan pada tautan pembayaran publik kini dapat menyalin kode bayar atau nomor Virtual Account dengan satu kali sentuhan tombol.

### Bug Fix / Solusi Masalah

- **Pencegahan Error 500 pada Unggah Dokumen Mitra**: Memperbaiki kegagalan sistem saat API mitra mengirimkan berkas dalam format array pada endpoint unggahan dokumen tunggal; kini ditolak secara rapi dengan status HTTP 400.
- **Pembersihan File Sementara**: Menjamin seluruh file sementara hasil unggahan dokumen pelanggan langsung dihapus dari disk setelah proses selesai, mencegah disk server penuh akibat penumpukan file temporer.
- **Mitigasi Infinite Reload Loop PWA**: Mencegah browser pengguna melakukan siklus muat ulang berulang (*reload loop*) saat terjadi pembaruan rilis aplikasi web melalui deteksi cerdas kegagalan muat modul chunk dan sanitasi cache lokal.
- **Pencegahan Gagal Backup Akun Google Drive**: Memberikan petunjuk jelas kepada admin jika izin akses berkas Google Drive belum dicentang pada saat proses otorisasi akun.

### Menu/Fitur Baru

- **Modal Diagnosa Google Drive**: Tersedia di menu **Pengaturan > Developer > Cadangan Database** pada kartu koneksi Google Drive.
- **Alat Pemulihan Akun Kuota Kedaluwarsa**: Tersedia di menu **Pengaturan > Developer > Alat Pemulihan Sistem**.
- **Panel Pantauan Langsung (Live Invoice)**: Tersedia pada halaman rincian faktur di menu **Keuangan > Tagihan & Faktur**.
- **Tampilan Baru Instruksi Pembayaran Online**: Tampil pada halaman tautan tagihan publik yang diakses oleh pelanggan.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Diagnostik dan Uji Koneksi Google Drive untuk Pencadangan Database

- **Penjelasan Fitur**:
  - Fitur ini memberikan visibilitas penuh kepada admin mengenai status kesehatan integrasi Google Drive yang digunakan untuk menyimpan cadangan basis data otomatis.
  - Sistem memeriksa 4 poin penting: validitas token otentikasi Google, izin akses berkas (`drive.file`), keterjangkauan folder tujuan pencadangan, dan sisa kuota ruang penyimpanan Google Drive.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi admin, lalu buka menu **Pengaturan > Developer**.
  2. Pilih tab **Cadangan Database (DB Tools)**.
  3. Pada kartu **Koneksi Google Drive**:
     - Klik tombol **"Uji Koneksi"** untuk pengecekan cepat kesiapan integrasi.
     - Klik tombol **"Diagnosa"** untuk membuka jendela rincian status pemeriksaan sistem secara mendalam.
  4. Periksa hasil pengujian:
     - Jika terdapat status peringatan atau gagal (misalnya izin berkas belum dicentang), ikuti petunjuk solusi yang tertera di modal.
     - Jika ingin mengganti akun atau memperbarui izin, klik tombol **"Hubungkan Ulang Akun"** dan pastikan mencentang seluruh izin akses yang diminta Google pada layar persetujuan.

---

### 2. Memantau Aktivitas Pelanggan Melalui Panel Pantauan Langsung Tagihan (Live Invoice)

- **Penjelasan Fitur**:
  - Membantu tim penagihan (*finance / billing*) mengetahui apakah tautan pembayaran yang dikirimkan via WhatsApp atau email benar-benar sudah dibuka oleh pelanggan.
  - Mendeteksi secara langsung saat pelanggan sedang berada di halaman pemilihan metode pembayaran atau sedang membuka aplikasi pembayaran di ponselnya.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Keuangan > Tagihan & Faktur**.
  2. Pilih salah satu faktur yang masih berstatus belum lunas (*unpaid*), lalu klik untuk membuka rincian faktur.
  3. Perhatikan panel **"Pantauan Langsung Tagihan"** pada bagian samping/atas rincian:
     - Status **"Belum ada pelanggan yang membuka link pembayaran"**: Pelanggan belum mengklik tautan tagihan.
     - Status **"Pelanggan sedang melihat tagihan"**: Pelanggan sedang aktif membaca rincian faktur di perambannya.
     - Status **"Halaman pelanggan di latar belakang"**: Pelanggan meminimalkan browser, kemungkinan sedang berpindah ke aplikasi m-Banking atau e-Wallet untuk membayar.
     - Status **"Pelanggan sedang melihat instruksi pembayaran"**: Pelanggan sudah memilih metode dan sedang membaca kode bayar/VA.
  4. Klik bagian **"Log aktivitas"** untuk melihat rekam jejak interaksi pelanggan dalam 24 jam terakhir.
