# 📝 Daily Work Report - Dedy S.N Putra (2026-09-21)

---

## 📅 Laporan Harian - 21 September 2026

---

## 🌿 Branch: `issue-175` — Implementasi ACS Monitoring V1, Keterkaitan Perangkat ACS ↔ Akun Pelanggan, & Riwayat Sinyal Optik (TR-069 / GenieACS)

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: Implementasi Sistem Monitoring ONT TR-069 Terisolasi (GenieACS), Pencocokan Akun Pelanggan Otomatis/Manual, dan Deteksi Degradasi Sinyal Optik Berbasis Riwayat Harian
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-175`, ahead of `origin/issue-175` sebanyak 2 commits: [`f18e0f3c`](file:///home/dhedhy/Project/Dekasimal-V2) dan [`5b9d111e`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [`f18e0f3c`](file:///home/dhedhy/Project/Dekasimal-V2) - save #175 - 21 September 2026, 19:56:56 WIB

- **Komponen yang Berubah**:
  - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md) — Memperbarui arsitektur monorepo dan tabel referensi cepat port default untuk menambahkan microservice `ACS Server` (GenieACS) yang beroperasi pada port `7547` (CWMP), `7557` (NBI), `7567` (FS), dan `7580` (UI).
  - [`backend/src/services/acs.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acs.service.js) — Menambahkan pembersihan status konflik tautan (`link_conflict: false`) saat admin menyelesaikan asosiasi secara manual (`linkAcsDeviceToAuthentication`), melepas relasi perangkat, mencatatkan serial nomor dari form akun pelanggan (`linkAuthenticationToSerial`), serta saat langganan pelanggan dihapus (`unlinkAcsDevicesByAuthentication`) guna mencegah akumulasi peringatan NOC harian selamanya.
  - [`backend/test/integration/acs.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/acs.service.test.js) — Menambahkan 4 pengujian integrasi baru untuk memvalidasi bahwa bendera `link_conflict` berhasil dibersihkan otomatis pada seluruh skenario intervensi manual dan penghapusan langganan (total 41 pengujian lulus).
  - [`cron-worker/src/jobs/processors/acsSignalCheck.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/processors/acsSignalCheck.js) — Merefaktor pencatatan log pada processor job BullMQ dengan Winston terstruktur (`logger.error`), menghapus dependensi `moment` yang tidak perlu, dan meniadakan log spam pada siklus pengecekan normal sesuai standar §4.12 AGENTS.md.
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — Membersihkan kunci lokalisasi yang tidak terpakai (`wifiChannel`, `wifiClients`, `showAllParameters`, `showLess`, `searchCustomer`, `noCustomerFound`) guna menjaga konsistensi kamus i18n.
- **Deskripsi Perubahan & Fungsi**:
  - Menyempurnakan mekanisme resolusi konflik relasi perangkat ONT-pelanggan, mematikan status peringatan palsu (*false alert*) di level database saat teknisi/admin telah mengambil tindakan administratif korektif.
  - Menyelaraskan modul microservice ACS dengan pedoman arsitektur monorepo, merapikan struktur observabilitas logger di `cron-worker`, serta mensterilkan kamus terjemahan dari entri usang.

#### [`5b9d111e`](file:///home/dhedhy/Project/Dekasimal-V2) - save #175 - 20 September 2026, 22:00:44 WIB (Dilanjutkan ke Hari Ini)

- **Komponen yang Berubah**:
  - `acs/` [NEW] — Modul microservice GenieACS terisolasi:
    - [`acs/index.js`](file:///home/dhedhy/Project/Dekasimal-V2/acs/index.js) [NEW] & [`acs/Dockerfile`](file:///home/dhedhy/Project/Dekasimal-V2/acs/Dockerfile) [NEW] — Inisialisasi proses server GenieACS dan kontainerisasi terisolasi.
    - [`acs/config/provisions/inform.js`](file:///home/dhedhy/Project/Dekasimal-V2/acs/config/provisions/inform.js) [NEW] — Skrip provisi GenieACS untuk mengekstrak parameter TR-069 saat ONT mengirim event *Inform* (identitas perangkat, WAN IP, PPPoE username, Optical Signal RX/TX power, PON temperature, WiFi SSID, dan connected hosts).
    - [`acs/ext/informWebhook.cjs`](file:///home/dhedhy/Project/Dekasimal-V2/acs/ext/informWebhook.cjs) [NEW] — Ekstensi GenieACS untuk mendorong payload data hasil Inform ke Backend melalui webhook internal (`POST /internal/acs/webhook/inform`).
    - [`acs/scripts/mockOnt.js`](file:///home/dhedhy/Project/Dekasimal-V2/acs/scripts/mockOnt.js) [NEW] — Skrip emulator/simulator pengujian ONT TR-069 untuk keperluan verifikasi lokal dan otomatisasi test.
    - [`acs/utils/logger.js`](file:///home/dhedhy/Project/Dekasimal-V2/acs/utils/logger.js) [NEW] — Konfigurasi logging Winston terstruktur untuk microservice ACS.
  - `backend/` — Penyimpanan data, agregasi sinyal, endpoint API, dan relasi pelanggan:
    - [`backend/src/models/acsDevice.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/acsDevice.model.js) [NEW] — Model Mongoose perangkat ACS mencakup serial number, pabrikan, tipe model, versi firmware, status WAN, username PPPoE, sinyal optik RX/TX, parameter TR-069 mentah, relasi `authentication` (`RadiusAuthentication`), sumber tautan (`auto`/`manual`), dan status konflik.
    - [`backend/src/models/acsSignalDaily.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/acsSignalDaily.model.js) [NEW] — Model Mongoose agregasi riwayat sinyal harian (nilai minimum, maksimum, rata-rata, dan jumlah sampel) per nomor seri ONT.
    - [`backend/src/services/acs.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acs.service.js) [NEW] — Layanan bisnis utama untuk memproses webhook inform, klasifikasi kualitas sinyal optik GPON/EPON, pencarian datatable, live query parameter via NBI GenieACS, pencocokan otomatis akun pelanggan lewat username PPPoE, penautan manual admin, pra-registrasi serial pelanggan, dan deteksi perebutan akun (*conflict detection*).
    - [`backend/src/services/acsSignal.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/acsSignal.service.js) [NEW] — Layanan pembanding baseline sinyal optik harian untuk mendeteksi degradasi (> 3 dBm dari rata-rata sebelumnya atau melampaui ambang batas) dan menyiarkan peringatan dini ke grup Telegram NOC.
    - [`backend/src/controllers/acs.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/acs.controller.js) [NEW] & [`backend/src/routes/acs.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/acs.route.js) [NEW] — Controller dan router REST API terproteksi JWT & hak akses (`acsDevice.read`, `acsDevice.write`) lengkap dengan spesifikasi OpenAPI Swagger (`/api/v1/acs-device/list`, `/stats`, `/detail/:serial_number`, `/link`, `/unlink`).
    - [`backend/src/controllers/internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internal.controller.js) & [`backend/src/routes/internal.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/internal.route.js) — Endpoint internal backend terproteksi API key untuk penerimaan webhook (`/internal/acs/webhook/inform`) dan pemanggilan scheduler degradasi sinyal (`/internal/cron/acs-signal-check`).
    - [`backend/src/services/radiusAuthentication.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusAuthentication.service.js) — Mengintegrasikan pembersihan relasi ONT saat dokumen autentikasi pelanggan dihapus.
    - [`backend/test/integration/`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/) (`acs.controller.test.js` [NEW], `acs.service.test.js` [NEW], `acsSignal.service.test.js` [NEW]) — Suite pengujian integrasi menyeluruh untuk endpoint API, alur webhook, pencocokan pelanggan, dan komparasi baseline sinyal.
  - `cron-worker/` — Penjadwal analisis sinyal berkala:
    - [`cron-worker/src/jobs/processors/acsSignalCheck.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/processors/acsSignalCheck.js) [NEW], [`scheduler.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/scheduler.js), [`worker.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/worker.js), & [`api.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/services/api.service.js) — Penjadwalan BullMQ untuk menjalankan pemeriksaan degradasi sinyal harian secara otomatis.
  - `docs/superpowers/specs/` [NEW] — Dokumen spesifikasi arsitektur & desain fitur:
    - [`docs/superpowers/specs/2026-09-20-acs-monitoring-v1-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-09-20-acs-monitoring-v1-design.md) [NEW] — Arsitektur pola Webhook + Cache untuk monitoring ribuan ONT via GenieACS.
    - [`docs/superpowers/specs/2026-09-21-acs-customer-linking-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-09-21-acs-customer-linking-design.md) [NEW] — Desain relasi perangkat ACS dengan `RadiusAuthentication` (otomatis vs manual, resolusi konflik).
    - [`docs/superpowers/specs/2026-09-21-acs-signal-history-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-09-21-acs-signal-history-design.md) [NEW] — Desain penyimpanan riwayat deret waktu sinyal harian dan deteksi degradasi optik preventif.
  - `frontend/` — Antarmuka pengguna dan visualisasi status perangkat:
    - [`frontend/src/app/pages/network/acsDevices/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/index.jsx) [NEW] — Halaman pemantauan utama perangkat ACS (`/networks/acs-devices`) dengan 4 kartu metrik statistik (Total ONT, Online, Offline, Sinyal Buruk), filter tab, dan pencarian cepat.
    - [`frontend/src/app/pages/network/acsDevices/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/schema/columns.jsx) [NEW] — Definisi kolom TanStack Table menampilkan Serial Number, Vendor & Model, Status Koneksi, IP & PPPoE User, Daya Terima Optik (RX Power), Pelanggan Tertaut (dengan avatar & tautan profil), serta waktu Inform terakhir.
    - [`frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx) [NEW] — Drawer rincian perangkat interaktif dengan indikator kualitas sinyal visual, parameter jaringan WAN, WiFi, tree tampilan parameter TR-069 mentah, serta tombol manajemen relasi pelanggan.
    - [`frontend/src/app/pages/network/acsDevices/components/AcsLinkCustomerModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/acsDevices/components/AcsLinkCustomerModal.jsx) [NEW] — Modal pencarian dan penautan manual akun pelanggan ke perangkat ONT.
    - [`frontend/src/components/shared/acs/OntDeviceCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/acs/OntDeviceCard.jsx) [NEW] — Komponen kartu pemantauan ONT terpadu yang disematkan langsung pada halaman detail pelanggan Broadband ([`broadband/detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/detail.jsx)) dan Radius Non-Customer ([`radiusNonCustomer/detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/radiusNonCustomer/detail.jsx)).
    - [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx) & [`status.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/status.js) — Sel badge status koneksi perangkat dan badge kualitas daya optik (`Sangat Baik`, `Baik`, `Waspada`, `Buruk`).
    - [`frontend/src/app/router/network/networkAcsRoute.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/network/networkAcsRoute.jsx) [NEW] & [`frontend/src/app/navigation/networks.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/networks.js) — Konfigurasi rute dan menu navigasi sidebar Jaringan.
- **Deskripsi Perubahan & Fungsi**:
  - Membangun kapabilitas monitoring perangkat CPE/ONT pelanggan berbasis TR-069 menggunakan arsitektur pola Webhook + Cache berkecepatan tinggi yang mampu menangani ribuan perangkat tanpa membebani server TR-069 GenieACS.
  - Menghubungkan perangkat ONT ke akun langganan pelanggan secara otomatis melalui pencocokan username PPPoE maupun secara manual oleh admin, sehingga teknisi NOC dapat langsung mengetahui pelanggan mana yang mengalami gangguan koneksi atau sinyal optik drop.
  - Menyediakan sistem deteksi dini degradasi sinyal harian otomatis yang memberi peringatan ke Telegram NOC sebelum pelanggan mengalami putus koneksi atau mengajukan keluhan.

---

## 🌿 Branch: `master` — Sanitasi & Validasi Nilai Default Speed Limit Kosong (0K/0K) pada Layanan Broadband PPP/Radius Authentication

### 📌 Informasi Issue

- **Nomor Issue**: #326
- **Judul Issue**: Perbaikan Nilai Default Batas Kecepatan (Speed Limit) Kosong atau 0K/0K agar Terhapus Otomatis ($unset) dari Dokumen Autentikasi Broadband
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch utama `master` via merge commit [`55e2adac`](file:///home/dhedhy/Project/Dekasimal-V2), tersinkronisasi penuh ke `origin/master` dan `origin/production`)

### 📅 Rincian Commit

#### [`55e2adac`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #326 (Merge to Master & Production) - 21 September 2026, 17:07:34 WIB

- **Komponen yang Berubah**:
  - Penggabungan resmi commit perbaikan bug issue `#326` ke dalam branch utama `master` dan lini produksi `production`.
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan seluruh deployment server dan basis kode produksi menerapkan penanganan sanitasi batas kecepatan broadband yang bersih, mencegah anomali konfigurasi profil pelanggan pada router MikroTik/RADIUS.

#### [`20353ef2`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #326 - 21 September 2026, 17:04:57 WIB

- **Komponen yang Berubah**:
  - [`backend/src/utils/validation-data.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/validation-data.js) — Menambahkan fungsi utilitas validasi `isSpeedLimitEmpty(speedLimit)` untuk mendeteksi apakah nilai batas kecepatan berupa string kosong, `'remove-data'`, format `'0K/0K'`, atau string burst dengan batas kecepatan 0.
  - [`backend/src/controllers/radiusAuthentication.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthentication.controller.js) & [`radiusAuthenticationNonCustomer.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthenticationNonCustomer.controller.js):
    - Pada `createAuthentication`: jika `speed_limit` kosong/0K, buang field `speed_limit`, `speed_option`, dan `use_burst` dari objek data sehingga tidak disimpan ke database MongoDB.
    - Pada `updateAuthentication` & `updateBatchAuthentication`: jika nilai limit kosong/0K, secara eksplisit daftarkan `speed_limit`, `speed_option`, dan `use_burst` ke dalam payload `unsetData` (`$unset: { speed_limit: 1, speed_option: 1, use_burst: 1 }`) dan hapus dari data pembaruan agar bersih dari database.
  - [`backend/test/unit/isSpeedLimitEmpty.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/isSpeedLimitEmpty.test.js) [NEW] & [`backend/test/integration/radiusAuthentication.speedLimit.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusAuthentication.speedLimit.test.js) [NEW] — Pengujian unit dan integrasi otomatis yang memvalidasi deteksi string limit serta memastikan field benar-benar terhapus (*unset*) di database MongoDB pada operasi buat, ubah per-item, dan ubah massal.
  - [`frontend/src/app/pages/services/broadband/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/create.jsx), [`detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/detail.jsx), [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/edit.jsx), dan [`editBatch.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/editBatch.jsx):
    - Membersihkan inisialisasi state form edit: bila nilai limit terbaca `0K/0K`, inisialisasi form sebagai opsi kosong (`initialSpeedOption = {}`, `initialUseBurst = false`).
    - Sinkronisasi efek kalkulasi `speedString`: hanya memformat string kecepatan jika kecepatan upload/download lebih besar dari 0.
    - Penanganan payload submit: jika `speedString` kosong, form secara tegas mengirimkan string kosong dan objek kosong agar controller backend melakukan `$unset`.
    - Tampilan UI: menyembunyikan komponen `SpeedBadge` bila string batas kecepatan kosong, menghilangkan tampilan badge `0K/0K` yang tidak informatif bagi pengguna.
- **Deskripsi Perubahan & Fungsi**:
  - Menyelesaikan akar permasalahan di mana form pembuatan dan pengeditan layanan broadband secara otomatis mengirimkan nilai `0K/0K` ke backend ketika admin tidak menentukan limitasi kecepatan (paket unlimited).
  - Menghilangkan bug di mana profil akun yang tidak memiliki limitasi kecepatan tetap menyimpan field `speed_limit` di database Mongoose, yang menyebabkan profil RADIUS menganggap pelanggan memiliki limit 0 kbps atau memunculkan badge limit kecepatan kosong pada kartu detail pelanggan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #175  | Implementasi Sistem Monitoring ONT TR-069 (GenieACS), Keterkaitan Akun Pelanggan, & Deteksi Degradasi Sinyal | Memperkenalkan modul pemantauan ribuan unit ONT pelanggan secara real-time via GenieACS, menautkan perangkat fisik ke data pelanggan Broadband secara otomatis/manual, memunculkan kartu status ONT di halaman profil pelanggan, dan mengaktifkan deteksi dini degradasi sinyal optik via peringatan bot Telegram NOC. |
| #326  | Sanitasi Nilai Default Speed Limit Kosong (0K/0K) Layanan Broadband | Menghilangkan bug penyimpanan nilai default `0K/0K` pada paket internet broadband tanpa limit, memastikan pembersihan field Mongoose secara bersih (`$unset`), dan menyembunyikan badge limit kecepatan tak valid pada UI. |

### Kemampuan Baru Pengguna/Admin

- **Pemantauan Terpadu Ribuan ONT**: Admin dan NOC kini dapat melihat status operasional seluruh ONT pelanggan (online/offline, IP WAN, PPPoE user, SSID WiFi, jumlah host terkoneksi, suhu PON, dan redaman sinyal RX/TX) secara tersentralisasi di `/networks/acs-devices`.
- **Identifikasi Pemilik Perangkat Instan**: Sinyal buruk atau ONT bermasalah tidak lagi bersifat anonim; sistem langsung menampilkan nama dan akun pelanggan yang terdampak berkat pencocokan otomatis PPPoE.
- **Kartu ONT Terintegrasi pada Halaman Pelanggan**: Teknisi yang membuka rincian langganan Broadband pelanggan ([`broadband/detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadband/detail.jsx)) kini langsung disajikan kartu status perangkat ONT fisik di rumah pelanggan lengkap dengan indikator mutu sinyal optik tanpa perlu berpindah ke menu lain.
- **Penautan Manual & Pra-Registrasi**: Admin dapat menautkan nomor seri ONT ke akun pelanggan secara manual atau mencatatkan nomor seri terlebih dahulu sebelum teknisi memasang ONT di lapangan (pra-registrasi).
- **Peringatan Degradasi Optik Preventif**: Tim NOC akan menerima notifikasi otomatis di Telegram saat ada ONT yang mengalami penurunan sinyal drastis (> 3 dBm dari rata-rata harian sebelumnya), memungkinkan tindakan perbaikan preventif sebelum pelanggan komplain.

### Bug Fix / Solusi Masalah

- **Resolusi Penanda Konflik ONT**: Penanda `link_conflict` pada perangkat ACS kini dibersihkan tuntas saat admin melakukan penautan manual, pelepasan relasi, atau penghapusan langganan, mencegah peringatan harian palsu terus-menerus.
- **Penyimpanan Speed Limit `0K/0K` Terselesaikan**: Paket broadband tanpa batasan kecepatan tidak lagi menyimpan `0K/0K` atau field limit usang di database MongoDB; controller backend secara atomik mengeksekusi operasi `$unset`.
- **Penghilangan Tampilan Badge Speed Palsu**: Antarmuka form dan detail broadband tidak lagi menampilkan badge kecepatan `0K/0K` yang membingungkan teknisi saat paket tidak memiliki kuota/limit kecepatan.

### Menu/Fitur Baru

- **Menu Jaringan Baru**: Sub-menu `Perangkat ACS` (`/networks/acs-devices`) di bawah menu navigasi Jaringan (*Networks*), dilengkapi 4 kartu ringkasan status metrik, penyaringan status sinyal, dan tabel interaktif TanStack Table.
- **Drawer Detail Perangkat ACS**: Drawer rincian perangkat dengan grafik level daya optik, informasi koneksi WAN/WiFi, panel parameter TR-069 lengkap, dan utilitas penautan pelanggan.
- **Modal Hubungkan Pelanggan**: Modal pencarian langganan pelanggan untuk menetapkan asosiasi kepemilikan perangkat ONT secara manual.
- **Komponen Kartu ONT Pelanggan**: Komponen modular `OntDeviceCard` pada profil langganan broadband dan radius non-customer.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  Sistem **ACS Monitoring V1** bekerja melalui komunikasi standar TR-069 (CWMP) antara modem/ONT di sisi pelanggan dan server GenieACS. Setiap kali ONT mengirimkan paket *Inform* (berkala, saat reboot, atau saat terjadi perubahan nilai WAN), skrip provisi GenieACS mengekstrak data identitas, status koneksi WAN, username PPPoE, parameter WiFi, dan redaman sinyal optik (RX/TX Power), lalu meneruskannya secara instan melalui webhook HTTP ke backend DEKASIMAL V2. Backend mencatat data ke database lokal dan secara cerdas mencocokkan perangkat ke akun pelanggan Broadband yang aktif. Selain itu, cron worker mengevaluasi rata-rata sinyal harian untuk mendeteksi penurunan kualitas kabel/konektor optik secara dini.

- **Langkah Penggunaan (Tutorial)**:
  1. **Melihat Daftar & Status ONT**:
     - Buka menu navigasi sebelah kiri, pilih **Jaringan** (*Networks*) → klik **Perangkat ACS** (`/networks/acs-devices`).
     - Pantau ringkasan metrik di bagian atas: total perangkat terdaftar, perangkat online, perangkat offline, dan perangkat dengan kualitas sinyal buruk (*warning/bad*).
     - Gunakan kolom pencarian untuk mencari nomor seri ONT, model perangkat, alamat IP, atau nama pelanggan tertaut.
  2. **Melihat Rincian Parameter & Sinyal Optik Perangkat**:
     - Pada baris perangkat yang diinginkan, klik ikon tombol aksi **Detail**.
     - Drawer rincian akan terbuka menampilkan kartu status konektivitas, identitas perangkat, parameter WAN/PPPoE, SSID WiFi & host terhubung, serta kartu indikator daya optik (RX Power dalam dBm beserta klasifikasi mutunya).
     - Gulir ke bawah untuk meninjau seluruh parameter TR-069 mentah yang dilaporkan oleh modem.
  3. **Menautkan atau Mengubah Relasi Pelanggan Secara Manual**:
     - Pada drawer detail perangkat, klik tombol **Hubungkan ke Pelanggan** (atau **Ubah Relasi** jika sudah tertaut).
     - Cari akun pelanggan berdasarkan nama atau username PPPoE, lalu klik tombol simpan. Relasi akan terkunci sebagai tipe `manual` dan tidak akan tertimpa oleh sinkronisasi otomatis.
  4. **Memeriksa Status Fisik ONT Langsung dari Halaman Pelanggan**:
     - Buka menu **Layanan** → **Broadband** → buka salah satu detail pelanggan.
     - Di bagian informasi teknis langganan, periksa kartu **Perangkat ONT Terhubung** yang menampilkan kondisi online, nomor seri modem, dan kualitas redaman sinyal secara langsung tanpa perlu berpindah halaman.
