# 📝 Daily Work Report - Dedy S.N Putra (26 September 2026)

---

## 📅 Laporan Harian - 26 September 2026

---

## 🌿 Branch: `issue-337` — Issue #337: Pengajuan Izin & Cuti per Pengajuan dengan Persetujuan per Hari & Developer Recovery Tool

### 📌 Informasi Issue

- **Nomor Issue**: #337
- **Judul Issue**: Pengajuan Izin & Cuti per Pengajuan dengan Persetujuan per Hari, Migrasi Data Induk & Developer Recovery Tool
- **Status Branch**: `Belum di-merge` (Branch aktif saat ini dengan perubahan lokal/uncommitted di working tree)

### 📅 Rincian Perubahan & Pekerjaan Aktif

#### [Uncommitted / In-Progress] - Backfill Migrasi Data Induk Izin & Cuti, API Recovery DB, dan Antarmuka Recovery Tools Developer

- **Komponen yang Berubah**:
  - `backend/scripts/backfill-attendance-requests.js`
  - `backend/src/controllers/attendance.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/attendance.route.js`
  - `backend/src/services/recovery.service.js`
  - `backend/test/integration/attendanceRecovery.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/RecoveryDryRunModal.jsx`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Backend Core & Database Recovery (`services/recovery.service.js`)**:
    - **Layanan Pemulihan & Migrasi Izin/Cuti (`recoveryAttendanceData` & `processAttendanceTarget`)**: Mengembangkan mesin pemulihan data untuk memigrasikan data izin (`PermissionDay` / `attendance_absences`) dan cuti (`PaidLeaveDay` / `attendance_permits`) warisan (*legacy*) yang belum memiliki referensi ke dokumen induk pengajuan (`PermissionRequest` / `PaidLeaveRequest`).
    - **Pengelompokan Cerdas Data Yatim (*Orphan Records*)**: Mengelompokkan rekaman harian yang tidak memiliki induk berdasarkan kriteria identitas pegawai (`admin`), kesamaan judul permohonan, dan kedekatan rentang tanggal kalender.
    - **Kalkulasi Metrik & Status Otomatis**: Menghitung `total_days`, `accepted_days`, dan `rejected_days` untuk setiap pengajuan hasil rekonstruksi, lalu menetapkan status akhir secara dinamis (`pending`, `accepted`, `rejected`, atau `partial`) menggunakan helper `resolveAttendanceRequestStatus`.
    - **Dukungan Simulasi Tanpa Risiko (*Dry-Run Mode*)**: Menyediakan mode simulasi (`dryRun: true`) yang hanya menghitung dan melaporkan jumlah hari yatim serta calon grup pengajuan tanpa melakukan penulisan apa pun ke MongoDB, dan mode eksekusi riil (`dryRun: false`) yang membuat dokumen induk serta memperbarui field referensi `request` pada setiap dokumen harian secara idempoten.
  - **Controller & Routing Backend (`controllers/attendance.controller.js`, `routes/attendance.route.js`)**:
    - **Endpoint Developer Recovery (`POST /api/v1/attendance/recovery-db`)**: Mendaftarkan rute pemulihan data yang dilindungi secara ketat oleh middleware `protectedDeveloper`.
    - **Validasi Keamanan Ganda (Konfirmasi Kata Sandi Aksi)**: Mewajibkan payload `{ dryRun: false, confirm: 'RECOVERY' }` untuk eksekusi riil, menolak request yang tidak valid dengan HTTP `400 Bad Request` guna mencegah eksekusi tanpa sengaja oleh pengguna non-teknis.
    - **Dokumentasi Swagger/OpenAPI Terstruktur**: Menambahkan anotasi Swagger lengkap dengan skema request body, parameter konfirmasi, respon status, dan deskripsi teknis alur kerja perbaikan.
  - **Skrip Utilitas Migrasi CLI (`scripts/backfill-attendance-requests.js`)**:
    - Memperbarui skrip migrasi backfill mandiri agar memanfaatkan abstraksi yang sama dari `recovery.service.js`, menyajikan laporan CLI yang rapi dengan ringkasan jumlah dokumen yang berhasil dipulihkan.
  - **Frontend Developer Tools UI (`pages/settings/sections/developer/recoveryTools/`)**:
    - **Konfigurasi Jalur Pemulihan Baru (`recoveryRoutes.config.js`)**: Menambahkan kategori/kelompok baru `'absensi'` (`Kehadiran & Absensi`) dan mendaftarkan item `attendanceRequestsBackfill` dengan tingkat risiko `medium`, dukungan mode *dry-run*, serta proteksi kata sandi konfirmasi `'RECOVERY'`.
    - **Dukungan Kata Kunci Konfirmasi Dinamis (`RecoveryDryRunModal.jsx`)**: Memperluas modal simulasi/eksekusi agar mendukung properti `requireConfirmWord` dari konfigurasi rute, sehingga modal secara otomatis menampilkan input teks konfirmasi kata kunci yang tepat sebelum tombol eksekusi permanen dapat diklik.
  - **Internasionalisasi / i18n (`translation.json` Backend & Frontend)**:
    - Menambahkan translasi dwibahasa (ID & EN) untuk grup `absensi`, judul serta deskripsi item `attendanceRequestsBackfill`, serta notifikasi umpan balik sukses/gagal simulasi dan eksekusi perbaikan database.
  - **Pengujian Integrasi Komprehensif (`backend/test/integration/attendanceRecovery.test.js`)**:
    - Membangun *test suite* integrasi otomatis Vitest yang memvalidasi bahwa mode *dry-run* tidak menghasilkan mutasi data apa pun pada basis data, memastikan eksekusi riil berhasil membuat dokumen induk serta menautkan dokumen harian, dan memastikan pemanggilan ulang menghasilkan operasi idempoten (0 sisa hari yatim).

---

## 🌿 Branch: `issue-175` — Issue #175: TR-069 GenieACS Integration & ONT/CPE Device Monitoring

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: TR-069 GenieACS Integration — Remote Reboot, Fault Tracking, Inform Heartbeat & Periodic State Sync Worker
- **Status Branch**: `Belum di-merge` (Branch `origin/issue-175`, commit `b32f2561`)

### 📅 Rincian Commit

#### [b32f2561] - resolve #175 - Sabtu, 26 September 2026, 23:59:19 WIB

- **Komponen yang Berubah**:
  - `acs/config/provisions/inform.js`
  - `acs/test/inform.provision.test.js`
  - `acs/test/sandbox.js`
  - `backend/src/controllers/acs.controller.js`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/acsDevice.model.js`
  - `backend/src/routes/acs.route.js`
  - `backend/src/routes/internal.route.js`
  - `backend/src/services/acs.service.js`
  - `backend/src/services/acsConfig.service.js`
  - `backend/src/services/cronSettings.service.js`
  - `backend/test/integration/acs.controller.test.js`
  - `backend/test/integration/acs.service.test.js`
  - `backend/test/integration/acsConfig.service.test.js`
  - `backend/test/unit/cronSettings.service.test.js`
  - `backend/tmp-uji.mjs` [NEW]
  - `backend/tmp-uji2.mjs` [NEW]
  - `backend/tmp-uji4.mjs` [NEW]
  - `cron-worker/src/jobs/processors/acsStateSync.js` [NEW]
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `frontend/src/app/pages/network/acsDevices/components/AcsDeviceConfigModal.jsx`
  - `frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Fase 0 Heartbeat Webhook GenieACS (`acs/config/provisions/inform.js`, `backend/src/services/acs.service.js`)**:
    - Mengimplementasikan denyut Inform Fase 0 (`touchAcsDeviceFromHeartbeat`) yang dikirim GenieACS ke Backend sebelum parameter perangkat dibaca. Hal ini menyelesaikan masalah kritis di mana perangkat sehat yang mengalami CWMP fault minor selain kode 9005 tidak pernah mengirimkan webhook lengkap sehingga sempat terhapus/hilang dari daftar aktif.
  - **Aksi Remote Reboot CPE via TR-069 RPC (`controllers/acs.controller.js`, `services/acs.service.js`, `routes/acs.route.js`)**:
    - Menambahkan fungsi `rebootAcsDevice` yang mengeksekusi RPC `Reboot` TR-069 via GenieACS NBI.
    - Menyediakan endpoint API `POST /api/v1/acs/reboot/:serial_number` lengkap dengan penanganan status respons yang presisi (status 404 jika perangkat tidak ada, 502 jika GenieACS tidak dapat dihubungi, atau 422 jika perangkat belum pernah terhubung/offline).
  - **Pelacakan & Visualisasi Fault CWMP (`models/acsDevice.model.js`, `services/acsConfig.service.js`)**:
    - Menambahkan field pelacakan error TR-069 pada skema perangkat: `acs_fault_code`, `acs_fault_message`, dan `acs_fault_at`.
    - Menangani skenario di mana ONT menjawab tetapi menolak konfigurasi parameter (`device_rejected`), serta membedakan secara tegas antara ONT yang tidak dapat dihubungi (*unreachable*) dengan ONT yang menolak konfigurasi dengan kode kesalahan spesifik.
  - **Sinkronisasi Berkala State ACS via Cron Worker (`cron-worker/src/jobs/processors/acsStateSync.js`)**:
    - Membangun job prosesor berkala `acsStateSync` di Cron Worker yang berinteraksi dengan API internal Backend (`/internal/acs/sync-state`) untuk melakukan rekonsiliasi berkala terhadap status perangkat yang terdaftar di GenieACS dengan data di MongoDB.
    - Menambahkan pengaturan jadwal sinkronisasi di `scheduler.js` dan pendaftaran worker di `worker.js`.
  - **Penyempurnaan UI Antarmuka Pengguna Frontend (`AcsDeviceDetailDrawer.jsx`, `AcsDeviceConfigModal.jsx`, `rows.jsx`)**:
    - Menambahkan tombol aksi **Reboot Perangkat** interaktif pada drawer detail perangkat ONT dengan konfirmasi pengguna.
    - Menampilkan banner peringatan status *fault* dan detail pesan error dari perangkat jika konfigurasi sebelumnya ditolak.
    - Menampilkan indikator status dan waktu inform/polling terakhir secara visual pada tabel perangkat.
  - **Pengujian Komprehensif (Unit & Integration Tests)**:
    - Menambahkan ratusan baris skenario pengujian otomatis di `acs.service.test.js`, `acsConfig.service.test.js`, `acs.controller.test.js`, dan `inform.provision.test.js` untuk memastikan keandalan alur reboot, penanganan fault, pembaruan konfigurasi nirkabel, dan denyut heartbeat.

---

## 🌿 Branch: `master` — Issue #335: Perbaikan URL Tanda Tangan Berkas BAP & Pratinjau Dokumen Instalasi

### 📌 Informasi Issue

- **Nomor Issue**: #335
- **Judul Issue**: Perbaikan URL Tanda Tangan Berkas BAP & Pratinjau Dokumen Instalasi Pelanggan
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Sabtu, 26 September 2026, 17:48:28 WIB via commit `a7706cce`)

### 📅 Rincian Commit

#### [a7706cce] - resolve #335 - Sabtu, 26 September 2026, 17:48:28 WIB
*(Merge commit menggabungkan branch issue-335 / commit `069e822b`)*

- **Komponen yang Berubah**:
  - `frontend/src/app/pages/public/PublicBAPDocument.jsx`
  - `frontend/src/app/pages/tickets/installation/components/InstallationDocumentPreview.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Resolusi URL Tanda Tangan Digital BAP (`PublicBAPDocument.jsx`)**: Memperbaiki logika pembentukan URL tanda tangan digital pelanggan pada dokumen Berita Acara Pemasangan (BAP) publik agar secara cerdas mendeteksi apakah path berkas yang tersimpan merupakan URL absolut MinIO/S3 atau relative path backend, sehingga tanda tangan pelanggan selalu muncul dengan sempurna saat dokumen dibuka oleh pelanggan.
  - **Pratinjau Dokumen Instalasi Tiket (`InstallationDocumentPreview.jsx`)**: Menerapkan standarisasi resolusi URL yang sama pada pratinjau dokumen instalasi di modul tiket teknisi, mencegah kegagalan pemuatan gambar tanda tangan (*broken image*) pada dialog pratinjau internal.

---

## 🌿 Branch: `master` — Issue #333: Self-Service Absensi Pegawai, Tab Profil & Akses Berkas Bukti Kehadiran Pribadi

### 📌 Informasi Issue

- **Nomor Issue**: #333
- **Judul Issue**: Self-Service Absensi Pegawai, Tab Absensi Profil Pribadi & Akses Berkas Bukti Izin/Sakit Mandiri
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Sabtu, 26 September 2026, 17:12:08 WIB via commit `09f60080` dan `117e131e`)

### 📅 Rincian Commit

#### [09f60080] / [117e131e] - resolve #333 - Sabtu, 26 September 2026, 17:12:08 WIB
*(Merge commit menggabungkan branch issue-333 / commit `77ab02de`)*

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/attendance.controller.js`
  - `backend/src/controllers/files.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/middlewares/privilegeSelf.middleware.js` [NEW]
  - `backend/src/models/attendanceAbsence.model.js`
  - `backend/src/models/attendancePermit.model.js`
  - `backend/src/models/attendancePresence.model.js`
  - `backend/src/routes/admin.route.js`
  - `backend/src/routes/attendance.route.js`
  - `backend/src/routes/files.route.js`
  - `backend/src/services/attendance.service.js`
  - `backend/src/utils/generate-permissions.js`
  - `backend/test/integration/attendancePermissionFileAccess.test.js` [NEW]
  - `backend/test/integration/attendanceSelfList.test.js` [NEW]
  - `backend/test/unit/privilegeSelf.middleware.test.js` [NEW]
  - `frontend/src/app/pages/profile/index.jsx`
  - `frontend/src/app/pages/users/components/UserAttendanceTabs.jsx`
  - `frontend/src/app/router/activities/paidLeave.jsx`
  - `frontend/src/app/router/activities/permission.jsx`
  - `frontend/src/components/shared/table/RowActions.jsx`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Mekanisme Self-Service Kehadiran (`privilegeSelf.middleware.js`, `attendance.service.js`)**: Membangun middleware otorisasi cerdas yang memungkinkan setiap pegawai yang sedang login untuk mengakses dan meninjau seluruh data riwayat presensi harian, izin sakit, dan kuota cuti milik pribadinya sendiri tanpa memerlukan hak akses administratif global (*elevated privilege*).
  - **Akses Berkas Mandiri yang Aman (`files.controller.js`, `files.route.js`)**: Memberikan izin bagi pegawai untuk mengunduh dan melihat kembali berkas bukti lampiran (seperti surat keterangan dokter atau formulir izin) yang pernah mereka unggah sendiri, dengan tetap memblokir akses ke berkas milik pegawai lain.
  - **Tab Terpadu Kehadiran di Halaman Profil (`pages/profile/index.jsx`, `UserAttendanceTabs.jsx`)**: Mengintegrasikan tab riwayat absensi komprehensif pada halaman profil pengguna, menampilkan tabel riwayat presensi, tabel izin, dan kuota cuti pribadi secara rapi dan responsif.
  - **Pengujian Unit & Integrasi Self-Service**: Mengimplementasikan *test suite* otomatis untuk menguji pembatasan cakupan data (*scope isolation*) dan verifikasi akses file mandiri.

---

## 🌿 Branch: `master` — Issue #330: Hak Akses Absensi, Rekapitulasi Presensi & Akses File Bukti

### 📌 Informasi Issue

- **Nomor Issue**: #330
- **Judul Issue**: Hak Akses Terinci Absensi, Rekapitulasi Presensi Bulanan & Proteksi Akses File Bukti Kehadiran
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Sabtu, 26 September 2026, 15:37:16 WIB via commit `889f97f0`)

### 📅 Rincian Commit

#### [889f97f0] - resolve #330 - Sabtu, 26 September 2026, 15:37:16 WIB
*(Merge commit menggabungkan branch issue-330 / commit `3c8c124a`)*

- **Komponen yang Berubah**:
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/attendance.controller.js`
  - `backend/src/controllers/files.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/attendanceAbsence.model.js`
  - `backend/src/models/attendancePermit.model.js`
  - `backend/src/models/attendancePresence.model.js`
  - `backend/src/routes/attendance.route.js`
  - `backend/src/routes/files.route.js`
  - `backend/src/services/admin.service.js`
  - `backend/src/services/attendance.service.js`
  - `backend/src/utils/resolveAdminFilter.js`
  - `backend/test/integration/attendanceFileAccess.test.js` [NEW]
  - `backend/test/integration/attendanceMonthlySummary.scope.test.js` [NEW]
  - `backend/test/integration/attendanceRequestList.scope.test.js` [NEW]
  - `frontend/src/app/layouts/Root.jsx`
  - `frontend/src/app/navigation/activities.js`
  - `frontend/src/app/pages/activities/attendance/index.jsx`
  - `frontend/src/app/pages/activities/paidLeave/index.jsx`
  - `frontend/src/app/pages/activities/paidLeave/schema/columns.jsx`
  - `frontend/src/app/pages/activities/permission/index.jsx`
  - `frontend/src/app/pages/activities/permission/schema/columns.jsx`
  - `frontend/src/app/router/activities/paidLeave.jsx`
  - `frontend/src/app/router/activities/permission.jsx`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Pemisahan Granular Hak Akses Modul Kehadiran (`privilege.json`, `privilegeDictionary.json`)**: Memisahkan hak akses presensi menjadi beberapa hak spesifik: `attendance.list` (melihat daftar presensi umum), `attendance.monthlySummary` (melihat rekapitulasi presensi bulanan), dan `attendance.file` (mengakses berkas bukti/lampiran presensi).
  - **Rekapitulasi Presensi Bulanan Pegawai (`attendance/index.jsx`, `attendance.service.js`)**: Membangun visualisasi data ringkasan kehadiran bulanan pegawai dengan agregasi total hadir, terlambat, izin, dan absen per periode bulan/tahun yang dapat difilter.
  - **Proteksi Berkas Bukti & Pengujian Scope**: Memperketat otorisasi unduh berkas lampiran presensi di `files.controller.js` serta menambahkan serangkaian uji integrasi untuk memastikan filter pembatasan data berdasarkan cabang/organisasi berjalan dengan benar.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #337  | Pengajuan Izin & Cuti per Pengajuan & Developer Recovery Tool | Migrasi database izin/cuti harian lama ke dokumen induk pengajuan baru via Developer Recovery Tool dengan mode simulasi (*dry-run*) dan pengamanan konfirmasi kata kunci. |
| #175  | TR-069 GenieACS Integration & ONT/CPE Device Monitoring | Penambahan fitur Remote Reboot ONT, Inform Phase 0 Heartbeat, pelacakan kode fault CWMP secara terperinci, sinkronisasi state periodik via Cron Worker, serta pengujian end-to-end yang solid. |
| #335  | Fix Signature URL BAP Document & Installation Preview | Memastikan pratinjau berkas BAP publik dan pratinjau instalasi tiket selalu dapat memuat tanda tangan digital pelanggan secara presisi tanpa kendala *broken image*. |
| #333  | Self-Service Absensi Pegawai & Tab Profil Pribadi | Memberikan akses mandiri kepada setiap pegawai untuk melihat rekapitulasi kehadiran, mengajukan dan melihat izin/cuti pribadi, serta melihat berkas lampiran mereka sendiri di halaman Profil. |
| #330  | Hak Akses Absensi, Rekapitulasi Presensi & Akses File Bukti | Pemisahan hak akses manajemen kehadiran secara terperinci, penyediaan rekapitulasi presensi bulanan, dan pengamanan hak unduh berkas bukti izin. |

### Kemampuan Baru Pengguna/Admin

- **Remote Reboot ONT Jarak Jauh**: Tim teknis NOC/Helpdesk dapat melakukan restart ONT pelanggan secara langsung dari dashboard Dekasimal V2 dengan mengeklik tombol *Reboot Perangkat* di drawer detail perangkat.
- **Deteksi Kesalahan & Penolakan Konfigurasi ONT Transparan**: Ketika konfigurasi WiFi atau parameter perangkat ditolak oleh ONT, sistem menampilkan kode fault CWMP dan deskripsi penyebab spesifik alih-alih pesan kegagalan generik.
- **Self-Service Kehadiran untuk Seluruh Karyawan**: Karyawan dapat memantau status presensi harian, izin sakit, dan saldo cuti mereka sendiri secara mandiri langsung dari halaman Profil akun mereka.
- **Akses Berkas Lampiran Pribadi yang Aman**: Karyawan dapat langsung melihat kembali berkas bukti yang pernah mereka unggah (seperti surat izin sakit dokter) tanpa harus meminta bantuan staf HRD.
- **Rekapitulasi Kehadiran Bulanan Terpusat**: Bagian HR/Kepegawaian dapat meninjau rekapitulasi kehadiran seluruh staf per bulan secara komprehensif untuk kebutuhan evaluasi kinerja dan payroll.
- **Pemulihan Basis Data Izin & Cuti Aman (Developer Tools)**: Tim developer dapat menjalankan simulasi *dry-run* dan eksekusi migrasi penggabungan hari-hari izin/cuti yatim menjadi dokumen induk pengajuan terpadu melalui panel Recovery Tools dengan pengaman kata sandi `RECOVERY`.

### Bug Fix / Solusi Masalah

- **Pencegahan Perangkat Sehat Hilang dari Monitoring (ACS Phase 0 Heartbeat)**: Menangani anomali di mana perangkat ONT yang mengalami fault kecil saat pembacaan parameter gagal mengirimkan webhook penuh sehingga sempat menghilang dari sistem; kini denyut Phase 0 selalu mencatat status aktif perangkat.
- **Pencegahan Broken Signature pada Dokumen BAP Publik**: Memperbaiki pembentukan URL tanda tangan digital pada dokumen BAP publik dan tiket instalasi agar mendukung baik format path relatif maupun URL absolut MinIO.
- **Pencegahan Kebocoran Berkas Bukti Kehadiran**: Mencegah akses tidak sah ke berkas bukti izin/sakit pegawai lain dengan validasi kepemilikan dokumen yang ketat di level API files controller.
- **Pencegahan Eksekusi Mutasi Database Tidak Sengaja**: Menambahkan pengamanan konfirmasi kata kunci ketat (`requireConfirmWord`) pada modal eksekusi Developer Recovery Tools.

### Menu/Fitur Baru

- **Panel Pemulihan Data Pengajuan Izin & Cuti (`/settings/developer/recovery-tools`)**: Opsi pemulihan data absensi baru pada kelompok 'Kehadiran & Absensi' untuk migrasi pengajuan izin dan cuti.
- **Tab Kehadiran di Halaman Profil (`/profile`)**: Tab baru untuk memeriksa data presensi, riwayat izin, dan riwayat cuti mandiri bagi setiap pegawai.
- **Aksi Reboot Perangkat di Drawer ACS Devices (`/network/acs`)**: Tombol aksi dan modal konfirmasi restart ONT berbasis TR-069.
- **Halaman Rekapitulasi Presensi Bulanan (`/activities/attendance`)**: Tab baru rekapitulasi kehadiran bulanan pegawai dengan filter periode.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Menjalankan Pemulihan Data Pengajuan Izin & Cuti (Developer Recovery Tools)

- **Penjelasan Fitur**: Fitur ini digunakan oleh pengembang/administrator teknis untuk mengelompokkan data izin dan cuti harian versi lama menjadi dokumen induk pengajuan tunggal (sesuai arsitektur Issue #337). Fitur ini dilengkapi mode simulasi (*dry-run*) untuk mengaudit berapa banyak data yatim sebelum benar-benar menulis ke basis data.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi menggunakan akun yang memiliki hak akses pengembang (*Developer*).
  2. Buka menu **Pengaturan → Developer → Recovery Tools** (`/settings/developer/recovery-tools`).
  3. Buka tab/kelompok **Kehadiran & Absensi**.
  4. Temukan opsi **Migrasi Pengajuan Izin & Cuti** (`attendanceRequestsBackfill`), lalu klik tombol **Pratinjau / Jalankan**.
  5. Pada modal yang terbuka, biarkan opsi **Hanya Pratinjau (Dry Run)** tercentang, lalu klik tombol **Jalankan Pratinjau**.
  6. Periksa hasil simulasi: pastikan jumlah dokumen hari yatim (*orphan days*) dan rencana pengajuan yang akan dibuat sesuai dengan data yang diharapkan.
  7. Jika hasil simulasi telah diverifikasi dan siap dieksekusi ke basis data:
     - Hilangkan centang pada opsi *Hanya Pratinjau (Dry Run)*.
     - Ketikkan kata konfirmasi persis: `RECOVERY` pada kotak teks konfirmasi yang disediakan.
     - Klik tombol **Eksekusi Perubahan**.
  8. Sistem akan memproses perbaikan data, membuat dokumen induk pengajuan, menautkan hari-hari terkait, dan menampilkan konfirmasi keberhasilan.

---

### 2. Melakukan Remote Reboot ONT Pelanggan via TR-069

- **Penjelasan Fitur**: Fitur Remote Reboot memungkinkan tim teknis NOC/Helpdesk merestart ONT pelanggan dari jarak jauh tanpa perlu memandu pelanggan mencabut adaptor daya secara manual.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Network → ACS Devices** (`/network/acs`) pada sidebar navigasi.
  2. Cari perangkat ONT pelanggan berdasarkan nomor serial, nama pelanggan, atau alamat IP.
  3. Klik baris perangkat untuk membuka panel **AcsDeviceDetailDrawer** di sisi kanan layar.
  4. Pastikan perangkat dalam status terhubung (*online*) atau pernah terhubung sebelumnya.
  5. Pada bagian atas atau tab tindakan perangkat, klik tombol **Reboot Perangkat**.
  6. Pada dialog konfirmasi yang muncul, konfirmasikan bahwa perangkat akan direstart.
  7. Sistem Dekasimal V2 akan mengirimkan instruksi RPC `Reboot` ke GenieACS untuk diteruskan ke ONT.
  8. Notifikasi toast akan memberi tahu bahwa perintah reboot berhasil dikirimkan, dan ONT akan memulai proses restart secara otomatis.

---

### 3. Mengakses Riwayat Presensi & Berkas Bukti Pribadi (Self-Service)

- **Penjelasan Fitur**: Setiap pegawai kini dapat meninjau rekaman kehadiran dan berkas lampiran yang pernah diajukan tanpa perlu meminta bantuan bagian personalia/HRD.
- **Langkah Penggunaan (Tutorial)**:
  1. Klik foto profil atau nama pengguna di pojok kanan atas, lalu pilih **Profil Saya** (`/profile`).
  2. Pada halaman profil, klik tab **Kehadiran** (`UserAttendanceTabs`).
  3. Pilih sub-tab yang ingin diperiksa:
     - **Presensi Harian**: Menampilkan tanggal, jam masuk, jam pulang, serta lokasi check-in/check-out.
     - **Izin / Sakit**: Menampilkan daftar permohonan izin atau sakit, status persetujuan, dan tautan pratinjau berkas surat dokter yang pernah diunggah.
     - **Cuti**: Menampilkan kuota cuti tahunan, sisa hari cuti yang dapat digunakan, serta riwayat pengajuan cuti sebelumnya.
  4. Untuk memeriksa surat dokter/lampiran yang pernah diunggah pada permohonan izin, klik ikon berkas pada baris terkait untuk melihat pratinjau berkas secara langsung di peramban.
