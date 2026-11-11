# 📝 Daily Work Report - Dedy S.N Putra (2026-10-29)

---

## 📅 Laporan Harian - 29 Oktober 2026

---

## 🌿 Branch: `issue-486` — Abstraksi Multi Payment Gateway, Adaptor Xendit (Fase 0 & Fase 1a), dan Pemisahan Jurnal PPN Masukan Fee Gateway

### 📌 Informasi Issue

- **Nomor Issue**: #486
- **Judul Issue**: Abstraksi Multi Payment Gateway, Adaptor Xendit Payments API v3, Kebijakan PPN Fee, dan Penguatan Pembukuan Akun Gateway (Payment Gateway Provider Abstraction, Xendit Adapter, Fee VAT Allocation, and Scoped Finance Accounts)
- **Status Branch**: `Belum di-merge` (Ahead 4 commit dari `origin/issue-486`, menunggu finalisasi pengujian menyeluruh sebelum digabungkan ke `master`)

### 📅 Rincian Commit

#### [fbc1a6ac] - save #486 fee_booked diturunkan dari cicilan yang dijurnal - 29 Oktober 2026, 22:40:26 WIB

- **Komponen yang Berubah**:
  - `backend/src/models/financePayment.model.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/test/integration/xenditGateway.e2e.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Pencatatan Porsi Fee & PPN pada Cicilan Pembayaran**: Menambahkan field `fee_journaled` dan `fee_tax_journaled` pada subskema `PaymentInstallmentSchema` di `financePayment.model.js`. Field ini sengaja tanpa nilai default agar data pembayaran lama tetap terlindungi integritasnya dan dapat dibedakan dengan jelas saat proses kalkulasi.
  - **Kalkulasi Akurat `fee_booked` Berbasis Cicilan Riil**: Menambahkan fungsi pembantu `sumJournaledGatewayFees` pada `financePayment.service.js` untuk menjumlahkan seluruh fee dan porsi PPN masukan yang benar-benar masuk ke jurnal umum dari cicilan faktur terkait. Nilai `fee_booked` dan `fee_tax_booked` pada `GatewayTransaction` kini bersumber langsung dari hasil agregasi cicilan riil, bukan lagi sekadar menyalin estimasi nominal callback.
  - **Penskalaan Proporsional PPN Masukan pada Batch Settlement**: Memperbaiki alokasi PPN Masukan pada penyelesaian banyak faktur sekaligus (`performBatchSettlement`). Apabila total fee item terpotong oleh sisa tagihan faktur, nominal PPN Masukan batch diskalakan secara proporsional sehingga rasio beban bank dan PPN Masukan tiap faktur tetap seimbang dan akurat terhadap transaksi induknya.
  - **Pengujian Integrasi Ujung-ke-Ujung**: Menambahkan pengujian komprehensif pada `xenditGateway.e2e.test.js` untuk memverifikasi pencatatan `fee_journaled`, `fee_tax_journaled`, serta nilai `fee_booked` transaksi setelah penyelesaian pembayaran tunggal maupun pembayaran batch.

---

#### [3851cea1] - save #486 pembukuan fee: pisahkan PPN Masukan sesuai kebijakan - 29 Oktober 2026, 22:26:40 WIB

- **Komponen yang Berubah**:
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/src/services/paymentGateways/xendit.gateway.js`
  - `backend/test/integration/xenditGateway.e2e.test.js`
  - `backend/test/unit/financeGateway.allocateFeeTax.test.js` [NEW]
  - `backend/test/unit/xenditGateway.normalize.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Pemisahan Jurnal PPN Masukan atas Biaya Gateway**: Memperbarui fungsi pembukuan jurnal cicilan `writeInstallmentJournal` pada `financePayment.service.js`. Apabila biaya gateway ditanggung merchant (`fee_direction === 'MERCHANT'`) dan kebijakan PPN gateway disetel ke `input_vat`, porsi pajak fee (`gatewayFeeTax`) dijurnal terpisah ke akun PPN Masukan (COA `1-6100`), sementara sisanya dijurnal ke Beban Administrasi Bank (COA `8-1000`).
  - **Helper Resolusi Pajak Fee Gateway**: Membuat fungsi `resolveBookedFeeTax` untuk menghitung nominal PPN Masukan yang sah dijurnal (memastikan nilai dibatasi antara `0` hingga `fee`) sesuai kebijakan vendor aktif. Untuk vendor tanpa kebijakan PPN terpisah (seperti iPaymu), seluruh biaya tetap masuk ke beban bank tanpa perubahan perilaku.
  - **Penambahan Field Audit Pajak Transaksi**: Menambahkan field `fee_tax_booked` pada schema `FinanceGatewayTransaction` untuk merekam jumlah nominal PPN Masukan yang telah dibukukan ke jurnal umum.
  - **Distribusi PPN Masukan Multi Faktur**: Menambahkan algoritma pembagian proporsional pajak fee (`allocateFeeTax`) pada `financeGateway.service.js` dengan pengujian unit terisolasi pada `financeGateway.allocateFeeTax.test.js`.

---

#### [088a8078] - save #486 pengaturan kebijakan PPN fee Xendit - 29 Oktober 2026, 22:12:12 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/financeSettings.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/financeSettings.route.js`
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/settingsBackup.registry.js`
  - `backend/test/integration/financeSettings.xendit.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Konfigurasi Kebijakan PPN Fee Xendit**: Menambahkan opsi pengaturan `xendit_fee_vat_policy` pada konfigurasi keuangan (`FinanceSetting`) dengan pilihan nilai enum `include_expense` (biaya digabung ke beban bank) atau `input_vat` (porsi PPN dipisah ke akun PPN Masukan).
  - **Validasi & Sanitasi Endpoint Pengaturan**: Menambahkan validasi nilai enum pada controller `updateXenditFinanceSettings` dan route `PATCH /finance/settings/xendit`.
  - **Dukungan Pencadangan & Pemulihan Sistem**: Memperbarui `settingsBackup.registry.js` agar field kebijakan PPN `xendit_fee_vat_policy` disertakan saat ekspor cadangan pengaturan keuangan tanpa mengekspos kredensial rahasia gateway.
  - **Pengujian Integrasi**: Menambahkan skenario pengujian pada `financeSettings.xendit.test.js` untuk memvalidasi penyimpanan, pembacaan, dan penolakan nilai enum kebijakan PPN yang tidak valid.

---

#### [1cc36a2d] - resolve #486 - 29 Oktober 2026, 21:13:58 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/xenditChannels.json` [NEW]
  - `backend/src/controllers/financeGateway.controller.js`
  - `backend/src/controllers/financeSettings.controller.js`
  - `backend/src/controllers/publicPayment.controller.js`
  - `backend/src/controllers/setup.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeAccount.model.js`
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/routes/financeGateway.route.js`
  - `backend/src/routes/financeSettings.route.js`
  - `backend/src/routes/public.route.js`
  - `backend/src/services/financeAccount.service.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/paymentGateway.service.js`
  - `backend/src/services/paymentGateways/ipaymu.gateway.js`
  - `backend/src/services/paymentGateways/registry.js`
  - `backend/src/services/paymentGateways/xendit.client.js` [NEW]
  - `backend/src/services/paymentGateways/xendit.gateway.js` [NEW]
  - `backend/src/services/paymentGateways/xenditCatalog.js` [NEW]
  - `backend/src/services/publicPaymentTarget.service.js`
  - `backend/src/services/settingsBackup.registry.js`
  - `backend/src/utils/gatewayCallMeter.js`
  - `backend/src/utils/undefinedFilter.js` [NEW]
  - `backend/test/helpers/fakeGateway.js` [NEW]
  - `backend/test/integration/financeAccount.gatewayProvision.test.js` [NEW]
  - `backend/test/integration/financeAccount.undefinedFilter.test.js` [NEW]
  - `backend/test/integration/financeGateway.balanceSummary.test.js` [NEW]
  - `backend/test/integration/financeGateway.callback.test.js`
  - `backend/test/integration/financeGateway.callbackEvents.test.js` [NEW]
  - `backend/test/integration/financeGateway.multiGateway.test.js` [NEW]
  - `backend/test/integration/financeGateway.reconCapability.test.js` [NEW]
  - `backend/test/integration/financeGateway.reconGatewayUnavailable.test.js` [NEW]
  - `backend/test/integration/financeGateway.refundReversal.test.js`
  - `backend/test/integration/financeGateway.settle.batch.test.js`
  - `backend/test/integration/financeGateway.settle.test.js`
  - `backend/test/integration/financeGateway.sweep.test.js`
  - `backend/test/integration/financeSettings.xendit.test.js` [NEW]
  - `backend/test/integration/liveInvoice.test.js`
  - `backend/test/integration/paymentGateway.facade.test.js` [NEW]
  - `backend/test/integration/publicPayment.test.js`
  - `backend/test/integration/xenditClient.test.js` [NEW]
  - `backend/test/integration/xenditGateway.createPayment.test.js` [NEW]
  - `backend/test/integration/xenditGateway.e2e.test.js` [NEW]
  - `backend/test/unit/gatewayCallMeter.test.js` [NEW]
  - `backend/test/unit/ipaymuGateway.normalize.test.js` [NEW]
  - `backend/test/unit/paymentGatewayRegistry.test.js` [NEW]
  - `backend/test/unit/undefinedFilter.test.js` [NEW]
  - `backend/test/unit/xenditCatalog.test.js` [NEW]
  - `backend/test/unit/xenditGateway.normalize.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/Finance.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Abstraksi Multi-Provider Payment Gateway (Fase 0)**:
    - Merancang standarisasi kontrak adaptor payment gateway (`normalizeTransaction`, `parseCallback`, `capabilities`, `requiresCallbackToken`, `isConfigured`) dan registri gateway (`registry.js`) dengan validasi kontrak ketat serta pencegahan ID duplikat.
    - Menjaga backward compatibility penuh terhadap iPaymu tanpa merusak perilaku sistem yang sudah berjalan.
    - Setiap transaksi kini diverifikasi ke provider miliknya masing-masing (`trx.gateway`), bukan terikat ke gateway aktif global saat verifikasi berjalan.
    - Mengisolasi akun Kas & Bank gateway menggunakan field `gateway_id` berindeks, menambahkan penjaga terhadap konfigurasi VA kosong saat booting, serta membuat utilitas `undefinedFilter.js` untuk mencegah bug query pada MongoDB.
    - Melakukan refaktor background sweep dan rekonsiliasi berkala agar mengecek kapabilitas provider (`capabilities.history`), sehingga provider tanpa endpoint mutasi riwayat tidak menyebabkan error sistem.
  - **Implementasi Adaptor Xendit Payments API v3 (Fase 1a)**:
    - Mengimplementasikan modul adaptor `xendit.gateway.js`, klien HTTP terisolasi `xendit.client.js` (dengan penonaktifan retry pembuatan transaksi guna mencegah double billing), dan katalog channel `xenditCatalog.js` untuk channel Virtual Account (BCA, BNI, BRI, Mandiri, Permata, BSI), QRIS Dinamis, dan Retail Outlet (Alfamart, Indomaret).
    - Membangun endpoint penerimaan webhook callback Xendit `/finance/gateway/xendit/callback` dengan otentikasi token aman (`x-callback-token`), mitigasi webhook asing melalui metadata `source === 'dekasimal'`, penanganan event standar, serta penanda status biaya `fee_pending`.
    - Menyediakan endpoint konfigurasi kredensial Xendit (`/finance/settings/xendit`) dengan prinsip write-only secret (kunci rahasia disamarkan `••••••` saat dibaca dan dikecualikan dari backup data), serta penjaga aktivasi gateway ketat (Xendit diblokir untuk diaktifkan di production hingga Fase 1b selesai, dengan bypass flag lingkungan `XENDIT_ACTIVATION_ENABLED=true` khusus pengujian dev).
    - Menambahkan provisi otomatis akun Kas & Bank untuk Xendit (`xendit` dan `xendit-sandbox`).
    - Mengharmonisasi UI pengaturan keuangan frontend dan ringkasan saldo gateway untuk menyembunyikan provider yang belum dikonfigurasi.

---

## 🌿 Branch: `master` / `issue-490` — Berita Acara Perubahan Layanan (BAPL) untuk Pelanggan Broadband

### 📌 Informasi Issue

- **Nomor Issue**: #490
- **Judul Issue**: Berita Acara Perubahan Layanan (BAPL, `service_change_documents`) untuk Pelanggan Broadband
- **Status Branch**: `Sudah di-merge` (ke `master` via commit merge `d014a26b`)

### 📅 Rincian Commit

#### [d014a26b] - resolve #490 - 29 Oktober 2026, 23:09:38 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/customer.controller.js`
  - `backend/src/controllers/serviceChangeDocument.controller.js` [NEW]
  - `backend/src/data/aiNavigationRoutes.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/middlewares/logger.middleware.js`
  - `backend/src/models/serviceChangeDocument.model.js` [NEW]
  - `backend/src/routes/public.route.js`
  - `backend/src/routes/serviceChangeDocument.route.js` [NEW]
  - `backend/src/services/option.service.js`
  - `backend/src/services/serviceChangeDocument.service.js` [NEW]
  - `backend/src/services/serviceChangeRequest.service.js`
  - `backend/test/integration/serviceChangeDocument.test.js` [NEW]
  - `backend/test/integration/serviceChangeProcess.test.js`
  - `backend/test/integration/serviceChangeRequest.admin.test.js`
  - `backend/test/integration/serviceChangeTerms.test.js`
  - `frontend/src/app/navigation/users.js`
  - `frontend/src/app/pages/mobileApp/serviceChange/components/ReviewDrawer.jsx`
  - `frontend/src/app/pages/mobileApp/serviceChange/components/WebRequestDrawer.jsx`
  - `frontend/src/app/pages/mobileApp/serviceChange/index.jsx`
  - `frontend/src/app/pages/public/PublicServiceChangeDocument.jsx` [NEW]
  - `frontend/src/app/pages/public/serviceChangeRequest.jsx`
  - `frontend/src/app/pages/settings/schema/applicationSchema.js`
  - `frontend/src/app/pages/settings/schema/applicationSchema.test.js`
  - `frontend/src/app/pages/settings/sections/Application.jsx`
  - `frontend/src/app/pages/users/document/registry.js`
  - `frontend/src/app/pages/users/document/serviceChange/CreateDrawer.jsx` [NEW]
  - `frontend/src/app/pages/users/document/serviceChange/ServiceChangeDocumentPanel.jsx` [NEW]
  - `frontend/src/app/pages/users/document/serviceChange/ServiceChangeDocumentView.jsx` [NEW]
  - `frontend/src/app/pages/users/document/serviceChange/index.jsx` [NEW]
  - `frontend/src/app/pages/users/document/serviceChange/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/users/document/serviceChange/schema/createSchema.js` [NEW]
  - `frontend/src/app/pages/users/document/serviceChange/useShareDocument.js` [NEW]
  - `frontend/src/app/pages/users/document/serviceChange/utils.js` [NEW]
  - `frontend/src/app/pages/users/document/serviceChange/utils.test.js` [NEW]
  - `frontend/src/app/router/public.jsx`
  - `frontend/src/app/router/users/documentRoute.jsx`
  - `frontend/src/components/shared/DocumentPreviewModal.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/main.jsx`
  - `frontend/src/styles/app/vendors/quill.css` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Modul Berita Acara Perubahan Layanan (BAPL)**: Mengimplementasikan sistem pembuatan, penomoran, peninjauan, dan pengesahan Berita Acara Perubahan Layanan (BAPL) untuk pelanggan broadband saat melakukan upgrade/downgrade paket internet.
  - **Multi-Sumber Dokumen BAPL**: BAPL dapat diterbitkan dari tiga saluran (`web`, `mobile`, dan `manual` via menu Dokumen), dengan penyusunan salinan data pelanggan dan spesifikasi paket dilakukan secara aman di sisi server (`resolveSource`).
  - **Tanda Tangan Elektronik Publik**: Menyediakan tautan publik berbasis token acak 192 bit (`share_token`) dan halaman web responsif (`PublicServiceChangeDocument.jsx`) agar pelanggan dapat membubuhkan tanda tangan langsung dari smartphone mereka dengan rate limiting dan validasi payload gambar tanda tangan ketat.
  - **Proteksi Integritas Dokumen Legal**: Dokumen yang sudah berstatus `signed` terkunci secara permanen (menolak perubahan atau penghapusan dengan status HTTP 422), dan masa aktif kontrak 12 bulan dihitung ulang otomatis sejak tanggal efektif perubahan.
  - **Kustomisasi Syarat & Ketentuan Dinamis**: Mendukung pengaturan template syarat dan ketentuan BAPL via Pengaturan Aplikasi dengan editor teks kaya (Quill) dan penanda versi syarat (`terms_version`).

---

## 🌿 Branch: `master` / `issue-493` — Potongan Biaya (Deduction) pada Pembayaran Faktur Manual & Kwitansi

### 📌 Informasi Issue

- **Nomor Issue**: #493
- **Judul Issue**: Penambahan Potongan Biaya (Deduction) pada Pembayaran Faktur Manual & Kwitansi Pembayaran
- **Status Branch**: `Sudah di-merge` (ke `master` via commit merge `907251bf`)

### 📅 Rincian Commit

#### [907251bf] - resolve #493 - 29 Oktober 2026, 10:19:20 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/financePayment.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financePayment.model.js`
  - `backend/src/routes/financePayment.route.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/src/utils/paymentCorrectionInput.js`
  - `backend/test/integration/financePayment.correction.test.js`
  - `backend/test/integration/financePayment.deduction.test.js` [NEW]
  - `backend/test/unit/paymentCorrectionInput.test.js`
  - `frontend/src/app/pages/finance/invoices/PaymentCorrectionDrawer.jsx`
  - `frontend/src/app/pages/finance/invoices/PaymentDrawer.jsx`
  - `frontend/src/app/pages/finance/invoices/PaymentReceiptDocument.jsx`
  - `frontend/src/app/pages/finance/invoices/detail.jsx`
  - `frontend/src/app/pages/finance/invoices/schema/paymentCorrectionSchema.js`
  - `frontend/src/app/pages/finance/invoices/schema/paymentSchema.js`
  - `frontend/src/app/pages/finance/invoices/utils/paymentCorrection.js`
  - `frontend/src/app/pages/finance/invoices/utils/paymentCorrection.test.js`
  - `frontend/src/app/pages/finance/journals/detail.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Fitur Potongan Biaya pada Pembayaran Manual**: Menambahkan dukungan field `deduction` (potongan/diskon saat bayar) pada pencatatan pembayaran faktur penjualan manual dan koreksi pembayaran faktur.
  - **Jurnal Keuangan Otomatis**: Menyesuaikan pembukuan jurnal akuntansi pada `financePayment.service.js` agar nominal potongan dijurnal secara proporsional ke akun Potongan Penjualan / Beban terkait tanpa merusak keseimbangan debet-kredit jurnal.
  - **Kwitansi Pembayaran Terperinci**: Memperbarui template cetak dokumen kwitansi (`PaymentReceiptDocument.jsx`) agar merinci nilai tagihan, nominal potongan yang diberikan, dan total bersih yang diterima dari pelanggan.
  - **Dukungan Koreksi Pembayaran**: Memungkinkan admin keuangan mengoreksi nilai potongan pada pembayaran terdahulu dengan validasi batas nominal (potongan harus bulat dan lebih kecil dari total tagihan).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #486  | Abstraksi Multi Payment Gateway, Adaptor Xendit (Fase 0 & Fase 1a), Kebijakan PPN Fee, dan Penguatan Pembukuan Akun | Fondasi multi-vendor payment gateway terstandarisasi, modul adaptor Xendit Payments API v3 lengkap (VA, QRIS, Retail), pemisahan PPN Masukan fee gateway ke akun COA 1-6100, isolasi akun Kas/Bank per gateway, dan audit fee akurat berbasis cicilan riil. |
| #490  | Berita Acara Perubahan Layanan (BAPL) Pelanggan Broadband | Sistem penerbitan dokumen resmi BAPL untuk permintaan perubahan paket (upgrade/downgrade) dari web, mobile, atau manual dengan fitur tanda tangan elektronik pelanggan via web publik. |
| #493  | Potongan Pembayaran Faktur Manual & Kwitansi | Fleksibilitas pencatatan potongan biaya saat pembayaran tagihan manual, integrasi otomatis ke jurnal akuntansi, dan rincian potongan pada kwitansi pembayaran. |

### Kemampuan Baru Pengguna/Admin

- **Admin Keuangan & Penjualan (Issue #486)**:
  - Dapat mengonfigurasi kredensial Xendit (API Key & Webhook Token) secara aman dengan model input *write-only secret*.
  - Dapat memilih kebijakan pembebanan PPN atas biaya gateway Xendit: apakah digabung ke Beban Bank (`include_expense`) atau dipisah ke akun PPN Masukan (`input_vat`).
  - Sistem secara otomatis memisahkan mutasi dan saldo Kas/Bank penampung gateway antar provider (`ipaymu`, `xendit`, `xendit-sandbox`).
  - Laporan transaksi gateway kini mencatat `fee_booked` dan `fee_tax_booked` secara akurat sesuai porsi cicilan yang benar-benar dijurnal.
- **Admin Customer Service & Helpdesk (Issue #490)**:
  - Dapat membuat Berita Acara Perubahan Layanan (BAPL) langsung dari pengajuan perubahan layanan web/mobile atau membuat secara manual dari tab Dokumen Pelanggan.
  - Dapat membagikan tautan tanda tangan elektronik ke WhatsApp pelanggan dengan satu klik via WhatsApp Web / Desktop PIC.
  - Pelanggan dapat menandatangani dokumen BAPL secara digital melalui smartphone tanpa perlu mencetak berkas fisik.
- **Kasir & Admin Keuangan (Issue #493)**:
  - Dapat memasukkan nominal potongan pembayaran (diskon penyelesaian atau negosiasi) langsung pada drawer pembayaran faktur.
  - Kwitansi resmi yang dicetak atau diunduh otomatis menampilkan baris rincian potongan kepada pelanggan.

### Bug Fix / Solusi Masalah

- **Pencegahan Error Gateway Tanpa Fitur Riwayat (Issue #486)**: Background cron sweep dan rekonsiliasi mutasi sebelumnya mengasumsikan semua gateway memiliki API riwayat transaksi. Kini sistem memeriksa kapabilitas `capabilities.history`; jika provider tidak mendukung, cron melewati penarikan riwayat mutasi dengan aman tanpa crash.
- **Penjaga Akun Gateway Kosong Saat Booting (Issue #486)**: Mencegah terbentuknya akun Kas & Bank hantu ketika nomor virtual account/rekening penampung merchant belum diisi pada pengaturan keuangan.
- **Perbaikan Alokasi PPN Masukan Fee Batch (Issue #486)**: Menghindari ketidakseimbangan jurnal saat pembayaran batch banyak faktur yang salah satu fakturnya terpotong sisa tagihan, melalui mekanisme penskalaan proporsional PPN Masukan.
- **Proteksi Dobel Approval & Penghapusan BAPL Bertanda Tangan (Issue #490)**: Dokumen BAPL yang sudah berstatus `signed` diproteksi secara ketat dari perubahan atau penghapusan data untuk menjamin aspek legalitas.

### Menu/Fitur Baru

- **Pengaturan Gateway Xendit**: Menu **Pengaturan > Keuangan > Xendit** untuk konfigurasi kredensial pembayaran dan kebijakan pajak PPN fee.
- **Menu Dokumen Perubahan Layanan**: Menu **Pelanggan > Dokumen > Perubahan Layanan** untuk mengelola seluruh dokumen BAPL pelanggan broadband.
- **Halaman Publik Tanda Tangan BAPL**: Endpoint publik `/public-docs/service-change/sign` untuk penandatanganan dokumen BAPL pelanggan secara daring.
- **Field Potongan Pembayaran**: Input potongan pada form pembayaran faktur manual (**Keuangan > Tagihan Penjualan > Catat Pembayaran**).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### Pengaturan Gateway Xendit & Pembukuan PPN Masukan Fee

- **Penjelasan Fitur**: Sistem kini mendukung arsitektur multi payment gateway. Selain iPaymu, sistem telah dilengkapi adaptor resmi untuk Xendit (Payments API v3) yang mendukung saluran Virtual Account berbagai bank, QRIS, dan Gerai Retail. Admin dapat menentukan kebijakan akuntansi apakah PPN atas biaya transaksi gateway dibukukan terpisah ke akun PPN Masukan (COA `1-6100`) atau digabungkan ke Beban Administrasi Bank (COA `8-1000`).
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi Dekasimal dengan hak akses admin keuangan.
  2. Buka menu **Pengaturan** ➔ tab **Keuangan** ➔ gulir ke seksi **Xendit Payment Gateway**.
  3. Masukkan **Secret API Key** dan **Webhook Verification Token** dari dashboard Xendit.
  4. Pilih **Kebijakan PPN Biaya**:
     - Pilih **Pisahkan ke PPN Masukan (1-6100)** jika perusahaan berstatus PKP dan ingin mengkreditkan pajak masukan fee gateway.
     - Pilih **Gabungkan ke Beban Bank (8-1000)** jika biaya gateway ingin dicatat utuh sebagai satu beban operasional bank.
  5. Klik tombol **Simpan Pengaturan**. Akun Kas & Bank penampung Xendit akan disiapkan secara otomatis oleh sistem.

---

### Penerbitan dan Penandatanganan Berita Acara Perubahan Layanan (BAPL)

- **Penjelasan Fitur**: Setiap perubahan paket langganan broadband pelanggan kini dapat dilengkapi dokumen resmi BAPL dengan klausul kontrak 12 bulan yang diperbarui otomatis. Tautan dokumen dapat dikirimkan ke WhatsApp pelanggan agar pelanggan dapat membubuhkan tanda tangan elektronik melalui layar smartphone.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Pelanggan** ➔ submenu **Dokumen** ➔ pilih tab **Perubahan Layanan**.
  2. Klik tombol **Buat Dokumen BAPL**.
  3. Pilih sumber permintaan:
     - **Web**: Mengambil data dari tiket form pengajuan perubahan layanan publik yang telah dikonfirmasi.
     - **Mobile**: Mengambil data dari pengajuan perubahan paket melalui aplikasi mobile pelanggan.
     - **Manual**: Memilih data pelanggan, nomor langganan, paket lama, dan paket baru secara manual.
  4. Periksa pratinjau klausul dan tanggal efektif perubahan, lalu klik **Terbitkan Dokumen**.
  5. Pada baris dokumen yang baru diterbitkan, klik ikon **Kirim Tautan WhatsApp** untuk membuka WhatsApp Web/Desktop dan mengirimkan tautan tanda tangan langsung ke nomor WhatsApp pelanggan.
  6. Pelanggan membuka tautan, memeriksa rincian perubahan layanan, menandatangani di kanvas tanda tangan, dan menekan **Kirim Tanda Tangan**.
  7. Status dokumen di admin otomatis berubah menjadi **Signed** dan terkunci dari perubahan lebih lanjut.

