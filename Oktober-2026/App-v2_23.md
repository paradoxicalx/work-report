# 📝 Daily Work Report - Dedy S.N Putra (2026-10-23)

---

## 📅 Laporan Harian - 23 Oktober 2026

---

## 🌿 Branch: `master` / `production` — Pembaruan Changelog & Rilis Produksi

### 📌 Informasi Issue

- **Nomor Issue**: Maintenance & Release (v1.98.21 – v1.98.25)
- **Judul Issue**: Pembaruan Catatan Rilis Changelog dan Sinkronisasi Branch Produksi
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [1c3e7c32] - Merge branch 'master' into production - 23 Oktober 2026, 23:15:47 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `acs/Dockerfile`
  - `backend/Dockerfile`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/financeInvoice.controller.js`
  - `backend/src/controllers/partnerApiInvoice.controller.js`
  - `backend/src/controllers/partnerApiPartner.controller.js`
  - `backend/src/controllers/publicInvoice.controller.js`
  - `backend/src/controllers/waOutreach.controller.js` [NEW]
  - `backend/src/controllers/waRecovery.controller.js` [NEW]
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-466.json` [NEW]
  - `backend/src/data/changelog/releases/issue-467.json` [NEW]
  - `backend/src/data/changelog/releases/issue-469.json` [NEW]
  - `backend/src/data/changelog/releases/issue-470.json` [NEW]
  - `backend/src/data/changelog/releases/issue-474.json` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/index.jsx`
  - `frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`
  - `partner-frontend/src/app/pages/home/index.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - Menggabungkan seluruh pembaruan dari branch `master` ke branch `production` untuk deployment live sistem.
  - Memastikan seluruh fitur baru, perbaikan keamanan, dan optimalisasi performa siap beroperasi di lingkungan produksi.

#### [9fec9b96] - update changelog - 23 Oktober 2026, 23:15:10 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-466.json` [NEW]
  - `backend/src/data/changelog/releases/issue-467.json` [NEW]
  - `backend/src/data/changelog/releases/issue-470.json` [NEW]
  - `backend/src/data/changelog/releases/issue-474.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Mendaftarkan rilis versi `v1.98.22`, `v1.98.23`, `v1.98.24`, dan `v1.98.25` ke dalam index catatan rilis sistem.
  - Menyusun rincian fitur, perbaikan bug, dan peningkatan sistem untuk masing-masing issue dalam format JSON standar changelog.

---

## 🌿 Branch: `issue-466` — Dukungan Potongan Nilai pada Faktur Tagihan Keuangan

### 📌 Informasi Issue

- **Nomor Issue**: #466
- **Judul Issue**: Dukungan Potongan Nilai pada Faktur Tagihan Keuangan
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [501aa2d0] - resolve #466 - 23 Oktober 2026, 23:07:04 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/financeInvoice.controller.js`
  - `backend/src/controllers/partnerApiInvoice.controller.js`
  - `backend/src/controllers/publicInvoice.controller.js`
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeInvoice.model.js`
  - `backend/src/routes/financeInvoice.route.js`
  - `backend/src/routes/mobileCustomer.route.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/routes/public.route.js`
  - `backend/src/services/financeCoa.service.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/financeInvoiceClassifier.service.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/src/services/financePeriod.service.js`
  - `backend/src/services/financeReport.service.js`
  - `backend/src/services/financeWallet.service.js`
  - `backend/src/services/paymentGateways/ipaymu.gateway.js`
  - `backend/src/utils/finance-error.js`
  - `backend/test/integration/financeInvoice.deduction.test.js` [NEW]
  - `backend/test/integration/financeInvoice.mobileOwnership.test.js`
  - `backend/test/integration/financePeriod.test.js`
  - `backend/test/integration/ipaymuGateway.deductionPayload.test.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`
  - `frontend/src/app/pages/finance/invoices/InvoiceDrawer.jsx`
  - `frontend/src/app/pages/finance/invoices/InvoiceItemsField.jsx`
  - `frontend/src/app/pages/finance/invoices/detail.jsx`
  - `frontend/src/app/pages/finance/invoices/schema/invoiceSchema.js`
  - `frontend/src/app/pages/finance/invoices/utils/invoiceDeductions.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/utils/invoiceDeductions.test.js` [NEW]
  - `frontend/src/components/shared/form/Combobox.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Dukungan Potongan Nilai (Deduction/Diskon)**: Menambahkan input dan perhitungan potongan nilai pada form pembuatan dan edit invoice keuangan, baik dalam bentuk nominal langsung (`fixed`) maupun persentase (`percentage`).
  - **Validasi Nilai Potongan**: Memastikan potongan tidak bernilai negatif dan tidak melebihi subtotal tagihan kotor, mencegah terjadinya nilai tagihan negatif yang tidak valid.
  - **Privasi Keterangan Potongan**: Catatan audit internal mengenai alasan pemberian potongan tetap disimpan di backend untuk transparansi manajemen, sedangkan pada dokumen faktur pelanggan hanya ditampilkan label diskon yang bersih dan profesional.
  - **Sinkronisasi Jurnal & Gateway Pembayaran**: Menghubungkan perhitungan total bersih tagihan (setelah potongan) ke pencatatan COA/jurnal akuntansi keuangan, dompet saldo pelanggan, dan integrasi payment gateway (iPaymu).
  - **Tampilan Dokumen Cetak & Detail**: Memperbarui antarmuka cetak PDF/faktur dan halaman detail tagihan agar menyajikan rincian baris potongan secara jelas dan transparan.

---

## 🌿 Branch: `issue-467` — Pesan Pembuka Obrolan WhatsApp dan Alat Pemulihan Retensi

### 📌 Informasi Issue

- **Nomor Issue**: #467
- **Judul Issue**: Pesan Pembuka Obrolan WhatsApp dan Alat Pemulihan Retensi
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [b972a4de] - resolve #467 - 23 Oktober 2026, 22:19:36 WIB
#### [35784854] - resolve #467 - 23 Oktober 2026, 14:57:41 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/waOutreach.controller.js` [NEW]
  - `backend/src/controllers/waRecovery.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/middlewares/logger.middleware.js`
  - `backend/src/models/waConversation.model.js`
  - `backend/src/models/waOutreachQuota.model.js` [NEW]
  - `backend/src/routes/waChat.route.js`
  - `backend/src/services/baileysControl.service.js`
  - `backend/src/services/option.service.js`
  - `backend/src/services/recovery.service.js` [NEW]
  - `backend/src/services/settingsBackup.registry.js`
  - `backend/src/services/waConversation.service.js`
  - `backend/src/services/waOutreach.service.js` [NEW]
  - `backend/src/services/waOutreachQuota.service.js` [NEW]
  - `backend/src/utils/waChatSerializer.js`
  - `backend/src/utils/waChatUtils.js`
  - `backend/src/utils/waOutreach.js` [NEW]
  - `backend/test/integration/waContactCreators.test.js`
  - `backend/test/integration/waOutreach.contactHistoryCensor.test.js` [NEW]
  - `backend/test/integration/waOutreach.conversations.test.js` [NEW]
  - `backend/test/integration/waOutreach.send.test.js` [NEW]
  - `backend/test/integration/waOutreachQuota.service.test.js` [NEW]
  - `backend/test/integration/waReport.service.test.js`
  - `backend/test/integration/waReportConversations.controller.test.js`
  - `backend/test/integration/waRetentionRecovery.test.js` [NEW]
  - `backend/test/unit/loggerSanitizer.test.js` [NEW]
  - `backend/test/unit/waChatUtils.outreach.test.js` [NEW]
  - `backend/test/unit/waConversationReaders.guard.test.js`
  - `backend/test/unit/waOutreach.test.js` [NEW]
  - `baileys-api/src/controllers/internal.controller.js`
  - `baileys-api/src/routes/internal.route.js`
  - `baileys-api/src/services/numberCheck.service.js` [NEW]
  - `baileys-api/test/numberCheck.test.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/ChatNotice.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/components/ConversationList.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/components/NewChatPanel.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/OutreachComposer.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/components/OutreachComposer.test.jsx` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/hooks/useContactHistory.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/hooks/useOutreachQuota.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/index.jsx`
  - `frontend/src/app/pages/customerService/whatsappChat/utils/outreach.js` [NEW]
  - `frontend/src/app/pages/customerService/whatsappChat/utils/outreach.test.js` [NEW]
  - `frontend/src/app/pages/settings/schema/systemSchema.js`
  - `frontend/src/app/pages/settings/sections/System.jsx`
  - `frontend/src/app/pages/developer/recoveryTools/RecoveryToolsTab.jsx`
  - `frontend/src/app/pages/developer/recoveryTools/recoveryFilter.js` [NEW]
  - `frontend/src/app/pages/developer/recoveryTools/recoveryFilter.test.js` [NEW]
  - `frontend/src/app/pages/developer/recoveryTools/recoveryRoutes.config.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Fitur Pesan Pembuka (Outreach WhatsApp)**: Admin dapat menginisiasi percakapan langsung ke nomor WhatsApp pelanggan baru atau membuka kembali sesi percakapan lama yang telah kedaluwarsa.
  - **Panel Obrolan Baru (New Chat Panel)**: Komponen panel interaktif untuk memasukkan nomor tujuan, memverifikasi apakah nomor aktif di WhatsApp secara realtime via Baileys API (`/internal/check-number`), dan menampilkan riwayat obrolan terdahulu dengan sensor kata sensitif.
  - **Manajemen Kuota Outreach Harian**: Model dan service kuota (`waOutreachQuota`) dengan konfigurasi batas harian total pesan keluar, batas khusus kontak baru, jeda pengiriman (anti-spam), serta pencegahan spam beruntun jika kontak belum merespons.
  - **Pengaturan Sistem Outreach**: Menambahkan formulir konfigurasi batas kuota pesan pembuka di halaman Pengaturan Sistem (System Settings) yang terintegrasi dengan pencadangan pengaturan.
  - **Alat Pemulihan Retensi Pesan & Sesi (Recovery Tool)**: Menyediakan utilitas perbaikan masa simpan (retensi) pesan dan sesi WhatsApp di menu Alat Pemulihan Pengembang, dilengkapi fitur simulasi (*dry-run*) sebelum data diperbarui secara permanen.
  - **Pencarian Cepat & Kategori Alat Pemulihan**: Memperbarui antarmuka Recovery Tools dengan tab kategori dan filter pencarian instan agar pengembang/admin mudah mengeksekusi skrip pemeliharaan database.

---

## 🌿 Branch: `issue-474` — Pembaruan Lingkungan Runtime Kontainer dan Keamanan Sistem

### 📌 Informasi Issue

- **Nomor Issue**: #474
- **Judul Issue**: Pembaruan Lingkungan Runtime Kontainer dan Keamanan Sistem
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [91417f9d] - resolve #474 - 23 Oktober 2026, 19:11:45 WIB
#### [b8d9ce4f] - resolve #474 - 23 Oktober 2026, 19:10:47 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `acs/Dockerfile`
  - `backend/Dockerfile`
  - `backend/src/server.js`
  - `backend/src/utils/generate-ai-knowledge.js`
  - `baileys-api/Dockerfile`
  - `cron-worker/Dockerfile`
  - `documentations/CHANGELOG_INSTRUCTION.md`
  - `frontend/Dockerfile`
  - `network-monitor/Dockerfile`
  - `partner-frontend/Dockerfile`
  - `radius-server/Dockerfile`
  - `reverse-proxy/Dockerfile`
  - `syslog-server/Dockerfile`
  - `telegram-api/Dockerfile`
  - `telegram-apps/Dockerfile`
  - `whatsapp-api/Dockerfile`
- **Deskripsi Perubahan & Fungsi**:
  - **Pembaruan Basis Image Kontainer (LTS Update)**: Memperbarui seluruh `Dockerfile` di monorepo ke versi basis runtime LTS terbaru guna meningkatkan stabilitas, performa execution loop, dan kompatibilitas paket npm/Go.
  - **Penguatan Keamanan Sistem**: Menambahkan pembaruan paket sistem operasi pada kontainer untuk memitigasi potensi kerentanan keamanan (CVE) bawaan base image.
  - **Optimalisasi Startup Layanan**: Menghapus redundansi overhead inisialisasi pada server backend (`server.js`), mempercepat waktu *cold boot* kontainer saat restart.
  - **Instruksi Dokumentasi Changelog**: Menyempurnakan panduan `CHANGELOG_INSTRUCTION.md` agar konsisten dengan prosedur rilis dan penomoran versi sistem.

---

## 🌿 Branch: `issue-470` — Pemisahan Jalur Notifikasi Topik Telegram Tiket dan Alert Jaringan

### 📌 Informasi Issue

- **Nomor Issue**: #470
- **Judul Issue**: Pemisahan Jalur Notifikasi Topik Telegram Tiket dan Alert Jaringan
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [18c1c4d5] - resolve #470 - 23 Oktober 2026, 18:10:11 WIB
#### [1d923322] - resolve #470 - 23 Oktober 2026, 18:09:35 WIB

- **Komponen yang Berubah**:
  - `backend/src/utils/telegram.js`
  - `backend/test/integration/telegramTopicRegistration.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Pemisahan Pengiriman Notifikasi Topik**: Memperbaiki logika perutean pesan bot Telegram agar pesan tiket gangguan pelanggan dan pesan peringatan status perangkat jaringan (network alert) dikirimkan secara akurat ke grup dan ID topik masing-masing.
  - **Pencegahan Tumpang Tindih Topik**: Menyempurnakan validasi dan pencocokan topik Telegram berbasis nama dan ID grup, menghindari kesalahan pengiriman pesan ke topik dengan nama serupa di grup yang berbeda.
  - **Pengujian Integrasi**: Menambahkan serangkaian tes integrasi otomatis (`telegramTopicRegistration.test.js`) untuk memverifikasi isolasi rute notifikasi ke topik grup Telegram.

---

## 🌿 Branch: `issue-400` — Revitalisasi Antarmuka Dashboard Mitra, Legalitas Dokumen POP, dan Integrasi Modul Layanan Mitra

### 📌 Informasi Issue

- **Nomor Issue**: #400
- **Judul Issue**: Revitalisasi Antarmuka Dashboard Mitra, Legalitas Dokumen POP, dan Integrasi Modul Layanan Mitra
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [c6d28cf1] - resolve #400 - 23 Oktober 2026, 17:48:26 WIB
#### [c07b8640] - resolve #400 - 23 Oktober 2026, 17:46:32 WIB
#### [9d79822d] - resolve #400 - 23 Oktober 2026, 17:44:21 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/partnerApiPartner.controller.js`
  - `backend/src/data/aiKnowledge/features.json`
  - `partner-frontend/src/app/contexts/theme/Provider.jsx`
  - `partner-frontend/src/app/layouts/DynamicLayout.jsx`
  - `partner-frontend/src/app/pages/account/document/configs.js` [NEW]
  - `partner-frontend/src/app/pages/account/document/document.test.jsx` [NEW]
  - `partner-frontend/src/app/pages/account/document/index.jsx` [NEW]
  - `partner-frontend/src/app/pages/customers/business/components/BusinessForm.jsx` [NEW]
  - `partner-frontend/src/app/pages/customers/business/create.jsx` [NEW]
  - `partner-frontend/src/app/pages/customers/business/edit.jsx` [NEW]
  - `partner-frontend/src/app/pages/customers/business/index.jsx` [NEW]
  - `partner-frontend/src/app/pages/customers/business/schema/businessSchema.js` [NEW]
  - `partner-frontend/src/app/pages/customers/business/schema/businessSchema.test.js` [NEW]
  - `partner-frontend/src/app/pages/customers/business/schema/columns.jsx` [NEW]
  - `partner-frontend/src/app/pages/customers/business/view.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/customerInvoice/detail.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/customerInvoice/index.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/customerInvoice/schema/columns.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/customerInvoice/schema/columns.test.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/partnerInvoice/detail.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/partnerInvoice/index.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/partnerInvoice/schema/columns.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/partnerInvoice/schema/columns.test.jsx` [NEW]
  - `partner-frontend/src/app/pages/home/home.test.jsx` [NEW]
  - `partner-frontend/src/app/pages/home/index.jsx`
  - `partner-frontend/src/app/pages/network/devices/create.jsx`
  - `partner-frontend/src/app/pages/network/devices/detail.jsx`
  - `partner-frontend/src/app/pages/network/devices/edit.jsx`
  - `partner-frontend/src/app/pages/network/devices/index.jsx`
  - `partner-frontend/src/app/pages/network/devices/schema/columns.jsx`
  - `partner-frontend/src/app/pages/network/devices/schema/columns.test.jsx` [NEW]
  - `partner-frontend/src/app/pages/network/fiberCable/services/fiberApi.js`
  - `partner-frontend/src/app/pages/services/broadbandPackage/create.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/detail.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/edit.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/editBatch.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/index.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/schema/columns.jsx`
  - `partner-frontend/src/app/pages/services/broadbandPackage/schema/columns.test.jsx` [NEW]
  - `partner-frontend/src/app/pages/services/broadbandProfile/create.jsx`
  - `partner-frontend/src/app/pages/services/broadbandProfile/detail.jsx`
  - `partner-frontend/src/app/pages/services/broadbandProfile/edit.jsx`
  - `partner-frontend/src/app/pages/services/broadbandProfile/editBatch.jsx`
  - `partner-frontend/src/app/pages/services/broadbandProfile/index.jsx`
  - `partner-frontend/src/app/pages/services/broadbandProfile/schema/columns.jsx`
  - `partner-frontend/src/app/pages/services/broadbandProfile/schema/columns.test.jsx` [NEW]
  - `partner-frontend/src/app/pages/services/session/index.jsx` [NEW]
  - `partner-frontend/src/app/pages/services/session/schema/columns.jsx` [NEW]
  - `partner-frontend/src/app/pages/services/session/schema/columns.test.jsx` [NEW]
  - `partner-frontend/src/app/router/account/accountDocumentRoute.jsx` [NEW]
  - `partner-frontend/src/app/router/customers/businessRoute.jsx` [NEW]
  - `partner-frontend/src/app/router/finance/customerInvoiceRoute.jsx` [NEW]
  - `partner-frontend/src/app/router/finance/partnerInvoiceRoute.jsx` [NEW]
  - `partner-frontend/src/app/router/network/fiberCableRoute.jsx`
  - `partner-frontend/src/app/router/network/networkDeviceRoute.jsx`
  - `partner-frontend/src/app/router/protected.jsx`
  - `partner-frontend/src/app/router/services/broadbandPackageRoute.jsx`
  - `partner-frontend/src/app/router/services/broadbandProfileRoute.jsx`
  - `partner-frontend/src/app/router/services/sessionRoute.jsx` [NEW]
  - `partner-frontend/src/configs/theme.config.js`
  - `partner-frontend/src/constants/privilegeDescriptions.en.json`
  - `partner-frontend/src/constants/privilegeDescriptions.id.json`
  - `partner-frontend/src/i18n/locales/en/translations.json`
  - `partner-frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Dashboard Mitra Modern & Komprehensif**: Revitalisasi total beranda `partner-frontend` dengan kartu KPI real-time (total pelanggan aktif/nonaktif, sesi online broadband PPPoE, kesehatan perangkat jaringan online/offline, status faktur tagihan), widget kesehatan jaringan POP, diagram distribusi pelanggan, riwayat faktur tagihan kemitraan, kartu kelengkapan dokumen POP, dan akses pintasan modul cepat.
  - **Manajemen Dokumen Legalitas POP Mitra**: Menghadirkan halaman unggah dan pratinjau dokumen resmi POP/Mitra (KTP PIC, Perjanjian Sewa Lokasi, Kontrak Kerjasama POP, Izin Lokasi, dan Foto Fasilitas POP) dengan indikator kelengkapan persentase dokumen.
  - **Manajemen Pelanggan Bisnis Mitra**: Implementasi lengkap CRUD dan halaman detail profil pelanggan bisnis khusus kemitraan POP, lengkap dengan skema validasi, filter tabel, dan pengujian unit/komponen.
  - **Faktur Tagihan Pelanggan & Mitra**: Integrasi tabel dan rincian faktur tagihan pelanggan (`customerInvoice`) serta faktur tagihan kemitraan (`partnerInvoice`) langsung dari antarmuka portal mitra.
  - **Pemantauan Sesi Aktif Broadband**: Penambahan modul pemantauan sesi online PPPoE pelanggan secara langsung (`services/session`) dengan informasi IP address, durasi koneksi, MAC address, dan status sesi.
  - **Pengelolaan Paket, Profil, dan Perangkat Jaringan**: Penyempurnaan alur kerja dan tabel data paket internet broadband, profil bandwidth, inventaris perangkat jaringan, dan rute kabel fiber optik untuk portal mitra.

---

## 🌿 Branch: `issue-469` — Isolasi Target Trafik Monitor Jaringan dan Aksi Tampilan Default

### 📌 Informasi Issue

- **Nomor Issue**: #469
- **Judul Issue**: Isolasi Target Trafik Monitor Jaringan dan Aksi Tampilan Default
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [ef35319e] - resolve #469 - 23 Oktober 2026, 17:42:34 WIB
#### [e335b84f] - resolve #469 - 23 Oktober 2026, 17:41:57 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/networkTraffic.controller.js`
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-469.json` [NEW]
  - `backend/src/services/networkTraffic.service.js`
  - `backend/test/integration/networkTrafficDeviceIsolation.test.js` [NEW]
  - `frontend/src/app/pages/network/radiusNonCustomer/detail.jsx`
  - `frontend/src/app/pages/network/traffic/index.jsx`
  - `frontend/src/app/pages/network/traffic/schema/columns.jsx`
  - `frontend/src/app/pages/services/broadband/detail.jsx`
  - `frontend/src/components/shared/table/SelectedRowsActions.jsx` [NEW]
  - `frontend/src/components/shared/table/Table.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `partner-frontend/src/app/pages/network/radiusNonCustomer/detail.jsx`
  - `partner-frontend/src/app/pages/network/traffic/index.jsx`
  - `partner-frontend/src/app/pages/network/traffic/schema/columns.jsx`
  - `partner-frontend/src/app/pages/services/broadband/detail.jsx`
  - `partner-frontend/src/app/pages/services/pppoe/view.jsx`
  - `partner-frontend/src/components/shared/table/SelectedRowsActions.jsx` [NEW]
  - `partner-frontend/src/components/shared/table/Table.jsx`
  - `partner-frontend/src/i18n/locales/en/translations.json`
  - `partner-frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Isolasi Target Pemantauan Trafik**: Memisahkan target pemantauan trafik khusus perangkat jaringan (`service_type: 'device'`) sehingga target interface layanan broadband dan dedicated pelanggan tidak tercampur pada tabel Trafik Monitor.
  - **Perlindungan Data Layanan Pelanggan**: Mengamankan aksi penghapusan individual dan penghapusan massal (*bulk delete*) agar tidak dapat menghapus target trafik yang dimiliki oleh akun layanan pelanggan.
  - **Pencegahan ReDoS & NoSQL Injection**: Menambahkan sanitasi `escapeRegex` pada query pencarian target trafik dan membatasi whitelist `service_type` yang diizinkan untuk diproses oleh database.
  - **Aksi Penetapan Tampilan Default Massal**: Menghadirkan tombol aksi baris terpilih (`SelectedRowsActions`) untuk menetapkan atau membatalkan status tampilan default grafik trafik secara massal langsung dari tabel.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #466  | Dukungan Potongan Nilai pada Faktur Tagihan Keuangan | Tagihan keuangan kini mendukung potongan nilai diskon (nominal/persentase) dengan audit privasi internal dan sinkronisasi otomatis ke jurnal akuntansi serta payment gateway. |
| #467  | Pesan Pembuka Obrolan WhatsApp dan Alat Pemulihan Retensi | Tim CS dapat menginisiasi percakapan WhatsApp baru ke pelanggan melalui panel Obrolan Baru yang aman kuota dan anti-spam, serta developer memiliki tool pemulihan retensi chat. |
| #474  | Pembaruan Lingkungan Runtime Kontainer dan Keamanan Sistem | Seluruh microservice monorepo diperbarui ke runtime LTS terkini dengan peningkatan keamanan OS kontainer dan startup aplikasi yang lebih cepat. |
| #470  | Pemisahan Jalur Notifikasi Topik Telegram Tiket dan Alert Jaringan | Pesan notifikasi tiket dan alert pemantauan jaringan dijamin terkirim ke topik dan grup Telegram yang tepat tanpa risiko salah kamar. |
| #400  | Revitalisasi Antarmuka Dashboard Mitra, Legalitas Dokumen POP, dan Integrasi Modul Layanan Mitra | Portal Mitra (`partner-frontend`) memiliki dashboard interaktif modern, modul upload berkas legalitas POP, pelanggan bisnis, sesi online PPPoE, serta faktur tagihan lengkap. |
| #469  | Isolasi Target Trafik Monitor Jaringan dan Aksi Tampilan Default | Halaman Trafik Monitor diisolasi khusus perangkat jaringan dengan proteksi hapus data pelanggan, pengamanan regex injection, dan aksi kelola default massal. |

### Kemampuan Baru Pengguna/Admin

- **Admin Keuangan**: Dapat memberikan potongan nilai (diskon promosi atau penyesuaian khusus) langsung pada faktur tagihan pelanggan, dengan pilihan tipe persentase maupun nominal kustom, tanpa mengkhawatirkan bocornya catatan audit internal kepada pelanggan.
- **Admin CS / Helpdesk**: Dapat menginisiasi percakapan WhatsApp baru (*outreach*) kepada calon pelanggan atau pelanggan lama langsung dari web app, lengkap dengan validasi nomor aktif dan pengecekan sisa kuota harian.
- **Mitra Bisnis / POP**: Dapat memantau seluruh performa jaringan POP, memverifikasi kelengkapan berkas legalitas, mengelola pelanggan bisnis, memeriksa sesi online pelanggan, serta memantau tagihan dari dashboard portal mitra baru.
- **Admin Jaringan (NOC)**: Dapat mengelola grafik interface trafik perangkat jaringan secara bersih tanpa tercampur traffic pelanggan, serta dapat menandai atau mencabut target default grafik secara massal.

### Bug Fix / Solusi Masalah

- **Pencegahan Nilai Tagihan Negatif**: Validasi ketat pada backend mencegah pemberian potongan nilai yang melebihi jumlah subtotal kotor tagihan.
- **Notifikasi Bot Telegram**: Menghilangkan insiden pesan notifikasi tiket yang terkirim ke topik alert jaringan saat terdapat nama topik yang identik di grup berbeda.
- **Keamanan Input Pencarian Trafik**: Mengatasi potensi serangan ReDoS (*Regular Expression Denial of Service*) dan NoSQL Injection pada pencarian target monitor trafik.
- **Proteksi Hapus Target Trafik Pelanggan**: Menutup celah di mana target trafik pelanggan berpotensi terhapus melalui aksi bulk delete di tabel perangkat jaringan.
- **Anti-Spam WhatsApp**: Membatasi laju pengiriman pesan baru per menit dan memblokir pengiriman berulang ke nomor yang belum membalas pesan pertama untuk menjaga reputasi nomor WhatsApp kantor.

### Menu/Fitur Baru

- **Menu WhatsApp CS**: Tombol & Drawer **Obrolan Baru** (*New Chat Panel*) dengan verifikasi nomor WhatsApp dan penghitung kuota harian.
- **Menu Pengaturan Sistem**: Konfigurasi parameter kuota dan jeda pesan pembuka WhatsApp (*Outreach Settings*).
- **Menu Developer Tools**: Tab **Alat Pemulihan (Recovery Tools)** dengan pencarian instan dan utilitas perbaikan retensi obrolan WhatsApp (*dry-run enabled*).
- **Portal Mitra - Beranda**: Dashboard analitik real-time dengan KPI pelanggan, perangkat, broadband, faktur, dan grafik kesehatan POP.
- **Portal Mitra - Akun**: Menu **Dokumen Legalitas** untuk mengunggah dokumen KTP PIC, Kontrak Kerjasama, Surat Sewa, dan Izin Lokasi.
- **Portal Mitra - Layanan**: Menu **Sesi Aktif** untuk memantau status online koneksi PPPoE pelanggan secara langsung.
- **Portal Mitra - Keuangan**: Menu **Faktur Tagihan Pelanggan** dan **Faktur Kemitraan**.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: **Inisiasi Pesan Pembuka Obrolan WhatsApp (Outreach WhatsApp)**
  Fitur ini memungkinkan staf layanan pelanggan untuk memulai chat WhatsApp ke nomor pelanggan yang belum ada di daftar kontak atau sesi obrolan aktifnya sudah kedaluwarsa. Sistem secara otomatis melakukan validasi apakah nomor terdaftar di WhatsApp melalui Baileys API, menerapkan kuota harian untuk menjaga keamanan nomor dari pemblokiran (ban), serta memeriksa riwayat interaksi sebelumnya.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke menu **Customer Service** > **Obrolan WhatsApp**.
  2. Klik tombol **Obrolan Baru** (ikon tambah pesan di samping daftar percakapan).
  3. Masukkan nomor telepon WhatsApp tujuan (format standar diawali `08...` atau `628...`).
  4. Sistem akan otomatis memvalidasi keaktifan nomor tersebut. Jika nomor terdaftar di WhatsApp, formulir pesan pembuka akan aktif dan indikator sisa kuota harian akan ditampilkan.
  5. Ketikkan pesan pembuka yang ingin disampaikan kepada pelanggan pada kotak pesan.
  6. Klik tombol **Kirim Pesan**. Sesi percakapan baru akan langsung dibuat dan pesan terkirim ke pelanggan.
  7. Ketika pelanggan membalas, percakapan dapat dilanjutkan seperti biasa melalui jendela obrolan standar.
