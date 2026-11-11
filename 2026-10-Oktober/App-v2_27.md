# 📝 Daily Work Report - Dedy S.N Putra (2026-10-27)

---

## 📅 Laporan Harian - 27 Oktober 2026

---

## 🌿 Branch: `master` / `issue-491` — Koreksi Pembayaran dan Penguatan Pembukuan Pembayaran

### 📌 Informasi Issue

- **Nomor Issue**: #491
- **Judul Issue**: Koreksi Pembayaran dan Penguatan Pembukuan Pembayaran (Sales & Purchase Invoice Payment Correction, Payment Gateway Hardening, and Ledger Integrity)
- **Status Branch**: `Sudah di-merge` (ke `master` via `855bcb5e` dan ke `production` via `811f059a`)

### 📅 Rincian Commit

#### [811f059a] - Merge branch 'master' into production - 27 Oktober 2026, 22:47:37 WIB

- **Komponen yang Berubah**:
  - Menggabungkan seluruh pembaruan dari branch `master` (fitur issue #491 / versi `v1.98.32`) ke branch `production`.
- **Deskripsi Perubahan & Fungsi**:
  - Commit penggabungan (merge commit) untuk deployment rilis versi `v1.98.32` ke branch `production`.

#### [855bcb5e] - resolve #491 - 27 Oktober 2026, 22:46:48 WIB

- **Komponen yang Berubah**:
  - Menggabungkan branch fitur `issue-491` ke branch utama `master`.
- **Deskripsi Perubahan & Fungsi**:
  - Commit penggabungan (merge commit) seluruh implementasi fitur koreksi pembayaran tagihan penjualan & pembelian, penguatan tabel gerbang pembayaran, mitigasi error severity gateway, serta pembaruan changelog ke branch `master`.

#### [39a951d4] - update changelog - 27 Oktober 2026, 22:46:03 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-491.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Pembaruan Berkas Riwayat Rilis (Changelog)**: Menambahkan berkas changelog rilis `issue-491.json` (versi `v1.98.32`, tanggal 27 Oktober 2026) dengan rincian fitur, perbaikan bug, dan peningkatan sistem, serta mendaftarkannya pada indeks changelog `index.json`.
  - **Sinkronisasi Metadata Basis Pengetahuan AI**: Memperbarui berkas `features.json` dengan mendaftarkan release `issue-491` pada seluruh modul terkait (Keuangan, Tagihan Penjualan, Tagihan Pembelian, Gerbang Pembayaran, Kas & Bank, Pengaturan Hak Akses), menambahkan field wajib `reason` pada skema koreksi, serta melengkapi kamus privilege `financeAccount.*`, `financePayment.create`, dan `financeTransfer.*`.

#### [84ba18de] - resolve #491 - 27 Oktober 2026, 22:41:17 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/config/privilege.json`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/financeExpense.controller.js`
  - `backend/src/controllers/financePayment.controller.js`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeExpense.model.js`
  - `backend/src/models/financeInvoice.model.js`
  - `backend/src/models/financeInvoiceUpdate.model.js`
  - `backend/src/models/financePayment.model.js`
  - `backend/src/routes/financeExpense.route.js`
  - `backend/src/routes/financeInvoice.route.js`
  - `backend/src/routes/financePayment.route.js`
  - `backend/src/services/financeExpense.service.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/financeInvoiceUpdate.service.js`
  - `backend/src/services/financeJournal.service.js`
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/src/services/paymentGateways/ipaymu.gateway.js`
  - `backend/src/utils/callWithRetry.js`
  - `backend/src/utils/finance-error.js`
  - `backend/src/utils/gatewayCallMeter.js`
  - `backend/src/utils/liveInvoicePresence.js`
  - `backend/src/utils/paymentCorrectionInput.js` [NEW]
  - `backend/src/utils/telegramAlert.js`
  - `backend/test/integration/financeExpense.paymentCorrection.controller.test.js` [NEW]
  - `backend/test/integration/financeExpense.paymentCorrection.test.js` [NEW]
  - `backend/test/integration/financeExpense.paymentIntegrity.test.js` [NEW]
  - `backend/test/integration/financeGateway.tableFilter.test.js` [NEW]
  - `backend/test/integration/financePayment.correction.controller.test.js` [NEW]
  - `backend/test/integration/financePayment.correction.test.js` [NEW]
  - `backend/test/integration/financePayment.voidSweep.test.js` [NEW]
  - `backend/test/integration/ipaymuGateway.errorSeverity.test.js` [NEW]
  - `backend/test/unit/paymentCorrectionInput.test.js` [NEW]
  - `backend/test/unit/telegramAlert.test.js`
  - `frontend/src/app/pages/finance/expenses/PaymentCorrectionModal.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/detail.jsx`
  - `frontend/src/app/pages/finance/expenses/schema/paymentCorrectionSchema.js` [NEW]
  - `frontend/src/app/pages/finance/expenses/utils/paymentCorrection.js` [NEW]
  - `frontend/src/app/pages/finance/expenses/utils/paymentCorrection.test.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/InvoiceNumbersCell.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/InvoiceNumbersCell.test.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/schema/columns.jsx`
  - `frontend/src/app/pages/finance/gateway/schema/columns.test.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/invoiceNumbers.js` [NEW]
  - `frontend/src/app/pages/finance/gateway/utils/invoiceNumbers.test.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/PaymentCorrectionDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/invoices/detail.jsx`
  - `frontend/src/app/pages/finance/invoices/live/liveInvoiceStage.js`
  - `frontend/src/app/pages/finance/invoices/payments/index.jsx`
  - `frontend/src/app/pages/finance/invoices/payments/schema/columns.jsx`
  - `frontend/src/app/pages/finance/invoices/schema/paymentCorrectionSchema.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/utils/paymentCorrection.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/utils/paymentCorrection.test.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/utils/updateAction.js` [NEW]
  - `frontend/src/app/pages/finance/invoices/utils/updateAction.test.js` [NEW]
  - `frontend/src/components/shared/payment/PaymentInstructionView.jsx`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `partner-frontend/src/app/pages/finance/gateway/InvoiceNumbersCell.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/gateway/InvoiceNumbersCell.test.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/gateway/schema/columns.jsx`
  - `partner-frontend/src/app/pages/finance/gateway/schema/columns.test.jsx` [NEW]
  - `partner-frontend/src/app/pages/finance/gateway/utils/invoiceNumbers.js` [NEW]
  - `partner-frontend/src/app/pages/finance/gateway/utils/invoiceNumbers.test.js` [NEW]
  - `partner-frontend/src/app/pages/finance/invoices/detail.jsx`
  - `partner-frontend/src/app/pages/finance/invoices/utils/updateAction.js` [NEW]
  - `partner-frontend/src/app/pages/finance/invoices/utils/updateAction.test.js` [NEW]
  - `partner-frontend/src/constants/privilegeDescriptions.en.json`
  - `partner-frontend/src/constants/privilegeDescriptions.id.json`
  - `partner-frontend/src/i18n/locales/en/translations.json`
  - `partner-frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Fitur Koreksi Pembayaran Tagihan Penjualan (Sales Invoices)**:
    - Mengembangkan endpoint `PATCH /finance/payment/:payment_id/correct/:seq` yang dilindungi privilege `financePayment.changeSensitive`.
    - Menerapkan prinsip akuntansi append-only: data pembayaran salah tidak diedit di tempat, melainkan cicilan lama dibalik pada buku besar dan jurnal bertanggal tanggal asli pembayaran (`reverseByRefKey` & `reverseJournal`), lalu cicilan baru dicatat dengan nomor urut (`seq`) dan nomor kwitansi baru bertanggal baru.
    - Menandai cicilan lama dengan `void: true` dan `void_kind: 'correction'`. Karena seluruh antarmuka pelanggan (halaman bayar publik, cetak faktur, mobile app, dan Partner API) menyaring `void: true`, cicilan lama otomatis tersembunyi tanpa mengekspos riwayat salah catat kepada pelanggan.
    - Pra-validasi komprehensif (`prepareCorrection`): menolak cicilan otomatis (`ipay`, `wallet`, `entrust`), dokumen migrasi V1, faktur dalam status pembatalan/refund, periode pembukuan tertutup, mutasi kas yang sudah rekonsiliasi bank (`matched`), akun kas asal yang berpotensi bersaldo negatif karena selisih koreksi, serta membatasi pemilihan akun kas pribadi milik admin lain via `assertPaymentAccountAccess`.
    - Eksekusi atomik: klaim cicilan lama + pembuatan rencana koreksi (`correction_plan`), pembaruan `paid_total` atomik (idempoten via `payment_correction_keys`), dan pemulihan otomatis via cron ledger recovery jika terputus di tengah jalan. Jika nominal tidak berubah, status faktur tetap `paid` tanpa pernah berstatus `unpaid` sesaat.
    - Drawer Antarmuka Admin (`PaymentCorrectionDrawer.jsx`): tombol "Koreksi" pada halaman Riwayat Pembayaran dan detail tagihan, form validasi Yup/React Hook Form, ringkasan perbandingan sebelum vs sesudah, input alasan wajib, serta peringatan cetak ulang kwitansi fisik.
  - **Fitur Koreksi Pembayaran Tagihan Pembelian (Expense / Purchase Bills)**:
    - Mengembangkan endpoint `PATCH /finance/expense/:expense_id/payments/:payment_id/correct` (dengan alias `.../payment/:payment_id/correct`) yang dilindungi privilege `financeExpense.changeSensitive`.
    - Memperbaiki bug query posisional Mongoose: update posisional pada array embedded `bill_payments` wajib menggunakan `bill_payments: { $elemMatch: { _id, void: false } }` (bukan dua kondisi terpisah `'bill_payments._id'` dan `'bill_payments.void'`) agar operator `$` menunjuk tepat pada elemen pembayaran yang dituju saat melakukan void pembayaran ke-2 dan seterusnya.
    - Rekalkulasi status otomatis via `derivePaymentStateStage`: menurunkan ulang `bill_status`, `status` (V1), dan salinan `payment` (V1) secara terpadu sehingga status lunas/dibayar sebagian dan sisa tagihan selalu sinkron.
    - Menangani proteksi saldo kas keluar akun tujuan, validasi hak akses akun kas/bank admin (`assertPaymentAccountAccess`), dan integrasi pemulihan otomatis jika server mati di tengah proses via `resumeIncompleteExpensePayments`.
    - Modal Antarmuka Admin (`PaymentCorrectionModal.jsx`): tombol koreksi per baris pada tabel Riwayat Pembayaran detail tagihan pembelian.
  - **Penguatan Tabel Transaksi Gerbang Pembayaran (Payment Gateway)**:
    - Memperbaiki pencarian kolom terhitung "No. Tagihan" (`buildInvoiceNoClause`): filter nomor tagihan diterjemahkan ke dalam klausa query MongoDB dan dikeluarkan dari `columnFilters` agar tidak diabaikan oleh mesin datatable.
    - Mengamankan pencarian teks bebas dengan `escapeRegex` pada nama pelanggan dan nomor transaksi untuk mencegah error crash/500 akibat karakter spesial seperti tanda kurung `(`.
    - Mengimplementasikan sel nomor tagihan ringkas (`InvoiceNumbersCell.jsx`): menampilkan 2 nomor tagihan teratas disertai tombol "+N lainnya" dengan dialog modal/popover untuk melihat seluruh nomor tagihan secara lengkap pada transaksi multi-faktur.
    - Standardisasi label status: mengubah label "Dicairkan" menjadi "Diterima" dan "Belum Dicairkan" menjadi "Belum Diterima" (`settled`), menampilkan kolom Tanggal Dibayar secara default, menampilkan keterangan penanggung biaya (BUYER/MERCHANT) di bawah nominal Biaya, serta menambahkan filter rentang numerik untuk Nilai Transaksi, Biaya, dan Diterima Bersih.
    - Menyelaraskan seluruh komponen tabel gerbang pembayaran ke kedua aplikasi web (`frontend` dan `partner-frontend`).
  - **Mitigasi Error Severity & Alert Telegram**:
    - Mengklasifikasikan error payment gateway (`isTransientGatewayError` pada `ipaymu.gateway.js`): kegagalan jaringan sementara (timeout, connection reset, HTTP 408/425/429/5xx) dicatat dengan tingkat `warn` dan tidak lagi membanjiri notifikasi alert darurat Telegram. Error deterministik (400, 401/403, invalid payload) tetap dicatat sebagai `error`.
    - Membatasi detail pesan error alert Telegram pada daftar-putih field teknis (`path`, `source`, `error_code`, `retry_after`) tanpa mengekspos payload atau header sensitif.
  - **Perbaikan Riwayat Perubahan Faktur (Invoice Updates)**:
    - Memetakan entri riwayat lama tanpa field `action` dari database V1 agar ditampilkan sebagai suntingan (`resolveUpdateActionKey`) dengan terjemahan "Diubah", meniadakan teks translation key rusak pada antarmuka detail tagihan.
  - **Proteksi Integritas Pembukuan & Pemulihan Otomatis (Ledger Recovery & Void Sweep)**:
    - Memperbarui cron sapuan buku besar `runFinanceLedgerRecoveryCron`: mengecualikan transaksi pembalikan bertanda `:reversal` agar pembayaran yang sudah dibatalkan tidak terdeteksi sebagai pembayaran yatim dan tidak muncul kembali di halaman pelanggan.
    - Menjamin idempotensi penuh pada pemulihan proses koreksi dan pembatalan pembayaran yang terputus di tengah jalan.
  - **Pengujian Unit & Integrasi Komprehensif**:
    - Menambahkan test suite Vitest backend (`financePayment.correction.test.js`, `financeExpense.paymentCorrection.test.js`, `financeExpense.paymentIntegrity.test.js`, `financeGateway.tableFilter.test.js`, `ipaymuGateway.errorSeverity.test.js`, `financePayment.voidSweep.test.js`, `paymentCorrectionInput.test.js`, `telegramAlert.test.js`).
    - Menambahkan test suite frontend & partner-frontend (`paymentCorrection.test.js`, `invoiceNumbers.test.js`, `columns.test.jsx`, `updateAction.test.js`).
  - **Pembaruan Aturan Arsitektur Monorepo**:
    - Mendokumentasikan aturan baku baru nomor 32 (Koreksi Pembayaran Faktur), nomor 33 (Pembayaran Tagihan Pembelian), dan nomor 34 (Payment Gateway: Tabel Transaksi & Severity Log) ke dalam `AGENTS.md`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #491  | Koreksi Pembayaran dan Penguatan Pembukuan Pembayaran | Memungkinkan pembetulan pembayaran salah catat pada tagihan penjualan dan pembelian secara aman tanpa merusak buku besar dan tanpa kebocoran jejak ke pelanggan, memperbaiki filter & tampilan tabel gerbang pembayaran, memitigasi spam alert Telegram dari gangguan gateway sementara, serta menjamin integritas pembukuan dan audit trail. |

### Kemampuan Baru Pengguna/Admin

- **Koreksi Pembayaran Tagihan Penjualan (Invoices)**: Admin dengan privilege khusus dapat mengoreksi nominal, metode pembayaran, akun bank/kas penerima, tanggal, nama penyetor, dan catatan pembayaran langsung dari tabel Riwayat Pembayaran atau detail faktur. Jurnal lama dibalik otomatis dan jurnal baru dicatat rapi.
- **Koreksi Pembayaran Tagihan Pembelian (Expenses)**: Admin dapat mengoreksi pembayaran pada tagihan pembelian/biaya operasional dengan penyesuaian otomatis terhadap sisa tagihan dan status tagihan (lunas atau dibayar sebagian).
- **Pencarian No. Tagihan & Karakter Khusus di Gerbang Pembayaran**: Admin dapat melakukan pencarian transaksi berdasarkan nomor tagihan (termasuk pembayaran multi-tagihan) serta mencari nama pelanggan yang mengandung karakter kurung `(` atau simbol tanpa menyebabkan tabel crash.
- **Penyaringan Rentang Angka & Informasi Fee di Gerbang Pembayaran**: Admin dapat menyaring transaksi berdasarkan rentang nilai nominal, melihat informasi pihak penanggung fee (BUYER / MERCHANT), serta melihat ringkasan multi-tagihan yang rapi.

### Bug Fix / Solusi Masalah

- **Pencarian No. Tagihan Tidak Berfungsi di Tabel Gerbang Pembayaran**: Memperbaiki logika filter kolom terhitung agar diterjemahkan ke query MongoDB dan tidak terabaikan oleh tabel.
- **Crash Saat Pencarian Karakter Khusus**: Mencegah kegagalan parsing regex dengan utilitas `escapeRegex` pada pencarian bebas.
- **Salah Tunjuk Elemen Array Saat Void Pembayaran Pembelian ke-2**: Memperbaiki query posisional Mongoose dengan `$elemMatch` agar tepat membatalkan pembayaran yang dipilih.
- **Status Tagihan Pembelian Tidak Sinkron Setelah Pembatalan**: Memastikan `derivePaymentStateStage` dipanggil sehingga status tagihan dan nominal terbayar langsung diperbarui.
- **Pembayaran Terputus / Server Mati**: Penuntasan otomatis oleh cron pemulihan malam hari (`resumeIncompletePayments` dan `resumeIncompleteExpensePayments`) sehingga tidak ada uang keluar/masuk yang menggantung.
- **Pembayaran Batal Muncul Kembali**: Mengecualikan jurnal pembalikan `:reversal` dari sapuan pembayaran yatim agar pembayaran yang sudah dibatalkan tidak muncul kembali di portal pelanggan.
- **Teks Terjemahan Rusak pada Riwayat Perubahan Faktur**: Menangani entri V1 tanpa field `action` agar diterjemahkan sebagai "Diubah".
- **Spam Notifikasi Error Telegram dari Payment Gateway**: Mengklasifikasikan kegagalan sementara (timeout/network drop) sebagai `warn` sehingga tidak memicu alert darurat Telegram.
- **Kelengkapan Terjemahan I18n**: Melengkapi label kolom tabel Blacklist, Pasif, Berhenti Berlangganan, serta pesan gagal salin kode pembayaran.

### Menu/Fitur Baru

- **Drawer Koreksi Pembayaran Tagihan Penjualan**: Tombol aksi "Koreksi" di menu Riwayat Pembayaran (`/finance/invoices/payments`) dan drawer riwayat pembayaran detail faktur.
- **Modal Koreksi Pembayaran Tagihan Pembelian**: Tombol "Koreksi" per baris pada Riwayat Pembayaran detail tagihan pembelian.
- **Hak Akses Sensitif Baru**:
  - `financePayment.changeSensitive`: Mengubah Data Sensitif Penerimaan Pembayaran.
  - `financeExpense.changeSensitive`: Mengubah Data Sensitif Pengeluaran / Tagihan Pembelian.
- **Komponen Sel Multi-Tagihan Gerbang Pembayaran (`InvoiceNumbersCell`)**: Tampilan ringkas nomor faktur dengan dialog modal popover untuk melihat seluruh nomor tagihan.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  - **Koreksi Pembayaran Faktur (Sales & Purchase)**: Fitur yang dirancang khusus untuk memecahkan dilema pembukuan ketika petugas salah mencatat nominal pembayaran, tanggal, metode transfer, atau akun bank. Daripada mengedit transaksi di tempat (yang melanggar kaidah audit akuntansi), sistem Dekasimal V2 menerapkan metode *reversal & reissue* secara atomik: pembayaran salah dibalik bertanggal asli pada buku besar dan jurnal, lalu pembayaran baru dicatat dengan nomor kwitansi baru. Fitur ini aman secara privasi karena pelanggan tidak akan melihat transaksi koreksi, melainkan hanya melihat data pembayaran sah yang berlaku.
- **Langkah Penggunaan (Tutorial)**:
  1. **Mengoreksi Pembayaran Tagihan Penjualan**:
     - Buka menu **Keuangan > Penerimaan Pembayaran > Riwayat Pembayaran** (atau buka detail tagihan yang bersangkutan).
     - Cari baris pembayaran manual yang ingin dibetulkan, lalu klik tombol aksi **Koreksi** (ikon pensil/koreksi).
     - Pada drawer yang muncul di sisi kanan layar, Anda akan melihat data pembayaran saat ini (sebelum) dan formulir perubahan (sesudah).
     - Ubah nilai yang keliru, misalnya nominal, akun rekening kas/bank tujuan, metode pembayaran, tanggal transaksi, nama penyetor, atau catatan.
     - Masukkan **Alasan Koreksi** secara jelas (wajib diisi untuk keperluan jejak audit).
     - Periksa ringkasan perubahan pada kotak konfirmasi di bagian bawah drawer.
     - Klik **Simpan Koreksi**. Sistem akan memproses pembalikan jurnal lama, mencatat jurnal baru, dan memperbarui status faktur serta saldo kas secara real-time.
  2. **Mengoreksi Pembayaran Tagihan Pembelian**:
     - Buka menu **Keuangan > Pengeluaran Operasional / Tagihan Pembelian** dan buka detail tagihan pembelian yang dimaksud.
     - Gulir ke tabel **Riwayat Pembayaran**, lalu klik tombol **Koreksi** pada pembayaran yang ingin diubah.
     - Pada modal yang terbuka, sesuaikan nominal, tanggal, akun kas/bank, atau catatan, serta cantumkan alasan koreksi.
     - Klik **Simpan Perubahan**. Sistem akan memvalidasi saldo akun, membalik mutasi lama, mencatat mutasi baru, dan menghitung ulang sisa tagihan tagihan pembelian.
