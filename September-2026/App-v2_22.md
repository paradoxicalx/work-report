# 📝 Daily Work Report - Dedy S.N Putra (2026-09-22)

---

## 📅 Laporan Harian - 22 September 2026

---

## 🌿 Branch: `issue-175` — Penyelesaian Sistem Monitoring & Konfigurasi Jarak Jauh (Remote Configuration) ONT TR-069 (GenieACS), Provisi Otomatis Parameter Kredensial, Matriks Kompatibilitas Model, dan Deteksi Degradasi Sinyal

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: Implementasi Sistem Monitoring & Remote Configuration ONT TR-069 (GenieACS), Auto-Provisioning Parameter Kredensial, Matriks Kompatibilitas Model, dan Deteksi Degradasi Sinyal Optik
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-175`, telah diselesaikan dan di-push ke `origin/issue-175` melalui commit [`cfa339ef`](file:///home/dhedhy/Project/Dekasimal-V2) dengan status `resolve #175`, siap untuk ditinjau dan di-merge ke branch `master`)

---

### ⏳ Pekerjaan Belum Di-commit (Working Tree Changes)

- **Komponen yang Berubah**:
  - [`backend/src/models/acsDevice.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/acsDevice.model.js) — Menambahkan field `last_poll_at: { type: Date, default: null }` dan `last_poll_failure: { type: String, default: null }` pada schema `AcsDevice` untuk menyimpan riwayat stempel waktu dan alasan kegagalan panggilan balik (*Connection Request*) terakhir dari ACS ke perangkat ONT.
  - [`backend/src/services/acs.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acs.service.js):
    - Mendaftarkan field `last_poll_at` dan `last_poll_failure` ke dalam proyeksi seleksi `ACS_LIST_FIELDS`.
    - Menambahkan fungsi helper `hasFailedPoll(device)` untuk mendeteksi apakah panggilan terakhir ke perangkat gagal dan belum terbantahkan oleh laporan *Inform* yang lebih baru (`lastInform <= polledAt`).
    - Memperbarui fungsi dekorator `decorateDevice` untuk menyematkan penanda terhitung `poll_failed = hasFailedPoll(device)`.
    - Mengimplementasikan fungsi pembantu `recordPollResult(deviceId, failure)` pada fungsi `requestAcsDeviceRefresh` guna mencatat alasan kegagalan spesifik (`acs_unreachable`, `not_in_acs`, `credentials_rejected`, atau `device_unreachable`), atau menghapus penanda kegagalan (`null`) saat sesi CWMP berhasil.
  - [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx):
    - Memperbarui komponen tombol aksi `AcsRefreshCellButton` agar tetap memicu pemuatan ulang tabel (`reloadTable?.()`) meskipun permintaan refresh gagal, sehingga baris tabel langsung tersinkronisasi menampilkan status penanda kegagalan terkini tanpa bertentangan dengan pesan toast peringatan.
    - Memperbarui komponen sel status `AcsDeviceStatusCell` untuk menampilkan badge peringatan `pollFailed` ("Tidak menjawab") berwarna kuning (*warning*) dengan tooltip edukatif di samping status koneksi dasar, sehingga membedakan secara tegas antara perangkat yang benar-benar offline dengan perangkat yang online namun tidak dapat dihubungi balik (misal karena berada di balik CGNAT atau kredensial Connection Request tidak cocok).
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan kejelasan observabilitas tingkat tinggi bagi teknisi NOC terhadap ONT yang berada di balik jaringan NAT/CGNAT; ONT tetap dilaporkan online karena rutin mengirim *Inform*, namun diberi label peringatan visual "Tidak menjawab" apabila panggilan balik jarak jauh dari ACS tidak direspons perangkat.

---

### 📅 Rincian Commit

#### [`cfa339ef`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #175 - 22 September 2026, 22:36:45 WIB

- **Komponen yang Berubah**:
  - **Layanan Inti & Konfigurasi Jarak Jauh TR-069 (Backend Services & Models)**:
    - [`backend/src/services/acsConfig.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acsConfig.service.js) [NEW] — Modul layanan tunggal (*single point of truth*) untuk operasi tulis konfigurasi ke perangkat ONT via GenieACS NBI:
      - **Dynamic Candidate Path Resolution**: Menentukan path parameter TR-069 secara dinamis dari pohon parameter yang benar-benar ada dan berstatus *writable* pada perangkat fisik yang bersangkutan, alih-alih menebak dari tabel statis vendor. Mendukung variasi data model heterogen (VSOL, Huawei, ZTE, Fiberhome, dll.).
      - **Instance Prefix Tracking**: Menggunakan prefix jalur WAN (`wan_prefix`), LAN (`lan_prefix`), dan radio WiFi yang disimpan saat sesi *Inform* guna mencegah salah konfigurasi PPPoE ke interface manajemen TR-069.
      - **Sparse Update Protection**: Hanya menuliskan parameter yang secara eksplisit dikirim oleh pengguna pada payload pembaruan, melindungi kolom kata sandi PPPoE/WiFi/Admin dari risiko tertimpa nilai bertopeng atau string kosong.
      - **Fungsi Konfigurasi Komprehensif**: Mengelola konfigurasi LAN & DHCP (IP Gateway, Subnet Mask, Server DHCP on/off, IP Pool, Lease Time), WAN (Username & Password PPPoE, VLAN ID, MTU), WiFi Dual-Band 2.4GHz & 5GHz (SSID, Radio on/off, Hide SSID, Security Mode WPA/WPA2-PSK, PreSharedKey), Kata Sandi Administrator Web GUI ONT, dan perintah Reboot jarak jauh.
      - `findAcsConfigCoverage()`: Menghitung persentase kesiapan dan matriks kompatibilitas parameter TR-069 per model perangkat yang terdaftar di jaringan.
    - [`backend/src/services/acsProvision.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acsProvision.service.js) [NEW] — Layanan auto-provisioning otomatis ONT saat sesi *Inform*:
      - Menyelesaikan dilema *chicken-and-egg* pada ONT baru yang datang dengan konfigurasi pabrik (interval inform 12 jam dan kata sandi Connection Request acak yang tidak dapat dibaca kembali).
      - Menyiapkan daftar nilai parameter yang wajib diatur (interval inform berkala dan kredensial Connection Request terpadu) dan menerapkannya langsung menumpang pada sesi *Inform* yang dimulai oleh ONT itu sendiri tanpa membutuhkan Connection Request awal.
      - Menghasilkan sidik jari (*provision signature*) untuk mendeteksi perubahan konfigurasi di panel admin dan memastikan operasi bersifat idempoten tanpa penulisan berulang yang membebani perangkat.
    - [`backend/src/services/acs.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acs.service.js) — Memperluas layanan ACS dengan integrasi pembacaan detail konfigurasi perangkat, filtrasi parameter sensitif berbasis privilege `acs.readSensitive`, mekanisme Connection Request refresh instan, serta pembersihan penanda konflik asosiasi perangkat pelanggan.
    - [`backend/src/services/acsSignal.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acsSignal.service.js) — Mengintegrasikan evaluasi baseline sinyal optik harian untuk mendeteksi penurunan daya terima optik (RX Power drop > 3 dBm) dan menyiarkan peringatan dini ke grup Telegram NOC.
    - [`backend/src/services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js) — Mendaftarkan kunci pengaturan sistem baru: `acs_provision_enabled`, `acs_auth_username`, `acs_auth_password`, `acs_inform_auth_enabled`, `acs_inform_interval`, dan `acs_offline_threshold_seconds`. Menambahkan daftar pengecualian `SYSTEM_SETTINGS_NEVER_RETURNED` untuk mencegah kebocoran kredensial OAuth pihak ketiga.
    - [`backend/src/models/acsDevice.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/acsDevice.model.js) — Memperkaya schema dokumen perangkat ACS dengan metadata provisi (`provisioned_at`, `provision_parameters`, `provision_inform_before`), daftar koneksi WAN (`wan_connections`), prefix interface LAN, dan cache cabang konfigurasi yang telah dikenali.
  - **API Controllers, Routes, & Security Privileges**:
    - [`backend/src/controllers/acs.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/acs.controller.js) — Menambahkan handler endpoint API:
      - `GET /api/v1/acs/config/:serial_number` (`acsDeviceConfig`): Mengambil konfigurasi perangkat ONT dengan opsi force re-poll langsung ke perangkat.
      - `POST /api/v1/acs/config/:serial_number` (`applyAcsDeviceConfigHandler`): Menerapkan perubahan parameter konfigurasi ke ONT secara selektif via GenieACS NBI.
      - `GET /api/v1/acs/coverage` (`acsConfigCoverage`): Mengambil data matriks cakupan fitur per model hardware ONT di jaringan.
      - `POST /api/v1/acs/refresh/:serial_number` (`refreshAcsDevice`): Mengirimkan Connection Request untuk meminta ONT melaporkan status terkini seketika.
      - `DELETE /api/v1/acs/device/:serial_number` (`deleteAcsDevice`): Menghapus perangkat yang sudah berstatus mati/offline dari database lokal dan GenieACS.
    - [`backend/src/routes/acs.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/acs.route.js) — Mendaftarkan rute REST API baru lengkap dengan middleware autentikasi JWT, proteksi privilege hak akses, dan dokumentasi OpenAPI/Swagger.
    - [`backend/src/controllers/settings.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/settings.controller.js) — Merefaktor handler `readSettingsSensitive` agar membangun objek response sensitif secara dinamis dari registri `SYSTEM_SETTINGS_SENSITIVE` dengan mengecualikan `SYSTEM_SETTINGS_NEVER_RETURNED`, mencegah kekosongan data kata sandi TR-069 saat form dimuat.
    - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) — Mendaftarkan hak akses baru `acs.delete` (`ACS_DELETE`).
    - [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) & [`id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) — Menambahkan terjemahan pesan error, status operasional, dan notifikasi keberhasilan manajemen perangkat ACS.
  - **GenieACS Provision Engine & Test Sandbox (Modul `/acs`)**:
    - [`acs/config/provisions/inform.js`](file:///home/dhedhy/Project/Dekasimal-V2/acs/config/provisions/inform.js) — Skrip provisi utama GenieACS:
      - Ekstraksi mendalam parameter TR-069 saat ONT mengirim event *Inform* (WAN IP, PPPoE username, VLAN ID, MAC address, Redaman Optik RX/TX Power, Suhu PON, Status WiFi 2.4G & 5G, daftar perangkat klien LAN yang terhubung).
      - Menjalankan penulisan parameter auto-provisioning secara langsung sesuai instruksi yang dikembalikan oleh backend dalam respons webhook Inform.
    - [`acs/ext/informWebhook.cjs`](file:///home/dhedhy/Project/Dekasimal-V2/acs/ext/informWebhook.cjs) — Ekstensi perantara GenieACS untuk mengirimkan payload Inform ke backend via HTTP POST internal dan meneruskan instruksi balik ke skrip provisi.
    - [`acs/test/inform.provision.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/acs/test/inform.provision.test.js) [NEW] & [`acs/test/sandbox.js`](file:///home/dhedhy/Project/Dekasimal-V2/acs/test/sandbox.js) [NEW] — Suite unit test untuk skrip provisi GenieACS yang berjalan di atas sandbox mock terisolasi (21 test suite lulus tanpa kegagalan).
    - [`acs/test/fixtures/`](file:///home/dhedhy/Project/Dekasimal-V2/acs/test/fixtures/) (`vsol-v2802dac-multi-ip.json` [NEW], `vsol-xpon-1ge-wifi-dual-pppoe.json` [NEW]) — Berkas data uji nyata yang memuat dump pohon parameter TR-069 dari perangkat ONT VSOL dual-band dan single-band.
  - **Antarmuka Pengguna & Manajemen Perangkat (Frontend)**:
    - [`frontend/src/app/pages/network/acsDevices/components/AcsDeviceConfigModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/components/AcsDeviceConfigModal.jsx) [NEW] — Modal konfigurasi perangkat komprehensif berbasis tab interaktif:
      - **Tab LAN**: Pengaturan IP Gateway ONT, Subnet Mask, Switch Server DHCP, Rentang IP Pool (Min/Max), dan Durasi Sewa DHCP.
      - **Tab WAN**: Pengaturan Username & Password PPPoE (dengan tombol intip sandi), VLAN ID, dan MTU jaringan.
      - **Tab WiFi 2.4 GHz & WiFi 5 GHz**: Pengaturan SSID jaringan, saklar aktif/nonaktif radio, sembunyikan SSID (hidden), mode keamanan (None, WPA/WPA2-PSK), dan kata sandi WiFi PreSharedKey.
      - **Tab Admin**: Fasilitas penggantian kata sandi administrator Web GUI modem dari jarak jauh.
      - **Opsi Reboot Otomatis**: Kemampuan untuk me-reboot ONT secara otomatis setelah parameter berhasil diterapkan agar konfigurasi segera aktif.
    - [`frontend/src/app/pages/network/acsDevices/components/AcsModelCoverageModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/components/AcsModelCoverageModal.jsx) [NEW] — Modal matriks cakupan fitur per model perangkat yang menampilkan persentase kompatibilitas TR-069 beserta rincian parameter yang tidak didukung untuk tiap varian modem yang terpasang di jaringan.
    - [`frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx) — Memperkaya drawer rincian ONT dengan tombol aksi pintas Konfigurasi Jarak Jauh (`FiSettings`), tombol Refresh Langsung via Connection Request (`FiRefreshCw`), indikator visual deteksi perangkat macet pasca-provisi (`provision_stalled`), serta perbaikan pesan fallback saat serial number kosong.
    - [`frontend/src/app/pages/network/acsDevices/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/index.jsx) & [`schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/schema/columns.jsx) — Penambahan tombol pembuka modal cakupan model (*Model Coverage*) pada header tabel, penambahan tombol aksi konfigurasi pada baris tabel, dan penyempurnaan kolom TanStack Table.
    - [`frontend/src/app/pages/settings/sections/System.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/System.jsx) & [`schema/systemSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/schema/systemSchema.js) — Menambahkan sub-bagian pengaturan **ACS (TR-069)** pada halaman Pengaturan Sistem untuk mengelola saklar aktivasi provisi otomatis, kredensial autentikasi inform, interval inform berkala, dan ambang batas waktu deteksi offline.
    - [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx) & [`status.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/status.js) — Penambahan sel badge status perangkat dan mutu daya optik RX/TX.
    - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — Penambahan kamus lokalisasi komprehensif untuk seluruh antarmuka konfigurasi ACS, modal cakupan model, dan opsi pengaturan sistem.
  - **Suite Pengujian Integrasi & Eliminasi Flaky Test**:
    - [`backend/test/integration/acsConfig.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acsConfig.service.test.js) [NEW] — 32 skenario pengujian integrasi yang memvalidasi resolusi kandidat path dinamis, pembacaan konfigurasi ONT, penulisan aman parameter parsial, pencegahan penimpaan password bertopeng, dan perhitungan matriks cakupan fitur.
    - [`backend/test/integration/acsProvision.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acsProvision.service.test.js) [NEW] — 20 skenario pengujian integrasi yang memverifikasi normalisasi pengaturan provisi, pembuatan sidik jari (*signature*), dan penyusunan instruksi provisi otomatis.
    - [`backend/test/integration/acs.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acs.service.test.js) — Diperluas hingga 71 pengujian lulus, memvalidasi keamanan kata sandi WiFi berdasar hak akses, refresh Connection Request, penghapusan perangkat mati, dan resolusi konflik relasi pelanggan.
    - [`backend/test/integration/acs.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acs.controller.test.js) — Diperluas hingga 19 pengujian lulus untuk memvalidasi endpoint-endpoint konfigurasi, refresh, dan coverage.
    - [`backend/test/integration/syslogAiAnalysis.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/syslogAiAnalysis.service.test.js) — Memperbaiki *flaky test* pada test suite `runSyslogHourlyDigest` dengan helper deterministik `makeHourlyWindow()`, memastikan jendela waktu pengujian konsisten dan mengeliminasi kegagalan balapan milidetik (49/49 pengujian lulus).
  - **Dokumentasi Arsitektur Monorepo**:
    - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md) — Memperbarui dokumen panduan arsitektur monorepo untuk mencakup microservice `ACS Server` (GenieACS), alokasi port default, arsitektur integrasi webhook satu arah Inform, strategi mitigasi perbedaan data model TR-069 lintas vendor, serta standar observabilitas logging.
- **Deskripsi Perubahan & Fungsi**:
  - Menyelesaikan implementasi penuh issue #175 dari pemantauan pasif menjadi sistem manajemen perangkat ONT dua arah yang aman, andal, dan siap produksi.
  - Memungkinkan admin dan teknisi ISP untuk melakukan konfigurasi ulang WiFi pelanggan (SSID & Password), parameter WAN/PPPoE, dan reboot modem langsung dari dashboard web tanpa perlu login manual ke Web GUI ONT pelanggan di lokasi.
  - Menyediakan perlindungan multi-lapis terhadap kegagalan penulisan TR-069: penyelesaian path dinamis yang mengenali variasi firmware vendor, perlindungan pembaruan parsial agar tidak menghapus kata sandi, dan auto-provisioning yang menjaga perangkat tetap dapat dihubungi dari jarak jauh.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #175  | Penyelesaian Sistem Monitoring & Konfigurasi Jarak Jauh (Remote Configuration) ONT TR-069 (GenieACS), Auto-Provisioning Parameter Kredensial, Matriks Kompatibilitas Model, dan Deteksi Degradasi Sinyal | Mengubah sistem monitoring pasif menjadi platform manajemen CPE/ONT dua arah terpadu; memungkinkan konfigurasi jarak jauh (LAN, WAN, WiFi, Sandi Admin, Reboot) via TR-069, auto-provisioning interval dan kredensial perangkat saat Inform, pemantauan matriks cakupan fitur per model modem, dan kejelasan status perangkat yang tidak menjawab panggilan Connection Request. |

### Kemampuan Baru Pengguna/Admin

- **Konfigurasi Jarak Jauh Tanpa Kunjungan Lapangan (*Remote Configuration*)**:
  Admin dan tim *Helpdesk* kini dapat mengubah SSID WiFi, kata sandi WiFi pelanggan (2.4 GHz dan 5 GHz), akun PPPoE, VLAN, serta kata sandi Web GUI modem secara instan langsung dari panel DEKASIMAL V2 tanpa perlu meminta pelanggan membuka port atau mengirim teknisi ke rumah pelanggan.
- **Reboot Perangkat Terjadwal & On-Demand**:
  Admin dapat memicu *remote reboot* pada ONT pelanggan saat koneksi modem mengalami perlambatan memori atau setelah konfigurasi baru diterapkan.
- **Pemaksaan Pelaporan Instan (*Connection Request Refresh*)**:
  Tersedia tombol aksi refresh langsung yang mengirimkan panggilan balik TR-069 Connection Request ke perangkat, memaksa modem memperbarui parameter koneksi dan redaman sinyal optik saat itu juga.
- **Pemeriksaan Kesiapan Model Modem (*Model Coverage Matrix*)**:
  Melalui tombol *Model Coverage*, admin jaringan dapat melihat daftar seluruh tipe ONT yang beroperasi di lapangan, persentase parameter TR-069 yang didukung oleh firmware masing-masing model, serta rincian parameter yang belum didukung untuk panduan standarisasi perangkat ISP.
- **Diferensiasi Status Offline vs Tidak Menjawab (*Poll Failed*)**:
  Antarmuka tabel kini membedakan antara perangkat yang mati (*Offline*) dengan perangkat yang tetap hidup namun tidak menjawab panggilan balik (*Tidak menjawab / Poll Failed*, misalnya karena berada di balik CGNAT), mencegah salah diagnosa gangguan jaringan.
- **Pengaturan Global ACS & Auto-Provisioning**:
  Admin utama dapat mengonfigurasi parameter provisi otomatis, kredensial panggilan balik TR-069, interval inform bawaan armada, dan ambang batas waktu deteksi offline langsung melalui menu Pengaturan Sistem.

### Bug Fix / Solusi Masalah

- **Pencegahan Penghapusan Kata Sandi Tersimpan (*Sparse Update Protection*)**:
  Form konfigurasi TR-069 dirancang dengan perlindungan pembaruan parsial ketat; kata sandi PPPoE dan WiFi yang tidak disentuh admin tidak akan tertimpa oleh string kosong atau nilai bertopeng yang dikembalikan oleh firmware modem.
- **Eliminasi Dilema Ayam-Telur Kredensial ONT Baru (*Chicken-and-Egg Resolved*)**:
  ONT baru dari pabrik dengan interval lapor lama (12 jam) dan kata sandi Connection Request acak kini secara otomatis dipasangi kredensial terpadu dan interval 300 detik pada sesi *Inform* pertama yang diinisiasi oleh ONT itu sendiri.
- **Eliminasi Inkonsistensi Visual Tabel Pasca Gagal Refresh**:
  Tabel perangkat ACS kini dimuat ulang secara otomatis saat permintaan refresh gagal, memastikan status penanda kegagalan terbaru segera terlihat oleh pengguna.
- **Penyelesaian Uji Flaky `runSyslogHourlyDigest`**:
  Memperbaiki kelemahan uji integrasi analisis AI syslog yang sesekali gagal akibat balapan milidetik pada database MongoDB in-memory, menjamin stabilitas pipeline CI/CD pengujian otomatis monorepo.

### Menu/Fitur Baru

- **Modal Konfigurasi Perangkat ONT (`AcsDeviceConfigModal`)**:
  Modal dialog berlapis tab di halaman `/networks/acs-devices` yang mendukung manipulasi parameter LAN, WAN/PPPoE, WiFi Dual-Band, Sandi Admin, dan opsi reboot otomatis.
- **Modal Cakupan Model Perangkat (`AcsModelCoverageModal`)**:
  Modal analitik yang menampilkan visualisasi persentase kesiapan kontrol TR-069 untuk setiap vendor dan tipe modem di jaringan pelanggan.
- **Tab Pengaturan Sistem Baru: ACS (TR-069)**:
  Sub-bagian baru di menu Pengaturan Sistem (`/settings` → tab `ACS`) untuk mengontrol parameter operasional GenieACS dan auto-provisioning.
- **Aksi Cepat pada Drawer & Tabel**:
  Tombol pintas konfigurasi dan refresh langsung pada sel baris tabel dan header drawer rincian ONT.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  Sistem **Remote Configuration & Auto-Provisioning TR-069** bekerja melalui jembatan komunikasi antara Backend DEKASIMAL V2 dan GenieACS NBI (Port 7557).
  Ketika admin mengajukan perubahan konfigurasi melalui antarmuka pengguna:
  1. Backend menelusuri pohon data model TR-069 spesifik milik ONT tersebut untuk mencocokkan nama kandidat path parameter yang benar-benar ada dan dapat ditulis (*writable*).
  2. Perubahan hanya disusun untuk field-field yang diisi, kemudian diteruskan ke GenieACS sebagai kumpulan task `setParameterValues`.
  3. GenieACS mengirimkan Connection Request ke ONT untuk membuka sesi CWMP, mengirimkan parameter baru, dan secara opsional memicu perintah `reboot` jika diminta.
  4. Bila ONT baru pertama kali melapor, skrip provisi `inform.js` secara otomatis menanamkan interval inform standar (300 detik) dan kredensial autentikasi Connection Request terpusat, memastikan seluruh armada modem selalu siap dikelola dari jarak jauh kapan saja.

- **Langkah Penggunaan (Tutorial)**:

  1. **Mengubah Kata Sandi atau SSID WiFi Pelanggan dari Jarak Jauh**:
     - Buka menu navigasi utama, pilih **Jaringan** (*Networks*) → klik **Perangkat ACS** (`/networks/acs-devices`).
     - Cari nomor seri ONT atau nama pelanggan yang bersangkutan melalui kolom pencarian.
     - Klik tombol ikon gerigi **Konfigurasi** pada kolom aksi baris perangkat (atau melalui tombol ikon gerigi di header drawer detail).
     - Pilih tab **WiFi 2.4 GHz** atau **WiFi 5 GHz**.
     - Ubah nama SSID pada kolom yang tersedia, atau masukkan kata sandi baru pada kolom **PreSharedKey** (gunakan ikon mata untuk melihat sandi yang diketik).
     - Centang opsi **Restart perangkat setelah konfigurasi disimpan** bila ingin modem langsung melakukan reboot untuk menerapkan perubahan.
     - Klik **Simpan Konfigurasi**. Notifikasi konfirmasi akan muncul dan parameter akan didorong langsung ke modem.

  2. **Melakukan Pemaksaan Pembaruan Data (*Connection Request Refresh*)**:
     - Pada baris perangkat yang diinginkan di tabel `/networks/acs-devices`, klik tombol ikon putar **Refresh** (atau klik tombol refresh di pojok kanan atas drawer detail).
     - Sistem akan mengirimkan Connection Request ke modem. Ikon akan berputar selama sesi CWMP berlangsung.
     - Jika berhasil, data IP WAN, status koneksi, dan redaman daya optik RX/TX terbaru akan langsung diperbarui di layar.
     - Jika perangkat berada di balik CGNAT atau menolak panggilan, badge kuning bertuliskan **"Tidak menjawab"** akan muncul di samping status online perangkat dengan penjelasan lengkap saat kursor diarahkan ke badge tersebut.

  3. **Memeriksa Cakupan Kompatibilitas Model Modem (*Model Coverage Matrix*)**:
     - Pada halaman utama **Perangkat ACS** (`/networks/acs-devices`), klik tombol **Cakupan Model** (*Model Coverage*) di sebelah kanan atas dekat kolom pencarian.
     - Tinjau daftar model ONT yang terdeteksi di jaringan beserta bilah persentase dukungan fiturnya.
     - Klik baris model tertentu untuk melihat daftar parameter spesifik yang belum didukung oleh firmware model tersebut (misal model lama yang tidak memiliki radio 5 GHz atau tidak mengekspos konfigurasi DHCP).

  4. **Mengonfigurasi Provisi Otomatis di Pengaturan Sistem**:
     - Buka menu **Pengaturan** (*Settings*) → pilih tab **Sistem** (*System*) → klik tab sub-menu **ACS**.
     - Aktifkan saklar **Provisi Otomatis ACS**.
     - Masukkan Username dan Password autentikasi Connection Request yang akan dipasang ke seluruh ONT pelanggan.
     - Tentukan interval waktu pengiriman Inform berkala (standar: 300 detik).
     - Atur ambang batas toleransi waktu deteksi offline perangkat.
     - Klik tombol **Simpan Pengaturan** di bagian bawah halaman.
