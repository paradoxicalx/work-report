# 📝 Daily Work Report - Dedy S.N Putra (2026-11-03)

---

## 📅 Laporan Harian - 3 November 2026

---

## 🌿 Branch: `issue-520` — Opsi Titik Awal & Koordinat Fleksibel pada Formulir Perubahan Layanan

### 📌 Informasi Issue

- **Nomor Issue**: #520
- **Judul Issue**: Opsi Titik Awal & Koordinat Fleksibel pada Formulir Perubahan Layanan Broadband, Dedicated Internet, dan Akses Data
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `3fb98f82` dan telah dideploy ke `origin/production` melalui commit `958c0a1e`)

### 📅 Rincian Commit

#### [3fb98f82] - resolve #520 - 03 November 2026, 22:07:19 WIB

- **Komponen yang Berubah**:
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/productDataAccess.controller.js`
  - `backend/src/controllers/productDedicatedInternet.controller.js`
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/productDataAccess.route.js`
  - `backend/src/routes/productDedicatedInternet.route.js`
  - `backend/src/routes/radiusAuthentication.route.js`
  - `backend/src/services/option.service.js`
  - `backend/src/services/settingsBackup.registry.js`
  - `backend/src/utils/serviceLocationInput.js` [NEW]
  - `backend/test/helpers/serviceLocation.js` [NEW]
  - `backend/test/integration/capacitySpeed.controller.test.js`
  - `backend/test/integration/radiusAuthentication.controller.updateIp.test.js`
  - `backend/test/integration/radiusAuthentication.invoiceOnActivated.test.js`
  - `backend/test/integration/radiusAuthentication.speedLimit.test.js`
  - `backend/test/integration/serviceLocationOptional.update.test.js` [NEW]
  - `backend/test/unit/serviceLocationInput.test.js` [NEW]
  - `frontend/src/app/pages/services/broadband/edit.jsx`
  - `frontend/src/app/pages/services/broadband/schema/createShema.js`
  - `frontend/src/app/pages/services/dataAccess/edit.jsx`
  - `frontend/src/app/pages/services/dataAccess/schema/editSchema.js`
  - `frontend/src/app/pages/services/dedicatedInternet/edit.jsx`
  - `frontend/src/app/pages/services/dedicatedInternet/schema/editSchema.js`
  - `frontend/src/app/pages/services/hooks/useServiceLocationOptional.js` [NEW]
  - `frontend/src/app/pages/services/utils/serviceLocationSchema.js` [NEW]
  - `frontend/src/app/pages/services/utils/serviceLocationSchema.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/developer/SpecialSettingsTab.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Penyelarasan Pengaturan Pengembang untuk Fleksibilitas Lokasi Layanan**: Mengintegrasikan pengaturan fleksibilitas lokasi di menu Pengembang (`SpecialSettingsTab.jsx`) yang memungkinkan admin mengaktifkan status opsional untuk *Titik Awal* (`location_point`) dan *Koordinat* (`coordinate`) khusus saat melakukan pembaruan (edit) layanan broadband, dedicated internet, dan data access.
  - **Sanitasi & Proteksi Nilai Lokasi Server-Side**: Membuat utilitas terisolasi `backend/src/utils/serviceLocationInput.js` yang memproses payload pembaruan layanan:
    - Jika sakelar opsional aktif dan kolom lokasi dibiarkan kosong, server tidak menolak permintaan dan tidak menghapus koordinat/titik awal yang sudah tersimpan sebelumnya.
    - Jika kolom koordinat diisi oleh user, server tetap memvalidasi format koordinat (`latitude,longitude`) secara ketat demi menjaga integritas data geospasial.
    - Menjamin bahwa saat pembuatan layanan baru (*create*), titik awal dan koordinat tetap mutlak wajib diisi.
  - **Dukungan Backup & Restore Konfigurasi Khusus**: Menambahkan kunci konfigurasi `special_settings` pada `option.service.js` dan mendaftarkannya pada `settingsBackup.registry.js` agar preferensi penanganan lokasi ikut tercakup dalam pencadangan sistem.
  - **Skema Validasi Dinamis di Sisi Antarmuka**: Mengembangkan hook `useServiceLocationOptional.js` serta skema Yup modular `serviceLocationSchema.js` yang menyesuaikan aturan validasi form edit secara dinamis berdasarkan konfigurasi sistem yang aktif.
  - **Cakupan Pengujian Menyeluruh**: Menyertakan unit test `serviceLocationInput.test.js`, integration test `serviceLocationOptional.update.test.js`, serta unit test antarmuka `serviceLocationSchema.test.js` guna memastikan tidak ada regresi pada alur modifikasi layanan.

---

#### [93f2e9f5] - resolve #520 - 03 November 2026, 18:06:59 WIB

- **Deskripsi Perubahan & Fungsi**:
  - Implementasi awal logika kondisional lokasi opsional pada controller layanan broadband, dedicated internet, dan akses data, serta antarmuka tab Pengaturan Khusus pada menu Pengembang.

---

## 🌿 Branch: `issue-514` — Penyesuaian Tanggal Jatuh Tempo, Periode Tagihan Multi-Bulan, dan Klausul Dinamis BAPL

### 📌 Informasi Issue

- **Nomor Issue**: #514
- **Judul Issue**: Penyesuaian Tanggal Jatuh Tempo, Periode Tagihan Multi-Bulan, dan Klausul Dinamis Berita Acara Perubahan Layanan (BAPL)
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `99e82a99` dan telah dideploy ke `origin/production` melalui commit `958c0a1e`)

### 📅 Rincian Commit

#### [99e82a99] - resolve #514 - 03 November 2026, 21:44:57 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/customer.controller.js`
  - `backend/src/controllers/customerPartner.controller.js`
  - `backend/src/controllers/serviceChangeDocument.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/serviceChangeDocument.model.js`
  - `backend/src/routes/serviceChangeDocument.route.js`
  - `backend/src/services/serviceChangeDocument.service.js`
  - `backend/test/integration/customerPid.test.js`
  - `backend/test/integration/serviceChangeDocument.test.js`
  - `frontend/src/app/pages/settings/sections/Application.jsx`
  - `frontend/src/app/pages/users/document/serviceChange/CreateDrawer.jsx`
  - `frontend/src/app/pages/users/document/serviceChange/ServiceChangeDocumentPanel.jsx`
  - `frontend/src/app/pages/users/document/serviceChange/ServiceChangeDocumentView.jsx`
  - `frontend/src/app/pages/users/document/serviceChange/schema/columns.jsx`
  - `frontend/src/app/pages/users/document/serviceChange/schema/createSchema.js`
  - `frontend/src/app/pages/users/document/serviceChange/utils.js`
  - `frontend/src/app/pages/users/document/serviceChange/utils.test.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Penambahan Atribut Tanggal Jatuh Tempo Kontrak**: Menambahkan field `due_date` pada model dokumen perubahan layanan (`serviceChangeDocument.model.js`) dan kolom baru pada datatable berita acara perubahan layanan (`schema/columns.jsx`).
  - **Kalkulasi Otomatis & Validasi Keselarasan Jatuh Tempo**: Mengembangkan utilitas kalkulasi tanggal jatuh tempo pada `utils.js` dan validasi ketat di `serviceChangeDocument.service.js`:
    - Tanggal jatuh tempo wajib lebih besar dari tanggal berlaku (`effective_date`).
    - Mendukung penentuan otomatis jatuh tempo minimal 12 bulan atau kelipatan periode tagihan paket baru.
    - Menghitung dan memvalidasi keselarasan jatuh tempo untuk paket prabayar dengan periode multi-bulan (misalnya per 3, 6, atau 12 bulan).
  - **Dukungan Periode Tagihan Multi-Bulan & Perhitungan Biaya Layanan**: Memperluas antarmuka dokumen cetak dan pratinjau (`ServiceChangeDocumentView.jsx`) untuk menampilkan periode tagihan (bulanan atau multi-bulan dibayar di muka) serta biaya layanan per periode (`serviceFee`) di samping abonemen per bulan.
  - **Klausul Pernyataan Berita Acara Dinamis (BAPL Terms)**: Memperbarui penanganan klausul ketentuan BAPL di `Application.jsx` dan penggantian variabel placeholder dinamis (`{paket_lama}`, `{paket_baru}`, `{harga_baru}`, `{tanggal_berlaku}`, `{tanggal_jatuh_tempo}`, `{skema_pembayaran}`, `{periode_tagihan}`, `{bulan_kontrak}`, `{tanggal_selesai}`, `{perusahaan}`).
  - **Penyelarasan PID & Kontrak Pelanggan**: Menyelaraskan pembaruan data pelanggan (`Customer` dan `CustomerPartner`) saat dokumen disetujui agar masa aktif dan data paket baru tercatat sinkron.

---

#### [46989608] - resolve #514 - 03 November 2026, 21:44:05 WIB

- **Deskripsi Perubahan & Fungsi**:
  - Penyusunan form pembuatan BAPL dengan kalkulator tanggal jatuh tempo, perbaikan tampilan pratinjau dokumen pelanggan, dan penyusunan test integrasi BAPL.

---

## 🌿 Branch: `issue-523` — Modul Node & Site Portal Mitra, Permohonan Paket Broadband Turunan, dan Autocomplete Tipe Perangkat (resolve #518)

### 📌 Informasi Issue

- **Nomor Issue**: #518
- **Judul Issue**: Modul Manajemen Node & Sites Mitra, Permohonan Paket Broadband Turunan (Derived Packages), Autocomplete Tipe Perangkat Jaringan, dan Pengamanan Akses Tanda Tangan Dokumen Admin
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `cd4b9b48` dan telah dideploy ke `origin/production` melalui commit `78a4e3b8` dan `958c0a1e`)

### 📅 Rincian Commit

#### [cd4b9b48] - resolve #518 - 03 November 2026, 18:12:27 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/files.controller.js`
  - `backend/src/controllers/partnerApiNetworkDevice.controller.js`
  - `backend/src/controllers/partnerApiSite.controller.js`
  - `backend/src/routes/files.route.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/locationPoint.service.js`
  - `backend/src/services/networkDevice.service.js`
  - `backend/test/integration/adminSignFile.controller.test.js` [NEW]
  - `backend/test/integration/locationPointSelectScope.test.js` [NEW]
  - `backend/test/integration/networkDeviceTypeSelectScope.test.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/PaymentReceiptDocument.jsx`
  - `frontend/src/app/pages/network/radiusNas/detail.jsx`
  - `frontend/src/app/pages/services/broadband/detail.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `partner-frontend/src/app/navigation/networks.js`
  - `partner-frontend/src/app/pages/customers/customer/create.jsx`
  - `partner-frontend/src/app/pages/customers/customer/edit.jsx`
  - `partner-frontend/src/app/pages/customers/customer/index.jsx`
  - `partner-frontend/src/app/pages/customers/customer/schema/columns.jsx`
  - `partner-frontend/src/app/pages/customers/customer/view.jsx`
  - `partner-frontend/src/app/pages/home/index.jsx`
  - `partner-frontend/src/app/pages/network/devices/create.jsx`
  - `partner-frontend/src/app/pages/network/devices/detail.jsx`
  - `partner-frontend/src/app/pages/network/devices/edit.jsx`
  - `partner-frontend/src/app/pages/network/fiberCable/services/fiberApi.js`
  - `partner-frontend/src/app/pages/network/fiberCable/sidebar/CableDraftForm.jsx`
  - `partner-frontend/src/app/pages/network/fiberCable/sidebar/SplitNodePanel.jsx`
  - `partner-frontend/src/app/pages/network/sites/detail.jsx`
  - `partner-frontend/src/app/pages/network/sites/index.jsx`
  - `partner-frontend/src/app/pages/network/sites/schema/NodeCreateDrawer.jsx` [NEW]
  - `partner-frontend/src/app/pages/network/sites/schema/NodeManageDrawer.jsx` [NEW]
  - `partner-frontend/src/app/pages/network/sites/schema/SiteCreateDrawer.jsx`
  - `partner-frontend/src/app/pages/network/sites/schema/SiteDetailDrawer.jsx`
  - `partner-frontend/src/app/pages/network/sites/schema/SiteEditBatchDrawer.jsx`
  - `partner-frontend/src/app/pages/network/sites/schema/SiteEditDrawer.jsx`
  - `partner-frontend/src/app/pages/network/sites/schema/columns.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/CancelDerivedRequestModal.jsx` [NEW]
  - `partner-frontend/src/app/pages/services/broadbandPackage/ChangeDerivedRequestDrawer.jsx` [NEW]
  - `partner-frontend/src/app/pages/services/broadbandPackage/DerivedRequestDetailDrawer.jsx` [NEW]
  - `partner-frontend/src/app/pages/services/broadbandPackage/createDerived.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/detail.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/index.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/schema/requestColumns.jsx` [NEW]
  - `partner-frontend/src/app/pages/services/pppoe/components/AccountForm.jsx`
  - `partner-frontend/src/app/router/network/sitesRoute.jsx`
  - `partner-frontend/src/app/router/protected.jsx`
  - `partner-frontend/src/constants/privilegeDescriptions.en.json`
  - `partner-frontend/src/constants/privilegeDescriptions.id.json`
  - `partner-frontend/src/i18n/locales/en/translations.json`
  - `partner-frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Modul Node & Sites Lengkap pada Portal Mitra (`partner-frontend`)**:
    - Menghadirkan menu navigasi `Node & Sites` (`navigation/networks.js`) dan halaman manajemen site/POP serta node fiber optic bagi pengguna portal mitra.
    - Menyediakan antarmuka pengelolaan node fiber (`NodeManageDrawer.jsx`) untuk memantau status port (AVAILABLE/BROKEN), mengubah kondisi operasional port, mencatat rincian port, dan melihat rasio spesifikasi splitter (IN:OUT).
    - Menambahkan drawer pembuatan node (`NodeCreateDrawer.jsx`), pengeditan site perorangan/massal (`SiteEditDrawer.jsx`, `SiteEditBatchDrawer.jsx`), dan detail site.
  - **Alur Lengkap Permohonan Paket Broadband Turunan (Derived Packages)**:
    - Menambahkan tab "Riwayat Pengajuan Paket" pada menu paket broadband portal mitra (`index.jsx`).
    - Membuat drawer pengajuan paket turunan (`createDerived.jsx`) yang memungkinkan mitra menurunkan paket master yang diizinkan untuk dijual kembali (resell) dengan batas kecepatan dan harga jual kustom.
    - Menyediakan drawer rincian permohonan (`DerivedRequestDetailDrawer.jsx`), drawer pengubahan permohonan (`ChangeDerivedRequestDrawer.jsx`), serta modal pembatalan permohonan (`CancelDerivedRequestModal.jsx`).
  - **Endpoint Autocomplete Tipe Perangkat & Site Terisolasi Scope Mitra**:
    - Menambahkan rute `/p-api/v1/network-devices/type-select` dan `/p-api/v1/site/select` pada Partner API dengan pembatasan hak akses berbasis peran (mitra hanya dapat melihat perangkat dan site miliknya atau yang berstatus publik).
    - Memperbaiki penggabungan kueri pencarian di `locationPoint.service.js` dan `networkDevice.service.js` dengan `$and` agar filter scope mitra tidak tertimpa oleh kueri pencarian teks.
  - **Endpoint Publik Tanda Tangan Admin yang Aman & Efisien**:
    - Membuat endpoint `/file/admin-sign/:id` (`getAdminSignFile`) dengan validasi ekspresi reguler nama ID `^[A-Za-z0-9_-]{1,64}$` untuk mencegah serangan path traversal dan *header injection*.
    - Memperbarui fungsi pencarian tanda tangan pada `files.controller.js` agar mencari kandidat kunci MinIO yang sesuai dan mendukung fallback pencarian melalui `admin_id`.
    - Mengintegrasikan pemuatan tanda tangan admin pada cetak kuitansi pembayaran (`PaymentReceiptDocument.jsx`) langsung dari endpoint gambar dengan proteksi *browser caching*.

---

#### [a1412749] - resolve #518 - 03 November 2026, 18:11:11 WIB

- **Deskripsi Perubahan & Fungsi**:
  - Penyusunan komponen drawer portal mitra, endpoint controller partner API untuk select site & device type, serta implementasi endpoint penayangan tanda tangan admin.

---

## 🌿 Branch: `master` — Pembaruan Changelog Rilis v1.99.3 s.d v1.99.6 & Basis Pengetahuan AI

### 📌 Informasi Issue

- **Nomor Issue**: N/A (Changelog & Documentation Sync)
- **Judul Issue**: Pembaruan Indeks Changelog Rilis v1.99.3 s.d v1.99.6 (Issue #509, #510, #511, #516) & Penyelarasan Basis Pengetahuan Fitur AI
- **Status Branch**: `Sudah di-merge` (Commit `001c6a5a` pada `origin/master` dan telah dideploy ke `origin/production` melalui commit `8823d267`)

### 📅 Rincian Commit

#### [001c6a5a] - update changelog - 03 November 2026, 14:44:19 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-509.json` [NEW]
  - `backend/src/data/changelog/releases/issue-510.json` [NEW]
  - `backend/src/data/changelog/releases/issue-511.json` [NEW]
  - `backend/src/data/changelog/releases/issue-516.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Penyusunan Catatan Rilis Resmi**: Menyusun berkas rilis terstruktur untuk 4 isu besar sebelumnya:
    - `issue-509.json` (v1.99.3): Log Akuntansi Router NAS, Pelacakan Router Asing, dan Migrasi Profil Bandwidth.
    - `issue-510.json` (v1.99.5): Pencatatan Penarikan Dana Gerbang Pembayaran dan Penyesuaian Saldo Awal.
    - `issue-511.json` (v1.99.4): Penyelarasan Jatuh Tempo Tagihan, Pemulihan Sesi Beku, dan Tata Letak Faktur.
    - `issue-516.json` (v1.99.6): Pemantauan Router Terhubung pada Sesi Jaringan, Bantuan Callback Pembayaran, dan Keandalan Pekerjaan Sistem.
  - **Sinkronisasi Indeks Changelog**: Menambahkan metadata rilis, versi, judul, deskripsi, dan indeks pencarian ke dalam `index.json`.
  - **Pembaruan Basis Pengetahuan Fitur AI**: Memperbarui berkas pengetahuan `features.json` agar asisten AI internal mengenali kemampuan sistem terbaru seputar integrasi NAS, rekonsiliasi gateway, permohonan paket, dan konfigurasi callback.

---

## 🌿 Branch: `issue-516` — Pemantauan Router Terhubung pada Sesi Jaringan, Bantuan Callback Pembayaran, dan Keandalan Pekerjaan Sistem

### 📌 Informasi Issue

- **Nomor Issue**: #516
- **Judul Issue**: Pemantauan Router Terhubung pada Sesi Jaringan, Bantuan Callback Pembayaran, dan Keandalan Pekerjaan Sistem
- **Status Branch**: `Sudah di-merge` (Di-merge ke `origin/master` melalui commit `16a34497` dan telah dideploy ke `origin/production` melalui commit `8823d267` dan `958c0a1e`)

### 📅 Rincian Commit

#### [16a34497] - resolve #516 - 03 November 2026, 12:13:57 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/app.js`
  - `backend/src/controllers/customer.controller.js`
  - `backend/src/controllers/customerPartner.controller.js`
  - `backend/src/controllers/financeInvoice.controller.js`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/controllers/radiusAuthenticationNonCustomer.controller.js`
  - `backend/src/controllers/radiusControl.controller.js`
  - `backend/src/controllers/radiusNas.controller.js`
  - `backend/src/controllers/radiusSession.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/internal.route.js`
  - `backend/src/routes/radiusNas.route.js`
  - `backend/src/services/cronJob.service.js` [NEW]
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/src/services/radiusNas.service.js`
  - `backend/src/services/radiusSession.service.js`
  - `backend/src/utils/image-resizer.js`
  - `backend/src/utils/ipayCallbackUrl.js` [NEW]
  - `backend/test/integration/financeInvoice.relatedInvoices.test.js` [NEW]
  - `backend/test/integration/financeSettings.ipayCallback.test.js` [NEW]
  - `backend/test/integration/internalCronJob.controller.test.js` [NEW]
  - `backend/test/integration/radiusNas.create.test.js` [NEW]
  - `backend/test/integration/radiusSession.nasEnrichment.test.js` [NEW]
  - `backend/test/unit/cronJob.service.test.js` [NEW]
  - `backend/test/unit/ipayCallbackUrl.test.js` [NEW]
  - `backend/test/unit/radiusControl.disconnectSession.controller.test.js`
  - `cron-worker/src/controllers/cron.controller.js`
  - `cron-worker/src/jobs/lastRun.js` [NEW]
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `cron-worker/src/services/cronJobPoller.js` [NEW]
  - `cron-worker/test/cronJobPoller.test.js` [NEW]
  - `cron-worker/test/lastRun.test.js` [NEW]
  - `frontend/src/app/pages/dashboards/radius/schema/sessionColumns.jsx`
  - `frontend/src/app/pages/network/radiusNas/components/NasCell.jsx` [NEW]
  - `frontend/src/app/pages/network/radiusNonCustomer/detail.jsx`
  - `frontend/src/app/pages/network/radiusSession/schema/columns.jsx`
  - `frontend/src/app/pages/services/broadband/detail.jsx`
  - `frontend/src/app/pages/services/broadband/schema/columnsLogs.jsx`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/IpaymuGatewaySettings.jsx`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/providerLogic.js`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/providerLogic.test.js`
  - `frontend/src/app/pages/users/business/profile.jsx`
  - `frontend/src/app/pages/users/customer/profile.jsx`
  - `frontend/src/app/pages/users/partner/profile.jsx`
  - `frontend/src/components/shared/Badge.jsx`
  - `frontend/src/components/shared/PaymentStatusBadge.test.jsx` [NEW]
  - `frontend/src/components/shared/form/FormInput.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/radiusConnectionStatus.js` [NEW]
  - `frontend/src/utils/radiusConnectionStatus.test.js` [NEW]
  - `frontend/src/utils/serverFieldErrors.js`
  - `radius-server/DOCUMENTATION.md`
  - `radius-server/internal/repository/mongo/hotspot_session_repo.go`
  - `radius-server/internal/repository/mongo/session_repo.go`
  - `radius-server/test/integration/session_nasip_test.go` [NEW]
  - `radius-server/test/testutil/fake_repos.go`
  - `reverse-proxy/templates/backend.conf.template`
- **Deskripsi Perubahan & Fungsi**:
  - **Penyajian Router NAS Asal pada Sesi Jaringan & Layanan**:
    - Menyempurnakan penyimpanan field `nas_ip` dan relasi router NAS di `radius-server` (`session_repo.go`, `hotspot_session_repo.go`).
    - Memperkaya (*enrichment*) data sesi aktif dan log akuntansi di `radiusSession.service.js` dengan informasi detail router NAS (nama router, IP, deskripsi).
    - Menghadirkan komponen reusable `NasCell.jsx` pada tabel sesi aktif, dasbor jaringan, serta detail layanan broadband dan akun non-pelanggan untuk mempermudah identifikasi router yang melayani koneksi pelanggan saat ini.
  - **Bantuan Konfigurasi URL Callback iPaymu**:
    - Mengembangkan utilitas `ipayCallbackUrl.js` dan panel panduan konfigurasi pada `IpaymuGatewaySettings.jsx` di Pengaturan Keuangan.
    - Menyediakan tombol otomatisasi satu-klik untuk mengisi alamat callback webhook pembayaran berbasis domain aplikasi resmi serta memberikan peringatan visual jika format tautan belum valid.
  - **Peningkatan Keandalan & Pemantauan Eksekusi Cron Worker**:
    - Mengembangkan mekanisme pelacakan eksekusi pekerjaan terjadwal pada `cron-worker` (`lastRun.js`, `cronJobPoller.js`) dan backend service `cronJob.service.js`.
    - Menyediakan endpoint internal `/internal/cron-job/last-run` untuk mencatat riwayat eksekusi setiap job secara atomik sehingga mencegah duplikasi proses atau *deadlock* antar worker.
  - **Validasi Ukuran File Unggahan & Penyempurnaan Status Koneksi**:
    - Menambahkan validasi dini di sisi klien pada `FormInput.jsx` untuk mendeteksi berkas yang melampaui batas server sebelum diunggah, mencegah galat 413 generik.
    - Menghadirkan utilitas `radiusConnectionStatus.js` yang menyajikan status koneksi akurat (ONLINE, OFFLINE, ISOLIR) pada profil layanan broadband pelanggan.
    - Memperbarui komponen `Badge.jsx` dan pemetaan status pembayaran agar seragam di seluruh tabel aplikasi.

---

#### [f6e2dcea] - resolve #516 - 03 November 2026, 12:12:13 WIB

- **Deskripsi Perubahan & Fungsi**:
  - Implementasi utama modul pengayaan data router NAS, pelacakan last-run job worker, utilitas penentu status koneksi RADIUS, dan integrasi tampilan callback gateway.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #520  | Opsi Titik Awal & Koordinat Fleksibel pada Formulir Perubahan Layanan | Memungkinkan pembaruan data layanan lama/migrasi tanpa terhambat validasi wajib titik awal dan koordinat jika fitur opsional diaktifkan di Pengaturan Pengembang. |
| #514  | Penyesuaian Tanggal Jatuh Tempo, Periode Multi-Bulan, dan Klausul BAPL | Menambahkan penentuan tanggal jatuh tempo kontrak, dukungan periode tagihan multi-bulan prabayar, dan klausul dinamis dengan variabel placeholder pada Berita Acara Perubahan Layanan. |
| #518  | Modul Node & Sites Mitra, Permohonan Paket Turunan, dan Autocomplete Tipe Perangkat | Menghadirkan fitur manajemen POP/Site dan port fiber optic di portal mitra, alur permohonan paket turunan (derived package), autocomplete efisien, dan endpoint tanda tangan admin yang aman. |
| #516  | Pemantauan Router Sesi Jaringan, Bantuan Callback iPaymu, dan Keandalan Cron | Menampilkan informasi router NAS asal pada sesi pelanggan, tombol konfigurasi otomatis URL callback iPaymu, pelacakan riwayat pekerjaan latar belakang, dan validasi berkas formulir. |
| Docs  | Pembaruan Indeks Changelog v1.99.3 s.d v1.99.6 & Basis Pengetahuan AI | Mendokumentasikan seluruh riwayat rilis sistem terkini secara terstruktur dan memperkaya basis pengetahuan model AI asisten internal. |

---

### Kemampuan Baru Pengguna/Admin

1. **Fleksibilitas Pengubahan Data Layanan Pelanggan**: Admin kini dapat memperbarui informasi paket atau spesifikasi layanan broadband, dedicated internet, dan data access milik pelanggan lama yang belum memiliki titik lokasi geospasial tanpa terhalang validasi form.
2. **Kustomisasi & Otomatisasi Berita Acara Perubahan Layanan (BAPL)**: Admin dapat menetapkan tanggal jatuh tempo kontrak baru dengan panduan kelipatan periode tagihan, serta menggunakan klausul perjanjian dinamis yang langsung mencantumkan nilai kontrak resmi secara otomatis.
3. **Pemberdayaan Operasional Mitra (Portal Mitra)**:
   - Pengguna mitra kini dapat memantau dan mengelola node jaringan, site/POP, dan port kabel fiber optik milik mereka sendiri.
   - Mitra dapat mengajukan permohonan paket broadband turunan secara mandiri dengan menetapkan margin harga jual dan batas kecepatan yang diinginkan.
4. **Penelusuran Lokasi Router Sesi Pelanggan**: Administrator jaringan dapat langsung mengetahui nama dan IP router NAS MikroTik yang sedang melayani sesi koneksi pelanggan aktif, mempercepat penanganan kendala jaringan lokal.
5. **Konfigurasi Cepat Gateway Pembayaran**: Tim keuangan dapat mengisi URL callback iPaymu secara instan hanya dengan mengklik satu tombol pada Pengaturan Keuangan.

---

### Bug Fix / Solusi Masalah

1. **Terhambatnya Edit Layanan Hasil Migrasi (Issue #520)**: Memperbaiki kendala di mana pelanggan lama hasil migrasi database yang belum memiliki koordinat atau titik awal tidak dapat diubah paket atau langganannya karena formulir mewajibkan koordinat baru.
2. **Ketiadaan Tanggal Jatuh Tempo Resmi pada BAPL (Issue #514)**: Menyelesaikan ketidaksesuaian dokumen perubahan layanan lama yang tidak mencatat batas akhir masa kontrak atau tanggal jatuh tempo, khususnya untuk paket dengan periode penagihan tahunan/multi-bulan dibayar di muka.
3. **Pemuatan Gambar Tanda Tangan Admin yang Lambat & Rawan Galat (Issue #518)**: Menggantikan pengiriman base64 mentah tanda tangan admin pada cetak kuitansi dengan endpoint `/file/admin-sign/:id` yang aman, terisolasi, dan ter-cache di browser.
4. **Penimpaan Filter Scope Mitra pada Kueri Autocomplete (Issue #518)**: Memperbaiki konstruksi agregasi kueri lokasi dan perangkat dengan operator `$and` sehingga filter pembatasan data milik mitra tidak tertimpa oleh kata kunci pencarian teks.
5. **Kegagalan Unggah File Melebihi Batas (Issue #516)**: Mengatasi kendala user yang mengunggah dokumen/gambar melebihi batas ukuran server tanpa pesan galat yang jelas melalui validasi awal di komponen formulir.

---

### Menu/Fitur Baru

1. **Menu Pengembang > Pengaturan Khusus > Titik Awal & Koordinat (Ubah Layanan)** (`SpecialSettingsTab.jsx`): Sakelar untuk menonaktifkan kewajiban titik awal dan koordinat pada form edit broadband, dedicated internet, dan data access.
2. **Portal Mitra > Menu Jaringan > Node & Sites** (`partner-frontend/src/app/pages/network/sites/`): Antarmuka manajemen infrastruktur site, POP, dan node fiber optik khusus mitra.
3. **Portal Mitra > Paket Broadband > Tab Riwayat Pengajuan Paket** (`partner-frontend/src/app/pages/services/broadbandPackage/`): Fitur permohonan paket turunan (derived package) lengkap dengan drawer pembuatan, pratinjau, pengubahan, dan pembatalan.
4. **Dokumen Perubahan Layanan > Kolom Tanggal Jatuh Tempo & Kalkulator Periode Tagihan** (`CreateDrawer.jsx` & `ServiceChangeDocumentView.jsx`): Kolom jatuh tempo dengan perhitungan kelipatan periode pembayaran prabayar.
5. **Pengaturan Keuangan > Gerbang Pembayaran > Bantuan URL Callback iPaymu** (`IpaymuGatewaySettings.jsx`): Tombol salin dan pengisian otomatis endpoint notifikasi pembayaran.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penonaktifan Lokasi Wajib saat Ubah Layanan (Migrasi Data Lama)
- **Penjelasan Fitur**: Pada sistem standar, setiap layanan wajib memiliki data *Titik Awal* dan *Koordinat*. Namun, data pelanggan lama seringkali belum dilengkapi koordinat saat migrasi awal sehingga tombol simpan formulir terkunci. Fitur ini memberikan keleluasaan bagi admin pengembang untuk mengizinkan perubahan data layanan tanpa mewajibkan pengisian koordinat (tetap wajib saat menambah layanan baru).
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Pengaturan** ➔ tab **Pengembang** ➔ pilih sub-tab **Pengaturan Khusus**.
  2. Gulir ke bagian **Titik Awal & Koordinat (Ubah Layanan)**.
  3. Aktifkan sakelar pada jenis layanan yang diinginkan (misalnya: *Ubah Layanan Broadband*).
  4. Buka kembali halaman **Layanan Broadband** ➔ pilih pelanggan yang ingin diperbarui ➔ klik **Ubah Layanan**.
  5. Kolom titik awal dan koordinat kini bertanda opsional dan formulir dapat disimpan meskipun koordinat kosong.

---

### 2. Pembuatan Berita Acara Perubahan Layanan (BAPL) dengan Tanggal Jatuh Tempo
- **Penjelasan Fitur**: Saat pelanggan melakukan upgrade/downgrade paket, Berita Acara Perubahan Layanan kini secara otomatis menghitung estimasi tanggal jatuh tempo kontrak berdasarkan periode penagihan paket baru (misal kelipatan 12 bulan atau multi-bulan prabayar), serta menyematkan klausul hukum yang dinamis sesuai data tagihan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Dokumen** ➔ **Perubahan Layanan** (atau melalui tombol *Buat Berita Acara* pada detail pelanggan).
  2. Klik tombol **Tambah Berita Acara**.
  3. Pilih pelanggan dan paket layanan tujuan yang baru.
  4. Tentukan **Tanggal Berlaku**. Sistem akan secara otomatis mengisi rekomendasi **Tanggal Jatuh Tempo** (minimal 12 bulan dan selaras dengan periode tagihan). Admin tetap dapat menyesuaikannya bila ada kesepakatan khusus.
  5. Klik **Simpan**. Sistem akan menyusun dokumen resmi lengkap dengan tabel perbandingan abonemen, periode tagihan, dan klausul pernyataan kontrak.

---

### 3. Pengajuan Permohonan Paket Broadband Turunan oleh Mitra
- **Penjelasan Fitur**: Mitra bisnis kini dapat menjual kembali (*resell*) paket internet induk master dari perusahaan pusat dengan mengkustomisasi profil kecepatan (speed limit) dan harga jual ke pelanggan akhir mitra, yang diajukan ke admin pusat untuk persetujuan.
- **Langkah Penggunaan (Tutorial)**:
  1. Login ke **Portal Mitra** (`partner-frontend`).
  2. Buka menu **Layanan** ➔ **Paket Broadband** ➔ pilih tab **Riwayat Pengajuan Paket**.
  3. Klik tombol **Permohonan Paket Turunan**.
  4. Pada langkah 1, pilih **Paket Induk Master** yang berstatus *Tersedia Resell*.
  5. Pada langkah 2, tentukan nama paket khusus mitra, batas kecepatan (upload/download), dan harga jual mitra.
  6. Pada langkah 3, tentukan konfigurasi profil MikroTik / RADIUS pool yang sesuai.
  7. Klik **Ajukan Permohonan**. Permohonan akan masuk ke antrean persetujuan administrator pusat dan statusnya dapat dipantau langsung dari tabel riwayat.

