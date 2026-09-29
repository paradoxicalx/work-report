# 📝 Daily Work Report - Dedy S.N Putra (2026-09-29)

---

## 📅 Laporan Harian - 29 September 2026

> Laporan ini mencakup seluruh sesi pekerjaan per 29 September 2026 hingga finalisasi commit dan *squashing* pada dini hari 30 September 2026 (00:09 WIB).

---

## 🌿 Branch: `issue-175` — Implementasi Sistem Monitoring & Remote Configuration ONT TR-069 (ACS), Auto-Provisioning Parameter Kredensial, Matriks Kompatibilitas Model, dan Deteksi Degradasi Sinyal Optik

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: Implementasi Sistem Monitoring & Remote Configuration ONT TR-069 (ACS), Auto-Provisioning Parameter Kredensial, Matriks Kompatibilitas Model, dan Deteksi Degradasi Sinyal Optik
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-175`, telah diselesaikan dan di-push ke remote `origin/issue-175` melalui commit [`818d722c`](file:///home/dhedhy/Project/Dekasimal-V2) dengan pesan `resolve #175`)

### 📅 Rincian Commit

#### [[`818d722c`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #175 - 30 September 2026, 00:09:54 WIB

*(Commit gabungan / rebase-squash yang menyatukan seluruh tahapan milestone penyelesaian issue #175: commit [`e25ef038`](file:///home/dhedhy/Project/Dekasimal-V2) [28 Sep 23:40 WIB], commit [`408bd4dd`](file:///home/dhedhy/Project/Dekasimal-V2) [29 Sep 15:57 WIB], dan commit [`bdb11309`](file:///home/dhedhy/Project/Dekasimal-V2) [30 Sep 00:09 WIB]).*

- **Komponen yang Berubah**:
  - **ACS CWMP Provisioning & Heuristik Sesi Inform (`acs/`)**:
    - [`acs/config/provisions/inform.js`](file:///home/dhedhy/Project/Dekasimal-V2/acs/config/provisions/inform.js):
      - *Pembacaan Ulang Parameter Segar pada Sesi Connection Request*: Mengidentifikasi penanda event `Events.6_CONNECTION_REQUEST` pada denyut Inform. Menggeser titik acuan `refreshAt` ke timestamp event `connReqAt` (bukan `Date.now()`, guna menghindari perputaran *infinite script loop* saat commit berulang di sandbox ACS) sehingga tombol "Muat Ulang" atau pengiriman konfigurasi admin selalu membaca nilai terkini dari ONT tanpa tertahan batas cache 5 menit (`REFRESH_MS`).
      - *Perbaikan Heuristik Seleksi Jalur Layanan WAN*: Menyaring `declaredInternet` dari seluruh koneksi WAN yang terdeteksi (dengan atau tanpa alamat IP). Memperbaiki bug kritis di mana ONT dengan koneksi Internet WAN yang sedang terputus (alamat IP kosong) tersaring dari `addressed`, sehingga script keliru memilih koneksi Manajemen TR-069 sebagai jalur layanan pelanggan dan membocorkan IP serta kredensial manajemen ke kartu pelanggan.
    - [`acs/.env.example`](file:///home/dhedhy/Project/Dekasimal-V2/acs/.env.example) — Menambahkan dokumentasi supresi log akses CWMP (`ACS_CWMP_ACCESS_LOG_FILE=/dev/null`) untuk produksi armada besar guna menghentikan pembanjiran log HTTP access request tanpa menonaktifkan supervisor logging Winston.
    - [`acs/test/inform.provision.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/acs/test/inform.provision.test.js) — Menambahkan 11 test otomatis baru untuk menguji pembacaan ulang nilai saat sesi Connection Request dan memastikan koneksi manajemen tidak pernah terpilih sebagai koneksi internet pelanggan saat status WAN terputus.

  - **Backend Core & Logika Bisnis ACS (`backend/src/`)**:
    - [`backend/src/models/acsDevice.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/acsDevice.model.js):
      - Menambahkan field `auto_provision_enabled: { type: Boolean, default: false }` sebagai gerbang ketiga provisioning otomatis per unit ONT untuk menahan penulisan interval inform berkala tanpa persetujuan admin.
      - Menambahkan field `link_conflict_since: { type: Date, default: null }` untuk mencatat awal mula terjadinya tabrakan username PPPoE antar-perangkat dan mendasari logika pergantian tautan otomatis.
    - [`backend/src/services/acsProvision.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acsProvision.service.js):
      - *Hashing Kriptografis Sidik Jari Kredensial*: Memperkuat fungsi `buildProvisionSignature(settings)` dengan *one-way cryptographic hash* SHA-256 (`crypto.createHash('sha256')`) menggantikan format teks terbuka, mengeliminasi risiko kebocoran kata sandi ACS ke antarmuka web.
      - *Pemisahan Keterjangkauan vs Konfigurasi*: Parameter keterjangkauan (kredensial Connection Request) SELALU ditulis agar ACS dapat menelepon balik ONT dan mencegah status `credentials_rejected`. Sementara parameter konfigurasi (`PeriodicInformInterval` & `PeriodicInformEnable`) tunduk pada saklar per perangkat.
      - *Pengecualian Provisi Pertama (`firstProvision`)*: Menuliskan interval inform standar satu kali saja pada perangkat yang baru pertama kali melapor (`provisioned_at` masih kosong) agar interval bawaan pabrik yang panjang (hingga 12 jam) dinormalkan menjadi 300 detik.
    - [`backend/src/services/acs.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acs.service.js):
      - *Penggantian Tautan Otomatis Tabrakan Bertahan (`LINK_TAKEOVER_MS = 48 jam`)*: Mengganti tautan manual secara otomatis ke perangkat baru jika tabrakan username PPPoE bertahan >= 48 jam dan ONT lama pemegang tautan tidak pernah melapor (`last_inform`) selama >= 48 jam (kasus penggantian modem di lapangan tanpa update sistem oleh teknisi), lengkap dengan jejak audit Winston.
      - *Pencarian Nomor Seri Efisien Berbasis Indeks (`findDeviceBySerial`)*: Mengganti kueri regex lambat dengan pencocokan exact match yang memanfaatkan indeks unik MongoDB, dan hanya menggunakan regex sebagai cadangan jika pencocokan eksak tidak ditemukan.
      - *Pemangkasan Data Sensitif & Internal (`tanpaBawaanInternal()` & `delete device.provision_signature`)*: Memastikan `provision_signature` dan field internal yang tidak digunakan frontend disaring pada corong tunggal `decorateDevice`, bahkan untuk admin pemegang izin `acs.readSensitive`.
      - *Pembagian Batch Sinkronisasi Armada (`syncAcsStateFromGenie`)*: Membagi eksekusi `bulkWrite` menjadi gelombang 500 dokumen (`ACS_SYNC_BATCH_SIZE`) dan menyematkan proyeksi `device,channel,code,message,timestamp` pada penarikan fault dari ACS NBI untuk mencegah lonjakan memori.
      - *Saklar Auto-Provisioning Per Perangkat (`setAcsDeviceAutoProvision`)*: Mengubah status saklar tanpa menghapus atau merusak nilai `provision_signature` yang tersimpan.
      - *Penyaringan Datatable Tanpa Migrasi Database*: Pada `translateComputedFilters`, penyaringan `auto_provision_enabled: false` menggunakan operator `{ $ne: true }` sehingga baris lama yang belum memiliki field tersebut tetap tersaring dengan tepat.
      - *Pembedaan Alasan Kegagalan Panggilan Balik*: Membedakan respons kegagalan antara `credentials_rejected` (kredensial ditolak saat provisioning aktif) dengan `credentials_rejected_provision_off` (kredensial ditolak saat provisioning global atau kredensial ACS belum diaktifkan di Pengaturan).
    - [`backend/src/services/acsConfig.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acsConfig.service.js):
      - *Dukungan Saklar Tagging VLAN (`vlan_enable`)*: Menambahkan parameter `vlan_enable` bertipe `intbool` (`X_CT-COM_VLANMode`) yang diserialisasi sebagai `xsd:unsignedInt` bernilai `0` atau `1` agar firmware ONT tidak mengabaikan konfigurasi VLAN ID.
      - *Penulisan Berurutan (`vlan_enable` sebelum `vlan`)*: Memastikan parameter saklar VLAN diproses mendahului VLAN ID dalam siklus `SetParameterValues`.
      - *Standarisasi Nama Layanan PPPoE TR-098*: Menjadikan `PPPoEServiceName` sebagai kandidat utama sebelum fallback ke `ServiceName`.
    - [`backend/src/controllers/acs.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/acs.controller.js) & [`backend/src/routes/acs.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/acs.route.js):
      - Menambahkan controller `setAcsDeviceAutoProvisionHandler` dan rute `PATCH /api/v1/acs/auto-provision/:serial_number` yang dilindungi hak akses `acs.update` dan validasi tipe boolean ketat.
      - Memperbarui penanganan error pada `refreshAcsDevice` untuk pesan `credentials_rejected_provision_off`.
    - [`backend/src/controllers/internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internal.controller.js) — Menstandarisasi pesan error `receiveAcsInform` menggunakan i18n (`req.t('acs.serialNumberRequired')`).
    - [`backend/src/middlewares/logger.middleware.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/middlewares/logger.middleware.js):
      - Menambahkan konstanta `ALWAYS_IGNORED_SUCCESS_PREFIXES = ['/api/v1/internal/acs/inform']` untuk membungkam pencatatan request sukses dari webhook Inform yang berfrekuensi sangat tinggi (~570 ribu panggilan/hari), guna mencegah ledakan koleksi `api_logs` MongoDB dan Logtail, dengan tetap mencatat panggilan berstatus gagal (HTTP >= 400).

  - **Antarmuka Pengguna & Diagnostik Frontend (`frontend/src/`)**:
    - [`frontend/src/components/shared/acs/OntDeviceCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/acs/OntDeviceCard.jsx):
      - Menambahkan saklar `StyledSwitch` untuk konfigurasi otomatis ONT langsung pada kartu informasi perangkat di halaman detail layanan broadband pelanggan.
      - Menampilkan peringatan tabrakan tautan modem (`link_conflict`) dengan daftar ONT penantang, tombol aksi "Tautkan perangkat ini" (`handleLinkConflict`), dialog konfirmasi `ConfirmModal`, validasi hak akses `canUpdate`, serta *cooldown timer* 15 detik untuk meredam klik ganda.
    - [`frontend/src/app/pages/network/acsDevices/components/AcsDeviceConfigModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/components/AcsDeviceConfigModal.jsx):
      - Menambahkan saklar `vlan_enable` pada form pembuatan koneksi WAN baru dan penyuntingan koneksi yang ada.
      - Mengunci kolom isian VLAN ID jika saklar VLAN Mode dalam kondisi nonaktif (`disabled={name === 'vlan' && !vlanTagged}`), disertai teks petunjuk (*hint*).
    - [`frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx):
      - Menambahkan seksi kartu baru "Seluruh Koneksi WAN" (`sectionWanAll`) berikon `GlobeAltIcon` untuk menampilkan semua koneksi WAN yang dilaporkan ONT (PPPoE, IP DHCP/Static, TR-069 Management, VLAN, status, dan IP), bukan hanya koneksi layanan utama.
    - [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx) & [`frontend/src/components/shared/table/status.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/status.js):
      - Membuat komponen sel tabel baru [`AcsAutoProvisionSwitchCell`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx) yang memungkinkan admin mengubah status auto-provisioning langsung dari tabel perangkat ACS.
      - Memisahkan tampilan visual badge pada `AcsDeviceStatusCell` antara kondisi "Kredensial ditolak" (`credentialsRejectedBadge`) dan "Tidak menjawab" (`pollFailed`).
      - Menambahkan opsi filter datatable `acsAutoProvisionOptions` (Aktif / Nonaktif).
    - [`frontend/src/app/pages/network/acsDevices/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/schema/columns.jsx) — Mendaftarkan kolom `auto_provision_enabled` dengan sel saklar dan opsi filter select.

  - **Pengujian Integrasi & Verifikasi Menyeluruh (`test/`)**:
    - [`backend/test/integration/internal.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/internal.controller.test.js) [NEW] — Menambahkan pengujian integrasi untuk penolakan payload logger internal dan penerimaan event syslog dengan pesan ber-i18n.
    - [`backend/test/integration/acs.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acs.service.test.js) — Memperluas pengujian hingga 159 test kasus, mencakup pencarian serial berbasis indeks, proteksi data sensitif sidik jari, auto-takeover tabrakan tautan 48 jam, pemotongan bulkWrite, dan filter tabel.
    - [`backend/test/integration/acsConfig.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acsConfig.service.test.js) — Menambahkan 72 test kasus untuk validasi tipe `intbool` pada `vlan_enable`, urutan penulisan SetParameterValues, dan pengenalan `PPPoEServiceName`.
    - [`backend/test/integration/acsProvision.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acsProvision.service.test.js) — Menambahkan 36 test kasus untuk pengujian integritas hash SHA-256, pemisahan keterjangkauan vs konfigurasi, dan penulisan interval pada firstProvision.
    - [`backend/test/integration/acs.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acs.controller.test.js) — Menguji endpoint `PATCH /acs/auto-provision/:serial_number` dan penolakan payload non-boolean.
    - [`backend/test/unit/loggerSanitizer.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/loggerSanitizer.test.js) — Memperluas pengujian sanitasi redaksi kredensial logger.
    - Pembersihan berkas uji coba temporer: [`backend/tmp-uji.mjs`](file:///home/dhedhy/Project/Dekasimal-V2/backend/tmp-uji.mjs) [DELETE], [`backend/tmp-uji2.mjs`](file:///home/dhedhy/Project/Dekasimal-V2/backend/tmp-uji2.mjs) [DELETE], dan [`backend/tmp-uji4.mjs`](file:///home/dhedhy/Project/Dekasimal-V2/backend/tmp-uji4.mjs) [DELETE].

  - **Dokumentasi & Internasionalisasi**:
    - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md) — Memperbarui aturan logging supervisor ACS dan proses ekstensi `ext/*.cjs`.
    - [`docs/superpowers/specs/2026-09-20-acs-monitoring-v1-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-09-20-acs-monitoring-v1-design.md) — Menyelaraskan referensi variabel lingkungan ACS.
    - [`backend/src/locales/`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/) & [`frontend/src/i18n/locales/`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/) (`en` & `id`) — Mendaftarkan seluruh kunci terjemahan: `autoProvisionEnabled`, `autoProvisionDisabled`, `autoConfig`, `autoConfigOn`, `autoConfigOff`, `autoProvisionSaved`, `autoProvisionFailed`, `autoProvisionHint`, `sectionWanAll`, `wanConnectionsNotReported`, `wanService`, `cfg_wan_vlan_enable`, `cfgVlanNeedsEnable`, `credentialsRejectedBadge`, `credentialsRejectedHint`, `credentialsRejectedProvisionOff`, `linkThisDevice`, `linkThisDeviceCooldown`, `linkConflictConfirmTitle`, dan `linkConflictConfirmDesc`.

- **Deskripsi Perubahan & Fungsi**:
  - Menyempurnakan stabilitas, keamanan, dan fungsionalitas manajemen modem jarak jauh TR-069 secara menyeluruh.
  - Memberikan perlindungan penuh terhadap firmware modem pelanggan dari risiko penulisan massal parameter konfigurasi yang tidak disengaja melalui saklar granular per perangkat, sembari tetap menjamin keterjangkauan panggilan balik ACS ke perangkat di balik CGNAT.
  - Mengeliminasi potensi kebocoran kredensial rahasia sistem ACS ke peramban web dengan mekanisme *hashing* kriptografis SHA-256 dan pemangkasan field internal.
  - Menghadirkan mekanisme pemulihan otomatis (*auto-takeover*) cerdas pada tabrakan tautan modem pelanggan yang ditinggalkan selama 48 jam serta antarmuka resolusi konflik instan di kartu pelanggan.
  - Mengoptimalkan performa basis data dan jaringan melalui pencarian berbasis indeks, supresi log berulang bernilai nol informasi, dan pembagian gelombang batch sinkronisasi berkala.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #175  | Monitoring & Remote Config ONT TR-069 (ACS), Auto-Provisioning & Resiliensi Armada | Menuntaskan sistem manajemen jarak jauh ONT TR-069: saklar auto-provisioning granular per perangkat dengan pemisahan keterjangkauan vs konfigurasi, pembacaan parameter segar saat Connection Request, perlindungan tagging VLAN & standar `PPPoEServiceName`, resolusi tabrakan tautan modem (auto-takeover 48 jam & UI resolusi di kartu pelanggan), supresi pembanjiran log webhook inform, dan pengamanan sidik jari kredensial dengan SHA-256. |

### Kemampuan Baru Pengguna/Admin

- **Saklar Auto-Provisioning Instan**: Admin dapat menyalakan atau mematikan fitur konfigurasi otomatis langsung dari kolom saklar pada tabel Perangkat ACS (`/network/acs-devices`) maupun pada kartu Informasi ONT di detail pelanggan broadband (`/services/broadband/:id`).
- **Resolusi Konflik Tautan Modem Sekali Klik**: Ketika dua atau lebih ONT mendeteksi username PPPoE yang sama, admin dapat meninjau daftar perangkat penantang langsung pada kartu pelanggan dan menautkan perangkat yang valid dengan satu klik tombol konfirmasi.
- **Auto-Takeover Modem Pengganti**: Pergantian modem rusak di lapangan oleh teknisi akan secara otomatis dialihkan ke akun pelanggan yang benar setelah 48 jam jika admin lupa memperbarui catatan di aplikasi.
- **Konfigurasi Tagging VLAN yang Akurat**: Admin kini dapat mengaktifkan mode VLAN tagging secara eksplisit pada form konfigurasi WAN, memastikan nilai VLAN ID yang dimasukkan benar-benar diterapkan oleh firmware ONT.
- **Pemantauan Multi-WAN Terpadu**: Melalui laci rincian perangkat (*detail drawer*), staf teknis dapat melihat seluruh koneksi WAN yang terkonfigurasi di ONT pelanggan (termasuk jalur internet pelanggan, jalur manajemen TR-069, status koneksi, dan IP masing-masing).
- **Pembacaan Status Terkini Tanpa Terjebak Cache**: Tombol "Muat Ulang" pada detail perangkat kini menjamin pembacaan nilai paling segar langsung dari modem pelanggan, bukan angka *cache* 5 menit sebelumnya.

### Bug Fix / Solusi Masalah

- **Pencegahan Terjebaknya ONT dalam `credentials_rejected`**: Memisahkan parameter keterjangkauan (kredensial Connection Request) dari parameter konfigurasi, sehingga mematikan saklar auto-provisioning tidak lagi memblokir kredensial panggilan balik ACS.
- **Pencegahan Salah Pilih Jalur Internet Pelanggan**: Memperbaiki algoritma penentuan koneksi layanan di mana WAN internet yang terputus (IP kosong) sebelumnya menyebabkan koneksi Manajemen TR-069 terpilih sebagai jalur internet pelanggan.
- **VLAN ID Diabaikan oleh Firmware**: Memperbaiki masalah pada perangkat yang membutuhkan parameter `X_CT-COM_VLANMode` aktif agar VLAN ID diakui oleh sistem ONT.
- **Dukungan Nama Layanan PPPoE Beragam Vendor**: Menambahkan pencocokan parameter standar TR-098 `PPPoEServiceName` di samping `ServiceName`.
- **Pencegahan Pembanjiran Log Sistem (`api_logs`)**: Menyaring request sukses dari webhook Inform ACS (~570 ribu per hari) sehingga menghemat puluhan gigabyte kapasitas penyimpanan log database dan Logtail.
- **Pencegahan Ledakan Collection Scan MongoDB**: Menggunakan pencocokan exact match nomor seri berindeks sebelum melakukan fallback ke regex.
- **Pencegahan Kebocoran Sandi ACS**: Sidik jari provisi kini disimpan dalam bentuk hash kriptografis SHA-256 dan dibersihkan dari seluruh payload respons API.

### Menu/Fitur Baru

- Kolom dan saklar **Auto Provisioning** pada tabel **Perangkat ACS** (`/network/acs-devices`) serta filter cepat status Auto Config.
- Seksi daftar **Seluruh Koneksi WAN** pada laci rincian perangkat ACS (`AcsDeviceDetailDrawer`).
- Blok penanganan **Konflik Tautan Perangkat** dengan tombol penautan manual dan saklar **Konfigurasi Otomatis** pada kartu **Informasi ONT** di halaman detail layanan broadband (`/services/broadband/:id`).
- Saklar **VLAN Tagging (`vlan_enable`)** pada modal konfigurasi WAN TR-069 (`AcsDeviceConfigModal`).
- Badge status khusus **Kredensial Ditolak** pada kolom status tabel perangkat ACS.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penggunaan Saklar Auto-Provisioning ONT dan Tata Kelola Parameter Kredensial

- **Penjelasan Fitur**:
  Auto-Provisioning adalah fitur otomatisasi yang menyematkan interval *Periodic Inform* standar dan kredensial *Connection Request* terpadu ke modem ONT pelanggan. Untuk melindungi armada modem dari risiko penulisan konfigurasi massal yang tidak diinginkan, sistem menerapkan gerbang berlapis tiga:
  1. Pengaturan global sistem (`acs_provision_enabled`).
  2. Kredensial ACS yang valid dan lengkap di pengaturan sistem.
  3. **Saklar Granular Per Perangkat (`auto_provision_enabled`)**: Bagian konfigurasi interval inform berkala hanya akan diterapkan ke ONT jika saklar pada perangkat tersebut dalam posisi aktif. Sementara kredensial Connection Request tetap dipasang agar perangkat selalu dapat dihubungi oleh sistem.
  *(Catatan: Khusus untuk ONT baru yang belum pernah ditulisi ACS sama sekali, interval inform tetap dinormalkan satu kali pada provisi pertama agar tidak tertahan interval bawaan pabrik yang mencapai 12 jam).*

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Jaringan > Perangkat ACS** (`/network/acs-devices`).
  2. Cari baris perangkat ONT yang ingin dikelola (dapat difilter berdasarkan kolom status Konfigurasi Otomatis).
  3. Klik saklar (*switch*) pada kolom **Konfigurasi Otomatis** untuk mengaktifkan atau menonaktifkan fitur.
  4. Pengaturan juga dapat diubah melalui menu **Layanan > Broadband**, pilih detail salah satu pelanggan, lalu ubah posisi saklar **Konfigurasi Otomatis** pada kartu **Informasi ONT**.
  5. Perubahan tersimpan secara instan dan akan diterapkan saat perangkat ONT mengirimkan sesi *Inform* berikutnya.

---

### 2. Resolusi Konflik Tautan Modem Pelanggan (Link Conflict Resolution & Auto-Takeover)

- **Penjelasan Fitur**:
  Ketika pelanggan mengganti modem atau teknisi memasang modem baru di lokasi pelanggan tanpa melepaskan modem lama di aplikasi, dua perangkat fisik bisa melaporkan username PPPoE yang sama. Sistem Dekasimal V2 menyediakan dua mekanisme pengamanan:
  - **Auto-Takeover Otomatis (48 Jam)**: Jika modem lama tidak pernah aktif melapor selama 48 jam dan modem baru terus melapor aktif selama 48 jam, sistem secara otomatis memindahkan kepemilikan akun ke modem baru.
  - **Resolusi Manual Seketika**: Admin dapat segera mengonfirmasi dan menautkan modem yang benar tanpa harus menunggu 48 jam.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Layanan > Broadband** (`/services/broadband`), lalu buka detail akun pelanggan yang bermasalah.
  2. Pada kartu **Informasi ONT**, sistem akan menampilkan kotak peringatan berwarna kuning bertuliskan **Konflik Tautan Perangkat** jika terdeteksi modem lain dengan username PPPoE yang sama.
  3. Periksa nomor seri modem penantang dan status online-nya pada daftar yang ditampilkan.
  4. Klik tombol **Tautkan perangkat ini** di samping nomor seri yang sesuai dengan modem fisik pelanggan saat ini.
  5. Konfirmasikan tindakan pada modal dialog yang muncul. Sistem akan menautkan modem baru, melepaskan modem lama, dan membersihkan penanda konflik secara otomatis.
  6. Tombol akan memasuki mode jeda (*cooldown*) selama 15 detik untuk mencegah klik ganda atau bentrok instruksi antar-admin.

---

### 3. Konfigurasi Tagging VLAN pada Modal Konfigurasi Jarak Jauh TR-069

- **Penjelasan Fitur**:
  Sebagian besar vendor modem ONT (seperti Realtek dan chipset berbasis CT-COM) mewajibkan mode tagging VLAN diaktifkan (`X_CT-COM_VLANMode = 1`) sebelum parameter VLAN ID diproses oleh firmware. Pada pembaruan ini, antarmuka menyediakan saklar VLAN Tagging yang saling terikat dengan kolom VLAN ID.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Jaringan > Perangkat ACS** (`/network/acs-devices`).
  2. Klik tombol aksi **Konfigurasi** pada baris perangkat ONT pelanggan.
  3. Buka tab **Koneksi WAN**, lalu pilih koneksi WAN yang ingin disunting (atau klik tombol **Buat Koneksi WAN Baru**).
  4. Aktifkan saklar **Tagging VLAN (VLAN Mode)**. Kolom isian **VLAN ID** yang sebelumnya terkunci akan terbuka secara otomatis.
  5. Masukkan ID VLAN internet pelanggan (contoh: `100` atau `200`).
  6. Klik tombol **Terapkan Konfigurasi**. Backend akan mengirimkan parameter saklar VLAN mendahului VLAN ID secara berurutan ke modem pelanggan.
