# 📝 Daily Work Report - Dedy S.N Putra (2026-11-06)

---

## 📅 Laporan Harian - 6 November 2026

---

## 🌿 Branch: `issue-526` — Implementasi Konfigurasi OLT Tulis, Dashboard Modern Terpadu, Manajemen Profil, Tabel Trafik, Port PON & Aksi Cepat ONT

### 📌 Informasi Issue

- **Nomor Issue**: #526
- **Judul Issue**: Implementasi Fitur Konfigurasi Tulis OLT Terpadu, Dasbor Modern OLT (Simulator Sasis & Telemetri), Manajemen Profil GPON (DBA, Line, Service, Traffic Table), Pengelolaan Port PON, serta Modal Aksi Cepat ONT
- **Status Branch**: `Belum di-merge` (Dalam pengembangan aktif di branch `issue-526`, ahead 1 commit dari `origin/issue-526`, 281 tes gateway dan 48 tes backend lulus 100%)

### 📅 Rincian Commit

#### [feeb575d] - feat(olt): implementasi konfigurasi OLT tulis, dashboard modern, dan manajemen terpadu (resolve #526) - 06 November 2026, 15:20:29 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/oltConfig.controller.js`
  - `backend/src/controllers/oltOntInsight.controller.js` [NEW]
  - `backend/src/models/oltDevice.model.js`
  - `backend/src/models/oltServicePort.model.js` [NEW]
  - `backend/src/routes/oltConfig.route.js`
  - `backend/src/routes/oltOntInsight.route.js` [NEW]
  - `backend/src/services/oltConfig.service.js`
  - `backend/src/services/oltGateway.service.js`
  - `backend/src/services/oltOntInsight.service.js` [NEW]
  - `backend/src/utils/oltConfig.constants.js`
  - `backend/src/utils/oltConfigInput.js`
  - `backend/test/integration/oltConfig.service.test.js`
  - `backend/test/integration/oltOntInsight.controller.test.js` [NEW]
  - `backend/test/integration/oltOntInsight.service.test.js` [NEW]
  - `backend/test/unit/oltConfigInput.sliceB.test.js` [NEW]
  - `backend/test/unit/oltConfigInput.sliceC1.test.js` [NEW]
  - `backend/test/unit/oltConfigInput.sliceC2.test.js` [NEW]
  - `backend/test/unit/oltConfigInput.sliceD.test.js` [NEW]
  - `backend/test/unit/oltConfigInput.test.js`
  - `backend/test/unit/oltServicePort.model.test.js` [NEW]
  - Frontend Dasbor Terpadu Modern (`frontend/src/app/pages/network/oltManagement/dashboard/*`) [NEW]:
    - `index.jsx`, `index.test.jsx`
    - `components/OltChassisSimulator.jsx`, `components/OltChassisSimulator.test.jsx`
    - `components/OltDashboardHeader.jsx`, `components/OltDashboardHeader.test.jsx`
    - `components/OltDashboardTabs.jsx`, `components/OltDashboardTabs.test.jsx`
    - `components/OltHeroStats.jsx`, `components/OltHeroStats.test.jsx`
    - `components/OltTelemetryCards.jsx`, `components/OltTelemetryCards.test.jsx`
    - `components/OltDownCauseCard.jsx`, `components/OltDownCauseCard.test.jsx`
    - `components/OltPonCapacityCard.jsx`, `components/OltPonCapacityCard.test.jsx`
    - `components/OltAutofindBanner.jsx`, `components/OltAutofindBanner.test.jsx`
    - `components/OltConfigSaveCard.jsx`, `components/OltConfigSaveCard.test.jsx`
    - `components/OltSubPills.jsx`, `components/OltSubPills.test.jsx`
    - `sections/OltOverviewSection.jsx`, `sections/OltOverviewSection.test.jsx`
  - Frontend Manajemen Profil, Tabel Trafik & Port PON:
    - `components/OltDbaProfileSection.jsx` [NEW]
    - `components/OltTrafficTableSection.jsx` [NEW]
    - `components/OltLineProfileSection.jsx` [NEW]
    - `components/OltServiceProfileSection.jsx` [NEW]
    - `components/OltPortPonSection.jsx` [NEW]
    - `components/OltVlanSection.jsx` [NEW]
  - Frontend Registrasi & Service-Port ONT:
    - `components/OntServicePortSection.jsx` [NEW]
    - `components/config/ServicePortCreateDrawer.jsx` [NEW]
    - `components/config/OntRegisterDrawer.jsx`
    - `components/config/OntRegisterDrawer.test.jsx` [NEW]
    - `components/config/forms/OntRegisterForm.jsx`
    - `components/config/forms/OntRegisterForm.test.jsx` [NEW]
  - Modul OLT Gateway (`olt-gateway/*`):
    - `src/dialects/huawei/ma5600.js` (perintah & resep DBA, Traffic Table, Line Profile, Service Profile, Port PON, Service-Port, VLAN)
    - `src/dialects/huawei/parsers.js` (parser DBA, Traffic Table, Line Profile, Service Profile, Service-Port, VLAN)
    - `src/services/servicePortSync.service.js` [NEW]
    - `src/services/vlanSync.service.js` [NEW]
    - `src/services/profileSync.service.js`
    - `src/models/oltServicePort.model.js` [NEW]
    - `src/repositories/oltServicePortRepo.js` [NEW]
    - `test/parsers.b.test.js` [NEW], `test/parsers.c1.test.js` [NEW], `test/syncSliceB.test.js` [NEW], `test/operationPlan.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Dasbor Modern OLT & Simulator Sasis Interaktif (`dashboard/*`)**:
    - Menggantikan halaman detail lama (`detail.jsx`) dengan dasbor modular berkinerja tinggi yang memvisualisasikan sasis perangkat OLT fisik (`OltChassisSimulator.jsx`).
    - Merender slot kontrol (SCUN/SCUK), slot GPON (GPBD/GPFD), dan slot uplink dengan indikator LED status real-time, nomor slot fisik, dan jumlah port aktif.
    - Menghadirkan kartu metrik telemetri modern (`OltHeroStats.jsx` & `OltTelemetryCards.jsx`): status koneksi live, uptime perangkat, kapasitas ONT terdaftar vs online, distribusi redaman optik, serta rasio utilisasi port.
    - Analisis cerdas penyebab ONT padam (`OltDownCauseCard.jsx`): mengkategorikan pemadaman berdasarkan *dying-gasp* (listrik padam), *LOS* (kabel optik putus), atau *offline biasa*.
    - Banner deteksi ONT baru *autofind* (`OltAutofindBanner.jsx`) dengan akses cepat satu klik untuk pendaftaran.
    - Kartu pengingat konfigurasi flash (`OltConfigSaveCard.jsx`): memberi tahu admin jika terdapat konfigurasi memori OLT yang belum disimpan permanen (`save`) dengan tombol eksekusi langsung.
  - **Manajemen Profil GPON & Tabel Trafik Terintegrasi (Irisan C1 & C2)**:
    - **DBA Profile (`OltDbaProfileSection.jsx`)**: Mengelola dan menyinkronkan profil DBA Type 1–5 dari OLT, mendukung pembuatan profil DBA baru dengan parameter alokasi fix/assurance/maximum bandwidth (kbps).
    - **Traffic Table (`OltTrafficTableSection.jsx`)**: Menampilkan dan mengonfigurasi tabel tarif bandwidth IP (CIR, PIR, CBS, PBS, Priority) untuk kontrol kualitas layanan (QoS) ONT.
    - **Line Profile & Service Profile (`OltLineProfileSection.jsx` & `OltServiceProfileSection.jsx`)**: Menampilkan profil binding ONT GPON yang tersimpan di OLT untuk mempermudah pemetaan layanan broadband.
    - **Sinkronisasi Dialect & Parser Cerdas**: Mengimplementasikan parser regex tangguh `parseDbaProfileList`, `parseTrafficTableList`, `parseLineProfileList`, dan `parseServiceProfileList` dari keluaran nyata OLT lab Huawei MA5683T.
  - **Manajemen Port GPON & Kapasitas PON (Irisan D)**:
    - Menghadirkan tab `OltPortPonSection.jsx` yang mengagregasikan status operasional per port GPON (Frame/Slot/Port), utilisasi ONT aktif vs kapasitas maksimal (128 ONT/port), serta aksi operasional administrator.
  - **Layanan Backend Insight OLT (`oltOntInsight.service.js` & `oltOntInsight.controller.js`)**:
    - Endpoint REST baru `GET /api/v1/olt/:olt_id/insights` yang menghitung agregasi telemetri live, breakdown penyebab padam, dan utilisasi kapasitas tanpa membebani perangkat fisik OLT.
  - **Alur Provisi Service-Port & Registrasi ONT Lengkap**:
    - Validasi menyeluruh pada form registrasi ONT autofind dan pembentukan service-port multi-service.

---

### 📝 Pekerjaan Belum Di-commit (Work In Progress)

- **Komponen**:
  - `frontend/src/app/pages/network/oltOnt/components/config/OntChangeProfileModal.jsx` [NEW]
  - `frontend/src/app/pages/network/oltOnt/components/config/OntDescriptionModal.jsx` [NEW]
  - `frontend/src/app/pages/network/oltOnt/components/config/ServicePortCreateModal.jsx` [NEW]
  - `frontend/src/app/pages/network/oltOnt/components/OntDetailDrawer.jsx`
  - `frontend/src/app/pages/network/oltOnt/components/OntDetailBody.jsx`
  - `frontend/src/app/pages/network/oltOnt/components/OntServicePortSection.jsx`
  - `frontend/src/app/pages/network/oltOnt/components/config/configApi.js`
  - `frontend/src/app/pages/network/oltOnt/components/config/utils/configActions.js`
  - `frontend/src/app/pages/network/oltOnt/components/config/utils/configActions.test.js`
  - `backend/src/routes/oltConfig.route.js`
  - `backend/src/services/oltConfig.service.js`
  - `backend/src/services/oltConfigNotify.js`
  - `backend/src/utils/oltConfig.constants.js`
  - `backend/src/utils/oltConfigInput.js`
  - `backend/test/unit/oltConfigInput.test.js`
  - `olt-gateway/src/dialects/huawei/ma5600.js`
  - `olt-gateway/test/operationPlan.test.js`
- **Deskripsi Progres**:
  - **Modal Aksi Cepat pada Detail ONT**:
    - `OntChangeProfileModal.jsx`: Modal pengubahan Line Profile dan Service Profile secara langsung pada ONT yang sudah aktif tanpa perlu melepas registrasi perangkat.
    - `OntDescriptionModal.jsx`: Modal pengubahan deskripsi nama pelanggan langsung dari drawer detail ONT.
    - `ServicePortCreateModal.jsx`: Modal pembuatan service-port yang menyederhanakan input alokasi port layanan dengan pemilihan VLAN dan kalkulasi otomatis.
  - **Dukungan Operasi Tulis Modifikasi Profil di OLT Gateway**:
    - Menambahkan resep dialect `ontModifyProfiles` pada Huawei MA5600 (`ont modify [port] [ont_id] ont-lineprofile-name [line_profile] ont-srvprofile-name [srv_profile]`).
    - Menambahkan validasi input `validateOntModifyProfilesInput` dan unit test di backend.
    - Menghubungkan tombol aksi cepat pada header drawer detail ONT (`OntDetailDrawer.jsx`).

---

## 🌿 Branch Lain Terkait di Repositori: `issue-529` — Manajemen Staff & Mitra pada Partner API

### 📌 Informasi Issue

- **Nomor Issue**: #529
- **Judul Issue**: Implementasi Endpoint Manajemen Staff & Mitra pada Partner API (Role Admin Portal Mitra)
- **Status Branch**: `Belum di-merge` (Commit `0a319b8a` oleh tim pengembang Idham)

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #526  | Konfigurasi OLT Tulis, Dashboard Modern, Profil GPON, & Manajemen Terpadu | Mengubah pengalaman operasional OLT Dekasimal menjadi dasbor kelas *enterprise* dengan simulator sasis visual interaktif, manajemen lengkap profil GPON (DBA, Traffic Table, Line/Service Profile), inventori port PON, alur tulis aman dua tahap (Plan-Execute), serta aksi cepat modifikasi ONT. |
| #529  | Manajemen Staff & Mitra pada Partner API *(Kontribusi Tim)* | Menyediakan endpoint REST backend untuk pengelolaan staf dan data mitra pada portal mitra dengan hak akses khusus admin. |

---

### Kemampuan Baru Pengguna/Admin

- **Inspeksi Visual Sasis OLT**: Admin NOC kini dapat melihat representasi visual sasis fisik OLT (kartu kontrol, kartu GPON, status port LED hijau/merah/abu-abu) secara real-time.
- **Monitoring Penyebab Padam (*Root Cause Analysis*)**: Sistem langsung membedakan apakah ONT mati karena listrik rumah padam (*dying-gasp*) atau kabel optik putus (*LOS*), memudahkan tim helpdesk mendiagnosis tiket komplain pelanggan.
- **Pengelolaan Profil & Bandwidth Terpusat**: Admin dapat membuat dan mengubah profil alokasi kecepatan internet (Tabel Trafik PIR/CIR dan DBA Profile) langsung dari antarmuka web Dekasimal tanpa harus mengetik perintah CLI manual di terminal OLT.
- **Aksi Cepat ONT Tanpa Downtime**: Admin dapat mengubah Line Profile dan Service Profile ONT pelanggan yang sudah terdaftar melalui modal aksi cepat tanpa perlu menghapus (*delete*) konfigurasi ONT.
- **Pengingat & Penyimpanan Konfigurasi Flash Cepat**: Kartu pengingat status penyimpanan konfigurasi menampilkan tombol *Simpan Konfigurasi* langsung saat terdeteksi ada perubahan belum permanen.

---

### Bug Fix / Solusi Masalah

- **Pencegahan Human-Error Saat Menulis Konfigurasi OLT**: Menerapkan mekanisme wajib pratinjau perintah CLI dan pengetikan nama perangkat sebelum eksekusi dilakukan oleh microservice gateway.
- **Pembersihan Modul Lama**: Menghapus implementasi halaman detail statis lama (`detail.jsx`) dan mengalihkannya sepenuhnya ke arsitektur dasbor baru yang reaktif.
- **Parsing Data Trafik OLT Non-Standar**: Parser tabel trafik kini mampu menangani berbagai format penomoran index indeks bertingkat pada firmware Huawei SmartAX MA5683T tanpa menyebabkan *unhandled exception*.

---

### Menu/Fitur Baru

- **Dasbor Terpadu Manajemen OLT**: Rute `/network/olt` kini menyajikan antarmuka modern dengan sub-tab: Ringkasan & Sasis, VLAN, Port PON, Profil DBA, Tabel Trafik, Line Profile, dan Service Profile.
- **Simulator Sasis Hardware OLT**: Komponen visual interaktif pemetaan slot fisik dan status port GPON.
- **Modal Aksi Cepat ONT**: Modal dialog Ubah Profil, Ubah Deskripsi, Tambah Service-Port, dan Reset ONT pada drawer detail ONT.
- **Endpoint Analitik OLT Insight**: REST API `/api/v1/olt/:olt_id/insights` untuk agregasi telemetri live.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **Dasbor Terpadu & Simulator Sasis OLT** menggabungkan data inventaris hardware, status telemetri koneksi, dan analitik ONT ke dalam satu antarmuka visual.
  - Setiap slot kartu board (kontrol, GPON, uplink) dimodelkan menyerupai bentuk fisik sasis OLT asli. Klik pada slot atau port GPON akan menampilkan ringkasan utilisasi kapasitas ONT terdaftar, redaman rata-rata, dan status operasional perangkat.

- **Langkah Penggunaan (Tutorial Mengubah Profil ONT via Aksi Cepat)**:
  1. **Buka Menu ONT**: Dari menu sidebar, pilih **Jaringan > ONT**.
  2. **Buka Detail ONT**: Klik baris ONT pelanggan yang ingin dikonfigurasi untuk membuka drawer detail ONT.
  3. **Pilih Aksi Ubah Profil**:
     - Pada header drawer detail ONT, klik tombol aksi **Ubah Profil**.
     - Dialog modal akan menampilkan Line Profile dan Service Profile yang sedang aktif saat ini.
     - Pilih Line Profile baru (misal: profil alokasi paket internet baru) atau Service Profile baru dari menu dropdown yang otomatis mengambil data profil terdaftar di OLT.
     - Klik **Lanjutkan**.
  4. **Tinjau & Eksekusi Perintah**:
     - Periksa pratinjau perintah CLI Huawei yang dihasilkan (contoh: `ont modify 3 15 ont-lineprofile-name "INTERNET-100M"`).
     - Ketikkan nama perangkat OLT pada kolom konfirmasi keamanan.
     - Klik **Eksekusi Konfigurasi**.
     - Sistem akan mengirimkan instruksi ke OLT Gateway, menerapkan perubahan tanpa memutus registrasi perangkat, dan mencatatnya ke log audit sistem.

