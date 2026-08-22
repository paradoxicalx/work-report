# 📝 Daily Work Report - Dedy S.N Putra (2026-08-09)

---

## 📅 Laporan Harian - 9 Agustus 2026

---

## 📌 Catatan Penting

> Tidak ada commit yang di-push pada hari ini. Seluruh pekerjaan berupa **uncommitted changes** (work-in-progress) pada branch `issue-211` yang sudah di-merge ke `master`. Perubahan ini merupakan pengembangan lanjutan modul **Finance** yang mencakup banyak area: pelacakan pihak terkait, pembalikan jurnal, pengaturan payment gateway, dan peningkatan UI di berbagai halaman.

---

## 🌿 Branch: `issue-211` — Pengembangan Lanjutan Modul Finance

### 📌 Informasi Issue

- **Nomor Issue**: #211
- **Judul Issue**: Pengembangan Lanjutan Modul Finance (Party Tracking, Journal Reversal, Settings Consolidation, Payment Gateway Config)
- **Status Branch**: `Sudah di-merge` ke master (uncommitted changes masih aktif)

### 📅 Rincian Pekerjaan (Uncommitted Changes)

Total: **75 file berubah**, +2923 baris, -914 baris

---

#### 🔧 Backend — Model & Skema

##### Perubahan [`financeTransaction.model.js`](backend/src/models/financeTransaction.model.js)

- **Komponen yang Berubah**:
  - [`backend/src/models/financeTransaction.model.js`](backend/src/models/financeTransaction.model.js)
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan field pihak lawan transaksi terstruktur (`customer`, `partner`, `employee`, `vendor`) sebagai pelengkap teks bebas `tofrom`. Memungkinkan pelacakan pihak secara terstruktur pada setiap transaksi keuangan.

##### Perubahan [`financeTransactionDraft.model.js`](backend/src/models/financeTransactionDraft.model.js)

- **Komponen yang Berubah**:
  - [`backend/src/models/financeTransactionDraft.model.js`](backend/src/models/financeTransactionDraft.model.js)
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan field pihak yang sama (`customer`, `partner`, `employee`, `vendor`) pada draf transaksi agar pihak tidak hilang saat draf disetujui menjadi transaksi final.

##### Perubahan [`financeLogs.model.js`](backend/src/models/financeLogs.model.js)

- **Komponen yang Berubah**:
  - [`backend/src/models/financeLogs.model.js`](backend/src/models/financeLogs.model.js)
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan field `employee` (ref ke Admin) dan `vendor` (ref ke Vendor) pada log keuangan. Field opsional baru ini aman untuk koleksi yang dipakai bersama V1 — kode V1 tidak pernah merujuknya.

##### Perubahan [`financeCoa.model.js`](backend/src/models/financeCoa.model.js)

- **Komponen yang Berubah**:
  - [`backend/src/models/financeCoa.model.js`](backend/src/models/financeCoa.model.js)
- **Deskripsi Perubahan & Fungsi**:
  - Penambahan field pendukung untuk tampilan COA yang lebih kaya.

##### Perubahan [`financeJournal.model.js`](backend/src/models/financeJournal.model.js)

- **Komponen yang Berubah**:
  - [`backend/src/models/financeJournal.model.js`](backend/src/models/financeJournal.model.js)
- **Deskripsi Perubahan & Fungsi**:
  - Penambahan field pendukung untuk fitur pembalikan jurnal.

---

#### 🔧 Backend — Service Layer

##### Perubahan [`financeCoa.service.js`](backend/src/services/financeCoa.service.js)

- **Komponen yang Berubah**:
  - [`backend/src/services/financeCoa.service.js`](backend/src/services/financeCoa.service.js) (+219 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **Saldo Kelompok Akun**: Akun induk (`is_group`) sekarang menampilkan "Saldo Terhitung" yang benar — dihitung dari gabungan seluruh akun anak/cucunya via pencarian `path` regex, bukan hanya dari baris jurnal langsung.
  - **Filter "Pihak"**: Kolom Pihak di buku besar COA bisa mencari karyawan/pelanggan/mitra/vendor yang tertaut via utility `buildPartySearchOr`.
  - **Peta Admin & Pihak**: `buildAdminMap` dan `buildPartyMap` untuk menampilkan nama admin pembuat dan pihak terkait di setiap baris jurnal.

##### Perubahan [`financeLedger.service.js`](backend/src/services/financeLedger.service.js)

- **Komponen yang Berubah**:
  - [`backend/src/services/financeLedger.service.js`](backend/src/services/financeLedger.service.js) (+109 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **Fallback ke Application Settings Lama**: Field yang dipindah dari Settings > Application (Tagihan, iPay) dibaca dari `application_settings` sebagai fallback jika belum pernah disimpan lewat form Finance baru — menjaga kontinuitas konfigurasi produksi.
  - **`getIpaymuGatewayConfig`**: Fungsi baru untuk membaca kredensial mentah iPaymu (API key, VA, sandbox, callback) dari database — SATU-SATUNYA tempat `ipay_api` boleh dibaca.
  - **Field `employee` & `vendor`**: Ditambahkan ke log keuangan (mutasi kas/bank) untuk pelacakan pihak terstruktur.
  - **Filter recon_status & party**: Mendukung filter status rekonsiliasi dan pihak terkait di buku besar.

##### Perubahan [`financeJournal.service.js`](backend/src/services/financeJournal.service.js)

- **Komponen yang Berubah**:
  - [`backend/src/services/financeJournal.service.js`](backend/src/services/financeJournal.service.js) (+118 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **`hasOpeningJournal`**: Mengecek apakah jurnal saldo awal sudah pernah dibuat (hanya boleh sekali seumur sistem).
  - **`reverseAdjustmentJournal`**: Membalik jurnal saldo awal atau koreksi manual dengan membuat jurnal balik. Jurnal dari transaksi/transfer/pembayaran ditolak di sini.

##### Perubahan [`financeTransaction.service.js`](backend/src/services/financeTransaction.service.js)

- **Komponen yang Berubah**:
  - [`backend/src/services/financeTransaction.service.js`](backend/src/services/financeTransaction.service.js) (+32 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Mendukung pihak lawan terstruktur (`customer`, `partner`, `employee`, `vendor`) saat membuat transaksi baru.

##### Perubahan [`paymentGateway.service.js`](backend/src/services/paymentGateway.service.js)

- **Komponen yang Berubah**:
  - [`backend/src/services/paymentGateway.service.js`](backend/src/services/paymentGateway.service.js) (+88 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **Konfigurasi dari Database**: Kredensial iPaymu (VA, API key, callback, sandbox) sekarang dibaca dari database (Settings > Finance) alih-alih hardcode dari `.env`. Fallback ke `.env` tetap ada untuk server yang belum dikonfigurasi ulang.
  - **Switch Sandbox/Produksi**: Admin bisa beralih antara sandbox dan produksi tanpa restart server.

##### Perubahan [`financeAccount.service.js`](backend/src/services/financeAccount.service.js)

- **Komponen yang Berubah**:
  - [`backend/src/services/financeAccount.service.js`](backend/src/services/financeAccount.service.js) (+62 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **`ensureIpaymuAccount`**: Otomatis membuat/menautkan akun Kas & Bank untuk payment gateway iPaymu saat konfigurasi gateway disimpan.

##### Perubahan [`option.service.js`](backend/src/services/option.service.js)

- **Komponen yang Berubah**:
  - [`backend/src/services/option.service.js`](backend/src/services/option.service.js) (+45 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Mendefinisikan konstanta `APPLICATION_SETTINGS_LEGACY_MIGRATED` untuk field-field yang dipindah dari Application ke Finance settings.

##### Perubahan Lainnya

- [`backend/src/services/customer.service.js`](backend/src/services/customer.service.js) — `findCustomerIdsBySearch` untuk pencarian pihak.
- [`backend/src/services/partner.service.js`](backend/src/services/partner.service.js) — `findPartnerIdsBySearch` untuk pencarian pihak.
- [`backend/src/services/vendor.service.js`](backend/src/services/vendor.service.js) — `findVendorIdsBySearch` untuk pencarian pihak.
- [`backend/src/services/financeLogs.service.js`](backend/src/services/financeLogs.service.js) — Mendukung field `employee` dan `vendor`.
- [`backend/src/services/financeTransfer.service.js`](backend/src/services/financeTransfer.service.js) — Penyesuaian kecil.

---

#### 🔧 Backend — Controller Layer

##### Perubahan [`financeSettings.controller.js`](backend/src/controllers/financeSettings.controller.js)

- **Komponen yang Berubah**:
  - [`backend/src/controllers/financeSettings.controller.js`](backend/src/controllers/financeSettings.controller.js) (+122 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **Sinkronisasi ke V1**: `syncMigratedFieldsToLegacyAppSettings` — best-effort sinkronisasi field yang dipindah ke `application_settings` lama agar V1 tetap melihat perubahan.
  - **Penanganan Boolean/Numeric**: Field boolean (`allow_negative_balance`, `ipay_sanbox`, `payment_link_active`, dll.) dan numerik (`transaction_approval_threshold`, `inv_auto_due_day`, `inv_auto_tax`) disisihkan sebelum `cleanFormData` agar nilai `false` dan `0` tidak terbuang.
  - **Otomasional Akun iPaymu**: Saat menyimpan konfigurasi iPaymu, otomatis membuat/menautkan akun Kas & Bank gateway.

##### Perubahan [`financeJournal.controller.js`](backend/src/controllers/financeJournal.controller.js)

- **Komponen yang Berubah**:
  - [`backend/src/controllers/financeJournal.controller.js`](backend/src/controllers/financeJournal.controller.js) (+29 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **`financeJournalOpeningStatus`**: Endpoint baru `GET /finance/journal/opening/status` — mengecek apakah jurnal saldo awal masih tersedia.
  - **`financeJournalReverse`**: Endpoint baru `POST /finance/journal/:journal_id/reverse` — membalik jurnal saldo awal atau koreksi manual dengan alasan wajib.
  - **Filter Pihak**: Mendukung filter "Pihak Terkait" di daftar jurnal via `buildPartySearchOr`.

##### Perubahan [`financeTransaction.controller.js`](backend/src/controllers/financeTransaction.controller.js)

- **Komponen yang Berubah**:
  - [`backend/src/controllers/financeTransaction.controller.js`](backend/src/controllers/financeTransaction.controller.js) (+68 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **Filter Pihak**: Kolom "Pihak Terkait" (`tofrom`) bisa mencari karyawan/pelanggan/mitra/vendor terstruktur maupun teks bebas.
  - **Resolusi Pihak**: `resolveTransactionParty` menyelesaikan `party_type`/`party_id` dari `PartyPicker` menjadi ObjectId asli sebelum disimpan.
  - **Save as Draft**: Mendukung penyimpanan draf transaksi dari form multipart.

##### Perubahan [`financeLedger.controller.js`](backend/src/controllers/financeLedger.controller.js)

- **Komponen yang Berubah**:
  - [`backend/src/controllers/financeLedger.controller.js`](backend/src/controllers/financeLedger.controller.js) (+19 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Filter "Pihak Terkait" di mutasi kas & bank.

##### Perubahan [`financeCoa.controller.js`](backend/src/controllers/financeCoa.controller.js)

- **Komponen yang Berubah**:
  - [`backend/src/controllers/financeCoa.controller.js`](backend/src/controllers/financeCoa.controller.js) (+20 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **`financeCoaSeedStatus`**: Endpoint baru `GET /finance/coa/seed/status` — mengecek apakah bagan akun bawaan masih perlu disiapkan.

##### Perubahan Lainnya

- [`backend/src/controllers/settings.controller.js`](backend/src/controllers/settings.controller.js) — Penyesuaian untuk field yang dipindah.
- [`backend/src/controllers/productDataAccess.controller.js`](backend/src/controllers/productDataAccess.controller.js) — Penyesuaian kecil.
- [`backend/src/controllers/productDedicatedInternet.controller.js`](backend/src/controllers/productDedicatedInternet.controller.js) — Penyesuaian kecil.
- [`backend/src/controllers/radiusAuthentication.controller.js`](backend/src/controllers/radiusAuthentication.controller.js) — Penyesuaian kecil.

---

#### 🔧 Backend — Routes & Utility

##### Perubahan [`financeJournal.route.js`](backend/src/routes/financeJournal.route.js)

- **Komponen yang Berubah**:
  - [`backend/src/routes/financeJournal.route.js`](backend/src/routes/financeJournal.route.js) (+74 baris)
- **Deskripsi Perubahan & Fungsi**:
  - 2 endpoint baru dengan dokumentasi Swagger lengkap:
    - `GET /finance/journal/opening/status` — Cek ketersediaan jurnal saldo awal
    - `POST /finance/journal/:journal_id/reverse` — Pembalikan jurnal

##### Perubahan [`financeCoa.route.js`](backend/src/routes/financeCoa.route.js)

- **Komponen yang Berubah**:
  - [`backend/src/routes/financeCoa.route.js`](backend/src/routes/financeCoa.route.js) (+23 baris)
- **Deskripsi Perubahan & Fungsi**:
  - 1 endpoint baru: `GET /finance/coa/seed/status` — Cek apakah bagan akun bawaan masih perlu disiapkan.

##### File Baru [`party-search.js`](backend/src/utils/party-search.js)

- **Komponen yang Berubah**:
  - [`backend/src/utils/party-search.js`](backend/src/utils/party-search.js) [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Utility reusable `buildPartySearchOr` — mengubah satu teks pencarian menjadi kondisi `$or` MongoDB yang mencakup keempat jenis pihak (karyawan/pelanggan/mitra/vendor) sekaligus teks bebas. Digunakan di controller COA, Journal, Transaction, dan Ledger.

##### Perubahan [`finance-error.js`](backend/src/utils/finance-error.js)

- **Komponen yang Berubah**:
  - [`backend/src/utils/finance-error.js`](backend/src/utils/finance-error.js) (+13 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Tipe error baru untuk mendukung fitur pembalikan jurnal dan validasi pihak.

##### Perubahan Terjemahan

- [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json) — +10 kunci baru
- [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json) — +10 kunci baru

---

#### 🎨 Frontend — Halaman Finance

##### Perubahan [`TransactionDrawer.jsx`](frontend/src/app/pages/finance/transactions/TransactionDrawer.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/transactions/TransactionDrawer.jsx`](frontend/src/app/pages/finance/transactions/TransactionDrawer.jsx) (+185 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **PartyPicker**: Integrasi komponen `PartyPicker` untuk memilih pihak lawan transaksi terstruktur.
  - **Otomatis Total**: Nominal total transaksi sekarang dihitung otomatis dari jumlah rincian (`lines`) — tidak lagi diinput manual, mencegah ketidaksesuaian.
  - **Approval Threshold**: Ambang persetujuan transaksi diambil dari Settings > Finance secara dinamis.
  - **Save as Draft**: Mendukung penyimpanan sebagai draf.

##### Perubahan [`journals/index.jsx`](frontend/src/app/pages/finance/journals/index.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/journals/index.jsx`](frontend/src/app/pages/finance/journals/index.jsx) (+135 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **Cek Status Saldo Awal**: Tombol "Jurnal Saldo Awal" disembunyikan otomatis jika sudah pernah dibuat.
  - **Pembalikan Jurnal**: Dialog konfirmasi pembalikan jurnal dengan field alasan wajib.
  - **Trial Balance Dipindah**: Neraca Saldo dipindah ke halaman Reports (bukan lagi inline di halaman Jurnal).

##### Perubahan [`journals/detail.jsx`](frontend/src/app/pages/finance/journals/detail.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/journals/detail.jsx`](frontend/src/app/pages/finance/journals/detail.jsx) (+74 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Tampilan detail jurnal yang lebih kaya — menampilkan sumber jurnal, pihak terkait, dan status pembalikan.

##### Perubahan [`journals/schema/columns.jsx`](frontend/src/app/pages/finance/journals/schema/columns.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/journals/schema/columns.jsx`](frontend/src/app/pages/finance/journals/schema/columns.jsx) (+98 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Filter sumber jurnal (`transaction`, `transfer`, `invoice`, `manual`, `opening`, dll.).
  - Kolom "Pihak" dengan cell `PartyCell` yang bisa diklik ke profil.

##### Perubahan [`coa/index.jsx`](frontend/src/app/pages/finance/coa/index.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/coa/index.jsx`](frontend/src/app/pages/finance/coa/index.jsx) (+66 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **Cek Status Seed**: Tombol "Siapkan Bagan Akun" disembunyikan otomatis jika semua akun bawaan sudah ada.
  - Tombol "Jurnal Saldo Awal" dipindah ke halaman Jurnal.

##### Perubahan [`coa/schema/columns.jsx`](frontend/src/app/pages/finance/coa/schema/columns.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/coa/schema/columns.jsx`](frontend/src/app/pages/finance/coa/schema/columns.jsx) (+88 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Kolom kode sekarang berupa link ke halaman detail COA.
  - Filter berdasarkan status, tipe, dan subtipe akun.
  - Badge subtipe akun dengan warna sesuai kategori.

##### File Baru [`coa/detail/`](frontend/src/app/pages/finance/coa/detail/)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/coa/detail/`](frontend/src/app/pages/finance/coa/detail/) [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Halaman detail COA baru — menampilkan informasi lengkap akun beserta buku besar transaksional.

##### File Baru [`ledger/detail.jsx`](frontend/src/app/pages/finance/ledger/detail.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/ledger/detail.jsx`](frontend/src/app/pages/finance/ledger/detail.jsx) [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Halaman detail mutasi kas & bank.

##### Perubahan [`reports/BalanceSheet.jsx`](frontend/src/app/pages/finance/reports/BalanceSheet.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/reports/BalanceSheet.jsx`](frontend/src/app/pages/finance/reports/BalanceSheet.jsx) (+28 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Peningkatan tampilan laporan Neraca.

##### File Baru [`reports/TrialBalance.jsx`](frontend/src/app/pages/finance/reports/TrialBalance.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/reports/TrialBalance.jsx`](frontend/src/app/pages/finance/reports/TrialBalance.jsx) [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Neraca Saldo dipindah dari halaman Jurnal ke halaman Reports sebagai komponen terpisah.

##### File Dihapus [`journals/TrialBalance.jsx`](frontend/src/app/pages/finance/journals/TrialBalance.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/journals/TrialBalance.jsx`](frontend/src/app/pages/finance/journals/TrialBalance.jsx) [DELETED]
- **Deskripsi Perubahan & Fungsi**:
  - Dihapus karena sudah dipindah ke `reports/TrialBalance.jsx`.

##### Perubahan Halaman Lainnya

- [`accounts/AccountDrawer.jsx`](frontend/src/app/pages/finance/accounts/AccountDrawer.jsx) — Penyesuaian field pihak.
- [`accounts/schema/columns.jsx`](frontend/src/app/pages/finance/accounts/schema/columns.jsx) — Peningkatan kolom.
- [`accounts/schema/ledgerColumns.jsx`](frontend/src/app/pages/finance/accounts/schema/ledgerColumns.jsx) — Peningkatan kolom.
- [`invoices/PaymentDrawer.jsx`](frontend/src/app/pages/finance/invoices/PaymentDrawer.jsx) — Penyesuaian.
- [`invoices/detail.jsx`](frontend/src/app/pages/finance/invoices/detail.jsx) — Penyesuaian.
- [`invoices/schema/columns.jsx`](frontend/src/app/pages/finance/invoices/schema/columns.jsx) — Penambahan kolom.
- [`transfers/TransferDrawer.jsx`](frontend/src/app/pages/finance/transfers/TransferDrawer.jsx) — Penyesuaian.
- [`transfers/detail.jsx`](frontend/src/app/pages/finance/transfers/detail.jsx) (+146 baris) — Peningkatan detail transfer.
- [`transfers/schema/columns.jsx`](frontend/src/app/pages/finance/transfers/schema/columns.jsx) — Penambahan kolom.
- [`reconciliation/ImportStatementDrawer.jsx`](frontend/src/app/pages/finance/reconciliation/ImportStatementDrawer.jsx) — Penyesuaian.
- [`reconciliation/schema/columns.jsx`](frontend/src/app/pages/finance/reconciliation/schema/columns.jsx) — Penambahan kolom.
- [`recurring/RecurringDrawer.jsx`](frontend/src/app/pages/finance/recurring/RecurringDrawer.jsx) — Penyesuaian.
- [`recurring/schema/columns.jsx`](frontend/src/app/pages/finance/recurring/schema/columns.jsx) — Penambahan kolom.
- [`ledger/schema/columns.jsx`](frontend/src/app/pages/finance/ledger/schema/columns.jsx) (+41 baris) — Filter pihak dan status rekonsiliasi.
- [`journals/ManualJournalDrawer.jsx`](frontend/src/app/pages/finance/journals/ManualJournalDrawer.jsx) — Penyesuaian.
- [`journals/OpeningJournalDrawer.jsx`](frontend/src/app/pages/finance/journals/OpeningJournalDrawer.jsx) — Dipindah dari `coa/`.

---

#### 🎨 Frontend — Pengaturan

##### Perubahan [`Finance.jsx`](frontend/src/app/pages/settings/sections/Finance.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/settings/sections/Finance.jsx`](frontend/src/app/pages/settings/sections/Finance.jsx) (+227 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **Penggabungan Settings**: Tab "Tagihan" (default catatan faktur, jatuh tempo, pajak) dan tab "iPay" (konfigurasi payment gateway) dipindah dari Settings > Application ke Settings > Finance.
  - **iPaymu Live Config**: Form lengkap untuk mengatur VA, API key (write-only), sandbox/produksi, callback URL, status payment link, dan cetak payment link.
  - **Otomasional Akun Gateway**: Saat menyimpan konfigurasi iPaymu, akun Kas & Bank gateway otomatis dibuat/ditautkan.

##### Perubahan [`Application.jsx`](frontend/src/app/pages/settings/sections/Application.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/settings/sections/Application.jsx`](frontend/src/app/pages/settings/sections/Application.jsx) (-191 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Tab "Tagihan" dan tab "iPay" dihapus karena sudah dipindah ke Settings > Finance.

---

#### 🎨 Frontend — Komponen Shared

##### File Baru [`PartyPicker.jsx`](frontend/src/components/shared/form/PartyPicker.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/components/shared/form/PartyPicker.jsx`](frontend/src/components/shared/form/PartyPicker.jsx) [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Komponen picker pihak terstruktur (Karyawan/Pelanggan/Vendor) yang bisa dipakai di form transaksi. Mengembalikan `party_type` dan `party_id` untuk diselesaikan di backend.

##### Perubahan [`rows.jsx`](frontend/src/components/shared/table/rows.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx) (+188 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **`VendorLink`**: Link teks ke profil vendor (vendor tidak punya avatar).
  - **`PartyCell`**: Cell "Pihak" yang mendukung dua bentuk data: dokumen ter-populate (`customer`/`employee`/`vendor` sebagai objek) DAN bentuk agregasi `party: {type, humanId, name}`. Menampilkan link klik-ke-profil jika ada, fallback ke `recipient_name`.

##### Perubahan [`status.js`](frontend/src/components/shared/table/status.js)

- **Komponen yang Berubah**:
  - [`frontend/src/components/shared/table/status.js`](frontend/src/components/shared/table/status.js) (+214 baris)
- **Deskripsi Perubahan & Fungsi**:
  - **`financeJournalSourceOptions`**: Opsi sumber jurnal (transaction, transfer, invoice, payment, manual, adjustment, opening, migration) dengan ikon dan warna.
  - **`financeCoaTypeOptions`**: Opsi tipe akun (asset, liability, equity, revenue, expense).
  - **`financeCoaSubtypeOptions`**: Opsi subtipe akun (cash_bank, receivable, inventory, dll.) — 19 opsi.
  - **`financeRecurringIntervalOptions`**: Opsi interval transaksi berulang (daily, weekly, monthly, quarterly, semiannual, yearly).
  - **`financeInvoiceReclassificationOptions`**: Opsi status reklasifikasi faktur.

##### Perubahan [`Table.jsx`](frontend/src/components/shared/table/Table.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/components/shared/table/Table.jsx`](frontend/src/components/shared/table/Table.jsx) (+32 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Peningkatan tabel untuk mendukung filter baru.

##### Perubahan [`Toolbar.jsx`](frontend/src/components/shared/table/Toolbar.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/components/shared/table/Toolbar.jsx`](frontend/src/components/shared/table/Toolbar.jsx) (+66 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Peningkatan toolbar untuk mendukung filter kolom relasi.

##### Perubahan [`FormInput.jsx`](frontend/src/components/shared/form/FormInput.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/components/shared/form/FormInput.jsx`](frontend/src/components/shared/form/FormInput.jsx) (+2 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Penambahan kecil untuk mendukung kebutuhan form baru.

---

#### 🎨 Frontend — Router & Terjemahan

##### Perubahan [`coa.jsx`](frontend/src/app/router/finance/coa.jsx)

- **Komponen yang Berubah**:
  - [`frontend/src/app/router/finance/coa.jsx`](frontend/src/app/router/finance/coa.jsx) (+9 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Route baru untuk halaman detail COA (`/finance/coa/view/:coa_id`).

##### Perubahan Terjemahan

- [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) — +119 kunci baru
- [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) — +121 kunci baru

---

#### 📁 File Lainnya

- [`V1_COMPAT_DEBT.md`](V1_COMPAT_DEBT.md) [NEW] — Dokumentasi utang kompatibilitas V1.
- [`backend/verify-party-search.mjs`](backend/verify-party-search.mjs) [NEW] — Skrip verifikasi utilitas party-search.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Area                   | Judul                       | Dampak Utama                                                                                                                   |
| ---------------------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Party Tracking         | Pelacakan Pihak Terstruktur | Setiap transaksi, jurnal, dan mutasi kas/bank sekarang bisa melacak pihak lawan secara terstruktur (Karyawan/Pelanggan/Vendor) |
| Journal Reversal       | Pembalikan Jurnal           | Jurnal Saldo Awal dan Koreksi Manual bisa dibalik dengan alasan wajib                                                          |
| Settings Consolidation | Konsolidasi Pengaturan      | Pengaturan Tagihan dan iPay dipindah dari Application ke Finance                                                               |
| Payment Gateway        | Konfigurasi Gateway dari DB | Kredensial iPaymu bisa diubah tanpa restart server                                                                             |
| COA Enhancement        | Peningkatan Bagan Akun      | Saldo kelompok akun dihitung benar, halaman detail COA baru                                                                    |
| Party Search           | Pencarian Pihak Silang      | Satu kotak pencarian bisa menemukan karyawan/pelanggan/mitra/vendor sekaligus                                                  |

### Kemampuan Baru Pengguna/Admin

- Admin bisa memilih **pihak lawan terstruktur** (Karyawan/Pelanggan/Vendor) saat membuat transaksi keuangan, bukan hanya teks bebas.
- Admin bisa **membalik jurnal** Saldo Awal atau Koreksi Manual dengan alasan wajib, langsung dari halaman Jurnal.
- Admin bisa **mengatur konfigurasi payment gateway iPaymu** (VA, API key, sandbox/produksi) dari halaman Settings > Finance tanpa restart server.
- Admin bisa melihat **detail halaman COA** dengan buku besar transaksional lengkap.
- Admin bisa **mencari pihak terkait** di seluruh tabel keuangan (transaksi, jurnal, mutasi kas, buku besar COA) dengan satu kotak pencarian.
- Saldo akun kelompok induk sekarang **dihitung benar** dari gabungan seluruh akun anak/cucunya.

### Bug Fix / Solusi Masalah

- **Nilai `false` dan `0` tidak lagi terbuang** oleh `cleanFormData` untuk field finance settings (switch nonaktif, pajak 0%, threshold 0).
- **Kontinuitas konfigurasi V1**: Field yang dipindah dari Application ke Finance settings tetap dibaca dari lokasi lama sebagai fallback, mencegah hilangnya pengaturan produksi.
- **Nominal transaksi selalu sinkron** dengan jumlah rincian karena dihitung otomatis dari `lines`.

### Menu/Fitur Baru

- **Halaman Detail COA** (`/finance/coa/view/:coa_id`) — informasi lengkap akun beserta buku besar.
- **Halaman Detail Mutasi Kas & Bank** — tampilan detail mutasi.
- **Neraca Saldo dipindah ke Reports** — konsolidasi laporan di satu lokasi.
- **Tab Tagihan & iPay di Settings > Finance** — semua pengaturan keuangan di satu tempat.
- **Kolom "Pihak" di semua tabel keuangan** — filter dan tampilan pihak terstruktur.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### Pelacakan Pihak Terstruktur

- **Penjelasan Fitur**: Setiap transaksi keuangan sekarang bisa dilengkapi dengan pihak lawan terstruktur (Karyawan, Pelanggan, atau Vendor) menggunakan komponen `PartyPicker`. Pihak ini ditampilkan di kolom "Pihak" pada tabel transaksi, jurnal, mutasi kas, dan buku besar COA. Pencarian pihak bekerja lintas semua jenis — satu kotak pencarian bisa menemukan baris yang tertaut ke karyawan, pelanggan, vendor, maupun teks bebas.

- **Langkah Penggunaan**:
  1. Buka halaman **Finance > Transactions** dan klik tombol tambah transaksi.
  2. Di form transaksi, cari field **"Pihak Terkait"** (PartyPicker).
  3. Ketik nama karyawan/pelanggan/vendor — komponen akan menampilkan saran yang sesuai.
  4. Pilih pihak yang diinginkan, lalu lengkapi rincian transaksi dan simpan.
  5. Di tabel transaksi, gunakan filter kolom "Pihak" untuk mencari transaksi berdasarkan pihak tertentu.

### Pembalikan Jurnal

- **Penjelasan Fitur**: Jurnal Saldo Awal dan Koreksi Manual bisa dibalik menggunakan fitur "Reverse" di halaman Jurnal. Pembalikan selalu membuat jurnal baru (jurnal balik) dengan alasan wajib — tidak pernah menghapus atau mengubah jurnal asli. Hanya jurnal dengan source `opening` atau `manual` yang berstatus `posted` dan belum pernah dibalik yang bisa dibalik.

- **Langkah Penggunaan**:
  1. Buka halaman **Finance > Journals**.
  2. Klik tombol **"Reverse"** pada baris jurnal yang ingin dibalik.
  3. Isi **alasan pembalikan** (wajib).
  4. Konfirmasi — jurnal balik akan dibuat secara otomatis.
  5. Jurnal asli tetap ada di daftar dengan status normal; jurnal balik akan muncul sebagai entri baru.

### Pengaturan Payment Gateway dari Database

- **Penjelasan Fitur**: Konfigurasi iPaymu (Virtual Account, API Key, Sandbox/Produksi, Callback URL) sekarang bisa diatur dari halaman **Settings > Finance** tanpa perlu mengedit `.env` atau restart server. Kredensial yang belum pernah diisi lewat form akan fallback ke nilai `.env` lama untuk menjaga kompatibilitas.

- **Langkah Penggunaan**:
  1. Buka **Settings > Finance**.
  2. Scroll ke bagian **"Payment Gateway (iPaymu)"**.
  3. Isi nomor Virtual Account, API Key (hanya perlu diisi saat mengganti), pilih Sandbox atau Produksi.
  4. Atur Callback URL, aktifkan/nonaktifkan Payment Link, dan opsi cetak.
  5. Klik **Simpan** — akun Kas & Bank untuk gateway akan otomatis dibuat/ditautkan.
