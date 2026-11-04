# 📝 Daily Work Report - Dedy S.N Putra (2026-11-04)

---

## 📅 Laporan Harian - 4 November 2026

---

## 🌿 Branch: `issue-526` — Microservice OLT Gateway, Dialect CLI Huawei, Manajemen Perangkat OLT & Sinkronisasi ONT GPON

### 📌 Informasi Issue

- **Nomor Issue**: #526
- **Judul Issue**: Implementasi Microservice OLT Gateway, Integrasi CLI Huawei (MA5600/MA5683T), Manajemen Perangkat OLT, serta Sinkronisasi Daftar ONT, Autofind, dan Redaman Optik
- **Status Branch**: `Belum di-merge` (Dalam pengembangan aktif di branch `issue-526`, ahead 3 commit dari remote, dengan pengujian microservice 100% lulus)

### 📅 Rincian Commit

#### [8dade3db] - save #526 detail ONT langsung dari OLT (info + redaman bila online) - 04 November 2026, 23:01:55 WIB

- **Komponen yang Berubah**:
  - `olt-gateway/src/services/ontDetail.service.js` [NEW]
  - `olt-gateway/test/ontDetail.service.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Layanan Pembacaan Detail & Redaman Optik On-Demand**: Mengimplementasikan `ontDetail.service.js` untuk membaca detail informasi ONT individual dan parameter optik (*Rx/Tx Optical Power*) secara langsung (*live*) dari OLT via CLI.
  - **Optimasi Eksekusi Perintah Cerdas**: Menambahkan pengecekan status ONT sebelum mengeksekusi pembacaan optik; perintah pembacaan redaman (`display ont optical-info`) hanya dieksekusi jika status ONT terdeteksi `online`, sehingga mengeliminasi pemborosan waktu tunggu (*timeout*) pada perangkat yang sedang padam/mati.
  - **Pengujian Komprehensif**: Menambahkan test suite `ontDetail.service.test.js` yang memvalidasi parsing detail ONT, pembacaan redaman saat online, penanganan status offline, hingga toleransi kegagalan dialect/timeout.

---

#### [551d262b] - save #526 sinkronisasi ONT: perencana port, operasi rekonsiliasi, dan layanan - 04 November 2026, 23:01:08 WIB

- **Komponen yang Berubah**:
  - `olt-gateway/src/models/oltDevice.model.js`
  - `olt-gateway/src/models/oltOnt.model.js` [NEW]
  - `olt-gateway/src/repositories/oltOntRepo.js` [NEW]
  - `olt-gateway/src/repositories/ontScan.js` [NEW]
  - `olt-gateway/src/services/ontSync.service.js` [NEW]
  - `olt-gateway/src/services/ponPlan.js` [NEW]
  - `olt-gateway/test/ontScan.test.js` [NEW]
  - `olt-gateway/test/ontSync.service.test.js` [NEW]
  - `olt-gateway/test/ponPlan.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Perencana Port GPON Otomatis (`ponPlan.js`)**: Mengembangkan modul perencana port yang mengurai snapshot board hardware OLT untuk menentukan daftar port GPON aktif (rentang 0..n-1 per slot GPON yang terpasang) tanpa menebak atau memindai slot kosong.
  - **Model Data & Repositori ONT (`oltOnt.model.js` & `oltOntRepo.js`)**: Mendefinisikan schema dan repository MongoDB untuk penyimpanan ONT GPON mencakup serial number (SN), status koneksi (online/offline), jarak (meter), deskripsi/nama pelanggan, vendor ONT, status autofind, serta pencatatan audit `first_seen_at` dan `last_seen_at`.
  - **Rekonsiliasi Atomik Port-by-Port (`ontScan.js`)**: Menerapkan pola *bulk write* cerdas per port: melakukan upsert untuk ONT yang ditemukan aktif dan membersihkan (*pruning*) data ONT yang sudah tidak lagi terdaftar pada port tersebut tanpa mengganggu integritas port lain.
  - **Layanan Orkestrasi Sinkronisasi ONT (`ontSync.service.js`)**: Mengorkestrasi pemindaian seluruh port GPON, pemindaian ONT *autofind* (ONT belum terkonfigurasi), proteksi konkurensi dengan versi dokumen Mongoose `__v`, serta penerapan *cooldown* 10 detik antar pemanggilan.

---

#### [f0d2b570] - save #526 dialect perintah ONT dan parser daftar ONT, autofind, detail, redaman - 04 November 2026, 22:59:32 WIB

- **Komponen yang Berubah**:
  - `olt-gateway/src/constants.js`
  - `olt-gateway/src/dialects/huawei/ma5600.js`
  - `olt-gateway/src/dialects/huawei/parsers.js`
  - `olt-gateway/src/services/errorMapper.js`
  - `olt-gateway/test/dialects.test.js`
  - `olt-gateway/test/errorMapper.test.js`
  - `olt-gateway/test/fixtures/huawei/autofind-empty.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/autofind-one.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/ont-detail-offline.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/ont-detail-online.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/ont-list-empty.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/ont-list-lab.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/ont-list-mismatch.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/ont-list-online.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/ont-not-found.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/ont-optical-offline.txt` [NEW]
  - `olt-gateway/test/fixtures/huawei/ont-optical-online.txt` [NEW]
  - `olt-gateway/test/ontParsers.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Dialect Perintah CLI Huawei ONT**: Menambahkan deklarasi perintah *read-only* pada dialect MA5600: `display ont info summary`, `display ont autofind`, `display ont info`, dan `display ont optical-info`.
  - **Parser Regex Non-Backtracking Berkinerja Tinggi**:
    - `parseOntList`: Mengekstrak tabel ONT per F/S/P lengkap dengan ID, SN, status, jarak, dan deskripsi dengan validasi ketat kecocokan jumlah baris terhadap footer total perangkat.
    - `parseAutofind`: Mengekstrak daftar ONT baru yang terdeteksi secara otomatis di port PON, membersihkan parameter sensitif (password dihilangkan).
    - `parseOntDetail`: Mengurai profil ONT, tipe ONT, versi software/firmware, run-state, dan waktu offline terakhir.
    - `parseOntOptical`: Mengurai data redaman optik Rx/Tx Power dalam satuan dBm, tegangan, bias current, dan temperatur modul optik.
  - **Fixture Data Nyata OLT Lab**: Menambahkan 11 file fixture tangkapan nyata dari OLT lab Huawei MA5683T untuk menguji toleransi parser terhadap berbagai variasi output vendor.

---

#### [c6009989] - save #526 - 04 November 2026, 17:02:23 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/.env.example`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/olt.controller.js` [NEW]
  - `backend/src/data/aiNavigationRoutes.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/middlewares/logger.middleware.js`
  - `backend/src/models/oltDevice.model.js` [NEW]
  - `backend/src/routes/olt.route.js` [NEW]
  - `backend/src/services/olt.service.js` [NEW]
  - `backend/src/services/oltGateway.service.js` [NEW]
  - `backend/src/services/serviceKeys.registry.js`
  - `backend/src/services/serviceKeys.service.js`
  - `backend/src/utils/oltCredentialCrypto.js` [NEW]
  - `backend/src/utils/oltInput.js` [NEW]
  - `backend/test/integration/olt.controller.test.js` [NEW]
  - `backend/test/integration/olt.service.test.js` [NEW]
  - `backend/test/integration/oltDevice.model.test.js` [NEW]
  - `backend/test/unit/loggerSanitizer.test.js`
  - `backend/test/unit/oltCredentialCrypto.test.js` [NEW]
  - `backend/test/unit/oltGateway.service.test.js` [NEW]
  - `backend/test/unit/oltInput.test.js` [NEW]
  - `frontend/src/app/navigation/networks.js`
  - `frontend/src/app/pages/network/oltManagement/components/OltDrawerFrame.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/components/OltFormFields.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/components/OltSnapshotSection.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/components/OltSnapshotSection.test.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/create.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/detail.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/edit.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/index.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/schema/columns.test.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/schema/oltSchema.js` [NEW]
  - `frontend/src/app/pages/network/oltManagement/utils/oltForm.js` [NEW]
  - `frontend/src/app/pages/network/oltManagement/utils/oltForm.test.js` [NEW]
  - `frontend/src/app/pages/network/oltManagement/utils/oltStatus.js` [NEW]
  - `frontend/src/app/pages/network/oltManagement/utils/oltStatus.test.js` [NEW]
  - `frontend/src/app/router/network/olt.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/networkLogRedaction.js`
  - `frontend/src/utils/networkLogRedaction.test.js`
  - Arsitektur Microservice Lengkap `olt-gateway/*` (65 berkas baru):
    - `Dockerfile`, `docker-compose.yml`, `package.json`, `nodemon.json`
    - `src/app.js`, `src/config.js`, `src/constants.js`, `src/container.js`, `src/server.js`, `src/swagger.js`
    - `src/transport/cliSession.js`, `src/transport/sshChannel.js`, `src/transport/telnetChannel.js`, `src/transport/telnetNegotiation.js`
    - `src/safety/commandGuard.js`, `src/sessions/oltQueue.js`
    - `src/dialects/defineDialect.js`, `src/dialects/registry.js`, `src/dialects/huawei/*`
    - `src/crypto/credentialCrypto.js`, `src/services/sync.service.js`, `src/services/errorMapper.js`
    - `src/routes/internal.route.js`, `src/controllers/internal.controller.js`
    - `test/*` (suite pengujian komprehensif dengan OLT mock SSH & Telnet)
- **Deskripsi Perubahan & Fungsi**:
  - **Pondasi Microservice `olt-gateway` Terisolasi**:
    - Dibangun khusus pada port 3060 untuk menjadi gerbang tunggal sesi telnet/SSH ke perangkat fisik OLT. Menghindari masalah perebutan sesi koneksi CLI pada OLT (Huawei memiliki batas ±4–8 sesi konkuren) saat backend diskalakan multi-instance.
    - **Pagar Keamanan CLI Berlapis (`commandGuard.js`)**: Menjamin perintah hanya dirakit dari templat *whitelisted* dengan parameter tipe data valid; mode *write* diblokir total pada layer transport demi menjamin keamanan perangkat produksi.
    - **Penanganan Protokol Telnet & SSH Presisi**:
      - Menyertakan mesin negosiasi opsi Telnet otomatis (menolak negosiasi opsi berbahaya via WONT/DONT dan menyaring karakter IAC).
      - Menyesuaikan penanganan terminator baris: SSH hanya mengirim `CR` (mencegah bug di mana `LF` dihitung sebagai penekanan Enter kedua pada pty Huawei), sedangkan Telnet mengirim `CRLF`.
    - **Antrean Sesi & Sinkronisasi Hardware OLT**: Serialisasi koneksi per OLT dengan pembacaan versi sistem (`display version`) dan inventaris board (`display board`).
    - **Enkripsi Kredensial AES-256-GCM**: Kredensial login & password enable OLT dienkripsi at-rest menggunakan kunci 256-bit dengan auth tag 128-bit; nilai plaintext tidak pernah dibocorkan ke log.
  - **Backend OLT Management API (`/api/v1/olt`)**:
    - Skema Mongoose `oltDevice.model.js` dengan proteksi IP unik (soft-delete safe), status konektivitas, snapshot hardware, dan audit trail.
    - Controller & Service lengkap untuk CRUD OLT, pengetesan sinkronisasi manual (`/sync`), dan pemanggilan ke `olt-gateway`.
    - Hak akses granular: `olt.list`, `olt.create`, `olt.read`, `olt.update`, `olt.delete`, `olt.sync`, dan `olt.readSensitive`.
  - **Frontend Antarmuka Pengguna Manajemen OLT**:
    - Menu navigasi baru di bawah grup *Jaringan* (`/network/olt`).
    - Datatable interaktif dengan badge status koneksi dinamis (*Connected*, *Authentication Failed*, *Unreachable*, dll.), ringkasan slot board, dan tombol *Trigger Sync*.
    - Form penambahan dan modifikasi OLT dengan dukungan protokol Telnet/SSH, port kustom, kredensial bertopeng, serta tab detail informasi snapshot hardware.

---

### 📝 Pekerjaan Belum Di-commit (Work In Progress)

- **Komponen**:
  - `olt-gateway/.env.example`
  - `olt-gateway/src/app.js`
  - `olt-gateway/src/config.js`
  - `olt-gateway/src/container.js`
  - `olt-gateway/src/controllers/internal.controller.js`
  - `olt-gateway/src/routes/internal.route.js`
  - `olt-gateway/src/server.js`
  - `olt-gateway/test/internal.route.test.js`
- **Deskripsi Progres**:
  - Menghubungkan endpoint REST internal pada `olt-gateway`:
    - `POST /internal/olts/:olt_id/sync-ont`: Memicu sinkronisasi ONT GPON menyeluruh (daftar ONT aktif per port, deteksi autofind, dan rekonsiliasi database).
    - `POST /internal/olts/:olt_id/ont-detail`: Membaca detail status dan redaman optik ONT secara instan dengan validasi koordinat Frame/Slot/Port/ONT ID.
  - Menambahkan konfigurasi batas waktu sinkronisasi ONT `OLT_ONT_SYNC_TIMEOUT_MS=120000` (2 menit) untuk mengakomodasi pemindaian puluhan port GPON.
  - Unit test HTTP route `internal.route.test.js` telah diperbarui dan berhasil memvalidasi seluruh rute baru beserta penanganan error-nya.

---

## 🌿 Branch: `issue-524` — Perbaikan Detail Notifikasi Telegram Autentikasi Broadband & Kompilasi Dokumentasi Teknis Huawei OLT

### 📌 Informasi Issue

- **Nomor Issue**: #524
- **Judul Issue**: Perbaikan Nilai Field Notifikasi Telegram saat Penambahan Akun Broadband & Kompilasi Panduan Referensi Perintah CLI Huawei OLT
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `3b2d406e` dan telah dideploy ke `origin/production` melalui commit `9aa99fb0`)

### 📅 Rincian Commit

#### [3b2d406e] / [44940e24] - resolve #524 - 04 November 2026, 16:03:39 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/test/integration/radiusAuthentication.createNotification.test.js` [NEW]
  - `documentations/huawei-olt-reference/01-system-and-hardware.md` [NEW]
  - `documentations/huawei-olt-reference/02-vlan-and-service-port.md` [NEW]
  - `documentations/huawei-olt-reference/03-profiles-and-qos.md` [NEW]
  - `documentations/huawei-olt-reference/04-gpon-and-ont-management.md` [NEW]
  - `documentations/huawei-olt-reference/05-diagnostics-and-troubleshooting.md` [NEW]
  - `documentations/huawei-olt-reference/06-dashboard-integration-guide.md` [NEW]
  - `documentations/huawei-olt-reference/commands-config-mode.md` [NEW]
  - `documentations/huawei-olt-reference/commands-diagnose-mode.md` [NEW]
  - `documentations/huawei-olt-reference/commands-display-reference.md` [NEW]
  - `documentations/huawei-olt-reference/commands-giu-uplink.md` [NEW]
  - `documentations/huawei-olt-reference/commands-gpon-interface.md` [NEW]
  - `documentations/huawei-olt-reference/commands-privileged-mode.md` [NEW]
  - `documentations/huawei-olt-reference/commands-user-mode.md` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Perbaikan Anomali Notifikasi Telegram Tiket Instalasi (`radiusAuthentication.controller.js`)**:
    - Memperbaiki bug di mana pesan notifikasi Telegram yang dikirimkan ke grup teknisi saat akun broadband dibuat menampilkan teks literal `undefined` pada baris Password dan IP Address.
    - Menambahkan penanganan fallback yang kuat: jika password tidak tersedia di objek return, controller membaca dari payload input `data.password` atau fallback `-`. Untuk IP Address, jika tidak menggunakan IP statis, otomatis mencantumkan `Dynamic (Pool)`.
  - **Penyelarasan Return Data Layanan (`radiusAuthentication.service.js`)**:
    - Memperbarui fungsi `createNewAuthentication` agar secara eksplisit mengembalikan field `password`, `ip_static`, `status`, dan `activated` selain ID dan username, memastikan modul pemanggil menerima representasi lengkap dokumen akun yang baru dibuat.
  - **Pengujian Integrasi Notifikasi (`radiusAuthentication.createNotification.test.js`)**:
    - Membuat automated test suite menggunakan mock Express dan spy Telegram untuk memverifikasi bahwa pesan notifikasi tidak lagi mengandung string `undefined`, baik untuk akun ber-IP statis maupun dinamis.
  - **Penyusunan Dokumentasi Komprehensif Huawei OLT Reference (`documentations/huawei-olt-reference/`)**:
    - Mengompilasi 12 dokumen panduan dan dokumentasi teknis mendalam mengenai pengoperasian Huawei OLT (SmartAX MA5600 series & MA5800) sebagai standar acuan engineering tim dan AI Agent:
      1. Arsitektur sistem dan hardware OLT.
      2. Konfigurasi VLAN dan alur *Service-Port*.
      3. Manajemen profil GPON (DBA, Line Profile, Service Profile) dan QoS.
      4. Tata cara registrasi, pemantauan, dan manajemen ONT.
      5. Diagnostik jaringan optik, isolasi redaman, dan troubleshooting.
      6. Panduan integrasi dashboard & automasi OLT Gateway.
      7. Daftar lengkap perintah CLI di seluruh tingkatan mode (User, Privileged, Config, Diagnose, GPON Interface, dan GIU Uplink).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #526  | Microservice OLT Gateway, Integrasi CLI Huawei, & Manajemen ONT GPON | Lahirnya microservice baru `olt-gateway` yang aman dan terisolasi untuk mengelola perangkat fisik OLT (Huawei MA5600/MA5683T/MA5800), sinkronisasi board hardware, serta pemindaian ONT, autofind, dan pembacaan redaman live. |
| #524  | Perbaikan Notifikasi Telegram & Dokumentasi Kompilasi Huawei OLT | Memastikan informasi kredensial PPPoE dan IP di notifikasi Telegram instalasi selalu valid dan informatif (tanpa string `undefined`), serta menyediakan panduan teknis operasional OLT Huawei yang komprehensif. |

---

### Kemampuan Baru Pengguna/Admin

- **Manajemen Terpusat OLT**: Admin jaringan/NOC kini dapat mendaftarkan perangkat OLT di sistem dengan menentukan IP, protokol koneksi (SSH/Telnet), port, serta kredensial yang dienkripsi secara aman.
- **Monitoring Board & Hardware OLT Realtime**: Admin dapat melihat snapshot informasi hardware OLT (tipe sasis, versi software, uptime, dan daftar board yang terpasang di setiap slot) langsung dari dasbor web.
- **Visibilitas ONT GPON Otomatis**: Sistem secara otomatis mengenali port-port GPON aktif dari snapshot board, memindai seluruh ONT yang terhubung, dan mendeteksi ONT baru (*autofind*) yang siap diaktivasi.
- **Pengecekan Redaman Optik On-Demand**: Admin dapat memeriksa kualitas sinyal optik (redaman Rx/Tx Power dalam dBm) ONT pelanggan secara langsung melalui antarmuka sistem.
- **Notifikasi Telegram yang Akurat**: Teknisi lapangan menerima notifikasi Telegram pembukaan akun internet yang bersih, rapi, dan memuat detail username, password, serta alokasi IP (Statis maupun Dynamic Pool).

---

### Bug Fix / Solusi Masalah

- **Pencegahan Kerapuhan Koneksi OLT (SSH & Telnet PTY)**: Menghindari masalah prompt basi dan *hung session* pada OLT Huawei MA5683T dengan membedakan terminator baris (SSH hanya menggunakan `CR`, sementara Telnet menggunakan `CRLF`), serta menerapkan mesin penyaring opsi negosiasi Telnet otomatis.
- **Pencegahan Injeksi & Perintah Berbahaya di OLT**: Mengamankan eksekusi perintah CLI OLT melalui `commandGuard.js` yang menolak karakter kontrol, tanda tanya (`?`), dan membatasi perintah hanya pada operasi *read-only*.
- **Eliminasi String `undefined` pada Telegram Notifikasi**: Mengatasi hilangnya password dan IP address pada pesan balasan tiket aktivasi broadband di Telegram dengan memastikan `createNewAuthentication` mengembalikan field esensial secara utuh.

---

### Menu/Fitur Baru

- **Menu Manajemen OLT**: Tersedia di rute `/network/olt` pada dasbor web dengan hak akses privilege terstandarisasi.
- **Drawer Detail & Snapshot Hardware OLT**: Komponen visual untuk menginspeksi status sinkronisasi, log error, dan kartu board OLT.
- **Microservice OLT Gateway (`olt-gateway`)**: Layanan mandiri berbasis Node.js ESM di port 3060 dengan integrasi Swagger (`/docs.json`), antrean konkurensi OLT, dan health check terproteksi.
- **Kompilasi Dokumentasi OLT Huawei**: Tersedia di direktori `documentations/huawei-olt-reference/` mencakup 12 berkas markdown panduan lengkap.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - Fitur **Manajemen OLT & Sinkronisasi ONT** menghubungkan sistem Dekasimal dengan perangkat fisik Optical Line Terminal (OLT) di jaringan distribusi fiber optik. Melalui microservice `olt-gateway`, sistem mengantrekan sesi CLI ke perangkat OLT secara efisien dan aman.
  - Snapshot hardware mengidentifikasi board kontrol (SCUN/SCUK) dan board GPON (GPBD/GPFD). Sistem kemudian merencanakan pemindaian port PON secara bertahap untuk mencatat seluruh ONT yang online/offline, memantau level redaman serat optik, serta menangkap perangkat baru (*autofind*).

- **Langkah Penggunaan (Tutorial)**:
  1. **Membuka Menu OLT**: Masuk ke aplikasi admin, buka sidebar **Jaringan** lalu pilih **Manajemen OLT**.
  2. **Menambahkan Perangkat OLT**:
     - Klik tombol **Tambah OLT**.
     - Masukkan nama perangkat (misal: `OLT-Pusat-MA5683T`), alamat IP, vendor (`Huawei`), tipe koneksi (`SSH` atau `Telnet`), dan port.
     - Masukkan username dan password login, serta password enable (jika OLT memerlukan eskalasi privilege).
     - Klik **Simpan**.
  3. **Melakukan Sinkronisasi Awal**:
     - Pada baris OLT yang baru dibuat di tabel, klik tombol aksi **Sinkronkan**.
     - Sistem akan membuka sesi CLI singkat ke OLT, mengambil versi firmware dan mendeteksi slot board yang aktif.
     - Periksa tab **Detail / Snapshot Board** untuk melihat kartu GPON yang terdeteksi beserta status operasionalnya.
  4. **Melihat Status ONT & Redaman**:
     - Akses detail OLT untuk meninjau daftar port GPON dan inventaris ONT yang terhubung.
     - Untuk ONT pelanggan yang sedang mengalami gangguan, gunakan tombol **Cek Redaman** untuk mengambil data Rx Optical Power secara langsung dari modul optik OLT.

