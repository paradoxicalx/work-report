# 📝 Daily Work Report - Dedy S.N Putra (9 Oktober 2026)

---

## 📅 Laporan Harian - 9 Oktober 2026

---

## 🌿 Branch: `issue-386` — Issue #386: Manajemen Pengguna Mesin Absensi, Pendaftaran Sidik Jari Jarak Jauh & Pencadangan Data Mesin

### 📌 Informasi Issue

- **Nomor Issue**: #386
- **Judul Issue**: Manajemen Pengguna Mesin Absensi, Pendaftaran Sidik Jari Jarak Jauh (Remote Biometric Enrollment), Pencadangan/Pemulihan Pengguna Mesin, dan Sinkronisasi Profil Karyawan
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Jumat, 9 Oktober 2026, 20:41:46 WIB via commit `160cb271`, dengan penyesuaian lanjutan via commit `b1b82ddb`)

### 📅 Rincian Commit

#### [b1b82ddb] - resolve #386 - Jumat, 9 Oktober 2026, 20:56:28 WIB

- **Komponen yang Berubah**:
  - `frontend/src/app/pages/activities/attendanceDevice/enrollModal.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Koreksi Pemetaan Indeks 10 Jari Standar Firmware ZKTeco (`enrollModal.jsx`)**:
    - Menyelaraskan urutan opsi pemilih jari pada formulir pendaftaran sidik jari agar sesuai dengan pemetaan biner protokol firmware ZKTeco:
      - **Tangan Kiri (Indeks 0..4)**: `0` Kelingking Kiri, `1` Jari Manis Kiri, `2` Jari Tengah Kiri, `3` Telunjuk Kiri, `4` Jempol Kiri.
      - **Tangan Kanan (Indeks 5..9)**: `5` Jempol Kanan, `6` Telunjuk Kanan, `7` Jari Tengah Kanan, `8` Jari Manis Kanan, `9` Kelingking Kanan.
    - Mengubah jari default seleksi pendaftaran dari indeks sebelumnya ke **Telunjuk Kanan (Indeks #6)** yang merupakan jari paling umum digunakan pada absensi perkantoran.
  - **Sinkronisasi Terjemahan Dwibahasa (`translations.json`)**:
    - Memperbarui label penamaan kesepuluh jari pada berkas terjemahan Bahasa Indonesia dan Bahasa Inggris agar menampilkan nomor indeks firmware yang tepat (`fingerLeftLittle` s/d `fingerRightLittle`).

---

#### [160cb271] - resolve #386 - Jumat, 9 Oktober 2026, 20:41:46 WIB

_(Merge commit menggabungkan branch `issue-386` / commit `37da47ea` ke branch `master`)_

---

#### [37da47ea] - resolve #386 - Jumat, 9 Oktober 2026, 20:40:56 WIB

- **Komponen yang Berubah**:
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/attendanceDevice.controller.js`
  - `backend/src/lib/attendanceDeviceClient.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/admin.model.js`
  - `backend/src/models/attendanceDevice.model.js`
  - `backend/src/models/attendanceDeviceBackup.model.js` [NEW]
  - `backend/src/routes/attendanceDevice.route.js`
  - `backend/src/services/attendanceDevice.service.js`
  - `backend/src/services/attendanceDeviceSync.service.js`
  - `backend/test/unit/attendanceDeviceClient.test.js` [NEW]
  - `backend/test/unit/attendanceDeviceFingerprintSync.test.js` [NEW]
  - `backend/test/unit/attendanceDeviceUserManagement.test.js` [NEW]
  - `frontend/src/app/pages/activities/attendanceDevice/enrollModal.jsx` [NEW]
  - `frontend/src/app/pages/activities/attendanceDevice/index.jsx`
  - `frontend/src/app/pages/activities/attendanceDevice/schema/DeviceExtraActions.jsx`
  - `frontend/src/app/pages/activities/attendanceDevice/schema/mappingColumns.jsx`
  - `frontend/src/app/pages/activities/attendanceDevice/userManager.jsx` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Abstraksi Klien Komunikasi Perangkat Tingkat Rendah (`backend/src/lib/attendanceDeviceClient.js`)**:
    - Membangun interface klien protokol ZKTeco terpusat yang mengisolasi ketergantungan library TCP/UDP pihak ketiga dari lapisan service bisnis.
    - Menambahkan metode `fetchUsers` untuk menarik seluruh pengguna terdaftar pada mesin absen dengan pembersihan karakter null-byte (`\0`).
    - Menambahkan metode `saveUser` (`setUser`) dengan validasi batas UID (1..3000), sanitasi User ID (maks 9 karakter), panjang nama (maks 24 karakter), peran hak akses mesin (0 = User, 14 = Device Admin), dan nomor kartu RFID.
    - Menambahkan metode `removeUser` (`deleteUser`) untuk menghapus pengguna dari memori mesin secara waktu nyata.
    - Menambahkan mekanisme penguncian sementara mesin saat penulisan masal (`disableDevice` / `enableDevice`) dan penyegaran cache memori perangkat (`CMD_REFRESHDATA` / kode 1013).
    - Mengembangkan penarikan biner template biometrik (`fetchFingerprintTemplates` / `decodeFingerprintTemplatesBuffer`) yang mendekode header record 6-byte (`size`, `uid`, `fid`, `valid`) menjadi string representasi Base64.
    - Mengimplementasikan pemicu pendaftaran jarak jauh (`startEnrollment` / `CMD_STARTENROLL` kode 61) dengan dukungan dua varian payload perangkat:
      - **Varian A (Layar TFT/Color Screen Modern)**: Payload biner 26-byte (`userId` 24-byte ASCII null-padded + 1-byte `fingerIndex` + 1-byte flag).
      - **Varian B (Layar Numerik/Black & White Compact)**: Payload biner 4-byte (`userId` uint16le + `fingerIndex` uint16le).
    - Menambahkan perintah pembatalan capture (`CMD_CANCELCAPTURE` kode 62) dan reset mode verifikasi (`CMD_STARTVERIFY` kode 60).
  - **Skema Basis Data Pencadangan & Biometrik Karyawan (`models/attendanceDeviceBackup.model.js`, `admin.model.js`, `attendanceDevice.model.js`)**:
    - Membuat model baru `AttendanceDeviceBackup` dengan dukungan soft-delete untuk mengabadikan rekam jejak pengguna mesin (UID, User ID/PIN, nama, role, cardno, password terenkripsi) beserta catatan dan pembuat snapshot.
    - Menambahkan array `fingerprint_templates` pada model `Admin` untuk memetakan template sidik jari karyawan berdasarkan indeks jari (0..9), referensi mesin asal, dan waktu pembaruan.
    - Menambahkan cache `registered_users` pada model `AttendanceDevice` untuk mendokumentasikan daftar pengguna aktif terakhir pada perangkat.
  - **Layanan Bisnis & REST API Manajemen Mesin Absensi (`attendanceDevice.service.js`, `attendanceDevice.controller.js`, `attendanceDevice.route.js`)**:
    - `GET /attendance-device/devices/:id/users`: Mengambil daftar pengguna langsung dari mesin secara live dengan perlindungan keamanan ketat (kata sandi tidak pernah dikirimkan ke frontend).
    - `POST /attendance-device/devices/:id/users`: Membuat atau memperbarui pengguna pada mesin, termasuk verifikasi benturan UID dan User ID.
    - `DELETE /attendance-device/devices/:id/users/:uid`: Menghapus pengguna tertentu dari mesin absensi.
    - `POST /attendance-device/devices/:id/backup`: Menyimpan snapshot cadangan seluruh pengguna aktif dari mesin ke basis data MongoDB.
    - `POST /attendance-device/devices/:id/backups`: Menampilkan riwayat snapshot cadangan per perangkat dalam format Datatable.
    - `GET /attendance-device/backups/:backupId` & `DELETE ...`: Mengambil detail isi cadangan dan menghapus data cadangan lama.
    - `POST /attendance-device/devices/:id/restore`: Memulihkan pengguna dari snapshot cadangan ke mesin absen (dapat memulihkan seluruh pengguna atau hanya UID terpilih).
    - `GET /attendance-device/devices/:id/sync-preview`: Menganalisis perbedaan data pengguna mesin dengan data karyawan di database sistem, menampilkan pratinjau tindakan (*Create*, *Update*, atau *In Sync*) serta alokasi slot UID berikutnya secara otomatis.
    - `POST /attendance-device/devices/:id/sync-users`: Menjalankan sinkronisasi massal data karyawan ke mesin absen dengan penanganan isolasi kesalahan per-item.
    - `POST /attendance-device/devices/:id/sync-fingerprints`: Menarik seluruh sidik jari dari mesin dan menyimpannya ke profil karyawan yang cocok berdasarkan nomor ID karyawan.
    - `POST /attendance-device/devices/:id/enroll-fingerprint`: Memicu mesin absensi ke mode pendaftaran sidik jari jarak jauh untuk karyawan terpilih.
  - **Keamanan, Redaksi Kata Sandi & Hak Akses Berbasis Peran (RBAC)**:
    - Menerapkan zero-secret logging: kata sandi keypad mesin diabaikan dari pencatatan log sistem dan tidak pernah dipaparkan ke antarmuka klien.
    - Menambahkan hak akses terstandarisasi pada `privilegeDictionary.json`: `attendanceDevice.read` dan `attendanceDevice.update`.
  - **Antarmuka Pengelolaan Pengguna Mesin Absensi (`userManager.jsx`, `DeviceExtraActions.jsx`, `mappingColumns.jsx`)**:
    - Mengembangkan panel antarmuka lengkap untuk mengelola pengguna mesin absen secara real-time.
    - Menampilkan tabel pengguna terdaftar di mesin dengan informasi UID, PIN/ID, Nama, Peran (Pengguna / Admin Mesin), dan Nomor Kartu RFID.
    - Menyediakan modal formulir tambah/edit pengguna, modal konfirmasi penghapusan pengguna, serta drawer/modal riwayat pencadangan & pemulihan.
    - Menyediakan dialog pratinjau sinkronisasi karyawan (*Sync Preview Modal*) yang menunjukkan perbandingan data sebelum disimpan ke perangkat keras.
  - **Wizard Interaktif Pendaftaran Sidik Jari Jarak Jauh (`enrollModal.jsx`)**:
    - Membangun modal panduan langkah demi langkah pendaftaran sidik jari (Remote Fingerprint Enrollment).
    - Menyajikan antarmuka visual pemilihan 10 jari tangan dengan indikator grafis.
    - Menyediakan panduan instruksi interaktif yang memandu operator untuk meminta karyawan menempelkan jari 3 kali pada sensor mesin setelah perintah dikirim.
    - Menyediakan tombol pintas sinkronisasi otomatis sidik jari ke profil karyawan sistem segera setelah pemindaian fisik di mesin selesai.
  - **Rangkaian Pengujian Unit Otomatis**:
    - Menambahkan pengujian unit menyeluruh untuk decoding buffer sidik jari (`attendanceDeviceClient.test.js`).
    - Menambahkan pengujian sinkronisasi sidik jari ke profil admin (`attendanceDeviceFingerprintSync.test.js`).
    - Menambahkan pengujian logika manajemen pengguna, backup, dan rekonsiliasi slot UID (`attendanceDeviceUserManagement.test.js`).

---

## 🌿 Branch: `master` — Pembaruan & Sinkronisasi Changelog Rilis Sistem (Issue #377, #378, #379, #386)

### 📌 Informasi Issue

- **Nomor Issue**: Changelog Release Management
- **Judul Issue**: Pembaruan & Sinkronisasi Metadata Changelog Rilis Sistem (#377, #378, #379, #386)
- **Status Branch**: `Sudah di-merge` (Di-commit langsung pada branch `master` via commit `5666c4c2`)

### 📅 Rincian Commit

#### [5666c4c2] - update changelog - Jumat, 9 Oktober 2026, 20:50:02 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-377.json` [NEW]
  - `backend/src/data/changelog/releases/issue-378.json` [NEW]
  - `backend/src/data/changelog/releases/issue-379.json` [NEW]
  - `backend/src/data/changelog/releases/issue-386.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Pencatatan Rilis Baru Fitur Manajemen Mesin Absensi (`issue-386.json`, v1.91.0)**:
    - Mendokumentasikan rilis versi `v1.91.0` yang mencakup fitur pengelolaan pengguna mesin absensi, pendaftaran sidik jari jarak jauh, pencadangan/pemulihan, serta pemetaan profil karyawan otomatis.
  - **Melengkapi Entri Changelog Rilis Sebelumnya yang Tertinggal**:
    - **Issue #377 (`issue-377.json`, v1.89.2)**: Riwayat Pengiriman WhatsApp Faktur & Penanda Kanal Percakapan — mendokumentasikan panel pelacakan status pesan WhatsApp (Terkirim, Diterima, Dibaca, Gagal) pada detail faktur penjualan dan penanda kanal resmi/pribadi pada tabel riwayat obrolan.
    - **Issue #378 (`issue-378.json`, v1.89.3)**: Fleksibilitas Tanggal Tagihan Penjualan & Validasi Jatuh Tempo — mendokumentasikan penyesuaian tanggal terbit tagihan belum lunas, kewajiban alasan audit, pembatasan pergeseran faktur pajak dalam bulan yang sama, dan proteksi periode tutup buku.
    - **Issue #379 (`issue-379.json`, v1.90.0)**: Formulir Publik Ubah Layanan & Alur Verifikasi Dua Tahap — mendokumentasikan portal publik perubahan paket internet mandiri tanpa login, tab verifikasi admin helpdesk & accounting, laci detail permintaan web, jejak proses (timeline) kronologis, dan pengamanan honeypot/rate-limit.
  - **Sinkronisasi Berkas Induk Changelog (`index.json`)**:
    - Memperbarui daftar katalog rilis pada `backend/src/data/changelog/index.json` agar modal notifikasi "What's New" pada antarmuka frontend dapat membaca seluruh riwayat pembaruan sistem secara utuh dan terurut.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #386 | Manajemen Pengguna Mesin Absensi & Pendaftaran Sidik Jari Jarak Jauh | Administrator kini dapat mengelola seluruh pengguna mesin absensi langsung dari browser (tambah, edit, hapus, backup, restore), memicu sensor mesin absen untuk pendaftaran sidik jari jarak jauh dengan panduan visual 10 jari, serta menyinkronkan data karyawan sistem ke mesin secara otomatis. |
| Changelog (#377, #378, #379, #386) | Pembaruan & Sinkronisasi Metadata Changelog Sistem | Seluruh catatan rilis fitur utama terdahulu (pelacakan WhatsApp faktur, fleksibilitas tanggal tagihan, formulir publik ubah layanan) dan rilis terbaru mesin absensi terdokumentasi rapi di sistem sehingga pengguna dan admin dapat meninjau pembaruan sistem secara transparan. |

### Kemampuan Baru Pengguna/Admin

- **Kontrol Penuh Pengguna Mesin Absensi via Web**: Admin tidak perlu lagi berdiri di depan mesin absensi untuk mendaftarkan nama karyawan atau mengatur hak akses mesin; semua dapat dilakukan langsung melalui antarmuka web Dekasimal.
- **Pendaftaran Sidik Jari Jarak Jauh (Remote Enrollment Wizard)**: Operator dapat memilih karyawan dan jari yang ingin didaftarkan, lalu menekan tombol pemicu di aplikasi web; mesin absensi yang dituju akan langsung masuk ke mode scan sidik jari di layarnya secara otomatis.
- **Pencadangan & Pemulihan Mesin Antar-Perangkat**: Admin dapat membuat snapshot cadangan data pengguna mesin sebelum melakukan reset atau pergantian perangkat, lalu memulihkan data tersebut kapan saja ke mesin yang sama atau mesin pengganti.
- **Deteksi Slot & Rekonsiliasi Karyawan Otomatis**: Sistem secara cerdas mendeteksi karyawan aktif yang belum terdaftar di mesin, mengalokasikan slot UID kosong secara otomatis, dan memberikan pratinjau perubahan sebelum data ditulis ke perangkat keras.
- **Transparansi Riwayat Pembaruan Aplikasi**: Pengguna dapat melihat detail fitur-fitur baru dan peningkatan performa sistem secara lengkap melalui modal informasi pembaruan di aplikasi.

### Bug Fix / Solusi Masalah

- **Pencegahan Kehilangan Data Angka Nol pada Form Input**: Memperbaiki logika sanitasi formulir agar nilai angka nol (`0`) seperti Jempol Kanan (indeks jari 0 pada logika tertentu atau kartu RFID 0) tidak terhapus oleh utilitas sanitasi data.
- **Koreksi Penataan Indeks Jari ZKTeco**: Menyelaraskan urutan indeks 10 jari pada antarmuka pengguna agar cocok 1:1 dengan standar firmware ZKTeco (0..4 Tangan Kiri, 5..9 Tangan Kanan), mencegah tertukarnya rekaman sidik jari tangan kiri dan tangan kanan.
- **Proteksi Kebocoran Kata Sandi Keypad Mesin**: Menjamin kata sandi mesin absensi berstatus *write-only*, tidak pernah dipaparkan ke response API atau dicatat ke log Winston sistem.
- **Pencegahan Eksepsi Null / Data Not Found**: Memperbaiki respons status code (menggunakan 404/422 yang sesuai) saat data perangkat atau snapshot backup tidak ditemukan, menggantikan error 500 generik.

### Menu/Fitur Baru

- **Tombol & Panel Pengelolaan Pengguna Mesin (`userManager.jsx`)**: Dapat diakses melalui menu **Aktivitas → Mesin Absensi → Aksi Tambahan → Kelola Pengguna Mesin**.
- **Modal Pendaftaran Sidik Jari Jarak Jauh (`enrollModal.jsx`)**: Dapat diakses langsung dari daftar pengguna mesin atau tombol aksi cepat pada tabel mesin absensi.
- **Modal Pencadangan & Pemulihan Pengguna Mesin**: Antarmuka untuk membuat snapshot, meninjau riwayat backup, dan merestorasi pengguna tertentu ke mesin.
- **Katalog Rilis Changelog Lengkap**: Penambahan catatan rilis v1.89.2, v1.89.3, v1.90.0, dan v1.91.0 pada modul Changelog aplikasi.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Pendaftaran Sidik Jari Karyawan Jarak Jauh (Remote Enrollment)

- **Penjelasan Fitur**: Fitur ini memungkinkan HR/Admin memicu mode pendaftaran sidik jari pada mesin absensi tertentu dari jarak jauh melalui browser. Setelah dipicu, mesin fisik akan langsung mengeluarkan instruksi suara/layar agar karyawan menempelkan jarinya sebanyak 3 kali, dan template sidik jari yang berhasil direkam dapat langsung ditarik ke sistem.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Aktivitas** → **Mesin Absensi**.
  2. Klik tombol **Daftarkan Sidik Jari** (atau melalui baris perangkat → **Aksi** → **Daftarkan Sidik Jari**).
  3. Pada modal pendaftaran:
     - Pilih **Mesin Absensi Tujuan** tempat karyawan saat ini berdiri.
     - Pilih nama **Karyawan** yang akan didaftarkan.
     - Pilih **Jari yang Didaftarkan** (bawaan sistem: *Telunjuk Kanan / Index #6*).
  4. Pastikan karyawan sudah berdiri di depan mesin absensi yang dipilih.
  5. Klik tombol **Mulai Pendaftaran di Mesin**.
  6. Mesin absensi akan berbunyi "beep" dan menampilkan panduan scan di layarnya. Minta karyawan menempelkan jarinya sebanyak 3 kali berturut-turut pada sensor mesin hingga terdengar konfirmasi berhasil.
  7. Klik tombol **Tarik & Sinkronkan Sidik Jari ke Profil Karyawan** untuk menyimpan salinan template biometrik ke database sistem.

### 2. Pengelolaan Pengguna & Sinkronisasi Massal ke Mesin Absensi

- **Penjelasan Fitur**: Modul ini menyajikan daftar pengguna yang tersimpan di dalam memori mesin absensi dan menyediakan sarana untuk menyinkronkan data karyawan aktif ke mesin secara massal tanpa perlu input manual satu per satu di keypad mesin.
- **Langkah Penggunaan (Tutorial)**:
  1. Pada menu **Aktivitas** → **Mesin Absensi**, cari perangkat yang dituju lalu klik opsi **Aksi Tambahan** → **Kelola Pengguna Mesin**.
  2. Sistem akan terhubung ke mesin via TCP socket dan menampilkan seluruh pengguna yang ada di mesin secara langsung.
  3. **Untuk Menambah / Mengubah Pengguna Satuan**:
     - Klik **Tambah Pengguna Baru** untuk mengalokasikan UID baru, mengisi Nama, User ID (PIN), Nomor Kartu RFID, dan Hak Akses (Pengguna Biasa / Administrator Mesin).
     - Atau klik ikon pensil pada baris pengguna untuk memperbarui informasi.
  4. **Untuk Sinkronisasi Otomatis dari Database Karyawan**:
     - Klik tombol **Sinkronkan Karyawan**.
     - Modal **Pratinjau Sinkronisasi** akan membandingkan daftar karyawan di sistem dengan pengguna di mesin, menandai mana karyawan yang baru akan ditambahkan (*Create*) dan mana yang perlu diperbarui namanya (*Update*).
     - Pilih karyawan yang ingin disinkronkan, lalu klik **Terapkan Sinkronisasi ke Mesin**. Data akan langsung tertulis ke memori mesin absensi.

### 3. Pencadangan & Pemulihan (Backup & Restore) Pengguna Mesin Absensi

- **Penjelasan Fitur**: Fasilitas untuk menyimpan snapshot seluruh daftar pengguna mesin absensi ke basis data pusat sebagai langkah pencegahan sebelum melakukan servis perangkat keras, reset pabrik, atau migrasi data ke mesin baru.
- **Langkah Penggunaan (Tutorial)**:
  1. Pada panel **Kelola Pengguna Mesin**, klik tombol **Pencadangan (Backup)**.
  2. Masukkan **Nama Cadangan** (contoh: `Backup Rutin Oktober 2026`) dan catatan tambahan bila diperlukan.
  3. Klik **Buat Snapshot Cadangan**. Seluruh daftar pengguna saat ini akan tersimpan dengan aman di basis data MongoDB.
  4. Jika di kemudian hari mesin mengalami reset atau diganti dengan unit baru:
     - Buka tab **Riwayat Cadangan**.
     - Pilih snapshot cadangan yang diinginkan, lalu klik **Pulihkan (Restore)**.
     - Pilih apakah ingin memulihkan seluruh pengguna atau hanya pengguna tertentu, kemudian konfirmasikan proses restore. Seluruh data pengguna akan ditulis ulang ke mesin absensi tujuan.
