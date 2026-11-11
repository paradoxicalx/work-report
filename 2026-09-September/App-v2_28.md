# 📝 Daily Work Report - Dedy S.N Putra (2026-09-28)

---

## 📅 Laporan Harian - 28 September 2026

---

## 🌿 Branch: `issue-175` — Penyempurnaan Manajemen Jarak Jauh ONT TR-069 (GenieACS), Saklar Auto-Provisioning Granular Per Perangkat, Hashing Sidik Jari Kredensial, Optimasi Sinkronisasi State Armada, dan Pengayaan Antarmuka Pengguna

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: Implementasi Sistem Monitoring & Remote Configuration ONT TR-069 (GenieACS), Auto-Provisioning Parameter Kredensial, Matriks Kompatibilitas Model, dan Deteksi Degradasi Sinyal Optik
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-175`, mencakup commit [`b32f2561`](file:///home/dhedhy/Project/Dekasimal-V2) di remote `origin/issue-175` serta modifikasi *working tree* terkini siap commit)

---

### ⏳ Pekerjaan Belum Di-commit (Working Tree Changes)

- **Komponen yang Berubah**:
  - [`backend/src/models/acsDevice.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/acsDevice.model.js) — Menambahkan field `auto_provision_enabled: { type: Boolean, default: false }` pada skema `AcsDevice` sebagai gerbang ketiga provisioning otomatis per unit ONT; bawaannya nonaktif (*default false*) guna mencegah penulisan kredensial Connection Request dan interval Inform tanpa persetujuan eksplisit admin.
  - [`backend/src/services/acsProvision.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acsProvision.service.js) — Memperkuat fungsi `buildProvisionSignature(settings)` dengan enkripsi *one-way cryptographic hash* SHA-256 (`crypto.createHash('sha256')`) menggantikan format teks terbuka (*plaintext delimiter* `<user>|<sandi>|...`), mengeliminasi risiko kebocoran kata sandi sistem ACS ke browser klien ketika dokumen detail perangkat dibaca.
  - [`backend/src/services/acs.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acs.service.js):
    - Mengimplementasikan fungsi `setAcsDeviceAutoProvision(serialNumber, enabled)` untuk mengubah saklar provisi otomatis per perangkat secara terisolasi tanpa merusak atau menghapus nilai `provision_signature`.
    - Menambahkan helper kueri `findDeviceBySerial(input, projection)` yang mengoptimalkan pencarian serial number menggunakan indeks case-insensitive yang efisien.
    - Menambahkan helper proyeksi seleksi `tanpaBawaanInternal()` guna memastikan field internal sensitif tidak bocor pada payload respons API.
    - Mengetatkan fungsi `resolveInformProvisionPlan(payload)` dengan evaluasi `device?.auto_provision_enabled === true`; perangkat yang belum diizinkan auto-provisioning tidak akan ditulisi parameter apa pun saat mengirimkan event *Inform*.
    - Mengoptimalkan fungsi sinkronisasi armada `syncAcsStateFromGenie()` dengan membagi eksekusi `bulkWrite` ke dalam gelombang *batch* terkendali (`ACS_SYNC_BATCH_SIZE = 500`) untuk menjaga kelancaran *event loop* Node.js, serta menyematkan parameter proyeksi `params: { projection: 'device,channel,code,message,timestamp' }` pada pemanggilan `fetchGenieFaultIndex` guna memangkas konsumsi memori dan *bandwidth* dari NBI GenieACS.
  - [`backend/src/controllers/acs.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/acs.controller.js) — Menambahkan controller `setAcsDeviceAutoProvisionHandler` untuk menangani rute `PATCH /api/v1/acs/auto-provision/:serial_number` lengkap dengan validasi tipe data boolean ketat (`typeof enabled === 'boolean'`).
  - [`backend/src/routes/acs.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/acs.route.js) — Mendaftarkan rute `PATCH /acs/auto-provision/:serial_number` dengan middleware proteksi `protectedAdmin`, pengecekan hak akses `checkPrivilege('acs.update')`, serta dokumentasi Swagger/OpenAPI.
  - [`backend/src/controllers/internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internal.controller.js) — Menstandarisasi pesan error pada `receiveAcsInform` menggunakan `req.t('acs.serialNumberRequired')` agar konsisten dengan standar i18n monorepo.
  - [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx) — Membuat komponen sel tabel baru [`AcsAutoProvisionSwitchCell`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx) berbasis `StyledSwitch` yang memungkinkan admin mengubah status auto-provisioning langsung dari tabel tanpa berpindah halaman, lengkap dengan penanganan status *loading* dan validasi hak akses `acs.update`.
  - [`frontend/src/app/pages/network/acsDevices/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/schema/columns.jsx) — Mendaftarkan kolom `auto_provision_enabled` pada tabel perangkat ACS dengan *header* `acsDevice.autoConfig`.
  - [`frontend/src/components/shared/acs/OntDeviceCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/acs/OntDeviceCard.jsx) — Menambahkan baris saklar `StyledSwitch` untuk konfigurasi otomatis ONT langsung pada kartu informasi perangkat di halaman detail autentikasi/langganan broadband pelanggan.
  - Lokalisasi [`backend/src/locales/`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/) & [`frontend/src/i18n/locales/`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/) (`en` & `id`) — Mendaftarkan kunci terjemahan: `autoProvisionEnabledRequired`, `autoProvisionEnabled`, `autoProvisionDisabled`, `autoConfig`, `autoProvisionSaved`, `autoProvisionFailed`, dan `autoProvisionHint`.
  - Suite Pengujian Integrasi ([`backend/test/integration/acs.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acs.service.test.js), [`acs.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acs.controller.test.js), [`acsProvision.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acsProvision.service.test.js)) — Menambahkan uji integrasi menyeluruh untuk saklar auto-provisioning per perangkat, integritas hashing SHA-256 sidik jari provisi, pembagian *batch* `bulkWrite`, dan validasi parameter API.
  - [`.gitignore`](file:///home/dhedhy/Project/Dekasimal-V2/.gitignore) — Menambahkan pola abaikan `tmp-*.mjs` untuk mencegah file uji manual lokal terunggah ke repositori.
  - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md) — Memperbarui panduan arsitektur terkait standar logging Winston pada supervisor GenieACS dan pengecualian khusus proses pekerja ekstensi `ext/*.cjs`.
- **Deskripsi Perubahan & Fungsi**:
  - Mencegah insiden operasional berbahaya di mana firmware ONT pelanggan tertimpa konfigurasi interval Inform atau kata sandi Connection Request secara massal tanpa disengaja saat ONT baru pertama kali mengirim Inform.
  - Mengamankan penyimpanan data sidik jari provisi di MongoDB agar tidak mengekspos kredensial rahasia sistem ke antarmuka web.
  - Meningkatkan skalabilitas sistem saat memproses ribuan perangkat ONT secara berkala melalui teknik kueri berindeks dan pemotongan operasi tulis massal (*chunked bulkWrite*).

---

### 📅 Rincian Commit

#### [[`b32f2561`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #175 - 26 September 2026, 23:59:19 WIB

- **Komponen yang Berubah**:
  - **Sinkronisasi Status & Fault Armada Berkala (Cron Worker & Backend)**:
    - [`cron-worker/src/jobs/processors/acsStateSync.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/processors/acsStateSync.js) [NEW] — Menambahkan *processor* tugas berkala BullMQ untuk memicu sinkronisasi status Inform dan *fault* perangkat ACS dari GenieACS NBI ke Backend setiap 5 menit.
    - [`cron-worker/src/jobs/scheduler.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/scheduler.js) & [`worker.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/worker.js) — Mendaftarkan penjadwalan antrean job `acsStateSyncJob`.
    - [`cron-worker/src/services/api.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/services/api.service.js) — Menambahkan metode pemanggilan internal `syncAcsState()` ke endpoint backend.
    - [`backend/src/routes/internal.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/internal.route.js) & [`backend/src/controllers/internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internal.controller.js) — Mengekspos endpoint internal aman `POST /internal/acs/sync-state` dengan autentikasi `INTERNAL_API_KEY`.
    - [`backend/src/services/acs.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acs.service.js) — Mengimplementasikan logika `syncAcsStateFromGenie()` untuk menarik data `_lastInform` dan mendeteksi penumpukan kegagalan sesi CWMP (*fault*) pada perangkat secara massal.
  - **Reboot Perangkat Jarak Jauh (Remote Reboot via TR-069)**:
    - [`backend/src/services/acs.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acs.service.js) — Mengimplementasikan fungsi `rebootAcsDevice(serialNumber)` yang menerbitkan tugas RPC TR-069 `Reboot` ke GenieACS NBI dan memicu *Connection Request* ke ONT.
    - [`backend/src/controllers/acs.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/acs.controller.js) — Menambahkan handler `rebootAcsDeviceHandler`.
    - [`backend/src/routes/acs.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/acs.route.js) — Mendaftarkan rute `POST /api/v1/acs/reboot/:serial_number` yang dilindungi privilege `acs.update`.
  - **Struktur Data Model & Pelacakan Fault**:
    - [`backend/src/models/acsDevice.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/acsDevice.model.js) — Menambahkan field `last_fault`, `fault_count`, dan `fault_cleared_at` pada skema `AcsDevice` untuk memantau perangkat yang mengalami kegagalan eksekusi perintah CWMP.
  - **Antarmuka Pengguna & Diagnostik**:
    - [`frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx) — Menambahkan tombol aksi Reboot Jarak Jauh dengan modal konfirmasi `ConfirmModal`, indikator visual riwayat *fault*, serta tombol aksi pintas penanganan kendala modem.
    - [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx) — Memperbarui sel tabel perangkat ACS untuk menampilkan lencana peringatan saat ONT mengalami *fault*.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan kemampuan kepada staf NOC untuk melakukan restart/reboot perangkat modem pelanggan dari jarak jauh langsung melalui aplikasi web tanpa perlu mengunjungi lokasi fisik pelanggan atau membuka antarmuka GUI modem.
  - Menghadirkan mekanisme deteksi otomatis terhadap perangkat-perangkat ONT yang mengalami kegagalan konfigurasi (*fault*) secara berkala setiap 5 menit melalui integrasi BullMQ Worker.

---

## 🌿 Branch: `master` — Pembaruan Berkas Riwayat Rilis & Publikasi Catatan Perubahan Sistem (Changelog)

### 📌 Informasi Issue

- **Nomor Issue**: N/A (Release / Documentation Maintenance)
- **Judul Issue**: Pembaruan Indeks Changelog & Publikasi Berkas Rilis Versi Produksi
- **Status Branch**: `Sudah di-merge` (Branch `master` dan `production`)

### 📅 Rincian Commit

#### [[`6abc2a7e`](file:///home/dhedhy/Project/Dekasimal-V2)] - update changelog - 28 September 2026, 01:06:04 WIB

- **Komponen yang Berubah**:
  - [`backend/src/data/changelog/index.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/index.json) — Memperbarui indeks rilis changelog pusat sistem dengan meregistrasikan 9 berkas rilis baru beserta metadata versi.
  - [`backend/src/data/changelog/releases/issue-304.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-304.json) [NEW]
  - [`backend/src/data/changelog/releases/issue-326.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-326.json) [NEW]
  - [`backend/src/data/changelog/releases/issue-329.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-329.json) [NEW]
  - [`backend/src/data/changelog/releases/issue-330.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-330.json) [NEW]
  - [`backend/src/data/changelog/releases/issue-332.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-332.json) [NEW]
  - [`backend/src/data/changelog/releases/issue-333.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-333.json) [NEW]
  - [`backend/src/data/changelog/releases/issue-335.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-335.json) [NEW]
  - [`backend/src/data/changelog/releases/issue-337.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-337.json)
  - [`backend/src/data/changelog/releases/issue-340.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-340.json) [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Menyinkronkan catatan rilis pada menu changelog aplikasi agar pengguna dan administrator dapat membaca rincian pembaruan fitur, perbaikan bug, dan peningkatan sistem dari versi-versi yang baru dirilis.

---

## 🌿 Branch: `issue-340` — Penambahan Kolom Wilayah Pelanggan & Filter Pencarian Layanan Broadband

### 📌 Informasi Issue

- **Nomor Issue**: #340
- **Judul Issue**: Penambahan Kolom Wilayah Pelanggan & Filter Pencarian Layanan Broadband
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` melalui commit [`1e97480c`](file:///home/dhedhy/Project/Dekasimal-V2) dan [`b16b136a`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`b16b136a`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #340 - 28 September 2026, 00:39:56 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/radiusAuthentication.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthentication.controller.js) — Menambahkan penanganan kueri pencarian multi-kolom wilayah (Area, Provinsi, Kabupaten/Kota) pada parameter pencarian datatable autentikasi radius; mengimplementasikan sanitasi karakter khusus regex guna mencegah kegagalan pemfilteran saat input mengandung simbol tertentu.
  - [`backend/src/services/radiusAuthentication.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusAuthentication.service.js) — Menyesuaikan kueri agregasi dan pencarian relasi data pelanggan untuk memproyeksikan field wilayah tempat tinggal pelanggan.
  - [`frontend/src/app/pages/services/broadband/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/schema/columns.jsx) — Menambahkan definisi kolom Area, Provinsi, dan Kabupaten/Kota pada tabel Layanan Broadband (`TanStack Table`) dengan konfigurasi `filter: "text"` dan visibilitas fleksibel.
  - [`frontend/src/app/pages/users/customer/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customer/schema/columns.jsx) — Menambahkan kolom Area pada tabel data Pelanggan.
- **Deskripsi Perubahan & Fungsi**:
  - Memudahkan staf administrasi dan *Customer Care* dalam mengidentifikasi persebaran lokasi geografis pelanggan langsung pada tabel Layanan Broadband dan tabel Pelanggan tanpa harus membuka rincian profil satu per satu.
  - Menyediakan filter pencarian data yang lebih kaya sehingga pengguna dapat menyaring pelanggan berdasarkan area kerja atau domisili kabupaten/kota secara instan.

---

## 🌿 Branch: `issue-332` — Sinkronisasi Rekonsiliasi Poin Admin & Akumulasi Poin Dinamis

### 📌 Informasi Issue

- **Nomor Issue**: #332
- **Judul Issue**: Sinkronisasi Rekonsiliasi Poin Admin & Akumulasi Poin Dinamis
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` melalui commit [`48e7fa6b`](file:///home/dhedhy/Project/Dekasimal-V2) dan [`e6450e23`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`e6450e23`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #332 - 28 September 2026, 00:24:10 WIB

- **Komponen yang Berubah**:
  - **Layanan Pemulihan & Rekonsiliasi Poin (Backend)**:
    - [`backend/src/services/recovery.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/recovery.service.js) — Mengembangkan modul rekonsiliasi total poin admin yang mencocokkan nilai total poin pada dokumen akun admin dengan hasil akumulasi sebenarnya dari seluruh catatan riwayat poin bulanan. Menghasilkan ringkasan audit terperinci berupa daftar akun yang dikoreksi serta selisih nilai poin sebelum dan sesudah sinkronisasi.
    - [`backend/src/controllers/admin.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/admin.controller.js) & [`backend/src/routes/admin.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/admin.route.js) — Menambahkan endpoint pemulihan `POST /api/v1/admin/sync-points` yang diproteksi privilege khusus administrator.
    - [`backend/src/config/privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json) — Memperbarui kamus hak akses sistem untuk mendaftarkan izin akses fungsi pemulihan poin.
    - [`backend/src/locales/`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/) (`en` & `id`) — Menambahkan terjemahan pesan status hasil rekonsiliasi poin.
  - **Telegram Mini App (TWA)**:
    - [`telegram-apps/src/pages/MyPoint.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/MyPoint.jsx) — Mengubah kalkulasi akumulasi poin dan level teknisi/karyawan agar dihitung secara dinamis dari riwayat perolehan poin yang tercatat di database, mengeliminasi selisih antara nilai di halaman mini app dengan kartu profil.
  - **Pengaturan & Antarmuka Alat Pemulihan (Frontend)**:
    - [`frontend/src/app/pages/settings/recoveryTools/recoveryRoutes.config.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/recoveryTools/recoveryRoutes.config.js) — Mendaftarkan menu aksi "Sinkronisasi Poin Admin" pada halaman Pengaturan Alat Pemulihan Sistem.
    - [`frontend/src/app/pages/activities/scheduler/`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/) (`create.jsx`, `edit.jsx`, `detail.jsx`) — Menyempurnakan pemetaan data pelaksana dan penanganan tiket yang belum terjadwal.
    - [`frontend/src/app/pages/archive/partnerCard/`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/partnerCard/) (`PartnerCardModal.jsx`, `PartnerCardPreview.jsx`, `detail.jsx`) — Memperbaiki visibilitas bidang kartu rekanan.
  - **Pengujian Integrasi**:
    - [`backend/test/integration/syslog.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslog.service.test.js) & [`syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) — Memperluas pengujian integrasi analisis log AI dan memperbaiki *flaky test*.
- **Deskripsi Perubahan & Fungsi**:
  - Menyelesaikan masalah ketidaksesuaian saldo poin karyawan/admin akibat inkonsistensi data lama atau mutasi poin yang tidak terakumulasi ke profil utama.
  - Memberikan alat mandiri bagi administrator sistem untuk menjalankan perbaikan data poin secara aman dan terverifikasi melalui menu Pemulihan Sistem.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #175  | Manajemen Jarak Jauh ONT TR-069 & Auto-Provisioning Granular | Perlindungan multi-tingkat auto-provisioning ONT per perangkat, pengamanan sidik jari sandi via hashing SHA-256, optimasi sinkronisasi armada GenieACS, dan penyediaan saklar instan di tabel serta kartu detail perangkat pelanggan. |
| #340  | Kolom Wilayah Pelanggan & Filter Pencarian Broadband | Tampilan tabel Layanan Broadband dan tabel Pelanggan kini memuat info Area, Provinsi, dan Kabupaten/Kota dengan dukungan pencarian multi-kolom yang aman dari karakter khusus. |
| #332  | Sinkronisasi Rekonsiliasi Poin Admin & Akumulasi Poin Dinamis | Penyediaan fitur rekonsiliasi poin admin pada menu Alat Pemulihan dan penghitungan poin serta level dinamis di Telegram Mini App. |
| Master| Publikasi Catatan Perubahan (Changelog Releases) | Dokumentasi rilis resmi yang mencakup pembaruan versi v1.82.2 (issue #332) dan v1.82.3 (issue #340) dapat dibaca langsung oleh pengguna. |

### Kemampuan Baru Pengguna/Admin

- **Kontrol Auto-Provisioning ONT Presisi**: Admin kini dapat secara spesifik memilih perangkat ONT mana saja yang diizinkan untuk menerima pembaruan kredensial dan interval Inform otomatis langsung melalui saklar di tabel perangkat ACS atau di kartu ONT halaman detail langganan.
- **Pencarian Wilayah Fleksibel pada Layanan Broadband**: Admin dan operator dapat memfilter dan menyortir daftar layanan broadband aktif berdasarkan area kerja tertentu atau kabupaten/kota tempat pelanggan berada.
- **Audit & Pemulihan Saldo Poin Sekali Klik**: Administrator kini dapat memperbaiki inkonsistensi saldo poin staf/karyawan melalui menu Alat Pemulihan dengan laporan audit yang transparan.
- **Reboot Modem Jarak Jauh**: Staf NOC dapat mereboot perangkat ONT pelanggan secara aman dari jarak jauh saat menangani keluhan jaringan tanpa perlu login ke GUI modem pelanggan.

### Bug Fix / Solusi Masalah

- **Pencegahan Kebocoran Kata Sandi ACS**: Sidik jari provisi kini disimpan dalam bentuk hash SHA-256 (`buildProvisionSignature`), menghilangkan risiko kata sandi sistem terbawa ke browser pada payload detail perangkat.
- **Proteksi Firmware ONT Asing**: ONT baru yang pertama kali mengirimkan *Inform* tidak akan langsung ditulisi parameter TR-069 secara agresif sebelum admin mengaktifkan saklar auto-provisioning untuk perangkat tersebut.
- **Penanganan Karakter Khusus Regex pada Pencarian Broadband**: Menghilangkan kemungkinan terjadinya *unhandled error* atau *crash* kueri Mongo saat pengguna memasukkan karakter regex spesial pada kolom filter wilayah atau nama pelanggan.
- **Sinkronisasi Saldo Poin Telegram Mini App**: Memastikan level dan total poin karyawan di Telegram Mini App selalu akurat dan selaras dengan histori perolehan poin aktual.

### Menu/Fitur Baru

- Kolom dan saklar **Auto Provisioning** pada tabel **Perangkat ACS** (`/network/acs-devices`) dan kartu **Informasi ONT** pada halaman detail broadband (`/services/broadband/:id`).
- Kolom **Area**, **Provinsi**, dan **Kabupaten/Kota** pada tabel **Layanan Broadband** (`/services/broadband`) serta kolom **Area** pada tabel **Pelanggan** (`/users/customer`).
- Aksi **Sinkronisasi Poin Admin** pada menu **Pengaturan > Alat Pemulihan Sistem** (`/settings/recovery-tools`).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Manajemen Auto-Provisioning Perangkat ONT TR-069

- **Penjelasan Fitur**:
  Auto-Provisioning adalah fitur otomatisasi yang menyematkan interval *Periodic Inform* standar dan kredensial *Connection Request* terpadu ke modem ONT pelanggan. Untuk melindungi armada modem heterogen dari risiko penulisan massal yang salah, sistem menerapkan gerbang berlapis tiga:
  1. Pengaturan global sistem (`acs_provision_enabled`).
  2. Kredensial ACS yang valid dan lengkap di pengaturan sistem.
  3. **Saklar Granular Per Perangkat (`auto_provision_enabled`)**: ONT hanya akan dikonfigurasi otomatis saat mengirimkan denyut *Inform* jika saklar pada perangkat tersebut dalam posisi aktif.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Jaringan > Perangkat ACS** (`/network/acs-devices`).
  2. Temukan baris perangkat ONT yang ingin dikelola.
  3. Pada kolom **Konfigurasi Otomatis**, klik saklar (*switch*) untuk mengaktifkan fitur. Sistem akan menampilkan notifikasi bahwa auto-provisioning telah dinyalakan.
  4. Atau, buka menu **Layanan > Broadband**, pilih salah satu pelanggan, lalu pada kartu **Informasi ONT**, ubah saklar **Konfigurasi Otomatis** ke posisi aktif.
  5. Saat perangkat ONT tersebut mengirimkan sesi *Inform* berikutnya ke GenieACS, backend akan secara otomatis menyiapkan dan menerapkan parameter interval inform dan kredensial yang dibutuhkan.

---

### 2. Rekonsiliasi Poin Admin pada Menu Pemulihan Sistem

- **Penjelasan Fitur**:
  Fungsi ini membaca seluruh catatan histori poin bulanan setiap admin dan menjumlahkannya kembali secara bersih, kemudian memperbarui total poin pada profil admin jika ditemukan perbedaan angka, lalu menampilkan daftar akun yang disesuaikan kepada pengguna.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Pengaturan > Alat Pemulihan** (`/settings/recovery-tools`).
  2. Temukan kartu aksi **Sinkronisasi Poin Admin**.
  3. Klik tombol **Jalankan Rekonsiliasi**.
  4. Konfirmasikan dialog persetujuan.
  5. Sistem akan memproses seluruh data admin dan menampilkan ringkasan hasil berupa daftar akun yang dikoreksi beserta nilai sebelum dan sesudahnya.
