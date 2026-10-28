# 📝 Daily Work Report - Dedy S.N Putra (2026-10-28)

---

## 📅 Laporan Harian - 28 Oktober 2026

---

## 🌿 Branch: `issue-486` — Abstraksi Multi Payment Gateway, Standarisasi Kontrak Adaptor, dan Isolasi Akun Gateway

### 📌 Informasi Issue

- **Nomor Issue**: #486
- **Judul Issue**: Abstraksi Multi Payment Gateway, Kontrak Adaptor Standar, dan Penguatan Akun Gateway Keuangan (Payment Gateway Provider Abstraction, Normalized Contract, Capabilities, and Scoped Finance Accounts)
- **Status Branch**: `Belum di-merge` (Ahead 9 commit dari `origin/issue-486`, menunggu integrasi pengujian lanjutan sebelum merge ke `master`)

### 📅 Rincian Commit

#### [98941269] - save #486 ganti nama fungsi gateway generik (checkGatewayBalance, getGatewayHistory, gatewayCallbackLimiter) - 28 Oktober 2026, 21:28:39 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/setup.controller.js`
  - `backend/src/routes/financeGateway.route.js`
  - `backend/src/routes/public.route.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/paymentGateway.service.js`
  - `backend/test/integration/financeGateway.callback.test.js`
  - `backend/test/integration/financeGateway.refundReversal.test.js`
  - `backend/test/integration/financeGateway.settle.batch.test.js`
  - `backend/test/integration/financeGateway.settle.test.js`
  - `backend/test/integration/financeGateway.sweep.test.js`
  - `backend/test/integration/liveInvoice.test.js`
  - `backend/test/integration/publicPayment.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Pembersihan Istilah Khusus Vendor Menjadi Fungsi Generik**: Mengganti seluruh penamaan fungsi dan middleware publik yang sebelumnya spesifik terikat ke iPaymu menjadi generik dan agnostik vendor di seluruh codebase backend (routes, controllers, services, dan test suite):
    - `checkIpaymuBalance` ➔ `checkGatewayBalance` (pengecekan saldo merchant gateway aktif).
    - `getIpaymuHistory` ➔ `getGatewayHistory` (pengambilan data riwayat mutasi transaksi).
    - `ipaymuCallbackLimiter` ➔ `gatewayCallbackLimiter` (rate limiter penerimaan callback webhook pembayaran).
  - **Refaktor Rujukan Endpoint & Rate Limiting**: Memperbarui rute pembayaran publik (`/public/payment/*`) dan rute internal finance gateway (`/finance/gateway/*`) agar menggunakan penamaan generik tanpa mengubah perilaku maupun signature API yang dikonsumsi frontend.

---

#### [d50044a6] - save #486 akun gateway: cakup per gateway_id dan penjaga VA kosong saat boot - 28 Oktober 2026, 21:23:23 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/setup.controller.js`
  - `backend/src/models/financeAccount.model.js`
  - `backend/src/services/financeAccount.service.js`
  - `backend/test/integration/financeAccount.gatewayProvision.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Penambahan Field `gateway_id` pada Skema Akun Kas/Bank**: Menambahkan field opsional `gateway_id` berindeks pada `models/financeAccount.model.js` untuk mengidentifikasi akun kas/bank penampung spesifik per provider gerbang pembayaran (misal `ipaymu`, `xendit`, `midtrans`).
  - **Modularisasi Provisi Akun Kas Gateway**: Memindahkan logika provisi otomatis akun kas gateway dari `setup.controller.js` ke fungsi terisolasi `ensureGatewayFinanceAccounts` pada `services/financeAccount.service.js`. Provisi akun mencari akun berdasarkan kombinasi nomor rekening dan `gateway_id`, mencegah tumpang tindih data akun antar provider.
  - **Penjaga Akun VA Kosong Saat Booting**: Menambahkan validasi ketat agar provisi otomatis akun gateway tidak dijalankan apabila nomor Virtual Account / rekening merchant gateway masih kosong atau belum dikonfigurasi di `FinanceSetting`, mencegah timbulnya record akun kas "hantu" tanpa nomor rekening sah saat booting server.
  - **Test Integrasi Provisi Akun**: Membuat berkas pengujian `financeAccount.gatewayProvision.test.js` untuk memvalidasi siklus pembuatan akun kas gateway, pencegahan akun duplikat, dan penolakan konfigurasi VA kosong.

---

#### [af5b24a7] - save #486 callback gateway: dispatch berbasis event standar - 28 Oktober 2026, 21:19:01 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/financeGateway.controller.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/paymentGateways/ipaymu.gateway.js`
  - `backend/test/integration/financeGateway.callbackEvents.test.js` [NEW]
  - `backend/test/unit/ipaymuGateway.normalize.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Dispatch Webhook Callback Berbasis Event Standar**: Menghapus percabangan `if-else` berbasis kode status vendor mentah (`statusCode === '1' || '6' || '7'`, dll.) pada `createGatewayCallbackHandler`. Alur penerimaan kini membaca `event` standar hasil ekstraksi `gateway.parseCallback(body)`:
    - `event === 'paid'`: Memicu penyelesaian dan pembukuan pembayaran atomik (`settleGatewayPayment`).
    - `event === 'pending'`: Menandai status transaksi pending (`markGatewayTransactionPending`) dan memperbarui status faktur langsung via Socket.io.
    - `event === 'closed'`: Menangani pembatalan, kadaluarsa, atau refund transaksi (`closeGatewayTransaction`) dengan pembalikan pembayaran otomatis jika transaksi sebelumnya sudah pernah dibukukan.
    - Event tidak dikenal dicatat rapi dan dibalas HTTP 200 agar provider tidak terus-menerus melakukan retry tanpa henti.
  - **Penegakan Keamanan Token Callback Dinamis**: Mendukung konfigurasi `requiresCallbackToken` per adaptor. Jika gateway mewajibkan token dan token tidak disetel atau tidak valid, request langsung ditolak dengan HTTP 401. Untuk gateway warisan yang belum menandatangani token, request diterima dengan pencatatan peringatan keamanan terstruktur.
  - **Jejak Audit Callback Terstandarisasi**: Memperbarui `recordCallback` untuk menyimpan `statusCode` yang telah dinormalkan pada riwayat audit callback transaksi.
  - **Test Integrasi Event Callback**: Menambahkan test suite `financeGateway.callbackEvents.test.js` untuk menguji skenario callback pembayaran sukses, pending, kadaluarsa, token tidak valid, dan id transaksi tidak ditemukan.

---

#### [1b407044] - save #486 rekonsiliasi: dilewati bila gateway aktif tanpa kapabilitas history - 28 Oktober 2026, 21:14:31 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/financeGateway.service.js`
  - `backend/test/integration/financeGateway.reconCapability.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Pengecekan Kapabilitas Riwayat pada Cron Rekonsiliasi**: Memperbarui fungsi `runGatewayHistoryReconciliation` agar membaca adaptor gateway aktif (`getActiveGateway()`) dan memverifikasi `capabilities.history`. Apabila gateway yang aktif tidak memiliki fitur penarikan riwayat mutasi dari API-nya, proses rekonsiliasi otomatis dilewati secara bersih (`{ skipped: true, reason: 'history_not_supported', gateway: reconGateway.id }`) tanpa melempar exception atau memicu peringatan crash.
  - **Normalisasi Baris Mutasi via Adaptor**: Mengganti fungsi helper lama `isPaidAtGateway` dan `mapGatewayStatus` dengan fungsi `reconGateway.normalizeTransaction(row)` milik adaptor gateway aktif untuk mendeteksi apakah baris mutasi berstatus lunas (`is_paid`) dan memetakan kode status transaksi secara tepat.
  - **Test Integrasi Kapabilitas Rekonsiliasi**: Menambahkan berkas test `financeGateway.reconCapability.test.js` untuk menguji perilaku skip rekonsiliasi ketika provider aktif tidak mendukung API riwayat mutasi.

---

#### [90959c8b] - save #486 sapuan gateway: riwayat hanya untuk gateway berkapabilitas history - 28 Oktober 2026, 21:08:12 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/paymentGateway.service.js`
  - `backend/test/integration/financeGateway.multiGateway.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Pemisahan Jalur Sapuan Terbuka (Sweep) Berdasarkan Kapabilitas**: Menyesuaikan logika background sweep transaksi menggantung (`sweepOpenGatewayTransactions`). Penelusuran transaksi lewat mutasi riwayat gateway hanya dilakukan untuk provider yang mendukung `capabilities.history === true`.
  - **Fallback Sapuan Per-Transaksi**: Untuk provider gateway yang tidak memiliki endpoint riwayat mutasi, sistem secara otomatis mengalihkan mekanisme sapuan hanya ke penelusuran status per-transaksi (`checkPayment`) untuk transaksi-transaksi yang masih berstatus terbuka/pending pada database lokal.
  - **Penghapusan Ketergantungan iPaymu pada Service Pembayaran**: Memperbarui fungsi `getPaymentHistory` di `paymentGateway.service.js` untuk memeriksa kapabilitas history sebelum melakukan pemanggilan API gateway.

---

#### [d0b5cb7a] - save #486 inti gateway: online_payment tetap payload mentah gateway - 28 Oktober 2026, 21:01:11 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/financeGateway.service.js`
  - `backend/test/integration/financeGateway.settle.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Preservasi Payload Mentah Vendor untuk Integritas Backward-Compatibility**: Memastikan fungsi `verifyWithGateway` menyertakan properti `raw` (payload asli dari vendor dalam format snake_case). Saat settlement berhasil dicatat (`performSettlement`), properti `data.raw` disimpan apa adanya pada dokumen `invoice.online_payment`. Hal ini menjamin sistem legacy seperti modul sinkronisasi V1, pengecekan status pelanggan, dan Partner API (p-api) tetap membaca struktur field yang diharapkannya tanpa menimbulkan breaking change.
  - **Pencegahan Kebocoran Payload Mentah ke Logika Bisnis**: Logika pengambilan keputusan status pelunasan internal tetap 100% menggunakan properti standar hasil normalisasi, bukan dari properti `raw`.
  - **Perbaikan Propagasi `gatewayId` pada Penutupan Transaksi**: Memperbaiki fungsi `closeGatewayTransaction` agar meneruskan `gatewayId` ke `reverseGatewaySettlement` saat melakukan pembalikan settlement transaksi.

---

#### [bf67047d] - save #486 inti gateway: verifikasi per-transaksi dan bentuk standar - 28 Oktober 2026, 20:55:14 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/paymentGateway.service.js`
  - `backend/test/integration/financeGateway.multiGateway.test.js` [NEW]
  - `backend/test/integration/financeGateway.sweep.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Resolusi Gateway Pemilik Transaksi (`resolveTransactionGatewayId`)**: Mengimplementasikan penentuan provider gateway per-transaksi. Transaksi lama sebelum pengenalan field `gateway` secara otomatis ditangani sebagai transaksi iPaymu (`legacy fallback`), sedangkan transaksi baru membaca field `gateway` dari record transaksi di database.
  - **Verifikasi Agnostik Provider (`verifyWithGateway`)**: Memperbarui fungsi verifikasi transaksi ke gateway agar menerima parameter `gatewayId`, memanggil `checkPayment({ transactionId, gatewayId })`, dan menormalkan respons vendor via `requireGatewayById(gatewayId).normalizeTransaction(remote)`.
  - **Standardisasi Ekstraksi Data Finansial**: Menyelaraskan perhitungan `fee_direction`, biaya admin, dan nomor Virtual Account menggunakan fungsi pembantu yang menerima parameter provider target (`getDefaultFeeDirection(gateway)`, `getMerchantVa(gateway)`).
  - **Test Integrasi Multi-Gateway**: Membuat test suite komprehensif `financeGateway.multiGateway.test.js` (298 baris kode) untuk memverifikasi alur verifikasi transaksi, settlement, dan handling error lintas beberapa provider pembayaran yang berbeda.

---

#### [6af26e56] - save #486 registry gateway: validasi kontrak dan requireGatewayById - 28 Oktober 2026, 20:49:39 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/paymentGateways/registry.js`
  - `backend/test/helpers/fakeGateway.js` [NEW]
  - `backend/test/unit/paymentGatewayRegistry.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Penegakan Kontrak Adaptor Gateway (`assertGatewayContract`)**: Menerapkan mekanisme runtime assertion ketat terhadap setiap modul adaptor gateway saat registrasi/booting server. Memastikan setiap adaptor memiliki:
    - Properti identitas wajib: `id` (string unik), `label` (nama tampilan).
    - Nilai boolean wajib: `requiresCallbackToken`.
    - Objek deklarasi fitur: `capabilities` (`history: boolean`, `balance: boolean`).
    - 9 fungsi wajib: `createPayment`, `checkStatus`, `getChannels`, `getMerchantAccount`, `buildCallbackUrl`, `getDefaultFeeDirection`, `verifyCallbackToken`, `parseCallback`, dan `normalizeTransaction`.
    - Fungsi kondisional: `getHistory` wajib ada bila `capabilities.history === true`, `getBalance` wajib ada bila `capabilities.balance === true`.
  - **Registrasi Adaptor Dinamis & Aman**: Menyediakan fungsi `registerGateway` dan `unregisterGateway` (untuk keperluan isolasi unit test).
  - **Fail-Fast Gateway Retrieval (`requireGatewayById`)**: Menambahkan fungsi pengambil gateway yang langsung melempar error jika provider tidak terdaftar, mencegah transaksi keuangan salah routing atau dieksekusi secara diam-diam oleh adaptor yang salah.
  - **Infrastruktur Mocking & Pengujian Unit Registry**: Membuat `fakeGateway.js` sebagai template implementasi dummy dan `paymentGatewayRegistry.test.js` untuk menguji seluruh skenario validasi kontrak dan pendaftaran gateway.

---

#### [319b83d2] - save #486 adaptor iPaymu: bentuk standar, event callback, kapabilitas - 28 Oktober 2026, 20:45:31 WIB

- **Komponen yang Berubah**:
  - `backend/src/services/paymentGateways/ipaymu.gateway.js`
  - `backend/test/unit/ipaymuGateway.normalize.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Implementasi Adaptor iPaymu Sesuai Kontrak Standar**: Memperbarui modul `ipaymu.gateway.js` agar memenuhi kontrak baku arsitektur payment gateway baru Dekasimal V2:
    - `mapIpaymuStatus`: Memetakan seluruh status numerik iPaymu (`0` pending, `1` settled, `6` paid/unsettled, `7` escrow, `-2` expired, `2`/`-1` canceled, `3` refunded, `4`/`5` failed) ke status internal yang seragam.
    - `normalizeTransaction`: Mengonversi objek transaksi mentah iPaymu menjadi bentuk standar dengan field: `status`, `status_code`, `is_paid`, `is_settled`, `reference_id`, `session_id`, `merchant_account`, `via`, `channel`, `payment_no`, `amount`, `sub_total`, `fee`, `total`, `fee_direction`, `paid_at`, `payer_name`, dan `notes`. Field-field spesifik iPaymu diisolasi dan tidak bocor ke luar adaptor.
    - `parseCallback`: Mengekstraksi `transactionId`, `statusCode`, dan menurunkan `event` standar (`paid`, `pending`, `closed`, `unknown`) dari payload webhook callback.
    - Deklarasi metadata: `requiresCallbackToken: false` (iPaymu tidak menandatangani callback) dan `capabilities: { history: true, balance: true }`.
  - **Pengujian Unit Normalisasi iPaymu**: Menambahkan berkas test `ipaymuGateway.normalize.test.js` (168 baris) untuk memverifikasi akurasi pemetaan status, normalisasi transaksi, dan ekstraksi event callback.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #486  | Abstraksi Multi Payment Gateway & Penguatan Kontrak Adaptor | Mentransformasikan arsitektur modul payment gateway dari monolitik yang terikat kuat ke satu vendor (iPaymu) menjadi arsitektur multi-provider berbasis adaptor standar, memperketat validasi kontrak saat inisialisasi boot, mengisolasi akun kas/bank per provider, serta menjamin integritas backward-compatibility dengan sistem eksternal. |

### Kemampuan Baru Pengguna/Admin

- **Kesiapan Dukungan Multi Payment Gateway**: Fondasi sistem keuangan kini siap untuk mengintegrasikan provider pembayaran tambahan (seperti Xendit, Midtrans, atau Duitku) cukup dengan membuat satu file adaptor yang memenuhi kontrak tanpa mengubah kode di controller atau service inti.
- **Akun Kas/Bank Terisolasi per Gateway Provider**: Sistem kini mengenali akun kas/bank penampung secara spesifik berdasarkan `gateway_id`, memungkinkan pencatatan saldo dan mutasi kas terpisah yang rapi jika perusahaan menggunakan lebih dari satu provider gateway.
- **Toleransi Kapabilitas Gateway Berbeda**: Provider yang hanya mendukung verifikasi per-transaksi (tanpa fitur penarikan riwayat mutasi berkala) kini dapat beroperasi normal tanpa menyebabkan error atau memicu kegagalan cron rekonsiliasi malam hari.

### Bug Fix / Solusi Masalah

- **Pencegahan Akun Kas 'Hantu' saat Booting**: Mencegah provisi akun kas gateway otomatis saat nomor Virtual Account/rekening merchant belum diisi pada konfigurasi, menghindari timbulnya baris akun kas kosong di database.
- **Eliminasi Hardcoded Status Vendor pada Logika Finansial**: Menghapus seluruh pengecekan status mentah vendor di controller dan service; status transaksi dan penentuan pelunasan kini ditangani secara deterministik oleh adaptor.
- **Penolakan Callback Webhook Tanpa Token pada Gateway Wajib-Token**: Mengamankan endpoint webhook callback agar langsung menolak request dengan status 401 jika gateway yang digunakan mewajibkan verifikasi token dan token tidak disetel/salah.
- **Eliminasi Potensi Breaking Change Sistem Legacy**: Menjaga ketersediaan payload asli vendor pada `invoice.online_payment.raw`, memastikan modul V1, portal pelanggan, dan Partner API tetap menerima format data lama tanpa kendala.

### Menu/Fitur Baru

- **Adaptor Contract Assertion (`assertGatewayContract`)**: Mekanisme fail-fast pada runtime registry untuk memvalidasi kelengkapan fungsionalitas setiap modul gateway baru saat server mulai berjalan.
- **Standardized Callback Event Dispatcher**: Alur webhook callback berbasis event terpadu (`paid`, `pending`, `closed`).
- **Skema Akun Kas `gateway_id`**: Identifikasi provider gateway pada tabel master akun keuangan (`finance_accounts`).
- **Pembersihan API Generik**: Fungsi-fungsi modul gateway kini menggunakan nama agnostik (`checkGatewayBalance`, `getGatewayHistory`, `gatewayCallbackLimiter`).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **Arsitektur Adaptor Payment Gateway Dekasimal V2**: Merupakan lapisan abstraksi yang memisahkan urusan integrasi teknis pihak ketiga (seperti format API, enkripsi token, dan nama field vendor) dari logika pembukuan internal Dekasimal (seperti pembuatan kwitansi, jurnal buku besar, dan pembaruan status faktur). Dengan arsitektur ini, seluruh data transaksi dari gateway dinormalkan ke dalam satu kontrak standar. Setiap provider memiliki metadata kapabilitas (`capabilities`), sehingga sistem tahu secara otomatis apakah provider tersebut mendukung penarikan mutasi massal atau hanya verifikasi status per transaksi.
- **Langkah Penggunaan & Integrasi Provider Baru (Tutorial bagi Pengembang)**:
  1. **Membuat Adaptor Provider Baru**:
     - Buat file adaptor baru di `backend/src/services/paymentGateways/<nama_provider>.gateway.js`.
     - Implementasikan fungsi-fungsi wajib sesuai kontrak: `createPayment`, `checkStatus`, `getChannels`, `getMerchantAccount`, `buildCallbackUrl`, `getDefaultFeeDirection`, `verifyCallbackToken`, `parseCallback`, dan `normalizeTransaction`.
     - Tentukan nilai deklarasi kapabilitas, contoh: `capabilities: { history: false, balance: true }`.
  2. **Mendaftarkan Adaptor pada Registry**:
     - Buka `backend/src/services/paymentGateways/registry.js`.
     - Impor adaptor baru dan daftarkan via `registerGateway(namaProviderGateway)`. Kontrak akan divalidasi otomatis saat booting.
  3. **Mengaktifkan Provider**:
     - Masuk ke menu **Pengaturan > Pengaturan Keuangan** pada dashboard admin.
     - Pilih provider aktif melalui field `active_gateway`.
     - Masukkan kredensial API dan nomor rekening/VA merchant. Saat disimpan, sistem akan secara otomatis memprovisi akun kas/bank penampung dengan `gateway_id` yang sesuai.
