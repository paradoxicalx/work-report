# 📝 Daily Work Report - Dedy S.N Putra (2026-11-12)

---

## 📅 Laporan Harian - 12 November 2026

---

## 🌿 Branch: `issue-542` — Konfigurasi OMCI ONT Lanjutan (WAN IP/DHCP/PPPoE, Port LAN ETH, Bind ACS TR-069), Auto-Sync OLT Terjadwal & Drift Detection, serta Detail Remote Profil & Port VLAN

### 📌 Informasi Issue

- **Nomor Issue**: #542
- **Judul Issue**: Implementasi Konfigurasi OMCI ONT Lanjutan via OLT Gateway (WAN, Port LAN, Binding TR-069 ACS), Fitur Sinkronisasi Otomatis OLT Terjadwal & Deteksi Drift Konfigurasi, serta Drawer Detail Remote Profil GPON & Port VLAN
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` dan telah dideploy ke `origin/production` melalui commit `abd76045` dengan 723 tes gateway dan seluruh suite pengujian backend lulus 100%)

### 📅 Rincian Commit

#### [abd76045] & [2ceba6ed] - resolve #542 - 12 November 2026, 18:10:06 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/olt.controller.js`
  - `backend/src/controllers/oltConfig.controller.js`
  - `backend/src/models/oltDevice.model.js`
  - `backend/src/models/oltDriftEvent.model.js` [NEW]
  - `backend/src/routes/internal.route.js`
  - `backend/src/routes/olt.route.js`
  - `backend/src/routes/oltConfig.route.js`
  - `backend/src/services/cronSettings.service.js`
  - `backend/src/services/oltAutoSync.service.js` [NEW]
  - `backend/src/services/oltConfig.service.js`
  - `backend/src/services/oltConfigNotify.js`
  - `backend/src/services/oltGateway.service.js`
  - `backend/src/services/oltOntConfig.service.js` [NEW]
  - `backend/src/utils/oltConfig.constants.js`
  - `backend/src/utils/oltConfigInput.js`
  - `backend/src/utils/oltDrift.js` [NEW]
  - `backend/src/utils/oltInput.js`
  - `backend/src/utils/oltOntConfigInput.js` [NEW]
  - `backend/test/integration/oltAutoSync.drift.test.js` [NEW]
  - `backend/test/integration/oltConfig.paramContract.test.js`
  - `backend/test/integration/oltConfig.profileDetail.test.js` [NEW]
  - `backend/test/integration/oltOntConfig.service.test.js` [NEW]
  - `backend/test/unit/oltAutoSync.test.js` [NEW]
  - `backend/test/unit/oltDrift.test.js` [NEW]
  - `backend/test/unit/oltOntConfigInput.test.js` [NEW]
  - Modul Cron Worker (`cron-worker/*`):
    - `src/jobs/processors/oltAutoSync.js` [NEW], `src/jobs/scheduler.js`, `src/jobs/worker.js`, `src/services/api.service.js`
  - Modul Frontend Manajemen OLT Terpadu (`frontend/src/app/pages/network/*`):
    - `oltManagement/components/OltFreshnessBar.jsx` [NEW], `OltFreshnessBar.test.jsx` [NEW]
    - `oltManagement/components/ProfileDetailDrawer.jsx` [NEW], `ProfileRemoteDetail.jsx` [NEW]
    - `oltManagement/components/PortVlanDetailDrawers.jsx` [NEW]
    - `oltManagement/components/OltMaintenanceActions.jsx` [NEW]
    - `oltManagement/utils/syncStatus.js` [NEW], `oltManagement/utils/syncStatus.test.js` [NEW]
    - `oltOnt/components/config/OntOmciConfigDrawer.jsx` [NEW], `OntOmciDrawers.test.jsx` [NEW]
    - `oltOnt/components/OntDetailDrawer.jsx`
  - Modul OLT Gateway (`olt-gateway/*`):
    - `src/dialects/huawei/ma5600.js` (perintah dialect & resep OMCI WAN/IP/ETH/ACS, display wan-info/ipconfig/capability/version)
    - `src/dialects/huawei/parsers.js` (parser wan-info, ipconfig, capability, port state, version, acs profiles)
    - `src/services/acsProfiles.service.js` [NEW], `src/services/acsRead.js` [NEW]
    - `src/services/ontConfigRead.service.js` [NEW], `src/services/profileDetail.service.js` [NEW]
    - `src/transport/cliSession.js`, `src/operations/operationPlan.js`
    - `test/acsProfiles.test.js` [NEW], `test/ontConfigParsers.test.js` [NEW], `test/ontConfigRead.test.js` [NEW], `test/profileDetail.test.js` [NEW], `test/secretParams.test.js` [NEW], `test/inScopeVerify.test.js` [NEW]
    - 134 berkas termodifikasi/terbuat, +12.886 baris kode
- **Deskripsi Perubahan & Fungsi**:

  1. **Konfigurasi OMCI ONT Lanjutan (WAN, Port LAN, & Binding TR-069 ACS)**:
     - **Manajemen WAN & IP Host ONT via OMCI**:
       - Mengimplementasikan alur konfigurasi WAN pada ONT tanpa perlu membuka antarmuka web ONT pelanggan. Mendukung 3 mode koneksi WAN: Static IP, DHCP Client, dan PPPoE Client.
       - Menyediakan opsi jenis layanan WAN: *Internet*, *TR-069*, atau *Internet + TR-069 (Both)*.
       - Validasi ketat indeks TR-069 pada line profile ONT: mencegah konflik penomoran indeks host IP dan menjamin alokasi VLAN WAN yang konsisten.
     - **Kontrol Port LAN / ETH ONT**:
       - Memungkinkan admin mengaktifkan atau menonaktifkan (*On/Off*) port Ethernet LAN ONT (ETH 1–8) secara terpusat dari OLT melalui protokol OMCI.
     - **Manajemen Profil ACS TR-069 di OLT**:
       - Mendukung pembacaan, pembuatan, penghapusan, serta pengikatan (*binding/unbinding*) profil ACS (GenieACS TR-069) ke ONT melalui OLT.
       - Parameter rahasia (seperti password PPPoE dan password ACS) diproteksi oleh modul `secretParams.js`, dienkripsi at-rest, dan disensor dari pencatatan log audit.
     - **Antarmuka OMCI Terpadu (`OntOmciConfigDrawer.jsx`)**:
       - Drawer interaktif pada detail ONT yang memungkinkan konfigurasi IP host, mode WAN, port LAN, dan profil ACS dalam satu tampilan form modern dengan konfirmasi keamanan plan-execute.

  2. **Auto-Sync OLT Terjadwal & Deteksi Drift Konfigurasi (*Drift Detection*)**:
     - **Worker Sinkronisasi Otomatis BullMQ (`oltAutoSync`)**:
       - Penjadwal background job yang berjalan secara periodik untuk memperbarui snapshot OLT (profil GPON, port uplink, port PON, VLAN, dan antarmuka L3).
     - **Deteksi Drift (*Configuration Drift Engine*)**:
       - Membandingkan konfigurasi nyata di perangkat OLT dengan data yang tersimpan di basis data sistem Dekasimal.
       - Jika terdeteksi perubahan yang dilakukan di luar aplikasi (misal: teknisi mengedit konfigurasi langsung via CLI konsol/Telnet), sistem secara otomatis mencatat `OltDriftEvent` dan memberikan notifikasi peringatan.
     - **Indikator Kesegaran Data (`OltFreshnessBar.jsx`)**:
       - Komponen visual pada dasbor OLT yang menampilkan usia data snapshot (contoh: *Disinkronkan 5 menit lalu*), status live sinkronisasi, dan tombol cepat *Sinkronkan Sekarang*.

  3. **Drawer Detail Remote Profil GPON & Port VLAN**:
     - **Detail Remote Line Profile & Service Profile (`ProfileRemoteDetail.jsx`)**:
       - Mengambil dan mengurai data konfigurasi mendalam langsung dari OLT via CLI: pemetaan GEM port, alokasi T-CONT, pemetaan port ETH/POTS, dan konfigurasi VLAN translation.
     - **Detail Port VLAN (`PortVlanDetailDrawers.jsx`)**:
       - Drawer inspeksi detail keanggotaan VLAN pada setiap port uplink dan port PON secara transparan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #542  | Konfigurasi OMCI ONT Lanjutan, Auto-Sync OLT, Drift Detection, & Detail Remote Profil | Melengkapi kapabilitas OLT Gateway dan backend Dekasimal dengan konfigurasi OMCI jarak jauh (WAN Static/DHCP/PPPoE, kontrol port LAN ETH, TR-069 ACS binding), otomasi sinkronisasi berkala via BullMQ, deteksi inkonsistensi data OLT (*drift detection*), serta inspeksi remote profil GPON. |

---

### Kemampuan Baru Pengguna/Admin

- **Konfigurasi WAN ONT Jarak Jauh via OMCI**: Admin teknisi dapat mengatur mode koneksi WAN ONT pelanggan (PPPoE, DHCP, atau Static IP) beserta alokasi VLAN langsung dari OLT tanpa memerlukan akses login ke web GUI modem pelanggan.
- **Manajemen Port LAN ONT Terpusat**: Admin dapat menyalakan atau mematikan port Ethernet LAN (ETH 1–8) pelanggan secara terpusat untuk keperluan isolasi atau penanganan komplain.
- **Binding Profil TR-069 ACS via OLT**: Memungkinkan integrasi otomatis ONT baru ke server GenieACS melalui binding profil ACS di OLT.
- **Deteksi Otomatis Perubahan Liar di OLT (*Drift Detection*)**: Sistem secara otomatis mendeteksi dan memberi peringatan jika terdapat konfigurasi yang diubah manual oleh teknisi via terminal CLI OLT di luar sistem Dekasimal.
- **Pemantauan Kesegaran Data (*Data Freshness Indicator*)**: Admin dapat melihat secara visual kapan data OLT terakhir disinkronkan dan memicu sinkronisasi manual jika diperlukan.
- **Inspeksi Rinci Profil GPON Remote**: Admin dapat melihat konfigurasi internal Line Profile (GEM Port, T-CONT) dan Service Profile OLT secara transparan.

---

### Bug Fix / Solusi Masalah

- **Pencegahan Kebocoran Kredensial OMCI**: Password PPPoE dan kredensial sensitif pada konfigurasi OMCI disaring oleh `secretParams.js` sehingga tidak pernah bocor ke log audit atau respons yang tidak berwenang.
- **Eliminasi Konflik Indeks TR-069 pada WAN ONT**: Validasi ketat pada backend memastikan indeks WAN tidak bertabrakan dengan indeks IP host TR-069 yang telah ditentukan pada line profile.

---

### Menu/Fitur Baru

- **Drawer Konfigurasi OMCI ONT**: Antarmuka terpadu di detail ONT untuk pengaturan WAN (Internet/TR-069), alokasi IP Host, port LAN ETH, dan profil ACS.
- **Bar Kesegaran Data OLT (`OltFreshnessBar`)**: Komponen status sinkronisasi dan drift alert pada dasbor OLT.
- **Drawer Detail Remote Profil GPON**: Tampilan rincian konfigurasi Line Profile, Service Profile, DBA, dan Traffic Table langsung dari OLT.
- **Job Latar `oltAutoSync` pada Cron Worker**: Penjadwalan sinkronisasi berkala data OLT via BullMQ.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - Fitur **Konfigurasi OMCI ONT Lanjutan** memanfaatkan protokol Optical Network Terminal Management and Control Interface (OMCI) standar ITU-T G.988 untuk mengonfigurasi perangkat ONT pelanggan langsung dari OLT.
  - Dengan fitur ini, teknisi NOC dapat menyetel kredensial PPPoE, mengonfigurasi IP statis/DHCP, mengatur port LAN, atau mengarahkan ONT ke ACS TR-069 tanpa harus datang ke rumah pelanggan atau meminta pelanggan membuka port remote web ONT.

- **Langkah Penggunaan (Tutorial Mengonfigurasi WAN PPPoE ONT via OMCI)**:
  1. **Buka Menu ONT**: Masuk ke aplikasi admin, buka sidebar **Jaringan > ONT**.
  2. **Buka Detail ONT**: Klik baris ONT pelanggan yang ingin dikonfigurasi untuk membuka drawer detail ONT.
  3. **Buka Tab Konfigurasi OMCI**:
     - Pada drawer detail ONT, pilih tab **Konfigurasi OMCI**.
     - Sistem akan menampilkan status konfigurasi WAN, port LAN ETH, dan profil ACS yang sedang aktif saat ini.
  4. **Menambahkan / Mengubah WAN**:
     - Klik tombol **Konfigurasi WAN**.
     - Pilih tipe layanan (misal: `Internet`).
     - Pilih mode akses `PPPoE`.
     - Masukkan Username dan Password PPPoE pelanggan.
     - Tentukan VLAN ID (misal: `100`).
     - Klik **Lanjutkan ke Pratinjau**.
  5. **Konfirmasi & Eksekusi**:
     - Tinjau perintah CLI OMCI Huawei yang dirakit oleh sistem.
     - Masukkan nama perangkat OLT pada kolom konfirmasi keamanan.
     - Klik **Eksekusi Konfigurasi**. OLT Gateway akan menerapkan konfigurasi langsung ke ONT pelanggan via OMCI.

