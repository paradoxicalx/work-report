# 📝 Daily Work Report - Dedy S.N Putra (2026-10-31)

---

## 📅 Laporan Harian - 31 Oktober 2026

---

## 🌿 Branch: `issue-486` — Abstraksi Multi Payment Gateway, Adaptor Xendit Lengkap (Fase 0 - Fase 4), Pembukuan Fee Asli, dan E-Wallet

### 📌 Informasi Issue

- **Nomor Issue**: #486
- **Judul Issue**: Abstraksi Multi Payment Gateway, Adaptor Xendit Payments API v3 (VA, QRIS, Retail, E-Wallet), Pembukuan Fee Asli & PPN Masukan, Penyesuaian Rekonsiliasi Riwayat, dan Pembaruan Antarmuka Gerbang Pembayaran
- **Status Branch**: `Belum di-merge` (Ahead 1 commit dari `origin/master` / squashed commit `9dc2c8eb`, menunggu pengujian akhir sebelum merge ke `master`)

### 📅 Rincian Commit

#### [9dc2c8eb] - feat(finance): payment gateway Xendit di samping iPaymu (resolve #486) - 31 Oktober 2026, 23:54:52 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/xenditChannels.json` [NEW]
  - `backend/src/controllers/financeGateway.controller.js`
  - `backend/src/controllers/financePayment.controller.js`
  - `backend/src/controllers/financeSettings.controller.js`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/publicPayment.controller.js`
  - `backend/src/controllers/setup.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeAccount.model.js`
  - `backend/src/models/financeGatewayReconRun.model.js`
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/models/financePayment.model.js`
  - `backend/src/routes/financeGateway.route.js`
  - `backend/src/routes/financeSettings.route.js`
  - `backend/src/routes/public.route.js`
  - `backend/src/services/financeAccount.service.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeGatewayFee.service.js` [NEW]
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/src/services/liveInvoice.service.js`
  - `backend/src/services/paymentGateway.service.js`
  - `backend/src/services/paymentGateways/ipaymu.gateway.js`
  - `backend/src/services/paymentGateways/registry.js`
  - `backend/src/services/paymentGateways/xendit.client.js` [NEW]
  - `backend/src/services/paymentGateways/xendit.gateway.js` [NEW]
  - `backend/src/services/paymentGateways/xenditCatalog.js` [NEW]
  - `backend/src/services/publicPaymentTarget.service.js`
  - `backend/src/services/settingsBackup.registry.js`
  - `backend/src/utils/ewalletPhone.js` [NEW]
  - `backend/src/utils/gatewayCallMeter.js`
  - `backend/src/utils/gatewayErrorSeverity.js` [NEW]
  - `backend/src/utils/paymentSupports.js` [NEW]
  - `backend/src/utils/sanitize.js`
  - `backend/src/utils/undefinedFilter.js` [NEW]
  - `backend/test/helpers/fakeGateway.js` [NEW]
  - `backend/test/integration/financeAccount.gatewayProvision.test.js` [NEW]
  - `backend/test/integration/financeAccount.undefinedFilter.test.js` [NEW]
  - `backend/test/integration/financeGateway.balanceSummary.test.js` [NEW]
  - `backend/test/integration/financeGateway.callback.test.js`
  - `backend/test/integration/financeGateway.callbackEvents.test.js` [NEW]
  - `backend/test/integration/financeGateway.channels.test.js` [NEW]
  - `backend/test/integration/financeGateway.historyNormalize.test.js` [NEW]
  - `backend/test/integration/financeGateway.multiGateway.test.js` [NEW]
  - `backend/test/integration/financeGateway.reconCapability.test.js` [NEW]
  - `backend/test/integration/financeGateway.reconGatewayUnavailable.test.js` [NEW]
  - `backend/test/integration/financeGateway.reconPerGateway.test.js` [NEW]
  - `backend/test/integration/financeGateway.refundReversal.test.js`
  - `backend/test/integration/financeGateway.settle.batch.test.js`
  - `backend/test/integration/financeGateway.settle.test.js`
  - `backend/test/integration/financeGateway.sweep.test.js`
  - `backend/test/integration/financeGateway.tableFilter.test.js` [NEW]
  - `backend/test/integration/financeGatewayFee.adjust.test.js` [NEW]
  - `backend/test/integration/financeGatewayFee.batch.test.js` [NEW]
  - `backend/test/integration/financeGatewayFee.refund.test.js` [NEW]
  - `backend/test/integration/financeGatewayFee.sweep.test.js` [NEW]
  - `backend/test/integration/financePayment.mobileCreatePayment.test.js` [NEW]
  - `backend/test/integration/financeSettings.feeDirection.test.js` [NEW]
  - `backend/test/integration/financeSettings.xendit.test.js` [NEW]
  - `backend/test/integration/ipaymuGateway.feeDirection.test.js` [NEW]
  - `backend/test/integration/liveInvoice.test.js`
  - `backend/test/integration/paymentGateway.facade.test.js` [NEW]
  - `backend/test/integration/publicPayment.test.js`
  - `backend/test/integration/xenditClient.test.js` [NEW]
  - `backend/test/integration/xenditGateway.createPayment.test.js` [NEW]
  - `backend/test/integration/xenditGateway.e2e.test.js` [NEW]
  - `backend/test/integration/xenditGateway.ewallet.test.js` [NEW]
  - `backend/test/integration/xenditGateway.ewalletCatalog.test.js` [NEW]
  - `backend/test/integration/xenditGateway.history.test.js` [NEW]
  - `backend/test/unit/ewalletPhone.test.js` [NEW]
  - `backend/test/unit/financeGateway.allocateFeeTax.test.js` [NEW]
  - `backend/test/unit/gatewayCallMeter.test.js` [NEW]
  - `backend/test/unit/gatewayErrorSeverity.test.js` [NEW]
  - `backend/test/unit/ipaymuGateway.normalize.test.js` [NEW]
  - `backend/test/unit/loggerSanitizer.test.js` [NEW]
  - `backend/test/unit/paymentGatewayRegistry.test.js` [NEW]
  - `backend/test/unit/paymentSupports.test.js` [NEW]
  - `backend/test/unit/undefinedFilter.test.js` [NEW]
  - `backend/test/unit/xenditCatalog.test.js` [NEW]
  - `backend/test/unit/xenditGateway.history.test.js` [NEW]
  - `backend/test/unit/xenditGateway.normalize.test.js` [NEW]
  - `frontend/public/images/logos/ipaymu.png` [NEW]
  - `frontend/public/images/logos/xendit.png` [NEW]
  - `frontend/public/images/payment-channels/alfamart.svg` [NEW]
  - `frontend/public/images/payment-channels/bca.svg` [NEW]
  - `frontend/public/images/payment-channels/bni.svg` [NEW]
  - `frontend/public/images/payment-channels/bri.svg` [NEW]
  - `frontend/public/images/payment-channels/bsi.svg` [NEW]
  - `frontend/public/images/payment-channels/dana.svg` [NEW]
  - `frontend/public/images/payment-channels/gopay.svg` [NEW]
  - `frontend/public/images/payment-channels/indomaret.png` [NEW]
  - `frontend/public/images/payment-channels/mandiri.svg` [NEW]
  - `frontend/public/images/payment-channels/ovo.svg` [NEW]
  - `frontend/public/images/payment-channels/permata.svg` [NEW]
  - `frontend/public/images/payment-channels/qris.svg` [NEW]
  - `frontend/public/images/payment-channels/shopeepay.svg` [NEW]
  - `frontend/src/app/pages/finance/gateway/GatewayChannelsPanel.jsx`
  - `frontend/src/app/pages/finance/gateway/GatewayOverallStrip.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/GatewayProviderSummaryCard.jsx`
  - `frontend/src/app/pages/finance/gateway/GatewayProviderSummaryCard.test.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/GatewayReconPanel.jsx`
  - `frontend/src/app/pages/finance/gateway/GatewaySummary.jsx`
  - `frontend/src/app/pages/finance/gateway/PaymentInstructionsModal.jsx`
  - `frontend/src/app/pages/finance/gateway/TransactionIdCell.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/TransactionIdCell.test.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/detail.jsx`
  - `frontend/src/app/pages/finance/gateway/index.jsx`
  - `frontend/src/app/pages/finance/gateway/schema/columns.jsx`
  - `frontend/src/app/pages/finance/gateway/schema/columns.test.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/schema/reconRunColumns.jsx`
  - `frontend/src/app/pages/finance/gateway/schema/reconRunColumns.test.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/channelPanel.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/channelPanel.test.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/feeStatus.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/feeStatus.test.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/summaryView.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/summaryView.test.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/transactionId.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/transactionId.test.js` [NEW]
  - `frontend/src/app/pages/public/PublicInvoicePayment.jsx`
  - `frontend/src/app/pages/settings/sections/Finance.jsx`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/IpaymuGatewaySettings.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/gatewayProviders/IpaymuGatewaySettings.test.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/gatewayProviders/XenditGatewaySettings.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/gatewayProviders/XenditGatewaySettings.test.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/gatewayProviders/accountResult.js` [NEW]
  - `frontend/src/app/pages/settings/sections/gatewayProviders/accountResult.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/gatewayProviders/index.js`
  - `frontend/src/app/pages/settings/sections/gatewayProviders/providerLogic.js` [NEW]
  - `frontend/src/app/pages/settings/sections/gatewayProviders/providerLogic.test.js` [NEW]
  - `frontend/src/components/shared/form/Listbox.jsx`
  - `frontend/src/components/shared/payment/PaymentChannelPicker.jsx`
  - `frontend/src/components/shared/payment/PaymentChannelPicker.test.jsx` [NEW]
  - `frontend/src/components/shared/payment/PaymentInstructionView.jsx`
  - `frontend/src/components/shared/payment/PaymentInstructionView.test.jsx` [NEW]
  - `frontend/src/components/shared/payment/channelLogo.js` [NEW]
  - `frontend/src/components/shared/payment/channelLogo.test.js` [NEW]
  - `frontend/src/components/shared/payment/ewalletRules.js` [NEW]
  - `frontend/src/components/shared/payment/ewalletRules.test.js` [NEW]
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/submitGuard.js` [NEW]
  - `frontend/src/utils/submitGuard.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Abstraksi Multi-Provider Payment Gateway (Fase 0)**:
    - Membangun antarmuka adaptor terstandarisasi (`normalizeTransaction`, `parseCallback`, `normalizeHistoryRow`, `capabilities`, `isConfigured`) dan registri gateway (`registry.js`) dengan penolakan ID provider ganda.
    - Menjamin backward compatibility penuh dengan iPaymu tanpa mengubah format respons yang dikonsumsi aplikasi.
    - Setiap transaksi diverifikasi langsung ke provider miliknya (`trx.gateway`), bukan terikat ke gateway yang sedang aktif saat verifikasi berjalan.
    - Mengisolasi rekening Kas & Bank per provider dengan field `gateway_id` berindeks, menambahkan filter pembersih parameter `undefined` pada query Mongo, serta penjagaan nomor VA kosong saat booting server.
  - **Integrasi Penuh Adaptor Xendit Payments API v3 (Fase 1a)**:
    - Menyediakan klien HTTP Xendit mandiri (`xendit.client.js`) tanpa retry otomatis untuk mencegah penagihan ganda (idempoten), serta katalog kanal pembayaran (`xenditCatalog.js`).
    - Mendukung saluran Virtual Account bank nasional (BCA, BNI, BRI, Mandiri, Permata, BSI), QRIS Dinamis, dan Retail Outlet (Alfamart, Indomaret).
    - Membangun endpoint penerimaan webhook callback `/finance/gateway/xendit/callback` dengan otentikasi token (`x-callback-token`), mitigasi webhook asing (`source === 'dekasimal'`), dan isolasi penanganan status.
    - Menyediakan antarmuka konfigurasi kredensial Xendit write-only di pengaturan admin, lengkap dengan penjaga aktivasi gateway dan akun Kas & Bank penampung (`xendit` dan `xendit-sandbox`).
  - **Pembukuan Fee Asli, Penyesuaian Selisih, dan PPN Masukan (Fase 1b)**:
    - Membangun service khusus `financeGatewayFee.service.js` untuk mengelola siklus fee gateway: penyesuaian selisih fee saat final (`fee_pending`), pembukuan PPN Masukan ke akun COA `1-6100` (bila opsi `input_vat` aktif), serta sapuan berkala untuk transaksi berstatus fee tertunda.
    - Menghubungkan nilai `fee_booked` dan `fee_tax_booked` pada dokumen transaksi langsung dari porsi cicilan yang benar-benar dijurnal pada buku besar (`fee_journaled` & `fee_tax_journaled`).
    - Menangani pembalikan fee secara otomatis saat terjadi refund atau pembatalan transaksi, serta pemulihan saldo jika pembatalan dibalikkan.
  - **Modernisasi Tampilan Gerbang Pembayaran Admin & Pengaturan (Fase 2)**:
    - Merancang ulang tab Gerbang Pembayaran pada Pengaturan Keuangan dengan antarmuka accordion per provider (`IpaymuGatewaySettings.jsx` dan `XenditGatewaySettings.jsx`).
    - Menghadirkan kartu ringkasan saldo per provider berlogo resmi (`ipaymu.png` dan `xendit.png`), jalur kontrol kanal per provider yang tahan terhadap balapan muat data, serta strip ringkasan performa global (`GatewayOverallStrip.jsx`).
    - Menambahkan fitur penyederhanaan ID transaksi pada tabel transaksi gateway (`TransactionIdCell.jsx`) dengan tooltip lengkap dan tombol salin cepat.
    - Menghadirkan pengaturan arah penanggung fee iPaymu (`ipay_fee_direction`: `BUYER` atau `MERCHANT`) di panel admin yang disinkronkan secara aman dengan respons riil gateway iPaymu.
  - **Integrasi Kanal E-Wallet Xendit (Fase 3)**:
    - Menambahkan dukungan pembayaran e-wallet (OVO, DANA, ShopeePay, GoPay) khusus pada antarmuka pembayaran faktur publik (`PublicInvoicePayment.jsx`).
    - Membuat utilitas `ewalletPhone.js` untuk menormalkan dan menyamarkan nomor akun e-wallet (nomor telepon OVO hanya berwujud mask sensor dan tidak pernah masuk ke log sistem demi kepatuhan privasi data).
    - Menambahkan tombol penjaga anti klik ganda (`submitGuard.js`) pada formulir pembayaran guna mencegah double submit pada transaksi berbeban tinggi.
  - **Rekonsiliasi Riwayat Mutasi Multi-Gateway (Fase 4)**:
    - Mengisolasi watermark rekonsiliasi per provider (`params.watermarks.<gateway_id>`) sehingga proses rekonsiliasi iPaymu dan Xendit berjalan independen tanpa saling menimpa.
    - Menambahkan fungsi `normalizeHistoryRow` pada adaptor Xendit untuk memfilter transaksi non-aplikasi dan menelusuri data riwayat pembayaran menggunakan kursor halaman (`after_id`).
    - Menyesuaikan penanganan tingkat keparahan log (`gatewayErrorSeverity.js`): kesalahan akibat kelalaian input pelanggan pada aplikasi mobile dicatat sebagai `warn` (bukan `error`), sehingga tidak memicu alarm palsu pada notifikasi bot Telegram.

---

## 🌿 Branch: `master` / `issue-503` — Penghapusan Data Pelanggan/Mitra/Bisnis pada Tiket Dismantle Selesai, Pembekuan Dompet, dan Pembatalan Tagihan

### 📌 Informasi Issue

- **Nomor Issue**: #503
- **Judul Issue**: Penghapusan Data Pelanggan/Mitra/Bisnis pada Tiket Dismantle Selesai, Pembekuan Dompet Otomatis, dan Pembatalan Tagihan Terkait
- **Status Branch**: `Sudah di-merge` (ke `master` via commit `effc1961` dan merge commit `f1bb5fd6`)

### 📅 Rincian Commit

#### [f1bb5fd6] - resolve #503 - 31 Oktober 2026, 18:25:08 WIB

- **Komponen yang Berubah**:
  - Menggabungkan seluruh pembaruan dari branch fitur `issue-503` ke branch utama `master`.
- **Deskripsi Perubahan & Fungsi**:
  - Commit penggabungan (merge commit) integrasi modul penghapusan data pelanggan dari tiket dismantle selesai beserta proteksi saldo dompet ke branch `master`.

---

#### [effc1961] - resolve #503 - 31 Oktober 2026, 18:24:30 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/customer.controller.js`
  - `backend/src/controllers/customerPartner.controller.js`
  - `backend/src/controllers/financeWallet.controller.js`
  - `backend/src/controllers/partnerApiBusiness.controller.js`
  - `backend/src/controllers/partnerApiCustomer.controller.js`
  - `backend/src/controllers/ticket.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/routes/ticket.route.js`
  - `backend/src/services/dismantleClose.service.js`
  - `backend/src/services/financeWallet.service.js`
  - `backend/src/services/ownerDeleteBlockers.service.js`
  - `backend/src/services/ticket.service.js`
  - `backend/src/utils/finance-error.js`
  - `backend/src/utils/resolve-party.js`
  - `backend/test/integration/dismantleClose.service.test.js`
  - `backend/test/integration/financeWallet.test.js` [NEW]
  - `backend/test/integration/ownerDeleteBlockers.test.js`
  - `backend/test/integration/partnerApiCustomer.delete.test.js`
  - `frontend/src/app/pages/finance/invoices/PaymentDrawer.jsx`
  - `frontend/src/app/pages/finance/wallet/index.jsx`
  - `frontend/src/app/pages/finance/wallet/schema/columns.jsx`
  - `frontend/src/app/pages/tickets/TicketDetailDrawer.jsx`
  - `frontend/src/app/pages/tickets/components/DeleteDismantleCustomerModal.jsx` [NEW]
  - `frontend/src/app/pages/tickets/dismantle/detail.jsx`
  - `frontend/src/app/pages/users/business/profile.jsx`
  - `frontend/src/app/pages/users/customer/profile.jsx`
  - `frontend/src/app/pages/users/partner/profile.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Endpoint Pratinjau Dampak Penghapusan Data Pelanggan**: Menambahkan endpoint `GET /api/v1/ticket/:type/delete-customer-preview/:ticketId` pada `ticket.route.js` dan controller `ticket.controller.js`. Endpoint ini menganalisis kondisi tiket dismantle yang telah selesai (`complete: true`) dan mengembalikan ringkasan dampak penghapusan data: daftar faktur unpaid yang akan dibatalkan, detail layanan yang akan dicabut, serta status dan saldo dompet pemilik akun.
  - **Eksekusi Penghapusan Terpadu dari Tiket Dismantle**: Menambahkan endpoint `POST /api/v1/ticket/:type/delete-customer/:ticketId` yang mengeksekusi aksi administratif terpadu:
    - Membatalkan semua faktur berstatus `unpaid` yang terkait dengan layanan dismantle secara bersih.
    - Menghapus record layanan terkait (soft delete).
    - Melakukan penghapusan data entitas pemilik (Pelanggan, Mitra Reseller, atau Pelanggan Bisnis).
  - **Pembekuan Saldo Dompet Otomatis (`frozen: true`)**: Memperbarui `financeWallet.service.js` sehingga saat sebuah akun pemilik dihapus, status dompetnya secara otomatis diubah menjadi `frozen: true`. Dompet yang dibekukan menolak seluruh transaksi mutasi baru (top-up, transfer, dan pembayaran faktur), sekaligus melindungi saldo tersimpan dari penyalahgunaan.
  - **Laci & Modal Konfirmasi Penghapusan Pelanggan**: Membuat komponen antarmuka `DeleteDismantleCustomerModal.jsx` pada detail tiket dismantle. Modal ini menampilkan evaluasi hak akses admin, rincian tagihan unpaid yang akan dibatalkan, dan peringatan saldo dompet sebelum admin mengonfirmasi tindakan penghapusan permanen.
  - **Penyempurnaan Indikator Status Dompet Keuangan**: Menambahkan badge status visual "Dibekukan" pada daftar dan profil dompet keuangan di frontend (`finance/wallet`), serta menyaring entitas terhapus pada drawer pembayaran tagihan.

---

## 🌿 Branch: `master` / `issue-496` — Paket & Profil Broadband Turunan Mitra (Approval, Adjustments, dan Penguncian Parameter Komersial)

### 📌 Informasi Issue

- **Nomor Issue**: #496
- **Judul Issue**: Manajemen Paket & Profil Broadband Turunan Mitra (Partner Broadband Derived Product Requests, Approval Adjustments, and Commercial Attributes Lockdown)
- **Status Branch**: `Sudah di-merge` (ke `master` via commit `9b7faccf` dan merge commit `2276630c`)

### 📅 Rincian Commit

#### [2276630c] - resolve #496 - 31 Oktober 2026, 15:32:57 WIB

- **Komponen yang Berubah**:
  - Menggabungkan seluruh pembaruan dari branch fitur `issue-496` ke branch utama `master`.
- **Deskripsi Perubahan & Fungsi**:
  - Commit penggabungan (merge commit) modul permohonan paket turunan mitra, form persetujuan admin dengan penyesuaian teknis, dan penguncian atribut komersial ke branch `master`.

---

#### [9b7faccf] - resolve #496 - 31 Oktober 2026, 14:16:02 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/partnerApiProductBroadband.controller.js`
  - `backend/src/controllers/partnerApiRadius.controller.js`
  - `backend/src/controllers/partnerApiRadiusProfile.controller.js`
  - `backend/src/controllers/partnerProductAdmin.controller.js` [NEW]
  - `backend/src/controllers/productBroadband.controller.js`
  - `backend/src/data/aiNavigationRoutes.json`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/partnerProductRequest.model.js` [NEW]
  - `backend/src/models/product.model.js`
  - `backend/src/models/radiusProfile.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/routes/partnerProductAdmin.route.js` [NEW]
  - `backend/src/server.js`
  - `backend/src/services/partnerProductRequest.service.js` [NEW]
  - `backend/src/services/productBroadband.service.js`
  - `backend/src/services/radiusProfile.service.js`
  - `backend/src/utils/partnerProductInput.js` [NEW]
  - `backend/src/utils/productIndexMigration.js` [NEW]
  - `backend/test/integration/partnerApiProductBroadband.test.js`
  - `backend/test/integration/partnerApiRadius.test.js`
  - `backend/test/integration/partnerApiRadiusProfile.test.js`
  - `backend/test/integration/partnerProductDerived.hardening.test.js` [NEW]
  - `backend/test/integration/partnerProductDerived.test.js` [NEW]
  - `frontend/src/app/layouts/Root.jsx`
  - `frontend/src/app/navigation/services.js`
  - `frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx`
  - `frontend/src/app/pages/network/syslog/components/SyslogDetailDrawer.jsx`
  - `frontend/src/app/pages/network/syslog/components/SyslogLiveView.jsx`
  - `frontend/src/app/pages/services/broadbandPackage/createDerived.jsx` [NEW]
  - `frontend/src/app/pages/services/broadbandPackage/index.jsx`
  - `frontend/src/app/pages/services/partnerPackage/components/ApprovalAdjustmentsForm.jsx` [NEW]
  - `frontend/src/app/pages/services/partnerPackage/components/PartnerPackageDetailDrawer.jsx` [NEW]
  - `frontend/src/app/pages/services/partnerPackage/components/ReviewRequestDrawer.jsx` [NEW]
  - `frontend/src/app/pages/services/partnerPackage/index.jsx` [NEW]
  - `frontend/src/app/pages/services/partnerPackage/schema/derivedProductColumns.jsx` [NEW]
  - `frontend/src/app/pages/services/partnerPackage/schema/requestColumns.jsx` [NEW]
  - `frontend/src/app/pages/services/partnerPackage/utils/approvalAdjustments.js` [NEW]
  - `frontend/src/app/pages/services/partnerPackage/utils/approvalAdjustments.test.js` [NEW]
  - `frontend/src/app/pages/settings/developer/logs/components/ArchivePreviewModal.jsx`
  - `frontend/src/app/pages/settings/developer/logs/components/LogDetailDrawer.jsx`
  - `frontend/src/app/pages/settings/developer/logs/drawers/LogDetailDrawer.jsx`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/router/services/partnerPackageRoute.jsx` [NEW]
  - `frontend/src/components/shared/table/RowActions.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/features/partnerProductRequestPendingSlice.js` [NEW]
  - `frontend/src/hooks/useTicketBadge.js`
  - `frontend/src/hooks/useTicketBadge.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/store.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Sistem Pengajuan Paket Broadband Turunan Mitra (`partner_product_requests`)**:
    - Menutup endpoint pembuatan paket dan profil langsung bagi mitra via Partner API (HTTP 403) untuk menjaga integritas kapasitas jaringan.
    - Menyediakan endpoint pengajuan paket baru (`POST /p-api/v1/products/broadband/request-derived`) dan perubahan paket turunan yang ada (`POST /p-api/v1/products/broadband/request-change/:code`).
  - **Kontrol Persetujuan Atomik & Penyesuaian Nilai Admin (`ReviewRequestDrawer.jsx`)**:
    - Menggunakan klaim atomik bersyarat status transisi `approving` dengan masa berlaku klaim 5 menit pada service `partnerProductRequest.service.js`, mencegah persetujuan ganda atau benturan dengan aksi pembatalan oleh mitra (HTTP 409).
    - Memungkinkan admin melakukan penyesuaian parameter teknis dan batas kecepatan (`ApprovalAdjustmentsForm.jsx`) yang disanitasi ketat melalui `sanitizeApprovalAdjustments`. Nilai persetujuan akhir dicatat pada `approved_values` tanpa menimpa proposal asli mitra demi transparansi audit.
  - **Penguncian Atribut Komersial Paket Turunan**:
    - Atribut komersial paket turunan (`name`, `price`, `description`, `capacity`, `group`, `radius_profile`, `use_radius`) dan limit kecepatan profil turunan dikunci dari modifikasi sepihak (PATCH/batch) oleh mitra (HTTP 422). Perubahan hanya dapat dilakukan melalui alur pengajuan perubahan `request-change`.
  - **Penegakan Kepemilikan Paket pada Akun RADIUS Mitra**:
    - Pemasangan baru akun pelanggan broadband mitra (`RadAuth`) wajib menggunakan paket dan profil turunan milik mitra bersangkutan. Akun lama yang masih terpasang paket induk dipertahankan fungsinya (`resolveOwnedRef`) hingga mitra memindahkannya sendiri.
  - **Manajemen Menu Admin Paket Mitra (`services/partnerPackage`)**:
    - Membuat halaman admin baru **Layanan > Paket Mitra** untuk meninjau antrean permohonan, riwayat persetujuan/penolakan, dan katalog paket turunan aktif.
    - Menambahkan lencana notifikasi jumlah permohonan pending pada sidebar navigasi secara realtime (`partnerProductRequestPendingSlice.js`).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #486  | Abstraksi Multi Payment Gateway, Adaptor Xendit Lengkap (Fase 0 - Fase 4), Pembukuan Fee Asli, dan E-Wallet | Dukungan provider multi-gateway (iPaymu & Xendit), saluran pembayaran lengkap (VA, QRIS, Retail, E-Wallet OVO/DANA/ShopeePay/GoPay), pembukuan fee riil & PPN Masukan (COA 1-6100), watermark rekonsiliasi terpisah per gateway, dan UI accordion gerbang pembayaran modern. |
| #503  | Penghapusan Pelanggan dari Tiket Dismantle Selesai & Pembekuan Dompet | Alur terpadu penghapusan data pelanggan dari tiket pembongkaran layanan (dismantle) selesai, otomatisasi pembatalan tagihan unpaid layanan terkait, dan pembekuan saldo dompet pelanggan (`frozen: true`). |
| #496  | Paket & Profil Broadband Turunan Mitra (Approval, Adjustments, & Penguncian Atribut) | Standardisasi pengajuan paket turunan mitra reseller, alur approval admin dengan penyesuaian teknis aman, penguncian parameter komersial dari perubahan sepihak, dan isolasi akun RADIUS mitra. |

### Kemampuan Baru Pengguna/Admin

- **Admin Keuangan & Kasir**:
  - Dapat mengaktifkan dan mengelola dua provider gerbang pembayaran sekaligus (iPaymu dan Xendit) melalui antarmuka accordion yang terorganisasi rapi.
  - Dapat menentukan arah penanggung fee iPaymu (`BUYER` atau `MERCHANT`) langsung dari antarmuka pengaturan keuangan.
  - Pembukuan biaya transaksi gateway kini memisahkan PPN Masukan (COA `1-6100`) dari Beban Administrasi Bank (COA `8-1000`) sesuai kebijakan perusahaan PKP.
  - Melihat riwayat transaksi gateway dengan ID transaksi yang rapi dan ringkas, dilengkapi tombol salin satu-klik dan lencana status audit fee (`Final`, `Pending`, `Disalin`).
  - Rekonsiliasi mutasi riwayat berjalan otomatis per gateway dengan watermark mandiri tanpa khawatir kehilangan jejak transaksi tertunda.
- **Pelanggan Umum / Publik**:
  - Pelanggan kini dapat membayar tagihan faktur melalui e-wallet terpopuler (OVO, DANA, ShopeePay, GoPay) pada halaman pembayaran publik resmi.
  - Proses pembayaran terlindungi dari klik ganda berkat integrasi penjaga formulir cerdas (`submitGuard`).
- **Petugas Helpdesk & Teknisi Tiket**:
  - Dapat menghapus data pelanggan yang layanannya telah dibongkar permanen langsung dari lembar tiket dismantle yang sudah selesai, lengkap dengan pembatalan otomatis tagihan unpaid dan pembekuan saldo dompet.
- **Admin Jaringan & Layanan**:
  - Mengelola dan menyetujui permohonan paket internet turunan yang diajukan oleh mitra reseller pada menu **Layanan > Paket Mitra**.
  - Menyesuaikan profil batas kecepatan atau parameter teknis sebelum paket diterbitkan secara resmi.
- **Mitra Reseller (Partner API)**:
  - Mengajukan paket broadband komersial khusus untuk pelanggan binaannya melalui antarmuka Partner API yang terstandardisasi.

### Bug Fix / Solusi Masalah

- **Pencegahan Double Submit E-Wallet (Issue #486)**: Penambahan pengaman formulir `submitGuard` pada tombol bayar OVO dan e-wallet untuk mencegah terbuatnya beberapa transaksi identik akibat klik ganda pelanggan.
- **Pembersihan Log Sensitif PII E-Wallet (Issue #486)**: Nomor akun telepon e-wallet disanitasi dan dimasker ketat sebelum disimpan, serta dilarang masuk ke log Winston demi menjaga privasi pengguna.
- **Pemisahan Watermark Rekonsiliasi (Issue #486)**: Watermark rekonsiliasi yang sebelumnya global kini dipecah per provider (`params.watermarks.<gateway_id>`), mencegah hilangnya jejak transaksi mutasi iPaymu saat Xendit dipindai (atau sebaliknya).
- **Pencegahan Pelanggan Hantu Pasca-Dismantle (Issue #503)**: Memastikan tiket pembongkaran layanan yang selesai dapat menuntaskan penghapusan data pelanggan secara komprehensif tanpa meninggalkan tagihan menggantung di sistem keuangan.
- **Pencegahan Pembuatan Profil Bebas Tanpa Kontrol oleh Mitra (Issue #496)**: Menutup celah pembuatan profil RADIUS mandiri oleh mitra tanpa batas kecepatan (`0K/0K`), mewajibkan verifikasi dan validasi kecepatan pada setiap permohonan paket turunan.

### Menu/Fitur Baru

- **Tab Pengaturan Provider Gateway Accordion**: Pengaturan Keuangan ➔ Gerbang Pembayaran kini berbentuk panel accordion per provider (iPaymu & Xendit) dengan kartu ringkasan berlogo resmi.
- **Pembayaran E-Wallet Publik**: Opsi pembayaran OVO, DANA, ShopeePay, dan GoPay pada portal pembayaran daring pelanggan (`/public/invoice/pay`).
- **Modal Hapus Pelanggan Tiket Dismantle**: Tombol dan modal aksi "Hapus Pelanggan" pada tiket pembongkaran layanan berstatus selesai (**Tiket > Pembongkaran Layanan**).
- **Menu Paket Mitra**: Halaman pengawasan permohonan dan katalog paket turunan mitra pada menu navigasi **Layanan > Paket Mitra**.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### Pengelolaan Multi Gateway Xendit & Pembayaran E-Wallet

- **Penjelasan Fitur**: Dekasimal kini mendukung sistem multi payment gateway yang memungkinkan penerimaan pembayaran melalui iPaymu maupun Xendit. Pada Xendit, pelanggan dapat memilih saluran pembayaran Virtual Account (BCA, BNI, BRI, Mandiri, Permata, BSI), QRIS Dinamis, Retail Outlet (Alfamart, Indomaret), dan E-Wallet (OVO, DANA, ShopeePay, GoPay). Biaya transaksi gateway secara otomatis dijurnal dengan pemisahan PPN Masukan jika diaktifkan.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi Dekasimal dengan akun Admin Keuangan.
  2. Buka menu **Pengaturan** ➔ tab **Keuangan** ➔ gulir ke bagian **Gerbang Pembayaran**.
  3. Buka accordion **Xendit Payment Gateway**:
     - Masukkan **Secret API Key** dan **Webhook Verification Token** yang valid.
     - Pilih **Kebijakan PPN Biaya**: pilih **Pisahkan ke PPN Masukan (1-6100)** jika perusahaan mengkreditkan faktur pajak biaya gateway.
     - Aktifkan saluran pembayaran yang diinginkan (VA, QRIS, Retail, E-Wallet).
  4. Simpan pengaturan. Sistem akan memverifikasi kredensial dan menyiapkan rekening kas/bank penampung Xendit.
  5. Untuk pembayaran pelanggan: Buka tautan pembayaran faktur publik, pilih metode pembayaran **E-Wallet** (misalnya OVO), masukkan nomor HP terdaftar, lalu klik **Bayar Sekarang**. Pelanggan akan menerima notifikasi konfirmasi di aplikasi e-wallet mereka.

---

### Peninjauan dan Persetujuan Permohonan Paket Broadband Turunan Mitra

- **Penjelasan Fitur**: Mitra reseller yang ingin menjual paket broadband dengan nama dan spesifikasi khusus dapat mengajukan permohonan paket turunan. Admin jaringan dapat meninjau permohonan tersebut, menyesuaikan profil kecepatan bila perlu, dan menyetujuinya agar paket dapat langsung digunakan oleh pelanggan mitra.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi Dekasimal dengan hak akses pengelola layanan.
  2. Perhatikan lencana notifikasi pada menu **Layanan** ➔ pilih submenu **Paket Mitra**.
  3. Pada tab **Permohonan Paket**, klik baris permohonan mitra berstatus `Pending`.
  4. Drawer peninjauan permohonan (`ReviewRequestDrawer`) akan terbuka, menampilkan detail paket induk, usulan nama paket turunan, harga jual mitra, dan batas kecepatan yang diminta.
  5. Jika terdapat parameter kecepatan yang perlu disesuaikan dengan kapasitas transmisi, centang opsi penyesuaian dan ubah batas kecepatan pada form penyesuaian persetujuan.
  6. Klik tombol **Setujui Permohonan** untuk menerbitkan paket turunan secara otomatis, atau klik **Tolak Permohonan** dengan menyertakan alasan penolakan.
  7. Paket yang telah disetujui akan langsung muncul pada tab **Paket Turunan Aktif** dan siap dipasangkan pada akun pelanggan mitra.
