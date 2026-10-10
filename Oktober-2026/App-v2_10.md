# 📝 Daily Work Report - Dedy S.N Putra (10 Oktober 2026)

---

## 📅 Laporan Harian - 10 Oktober 2026

---

## 🌿 Branch: `issue-389` — Issue #389: Sinkronisasi Waktu Mesin Absensi, Penulisan Balik Sidik Jari & Alat Pemulihan Biometrik

### 📌 Informasi Issue

- **Nomor Issue**: #389
- **Judul Issue**: Sinkronisasi Waktu Mesin Absensi Fisik, Penulisan Balik Template Sidik Jari (Biometric Writeback), Sakelar Status Mesin Aktif & Alat Pemulihan Data Biometrik
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Sabtu, 10 Oktober 2026, 19:07:38 WIB via commit `7e2348a7`)

### 📅 Rincian Commit

#### [7e2348a7] - resolve #389 - Sabtu, 10 Oktober 2026, 19:07:38 WIB

_(Merge commit menggabungkan branch `issue-389` / commit `3831eba3` ke branch `master`)_

---

#### [3831eba3] - resolve #389 - Sabtu, 10 Oktober 2026, 11:10:54 WIB

- **Komponen yang Berubah**:
  - `.env.production.example`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/attendance.controller.js`
  - `backend/src/controllers/attendanceDevice.controller.js`
  - `backend/src/controllers/utils.controller.js`
  - `backend/src/lib/attendanceDeviceClient.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/admin.model.js`
  - `backend/src/models/attendanceFingerprint.model.js` [NEW]
  - `backend/src/routes/attendance.route.js`
  - `backend/src/routes/attendanceDevice.route.js`
  - `backend/src/routes/public.route.js`
  - `backend/src/services/attendanceDevice.service.js`
  - `backend/src/services/attendanceDeviceSync.service.js`
  - `backend/src/services/recovery.service.js`
  - `backend/test/integration/attendanceFingerprintRecovery.test.js` [NEW]
  - `backend/test/unit/attendanceDeviceActiveStatus.test.js` [NEW]
  - `backend/test/unit/attendanceDeviceClient.test.js`
  - `backend/test/unit/attendanceDeviceFingerprintSync.test.js`
  - `backend/test/unit/attendanceDeviceUserManagement.test.js`
  - `backend/test/unit/computeNextBillingDate.test.js`
  - `backend/test/unit/publicVersion.test.js` [NEW]
  - `frontend/src/app/navigation/warehouse.js`
  - `frontend/src/app/pages/activities/attendanceDevice/create.jsx`
  - `frontend/src/app/pages/activities/attendanceDevice/schema/DeviceExtraActions.jsx`
  - `frontend/src/app/pages/activities/attendanceDevice/schema/DeviceStatusSwitchCell.jsx` [NEW]
  - `frontend/src/app/pages/activities/attendanceDevice/schema/deviceColumns.jsx`
  - `frontend/src/app/pages/activities/attendanceDevice/schema/deviceSchema.js`
  - `frontend/src/app/pages/activities/attendanceDevice/userManager.jsx`
  - `frontend/src/app/pages/settings/sections/Developer.jsx`
  - `frontend/src/app/pages/settings/sections/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `reverse-proxy/.env.example`
  - `reverse-proxy/Dockerfile`
  - `reverse-proxy/docker-entrypoint.d/15-default-backend-server-name.envsh` [NEW]
  - `reverse-proxy/templates/backend.conf.template`
- **Deskripsi Perubahan & Fungsi**:
  - **Sinkronisasi Waktu Mesin Absensi ke Jam Server WIB (`attendanceDeviceClient.js`, `attendanceDevice.service.js`)**:
    - Membangun utilitas `encodeWibTime` untuk mengonversi waktu standar WIB (UTC+7) ke dalam format biner 32-bit yang dipahami perangkat keras ZKTeco: `((year - 2000) * 12 * 31 + ((month - 1) * 31) + day - 1) * 86400 + (hour * 3600 + minute * 60 + second)`.
    - Menambahkan metode `getTime` dan `setTime` pada interface klien perangkat untuk membaca dan mengatur jam mesin absensi via perintah protokol ZKTeco (`CMD_SET_TIME`).
    - Membuat endpoint baru `POST /attendance-device/devices/:id/sync-time` yang menyelaraskan jam RTC (*Real-Time Clock*) mesin secara instan dengan server aplikasi saat tombol aksi ditekan.
  - **Inspeksi Status Koneksi Mendalam & Selisih Waktu (`attendanceDevice.controller.js`, `DeviceExtraActions.jsx`)**:
    - Memperluas fungsi `checkDeviceConnection` untuk mengekstrak informasi detail perangkat keras: waktu mesin saat ini, waktu server, selisih detik (*time delta*), nomor seri, versi firmware, nama platform, dan algoritma biometrik yang digunakan (misal ZKFinger VX10.0).
    - Memperbarui dialog **Cek Koneksi Mesin** pada UI frontend agar menyajikan status drift jam secara informatif dengan lencana peringatan warna jika terdapat deviasi waktu yang signifikan.
  - **Penulisan Balik Template Sidik Jari ke Mesin Fisik / Biometric Writeback (`attendanceDeviceClient.js`, `attendanceDevice.service.js`)**:
    - Mengimplementasikan metode `saveUserTemplate`, `saveUserWithTemplates`, dan `saveUsersWithTemplates` untuk menuliskan template biometrik dari database kembali ke memori flash mesin absensi.
    - Mengembangkan encoder format biner `packUser73` (struktur data pengguna 73-byte) dan `buildHighRateUserTemplatesPayload` untuk transfer data sidik jari berkecepatan tinggi tanpa membebani memori mesin.
    - Pada fitur sinkronisasi pengguna (`syncDeviceUsersFromAdmins`), sistem kini secara otomatis memeriksa template sidik jari tersimpan di database dan langsung menuliskannya ke mesin jika pengguna di mesin belum memiliki template sidik jari aktif.
  - **Model Biometrik Terisolasi & Alat Pemulihan Struktur Data (`models/attendanceFingerprint.model.js`, `recovery.service.js`)**:
    - Memisahkan penyimpanan template sidik jari dari dokumen `Admin` ke koleksi baru yang terisolasi `attendance_fingerprint_templates` (`AttendanceFingerprint`).
    - Mengatasi masalah ukuran payload dokumen admin yang sebelumnya berisiko melampaui batas sesi token autentikasi (JWT header overflow).
    - Menyediakan alat pemulihan struktur data baru `recoveryAttendanceFingerprintsData` pada panel **Pengaturan → Pengembang → Alat Pemulihan** dengan opsi simulasi (*dry-run*) sebelum data dimigrasikan secara permanen.
  - **Sakelar Status Mesin Aktif / Nonaktif (`DeviceStatusSwitchCell.jsx`, `attendanceDeviceSync.service.js`)**:
    - Menambahkan field `is_active` (Boolean, default `true`) pada skema perangkat absensi.
    - Menambahkan komponen sakelar status langsung pada tabel daftar mesin absensi untuk memudahkan aktivasi atau penonaktifan perangkat tanpa harus masuk ke formulir edit.
    - Memperbaiki layanan sinkronisasi berkala di latar belakang (`attendanceDeviceSync.service.js`) agar secara otomatis mengabaikan mesin yang berstatus nonaktif (`is_active: false`), mencegah pemborosan siklus CPU dan timeout jaringan pada mesin yang sedang dicabut atau dalam perbaikan.
  - **Indikator Visual Sidik Jari pada Manajer Pengguna (`userManager.jsx`)**:
    - Menambahkan kolom status perbandingan jumlah sidik jari di mesin fisik vs jumlah sidik jari di database sistem.
    - Memudahkan operator melihat karyawan mana yang sidik jarinya belum tersinkronisasi atau membutuhkan pendaftaran ulang.
  - **Peningkatan Konfigurasi Infrastruktur Reverse Proxy Nginx**:
    - Menambahkan script inisialisasi lingkungan `15-default-backend-server-name.envsh` pada kontainer reverse proxy untuk mendukung resolusi nama backend dinamis dan template `backend.conf.template` yang lebih tangguh.

---

## 🌿 Branch: `issue-381` — Issue #381: Pemisahan Hak Akses Mutasi Gudang & Penguatan Proteksi Akses

### 📌 Informasi Issue

- **Nomor Issue**: #381
- **Judul Issue**: Pemisahan Hak Akses Mutasi Gudang, Pengetatan Akses Laporan Cepat & Fitur Super Password Developer (AGENTS.md Pasal 24)
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Sabtu, 10 Oktober 2026, 16:03:47 WIB via commit `0c80f6d7`)

### 📅 Rincian Commit

#### [0c80f6d7] - resolve #381 - Sabtu, 10 Oktober 2026, 16:03:47 WIB

_(Merge commit menggabungkan branch `issue-381` / commit `5e09fd86` ke branch `master`)_

---

#### [5e09fd86] - resolve #381 - Sabtu, 10 Oktober 2026, 11:12:30 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/admin.controller.js`
  - `backend/src/controllers/auth.controller.js`
  - `backend/src/controllers/superPassword.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/middlewares/auth.middleware.js`
  - `backend/src/middlewares/logger.middleware.js`
  - `backend/src/middlewares/superPasswordLimiter.middleware.js` [NEW]
  - `backend/src/models/authSession.model.js`
  - `backend/src/models/superPassword.model.js` [NEW]
  - `backend/src/routes/admin.route.js`
  - `backend/src/routes/employee.route.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/routes/superPassword.route.js` [NEW]
  - `backend/src/routes/warehouseMutation.route.js`
  - `backend/src/services/authSession.service.js`
  - `backend/src/services/superPassword.service.js` [NEW]
  - `backend/src/utils/migrate-warehouse-mutation-list-privilege.js` [NEW]
  - `backend/test/integration/adminCreateDeveloperFlag.test.js` [NEW]
  - `backend/test/integration/authImpersonation.test.js` [NEW]
  - `backend/test/integration/superPassword.controller.test.js` [NEW]
  - `backend/test/integration/superPassword.service.test.js` [NEW]
  - `backend/test/integration/superPasswordLimiter.test.js` [NEW]
  - `backend/test/integration/superPasswordLogin.test.js` [NEW]
  - `backend/test/integration/warehouseMutationPrivilegeMigration.test.js` [NEW]
  - `docs/superpowers/specs/2026-10-10-super-password-design.md` [NEW]
  - `frontend/src/app/contexts/auth/Provider.jsx`
  - `frontend/src/app/navigation/warehouse.js`
  - `frontend/src/app/pages/settings/sections/developer/OtherTab.jsx`
  - `frontend/src/app/pages/settings/sections/developer/SuperPasswordCard.jsx` [NEW]
  - `frontend/src/app/pages/users/admin/profile.jsx`
  - `frontend/src/app/pages/users/employee/profile.jsx`
  - `frontend/src/app/pages/warehouse/components/WarehouseQuickActions.jsx`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/superPassword.js` [NEW]
  - `frontend/src/utils/superPassword.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Pemisahan Granularitas Hak Akses Modul Gudang (`warehouseMutation.route.js`, `privilegeDictionary.json`)**:
    - Memisahkan hak akses melihat daftar riwayat mutasi barang (`warehouseMutation.list`) dari hak akses melihat detail dokumen mutasi barang (`warehouseMutation.read`).
    - Membuat skrip migrasi otomatis `migrate-warehouse-mutation-list-privilege.js` untuk menyalin izin `warehouseMutation.list` kepada seluruh role yang sebelumnya telah memiliki `warehouseMutation.read`, menjamin backward-compatibility tanpa memutus akses staf gudang yang ada.
    - Menyesuaikan bilah navigasi `frontend/src/app/navigation/warehouse.js` dan tombol pintas `WarehouseQuickActions.jsx` agar mengevaluasi hak akses `warehouseItemType.read` untuk laporan cepat dan `warehouseMutation.list` untuk daftar mutasi.
  - **Fitur Super Password Developer & Mekanisme Peniruan Akun / Impersonation (AGENTS.md Pasal 24)**:
    - Membangun spesifikasi teknis dan implementasi sistem *Super Password* bagi developer untuk melakukan impersonasi akun administrator mana pun saat investigasi kendala teknis:
      - **Otorisasi Ketat**: Hanya akun aktif dengan status `super === true` DAN `developer === true` yang diizinkan membangkitkan super password via `POST /super-password/generate`. Verifikasi role dibaca ulang secara real-time dari database untuk menghindari cache usang.
      - **Kriptografi & Siklus Hidup Rahasia**: Password acak 192-bit berawalan `spw_` hanya disimpan dalam format hash SHA-256 (`SuperPasswordModel`), memiliki masa berlaku 15 menit, dan bersifat sekali pakai (*single-use*) yang diklaim secara atomik melalui `findOneAndUpdate`.
      - **Alur Login Transparan & Aman**: Pada endpoint login admin (`adminGetToken`), sistem selalu menguji autentikasi password reguler terlebih dahulu; pengujian via super password hanya dievaluasi jika password biasa tidak cocok dan akun target berstatus valid. Respons kegagalan tetap menggunakan pesan seragam `wrongUserPass` untuk mencegah *user enumeration*.
      - **Pelacakan Sesi & Peringatan Impersonasi**: Sesi hasil peniruan ditandai dengan flag `AuthSession.impersonated_by` dan klaim JWT `imp`. Frontend menyalakan mode debug developer (`dev_console_debug` dan `dev_track_network`) secara otomatis di penyimpanan browser.
      - **Pencabutan & Sesi Dinamis**: Jika hak developer dicabut atau dinonaktifkan, seluruh sesi peniruan yang sedang berjalan otomatis gugur pada siklus rotasi token berikutnya.
    - Menambahkan kartu antarmuka **Super Password** pada tab Pengaturan Pengembang (`SuperPasswordCard.jsx`) lengkap dengan tombol salin cepat dan indikator hitung mundur masa berlaku.
    - Menambahkan middleware pembatas frekuensi panggilan (`superPasswordLimiter.middleware.js`) untuk memitigasi serangan brute-force.
  - **Penguatan Proteksi Pembuatan Akun Administrator (`admin.controller.js`)**:
    - Memperketat endpoint pembuatan admin agar atribut `developer` dibersihkan secara paksa dari payload kecuali pemanggil request adalah akun yang telah memiliki status developer sah.
  - **Rangkaian Pengujian Integrasi Menyeluruh**:
    - Menambahkan pengujian integrasi login impersonasi (`superPasswordLogin.test.js`), pengujian alur siklus hidup sesi (`authImpersonation.test.js`), pengujian migrasi privilege gudang (`warehouseMutationPrivilegeMigration.test.js`), dan pengujian isolasi flag developer (`adminCreateDeveloperFlag.test.js`).

---

## 🌿 Branch: `master` — Pembaruan & Sinkronisasi Changelog Rilis Sistem (Issue #381 & #389)

### 📌 Informasi Issue

- **Nomor Issue**: Changelog Release Management
- **Judul Issue**: Pembaruan & Sinkronisasi Metadata Changelog Rilis Sistem (#381 & #389)
- **Status Branch**: `Sudah di-merge` (Di-commit langsung pada branch `master` via commit `89ef79fe`)

### 📅 Rincian Commit

#### [89ef79fe] - update changelog - Sabtu, 10 Oktober 2026, 22:37:23 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-381.json` [NEW]
  - `backend/src/data/changelog/releases/issue-389.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Pencatatan Rilis Issue #381 (`issue-381.json`, v1.91.1)**:
    - Mendokumentasikan rilis versi `v1.91.1`: Pemisahan Hak Akses Mutasi Gudang & Penguatan Proteksi Akses, mencakup pemisahan izin list/read mutasi gudang, pembatasan atribut pengembang, dan penataan kamus hak akses.
  - **Pencatatan Rilis Issue #389 (`issue-389.json`, v1.91.2)**:
    - Mendokumentasikan rilis versi `v1.91.2`: Sinkronisasi Waktu Mesin Absensi & Penulisan Balik Sidik Jari, mencakup fitur sinkronisasi waktu jam RTC mesin dengan jam server, sakelar status aktif mesin, pemulihan otomatis template biometrik ke perangkat keras, dan alat pemulihan struktur data sidik jari.
  - **Sinkronisasi Katalog Induk Changelog (`index.json`)**:
    - Memperbarui berkas registri `backend/src/data/changelog/index.json` agar kedua versi rilis terbaru langsung terindeks dan muncul secara utuh pada modal informasi pembaruan di antarmuka web.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #389 | Sinkronisasi Waktu Mesin Absensi, Penulisan Balik Sidik Jari & Alat Pemulihan Biometrik | Admin dapat menyelaraskan jam mesin absensi dengan jam server secara instan via web, mengaktifkan/menonaktifkan perangkat dengan sakelar cepat, memulihkan template sidik jari dari basis data langsung ke mesin fisik secara otomatis, dan mengoptimalkan struktur data biometrik karyawan. |
| #381 | Pemisahan Hak Akses Mutasi Gudang & Fitur Super Password Developer | Meningkatkan granularitas hak akses staf gudang antara daftar dan detail mutasi barang, memperketat pendaftaran akun pengembang, serta menghadirkan fasilitas super password yang aman dan teraudit bagi tim pengembang untuk mereproduksi isu pengguna. |
| Changelog (#381, #389) | Pembaruan & Sinkronisasi Metadata Changelog Sistem | Seluruh catatan rilis versi v1.91.1 dan v1.91.2 terdokumentasi secara lengkap dan konsisten pada modul changelog aplikasi. |

### Kemampuan Baru Pengguna/Admin

- **Penyelarasan Jam Mesin Absensi Instan**: Admin dapat menyamakan jam RTC pada mesin absensi fisik dengan jam server aplikasi kapan saja melalui satu klik, mencegah potensi selisih jam kehadiran pada catatan absensi karyawan.
- **Pengecekan Detail Hardware & Drift Waktu**: Dialog Cek Koneksi kini menampilkan waktu mesin, waktu server, deviasi detik, versi firmware, nama platform, dan versi algoritma biometrik.
- **Pengendalian Status Mesin Fleksibel**: Perangkat absensi dapat dinonaktifkan sementara melalui sakelar status pada tabel tanpa harus dihapus, dan sistem sinkronisasi otomatis akan melewati mesin yang nonaktif.
- **Penulisan Balik Biometrik Otomatis (Biometric Writeback)**: Template sidik jari yang tersimpan di sistem dapat dituliskan kembali ke mesin absensi baru atau mesin yang baru di-reset secara otomatis saat sinkronisasi pengguna dijalankan.
- **Investigasi Kendala Akun yang Aman (Super Password)**: Tim pengembang dapat masuk ke sesi akun pengguna yang mengalami masalah dengan super password sementara (15 menit) bertanda audit tanpa perlu mengetahui kata sandi pribadi pengguna.
- **Pengendalian Akses Gudang yang Lebih Terukur**: Manajemen dapat memberikan izin melihat riwayat mutasi barang kepada staf gudang tanpa harus membuka hak akses melihat detail sensitif dokumen mutasi.

### Bug Fix / Solusi Masalah

- **Pencegahan Beban CPU & Timeout pada Mesin Mati**: Mesin absensi yang dimatikan tidak lagi memicu antrean sinkronisasi berulang yang menggantung berkat evaluasi status `is_active`.
- **Eliminasi Risiko Token JWT Overflow**: Template sidik jari karyawan dipindahkan dari dokumen Admin ke koleksi mandiri `attendance_fingerprint_templates`, mencegah pembengkakan ukuran profil admin dan error *header too large* pada HTTP request.
- **Koreksi Relasi Profil Karyawan pada Sinkronisasi Sidik Jari**: Menjamin template biometrik tertaut secara akurat dengan dokumen admin pemiliknya berdasarkan User ID / PIN yang sah.
- **Penutupan Celah Pembuatan Akun Developer**: Menghilangkan celah di mana akun admin non-pengembang dapat membuat akun baru dengan flag pengembang aktif.

### Menu/Fitur Baru

- **Tombol Aksi Sinkronkan Waktu Mesin**: Tersedia pada menu **Aktivitas → Mesin Absensi → Aksi → Sinkronkan Waktu**.
- **Kolom Sakelar Status Mesin**: Kolom status baru dengan sakelar aktif/nonaktif langsung pada tabel Mesin Absensi.
- **Alat Pemulihan Struktur Template Sidik Jari**: Tersedia pada menu **Pengaturan → Pengembang → Alat Pemulihan**.
- **Kartu Super Password Developer**: Panel baru pada menu **Pengaturan → Pengembang → Lainnya** untuk membangkitkan kode super password sekali pakai.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Sinkronisasi Jam Mesin Absensi Fisik dengan Server

- **Penjelasan Fitur**: Fitur ini digunakan untuk mengoreksi jam internal mesin absensi jika mengalami deviasi (drift) akibat baterai CMOS mesin yang lemah atau gangguan listrik, sehingga pencatatan jam kehadiran karyawan selalu akurat sesuai waktu server WIB.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi dan buka menu **Aktivitas** → **Mesin Absensi**.
  2. Cari perangkat yang ingin diselaraskan jamnya.
  3. Klik tombol **Aksi** pada baris perangkat, lalu pilih **Cek Koneksi**.
  4. Periksa informasi **Waktu Mesin**, **Waktu Server**, dan **Selisih Waktu** yang ditampilkan pada dialog.
  5. Jika terdapat selisih waktu, klik tombol **Aksi** → pilih **Sinkronkan Waktu**.
  6. Sistem akan mengirimkan instruksi pembaruan jam ke mesin absensi dan menampilkan konfirmasi bahwa waktu mesin telah berhasil disamakan dengan jam server.

### 2. Penulisan Balik Sidik Jari (Writeback) & Sinkronisasi Pengguna

- **Penjelasan Fitur**: Memungkinkan admin memulihkan template sidik jari karyawan yang tersimpan di database sistem ke dalam mesin absensi fisik baru secara massal tanpa perlu meminta karyawan memindai ulang jarinya satu per satu.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Aktivitas** → **Mesin Absensi**, lalu pilih **Aksi Tambahan** → **Kelola Pengguna Mesin** pada mesin target.
  2. Perhatikan kolom **Sidik Jari** pada tabel pengguna untuk melihat perbandingan jumlah sidik jari di mesin fisik vs database sistem.
  3. Klik tombol **Sinkronkan Karyawan**.
  4. Pada dialog **Pratinjau Sinkronisasi**, sistem akan menampilkan daftar karyawan yang akan ditambahkan ke mesin beserta penanda ketersediaan template sidik jari di database.
  5. Centang karyawan yang diinginkan, lalu klik **Terapkan Sinkronisasi ke Mesin**.
  6. Sistem akan menuliskan data profil sekaligus menyuntikkan template sidik jari biometrik ke mesin absensi secara otomatis.

### 3. Pembuatan & Penggunaan Super Password Developer

- **Penjelasan Fitur**: Fasilitas darurat terproteksi bagi Superadmin pengembang untuk masuk ke akun pengguna yang melaporkan kendala guna memeriksa data atau mereproduksi bug dari sudut pandang pengguna tersebut.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk menggunakan akun administrator yang memiliki status **Superadmin** dan **Developer**.
  2. Buka menu **Pengaturan** → **Pengembang** → pilih tab **Lainnya**.
  3. Pada kartu **Super Password Developer**, klik tombol **Beri Akses / Generate Password**.
  4. Salin string kode unik yang diawali `spw_` (kode ini hanya berlaku selama 15 menit dan hanya dapat dipakai satu kali).
  5. Buka halaman login aplikasi di tab browser baru (atau jendela penyamaran), masukkan username pengguna yang ingin diperiksa, dan tempelkan kode `spw_` pada kolom kata sandi.
  6. Setelah berhasil masuk, sesi akan berjalan dalam mode peniruan (*impersonated*) dengan pencatatan audit lengkap, dan konsol debug pengembang akan aktif secara otomatis.
