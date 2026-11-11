# 📝 Daily Work Report - Dedy S.N Putra (2026-08-15)

---

## 📅 Laporan Harian - 15 Agustus 2026

---

## 🌿 Branch: `issue-216` — Perbaikan & Refactoring Modul Keuangan (Finance Invoices, Payments, Auto Invoicing, Tax Recap, WhatsApp Variable Mapping, Cron Worker Job Toggles & Layout Pengaturan)

### 📌 Informasi Issue

- **Nomor Issue**: #216
- **Judul Issue**: Perbaikan & Refactoring Modul Keuangan (Finance Invoices, Payments, Auto Invoicing, Tax Recap, WhatsApp Variable Mapping, Cron Worker Job Toggles & Layout Pengaturan)
- **Status Branch**: `Dalam Pengerjaan` (Work In Progress / Uncommitted Changes)

### 📅 Rincian Commit

#### [Work In Progress] - Pengembangan Auto Invoicing Broadband, Rekap PPN Keluaran, Toggle Job Cron Worker & Pemetaan Variabel WhatsApp - 15 Agt 2026

- **Komponen yang Berubah**:
  - `backend/src/services/cronSettings.service.js` [NEW]
  - `backend/src/services/financeAutoInvoice.service.js` [NEW]
  - `backend/test/integration/financeAutoInvoice.test.js` [NEW]
  - `backend/test/integration/financeInvoiceTaxRecap.test.js` [NEW]
  - `cron-worker/src/jobs/processors/financeAutoInvoiceGenerate.js` [NEW]
  - `cron-worker/src/jobs/processors/invoiceFreeze.js` [NEW]
  - `cron-worker/src/services/cronSettings.service.js` [NEW]
  - `frontend/src/app/pages/finance/reports/TaxRecap.jsx` [NEW]
  - `FINANCE_AUDIT.md`
  - `backend/src/controllers/financeInvoice.controller.js`
  - `backend/src/controllers/financeSettings.controller.js`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/publicInvoice.controller.js`
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeInvoice.model.js`
  - `backend/src/routes/financeInvoice.route.js`
  - `backend/src/routes/internal.route.js`
  - `backend/src/routes/radiusAuthentication.route.js`
  - `backend/src/routes/settings.route.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/financeInvoiceClassifier.service.js`
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/option.service.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/src/services/waBroadcast.service.js`
  - `backend/src/utils/finance-error.js`
  - `backend/test/helpers/factories.js`
  - `backend/test/integration/financeInvoice.update.test.js`
  - `backend/test/integration/financeInvoiceClassifier.test.js`
  - `backend/test/integration/financeInvoiceWhatsapp.test.js`
  - `backend/test/unit/financeInvoiceTotal.test.js`
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`
  - `frontend/src/app/pages/finance/invoices/InvoiceDrawer.jsx`
  - `frontend/src/app/pages/finance/invoices/InvoiceItemsField.jsx`
  - `frontend/src/app/pages/finance/invoices/detail.jsx`
  - `frontend/src/app/pages/finance/invoices/payments/index.jsx`
  - `frontend/src/app/pages/finance/invoices/payments/schema/columns.jsx`
  - `frontend/src/app/pages/finance/invoices/schema/columns.jsx`
  - `frontend/src/app/pages/finance/invoices/schema/invoiceSchema.js`
  - `frontend/src/app/pages/finance/reports/index.jsx`
  - `frontend/src/app/pages/settings/schema/systemSchema.js`
  - `frontend/src/app/pages/settings/sections/Finance.jsx`
  - `frontend/src/app/pages/settings/sections/System.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Generator Tagihan Otomatis Langganan Broadband (`financeAutoInvoice.service.js` & `financeAutoInvoiceGenerate.js`)**:
    - Mengintegrasikan pembuatan tagihan otomatis harian untuk pelanggan broadband (`RadiusAuthentication`) dengan dua mode pemicu: mode bulanan (`auto_invoice: true` setiap tanggal 1) dan mode tanggal aktivasi (`invoice_on_activated` dengan penyesuaian akhir bulan otomatis).
    - Memastikan idempotensi pembuatan invoice via `client_ref` sehingga tidak terjadi tagihan ganda pada eksekusi berulang di periode yang sama.
    - Menghubungkan invoice yang terbit langsung ke `ref_auth` dan `off_on_expired` agar tersinkronisasi penuh dengan mekanisme isolir otomatis (`invoiceFreeze.js`).
  - **Isolir Otomatis Akun RADIUS Telat Bayar (`invoiceFreeze.js` & `api.service.js`)**:
    - Penjadwalan cron job harian pada pukul 01:30 di `cron-worker` untuk memicu endpoint backend `/internal/cron/invoice-freeze` guna menonaktifkan/mengisolir layanan pelanggan broadband yang tunggakannya melewati ambang batas (`max_invoice_expired`/`max_invoice_allowed`).
  - **Kontrol Sakelar Job Cron Worker (`cronSettings.service.js`, `System.jsx`, `settings.controller.js`)**:
    - Implementasi API dan antarmuka switch toggle on/off untuk tiap job pada tab `Settings > System > Cron Worker`, memungkinkan admin menonaktifkan job spesifik secara aman dari antarmuka web (misalnya saat migrasi atau sinkronisasi dengan V1).
  - **Pemetaan Variabel Template WhatsApp Billing Dinamis (`waBroadcast.service.js`, `financeInvoice.service.js`, `System.jsx`)**:
    - Penambahan `TemplateVariableMapper` untuk template WhatsApp tagihan (`variable_mapping_billing`) pada `Settings > System > WhatsApp`. Variabel body WhatsApp di-resolve secara dinamis sesuai pemetaan yang diatur admin dan nomor tujuan selalu dinormalisasi dengan `toWhatsappNumber`.
  - **Laporan Rekap PPN Keluaran (`TaxRecap.jsx` & `financeInvoiceTaxRecap.test.js`)**:
    - Pembuatan sub-menu laporan baru **Rekap PPN Keluaran** pada menu `Laporan Keuangan` (`/finance/reports`), menampilkan data transaksi faktur yang memuat DPP, nominal pajak, nomor Faktur Pajak, serta NPWP lawan transaksi.
  - **Perbaikan Tata Letak Form Pengaturan Tagihan (`Finance.jsx`)**:
    - Perbaikan layout grid pada tab Tagihan di `/settings/finance` dengan membungkus elemen `InputAppend` field **Jatuh Tempo Tagihan** (`inv_auto_due_day`) dan **Pajak Tagihan** (`inv_auto_tax`) ke dalam pembungkus `<div>` agar judul/label berada rapi di atas kolom input secara konsisten dengan field lainnya, serta perataan switch `inv_auto_tax_inclusive`.
  - **Suite Pengujian Terpadu (`backend/test/`)**:
    - Penambahan dan pembaruan unit test serta integration test untuk auto invoicing, rekap PPN, update faktur, klasifikasi faktur, kalkulasi total invoice, dan notifikasi WhatsApp invoice.

---

## 🌿 Branch: `issue-219` — Integrasi Notifikasi Tiket & Notifikasi Real-time Sistem

### 📌 Informasi Issue

- **Nomor Issue**: #219
- **Judul Issue**: Integrasi Notifikasi Tiket & Notifikasi Real-time Sistem
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [69cf939] - resolve #219 - 15 Agt 2026 12:17:19

- **Komponen yang Berubah**:
  - `backend/src/models/notification.model.js` [NEW]
  - `backend/src/routes/notification.route.js` [NEW]
  - `backend/src/services/notification.service.js` [NEW]
  - `backend/src/controllers/notification.controller.js` [NEW]
  - `backend/src/utils/stripHtml.util.js` [NEW]
  - `frontend/src/app/pages/notifications/index.jsx` [NEW]
  - `frontend/src/app/router/notificationsRoute.jsx` [NEW]
  - `frontend/src/utils/notificationFormatter.js` [NEW]
  - `frontend/src/utils/stripHtml.js` [NEW]
  - `radius-server/TUTORIAL_DEPLOY_PM2.md` [NEW]
  - `backend/src/app.js`
  - `backend/src/controllers/attendance.controller.js`
  - `backend/src/controllers/customerPO.controller.js`
  - `backend/src/controllers/customerSO.controller.js`
  - `backend/src/controllers/prospect.controller.js`
  - `backend/src/controllers/registration.controller.js`
  - `backend/src/controllers/ticket.controller.js`
  - `backend/src/controllers/warehouseItem.controller.js`
  - `backend/src/controllers/warehouseMutation.controller.js`
  - `backend/src/controllers/warehouseRequest.controller.js`
  - `backend/src/controllers/warehouseType.controller.js`
  - `backend/src/controllers/workOrder.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/services/ticket.service.js`
  - `backend/src/services/workOrder.service.js`
  - `backend/src/sockets/admin.controller.js`
  - `frontend/src/app/pages/activities/components/CreatePaidLeaveModal.jsx`
  - `frontend/src/app/pages/activities/components/CreatePermissionModal.jsx`
  - `frontend/src/app/pages/services/workOrder/WorkOrderDetailDrawer.jsx`
  - `frontend/src/app/pages/services/workOrder/detail.jsx`
  - `frontend/src/app/pages/users/components/UserAttendanceTabs.jsx`
  - `frontend/src/app/pages/users/privilege/create.jsx`
  - `frontend/src/app/pages/users/privilege/detail.jsx`
  - `frontend/src/app/pages/users/privilege/edit.jsx`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/template/Notifications.jsx`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Notifikasi Real-time Terpusat**:
    - Implementasi model, service, controller, dan socket handler untuk pencatatan dan distribusi notifikasi sistem secara real-time ke admin dan user terkait via WebSocket (Socket.io).
    - Integrasi pemicu notifikasi pada berbagai peristiwa kritis sistem: pembuatan & pembaruan Tiket Gangguan, Work Order, Customer PO/SO, Permintaan & Mutasi Gudang, Prospek & Registrasi Pelanggan, serta Izin/Cuti Karyawan.
  - **Antarmuka Notifikasi**:
    - Pembuatan halaman pusat notifikasi (`/notifications`) dengan fitur filter kategori, status baca, paginasi, dan aksi tandai telah dibaca (`mark as read`).
    - Penyempurnaan drawer/panel popover notifikasi pada navbar atas (`Notifications.jsx`) dengan badge hitungan belum dibaca secara langsung.
  - **Pengaturan Hak Akses (Privilege)**:
    - Penambahan key privilege dan konfigurasi peran untuk modul notifikasi (`notification.read`, `notification.update`, dsb.).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #216  | Perbaikan & Refactoring Modul Keuangan (Auto Invoicing, Tax Recap, Cron Control, WA Variables) | Tagihan broadband terbit otomatis dengan idempotensi terjamin, isolir akun telat bayar terjadwal otomatis, kontrol on/off job cron worker dari UI, laporan Rekap PPN Keluaran baru, serta fleksibilitas pemetaan variabel WhatsApp billing. |
| #219  | Integrasi Notifikasi Tiket & Notifikasi Real-time Sistem | Seluruh aktivitas operasional kritis (tiket, WO, gudang, prospek, PO/SO, absensi) kini memicu notifikasi real-time ke admin terkait melalui socket dan halaman pusat notifikasi. |

### Kemampuan Baru Pengguna/Admin

- Admin dapat mengaktifkan atau menonaktifkan masing-masing cron job worker secara mandiri melalui tombol switch di menu `Pengaturan > Sistem > Cron Worker`.
- Admin dapat mengatur pemetaan variabel template WhatsApp tagihan secara dinamis dan fleksibel via `TemplateVariableMapper` pada pengaturan sistem WhatsApp.
- Tim keuangan dapat mengakses laporan **Rekap PPN Keluaran** lengkap dengan informasi DPP, tarif/nominal PPN, nomor Faktur Pajak, dan NPWP pembeli.
- Admin dan staf operasional menerima pemberitahuan instan saat tiket dibuat/diedit, work order diterbitkan, mutasi barang gudang diajukan, atau cuti diajukan melalui panel notifikasi real-time.

### Bug Fix / Solusi Masalah

- **Pemberian Label Kolom pada Pengaturan Tagihan**: Memperbaiki masalah tampilan pada `/settings/finance` tab Tagihan di mana judul 'Jatuh Tempo Tagihan' dan 'Pajak Tagihan' sebelumnya terpisah ke samping kolom input akibat penggunaan `InputAppend` tanpa kontainer dalam grid; kini judul berada tepat di atas input kolom secara presisi.
- **Pencegahan Duplikasi Tagihan Otomatis**: Menghilangkan risiko tagihan ganda pada langganan broadband dengan penerapan kunci unik idempotensi `client_ref`.
- **Normalisasi Nomor WhatsApp**: Mengatasi kegagalan pengiriman pesan tagihan WhatsApp yang disebabkan oleh format nomor telepon lokal berawalan `0` atau simbol pemisah melalui fungsi `toWhatsappNumber`.

### Menu/Fitur Baru

- **Menu Laporan Rekap PPN Keluaran**: Tab baru di `/finance/reports` untuk rekapitulasi PPN Keluaran per transaksi faktur.
- **Halaman Notifikasi Sistem**: Halaman terpusat `/notifications` dan popover notifikasi header dengan filter status baca dan pembaruan real-time.
- **Switch Pengaturan Job Cron Worker**: Antarmuka kontrol individual untuk setiap job periodik di `/settings/system`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: **Auto Invoicing & Isolir Otomatis Pelanggan Broadband (`issue-216`)**
  Fitur ini mengotomatisasi siklus penerbitan tagihan bulanan pelanggan broadband dan penegakan isolir akun jika melewati masa tenggang pembayaran. Setiap hari pada pukul 01:00, cron worker mengevaluasi pelanggan broadband aktif (`auto_invoice` atau `invoice_on_activated`) dan menerbitkan faktur dengan `ref_auth` dan batas jatuh tempo terhitung. Selanjutnya pada pukul 01:30, cron worker mengecek faktur yang belum terbayar melewati jatuh tempo dan mengisolir akun yang melewati batas tunggakan (`max_invoice_allowed`).

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Pengaturan** > **Keuangan** > tab **Tagihan**.
  2. Atur **Jatuh Tempo Tagihan** (jumlah hari setelah terbit) dan persentase **Pajak Tagihan**.
  3. Buka menu **Pengaturan** > **Sistem** > tab **WhatsApp** untuk memilih template penagihan dan memetakan variabel pesan via `Template Variable Mapper`.
  4. Buka tab **Cron Worker** pada Pengaturan Sistem dan pastikan switch **Tagihan Otomatis (Auto Invoice Generate)** dan **Isolir Otomatis (Invoice Freeze)** dalam kondisi aktif (ON).
  5. Untuk memantau pajak tagihan yang telah terbit, buka menu **Keuangan** > **Laporan Keuangan** > tab **Rekap PPN Keluaran**.
