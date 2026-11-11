# 📝 Daily Work Report - Dedy S.N Putra (2026-11-07)

---

## 📅 Laporan Harian - 7 November 2026

---

## 🌿 Branch: `issue-526` — Operasi Tulis OLT Terpadu, Remediasi Audit & Lab (R1–R9), serta Manajemen Port Uplink, Port PON & VLAN Uplink OLT

### 📌 Informasi Issue

- **Nomor Issue**: #526
- **Judul Issue**: Operasi Tulis Konfigurasi OLT dari Aplikasi Web, Remediasi Hasil Audit & Pengujian Lab Nyata (R1–R9), serta Fitur Manajemen Port Uplink, Port PON, dan Pagar Keselamatan VLAN Uplink
- **Status Branch**: `Belum di-merge` (Dalam pengembangan aktif di branch `issue-526`, 448 tes gateway dan seluruh tes backend lulus 100%)

### 📅 Rincian Commit

---

### 🌐 Bagian 1: Manajemen Port Uplink, Port PON & Pagar Keselamatan VLAN Uplink

#### [0bafe671] - save #526 gateway+backend: pagar anggota uplink terakhir memperhitungkan status port (anggota lain down bukan jalur hidup) - 07 November 2026, 19:21:59 WIB
- **Komponen yang Berubah**:
  - `backend/src/services/oltConfig.service.js`
  - `backend/test/integration/oltConfig.uplinkVlan.test.js`
  - `olt-gateway/src/dialects/huawei/parsers.js`
  - `olt-gateway/test/productionDialect.operations.test.js`
  - `olt-gateway/test/uplinkParsers.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Penyempurnaan Pagar Keselamatan Uplink (*Fail-Safe Uplink Guard*)**: Memastikan pengecekan port anggota VLAN uplink tidak hanya menghitung kuantitas port, melainkan memverifikasi status operasional (*link status*). Jika port anggota lain dalam kondisi `down`/`offline`, port tersebut tidak dianggap sebagai jalur hidup alternatif, sehingga pelepasan VLAN dari port yang sedang aktif tetap diblokir demi mencegah terputusnya akses OLT ke jaringan inti.

#### [7d858dbb] - save #526 docs: AGENTS.md ringkas fitur uplink dan port PON OLT - 07 November 2026, 19:06:58 WIB
- **Komponen yang Berubah**:
  - `AGENTS.md`
- **Deskripsi Perubahan & Fungsi**:
  - Memperbarui dokumentasi panduan arsitektur `AGENTS.md` pada modul OLT Gateway dengan merangkum kapabilitas pembacaan port uplink, port PON, batch scope execution, serta aturan pagar pelepasan VLAN uplink.

#### [4a731f2b] - save #526 frontend: kelola VLAN uplink (drawer, tambah, hapus lewat dialog bertarget) - 07 November 2026, 19:03:29 WIB
- **Komponen yang Berubah**:
  - `frontend/src/app/pages/network/oltManagement/components/OltUplinkSection.jsx`
  - `frontend/src/app/pages/network/oltManagement/components/UplinkPortsTable.jsx`
  - `frontend/src/app/pages/network/oltManagement/components/UplinkPortsTable.test.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/components/UplinkVlanDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/components/UplinkVlanDrawer.test.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/utils/uplinkView.js` [NEW]
  - `frontend/src/app/pages/network/oltManagement/utils/uplinkView.test.js` [NEW]
  - `frontend/src/app/pages/network/oltOnt/components/config/configApi.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Drawer & Antarmuka Pengelolaan VLAN Uplink (`UplinkVlanDrawer.jsx`)**: Menghadirkan antarmuka visual untuk menginspeksi daftar VLAN yang terikat pada masing-masing port uplink, menambahkan VLAN anggota baru, atau menghapus VLAN dari port uplink.
  - **Dialog Konfirmasi Bertarget (*Targeted Confirmation Dialog*)**: Menerapkan modal konfirmasi terarah dengan verifikasi pengetikan nama OLT dan rincian parameter sebelum mengeksekusi penambahan atau penghapusan VLAN uplink.

#### [a6bc7dfb] & [7c43408b] - save #526 backend: operasi uplinkVlanAdd/Remove dengan pagar dini dari snapshot - 07 November 2026, 19:00:49 WIB
- **Komponen yang Berubah**:
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/oltConfig.route.js`
  - `backend/src/services/oltConfig.service.js`
  - `backend/src/utils/oltConfig.constants.js`
  - `backend/src/utils/oltConfigInput.js`
  - `backend/test/integration/oltConfig.paramContract.test.js`
  - `backend/test/integration/oltConfig.uplinkVlan.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Operasi Konfigurasi `UPLINK_VLAN_ADD` & `UPLINK_VLAN_REMOVE`**: Menyediakan alur dua tahap (plan-execute) untuk penambahan dan pelepasan VLAN pada interface uplink OLT (`interface giu` / `interface scu`).
  - **Pagar Validasi Dini (*Early Validation Guards*)**: Menolak rencana penghapusan VLAN jika:
    1. Mencoba menghapus VLAN default (VLAN 1).
    2. Merupakan *Native VLAN* pada port terkait.
    3. Merupakan VLAN L3 / VLANIF yang digunakan untuk manajemen IP OLT.
    4. Merupakan anggota uplink terakhir yang melayani service-port pelanggan aktif (`cuts_customers`).

#### [f71bcb62] & [ce0ff03f] - save #526 gateway: resep uplinkVlanAdd/Remove dengan pagar live dan bukti probe lab - 07 November 2026, 18:58:53 WIB
- **Komponen yang Berubah**:
  - `olt-gateway/src/dialects/huawei/ma5600.js`
  - `olt-gateway/src/transport/cliSession.js`
  - `olt-gateway/test/listChecks.test.js` [NEW]
  - `olt-gateway/test/productionDialect.operations.test.js`
  - `olt-gateway/test/uplinkParsers.test.js` [NEW]
  - Fixture lab Huawei MA5683T nyata: `port-vlan-list-two-lab.txt`, `port-vlan-remove-nonmember-lab.txt`, `vlan-members-3900-lab.txt`
- **Deskripsi Perubahan & Fungsi**:
  - Mendeklarasikan resep perintah CLI pada dialect Huawei MA5600 untuk operasi uplink:
    - Penambahan: `port vlan [vlan_id] [frame]/[slot] [port]`
    - Pelepasan: `undo port vlan [vlan_id] [frame]/[slot] [port]`
  - Menambahkan verifikasi hasil perintah dari probe lab nyata, memastikan OLT Huawei merespons perintah `undo port vlan` dengan tepat dan bebas dari ambigu prompt pager.

#### [a9d7fe81] s/d [054a1c40] (6 commit) - save #526 backend & gateway: sisi baca uplink, port PON, dan eksekusi batch - 07 November 2026, 18:46:35 WIB
- **Komponen yang Berubah**:
  - `backend/src/models/oltDevice.model.js`
  - `backend/src/routes/oltConfig.route.js`
  - `backend/src/services/oltUplink.service.js` [NEW]
  - `frontend/src/app/pages/network/oltManagement/components/OltUplinkSection.jsx` [NEW]
  - `frontend/src/app/pages/network/oltManagement/components/OltPortPonSection.jsx`
  - `olt-gateway/src/transport/cliSession.js` (`executeScopedBatch`)
  - `olt-gateway/src/services/syncUplink.service.js` [NEW]
  - `olt-gateway/src/services/syncPonPort.service.js` [NEW]
  - `olt-gateway/src/dialects/huawei/ma5600.js`
  - `olt-gateway/src/dialects/huawei/parsers.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Inovasi `executeScopedBatch` pada OLT Gateway**: Memungkinkan eksekusi puluhan perintah pembacaan interface (misal: 16 port PON atau 4 port uplink) hanya dalam satu kali masuk sub-mode interface (`config` → `interface gpon 0/x` → eksekusi N perintah → `quit` → `quit`), memangkas waktu sinkronisasi dari ~30 detik menjadi <2 detik.
  - **Sensor Serial Number Modul Optik**: Memastikan serial number modul SFP/optik disensor demi keamanan saat data telemetri disimpan ke basis data.
  - **Pemantauan Kualitas Sinyal Optik Port PON Live**: Membaca daya pancar (*Tx Optical Power*), temperatur modul optik, arus bias, dan tegangan pada port PON OLT secara instan.

---

### 🛡️ Bagian 2: Remediasi Hasil Audit & Pengujian Lab Nyata (R1–R9)

#### [8abeb2eb] & [129cae2d] s/d [307d7785] (12 commit) - feat(olt): operasi tulis OLT dari aplikasi, remediasi audit, dan penerimaan lab B/C/D - 07 November 2026, 17:56:06 WIB
- **Komponen yang Berubah**:
  - `backend/src/services/oltConfig.service.js`
  - `backend/src/services/oltConfigNotify.js`
  - `backend/test/integration/oltConfig.service.test.js`
  - `frontend/src/app/pages/network/oltOnt/components/config/*`
  - `olt-gateway/src/dialects/huawei/ma5600.js`
  - `olt-gateway/src/dialects/defineDialect.js`
  - `olt-gateway/src/operations/operationPlan.js`
  - `olt-gateway/src/transport/cliSession.js`
  - `olt-gateway/test/productionDialect.operations.test.js` [NEW]
  - `olt-gateway/test/writeHardening.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **R1 — Sinkronisasi Kontrak Parameter Backend-Gateway**: Menyelaraskan seluruh pemetaan parameter operasi profil GPON, service-port, dan port PON dengan kontrak interface gateway yang divalidasi via automated contract test.
  - **R2 — Kejujuran Laporan Hasil Operasi (*Honest Status Reporting*)**: Memperbaiki logika pelaporan status operasi di frontend; status "Sukses" hanya ditampilkan jika verifikasi perangkat menyatakan status `applied` (bukan hanya perintah berhasil dikirim).
  - **R3 — Prapemeriksaan Berbasis Bukti Nyata (*Evidence-Based Prechecks*)**: OLT Gateway menjalankan serangkaian `prechecks[]` sebelum perintah tulis dikirim; jika perangkat mengembalikan data di luar dugaan, operasi ditandai `unverified` dan tidak mengeksekusi mutasi.
  - **R4 — Verifikasi Baca-Ulang (*Read-Back Verification*)**: Setelah perintah mutasi dieksekusi di OLT, gateway melakukan pembacaan ulang konfigurasi (*post-verification*) untuk membuktikan bahwa konfigurasi benar-benar aktif di perangkat.
  - **R5 — Proteksi Langkah Scope Mutating**: Dialect OLT Gateway menandai langkah sub-mode yang mengubah konfigurasi sebagai `mutating: true`, sehingga dilarang keras dipanggil oleh alur perintah baca biasa.
  - **R6 — Lokalisasi Penuh & Eliminasi Fallback i18n**: Melengkapi seluruh kamus terjemahan i18n pada fitur OLT dan menghapus 88 fallback string literal `t() || text` agar mematuhi standar internasionalisasi monorepo Dekasimal V2.
  - **R7 — Dialog Konfirmasi Bertarget (*Targeted Confirmation Plan-First*)**: Seluruh operasi berisiko tinggi (seperti reset ONT, hapus ONT, dan hapus service-port) kini menampilkan pratinjau perintah CLI dan mewajibkan konfirmasi sadar ketik nama OLT.
  - **R8 — Kebersihan Arsitektur & Penanganan Error**: Mencegah kebocoran stack trace dan memformat log audit sesuai prinsip zero-PII.
  - **R9 — Penerimaan Lab OLT Nyata (Huawei MA5683T V800R018C00)**: Melakukan pengujian nyata terhadap perangkat fisik OLT lab (IP 192.168.8.2 via Telnet dan SSH). Memvalidasi eksekusi `ont reset` pada ONT online nyata, pembacaan konfigurasi autofind port PON, serta penanganan prompt interaktif OLT.

---

## 🌿 Branch: `issue-531` — Klaim Atomik Pemakaian Barang Gudang pada Tiket Bongkar / Dismantle (`warehouseItemLost`)

### 📌 Informasi Issue

- **Nomor Issue**: #531
- **Judul Issue**: Klaim Atomik Pemakaian Barang Gudang pada Tiket Bongkar/Dismantle: Mencegah Duplikasi Perangkat & Race Condition Saat Pelaporan Perangkat Rusak/Hilang
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `b3da8f50` dan telah dideploy ke `origin/production` melalui commit `64d2d268`)

### 📅 Rincian Commit

#### [64d2d268], [b3da8f50], [d54c0102] - resolve #531 - 07 November 2026, 11:34:41 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/warehouseItem.controller.js`
  - `backend/src/services/warehouseItem.service.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/test/integration/warehouseItemLost.dismantle.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Klaim Atomik Pemakaian Barang Tiket Dismantle (`claimItemUsageForTicket`)**:
    - Memperbaiki celah *race condition* dan klik ganda pada controller `warehouseItemLost` saat teknisi/admin melaporkan perangkat bongkaran pelanggan sebagai rusak atau hilang.
    - Sistem melakukan klaim atomik dokumen pemakaian barang terhadap tiket dismantle terkait. Jika barang sudah dilaporkan sebelumnya, sistem menolak request dengan status HTTP `409 Conflict` (`warehouse.item.alreadyReported`) dan mencegah penggandaan item pada array `ticket.devices`.
  - **Mekanisme Rollback Aman (`releaseItemUsageForTicket`)**:
    - Jika proses pembaruan data barang gudang mengalami kegagalan di tengah jalan, sistem secara otomatis mengembalikan (*rollback*) status pemakaian barang ke keadaan semula (`lent`).
  - **Automated Integration Test**: Menambahkan suite pengujian integrasi `warehouseItemLost.dismantle.test.js` yang memvalidasi pencegahan double-report dan rollback saat error.

---

## 🌿 Branch: `issue-529` — Manajemen Staff & Mitra pada Partner API (Role Admin Portal Mitra)

### 📌 Informasi Issue

- **Nomor Issue**: #529
- **Judul Issue**: Implementasi Endpoint REST Backend Manajemen Staff & Mitra pada Partner API (Role Admin Portal Mitra)
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `07d0f9a5`)

### 📅 Rincian Commit

#### [07d0f9a5] - resolve #529 - 07 November 2026, 14:40:41 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/partnerApiPartner.controller.js`
  - `backend/src/controllers/partnerApiStaff.controller.js` [NEW]
  - `backend/src/middlewares/partnerApiAdminOnly.middleware.js` [NEW]
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/admin.service.js`
  - `backend/src/services/employee.service.js`
  - `backend/src/services/partner.service.js`
  - `backend/src/services/privilege.service.js`
  - `backend/src/utils/dataTableParamsInput.js` [NEW]
  - `backend/src/utils/flagQuery.js` [NEW]
  - `backend/test/integration/partnerApiPartner.list.test.js` [NEW]
  - `backend/test/integration/partnerApiStaff.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Endpoint REST Manajemen Staf & Mitra pada Partner API (`partnerApi.route.js`)**:
    - Menyediakan endpoint lengkap untuk manajemen staf mitra (daftar staf, detail, pembuatan, dan pengubahan hak akses) yang diproteksi oleh middleware `partnerApiAdminOnly`.
    - Menyediakan endpoint query parameter datatable terstandarisasi (`dataTableParamsInput.js`) dan filter boolean query flag aman (`flagQuery.js`).
    - Menyertakan unit dan integrasi test menyeluruh untuk memastikan integritas data pengguna portal mitra.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #526  | Operasi Tulis OLT, Remediasi Lab R1–R9, serta Manajemen Port Uplink & VLAN | Mengukuhkan OLT Gateway dan modul OLT Dekasimal dengan keandalan produksi: eksekusi batch scope super cepat (<2 detik), pagar keselamatan pelepasan VLAN uplink (mencegah OLT terisolasi), audit lab nyata MA5683T (R1–R9), verifikasi baca-ulang otomatis, dan UI pengelolaan uplink. |
| #531  | Klaim Atomik Barang Gudang pada Tiket Bongkar (`warehouseItemLost`) | Menghilangkan bug duplikasi perangkat pada tiket dismantle akibat klik ganda saat pelaporan barang hilang/rusak, dilengkapi klaim atomik dan rollback otomatis. |
| #529  | Manajemen Staff & Mitra pada Partner API | Menyelesaikan fitur administrasi staf dan mitra pada API portal mitra khusus role admin. |

---

### Kemampuan Baru Pengguna/Admin

- **Manajemen VLAN Uplink Terarah**: Admin jaringan dapat melihat VLAN apa saja yang dialirkan melalui setiap port uplink OLT, menambahkan VLAN layanan baru ke uplink, atau melepaskan VLAN melalui dialog konfirmasi terpandu.
- **Perlindungan Anti-Isolasi OLT (*Anti-Isolation Protection*)**: Sistem secara cerdas menolak penghapusan VLAN dari port uplink jika VLAN tersebut merupakan satu-satunya jalur yang membawa service-port pelanggan aktif atau merupakan VLAN manajemen OLT.
- **Pemantauan Kualitas Optik Port PON Cepat**: Admin dapat melihat informasi daya optik modul PON OLT secara live berkat optimalisasi `executeScopedBatch`.
- **Kepastian Konfigurasi OLT**: Status keberhasilan konfigurasi OLT dijamin oleh verifikasi baca-ulang otomatis (*read-back verification*), memberikan kepastian 100% bahwa perintah benar-benar diterapkan di hardware.
- **Pelaporan Barang Rusak/Hilang Tiket Dismantle yang Aman**: Admin gudang dan teknisi tidak perlu khawatir terjadi duplikasi data saat melaporkan perangkat bongkaran karena diproteksi klaim atomik berstatus HTTP 409.

---

### Bug Fix / Solusi Masalah

- **Pencegahan OLT Terputus dari Core Network**: Mengatasi potensi insiden jaringan di mana admin tidak sengaja melepas VLAN distribusi utama dari port uplink terakhir yang sedang aktif (*up*).
- **Eliminasi Race Condition Tiket Bongkar**: Menutup celah penggandaan perangkat pada `ticket.devices` saat terjadi double-click pada tombol lapor barang hilang/rusak.
- **Optimalisasi Waktu Tunggu Sesi CLI OLT**: Menghilangkan overhead buka-tutup mode interface berulang-ulang melalui implementasi `executeScopedBatch` pada OLT Gateway.

---

### Menu/Fitur Baru

- **Tab Port Uplink pada Detail OLT**: Menampilkan tabel port uplink (GIU/SCU), status link (*online/offline/LOS*), informasi modul optik, dan daftar VLAN terpasang.
- **Drawer Kelola VLAN Uplink**: Antarmuka untuk menambah dan menghapus VLAN anggota port uplink.
- **Endpoint Analitik Uplink & Port PON**: Rute internal OLT Gateway `/internal/olts/:id/sync-uplink` dan `/internal/olts/:id/sync-pon-port`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - Fitur **Manajemen Port Uplink & Pagar Keselamatan VLAN** mengatur distribusi lalu lintas data antara perangkat OLT dan router distribusi/core.
  - Setiap port uplink (kartu GIU atau port kontrol SCU) dapat membawa banyak VLAN (trunking). Saat admin ingin mengubah konfigurasi VLAN pada port uplink, sistem Dekasimal secara proaktif memeriksa apakah port tersebut adalah port hidup terakhir yang mengalirkan layanan pelanggan. Jika ya, pelepasan VLAN dibatalkan demi menjaga ketersediaan layanan (*high availability*).

- **Langkah Penggunaan (Tutorial Mengelola VLAN pada Port Uplink OLT)**:
  1. **Buka Menu OLT**: Masuk ke aplikasi admin, buka sidebar **Jaringan > Manajemen OLT**.
  2. **Buka Tab Port Uplink**: Pada dasbor OLT terpilih, klik sub-tab **Port Uplink**.
  3. **Inspeksi Status Port**: Periksa status fisik port uplink (kecepatan link, status link *Up/Down*, dan informasi modul SFP optik).
  4. **Buka Drawer Kelola VLAN**:
     - Pada baris port uplink yang diinginkan, klik tombol aksi **Kelola VLAN**.
     - Drawer akan menampilkan daftar seluruh VLAN yang saat ini menjadi anggota port tersebut.
  5. **Menambahkan VLAN ke Port Uplink**:
     - Klik tombol **Tambah VLAN**.
     - Pilih ID VLAN yang terdaftar di OLT.
     - Tinjau pratinjau perintah CLI (`port vlan ...`).
     - Masukkan nama OLT untuk konfirmasi keamanan dan klik **Terapkan**.
  6. **Melepaskan VLAN dari Port Uplink**:
     - Klik ikon hapus pada VLAN yang ingin dilepas.
     - Jika VLAN tersebut merupakan jalur hidup terakhir pelanggan aktif, sistem akan menolak rencana tersebut dengan penjelasan detail bahaya.
     - Jika aman, masukkan nama OLT pada dialog konfirmasi dan klik **Eksekusi Pelepasan**.

