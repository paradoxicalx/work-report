# 📝 Daily Work Report - Dedy (2026-08-25)

---

## 📅 Laporan Harian - 25 Agustus 2026

---

## 🌿 Branch: `issue-230` — Payment Gateway Abstraction, Public Payment Link & Automated Reconciliation

### 📌 Informasi Issue

- **Nomor Issue**: #230
- **Judul Issue**: Payment Gateway Abstraction, Public Payment Link & Automated Reconciliation
- **Status Branch**: `Belum di-merge`
- **Status Hari Ini**: Pekerjaan **belum di-commit** (work-in-progress, seluruh perubahan belum staged)

### 📅 Rincian Pekerjaan Hari Ini (Uncommitted Changes)

> **Catatan:** Seluruh pekerjaan hari ini berupa uncommitted changes lanjutan pada branch `issue-230`. Ringkasan di bawah mencakup modul dan fitur yang dikembangkan dan disempurnakan sepanjang hari ini.

---

#### 🔄 1. Rekonsiliasi Riwayat Gateway Otomatis & Terjadwal (Backend & Cron Worker)

- **Komponen yang Berubah**:
  - [`cron-worker/src/jobs/processors/financeGatewayHistoryReconcile.js`](cron-worker/src/jobs/processors/financeGatewayHistoryReconcile.js) **[NEW]**
  - [`cron-worker/src/jobs/scheduler.js`](cron-worker/src/jobs/scheduler.js)
  - [`cron-worker/src/jobs/worker.js`](cron-worker/src/jobs/worker.js)
  - [`cron-worker/src/services/api.service.js`](cron-worker/src/services/api.service.js)
  - [`backend/src/models/financeGatewayReconRun.model.js`](backend/src/models/financeGatewayReconRun.model.js) **[NEW]**
  - [`backend/src/services/financeGateway.service.js`](backend/src/services/financeGateway.service.js)
  - [`backend/src/controllers/internal.controller.js`](backend/src/controllers/internal.controller.js)
  - [`backend/src/routes/internal.route.js`](backend/src/routes/internal.route.js)
  - [`backend/src/routes/financeGateway.route.js`](backend/src/routes/financeGateway.route.js)
  - [`frontend/src/app/pages/finance/gateway/GatewayReconPanel.jsx`](frontend/src/app/pages/finance/gateway/GatewayReconPanel.jsx)
  - [`frontend/src/app/pages/finance/gateway/ReconRunDetailDrawer.jsx`](frontend/src/app/pages/finance/gateway/ReconRunDetailDrawer.jsx) **[NEW]**
  - [`frontend/src/app/pages/finance/gateway/schema/reconRunColumns.jsx`](frontend/src/app/pages/finance/gateway/schema/reconRunColumns.jsx) **[NEW]**
- **Deskripsi Perubahan & Fungsi**:
  - Membangun sistem **rekonsiliasi riwayat gateway otomatis** (`financeGatewayHistoryReconcile`) via Cron Worker yang berjalan secara periodik setiap 5 menit (`*/5 * * * *`). Berbeda dengan `sweep` (yang menyisir transaksi lokal yang belum lunas), rekonsiliasi riwayat ini menyapu riwayat dari sisi gateway untuk menangkap transaksi yang berhasil dibayar di gateway namun tidak memiliki catatan lokal atau callback gagal total.
  - Membuat model Mongoose [`financeGatewayReconRun.model.js`](backend/src/models/financeGatewayReconRun.model.js) untuk merekam jejak setiap eksekusi rekonsiliasi (waktu eksekusi, pemicu cron/manual, status, watermark, jumlah halaman disapu, item dipindai, dicocokkan, dibukukan, dilewati, gagal, dan rincian log item).
  - Menambahkan endpoint internal `POST /api/v1/internal/cron/finance-gateway-history-reconcile` dan controller internal yang dipanggil oleh BullMQ processor.
  - Di Frontend, merombak tab **Rekonsiliasi** pada halaman Gerbang Pembayaran:
    - Menghapus judul tabel redundan dan memindahkan tombol **"Jalankan Sekarang"** ke header tabel.
    - Menampilkan tabel riwayat rekonsiliasi (`reconRunColumns.jsx`) dengan metrik lengkap (Waktu, Pemicu, Halaman, Dipindai, Cocok, Dibukukan, Dilewati, Gagal, Status, Aksi).
    - Menambahkan drawer detail rekonsiliasi [`ReconRunDetailDrawer.jsx`](frontend/src/app/pages/finance/gateway/ReconRunDetailDrawer.jsx) untuk menginspeksi rincian per transaksi yang diproses selama rekonsiliasi.

---

#### 💳 2. Penyempurnaan Alur & Modal Instruksi Pembayaran Publik (QRIS, VA, E-Wallet, CC)

- **Komponen yang Berubah**:
  - [`backend/src/config/paymentInstructions.json`](backend/src/config/paymentInstructions.json) **[NEW]**
  - [`frontend/src/constants/paymentInstructions.js`](frontend/src/constants/paymentInstructions.js) **[NEW]**
  - [`frontend/src/app/pages/public/PublicInvoicePayment.jsx`](frontend/src/app/pages/public/PublicInvoicePayment.jsx) **[NEW]**
  - [`frontend/src/app/pages/public/PublicInvoiceDocument.jsx`](frontend/src/app/pages/public/PublicInvoiceDocument.jsx)
  - [`frontend/src/app/pages/settings/sections/gatewayProviders/PaymentInstructionsModal.jsx`](frontend/src/app/pages/settings/sections/gatewayProviders/PaymentInstructionsModal.jsx) **[NEW]**
  - [`frontend/src/app/pages/settings/sections/gatewayProviders/IpaymuGatewaySettings.jsx`](frontend/src/app/pages/settings/sections/gatewayProviders/IpaymuGatewaySettings.jsx)
  - [`backend/src/controllers/publicPayment.controller.js`](backend/src/controllers/publicPayment.controller.js)
- **Deskripsi Perubahan & Fungsi**:
  - Memperbarui dan menyusun instruksi pembayaran lengkap per-kanal (bukan per-metode umum) untuk berbagai channel perbankan dan retail di [`paymentInstructions.json`](backend/src/config/paymentInstructions.json):
    - **Convenience Store**: Alfamart, Indomaret (dengan kode merchant spesifik).
    - **Virtual Account**: BCA, Mandiri, BNI, BRI, Permata, Danamon, CIMB Niaga, Bank Muamalat (BMI), BSI, Bank Artha Graha (BAG).
    - **QRIS**: Panduan scan dan pembayaran m-Banking / e-wallet.
    - **E-Wallet**: DANA, ShopeePay.
    - **Kartu Kredit (CC)**: Integrasi alur bayar langsung ke URL payment gateway eksternal.
  - Pada halaman publik pembayaran [`PublicInvoicePayment.jsx`](frontend/src/app/pages/public/PublicInvoicePayment.jsx):
    - Khusus kanal **QRIS**, membuat render QR Code interaktif menggunakan `qrcode.react` beserta tombol **Download QR Code** untuk mempermudah pelanggan menyimpan kode bayar di ponsel mereka.
    - Mengganti judul halaman publik dari yang sebelumnya teks statis `"Tagihan Penjualan"` menjadi **Nomor Tagihan** dinamis.
    - Menangani alur pembayaran Kartu Kredit (CC) dengan tombol **"Bayar Sekarang"** langsung membuka gateway eksternal secara aman.
  - Pada menu **Settings > Finance > iPaymu**, menyediakan tombol edit pada setiap metode pembayaran yang membuka [`PaymentInstructionsModal.jsx`](frontend/src/app/pages/settings/sections/gatewayProviders/PaymentInstructionsModal.jsx) sehingga admin dapat mengkustomisasi langkah-langkah instruksi cara bayar secara langsung per channel.

---

#### 🛡️ 3. Pemrosesan Status Callback & Siklus Hidup Transaksi Gateway Lengkap

- **Komponen yang Berubah**:
  - [`backend/src/models/financeGatewayTransaction.model.js`](backend/src/models/financeGatewayTransaction.model.js)
  - [`backend/src/services/financeGateway.service.js`](backend/src/services/financeGateway.service.js)
  - [`backend/src/controllers/financeGateway.controller.js`](backend/src/controllers/financeGateway.controller.js)
  - [`frontend/src/app/pages/finance/gateway/detail.jsx`](frontend/src/app/pages/finance/gateway/detail.jsx)
- **Deskripsi Perubahan & Fungsi**:
  - Melengkapi pemetaan status transaksi gateway callback dari iPaymu secara menyeluruh:
    - `-2`: Expired (Kedaluwarsa)
    - `0`: Pending (Menunggu Pembayaran)
    - `1`: Success (Berhasil & Terbukukan)
    - `2`: Cancelled (Dibatalkan)
    - `3`: Refund (Dikembalikan)
    - `4`: Error
    - `5`: Failed (Gagal)
    - `6`: Success - Unsettled (Berhasil Belum Settlement)
    - `7`: Escrow
  - Menambahkan field `initial_response` pada model `financeGatewayTransaction` untuk merekam payload respons pertama saat transaksi pertama kali diinisiasi ke gateway, selain riwayat callback yang diterima.
  - Menerapkan mekanisme pembersihan log callback (TTL index / retention 90 hari) agar dokumen transaksi tidak mengalami bloat seiring bertambahnya volume transaksi.
  - Menambahkan tombol dan endpoint **"Cek Pembayaran"** (`POST /finance/gateway/transactions/:id/check-payment` / `queryGatewayTransaction`) pada drawer detail transaksi untuk memungkinkan admin melakukan verifikasi status real-time langsung ke provider iPaymu dan menyinkronkan status pembukuan secara instan.

---

#### 📊 4. Penyempurnaan UI/UX Modul Gerbang Pembayaran & Pengaturan

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/gateway/index.jsx`](frontend/src/app/pages/finance/gateway/index.jsx)
  - [`frontend/src/app/pages/finance/gateway/detail.jsx`](frontend/src/app/pages/finance/gateway/detail.jsx)
  - [`frontend/src/app/pages/finance/gateway/schema/columns.jsx`](frontend/src/app/pages/finance/gateway/schema/columns.jsx)
  - [`frontend/src/app/pages/finance/gateway/GatewaySummary.jsx`](frontend/src/app/pages/finance/gateway/GatewaySummary.jsx)
  - [`frontend/src/app/pages/finance/gateway/GatewayProviderSummaryCard.jsx`](frontend/src/app/pages/finance/gateway/GatewayProviderSummaryCard.jsx) **[NEW]**
  - [`frontend/src/app/pages/finance/gateway/GatewayChannelsPanel.jsx`](frontend/src/app/pages/finance/gateway/GatewayChannelsPanel.jsx)
  - [`frontend/src/app/pages/finance/invoices/detail.jsx`](frontend/src/app/pages/finance/invoices/detail.jsx)
  - [`frontend/src/app/pages/settings/sections/Finance.jsx`](frontend/src/app/pages/settings/sections/Finance.jsx)
  - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx)
  - [`frontend/src/components/shared/table/status.js`](frontend/src/components/shared/table/status.js)
- **Deskripsi Perubahan & Fungsi**:
  - Di tabel transaksi gerbang pembayaran (`columns.jsx`):
    - Kolom **Nomor Tagihan** kini dilengkapi icon mata interaktif yang langsung membuka `InvoiceDetailDrawer`.
    - Kolom **Metode** dan **Kanal** dibungkus menggunakan komponen string `Badge` huruf besar (uppercase).
    - Kolom **Pelanggan** dibuat dinamis menjadi link yang mengarahkan ke profil entitas terkait (`customer`, `partner`, atau `business`) sesuai tipe relasi pihak pembayar.
    - Menambahkan kolom **Provider Gateway** untuk memfasilitasi arsitektur multi-provider.
    - Mengubah penamaan label kolom menjadi lebih ringkas: `"Biaya"` (sebelumnya `"Biaya Gerbang Pembayaran"`), memindahkan kolom status ke sebelah kanan tanggal, dan merapikan kolom summary rekonsiliasi (*Halaman*, *Dibukukan*, *Gagal*).
  - Pada drawer detail transaksi (`detail.jsx`):
    - Keterangan biaya gateway seperti `(ditanggung pembayar)` atau `(ditanggung merchant)` kini dibungkus dengan `Badge` rapi tanpa tanda kurung mentah.
    - Menampilkan informasi data pihak pembayar lengkap dengan link ke profilnya.
    - Menampilkan data respons awal inisiasi transaksi dan riwayat callback yang diterima.
  - Pada drawer detail faktur ([`invoices/detail.jsx`](frontend/src/app/pages/finance/invoices/detail.jsx)):
    - Menambahkan tombol dropdown di samping tombol cetak untuk opsi **"Salin Link Publik"** dan **"Buka Link Publik"**.
  - Mengganti istilah antarmuka dari bahasa asing *"payment gateway"* menjadi istilah baku Bahasa Indonesia **"Gerbang Pembayaran"** pada seluruh teks aplikasi dan translation file.

---

#### 📑 5. Tagihan Prorata Aktivasi Layanan Broadband di Tengah Bulan

- **Komponen yang Berubah**:
  - [`backend/src/services/financeAutoInvoice.service.js`](backend/src/services/financeAutoInvoice.service.js)
  - [`backend/src/controllers/radiusAuthentication.controller.js`](backend/src/controllers/radiusAuthentication.controller.js)
  - [`backend/src/routes/radiusAuthentication.route.js`](backend/src/routes/radiusAuthentication.route.js)
  - [`frontend/src/app/pages/services/broadband/create.jsx`](frontend/src/app/pages/services/broadband/create.jsx)
  - [`backend/test/integration/financeAutoInvoice.prorata.test.js`](backend/test/integration/financeAutoInvoice.prorata.test.js) **[NEW]**
- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan fungsi `createProratedInvoiceForRemainingMonth` pada [`financeAutoInvoice.service.js`](backend/src/services/financeAutoInvoice.service.js) untuk menghitung dan membuat tagihan prorata otomatis bagi langganan broadband yang diaktifkan di tengah bulan berjalan.
  - Mendukung mode `dryRun: true` saat pembuatan akun broadband baru (`createAuthentication`), yang mengembalikan pratinjau perhitungan prorata (periode sisa hari, nominal tarif prorata, diskon proporsional, pajak, dan tanggal jatuh tempo).
  - Di Frontend ([`broadband/create.jsx`](frontend/src/app/pages/services/broadband/create.jsx)), menampilkan modal konfirmasi [`ConfirmModal`](frontend/src/components/shared/ConfirmModal.jsx) berisi rincian tagihan prorata setelah form aktivasi berhasil disimpan, meminta persetujuan admin sebelum invoice prorata dibuat.
  - Menambahkan endpoint `POST /api/v1/radius/authentications/:id/prorated-invoice/confirm` untuk konfirmasi pembuatan tagihan prorata dengan validasi idempotensi dan pencegahan dobel tagih.
  - Menambahkan pengujian integrasi otomatis komprehensif pada [`financeAutoInvoice.prorata.test.js`](backend/test/integration/financeAutoInvoice.prorata.test.js).

---

#### 🐛 6. Perbaikan Bug & Peningkatan Sistem Lainnya

- **Komponen yang Berubah**:
  - [`backend/src/controllers/files.controller.js`](backend/src/controllers/files.controller.js)
  - [`backend/src/services/financeGateway.service.js`](backend/src/services/financeGateway.service.js)
  - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json)
  - [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json)
  - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json)
  - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json)
  - [`frontend/src/constants/privilegeDescriptions.id.json`](frontend/src/constants/privilegeDescriptions.id.json)
- **Deskripsi Perubahan & Fungsi**:
  - **Perbaikan Pengambilan Logo Perusahaan**: Memperbaiki fungsi `getLogo` pada [`files.controller.js`](backend/src/controllers/files.controller.js) agar mengambil file logo dinamis dari `app_settings.company_logo` di MinIO, dengan fallback aman ke `logo.png` jika file kustom tidak tersedia.
  - **Perbaikan Regex Helper**: Memperbaiki referensi helper `escapeRegex` pada [`financeGateway.service.js`](backend/src/services/financeGateway.service.js) untuk pencarian datatable.
  - **Kelengkapan i18n**: Mendaftarkan seluruh translation key baru di sisi Backend dan Frontend untuk bahasa Indonesia (`id`) dan Inggris (`en`).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                             | Dampak Utama                                                                                                                                                                          |
| ----- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #230  | Payment Gateway Abstraction, Public Payment Link & Automated Reconciliation       | Rekonsiliasi riwayat gateway otomatis via BullMQ cron worker, instruksi bayar per-kanal lengkap, render QRIS & download QR, tombol Cek Pembayaran real-time, dan tagihan prorata broadband |

### Kemampuan Baru Pengguna/Admin

- **Admin** dapat memantau dan memicu **Rekonsiliasi Riwayat Gateway** secara otomatis maupun manual dengan metrik dan rincian log per transaksi.
- **Admin** dapat mengkustomisasi instruksi tata cara pembayaran per masing-masing channel perbankan dan gerai retail langsung dari modal pengaturan iPaymu.
- **Admin** dapat melakukan **Cek Pembayaran** langsung dari drawer detail transaksi gerbang pembayaran untuk memverifikasi dan menyinkronkan status transaksi terkini dari provider.
- **Admin** mendapatkan modal konfirmasi pratinjau **Tagihan Prorata** secara otomatis saat mengaktifkan layanan broadband pelanggan di pertengahan bulan.
- **Admin** dapat dengan mudah menyalin atau membuka link pembayaran publik faktur langsung dari drawer detail faktur.
- **Pelanggan** mendapatkan panduan pembayaran yang sangat jelas dan akurat sesuai kanal yang dipilih (Alfamart, Indomaret, VA BCA, Mandiri, BNI, BRI, Permata, Danamon, CIMB Niaga, BMI, BSI, BAG, Dana, ShopeePay).
- **Pelanggan** yang memilih metode **QRIS** dapat langsung melihat QR Code yang di-generate dan men-download gambar QR Code untuk pembayaran via e-wallet / m-banking.

### Bug Fix / Solusi Masalah

- **Sinkronisasi Logo Perusahaan**: Mengatasi masalah logo perusahaan yang tidak berubah setelah diunggah di Pengaturan Aplikasi dengan membaca nama berkas logo aktif dari database MinIO.
- **Penanganan Status Callback Komprehensif**: Memastikan seluruh kode status transaksi gateway (dari status sukses, tertunda, gagal, kadaluarsa, hingga pembatalan) dipetakan dan ditangani secara konsisten tanpa error parsing.
- **Pencegahan Bloat Database**: Menerapkan retensi otomatis 90 hari untuk log data callback transaksi gateway.
- **Perbaikan Helper Filter & Rate Limiting**: Memperbaiki fungsi pencarian regex dan konfigurasi trust proxy pada middleware rate limiter.

### Menu/Fitur Baru

- **Keuangan > Gerbang Pembayaran > Tab Rekonsiliasi**: Tampilan monitoring riwayat proses rekonsiliasi gateway beserta drawer rincian log item.
- **Pengaturan > Keuangan > iPaymu > Modal Instruksi Cara Bayar**: Editor instruksi langkah pembayaran per channel gateway.
- **Tombol Cek Pembayaran**: Fitur inspeksi status real-time ke provider gateway pada drawer detail transaksi.
- **Render & Download QR Code QRIS**: Fitur generate visual dan download QR Code di halaman publik pembayaran tagihan.
- **Modal Konfirmasi Tagihan Prorata Broadband**: Dialog konfirmasi tagihan sisa bulan berjalan saat pendaftaran langganan internet baru.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: Modul Gerbang Pembayaran (Payment Gateway) kini memiliki sistem ketahanan data berlapis: (1) Callback webhook instan saat pembayaran terjadi, (2) Sweep periodik untuk transaksi lokal berstatus tertunda, dan (3) **Rekonsiliasi Riwayat Gateway Otomatis** setiap 5 menit yang menyapu data dari provider untuk mendeteksi transaksi yang lunas di gateway namun belum tercatat di sistem lokal. Selain itu, halaman pembayaran publik pelanggan telah dilengkapi panduan instruksi interaktif per kanal, dukungan QRIS dengan QR Code yang dapat diunduh, serta alur direct-pay untuk Kartu Kredit.
- **Langkah Penggunaan (Tutorial)**:
  1. **Melihat Riwayat Rekonsiliasi Gateway**: Buka **Keuangan > Gerbang Pembayaran** → klik tab **Rekonsiliasi**. Klik tombol **"Jalankan Sekarang"** di atas tabel jika ingin melakukan rekonsiliasi manual seketika. Klik baris riwayat untuk melihat detail transaksi yang diproses.
  2. **Mengubah Instruksi Cara Bayar**: Buka **Pengaturan > Keuangan** → pilih tab **iPaymu** → pada daftar metode pembayaran, klik ikon pensil (edit) di samping nama metode/kanal untuk menyesuaikan langkah pembayaran.
  3. **Melakukan Cek Pembayaran Transaksi**: Buka **Keuangan > Gerbang Pembayaran** → klik transaksi yang ingin diperiksa → pada drawer detail, klik tombol **"Cek Pembayaran"** untuk meminta status terbaru dari gateway dan otomatis membukukan jika sudah lunas.
  4. **Membayar via QRIS di Halaman Publik**: Pelanggan membuka link faktur → klik **Bayar Sekarang** → pilih metode **QRIS** → QR Code akan tampil di layar → pelanggan dapat memindai langsung atau menekan tombol **"Download QR Code"** untuk membayar lewat aplikasi e-wallet / m-Banking mereka.
  5. **Menerbitkan Tagihan Prorata Broadband**: Buka menu **Layanan > Broadband > Tambah Baru** → isi form aktivasi internet pelanggan dengan tanggal aktif di tengah bulan berjalan → simpan. Setelah berhasil, modal konfirmasi pratinjau prorata akan muncul → klik **"Buat Tagihan Prorata"** untuk langsung menerbitkan tagihan proporsional sisa hari.
