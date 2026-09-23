# 📝 Daily Work Report - Dedy S.N Putra (23 September 2026)

---

## 📅 Laporan Harian - 23 September 2026

---

## 🌿 Branch: `issue-304` — Issue #304: Refactoring UX Modal, Preview In-App & Peningkatan Tampilan Kartu dan Dokumen Mitra

### 📌 Informasi Issue

- **Nomor Issue**: #304
- **Judul Issue**: Refactoring UX Modal, Pratinjau In-App, Perbaikan Filtering, dan Peningkatan Visual Tabel Kartu Informasi Mitra & Dokumen Mitra
- **Status Branch**: `Belum di-merge` (Perubahan aktif dalam proses pengembangan di working tree, Rabu, 23 September 2026)

### 📅 Rincian Commit

#### [Uncommitted] - Refactoring Modal, In-App Document Preview, Filter & Visual Polish - Rabu, 23 September 2026, 11:54:15 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/partnerCard.service.js`
  - `backend/src/services/partnerDocument.service.js`
  - `frontend/src/app/navigation/users.js`
  - `frontend/src/app/pages/archive/partnerCard/PartnerCardDrawer.jsx`
  - `frontend/src/app/pages/archive/partnerCard/PartnerCardModal.jsx` [NEW]
  - `frontend/src/app/pages/archive/partnerCard/detail.jsx`
  - `frontend/src/app/pages/archive/partnerCard/index.jsx`
  - `frontend/src/app/pages/archive/partnerCard/schema/columns.jsx`
  - `frontend/src/app/pages/users/partnerDocument/EditDocumentDrawer.jsx`
  - `frontend/src/app/pages/users/partnerDocument/EditDocumentModal.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/PartnerDocumentPreviewModal.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/UploadDocumentDrawer.jsx`
  - `frontend/src/app/pages/users/partnerDocument/UploadDocumentModal.jsx` [NEW]
  - `frontend/src/app/pages/users/partnerDocument/index.jsx`
  - `frontend/src/app/pages/users/partnerDocument/schema/columns.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Refactoring Drawer ke Modal Dialog Terpusat**:
    - Mengonversi `UploadDocumentDrawer` menjadi `UploadDocumentModal` (`UploadDocumentModal.jsx`) dan `EditDocumentDrawer` menjadi `EditDocumentModal` (`EditDocumentModal.jsx`) untuk memberikan ruang fokus kerja yang lebih proporsional di tengah layar dengan layout Headless UI `DialogPanel`. Berkas lama tetap diekspor sebagai alias/re-export guna menjamin backward-compatibility.
    - Mengonversi `PartnerCardDrawer` menjadi `PartnerCardModal` (`PartnerCardModal.jsx`) sebagai modal ringkasan baca-saja yang menampilkan status kartu, informasi mitra, jumlah dokumen terlampir, statistik scan/cetak, dan tombol langsung menuju pusat kelola kartu.
  - **Fitur Modal Pratinjau Dokumen In-App (`PartnerDocumentPreviewModal`)**:
    - Mengembangkan komponen modal baru `PartnerDocumentPreviewModal.jsx` yang memungkinkan pengguna melihat langsung berkas PDF dan gambar (JPG/PNG) di dalam aplikasi tanpa harus dialihkan ke tab browser baru (`target="_blank"`).
    - Berkas diunduh aman via Axios blob dengan otentikasi Bearer token, kemudian ditampilkan melalui renderer `iframe`/`object` untuk PDF dan elemen gambar responsif untuk image, dilengkapi tombol unduh berkas lokal.
  - **Peningkatan & Perbaikan Filter Backend**:
    - `backend/src/services/partnerDocument.service.js`: Memperbaiki filter mitra pada datatable dokumen. Mendukung pencarian teks bebas (case-insensitive regex) yang mencakup `partner_id` dan `name` sekaligus, serta menambahkan populasi field `reseller` dan `image` pada relasi dokumen mitra.
    - `backend/src/services/partnerCard.service.js`: Menambahkan dukungan filter status berbasis string tunggal selain array, serta menyertakan field `image` pada proyeksi data tabel kartu mitra.
  - **Peningkatan Visual & Interaktivitas Tabel (`rows.jsx` & `columns.jsx`)**:
    - `PartnerCardNameCell`: Menampilkan foto/avatar mitra dalam bentuk squircle (`is-squircle rounded-none`), menampilkan nama dan ID mitra monospaced, serta tombol konfigurasi kartu yang interaktif.
    - `PartnerCardPartnerIdCell`: Membungkus ID mitra dengan komponen `OpenLink` yang divalidasi hak akses `partner.read` untuk menuju detail profil mitra.
    - `PartnerDocumentLabelCell`: Mengintegrasikan trigger klik langsung untuk memunculkan modal pratinjau dokumen in-app.
    - `PartnerDocumentFileNameCell`: Menambahkan styling teks terpotong (`truncate`) dengan `data-tooltip` untuk menampilkan nama berkas asli secara lengkap saat di-hover.
    - Mengganti cell renderer `PartnerDocumentPartnerCell` agar menggunakan `PartnerNameCell` standar aplikasi.
  - **Penguatan Proteksi Hak Akses (Privilege)**:
    - Menerapkan pemeriksaan hak akses terpusat pada `frontend/src/app/pages/users/partnerDocument/index.jsx` dan `archive/partnerCard/index.jsx` menggunakan `checkPrivilege('partnerDocument.list', ...)` dan `useHasPrivilege('partnerDocument.create')` agar tampilan tabel disembunyikan dengan pesan unauthorized bila pengguna tidak berhak.
  - **Penyempurnaan Struktur Menu Sidebar**:
    - Menambahkan elemen pemisah (`NAV_TYPE_DIVIDER`) pada `frontend/src/app/navigation/users.js` sebelum entri menu Dokumen Mitra agar visual navigasi di sidebar lebih terstruktur dan rapi.

---

## 🌿 Branch: `issue-175` — Issue #175: TR-069 GenieACS Integration & ONT/CPE Device Monitoring

### 📌 Informasi Issue

- **Nomor Issue**: #175
- **Judul Issue**: TR-069 GenieACS Integration — ACS Device Monitoring, Customer Linking, Signal History & Device Configuration
- **Status Branch**: `Belum di-merge` (Branch `origin/issue-175` / `issue-175`, commit `1f779d90`)

### 📅 Rincian Commit

#### [1f779d90] - resolve #175 - Rabu, 23 September 2026, 11:00:04 WIB

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
  - `backend/src/services/option.service.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/test/integration/acs.controller.test.js` [NEW]
  - `backend/test/integration/acs.service.test.js` [NEW]
  - `backend/test/integration/acsConfig.service.test.js` [NEW]
  - `backend/test/integration/acsProvision.service.test.js` [NEW]
  - `backend/test/integration/acsSignal.service.test.js` [NEW]
  - `backend/test/integration/syslogAiAnalysis.service.test.js`
  - `cron-worker/src/jobs/processors/acsSignalCheck.js` [NEW]
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `docs/superpowers/specs/2026-09-20-acs-monitoring-v1-design.md` [NEW]
  - `docs/superpowers/specs/2026-09-21-acs-customer-linking-design.md` [NEW]
  - `docs/superpowers/specs/2026-09-21-acs-signal-history-design.md` [NEW]
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
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Microservice ACS TR-069 (`/acs`)**:
    - Membangun microservice baru `acs/` berbasis Node.js yang bertindak sebagai jembatan ekstensi GenieACS untuk menangkap berkas *inform webhook* (`informWebhook.cjs`) dan skrip *provisioning* (`config/provisions/inform.js`).
    - Menyediakan *mock ONT script* (`mockOnt.js`) dan data uji fixture untuk pengujian multi-vendor (V-SOL V2802DAC, XPON 1GE WiFi, dll.).
    - Containerization mandiri dengan `Dockerfile` dan konfigurasi environment terisolasi.
  - **Backend Core ACS (Controller, Service, Models, Routes)**:
    - Membuat model data `AcsDevice` (`models/acsDevice.model.js`) untuk menyimpan metadata ONT pelanggan (serial number, manufacturer, model, OUI, MAC address, versi firmware/hardware, status online/offline, uptime, IP address WAN/LAN, dan parameter optik Rx/Tx power).
    - Membuat model data `AcsSignalDaily` (`models/acsSignalDaily.model.js`) untuk merekam riwayat kualitas sinyal optik harian (Rx/Tx dBm, voltase, temperatur, bias current).
    - Membangun controller dan routing REST API lengkap di `backend/src/controllers/acs.controller.js` dan `backend/src/routes/acs.route.js` untuk daftar perangkat, detail teknis, konfigurasi parameter, reboot ONT, factory reset, dan penautan akun pelanggan (`RadiusAuthentication`).
    - Mengimplementasikan service bisnis:
      - `acs.service.js`: agregasi datatable perangkat, sinkronisasi webhook GenieACS, manajemen status online/offline.
      - `acsConfig.service.js`: manipulasi parameter TR-069 jarak jauh (SSID WiFi, password WPA, WAN PPPoE credentials, LAN IP).
      - `acsProvision.service.js`: otomasi alur provisi perangkat baru yang terhubung ke jaringan.
      - `acsSignal.service.js`: analisis tren sinyal optik dan agregasi data historis.
    - Menambahkan rute internal di `backend/src/routes/internal.route.js` untuk menerima webhook dari service ACS secara aman dengan API key.
  - **Integrasi Cron Worker (`/cron-worker`)**:
    - Menambahkan scheduled job `acsSignalCheck` (`cron-worker/src/jobs/processors/acsSignalCheck.js`) yang berjalan secara periodik untuk mengecek degradasi sinyal optik ONT dan mencatat peringatan bila Rx power turun di bawah ambang batas toleransi (red-zone optical signal).
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
    - Menulis rangkaian pengujian otomatis integrasi backend (`acs.controller.test.js`, `acs.service.test.js`, `acsConfig.service.test.js`, `acsProvision.service.test.js`, `acsSignal.service.test.js`).
    - Menulis berkas dokumen spesifikasi desain arsitektur di `docs/superpowers/specs/`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #304  | Refactoring UX Modal, Preview In-App & Peningkatan Visual Mitra | Interaksi modal terpusat, pratinjau dokumen in-app instan, pencarian fleksibel, dan visual avatar mitra yang lebih informatif |
| #175  | TR-069 GenieACS Integration & ONT/CPE Monitoring | Pemantauan ribuan ONT/CPE pelanggan, riwayat sinyal optik, konfigurasi jarak jauh TR-069, dan penautan akun pelanggan otomatis |

### Kemampuan Baru Pengguna/Admin

- **Pratinjau Dokumen Mitra Tanpa Keluar Halaman**: Admin dapat langsung melihat pratinjau dokumen PDF dan foto mitra dalam modal in-app yang cepat tanpa membuka jendela atau tab baru peramban.
- **Pengunggahan dan Pengubahan Dokumen yang Lebih Nyaman**: Form upload dan edit dokumen mitra kini beroperasi dalam bentuk modal dialog terpusat dengan validasi nama berkas, ukuran, dan format secara real-time.
- **Pencarian Mitra Lebih Fleksibel**: Kolom pencarian mitra pada tabel Dokumen Mitra dapat menerima nama mitra maupun kode `partner_id` dengan pencocokan ekspresi reguler yang akurat.
- **Monitoring ONT/CPE Jarak Jauh (TR-069)**: Tim operasional jaringan (NOC/Helpdesk) dapat memantau status perangkat ONT pelanggan secara real-time (online/offline, serial number, redaman optik Rx/Tx, temperatur, voltase, dan versi firmware).
- **Konfigurasi Remote CPE**: Admin dapat mengubah nama WiFi (SSID), sandi WiFi, atau merestart (reboot) ONT pelanggan dari antarmuka web tanpa harus login ke antarmuka web ONT fisik pelanggan.
- **Penautan Otomatis ONT ke Pelanggan**: Admin dapat menautkan serial number ONT yang terdeteksi dengan akun langganan pelanggan broadband (`RadiusAuthentication`).

### Bug Fix / Solusi Masalah

- **Penyelesaian Masalah Filter Dokumen Mitra**: Filter mitra sebelumnya gagal jika dicari dengan teks nama biasa karena perbedaan struktur relasi `ObjectId`. Masalah ini diselesaikan dengan resolusi nama dan ID via regex di backend service.
- **Perbaikan Redundansi UI Drawer**: Drawer samping yang sebelumnya terlalu sempit untuk pratinjau dan input multi-file kini distandarisasi ke modal dialog berukuran proporsional dengan layout Headless UI.
- **Pencegahan Berkas Tidak Berizin**: Pratinjau berkas dokumen mitra kini sepenuhnya melewati otentikasi Bearer token via endpoint API blob, mencegah kebocoran link berkas mentah.

### Menu/Fitur Baru

- **Menu ACS Devices (`/network/acs`)**: Menu baru di sidebar navigasi kategori Network untuk melihat seluruh inventaris ONT pelanggan yang terhubung via TR-069.
- **Modal Pratinjau Dokumen Mitra**: Fitur pratinjau dokumen in-app dengan dukungan zoom visual dan tombol download berkas langsung.
- **Pemisah Navigasi Dokumen Mitra**: Separator visual baru pada grup menu Pengguna untuk memisahkan domain data mitra dan dokumen operasional.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Pratinjau dan Pengelolaan Dokumen Mitra (In-App Preview)

- **Penjelasan Fitur**: Fitur ini memungkinkan staf administrasi untuk mengunggah, memperbarui label, mengganti berkas, dan meninjau berkas dokumen legalitas mitra (KTP, NPWP, NIB, Perjanjian Kerjasama) secara aman langsung dari peramban web.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Pengguna → Dokumen Mitra** di navigasi sidebar.
  2. Untuk mengunggah dokumen baru, klik tombol **+ Unggah Dokumen** di pojok kanan atas tabel.
  3. Pada modal unggah, pilih nama Mitra yang dituju, pilih satu atau beberapa berkas (PDF, JPG, PNG), sesuaikan label dokumen, lalu klik **Simpan**.
  4. Untuk melihat pratinjau dokumen, klik pada teks **Label Dokumen** di salah satu baris tabel.
  5. Modal pratinjau in-app akan terbuka menampilkan isi berkas secara instan. Pengguna dapat membaca dokumen atau mengeklik tombol **Unduh** di pojok kanan atas modal jika ingin mengunduh salinan berkas lokal.
  6. Untuk mengedit label atau mengganti berkas yang sudah ada, klik tombol **Aksi (Edit)** pada baris tabel bersangkutan, sesuaikan isian pada modal edit, lalu klik **Simpan Perubahan**.

---

### 2. Monitoring & Konfigurasi ONT Pelanggan via TR-069 (ACS Devices)

- **Penjelasan Fitur**: Fitur ACS Devices mengintegrasikan server GenieACS ke dalam aplikasi Dekasimal V2 untuk memonitor parameter operasional ONT/CPE di rumah pelanggan, melacak degradasi sinyal fiber optik, dan mengonfigurasi perangkat secara nirkabel dari pusat operasional jaringan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Network → ACS Devices** di sidebar navigasi.
  2. Pada tabel utama, pantau daftar ONT yang terhubung, status *online/offline*, serial number, nama model/vendor, dan nilai daya terima optik (*Rx Optical Power*).
  3. Klik salah satu baris perangkat untuk membuka **AcsDeviceDetailDrawer** yang memuat tab informasi perangkat, status WAN (IP publik/PPPoE), LAN port, dan riwayat sinyal optik harian.
  4. Untuk menautkan perangkat ke akun pelanggan, klik tombol **Tautkan Pelanggan**, cari nama atau ID pelanggan broadband, lalu konfirmasi penautan.
  5. Untuk mengubah konfigurasi WiFi atau merestart ONT, klik tombol **Konfigurasi** pada drawer detail:
     - Masukkan nama SSID baru atau password WiFi WPA2/WPA3.
     - Klik **Kirim Konfigurasi** untuk mengirimkan perintah TR-069 RPC `SetParameterValues` ke ONT pelanggan.
     - Bila perangkat membutuhkan restart, klik tombol **Reboot Perangkat** untuk mengirimkan RPC `Reboot`.
