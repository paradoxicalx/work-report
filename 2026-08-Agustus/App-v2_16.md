# 📝 Daily Work Report - Dedy S.N Putra (2026-08-16)

---

## 📅 Laporan Harian - 16 Agustus 2026

---

## 🌿 Branch: `issue-217` — Pengembangan Modul Pengadaan & Pengeluaran Keuangan (RAB / Budgeting, Permintaan Belanja / Purchase Requests, Tagihan Pembelian & Pembayaran Beban, Jurnal Hutang Usaha / AP, Rekap PPN Masukan & Umur Hutang)

### 📌 Informasi Issue

- **Nomor Issue**: #217
- **Judul Issue**: Pengembangan Modul Pengadaan & Pengeluaran Keuangan (RAB / Budgeting, Permintaan Belanja / Purchase Requests, Tagihan Pembelian & Pembayaran Beban, Jurnal Hutang Usaha / AP, Rekap PPN Masukan & Umur Hutang)
- **Status Branch**: `Dalam Pengerjaan` (Work In Progress / Belum di-merge)

### 📅 Rincian Commit

#### [Work In Progress] - Penyempurnaan Standar Drawer, Kompatibilitas Data Warisan V1 & Perbaikan Translasi Pembayaran - 16 Agt 2026

- **Komponen yang Berubah**:
  - `backend/src/config/privilege.json`
  - `backend/src/models/financeExpense.model.js`
  - `backend/src/services/financeBudgeting.service.js`
  - `backend/src/services/financeExpenseOrder.service.js`
  - `backend/test/integration/financeExpenseV1Compat.test.js`
  - `frontend/src/app/navigation/finance.js`
  - `frontend/src/app/pages/finance/budgeting/BudgetingDrawer.jsx`
  - `frontend/src/app/pages/finance/budgeting/detail.jsx`
  - `frontend/src/app/pages/finance/budgeting/schema/columns.jsx`
  - `frontend/src/app/pages/finance/expenses/BillPaymentDrawer.jsx`
  - `frontend/src/app/pages/finance/expenses/ExpenseDrawer.jsx`
  - `frontend/src/app/pages/finance/expenses/detail.jsx`
  - `frontend/src/app/pages/finance/expenses/schema/columns.jsx`
  - `frontend/src/app/pages/finance/purchase-requests/PurchaseRequestDrawer.jsx`
  - `frontend/src/app/pages/finance/purchase-requests/detail.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Penyelarasan Standar Desain Drawer & Dialog Detail**:
    - Memperbarui antarmuka drawer pada `BudgetingDrawer.jsx`, `ExpenseDrawer.jsx`, `PurchaseRequestDrawer.jsx`, serta drawer detail masing-masing (`detail.jsx`) agar mengikuti standar drawer monorepo (overlay backdrop blur, transisi animasi halus `translate-x-full` / `translate-x-0`, lebar responsif `md:w-[65vw]` dan `lg:w-[50vw]`, serta header berlatar abu-abu yang terintegrasi rapi dengan tombol aksi dan tombol tutup).
  - **Dukungan Penuh Data Warisan V1 & Perlindungan Integritas**:
    - Menambahkan deklarasi eksplisit `created_by` pada model Mongoose `FinanceExpense` guna mencegah `StrictPopulateError` saat query populate dilakukan pada endpoint list maupun detail.
    - Menghapus filter server `{ pid: 'master' }` pada `findListFinanceBudgetingForTable` dan `findListFinanceExpenseOrderForTable` karena dokumen warisan V1 tidak memiliki field `pid`, sehingga seluruh data riwayat lama tetap muncul lengkap pada tabel.
    - Menambahkan helper `resolveLegacyExpenseBillStatus` di `rows.jsx` untuk menurunkan status tampilan yang akurat (`paid`, `approved`, `partial`, `void`, `waiting`) dari kombinasi field warisan V1 (`canceled`, `status`, `proccess`, `paid_total`) agar tagihan lama yang sudah lunas tidak keliru ditampilkan sebagai draf.
    - Menambahkan flag `isLegacy` pada `detail.jsx` tagihan pembelian untuk menonaktifkan tombol aksi alur V2 (submit, approve, reject, pay, void) pada dokumen warisan V1 yang tidak memiliki struktur item atau jurnal AP lengkap.
  - **Perbaikan Translasi Opsi Metode Pembayaran Tagihan**:
    - Memperbaiki rujukan key translasi pada `BillPaymentDrawer.jsx` dari `finance.payment.method.*` menjadi `finance.payment.methodOptions.*` (`cash`, `transfer`, `other`) untuk menghilangkan log peringatan missing key i18n.
    - Memperbarui tabel riwayat pembayaran di `detail.jsx` agar menampilkan label metode pembayaran terlokalisasi via `t('finance.payment.methodOptions.' + payment.method)`.

---

#### [8fa20e8] - save #217 - 16 Agt 2026 19:02:37

- **Komponen yang Berubah**:
  - `backend/src/models/financeBudgeting.model.js` [NEW]
  - `backend/src/models/financeExpense.model.js` [NEW]
  - `backend/src/models/financeExpenseOrder.model.js` [NEW]
  - `backend/src/routes/financeBudgeting.route.js` [NEW]
  - `backend/src/routes/financeExpense.route.js` [NEW]
  - `backend/src/routes/financeExpenseOrder.route.js` [NEW]
  - `backend/src/services/financeBudgeting.service.js` [NEW]
  - `backend/src/services/financeExpense.service.js` [NEW]
  - `backend/src/services/financeExpenseOrder.service.js` [NEW]
  - `backend/test/integration/financeExpense.create.test.js` [NEW]
  - `backend/test/integration/financeExpense.payment.race.test.js` [NEW]
  - `backend/test/integration/financeExpense.payment.test.js` [NEW]
  - `backend/test/integration/financeExpenseAP.journal.test.js` [NEW]
  - `backend/test/integration/financeExpensePayableAging.test.js` [NEW]
  - `backend/test/integration/financeExpenseV1Compat.test.js` [NEW]
  - `frontend/src/app/pages/finance/budgeting/BudgetingDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/BudgetingItemsField.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/detail.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/index.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/schema/budgetingSchema.js` [NEW]
  - `frontend/src/app/pages/finance/budgeting/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/BillPaymentDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/ExpenseDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/ExpenseItemsField.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/detail.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/index.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/schema/expenseSchema.js` [NEW]
  - `frontend/src/app/pages/finance/expenses/schema/paymentSchema.js` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/PurchaseRequestDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/PurchaseRequestItemsField.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/detail.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/index.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/schema/purchaseRequestSchema.js` [NEW]
  - `frontend/src/app/pages/finance/reports/InputTaxRecap.jsx` [NEW]
  - `frontend/src/app/pages/finance/reports/PayableAging.jsx` [NEW]
  - `frontend/src/app/router/finance/budgeting.jsx` [NEW]
  - `frontend/src/app/router/finance/expenses.jsx` [NEW]
  - `frontend/src/app/router/finance/purchaseRequests.jsx` [NEW]
  - `backend/src/models/financeJournal.model.js`
  - `backend/src/models/financeLogs.model.js`
  - `backend/src/services/changelog.service.js`
  - `backend/src/services/financeAutoInvoice.service.js`
  - `backend/src/services/financeCoa.service.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/financeJournal.service.js`
  - `backend/src/services/notification.service.js`
  - `backend/src/utils/finance-error.js`
  - `frontend/src/app/navigation/finance.js`
  - `frontend/src/app/pages/finance/invoices/payments/index.jsx`
  - `frontend/src/app/pages/finance/reports/index.jsx`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Modul Rencana Anggaran Biaya / RAB (`financeBudgeting`)**:
    - Backend: Model `FinanceBudgeting`, service, dan controller untuk mengelola pengajuan RAB berkala/proyek dengan alur status persetujuan (`draft` -> `waiting` -> `approved` / `rejected`).
    - Frontend: Halaman `/finance/budgeting`, drawer form pembuatan/edit RAB dengan rincian baris item dinamis, drawer detail dengan tampilan ringkasan anggaran, serta aksi pengajuan dan persetujuan.
  - **Modul Permintaan Belanja / Purchase Requests (`financeExpenseOrder`)**:
    - Backend: Model `FinanceExpenseOrder`, service, dan controller untuk mencatat permintaan pengadaan barang/jasa dari tim operasional/teknis dengan status (`waiting` -> `accept` / `decline`, serta flag `complete`).
    - Frontend: Halaman `/finance/purchase-requests`, drawer form permintaan belanja, drawer detail untuk review pimpinan, tombol persetujuan/penolakan, serta tombol pintasan langsung pembuatan Tagihan Pembelian dari permintaan yang sudah disetujui.
  - **Modul Tagihan Pembelian & Pembayaran Beban (`financeExpense`)**:
    - Backend: Model `FinanceExpense` untuk menampung tagihan dari vendor atau pengeluaran operasional. Mendukung 3 pilihan sumber anggaran: dari RAB (`source_type: budgeting`), dari Permintaan Belanja (`source_type: request`), atau Pembelian Langsung (`source_type: direct`).
    - Otomatisasi Jurnal Hutang Usaha (AP): Saat tagihan disetujui (`approved`), sistem otomatis membentuk entri jurnal akrual (Debit: Akun Beban/Aset/Persediaan per item, Debit: PPN Masukan jika ada, Kredit: Hutang Usaha / AP Vendor). Saat tagihan dibatalkan (`void`), jurnal pembalik otomatis dibuat.
    - Pembayaran Tagihan: Endpoint `/finance/expense/:expense_id/pay` untuk mencatat pembayaran bertahap (parsial) atau lunas. Sistem otomatis membentuk jurnal pengeluaran kas (Debit: Hutang Usaha, Kredit: Kas/Bank) dan memutakhirkan `paid_total` serta status tagihan (`partial`/`paid`). Dilengkapi proteksi race condition / atomisitas transaksi.
  - **Laporan Rekap PPN Masukan & Jadwal Umur Hutang (Payable Aging)**:
    - Backend & Frontend: Pembuatan endpoint dan antarmuka laporan baru pada `/finance/reports`:
      - **Rekap PPN Masukan (`InputTaxRecap.jsx`)**: Menampilkan daftar faktur pembelian yang memuat PPN Masukan, nomor faktur pajak, DPP, dan identitas lawan transaksi untuk pelaporan SPT Masa PPN.
      - **Jadwal Umur Hutang (`PayableAging.jsx`)**: Mengelompokkan kewajiban hutang yang belum lunas ke dalam bucket umur jatuh tempo (Belum Jatuh Tempo, 1-30 Hari, 31-60 Hari, 61-90 Hari, >90 Hari).
  - **Pengujian Komprehensif (Integration & Race Condition Tests)**:
    - Penambahan test suite Vitest di `backend/test/integration/`: pengujian pembuatan tagihan, pembentukan jurnal AP, pembayaran bertahap, pencegahan overpayment pada eksekusi konkuren (race condition test), kalkulasi umur hutang, serta pengujian kompatibilitas data warisan V1.

---

## 🌿 Branch: `issue-216` — Perbaikan & Refactoring Modul Keuangan (Finance Invoices, Payments, Auto Invoicing, Tax Recap, WhatsApp Variable Mapping, Cron Worker Job Toggles & Layout Pengaturan)

### 📌 Informasi Issue

- **Nomor Issue**: #216
- **Judul Issue**: Perbaikan & Refactoring Modul Keuangan (Finance Invoices, Payments, Auto Invoicing, Tax Recap, WhatsApp Variable Mapping, Cron Worker Job Toggles & Layout Pengaturan)
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [977c604] - resolve #216 - 16 Agt 2026 11:41:11

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
  - **Generator Tagihan Otomatis Pelanggan Broadband (`financeAutoInvoice.service.js` & `financeAutoInvoiceGenerate.js`)**:
    - Implementasi engine pembuatan faktur tagihan berkala otomatis untuk akun broadband `RadiusAuthentication` dengan opsi mode tanggal 1 bulanan (`auto_invoice`) maupun tanggal aktivasi (`invoice_on_activated`). Dilengkapi kunci idempotensi `client_ref` untuk mencegah duplikasi tagihan.
  - **Isolir Otomatis Akun Menunggak (`invoiceFreeze.js`)**:
    - Implementasi processor job harian di `cron-worker` untuk mengecek dan menonaktifkan akun broadband yang memiliki tunggakan faktur melebihi batas toleransi.
  - **Sakelar Kontrol Job Cron Worker di Web UI (`cronSettings.service.js`, `System.jsx`)**:
    - Fasilitas toggle sakelar aktif/nonaktif untuk setiap cron job langsung dari halaman `Pengaturan > Sistem > Cron Worker`.
  - **Pemetaan Variabel Template WhatsApp Billing (`waBroadcast.service.js`, `System.jsx`)**:
    - Penyusunan mapper variabel dinamis untuk format pesan penagihan via WhatsApp dengan normalisasi nomor tujuan otomatis.
  - **Laporan Rekap PPN Keluaran (`TaxRecap.jsx`)**:
    - Menu laporan baru untuk merekap DPP, pajak, dan nomor Faktur Pajak dari tagihan penjualan yang diterbitkan.
  - **Perapian Layout Pengaturan Keuangan (`Finance.jsx`)**:
    - Perbaikan tampilan input jatuh tempo dan persentase pajak tagihan pada tab form `/settings/finance`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #217  | Pengembangan Modul Pengadaan & Pengeluaran Keuangan (RAB, Purchase Requests, Purchase Bills, Jurnal Hutang, Rekap PPN Masukan, Umur Hutang) | Memperluas sistem keuangan Dekasimal V2 dengan siklus pengeluaran lengkap (Procure-to-Pay): perencanaan anggaran (RAB), permintaan barang/jasa internal, pencatatan tagihan vendor, jurnal akrual hutang usaha otomatis, pembayaran kas bertahap, serta laporan kepatuhan pajak masukan dan manajemen likuiditas hutang. |
| #216  | Perbaikan & Refactoring Modul Keuangan (Invoices, Payments, Auto Invoicing, Tax Recap, WhatsApp Mapping, Cron Worker Toggles) | Otomatisasi penagihan langganan internet harian dan isolir akun menunggak, laporan rekap PPN keluaran, fleksibilitas integrasi pesan tagihan WhatsApp, dan kendali operasional cron worker dari UI admin. |

### Kemampuan Baru Pengguna/Admin

- **Pengajuan & Persetujuan RAB**: Tim proyek/divisi dapat membuat rincian anggaran biaya dan mengajukannya ke manajemen untuk disetujui sebelum pengadaan dilakukan.
- **Permintaan Pengadaan Terstruktur**: Karyawan/teknisi dapat mengajukan permintaan belanja barang/jasa dan melacak status persetujuan dari pimpinan.
- **Pencatatan & Pelunasan Tagihan Pembelian**: Tim keuangan dapat mencatat tagihan masuk dari vendor (baik berdasarkan RAB, permintaan belanja, atau pembelian langsung), mencatat pembayaran bertahap/lunas melalui akun kas/bank terpilih, serta membatalkan pembayaran secara aman.
- **Otomatisasi Akuntansi Hutang Usaha (AP)**: Setiap tagihan pembelian yang disetujui langsung membentuk jurnal akrual hutang usaha, dan pelunasan kas otomatis mengurangi saldo hutang vendor tanpa perlu entri jurnal manual.
- **Analisis Umur Hutang (Payable Aging)**: Admin dapat memonitor jatuh tempo seluruh kewajiban pembayaran tagihan berdasarkan kelompok hari keterlambatan guna menjaga arus kas perusahaan.
- **Rekap PPN Masukan & Keluaran**: Admin pajak/keuangan dapat mengekspor dan meninjau seluruh transaksi yang memuat PPN Masukan maupun PPN Keluaran untuk kebutuhan rekonsiliasi SPT Masa PPN.

### Bug Fix / Solusi Masalah

- **StrictPopulateError pada List/Detail Tagihan**: Memperbaiki ketiadaan field `created_by` pada skema `FinanceExpense` yang sebelumnya menyebabkan kegagalan populate admin pembuat tagihan.
- **Data Riwayat Lama V1 Kosong pada Tabel**: Menghapus filter server `{ pid: 'master' }` pada query datatable RAB dan Permintaan Belanja yang sebelumnya menyaring habis dokumen warisan V1.
- **Status Tagihan Legacy Terbaca Salah**: Menyediakan helper `resolveLegacyExpenseBillStatus` agar tagihan warisan V1 yang sudah lunas/disetujui di V1 tidak keliru ditampilkan sebagai draf di V2.
- **Missing Translation Key pada Metode Pembayaran Tagihan**: Memperbaiki rujukan key i18n opsi metode pembayaran di `BillPaymentDrawer.jsx` dan tampilan detail riwayat pembayaran.
- **Pencegahan Overpayment Konkuren**: Implementasi kunci dan pengecekan saldo tersisa pada service pembayaran tagihan untuk mencegah double-payment saat beberapa transaksi dieksekusi bersamaan.

### Menu/Fitur Baru

- **Menu `Keuangan > Rencana Anggaran` (`/finance/budgeting`)**: Manajemen dokumen RAB dan persetujuannya.
- **Menu `Keuangan > Permintaan Belanja` (`/finance/purchase-requests`)**: Manajemen permintaan pengadaan barang/jasa internal.
- **Menu `Keuangan > Tagihan Pembelian` (`/finance/expenses`)**: Manajemen tagihan vendor, beban pengeluaran, dan pencatatan pembayaran cicilan/lunas.
- **Sub-menu `Keuangan > Laporan Keuangan > Rekap PPN Masukan` (`/finance/reports` tab PPN Masukan)**: Laporan faktur pembelian berpajak.
- **Sub-menu `Keuangan > Laporan Keuangan > Umur Hutang` (`/finance/reports` tab Umur Hutang)**: Jadwal analisis jatuh tempo hutang usaha vendor.
- **Sub-menu `Keuangan > Laporan Keuangan > Rekap PPN Keluaran` (`/finance/reports` tab PPN Keluaran)**: Laporan faktur penjualan berpajak.
- **Pengaturan `Sistem > Cron Worker`**: Sakelar on/off untuk penjadwalan job background sistem.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: **Alur Pengeluaran & Tagihan Pembelian (Procure-to-Pay)**
  - Modul Tagihan Pembelian memungkinkan pencatatan beban atau pembelian perlengkapan dengan menghubungkan sumber anggaran (RAB yang telah disetujui, Permintaan Belanja yang telah di-accept, atau Pembelian Langsung). Saat tagihan diajukan dan disetujui oleh admin berwenang, sistem secara otomatis membukukan Jurnal Hutang Usaha (AP). Pembayaran tagihan dapat dicatat secara bertahap atau sekaligus ke akun Kas/Bank yang dituju, yang akan otomatis membukukan pengeluaran kas dan mengurangi saldo tagihan hingga status berubah menjadi **Lunas**.

- **Langkah Penggunaan (Tutorial)**:
  1. **Mencatat Tagihan Baru**:
     - Buka menu **Keuangan > Tagihan Pembelian** (`/finance/expenses`).
     - Klik tombol **Tambah Tagihan** di sudut kanan atas.
     - Pilih **Sumber Tagihan**:
       - *RAB*: Pilih dokumen RAB yang telah disetujui.
       - *Permintaan Belanja*: Pilih nomor permintaan pengadaan yang telah disetujui.
       - *Langsung*: Masukkan nama penjual/vendor secara manual.
     - Isi nama tagihan, tanggal jatuh tempo, dan rincian baris item (nama barang, kuantitas, harga satuan, akun perkiraan/COA beban, dan status pajak).
     - Klik **Simpan**. Tagihan akan berstatus **Draf**.
  2. **Pengajuan & Persetujuan**:
     - Buka baris tagihan untuk membuka drawer detail.
     - Klik **Ajukan** untuk mengubah status menjadi **Menunggu**.
     - Admin dengan hak akses persetujuan dapat mengklik **Setujui** (jurnal hutang usaha akan otomatis terbentuk).
  3. **Pencatatan Pembayaran**:
     - Pada tagihan berstatus **Disetujui** atau **Dibayar Sebagian**, klik tombol **Catat Pembayaran** di drawer detail.
     - Pilih **Akun Kas/Bank Penerima/Pengeluar**, masukkan **Nominal Pembayaran**, pilih **Metode Pembayaran** (Transfer / Tunai / Lainnya), serta tanggal transaksi.
     - Klik **Catat Pembayaran**. Status tagihan akan otomatis terbarui menjadi **Dibayar Sebagian** atau **Lunas**, dan riwayat pembayaran akan tercatat rapi di tabel drawer detail.
