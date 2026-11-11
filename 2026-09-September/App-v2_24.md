# 📝 Daily Work Report - Dedy S.N Putra (24 September 2026)

---

## 📅 Laporan Harian - 24 September 2026

---

## 🌿 Branch: `master` — Issue #304: Sistem Kartu Informasi Mitra, Manajemen Dokumen Legalitas & Pratinjau Dokumen In-App

### 📌 Informasi Issue

- **Nomor Issue**: #304
- **Judul Issue**: Sistem Kartu Informasi Mitra, Manajemen Dokumen Legalitas Mitra, Publikasi Verifikasi Kartu & Pratinjau Dokumen In-App
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Kamis, 24 September 2026, 21:04:43 WIB via commit `a1ac3ece918d85fdba46c43e2c71c3146e17608f`)

### 📅 Rincian Commit

#### [a1ac3ece] - resolve #304 - Kamis, 24 September 2026, 21:04:43 WIB

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/files.controller.js`
  - `backend/src/controllers/partner.controller.js`
  - `backend/src/controllers/partnerApiPartner.controller.js`
  - `backend/src/controllers/partnerCard.controller.js` [NEW]
  - `backend/src/controllers/partnerDocument.controller.js` [NEW]
  - `backend/src/controllers/publicPartnerCard.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/partnerCard.model.js` [NEW]
  - `backend/src/models/partnerDocument.model.js` [NEW]
  - `backend/src/routes/files.route.js`
  - `backend/src/routes/partnerCard.route.js` [NEW]
  - `backend/src/routes/partnerDocument.route.js` [NEW]
  - `backend/src/routes/public.route.js`
  - `backend/src/services/partner.service.js`
  - `backend/src/services/partnerCard.service.js` [NEW]
  - `backend/src/services/partnerDocument.service.js` [NEW]
  - `backend/src/utils/migrate-partner-documents.js` [NEW]
  - `backend/src/utils/random-image-pexels.js`
  - `backend/src/utils/validation-data.js`
  - `backend/test/helpers/factories.js`
  - `backend/test/integration/partnerApiPartner.profile.test.js`
  - `backend/test/integration/partnerApiPartner.uploadDocuments.test.js`
  - `backend/test/integration/partnerCard.invalidId.test.js` [NEW]
  - `backend/test/integration/publicPartnerCard.test.js` [NEW]
  - `frontend/src/app/navigation/archive.js`
  - `frontend/src/app/navigation/users.js`
  - `frontend/src/app/pages/archive/ArchiveIndexRedirect.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/PartnerCardDocumentPicker.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/PartnerCardDrawer.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/PartnerCardExportActions.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/PartnerCardModal.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/PartnerCardPreview.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/PartnerCardVisibilityFields.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/PrintBatchDrawer.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/PrintSheet.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/detail.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/index.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/schema/cardSchema.js` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/public/PublicPartnerCard.jsx` [NEW]
  - `frontend/src/app/pages/public/partnerCard/PartnerDocumentBrowser.jsx` [NEW]
  - `frontend/src/app/pages/public/partnerCard/PartnerDocumentPreview.jsx` [NEW]
  - `frontend/src/app/pages/public/partnerCard/PartnerDocumentThumbnail.jsx` [NEW]
  - `frontend/src/app/pages/public/partnerCard/documentUtils.js` [NEW]
  - `frontend/src/app/pages/users/partner/profile.jsx`
  - `frontend/src/app/pages/users/partnerDocument/EditDocumentDrawer.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/EditDocumentModal.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/PartnerDocumentPreviewModal.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/UploadDocumentDrawer.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/UploadDocumentModal.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/index.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/schema/documentSchema.js` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/router/public.jsx`
  - `frontend/src/app/router/users/partnerDocumentRoute.jsx` [NEW]
  - `frontend/src/components/shared/ConfirmModal.jsx`
  - `frontend/src/components/shared/partnerCard/PartnerCardProfileLink.jsx` [NEW]
  - `frontend/src/components/shared/partnerCard/PartnerCardStatusBadge.jsx` [NEW]
  - `frontend/src/components/shared/partnerCard/PartnerCardSticker.jsx` [NEW]
  - `frontend/src/components/shared/partnerCard/constants.js` [NEW]
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Backend Core: Arsitektur Model, Controller, Service & Routing**:
    - **Model Data Kartu Mitra (`models/partnerCard.model.js`)**: Membangun skema penyimpanan data kartu informasi mitra dengan dukungan field konfigurasi visibilitas (tampilkan/sembunyikan nama, telepon, alamat, NPWP, NIB), daftar lampiran dokumen terpilih, status kartu (`active`/`inactive`), hash/slug kode verifikasi unik, dan counter statistik pemindaian/unduhan.
    - **Model Data Dokumen Legalitas Mitra (`models/partnerDocument.model.js`)**: Membangun model terpisah untuk mengelola dokumen mitra (KTP, NPWP, NIB, PKS, dll.) lengkap dengan metadata file (nama file, mimetype, ukuran berkas, label dokumen) serta relasi referensi ke entitas `Partner`.
    - **Layanan Bisnis Kartu Mitra (`services/partnerCard.service.js`)**: Logika bisnis lengkap untuk pembuatan kartu otomatis/manual, pembaruan preferensi visibilitas data, pemilihan dokumen yang disertakan pada kartu, aktivasi/deaktivasi kartu, serta pelacakan log kunjungan publik. Mendukung filter pencarian case-insensitive regex pada datatable.
    - **Layanan Bisnis Dokumen Mitra (`services/partnerDocument.service.js`)**: Menangani pengunggahan berkas dokumen ke penyimpanan terenkripsi/MinIO, pembaruan label, validasi jenis berkas yang diizinkan (PDF, PNG, JPG/JPEG), dan penghapusan aman. Dilengkapi filter pencarian fleksibel berdasarkan ID mitra maupun nama mitra.
    - **API Publik Kartu Mitra (`controllers/publicPartnerCard.controller.js` & `routes/public.route.js`)**: Menyediakan endpoint publik tanpa autentikasi JWT (`/public/partner-card/:code`) untuk verifikasi keaslian kartu mitra oleh pihak eksternal/klien, dengan penyaringan data ketat agar informasi rahasia tidak bocor.
    - **Script Migrasi Dokumen Eksisting (`utils/migrate-partner-documents.js`)**: Utilitas otomasi migrasi data untuk mengekstrak berkas dokumen yang sebelumnya tersimpan pada dokumen induk `Partner` ke koleksi terpisah `partner_documents`.
    - **Sistem Hak Akses & Privilese (`privilege.json` & `privilegeDictionary.json`)**: Menambahkan izin akses terperinci: `partnerCard.list`, `partnerCard.read`, `partnerCard.create`, `partnerCard.update`, `partnerCard.delete`, `partnerDocument.list`, `partnerDocument.read`, `partnerDocument.create`, `partnerDocument.update`, `partnerDocument.delete`.
    - **Pengujian Integrasi Komprehensif**: Menambahkan pengujian integrasi otomatis untuk verifikasi ID tidak valid (`partnerCard.invalidId.test.js`) dan skenario pengaksesan publik (`publicPartnerCard.test.js`).
  - **Frontend UI: Modul Arsip Kartu Informasi Mitra (`/archive/partner-card`)**:
    - **Halaman Utama & Datatables (`index.jsx`, `columns.jsx`, `cardSchema.js`)**: Menampilkan daftar seluruh kartu mitra yang terdaftar, status keaktifan, tautan verifikasi publik, jumlah dokumen tertaut, dan aksi cepat.
    - **Halaman Detail Kartu Mitra (`detail.jsx`)**: Tampilan komprehensif untuk mengelola pengaturan kartu, meninjau pratinjau kartu secara real-time (`PartnerCardPreview.jsx`), mengatur visibilitas field (`PartnerCardVisibilityFields.jsx`), dan memilih dokumen mitra yang ditampilkan di kartu (`PartnerCardDocumentPicker.jsx`).
    - **Fitur Pencetakan Stiker & Cetak Massal (`PrintSheet.jsx`, `PrintBatchDrawer.jsx`, `PartnerCardSticker.jsx`)**: Memungkinkan admin mencetak kartu fisik atau stiker QR-code secara individual maupun secara massal (batch print) dengan format tata letak cetak khusus (`@media print`).
    - **Komponen Modal & Drawer Interaktif**: Menyediakan modal pratinjau ringkasan (`PartnerCardModal.jsx`) serta drawer backward-compatibility (`PartnerCardDrawer.jsx`).
  - **Frontend UI: Modul Dokumen Mitra (`/users/partner-document`)**:
    - **Tabel Dokumen Mitra (`index.jsx`, `columns.jsx`, `documentSchema.js`)**: Antarmuka visual untuk melihat seluruh berkas dokumen legalitas mitra dengan informasi label, nama berkas, ukuran, dan tanggal unggah.
    - **Refactoring Form ke Modal Dialog Terpusat**:
      - `UploadDocumentModal.jsx`: Modal unggah dokumen baru dengan drag-and-drop, pemilihan mitra, dan penyesuaian label berkas.
      - `EditDocumentModal.jsx`: Modal pengeditan label atau pergantian berkas dokumen yang sudah ada.
      - Berkas drawer lama (`UploadDocumentDrawer.jsx`, `EditDocumentDrawer.jsx`) dipertahankan sebagai re-export alias untuk stabilitas sistem.
    - **Modal Pratinjau Dokumen In-App (`PartnerDocumentPreviewModal.jsx`)**: Pratinjau instan dokumen PDF dan gambar (JPG/PNG) di dalam modal aplikasi menggunakan otentikasi Bearer token via endpoint Axios blob tanpa perlu membuka tab baru peramban.
  - **Frontend UI: Halaman Publik Verifikasi Kartu Mitra (`/public/partner-card/:code`)**:
    - Halaman antarmuka publik responsif yang dapat diakses oleh publik/klien saat memindai kode QR kartu mitra.
    - Memuat komponen penjelajah dokumen publik (`PartnerDocumentBrowser.jsx`), thumbnail interaktif (`PartnerDocumentThumbnail.jsx`), dan pratinjau dokumen publik yang aman (`PartnerDocumentPreview.jsx`).
  - **Penguatan Proteksi Hak Akses (Privilege)**:
    - Seluruh tombol aksi, navigasi menu, dan pembuka modal divalidasi dengan `useHasPrivilege(...)` selaras dengan aturan backend.

---

## 🌿 Branch: `issue-175` — Issue #175: TR-069 GenieACS Integration & ONT/CPE Device Monitoring

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: TR-069 GenieACS Integration — ACS Device Monitoring, Customer Linking, Signal History & Device Configuration
- **Status Branch**: `Belum di-merge` (Branch `origin/issue-175` / `issue-175`, commit `70754560`)

### 📅 Rincian Commit

#### [70754560] - resolve #175 - Minggu, 20 September 2026, 22:00:44 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `acs/.env.example` [NEW]
  - `acs/.gitignore` [NEW]
  - `acs/Dockerfile` [NEW]
  - `acs/config/provisions/inform.js` [NEW]
  - `acs/ext/informWebhook.cjs` [NEW]
  - `acs/index.js` [NEW]
  - `acs/package-lock.json` [NEW]
  - `acs/package.json` [NEW]
  - `acs/scripts/mockOnt.js` [NEW]
  - `acs/test/fixtures/vsol-v2802dac-multi-ip.json` [NEW]
  - `acs/test/fixtures/vsol-xpon-1ge-wifi-dual-pppoe.json` [NEW]
  - `acs/test/inform.provision.test.js` [NEW]
  - `acs/test/sandbox.js` [NEW]
  - `acs/utils/logger.js` [NEW]
  - `backend/.env.example`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/acs.controller.js` [NEW]
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/acsDevice.model.js` [NEW]
  - `backend/src/models/acsSignalDaily.model.js` [NEW]
  - `backend/src/routes/acs.route.js` [NEW]
  - `backend/src/routes/internal.route.js`
  - `backend/src/services/acs.service.js` [NEW]
  - `backend/src/services/acsConfig.service.js` [NEW]
  - `backend/src/services/acsProvision.service.js` [NEW]
  - `backend/src/services/acsSignal.service.js` [NEW]
  - `backend/src/services/cronSettings.service.js`
  - `backend/src/services/option.service.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/src/utils/generate-permissions.js`
  - `backend/test/integration/acs.controller.test.js` [NEW]
  - `backend/test/integration/acs.service.test.js` [NEW]
  - `backend/test/integration/acsConfig.service.test.js` [NEW]
  - `backend/test/integration/acsProvision.service.test.js` [NEW]
  - `backend/test/integration/acsSignal.service.test.js` [NEW]
  - `backend/test/integration/syslogAiAnalysis.service.test.js`
  - `backend/test/unit/cronSettings.service.test.js` [NEW]
  - `cron-worker/src/jobs/processors/acsSignalCheck.js` [NEW]
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `docs/superpowers/specs/2026-09-20-acs-monitoring-v1-design.md` [NEW]
  - `docs/superpowers/specs/2026-09-21-acs-customer-linking-design.md` [NEW]
  - `docs/superpowers/specs/2026-09-21-acs-signal-history-design.md` [NEW]
  - `frontend/src/app/layouts/Root.jsx`
  - `frontend/src/app/navigation/networks.js`
  - `frontend/src/app/pages/network/acsDevices/components/AcsDeviceConfigModal.jsx` [NEW]
  - `frontend/src/app/pages/network/acsDevices/components/AcsDeviceDetailDrawer.jsx` [NEW]
  - `frontend/src/app/pages/network/acsDevices/components/AcsLinkCustomerModal.jsx` [NEW]
  - `frontend/src/app/pages/network/acsDevices/components/AcsModelCoverageModal.jsx` [NEW]
  - `frontend/src/app/pages/network/acsDevices/index.jsx` [NEW]
  - `frontend/src/app/pages/network/acsDevices/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/network/radiusNonCustomer/detail.jsx`
  - `frontend/src/app/pages/services/broadband/detail.jsx`
  - `frontend/src/app/pages/settings/schema/systemSchema.js`
  - `frontend/src/app/pages/settings/sections/System.jsx`
  - `frontend/src/app/router/network/networkAcsRoute.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/acs/OntDeviceCard.jsx` [NEW]
  - `frontend/src/components/shared/form/TagListInput.jsx` [NEW]
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/features/acsDeviceSlice.js` [NEW]
  - `frontend/src/hooks/useTicketBadge.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/store.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Microservice ACS TR-069 (`/acs`)**:
    - Membangun microservice terisolasi berbasis Node.js yang bertindak sebagai jembatan ekstensi GenieACS untuk menangkap berkas *inform webhook* (`informWebhook.cjs`) dan skrip *provisioning* (`config/provisions/inform.js`).
    - Menyediakan *mock ONT script* (`mockOnt.js`) dan data uji fixture untuk pengujian multi-vendor (V-SOL V2802DAC, XPON 1GE WiFi, dll.).
    - Containerization mandiri dengan `Dockerfile` dan konfigurasi environment terisolasi.
  - **Backend Core ACS (Controller, Service, Models, Routes)**:
    - Model data `AcsDevice` (`models/acsDevice.model.js`) untuk menyimpan metadata ONT pelanggan (serial number, manufacturer, model, OUI, MAC address, versi firmware/hardware, status online/offline, uptime, IP address WAN/LAN, dan parameter optik Rx/Tx power).
    - Model data `AcsSignalDaily` (`models/acsSignalDaily.model.js`) untuk merekam riwayat kualitas sinyal optik harian (Rx/Tx dBm, voltase, temperatur, bias current).
    - Membangun controller dan routing REST API lengkap di `backend/src/controllers/acs.controller.js` dan `backend/src/routes/acs.route.js` untuk daftar perangkat, detail teknis, konfigurasi parameter, reboot ONT, factory reset, dan penautan akun pelanggan (`RadiusAuthentication`).
    - Mengimplementasikan service bisnis:
      - `acs.service.js`: agregasi datatable perangkat, sinkronisasi webhook GenieACS, manajemen status online/offline.
      - `acsConfig.service.js`: manipulasi parameter TR-069 jarak jauh (SSID WiFi, password WPA, WAN PPPoE credentials, LAN IP).
      - `acsProvision.service.js`: otomasi alur provisi perangkat baru yang terhubung ke jaringan.
      - `acsSignal.service.js`: analisis tren sinyal optik dan agregasi data historis.
    - Menambahkan rute internal di `backend/src/routes/internal.route.js` untuk menerima webhook dari service ACS secara aman dengan API key.
  - **Integrasi Cron Worker (`/cron-worker`)**:
    - Menambahkan scheduled job `acsSignalCheck` (`cron-worker/src/jobs/processors/acsSignalCheck.js`) yang berjalan secara periodik untuk mengecek degradasi sinyal optik ONT dan mencatat peringatan bila Rx power turun di bawah ambang batas toleransi (*red-zone optical signal*).
  - **Antarmuka Frontend ACS Monitoring (`frontend/src/app/pages/network/acsDevices`)**:
    - Membuat halaman utama `index.jsx` dengan Datatables terintegrasi untuk memonitor ribuan perangkat ONT/CPE.
    - `AcsDeviceDetailDrawer.jsx`: Drawer komprehensif untuk memeriksa status real-time ONT, statistik optik, informasi WAN, port LAN, perangkat WiFi terhubung, dan grafik tren sinyal optik harian.
    - `AcsDeviceConfigModal.jsx`: Modal konfigurasi parameter TR-069 jarak jauh untuk mengubah konfigurasi nirkabel (SSID, password, channel) dan konfigurasi koneksi WAN tanpa perlu mengunjungi lokasi pelanggan.
    - `AcsLinkCustomerModal.jsx`: Modal untuk menautkan perangkat ONT dengan akun pelanggan (`RadiusAuthentication` broadband) secara cepat.
    - `AcsModelCoverageModal.jsx`: Modal ringkasan cakupan model dan vendor ONT yang aktif di jaringan.
    - Komponen visual kartu status perangkat `OntDeviceCard.jsx` yang dapat disematkan di halaman detail pelanggan broadband.
  - **Sistem Privilege & Konfigurasi Navigasi**:
    - Menambahkan hak akses baru: `acsDevice.read`, `acsDevice.update`, `acsDevice.delete`, `acsDevice.list`, `acsDevice.linkCustomer`, `acsDevice.configure` pada `privilege.json` dan menu sidebar Network (`frontend/src/app/navigation/networks.js`).
    - Menambahkan pengaturan endpoint GenieACS dan kredensial NBI di konfigurasi sistem (`System.jsx`).
  - **Pengujian Integrasi Komprehensif**:
    - Menulis rangkaian pengujian otomatis integrasi backend (`acs.controller.test.js`, `acs.service.test.js`, `acsConfig.service.test.js`, `acsProvision.service.test.js`, `acsSignal.service.test.js`, `cronSettings.service.test.js`).
    - Menulis berkas dokumen spesifikasi desain arsitektur di `docs/superpowers/specs/`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #304  | Sistem Kartu Informasi Mitra, Manajemen Dokumen & Pratinjau In-App | Standarisasi kartu mitra, publikasi verifikasi QR, pratinjau dokumen PDF/gambar in-app tanpa reload/tab baru, serta pencetakan batch stiker kartu |
| #175  | TR-069 GenieACS Integration & ONT/CPE Monitoring | Pemantauan ribuan ONT pelanggan secara real-time, pencatatan tren sinyal optik harian, konfigurasi nirkabel jarak jauh, dan penautan akun pelanggan otomatis |

### Kemampuan Baru Pengguna/Admin

- **Verifikasi Publik Kartu Mitra via QR/Tautan Unik**: Klien, vendor, atau publik dapat memverifikasi identitas mitra resmi melalui halaman publik khusus (`/public/partner-card/:code`) lengkap dengan dokumen legalitas yang diizinkan untuk ditampilkan.
- **Pencetakan Kartu & Stiker Massal (Batch Print)**: Admin dapat mencetak stiker atau kartu mitra baik perorangan maupun secara massal dalam format layout kertas A4/kustom yang rapi dan siap cetak.
- **Pratinjau Dokumen Mitra In-App Tanpa Tab Baru**: Dokumen legalitas mitra (PDF, JPG, PNG) dapat langsung ditinjau di dalam modal aplikasi yang aman dengan kontrol Bearer token, zoom visual, dan tombol unduh langsung.
- **Formulir Terpusat & Fokus (Modal Dialog)**: Pengunggahan dan pengubahan dokumen mitra telah dimodernisasi menggunakan modal dialog terpusat berukuran proporsional yang memudahkan pengoperasian di berbagai ukuran layar.
- **Monitoring ONT/CPE Jarak Jauh (TR-069)**: Tim teknis NOC/Helpdesk dapat memantau status operasional ONT pelanggan secara real-time (status online/offline, redaman Rx/Tx, voltase, temperatur, dan versi firmware).
- **Konfigurasi Remote CPE & Reboot**: Admin dapat mengubah SSID WiFi, password WPA, atau melakukan *reboot* ONT pelanggan langsung dari dashboard tanpa harus datang ke lokasi pelanggan.
- **Penautan Cepat Akun Pelanggan**: Admin dapat menautkan serial number ONT yang terdeteksi ke akun langganan broadband pelanggan (`RadiusAuthentication`).

### Bug Fix / Solusi Masalah

- **Penyelesaian Masalah Filter Dokumen Mitra**: Filter mitra sebelumnya gagal jika dicari dengan nama teks biasa karena perbedaan struktur relasi `ObjectId`. Masalah ini diselesaikan dengan resolusi nama dan ID via regex di backend service.
- **Keamanan Dokumen Mitra Terproteksi Penuh**: Berkas dokumen mitra tidak lagi diekspos melalui tautan langsung tanpa autentikasi; seluruh akses melewati endpoint otentikasi blob yang aman.
- **Eliminasi Redundansi UI Drawer**: Drawer samping yang sempit untuk pratinjau dokumen telah digantikan dengan modal dialog yang luas dan responsif.
- **Pencegahan Human-Error Saat Cetak**: Tata letak cetak lembar stiker kartu mitra distandarisasi menggunakan CSS `@media print` sehingga elemen antarmuka aplikasi tidak ikut tercetak.

### Menu/Fitur Baru

- **Menu Arsip Kartu Informasi Mitra (`/archive/partner-card`)**: Halaman arsip untuk mengelola seluruh data kartu identitas mitra resmi, status aktivasi, dan preferensi visibilitas data publik.
- **Menu Dokumen Mitra (`/users/partner-document`)**: Halaman manajemen dokumen legalitas mitra dengan modal upload, edit, dan pratinjau in-app.
- **Halaman Publik Kartu Mitra (`/public/partner-card/:code`)**: Halaman publik verifikasi kartu mitra dengan thumbnail dan viewer dokumen legalitas terlampir.
- **Menu ACS Devices (`/network/acs`)**: Halaman inventaris perangkat ONT/CPE berbasis TR-069 di sidebar jaringan.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Pengelolaan & Verifikasi Publik Kartu Informasi Mitra (Partner Card)

- **Penjelasan Fitur**: Fitur Kartu Informasi Mitra memungkinkan perusahaan menerbitkan kartu identitas digital dan fisik bagi mitra kerja resmi. Setiap kartu memiliki kode unik dan QR Code yang mengarah ke halaman verifikasi publik. Admin dapat mengatur field apa saja yang boleh dilihat publik (misal nomor telepon, alamat, NPWP) serta memilih dokumen legalitas apa saja yang dilampirkan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Arsip → Kartu Informasi Mitra** (`/archive/partner-card`) pada sidebar navigasi.
  2. Klik baris mitra yang ingin dikelola untuk membuka halaman **Detail Kartu Mitra**.
  3. Pada tab **Pengaturan Kartu**, tentukan visibilitas informasi (centang field yang ingin ditampilkan pada halaman publik).
  4. Pada bagian **Dokumen Terlampir**, pilih dokumen mitra (seperti KTP, NPWP, Surat Tugas) yang diizinkan untuk diakses oleh publik.
  5. Simpan pengaturan. Klik tombol **Lihat Halaman Publik** untuk meninjau tampilan halaman verifikasi kartu sebagaimana dilihat oleh pihak luar.
  6. Untuk mencetak kartu, klik tombol **Cetak Stiker/Kartu**, atau gunakan fitur **Cetak Massal (Batch Print)** dari halaman tabel utama untuk mencetak beberapa kartu mitra sekaligus dalam satu lembar kertas.

---

### 2. Pengunggahan & Pratinjau Dokumen Legalitas Mitra (In-App Preview)

- **Penjelasan Fitur**: Fitur Dokumen Mitra menyediakan repositori berkas legalitas resmi mitra kerja yang aman dan mudah diakses. Dilengkapi dengan pratinjau dokumen in-app, pengguna dapat langsung memeriksa berkas PDF atau gambar tanpa harus mengunduh atau membuka tab baru peramban.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Pengguna → Dokumen Mitra** (`/users/partner-document`).
  2. Untuk menambahkan dokumen baru, klik tombol **+ Unggah Dokumen** di pojok kanan atas tabel.
  3. Pada modal yang muncul, pilih nama Mitra yang relevan, pilih berkas dokumen (PDF, JPG, PNG), beri label dokumen (misal: *Surat Perjanjian Kerjasama 2026*), lalu klik **Simpan**.
  4. Untuk meninjau isi berkas, klik teks **Label Dokumen** pada baris tabel bersangkutan.
  5. Modal pratinjau in-app akan terbuka menampilkan isi berkas secara instan. Anda dapat membaca dokumen, memperbesar gambar, atau mengunduh berkas dengan mengeklik tombol **Unduh Dokumen** di kanan atas modal.
  6. Jika ingin mengubah label dokumen atau memperbarui berkas yang salah unggah, klik tombol **Edit** pada kolom aksi tabel, lakukan pembaruan pada modal edit, lalu klik **Simpan Perubahan**.

---

### 3. Pemantauan & Konfigurasi ONT Pelanggan via TR-069 (ACS Devices)

- **Penjelasan Fitur**: Modul ACS Devices menghubungkan Dekasimal V2 dengan GenieACS untuk memantau status operasional ONT pelanggan secara real-time, mendeteksi gangguan sinyal optik fiber, serta mengeksekusi konfigurasi parameter WiFi dan WAN dari jarak jauh.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Network → ACS Devices** (`/network/acs`) pada navigasi sidebar.
  2. Pantau daftar ONT yang terhubung, serial number, status koneksi (*online/offline*), serta nilai penerimaan daya optik (*Rx Optical Power*).
  3. Klik salah satu perangkat untuk membuka **AcsDeviceDetailDrawer** guna memeriksa status port LAN, detail WAN PPPoE, dan grafik historis degradasi sinyal optik harian.
  4. Untuk menautkan ONT ke akun broadband pelanggan, klik tombol **Tautkan Pelanggan**, cari ID atau nama pelanggan, lalu simpan penautan.
  5. Untuk mengubah konfigurasi nirkabel atau merestart perangkat, klik tombol **Konfigurasi**:
     - Masukkan nama SSID baru dan password WPA yang diinginkan.
     - Klik **Kirim Konfigurasi** untuk mengirimkan perintah TR-069 RPC `SetParameterValues`.
     - Bila perangkat membutuhkan restart, klik tombol **Reboot Perangkat** untuk mengirimkan RPC `Reboot`.
