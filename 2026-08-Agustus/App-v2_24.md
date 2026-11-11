# 📝 Daily Work Report - Dedy (2026-08-24)

---

## 📅 Laporan Harian - 24 Agustus 2026

---

## 🌿 Branch: `issue-230` — Payment Gateway Abstraction & Public Payment Link

### 📌 Informasi Issue

- **Nomor Issue**: #230
- **Judul Issue**: Payment Gateway Abstraction & Public Payment Link
- **Status Branch**: `Belum di-merge`
- **Status Hari Ini**: Pekerjaan **belum di-commit** (work-in-progress, semua perubahan belum staged)

### 📅 Rincian Pekerjaan Hari Ini (Uncommitted Changes)

> **Catatan:** Tidak ada commit baru hari ini. Seluruh pekerjaan berupa uncommitted changes di branch `issue-230`. Ringkasan di bawah mencakup perubahan yang sedang dikerjakan.

---

#### 🔧 1. Payment Gateway Abstraction Layer (Backend)

- **Komponen yang Berubah**:
  - [`backend/src/services/paymentGateways/registry.js`](backend/src/services/paymentGateways/registry.js) **[NEW]**
  - [`backend/src/services/paymentGateways/ipaymu.gateway.js`](backend/src/services/paymentGateways/ipaymu.gateway.js) **[NEW]**
  - [`backend/src/services/paymentGateway.service.js`](backend/src/services/paymentGateway.service.js)
  - [`backend/src/services/financeGateway.service.js`](backend/src/services/financeGateway.service.js)
  - [`backend/src/controllers/financeGateway.controller.js`](backend/src/controllers/financeGateway.controller.js)
  - [`backend/src/models/financeGatewayTransaction.model.js`](backend/src/models/financeGatewayTransaction.model.js)
- **Deskripsi Perubahan & Fungsi**:
  - Membuat arsitektur **registry pattern** untuk payment gateway agar provider-agnostic. [`registry.js`](backend/src/services/paymentGateways/registry.js) mendefinisikan kontrak interface (`GATEWAYS` map) dan menyediakan fungsi `getActiveGatewayId()` yang membaca `finance_settings.active_gateway` dari database.
  - Logika iPaymu spesifik dipindah dari [`paymentGateway.service.js`](backend/src/services/paymentGateway.service.js) ke [`ipaymu.gateway.js`](backend/src/services/paymentGateways/ipaymu.gateway.js) (502 baris). [`paymentGateway.service.js`](backend/src/services/paymentGateway.service.js) sekarang hanya menjadi facade generik (~36 baris, turun dari ~360 baris).
  - [`financeGateway.service.js`](backend/src/services/financeGateway.service.js) di-refactor agar `upsertGatewayTransaction()` mendukung transaksi gabungan (`invoices` array + `party` context) sebagai alternatif dari jalur lama satu-faktur (`invoice` tunggal). `settleGatewayPayment()` sekarang membaca `gateway` dari dokumen transaksi yang sudah ada (bukan hardcode `'ipaymu'`), sehingga `ref_key` idempoten tetap stabil meski admin mengganti provider aktif di tengah jalan.
  - [`financeGatewayTransaction.model.js`](backend/src/models/financeGatewayTransaction.model.js) ditambah field `invoices` (array sub-dokumen `{invoice, amount}`) untuk mendukung pembayaran gabungan multi-faktur.

---

#### 🌐 2. Public Payment Link — Backend

- **Komponen yang Berubah**:
  - [`backend/src/controllers/publicPayment.controller.js`](backend/src/controllers/publicPayment.controller.js) **[NEW]** (508 baris)
  - [`backend/src/routes/public.route.js`](backend/src/routes/public.route.js)
  - [`backend/src/controllers/publicInvoice.controller.js`](backend/src/controllers/publicInvoice.controller.js)
  - [`backend/src/controllers/utils.controller.js`](backend/src/controllers/utils.controller.js)
  - [`backend/src/controllers/financeSettings.controller.js`](backend/src/controllers/financeSettings.controller.js)
  - [`backend/src/services/financeInvoice.service.js`](backend/src/services/financeInvoice.service.js)
  - [`backend/src/services/financeLedger.service.js`](backend/src/services/financeLedger.service.js)
  - [`backend/src/services/financePayment.service.js`](backend/src/services/financePayment.service.js)
- **Deskripsi Perubahan & Fungsi**:
  - Membuat controller baru [`publicPayment.controller.js`](backend/src/controllers/publicPayment.controller.js) yang menangani seluruh alur pembayaran publik tanpa autentikasi:
    - `getPublicPaymentOptions` — Menentukan mode pembayaran (`single`/`combined`/`redirect`) berdasarkan `finance_settings.payment_scope` dan jumlah faktur belum lunas milik pelanggan.
    - `createPublicPayment` — Membuat permintaan pembayaran sungguhan ke gateway (VA/QRIS), mendukung pembayaran multi-faktur sekaligus.
    - `getPublicPaymentStatus` — Polling status transaksi gateway untuk前端.
  - Menambah 3 endpoint publik baru di [`public.route.js`](backend/src/routes/public.route.js:548): `GET /payment-options`, `POST /pay`, `GET /payment-status`, masing-masing dengan rate limiter terpisah.
  - [`publicInvoice.controller.js`](backend/src/controllers/publicInvoice.controller.js) di-update untuk menyertakan field `print_payment_link` dari `finance_settings` ke response DTO publik.
  - [`utils.controller.js`](backend/src/controllers/utils.controller.js) di-update untuk menyertakan `print_payment_link` dan `payment_link_active` ke `getCompanyData` response.
  - [`financeInvoice.service.js`](backend/src/services/financeInvoice.service.js) ditambah fungsi `findUnpaidInvoicesForParty()` untuk mengambil semua faktur belum lunas milik satu pihak (customer/partner/userHSA), diurutkan jatuh tempo paling lama lebih dulu. Juga, regex `payment_code` di-relax untuk kompatibilitas data warisan V1.
  - `finance_settings` sekarang mendukung field baru: `active_gateway`, `payment_scope`, `ipay_channels`, `ipay_expired_duration`, `show_gateway_fee`.

---

#### 🖥️ 3. Public Payment Link — Frontend

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/public/PublicInvoicePayment.jsx`](frontend/src/app/pages/public/PublicInvoicePayment.jsx) **[NEW]** (726 baris)
  - [`frontend/src/app/pages/public/PublicInvoiceDocument.jsx`](frontend/src/app/pages/public/PublicInvoiceDocument.jsx)
  - [`frontend/src/i18n/config.js`](frontend/src/i18n/config.js)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
- **Deskripsi Perubahan & Fungsi**:
  - Membuat halaman publik baru [`PublicInvoicePayment.jsx`](frontend/src/app/pages/public/PublicInvoicePayment.jsx) yang menampilkan UI pembayaran untuk pelanggan: pemilihan kanal pembayaran (VA/QRIS), tampilan instruksi bayar, dan polling status pembayaran secara otomatis.
  - [`PublicInvoiceDocument.jsx`](frontend/src/app/pages/public/PublicInvoiceDocument.jsx) di-update untuk menampilkan tombol/link ke halaman pembayaran dan informasi terkait payment link.
  - Config i18n di-update, translation key baru ditambah di kedua bahasa (id & en).

---

#### ⚙️ 4. Settings > Finance — Refactoring & Fitur Baru

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/settings/sections/Finance.jsx`](frontend/src/app/pages/settings/sections/Finance.jsx)
  - [`frontend/src/app/pages/settings/sections/gatewayProviders/index.js`](frontend/src/app/pages/settings/sections/gatewayProviders/index.js) **[NEW]**
  - [`frontend/src/app/pages/settings/sections/gatewayProviders/IpaymuGatewaySettings.jsx`](frontend/src/app/pages/settings/sections/gatewayProviders/IpaymuGatewaySettings.jsx) **[NEW]** (507 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Finance settings di-refactor: field iPaymu yang sebelumnya inline di [`Finance.jsx`](frontend/src/app/pages/settings/sections/Finance.jsx) dipindah ke komponen terpisah [`IpaymuGatewaySettings.jsx`](frontend/src/app/pages/settings/sections/gatewayProviders/IpaymuGatewaySettings.jsx) yang diregistrasi lewat [`gatewayProviders/index.js`](frontend/src/app/pages/settings/sections/gatewayProviders/index.js). Pendekatan registry ini memudahkan penambahan provider baru tanpa mengubah `Finance.jsx`.
  - Field settings baru ditambah: `active_gateway` (pilihan provider), `payment_scope` (cakupan penagihan link publik), `ipay_channels` (filter kanal pembayaran), `ipay_expired_duration`, `show_gateway_fee`.

---

#### 📄 5. QR Code Pembayaran di Cetak Faktur

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`](frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx)
  - [`frontend/src/utils/formatMoney.js`](frontend/src/utils/formatMoney.js)
- **Deskripsi Perubahan & Fungsi**:
  - [`InvoiceDocument.jsx`](frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx) sekarang menampilkan **QR Code** pembayaran di bagian bawah dokumen cetak faktur (menggunakan `qrcode.react`). QR hanya muncul untuk faktur yang belum lunas/batal dan jika opsi `print_payment_link` aktif di pengaturan.
  - URL yang di-encode ke QR adalah `/p/invoice/{payment_code}` yang mengarah ke halaman publik pembayaran.

---

#### 🔗 6. Integrasi Invoice Detail Drawer di Profil User

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/users/customer/profile.jsx`](frontend/src/app/pages/users/customer/profile.jsx)
  - [`frontend/src/app/pages/users/business/profile.jsx`](frontend/src/app/pages/users/business/profile.jsx)
  - [`frontend/src/app/pages/users/partner/profile.jsx`](frontend/src/app/pages/users/partner/profile.jsx)
  - [`frontend/src/app/pages/finance/gateway/detail.jsx`](frontend/src/app/pages/finance/gateway/detail.jsx)
  - [`frontend/src/app/pages/finance/ledger/detail.jsx`](frontend/src/app/pages/finance/ledger/detail.jsx)
  - [`frontend/src/app/pages/finance/invoices/detail.jsx`](frontend/src/app/pages/finance/invoices/detail.jsx)
  - [`frontend/src/app/pages/finance/reports/TaxRecap.jsx`](frontend/src/app/pages/finance/reports/TaxRecap.jsx)
- **Deskripsi Perubahan & Fungsi**:
  - Kolom `invoice_id` di tabel faktur pada profil customer/business/partner sekarang membuka `InvoiceDetailDrawer` secara inline (klik → drawer terbuka) alih-alih navigasi ke halaman baru. Ini meningkatkan UX karena user tidak kehilangan konteks halaman profil.
  - [`gateway/detail.jsx`](frontend/src/app/pages/finance/gateway/detail.jsx) juga di-update: kolom faktur di detail transaksi gateway sekarang bisa diklik untuk membuka `InvoiceDetailDrawer`.
  - [`ledger/detail.jsx`](frontend/src/app/pages/finance/ledger/detail.jsx) di-update serupa: referensi faktur di detail jurnal buku besar bisa diklik untuk membuka drawer faktur.
  - Semua integrasi memakai privilege check `financeInvoice.read` via `useHasPrivilege`.

---

#### 🧪 7. Testing

- **Komponen yang Berubah**:
  - [`backend/test/integration/publicPayment.test.js`](backend/test/integration/publicPayment.test.js) **[NEW]** (430 baris)
  - [`backend/test/integration/financeGateway.settle.batch.test.js`](backend/test/integration/financeGateway.settle.batch.test.js) **[NEW]** (236 baris)
  - [`backend/test/integration/publicInvoice.test.js`](backend/test/integration/publicInvoice.test.js)
- **Deskripsi Perubahan & Fungsi**:
  - Menambah integration test untuk alur pembayaran publik (`publicPayment.test.js`) dan settlement batch transaksi gateway (`financeGateway.settle.batch.test.js`).
  - [`publicInvoice.test.js`](backend/test/integration/publicInvoice.test.js) di-update untuk menyesuaikan dengan response DTO baru (field `print_payment_link`).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                             | Dampak Utama                                                                                                                 |
| ----- | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| #230  | Payment Gateway Abstraction & Public Payment Link | Arsitektur payment gateway menjadi provider-agnostic, link pembayaran publik mendukung multi-faktur, QR code di cetak faktur |

### Kemampuan Baru Pengguna/Admin

- **Admin** dapat memilih provider payment gateway aktif dari Settings > Finance (saat ini hanya iPaymu, tapi arsitektur siap untuk provider baru).
- **Admin** dapat mengatur cakupan penagihan link pembayaran publik (`payment_scope`): bayar semua faktur sekaligus atau hanya faktur terlama.
- **Admin** dapat mengaktifkan/menonaktifkan tampilan QR code pembayaran di cetak faktur via `print_payment_link`.
- **Admin** dapat memfilter kanal pembayaran yang tersedia untuk pelanggan (`ipay_channels`).
- **Pelanggan** dapat membayar **beberapa faktur sekaligus** melalui satu link pembayaran publik (mode `combined`).
- **Pelanggan** mendapat pengalaman pembayaran yang lebih baik dengan UI baru yang menampilkan pemilihan kanal, instruksi bayar, dan polling status otomatis.

### Bug Fix / Solusi Masalah

- **Payment code regex V1 legacy**: Regex `payment_code` yang terlalu ketat (`/^\d{4}[A-Za-z0-9]{10}$/`) menyebabkan faktur data warisan V1 (kode 10 karakter tanpa awalan 4 digit) selalu "tidak ditemukan" di halaman publik. Regex di-relax menjadi lebih longgar untuk kompatibilitas.
- **Idempotensi `ref_key` gateway**: `ref_key` idempoten sekarang menggunakan format `<gateway_id>:<transaction_id>` (bukan hardcode `ipaymu:...`), sehingga tetap stabil meski admin mengganti provider aktif di tengah siklus hidup transaksi.
- **Rate limit public invoice**: Limit dinaikkan dari 30 ke 120 per 15 menit untuk mengakomodasi polling status pembayaran dan refresh halaman saat proses pembayaran.

### Menu/Fitur Baru

- **Settings > Finance > Payment Gateway**: Tab/section baru untuk mengatur provider aktif, kanal pembayaran, durasi kadaluarsa, dan visibilitas fee gateway.
- **Halaman Pembayaran Publik** (`/p/invoice/:payment_code/pay`): Halaman baru untuk pelanggan melakukan pembayaran online.
- **QR Code di Cetak Faktur**: Faktur cetak sekarang menyertakan QR Code yang mengarah ke halaman pembayaran publik.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Sistem payment gateway sekarang menggunakan arsitektur registry pattern yang memisahkan logika umum (`financeGateway.service.js`, `paymentGateway.service.js`) dari logika spesifik provider (`paymentGateways/ipaymu.gateway.js`). Penambahan provider baru hanya memerlukan: (1) membuat file `<provider>.gateway.js` dengan kontrak yang sama, (2) mendaftarkannya di `registry.js` dan `GATEWAY_PROVIDERS` frontend. Selain itu, link pembayaran publik sekarang mendukung pembayaran multi-faktur — pelanggan dengan beberapa faktur belum lunas dapat membayar semuanya sekaligus melalui satu transaksi gateway.
- **Langkah Penggunaan (Tutorial)**:
  1. **Mengatur Provider Gateway**: Buka **Settings > Finance** → pilih tab **Payment Gateway** → pilih provider aktif dari dropdown → simpan.
  2. **Mengatur Cakupan Pembayaran Publik**: Di halaman yang sama, pilih `payment_scope`: `all` (bayar semua faktur sekaligus) atau `oldest` (hanya faktur terlama).
  3. **Mengaktifkan QR di Faktur**: Centang opsi **"Tampilkan Link Pembayaran di Cetak Faktur"** di Settings > Finance.
  4. **Pelanggan Membayar**: Pelanggan membuka link faktur dari WhatsApp/email → klik **Bayar Sekarang** → pilih kanal pembayaran → ikuti instruksi (transfer VA / scan QRIS) → status pembayaran di-poll otomatis.
