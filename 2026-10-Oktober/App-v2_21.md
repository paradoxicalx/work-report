# 📝 Daily Work Report - Dedy S.N Putra (21 Oktober 2026)

---

## 📅 Laporan Harian - 21 Oktober 2026

---

## 🌿 Branch: `issue-444` — Issue #444: Penataan dan Pemetaan Kunci Antar Microservice (Service Keys Management & Architecture)

### 📌 Informasi Issue

- **Nomor Issue**: #444
- **Judul Issue**: Penataan dan Pemetaan kunci antar microservice
- **Status Branch**: `Belum di-merge` (Branch kerja `issue-444`, sinkron dengan `origin/issue-444`; seluruh implementasi backend, shared library, integrasi microservice, UI frontend developer tab, serta suite unit/integration test telah selesai secara komprehensif dan disatukan via commit `d6c3352f` dengan riwayat pre-squash tersimpan di `backup/issue-444-pre-squash`)

### 📅 Rincian Commit

#### [d6c3352f] - resolve #444 - Rabu, 21 Oktober 2026, 04:47:42 WIB

_(Commit final resolusi Issue #444 yang menggabungkan seluruh implementasi lanjutan dari rangkaian 15 commit pre-squash `2639860e` s/d `8c42d8ee` pada 21 Oktober 2026 dini hari)_

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `acs/ext/informWebhook.cjs`
  - `acs/test/informWebhook.header.test.js` [NEW]
  - `backend/src/middlewares/auth.middleware.js`
  - `backend/src/routes/public.route.js`
  - `baileys-api/.env.example`
  - `baileys-api/src/utils/logger.js`
  - `baileys-api/src/utils/serviceKeys.core.js`
  - `baileys-api/src/utils/serviceKeys.js`
  - `baileys-api/test/fatalReport.test.js` [NEW]
  - `cron-worker/.env.example`
  - `cron-worker/src/utils/serviceKeys.core.js`
  - `frontend/src/app/pages/settings/schema/systemSchema.js`
  - `frontend/src/app/pages/settings/sections/Developer.jsx`
  - `frontend/src/app/pages/settings/sections/System.jsx`
  - `frontend/src/app/pages/settings/sections/developer/CronWorkerTab.jsx`
  - `frontend/src/app/pages/settings/sections/developer/serviceKeys/PartyRow.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/serviceKeys/RevealValue.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/serviceKeys/RotateKeyModal.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/serviceKeys/ServiceKeyCard.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/serviceKeys/ServiceKeysTab.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/serviceKeys/StatusBadge.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/serviceKeys/i18nKeys.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/serviceKeys/utils.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/serviceKeys/utils.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/axios.js`
  - `frontend/src/utils/networkLogRedaction.js` [NEW]
  - `frontend/src/utils/networkLogRedaction.test.js` [NEW]
  - `network-monitor/.env.example`
  - `network-monitor/src/utils/logger.js`
  - `network-monitor/src/utils/serviceKeys.core.js`
  - `network-monitor/src/utils/serviceKeys.js`
  - `network-monitor/test/fatalReport.test.js` [NEW]
  - `syslog-server/.env.example`
  - `syslog-server/src/middlewares/auth.middleware.js`
  - `syslog-server/src/middlewares/requireApiKey.factory.js` [NEW]
  - `syslog-server/src/server.js`
  - `syslog-server/src/services/backendNotifier.service.js`
  - `syslog-server/src/utils/logger.js`
  - `syslog-server/src/utils/serviceKeys.core.js`
  - `syslog-server/src/utils/serviceKeys.js`
  - `syslog-server/test/unit/backendNotifier.test.js` [NEW]
  - `syslog-server/test/unit/fatalReport.test.js` [NEW]
  - `syslog-server/test/unit/requireApiKey.factory.test.js` [NEW]
  - `telegram-api/.env.example`
  - `telegram-api/src/config.js`
  - `telegram-api/src/server.js`
  - `telegram-api/src/utils/logger.js`
  - `telegram-api/src/utils/serviceKeys.core.js`
  - `telegram-api/src/utils/serviceKeys.js`
  - `telegram-api/test/fatalReport.test.js` [NEW]
  - `tools/service-keys/serviceKeys.core.js`
  - `tools/service-keys/serviceKeys.core.test.js`
  - `tools/service-keys/wiring.test.js` [NEW]
  - `whatsapp-api/.env.example`
  - `whatsapp-api/src/utils/logger.js`
  - `whatsapp-api/src/utils/serviceKeys.core.js`
  - `whatsapp-api/src/utils/serviceKeys.js`
  - `whatsapp-api/test/fatalReport.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Integrasi Service Keys Penuh pada Syslog Server (`syslog-server`)**:
    - Mengintegrasikan loader `serviceKeys.js` yang otomatis menyinkronkan kunci layanan langsung dari koleksi MongoDB `system_settings` dengan fallback aman ke environment variable `INTERNAL_API_KEY`.
    - Mengganti middleware autentikasi lama dengan pabrik middleware `requireApiKey.factory.js` berbasis perbandingan *timing-safe* (`timingSafeEqual`) untuk rute proteksi inter-service.
    - Menambahkan rute `GET /key-check` untuk inspeksi status sinkronisasi kunci layanan secara langsung oleh Backend.
    - Memperbarui `backendNotifier.service.js` agar menyertakan header `x-api-key` yang valid saat mengirim batch event log ke Backend Dekasimal.
  - **Autentikasi Header Webhook Inform ACS (`acs/ext/informWebhook.cjs`)**:
    - Menambahkan penyematan header `x-api-key` pada pengiriman webhook event Inform CPE dari GenieACS ke Backend Dekasimal agar lolos verifikasi kunci layanan internal.
    - Menambahkan unit test `informWebhook.header.test.js` untuk memvalidasi keberadaan dan format header.
  - **Antarmuka Pengembang Manajemen Kunci Layanan (Frontend Developer Tab)**:
    - Membangun tab khusus "Kunci Layanan" (`ServiceKeysTab.jsx`) di dalam menu Pengaturan Pengembang (`Developer.jsx`):
      - `ServiceKeyCard.jsx`: Menampilkan visualisasi kartu untuk setiap kunci layanan terdaftar beserta ringkasan status aktif, masa tenggang (*grace period*), dan status sinkronisasi microservice terkait.
      - `PartyRow.jsx`: Menampilkan daftar microservice pembuat (*source*) dan konsumen (*receiver*) beserta indikator status koneksi aktual.
      - `RevealValue.jsx`: Komponen interaktif untuk melihat (*peek/reveal*) nilai kunci layanan dengan proteksi sensor, copy-to-clipboard, dan audit log tanpa membocorkan nilai ke log konsol.
      - `RotateKeyModal.jsx`: Modal proses rotasi kunci layanan secara aman dengan opsi grace period 24 jam agar microservice yang belum di-restart tetap dapat bertransaksi.
      - `StatusBadge.jsx`: Indikator status berwarna (*Active*, *Grace Period*, *Mismatch*, *Offline*).
      - Utilitas: `utils.js`, `utils.test.js`, dan `i18nKeys.test.js` untuk parsing label, i18n, dan status pemetaan.
    - Membersihkan input kunci layanan manual lama dari antarmuka tab `System.jsx` dan skema validasi `systemSchema.js` guna mencegah inkonsistensi konfigurasi.
    - Mengintegrasikan modul proteksi redaksi log jaringan browser developer `networkLogRedaction.js` dan unit test-nya untuk menjamin nilai kunci tidak tersingkap pada dev tools browser.
  - **Pengujian Ketahanan Fatal Error Reporting & Microservice Wiring Test**:
    - Mengimplementasikan pengujian `fatalReport.test.js` di seluruh microservice (`baileys-api`, `network-monitor`, `syslog-server`, `telegram-api`, dan `whatsapp-api`) untuk memastikan kegagalan fatal tertangani dan tercatat dengan benar.
    - Membangun `tools/service-keys/wiring.test.js` (305 baris test) yang secara otomatis memindai seluruh direktori microservice untuk memverifikasi loader `serviceKeys.js`, factory middleware, endpoint `GET /key-check`, dan fallback env masing-masing.

---

### 📂 Rincian Rangkaian Commit Pre-Squash (`backup/issue-444-pre-squash`)

Berikut rincian commit bertahap sebelum disatukan menjadi commit resolusi `d6c3352f`:

1. **[8c42d8ee]** - _save #444: Polish service keys UI, redaksi log jaringan developer, dan unit test i18n_ (21 Okt 2026, 04:42:59 WIB)
   - Penyempurnaan `RevealValue.jsx`, `RotateKeyModal.jsx`, `ServiceKeyCard.jsx`, dan penambahan pengujian `networkLogRedaction.test.js` serta `i18nKeys.test.js`.
2. **[257cc59c]** - _save #444: Penyesuaian dokumentasi aturan AGENTS.md_ (21 Okt 2026, 01:36:58 WIB)
3. **[1adb0c85]** - _save #444: Pembersihan field kunci lama dari form System settings & CronWorkerTab_ (21 Okt 2026, 01:34:01 WIB)
   - Menghapus field kunci manual dari `System.jsx` dan `systemSchema.js` serta merapikan `CronWorkerTab.jsx`.
4. **[14c1b92b]** - _save #444: Komponen RevealValue & RotateKeyModal dengan drawer ServiceKeysTab_ (21 Okt 2026, 01:30:28 WIB)
5. **[4f08abb7]** - _save #444: Komponen PartyRow, ServiceKeyCard, dan ServiceKeysTab di menu Pengaturan Pengembang_ (21 Okt 2026, 01:26:33 WIB)
6. **[c651d7d2]** - _save #444: Penambahan i18n key bahasa Indonesia dan Inggris untuk Service Keys UI_ (21 Okt 2026, 01:23:31 WIB)
7. **[2e422c50]** - _save #444: Utilitas pembantu format status service keys & unit test_ (21 Okt 2026, 01:21:18 WIB)
8. **[1f31f6de]** - _save #444: Dokumentasi lanjutan aturan arsitektur service keys di AGENTS.md_ (21 Okt 2026, 01:14:13 WIB)
9. **[2e935556]** - _save #444: Comprehensive wiring integration test lintas microservice_ (21 Okt 2026, 01:13:36 WIB)
   - Penambahan `tools/service-keys/wiring.test.js` memvalidasi arsitektur di seluruh direktori microservice.
10. **[962d0637]** - _save #444: Sinkronisasi .env.example untuk seluruh microservice_ (21 Okt 2026, 01:02:16 WIB)
11. **[cb4c98b0]** - _save #444: Penyesuaian backendNotifier syslog server & unit test_ (21 Okt 2026, 00:58:01 WIB)
12. **[b53f94c6]** - _save #444: Fatal report error handling & tests di seluruh microservice_ (21 Okt 2026, 00:58:01 WIB)
13. **[7b359b53]** - _save #444: Refactoring core serviceKeys reader & penanganan backward-compatibility_ (21 Okt 2026, 00:55:52 WIB)
14. **[a7c6e96c]** - _save #444: Integrasi header x-api-key pada GenieACS informWebhook & unit test_ (21 Okt 2026, 00:52:48 WIB)
15. **[2639860e]** - _save #444: Syslog Server service keys loader, requireApiKey factory, & route key check_ (21 Okt 2026, 00:49:56 WIB)

---

## 🌿 Branch: `master` / `production` — Issue #450: Perbaikan Pemangkasan Spasi Nama Pelanggan/Mitra, Payload Payment Gateway iPaymu, Kontrol Riwayat Sesi RADIUS Dashboard, & Standarisasi Tooltip UI

### 📌 Informasi Issue

- **Nomor Issue**: #450
- **Judul Issue**: Perbaikan pemangkasan spasi nama pelanggan dan mitra, payload payment gateway iPaymu, kontrol riwayat aktivitas sesi RADIUS dashboard, serta standarisasi tooltip antarmuka
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Rabu, 21 Oktober 2026, 04:22:07 WIB via commit `94ecc79c`, di-merge ke branch `production` via commit `e76bfebd`, dan issue GitHub #450 resmi ditutup / `CLOSED`)

### 📅 Rincian Commit

#### [e76bfebd] - Merge branch 'master' into production - Rabu, 21 Oktober 2026, 04:22:52 WIB

_(Merge commit menyinkronkan branch `master` terbaru yang memuat penyelesaian Issue #450 langsung ke branch `production`)_

---

#### [94ecc79c] - resolve #450 - Rabu, 21 Oktober 2026, 04:22:07 WIB

_(Merge commit menggabungkan commit penyelesaian `86e18ff7` ke branch `master`)_

---

#### [86e18ff7] - resolve #450 - Rabu, 21 Oktober 2026, 04:21:30 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/customer.controller.js`
  - `backend/src/controllers/isolirBatch.controller.js`
  - `backend/src/controllers/publicPayment.controller.js`
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/controllers/radiusControl.controller.js`
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/customer.model.js`
  - `backend/src/models/partner.model.js`
  - `backend/src/routes/customer.route.js`
  - `backend/src/routes/radiusControl.route.js`
  - `backend/src/services/invoiceFreeze.service.js`
  - `backend/src/services/paymentGateways/ipaymu.gateway.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/src/services/radiusControl.service.js`
  - `backend/src/services/radiusStats.service.js`
  - `backend/src/services/recovery.service.js`
  - `backend/src/utils/ipaymuPayload.js` [NEW]
  - `backend/test/integration/ipaymuGateway.createPayment.test.js` [NEW]
  - `backend/test/integration/nameTrim.model.test.js` [NEW]
  - `backend/test/integration/nameTrimRecovery.test.js` [NEW]
  - `backend/test/integration/radiusStats.service.test.js`
  - `backend/test/unit/ipaymuPayload.test.js` [NEW]
  - `backend/test/unit/radiusControl.batch.test.js` [NEW]
  - `backend/test/unit/radiusControl.clearUserActivity.test.js` [NEW]
  - `backend/test/unit/radiusControl.disconnectBestEffort.test.js` [NEW]
  - `backend/test/unit/radiusControl.shouldDisconnect.test.js` [NEW]
  - `frontend/src/app/pages/dashboards/radius/components/ActiveSessionsTable.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/PppoeDisconnectPanel.jsx`
  - `frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx`
  - `frontend/src/app/pages/dashboards/radius/index.jsx`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/app/pages/users/business/edit.jsx`
  - `frontend/src/app/pages/users/customer/edit.jsx`
  - `frontend/src/app/pages/users/partner/edit.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/main.jsx`
  - `radius-server/DOCUMENTATION.md`
  - `radius-server/cmd/radiusd/wire.go`
  - `radius-server/internal/domain/accounting/handlers.go`
  - `radius-server/internal/domain/accounting/handlers_test.go`
  - `radius-server/internal/domain/hotspot/accounting.go`
  - `radius-server/internal/domain/hotspot/accounting_test.go`
  - `radius-server/internal/domain/ports/repositories.go`
  - `radius-server/internal/repository/mongo/hotspot_session_repo.go`
  - `radius-server/internal/repository/mongo/session_repo.go`
  - `radius-server/pkg/metrics/metrics.go`
  - `radius-server/test/testutil/fake_repos.go`
- **Deskripsi Perubahan & Fungsi**:
  - **Pembersihan Spasi Nama Pelanggan & Mitra (Model Pre-Save & Recovery Tool)**:
    - Menambahkan hook sanitasi pre-save `trim()` pada schema Mongoose `Customer` dan `Partner` (`backend/src/models/customer.model.js`, `partner.model.js`) untuk memangkas spasi liar di awal dan akhir nama.
    - Mengatasi masalah tanda tangan tidak valid (*unauthorized signature*) pada transaksi payment gateway iPaymu akibat nama pelanggan yang memuat spasi ekstra di awal/akhir string.
    - Membangun service pemulihan data `customerNameTrim` pada `recovery.service.js` lengkap dengan mode simulasi (*dry-run*) dan mode eksekusi nyata untuk membersihkan data lama di database (termasuk pelanggan dan mitra yang dihapus lunak / *soft-deleted*).
    - Mendaftarkan rute recovery di `recoveryRoutes.config.js` dan menambahkan suite test `nameTrim.model.test.js` serta `nameTrimRecovery.test.js`.
  - **Standardisasi Payload & Hash Signature iPaymu Payment Gateway (`ipaymuPayload.js`, `ipaymu.gateway.js`)**:
    - Membangun modul khusus `backend/src/utils/ipaymuPayload.js` untuk konstruksi, sanitasi, dan pengurutan payload pembayaran iPaymu secara deterministik.
    - Mengintegrasikan modul ke dalam `ipaymu.gateway.js` dan rute pembayaran publik `publicPayment.controller.js` sehingga menghasilkan *signature hash* yang selalu valid dan konsisten.
    - Menambahkan pengujian komprehensif: `ipaymuPayload.test.js` dan `ipaymuGateway.createPayment.test.js`.
  - **Fitur Hapus Riwayat Aktivitas & Optimasi Accounting RADIUS (`radiusControl`, `radius-server`)**:
    - **Endpoint Hapus Riwayat Aktivitas**: Menambahkan rute `POST /radius-control/user-activity/clear` yang memungkinkan admin menghapus catatan log aktivitas 24 jam terakhir dari `radius_logs`, baik untuk satu pengguna spesifik (`username`) maupun seluruh riwayat berdasarkan tipe (`success`/`failed`).
    - **Penyempurnaan Panel Dashboard Radius**: Memperbarui komponen `UserActivityPanel.jsx` dengan tombol "Hapus Riwayat Permanen", modal konfirmasi interaktif `clearConfirmTitle`/`clearUserConfirmTitle`, serta penyesuaian tampilan durasi waktu pada `LiveLogPanel.jsx`, `ActiveSessionsTable.jsx`, dan `PppoeDisconnectPanel.jsx`.
    - **Optimasi Radius Server Go (`radius-server`)**:
      - Mengimplementasikan metode `UpdateInterim` pada repository sesi PPPoE dan Hotspot (`session_repo.go`, `hotspot_session_repo.go`).
      - Menjamin paket *Accounting-Interim* tidak membangkitkan kembali (*resurrect*) sesi yang sudah ditutup (*Accounting-Stop*), dan melakukan *upsert* aman jika paket *Accounting-Start* sebelumnya hilang di transmisi UDP.
      - Mengekspos metrik Prometheus baru `radius_session_interim_write_errors_total` untuk memantau kegagalan penulisan sesi interim.
      - Menambahkan pengujian unit handler dan repository di `handlers_test.go` dan `accounting_test.go`.
  - **Standarisasi Tooltip Seluruh Antarmuka (Aturan #25 AGENTS.md)**:
    - Merumuskan aturan baku arsitektur frontend #25 di `AGENTS.md`: melarang keras penggunaan atribut tooltip HTML mentah (`title="..."`) pada tombol, ikon, badge, dan teks terpotong.
    - Seluruh tooltip diwajibkan menggunakan sistem terpusat `react-tooltip` dengan atribut `data-tooltip` dan `data-tooltip-content="..."`.
    - Menerapkan standarisasi ini pada formulir edit pengguna (`business/edit.jsx`, `customer/edit.jsx`, dan `partner/edit.jsx`).

---

## 🌿 Pekerjaan Belum Di-commit (Work In Progress)

### 📌 Komponen: `frontend/src/app/pages/settings/sections/OperationalSettings.jsx`

- **Status**: Perubahan lokal di branch aktif `issue-444`.
- **Perubahan**:
  - Mengganti komponen form `InputDefault` menjadi `InputAppend`.
  - Mengubah prop `unit={unit}` menjadi `append={unit}` pada fungsi helper `renderField`.
- **Tujuan**: Memastikan label satuan metrik operasional (misal `%`, `ms`, `Mbps`) tampil rapi di sebelah kanan kolom input angka pada pengaturan operasional dashboard.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #444  | Penataan dan Pemetaan Kunci Antar Microservice (Service Keys Management) | Seluruh microservice (`backend`, `network-monitor`, `syslog-server`, `telegram-api`, `whatsapp-api`, `baileys-api`, `cron-worker`, `acs`) kini terotentikasi secara dinamis dengan rotasi kunci aman, grace period 24 jam, timing-safe validation, serta antarmuka visual terpadu bagi pengembang di Pengaturan Developer. |
| #450  | Perbaikan Spasi Nama Pelanggan/Mitra, Payload iPaymu, Kontrol Riwayat RADIUS & Tooltip UI | Mencegah dan memulihkan kegagalan pembayaran online akibat spasi nama; menambah fitur pembersihan log aktivitas sesi RADIUS; mengoptimalkan akurasi status sesi accounting interim pada RADIUS server; serta menstandarisasi tooltip UI di seluruh aplikasi. |

---

### Kemampuan Baru Pengguna/Admin

- **Manajemen Kunci Layanan Mandiri di Tab Developer**: Admin berwenang (Developer) kini dapat meninjau seluruh status kunci layanan antar-microservice, melihat kapan kunci terakhir dilihat (*sighting*), mengintip nilai kunci dengan sensor aman, dan melakukan rotasi kunci otomatis tanpa menyebabkan downtime layanan penerima berkat *grace period* 24 jam.
- **Pembersihan Riwayat Aktivitas RADIUS**: Admin jaringan di halaman Dashboard RADIUS kini dapat menghapus riwayat log aktivitas pengguna yang bermasalah (baik per pengguna individual maupun seluruh riwayat koneksi berhasil/gagal) secara permanen langsung dari antarmuka.
- **Alat Pemulihan Spasi Nama (Name Trim Recovery)**: Pengembang dan administrator dapat menjalankan simulasi (*dry-run*) dan pemulihan langsung untuk merapikan karakter spasi berlebih pada nama pelanggan, mitra, dan bisnis di database guna menjamin kelancaran transaksi payment gateway.

---

### Bug Fix / Solusi Masalah

1. **Gagal Tanda Tangan Pembayaran iPaymu (Unauthorized Signature)**: Berhasil diselesaikan melalui standardisasi modul payload `ipaymuPayload.js` serta penambahan hook otomatis sanitasi spasi `trim()` pada schema model `Customer` dan `Partner`.
2. **Sesi Zombie / Bangkit Kembali pada RADIUS Accounting**: Berhasil diselesaikan pada repository sesi `radius-server` Go (`UpdateInterim`) yang memastikan paket interim update tidak lagi menyalakan kembali flag `isOnline: true` pada sesi yang sudah memiliki `endTime`.
3. **Penyusupan Nilai Kunci Sensitif pada Console/Network Log**: Terisolasi penuh melalui modul `networkLogRedaction.js` dan sanitasi logger backend Winston.
4. **Tampilan Tooltip Browser Mentah**: Dihentikan dan distandarisasi ke komponen global `react-tooltip` via atribut `data-tooltip` dan `data-tooltip-content`.

---

### Menu/Fitur Baru

- **Tab "Kunci Layanan" (Service Keys)** di menu Pengaturan Pengembang (`/settings` → Pengembang).
- **Tombol & Modal "Hapus Riwayat Permanen"** pada panel Riwayat Aktivitas Pengguna Dashboard RADIUS (`/dashboards/radius`).
- **Alat Pemulihan "Pemangkasan Spasi Nama Pelanggan & Mitra"** di menu Pengaturan Pengembang → Recovery Tools.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Fitur Manajemen Kunci Layanan (Service Keys Tab)

- **Penjelasan Fitur**: Fitur ini memusatkan seluruh kunci komunikasi antar-layanan (seperti kunci Backend ke Telegram API, WhatsApp API, Network Monitor, Syslog Server, dsb.) ke dalam satu katalog terstruktur. Setiap kunci memiliki status real-time, catatan *sighting* terakhir, dan fasilitas rotasi aman yang memberikan tenggang waktu 24 jam bagi microservice konsumen untuk memperbarui konfigurasinya.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi sebagai admin dengan hak akses Pengembang (*Developer*).
  2. Buka menu **Pengaturan** (`Settings`) > pilih tab **Pengembang** (*Developer*).
  3. Klik sub-tab **Kunci Layanan** (*Service Keys*).
  4. Periksa kartu kunci layanan untuk melihat microservice asal (*source*) dan tujuan (*receivers*), serta status sinkronisasinya (*Active* / *Grace Period*).
  5. Untuk melihat nilai kunci: klik ikon mata (**Intip Nilai Kunci**), nilai akan ditampilkan dan dapat disalin ke clipboard.
  6. Untuk merotasi kunci: klik tombol **Rotasi Kunci**, centang opsi *Grace Period 24 Jam* (sangat direkomendasikan agar tidak terjadi pemutusan transaksi seketika), lalu konfirmasi rotasi.

---

### 2. Fitur Pembersihan Riwayat Aktivitas Pengguna pada Dashboard RADIUS

- **Penjelasan Fitur**: Panel "Riwayat Aktivitas Pengguna" pada Dashboard RADIUS menyajikan daftar pelanggan dengan percobaan koneksi PPPoE/Hotspot berhasil dan gagal terbanyak. Fitur baru ini memungkinkan admin menghapus riwayat tersebut dari basis data secara instan guna mereset metrik pencatatan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Dashboard** > pilih **RADIUS**.
  2. Gulir ke panel **Riwayat Aktivitas Pengguna** (*User Activity Panel*).
  3. Pilih tab **Koneksi Berhasil** atau **Percobaan Gagal**.
  4. **Untuk menghapus satu pengguna tertentu**: Arahkan kursor ke baris pengguna target, lalu klik tombol tempat sampah (**Hapus riwayat pengguna ini**), lalu konfirmasi pada modal yang muncul.
  5. **Untuk membersihkan seluruh kategori**: Klik tombol merah **Hapus Riwayat Permanen** di sudut kanan atas panel, baca peringatan konfirmasi, lalu klik tombol konfirmasi untuk mengeksekusi penghapusan.

---

### 3. Alat Pemulihan Pemangkasan Spasi Nama Pelanggan & Mitra

- **Penjelasan Fitur**: Alat pemulihan data untuk mendeteksi dan memotong spasi liar di ujung nama pelanggan/mitra lama yang belum tersanitasi secara otomatis.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Pengaturan** > tab **Pengembang** > pilih bagian **Recovery Tools**.
  2. Cari kartu **Pemangkasan Spasi Nama Pelanggan & Mitra** (*customerNameTrim*).
  3. Klik tombol **Pratinjau (Dry Run)** terlebih dahulu untuk melihat berapa banyak record yang terdampak tanpa mengubah data di database.
  4. Setelah meninjau hasil pratinjau, jika ingin menerapkan perubahan, ketik teks konfirmasi yang diminta dan jalankan eksekusi nyata.
