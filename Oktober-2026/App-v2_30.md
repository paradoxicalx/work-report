# 📝 Daily Work Report - Dedy S.N Putra (30 Oktober 2026)

---

## 📅 Laporan Harian - 30 Oktober 2026

---

## 🌿 Branch: `master` & `production` — Pembaruan Catatan Rilis (Changelog v1.98.35 – v1.98.38), Sinkronisasi AI Knowledge, dan Perbaikan Build Frontend

### 📌 Informasi Pembaruan

- **Nomor Versi**: `v1.98.35`, `v1.98.36`, `v1.98.37`, `v1.98.38` (Total 4 rilis tercakup)
- **Judul Rilis**: Sinkronisasi Catatan Rilis Dekasimal V2, AI Knowledge Base, dan Perbaikan Build Frontend
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `production` pada Jumat, 30 Oktober 2026, 21:18:22 WIB via commit `62a1c78c`)

### 📅 Rincian Commit

#### [62a1c78c] - Merge branch 'master' into production - Jumat, 30 Oktober 2026, 21:18:22 WIB

_(Merge commit menyinkronkan seluruh pembaruan kode, fitur, dan perbaikan dari branch `master` ke branch `production`)_

---

#### [445894f3] - fix build - Jumat, 30 Oktober 2026, 21:17:29 WIB

- **Komponen yang Berubah**:
  - `frontend/src/app/pages/services/dataAccess/detail.jsx`
  - `frontend/src/app/pages/services/dedicatedInternet/detail.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Pembersihan Import Tidak Terpakai pada Komponen Detail Layanan**:
    - Menghapus impor fungsi utilitas `randomId` yang tidak digunakan pada halaman detail Layanan Akses Data (`dataAccess/detail.jsx`) dan Layanan Dedicated Internet (`dedicatedInternet/detail.jsx`).
    - Menyelesaikan peringatan linting (*unused variables*) dan menjamin kelancaran proses kompilasi bundel (*production build*) Vite di lingkungan CI/CD.

---

#### [336c017b] - Merge branch 'master' into production - Jumat, 30 Oktober 2026, 19:51:31 WIB

_(Merge commit menyinkronkan hasil rilis catatan versi v1.98.35 hingga v1.98.38 ke branch `production`)_

---

#### [3a7b4e07] - update changelog - Jumat, 30 Oktober 2026, 19:51:17 WIB

- **Komponen yang Berubah**:
  - `backend/src/data/aiKnowledge/features.json`
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-490.json` [NEW]
  - `backend/src/data/changelog/releases/issue-493.json` [NEW]
  - `backend/src/data/changelog/releases/issue-497.json` [NEW]
  - `backend/src/data/changelog/releases/issue-498.json` [NEW]
  - `backend/src/data/changelog/releases/issue-501.json` [NEW]
  - `backend/src/data/changelog/releases/issue-502.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Generasi Berkas Detail Rilis Changelog**:
    - Menghasilkan 6 berkas rilis terstruktur berbasis JSON di direktori `backend/src/data/changelog/releases/` untuk issue #490 (`v1.98.34`), issue #493 (`v1.98.33`), issue #497 (`v1.98.36`), issue #498 (`v1.98.35`), issue #501 (`v1.98.38`), dan issue #502 (`v1.98.37`).
    - Menyusun dokumentasi rilis yang ramah pengguna, merinci fitur baru, bug fix, dan peningkatan stabilitas sistem tanpa jargon teknis internal.
  - **Pembaruan Indeks Metadata Rilis (`index.json`)**:
    - Memperbarui berkas `backend/src/data/changelog/index.json` dengan menambahkan entri rilis terbaru di posisi teratas secara kronologis mundur.
    - Menyediakan teks pencarian komprehensif (`searchText`) pada setiap entri rilis untuk mendukung fitur penelusuran cepat pada antarmuka Catatan Rilis admin.
  - **Sinkronisasi AI Knowledge Base (`features.json`)**:
    - Memperbarui dan menyelaraskan basis pengetahuan AI internal pada `backend/src/data/aiKnowledge/features.json` dengan penambahan informasi fitur terbaru seputar penandatanganan dokumen publik BAPL & BAP, integrasi faktur partner API, validasi fleksibel tiket instalasi, dan kalkulasi DPP pada faktur keuangan.

---

## 🌿 Branch: `issue-501` — Kalkulasi Potongan Pajak Berbasis DPP dan Validasi Batas Potongan Faktur

### 📌 Informasi Issue

- **Nomor Issue**: #501
- **Judul Issue**: Kalkulasi Potongan Pajak Berbasis DPP dan Validasi Batas Potongan Faktur Tagihan
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Jumat, 30 Oktober 2026, 19:39:51 WIB via commit `00f139f6`)

### 📅 Rincian Commit

#### [00f139f6] - resolve #501 - Jumat, 30 Oktober 2026, 19:39:51 WIB

_(Merge commit menggabungkan branch `issue-501` ke branch `master`)_

---

#### [cc194cdb] - resolve #501 - Jumat, 30 Oktober 2026, 18:22:43 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/routes/financeInvoice.route.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/test/integration/financeInvoice.deduction.test.js`
  - `frontend/src/app/pages/finance/invoices/InvoiceDrawer.jsx`
  - `frontend/src/app/pages/finance/invoices/InvoiceItemsField.jsx`
  - `frontend/src/app/pages/finance/invoices/utils/invoiceDeductions.js`
  - `frontend/src/app/pages/finance/invoices/utils/invoiceDeductions.test.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Kalkulasi Potongan Pajak Presisi Berbasis DPP (Dasar Pengenaan Pajak)**:
    - Memperbarui formula perhitungan potongan biaya persentase pajak (seperti potongan PPh Pasal 23 sebesar 2%) pada backend (`financeInvoice.service.js`) dan frontend (`invoiceDeductions.js`) agar dihitung berdasarkan nilai DPP murni (sebelum penambahan PPN), bukan dari total kotor tagihan.
    - Menjamin keselarasan angka potongan pajak dan nilai tagihan sesuai kaidah perpajakan dan audit akuntansi.
  - **Penyajian Baris Ringkasan DPP Transparan pada Laci Faktur**:
    - Menghadirkan baris ringkasan Dasar Pengenaan Pajak (DPP) pada rincian faktur ketika terdapat potongan persentase pajak yang aktif, memberikan visibilitas penuh kepada tim keuangan.
  - **Validasi Batas Maksimum Potongan Faktur**:
    - Menambahkan validasi ketat pada formulir faktur untuk mencegah pengiriman dokumen jika total potongan melebihi nilai total tagihan berpajak.
    - Mencegah inkonsistensi matematis antara total saldo pengurang dan rincian baris potongan yang tersimpan pada tagihan.
  - **Stabilitas Pemuatan Katalog Layanan Mitra**:
    - Memperbaiki penanganan pemuatan katalog layanan mitra pada `InvoiceDrawer.jsx` agar tidak mengalami kedip (*flicker*) atau status yang tidak konsisten saat beralih pilihan penagihan pada laci faktur tagihan.
  - **Pengujian Terpadu & Dokumentasi**:
    - Menyusun rangkaian tes integrasi `financeInvoice.deduction.test.js` dan unit test `invoiceDeductions.test.js` untuk memvalidasi akurasi perhitungan DPP, batasan potongan, dan potongan bertingkat.
    - Memperbarui panduan dokumentasi `AGENTS.md` serta pesan multibahasa i18n untuk peringatan batas potongan.

---

## 🌿 Branch: `issue-502` — Tanda Tangan Publik Berita Acara Pemasangan dan Penyesuaian Navigasi

### 📌 Informasi Issue

- **Nomor Issue**: #502
- **Judul Issue**: Tanda Tangan Publik Berita Acara Pemasangan (BAP) dan Penyesuaian Navigasi Mitra
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Jumat, 30 Oktober 2026, 19:04:03 WIB via commit `e7be8217`)

### 📅 Rincian Commit

#### [e7be8217] - resolve #502 - Jumat, 30 Oktober 2026, 19:04:03 WIB

_(Merge commit menggabungkan branch `issue-502` ke branch `master`)_

---

#### [0e85cc4c] - resolve #502 - Jumat, 30 Oktober 2026, 17:42:37 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/publicDocument.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/middlewares/logger.middleware.js`
  - `backend/src/middlewares/publicBapSignLimiter.middleware.js` [NEW]
  - `backend/src/models/document.model.js`
  - `backend/src/routes/public.route.js`
  - `backend/src/services/document.service.js`
  - `backend/src/services/serviceChangeDocument.service.js`
  - `backend/src/services/ticket.service.js`
  - `backend/src/utils/bapSignInput.js` [NEW]
  - `backend/src/utils/signatureInput.js` [NEW]
  - `backend/test/integration/publicDocumentBap.test.js` [NEW]
  - `backend/test/unit/bapSignInput.test.js` [NEW]
  - `backend/test/unit/loggerSanitizer.test.js` [NEW]
  - `frontend/src/app/pages/services/dataAccess/detail.jsx`
  - `frontend/src/app/pages/services/dedicatedInternet/detail.jsx`
  - `frontend/src/app/pages/tickets/GeneralInformation.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - **Tanda Tangan Publik Mandiri Berita Acara Pemasangan (BAP)**:
    - Mengembangkan endpoint publik mandiri `POST /api/v1/public/document/bap/sign` dan pembacaan berkas publik agar pelanggan dapat menandatangani dokumen BAP instalasi secara mandiri dari perangkat mereka tanpa memerlukan akun login admin.
    - Menerapkan utilitas pembersihan dan validasi payload tanda tangan digital (`bapSignInput.js` dan `signatureInput.js`) untuk memastikan integritas grafis tanda tangan, nama penandatangan, dan cap waktu.
    - Menerapkan pengamanan pembatasan laju (*rate limiting*) via `publicBapSignLimiter.middleware.js` guna mencegah brute-force atau spam penandatanganan berulang.
  - **Redaksi Data Sensitif dan Payload Tanda Tangan pada Logger**:
    - Memperbarui `logger.middleware.js` dan pengujian `loggerSanitizer.test.js` untuk menyensor string grafis Base64 tanda tangan digital dan data PII sensitif (`[REDACTED]`), mencegah pembengkakan log server dan menjaga privasi data.
  - **Penyempurnaan Navigasi Profil Mitra pada Halaman Layanan**:
    - Memperbaiki tautan navigasi dan lencana mitra pada halaman detail Layanan Akses Data dan Dedicated Internet agar secara tepat mengarahkan ke profil Mitra Reseller atau Pelanggan Bisnis sesuai jenis entitas, lengkap dengan verifikasi hak akses privilege.
  - **Penanganan Kartu Tiket Berelasi Layanan Terhapus**:
    - Memperbaiki tampilan informasi layanan pada kartu tiket instalasi (`GeneralInformation.jsx`) saat data layanan terkait telah dihapus dari basis data agar tidak menampilkan tombol tautan yang rusak (*broken link*).
  - **Pencegahan Nilai Undefined pada Judul Dokumen**:
    - Menghilangkan penulisan string nama yang tidak terdefinisi (`undefined`) pada penamaan berkas dokumen BAP instalasi.

---

## 🌿 Branch: `issue-497` — Manajemen Faktur Mitra dan Penahanan Tagihan Otomatis Menunggak

### 📌 Informasi Issue

- **Nomor Issue**: #497
- **Judul Issue**: Manajemen Faktur Mitra melalui Partner API dan Penahanan Tagihan Otomatis Menunggak
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Jumat, 30 Oktober 2026, 16:00:22 WIB via commit `0d16eccb`)

### 📅 Rincian Commit

#### [0d16eccb] - resolve #497 - Jumat, 30 Oktober 2026, 16:00:22 WIB

_(Merge commit menggabungkan branch `issue-497` ke branch `master`)_

---

#### [c9d0975e] - resolve #497 - Kamis, 29 Oktober 2026, 16:42:08 WIB (Di-merge Jumat, 30 Oktober 2026)

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/controllers/partnerApiInvoice.controller.js`
  - `backend/src/controllers/partnerApiRadius.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/financeAutoInvoice.service.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/utils/finance-error.js`
  - `backend/src/utils/partnerApiInvoiceInput.js` [NEW]
  - `backend/test/integration/financeAutoInvoice.partnerOverdue.test.js` [NEW]
  - `backend/test/integration/partnerApiInvoice.write.test.js` [NEW]
  - `backend/test/integration/partnerApiRadius.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Pengelolaan Faktur Terverifikasi melalui Partner API**:
    - Mendukung penerbitan, penelusuran, dan pembatalan faktur tagihan pelanggan mitra melalui integrasi sistem mitra (`partnerApiInvoice.controller.js`) dengan verifikasi hak akses dan cakupan referensi klien (`scopePartnerApiClientRef`).
    - Menerapkan penyaringan ketat data input pembuatan faktur (`partnerApiInvoiceInput.js`) dengan daftar putih parameter yang aman, menolak injeksi tanggal terbit sembarangan, potongan tak sah, atau data faktur pajak melalui API mitra.
  - **Pencegahan Pembatalan Faktur Berstatus Terbayar**:
    - Mencegah pembatalan faktur melalui integrasi mitra apabila faktur telah menerima pembayaran sebagian atau lunas (`unpaidOnly: true`), menolak dengan kode kesalahan HTTP 422 `INVOICE_HAS_PAYMENT` demi menjaga integritas pembukuan kas.
  - **Proteksi Penumpukan Tagihan Menunggak (Overdue Safeguard)**:
    - Menambahkan validasi pembatasan penerbitan faktur baru (`assertCustomerInvoiceIssuanceAllowed`) jika pelanggan masih memiliki 2 tagihan belum lunas (*unpaid*) atau setidaknya 1 tagihan menunggak yang telah melewati batas tanggal jatuh tempo (*overdue*).
  - **Penahanan Otomatis Tagihan Bulanan Akun Mitra Menunggak**:
    - Memperbarui worker penagihan otomatis (`financeAutoInvoice.service.js`) untuk menahan (*skip*) penerbitan tagihan berkala bagi akun layanan milik mitra (`ref_partner`) jika pelanggan masih memiliki faktur belum lunas yang telah melewati batas tanggal jatuh tempo, mencegah penumpukan tagihan ganda atau akumulasi piutang tak tertagih.

---

## 🌿 Branch: `issue-498` — Pengaturan Tiket Instalasi Opsional Pengguna Bisnis dan Layanan

### 📌 Informasi Issue

- **Nomor Issue**: #498
- **Judul Issue**: Pengaturan Tiket Instalasi Opsional Pengguna Bisnis dan Layanan Dedicated Internet
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` pada Jumat, 30 Oktober 2026, 14:51:59 WIB via commit `d652e3b7`)

### 📅 Rincian Commit

#### [d652e3b7] - resolve #498 - Jumat, 30 Oktober 2026, 14:51:59 WIB

_(Merge commit menggabungkan branch `issue-498` ke branch `master`)_

---

#### [1640032a] - resolve #498 - Kamis, 29 Oktober 2026, 15:42:58 WIB (Di-merge Jumat, 30 Oktober 2026)

- **Komponen yang Berubah**:
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/business.controller.js`
  - `backend/src/controllers/customer.controller.js`
  - `backend/src/controllers/customerPartner.controller.js`
  - `backend/src/controllers/partner.controller.js`
  - `backend/src/controllers/productDedicatedInternet.controller.js`
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/routes/business.route.js`
  - `backend/src/routes/customer.route.js`
  - `backend/src/routes/customerPartner.route.js`
  - `backend/src/routes/partner.route.js`
  - `backend/src/routes/productDedicatedInternet.route.js`
  - `backend/src/routes/radiusAuthentication.route.js`
  - `backend/src/services/option.service.js`
  - `backend/src/services/settingsBackup.registry.js`
  - `backend/src/utils/ticketInput.js` [NEW]
  - `backend/test/integration/capacitySpeed.controller.test.js`
  - `backend/test/integration/ticketOptional.create.test.js` [NEW]
  - `backend/test/unit/ticketInput.test.js` [NEW]
  - `frontend/src/app/pages/services/dedicatedInternet/create.jsx`
  - `frontend/src/app/pages/services/dedicatedInternet/schema/createShema.js`
  - `frontend/src/app/pages/settings/sections/developer/SpecialSettingsTab.jsx`
  - `frontend/src/app/pages/users/business/create.jsx`
  - `frontend/src/app/pages/users/business/schema/createShema.js`
  - `frontend/src/app/pages/users/partner/create.jsx`
  - `frontend/src/app/pages/users/partner/schema/createShema.js`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Opsi Konfigurasi Tiket Instalasi Opsional pada Pengaturan Pengembang**:
    - Menyediakan opsi konfigurasi pada tab **Pengaturan Khusus (Special Settings)** untuk menentukan apakah tiket instalasi bersifat opsional saat mendaftarkan Pelanggan Bisnis, Mitra Bisnis, maupun Layanan Dedicated Internet.
    - Memberikan fleksibilitas operasional bagi registrasi entitas atau layanan yang tidak memerlukan kunjungan instalasi lapangan (migrasi data, aktivasi virtual, dsb.).
  - **Validasi Integritas Tiket Instalasi pada Backend & Frontend**:
    - Mencegah penggunaan tiket instalasi yang statusnya sudah selesai (*closed*), tiket yang bukan bertipe instalasi, atau tiket yang sudah terikat ke akun layanan lain saat mendaftarkan layanan internet.
    - Mencegah pemakaian silang tiket instalasi yang sama antara pelanggan retail dan pelanggan bisnis.
    - Melakukan pembersihan otomatis data isian tiket yang kosong sebelum dikirim ke server melalui `ticketInput.js`.
  - **Ketahanan Formulir & Perbaikan Dropdown IP Pool**:
    - Menangani kondisi kesalahan tanpa objek detail pada formulir pendaftaran pengguna bisnis dan layanan agar tidak menyebabkan kegagalan tampilan antarmuka (*crash*).
    - Memperbaiki pengaturan status pemuatan dan pengosongan daftar IP saat terjadi kendala koneksi atau pemilihan IP Pool.

---

## 🌿 Branch: `issue-486` — Dukungan Multi Payment Gateway & Adaptor Xendit Fase 1b (Penyesuaian Fee, Refund, & Rekonsiliasi)

### 📌 Informasi Issue

- **Nomor Issue**: #486
- **Judul Issue**: Dukungan Multi Payment Gateway dan Adaptor Xendit Fase 1b: Penyesuaian Selisih Fee, Pembalikan Refund, dan Rekonsiliasi Otomatis
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-486`)

### 📅 Rincian Commit

#### [5fe04ee1] - save #486 AGENTS.md dan spec: fee Xendit Fase 1b - Jumat, 30 Oktober 2026, 02:03:52 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
- **Deskripsi Perubahan & Fungsi**:
  - Memperbarui dokumentasi arsitektur dan aturan baku monorepo `AGENTS.md` dengan spesifikasi teknis penyesuaian fee gateway, aturan rekonsiliasi selisih fee, pembukuan PPN masukan fee, dan penanganan pembalikan refund.

---

#### [51a29e24] - save #486 xendit: pesan i18n dan aktivasi dibuka setelah fee lengkap - Jumat, 30 Oktober 2026, 01:48:38 WIB

- **Komponen yang Berubah**:
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/routes/financeSettings.route.js`
  - `backend/src/services/paymentGateways/registry.js`
  - `backend/src/services/paymentGateways/xendit.gateway.js`
  - `backend/test/integration/financeSettings.xendit.test.js`
  - `backend/test/integration/xenditGateway.createPayment.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Pembukaan Aktivasi Gateway Xendit**:
    - Membuka pembatas aktivasi gateway Xendit pada `financeSettings.route.js` dan adaptor `xendit.gateway.js` setelah seluruh kapabilitas perhitungan dan rekonsiliasi fee Fase 1b terpenuhi.
    - Menambahkan pesan multibahasa i18n untuk notifikasi status aktivasi, verifikasi konfigurasi kredensial, dan panduan pengguna.

---

#### [3e7f0a55] - save #486 pemulihan refund: hentikan alert berulang - Jumat, 30 Oktober 2026, 01:40:38 WIB

- **Komponen yang Berubah**:
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeGatewayFee.service.js`
  - `backend/test/integration/financeGatewayFee.refund.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Mitigasi Peringatan Berulang pada Pemulihan Refund**:
    - Menambahkan penanda pelacakan kegagalan pemulihan refund pada model transaksi gateway guna menghentikan pengiriman notifikasi/alert berulang (*deduplicated alert*) ke pengelola sistem saat terjadi kendala koneksi atau proses pembalikan refund yang tertunda.

---

#### [b016566c] - save #486 pemulihan refund gateway yang tertunda - Jumat, 30 Oktober 2026, 01:35:01 WIB

- **Komponen yang Berubah**:
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeGatewayFee.service.js`
  - `backend/test/integration/financeGatewayFee.refund.test.js`
  - `backend/test/integration/financeGatewayFee.sweep.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Mekanisme Pemulihan Otomatis Refund Tertunda**:
    - Membangun mekanisme pemulihan (*recovery retry*) untuk transaksi refund gateway yang tertunda atau mengalami gangguan sementara, menjamin status pengembalian dana terselesaikan secara konsisten.

---

#### [757b221f] - save #486 refund gateway membalik penyesuaian fee - Jumat, 30 Oktober 2026, 01:24:47 WIB

- **Komponen yang Berubah**:
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeGatewayFee.service.js`
  - `backend/test/integration/financeGatewayFee.refund.test.js`
  - `backend/test/integration/financeGatewayFee.sweep.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Pembalikan Penyesuaian Fee saat Refund Transaksi**:
    - Menangani pembalikan penyesuaian fee pada `financeGatewayFee.service.js` saat transaksi pembayaran di-refund, memastikan jurnal beban fee dan saldo rekening gateway dikoreksi secara proporsional.

---

#### [4c338f15] - save #486 sapuan fee gateway tertunda - Jumat, 30 Oktober 2026, 01:16:18 WIB

- **Komponen yang Berubah**:
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeGatewayFee.service.js`
  - `backend/test/integration/financeGatewayFee.adjust.test.js`
  - `backend/test/integration/financeGatewayFee.sweep.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Penyapuan Fee Gateway Tertunda (Pending Fee Sweep)**:
    - Mengimplementasikan proses sapuan (*sweep worker*) untuk mendeteksi transaksi dengan biaya gateway tertunda (*fee_pending*), menyinkronkan data biaya riil dari penyedia gateway, dan melakukan penjurnalan penyesuaian secara otomatis.

---

#### [14fd1cae] - save #486 penyesuaian fee gateway: jurnal selisih idempoten - Jumat, 30 Oktober 2026, 01:02:34 WIB

- **Komponen yang Berubah**:
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeGatewayTransaction.model.js`
  - `backend/src/services/financeGateway.service.js`
  - `backend/src/services/financeGatewayFee.service.js` [NEW]
  - `backend/src/services/financePayment.service.js`
  - `backend/test/integration/financeGatewayFee.adjust.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Penjurnalan Selisih Fee Idempoten**:
    - Membuat layanan `financeGatewayFee.service.js` untuk membukukan selisih antara estimasi awal fee dan biaya aktual yang dipotong payment gateway ke dalam jurnal akuntansi.
    - Menggunakan mekanisme idempoten berbasis status transaksi cicilan buku besar agar penyesuaian berulang tidak menyebabkan duplikasi beban keuangan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #501 | Kalkulasi Potongan Pajak Berbasis DPP & Batas Potongan Faktur | Menghitung potongan persentase pajak (PPh 23) berbasis DPP murni, menampilkan baris ringkasan DPP transparan pada rincian tagihan, serta memvalidasi batas maksimum potongan agar tidak melebihi tagihan kena pajak. |
| #502 | Tanda Tangan Publik Berita Acara Pemasangan (BAP) & Navigasi Mitra | Menyediakan tautan publik mandiri bagi pelanggan untuk menandatangani dokumen BAP instalasi langsung dari peramban mereka tanpa login, dilengkapi pembatasan laju (*rate limiting*), sensor log tanda tangan, dan perbaikan navigasi profil mitra. |
| #497 | Manajemen Faktur Mitra & Penahanan Tagihan Otomatis Menunggak | Membuka API manajemen faktur pelanggan mitra melalui Partner API, mencegah pembatalan faktur yang telah berbayar, dan menahan penagihan otomatis berkala bagi akun mitra yang memiliki tunggakan lewat jatuh tempo. |
| #498 | Pengaturan Tiket Instalasi Opsional Pengguna Bisnis & Layanan | Memberikan opsi konfigurasi khusus untuk membebaskan kewajiban tiket instalasi pada pendaftaran Pelanggan Bisnis, Mitra Bisnis, dan Layanan Dedicated Internet, disertai penguatan validasi tiket di server dan antarmuka. |
| #486 | Dukungan Multi Payment Gateway & Adaptor Xendit Fase 1b | Menyempurnakan rekonsiliasi selisih fee gateway idempoten, sapuan fee tertunda (*pending fee sweep*), pembalikan fee saat refund, dan membuka opsi aktivasi gateway Xendit setelah seluruh persyaratan fee terpenuhi. |
| Master / Prod | Pembaruan Catatan Rilis & Perbaikan Build Frontend | Menghasilkan berkas rilis v1.98.35–v1.98.38, memperbarui indeks pencarian changelog, sinkronisasi AI knowledge base, serta membersihkan impor tidak terpakai pada detail layanan untuk kelancaran build frontend. |

### Kemampuan Baru Pengguna/Admin

- **Tanda Tangan Publik Mandiri BAP Instalasi**: Pelanggan kini dapat menandatangani Berita Acara Pemasangan (BAP) secara mandiri melalui tautan web publik aman yang dikirimkan petugas, tanpa perlu memiliki akun login admin atau mengunduh aplikasi tambahan.
- **Kalkulasi Potongan Pajak Berbasis DPP**: Bagian keuangan (*finance / accounting*) kini mendapatkan kalkulasi pemotongan pajak persentase (PPh Pasal 23) yang presisi berbasis DPP murni (bukan nilai bruto setelah PPN), lengkap dengan baris ringkasan Dasar Pengenaan Pajak pada laci faktur tagihan.
- **Pengaturan Fleksibilitas Tiket Instalasi**: Staf administrator kini dapat mendaftarkan Pelanggan Bisnis, Mitra Bisnis, maupun Layanan Dedicated Internet tanpa kewajiban melampirkan tiket instalasi apabila opsi tiket opsional telah diaktifkan pada Pengaturan Khusus.
- **Pengelolaan Faktur Terpadu via Partner API**: Mitra terintegrasi kini dapat menerbitkan, memeriksa, dan membatalkan faktur tagihan pelanggan mereka langsung dari sistem mitra melalui API resmi yang aman dan terisolasi.

### Bug Fix / Solusi Masalah

- **Pencegahan Pengiriman Formulir Faktur dengan Potongan Berlebih**: Memperbaiki validasi formulir agar sistem menolak penyimpanan faktur jika total nilai potongan melampaui nilai tagihan kena pajak.
- **Pencegahan Pembatalan Faktur Berbayar via Partner API**: Mencegah mitra membatalkan faktur yang telah menerima pembayaran sebagian atau lunas, melindungi keabsahan laporan arus kas keuangan.
- **Penahanan Otomatis Tagihan Menunggak Akun Mitra**: Menghentikan penumpukan tagihan baru pada akun layanan milik mitra jika pelanggan masih memiliki faktur belum lunas yang telah melewati batas tanggal jatuh tempo.
- **Pencegahan Pemakaian Tiket Selesai atau Silang Jenis**: Memastikan tiket instalasi yang berstatus selesai (*closed*), berjenis non-instalasi, atau telah terhubung ke layanan lain tidak dapat dipakai ulang secara keliru.
- **Perbaikan Tautan Profil Mitra pada Halaman Layanan**: Menyesuaikan tautan lencana mitra pada detail Layanan Akses Data dan Dedicated Internet agar mengarah secara akurat ke halaman profil Mitra Reseller atau Pelanggan Bisnis sesuai tipe entitasnya.
- **Pembersihan Judul Dokumen BAP**: Menghilangkan tampilan string `undefined` pada judul dan nama file Berita Acara Pemasangan instalasi.
- **Pembersihan Impor Build Frontend**: Menghapus deklarasi impor tidak terpakai `randomId` pada halaman detail layanan guna mencegah kegagalan proses kompilasi bundel produksi.

### Menu/Fitur Baru

- **Halaman Tanda Tangan Publik BAP**: Tautan publik mandiri bagi pelanggan untuk menandatangani dokumen penerimaan Berita Acara Pemasangan instalasi secara digital.
- **Pengaturan Tiket Instalasi Opsional**: Tersedia di menu **Pengaturan > Developer > Pengaturan Khusus (Special Settings)**.
- **Baris Ringkasan DPP pada Faktur**: Ditampilkan otomatis pada laci formulir faktur di menu **Keuangan > Tagihan & Faktur** saat potongan persentase pajak aktif.
- **Endpoint Partner API Invoice**: Jalur integrasi baru untuk penerbitan dan pengelolaan faktur pelanggan mitra.
- **Rekonsiliasi Fee & Pengaturan Gateway Xendit**: Modul penyesuaian biaya selisih fee transaksi dan opsi aktivasi gateway Xendit di menu **Pengaturan > Keuangan > Pembayaran Online**.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penandatanganan Mandiri Berita Acara Pemasangan (BAP) oleh Pelanggan Melalui Tautan Publik

- **Penjelasan Fitur**:
  - Setelah teknisi menyelesaikan proses instalasi layanan internet di lokasi pelanggan, pelanggan dapat memverifikasi dan menandatangani dokumen BAP instalasi langsung dari smartphone atau komputer mereka melalui tautan publik aman.
  - Halaman publik ini dilengkapi pembatasan laju (*rate limiting*) untuk keamanan, memuat informasi ringkas instalasi tanpa membocorkan data sensitif pengelola, dan otomatis menyensor tanda tangan dari log server.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi admin, lalu buka menu **Tiket > Tiket Instalasi**.
  2. Buka tiket instalasi yang pekerjaannya telah rampung di lapangan.
  3. Pada kartu dokumen instalasi, salin tautan publik dokumen BAP yang tersedia atau bagikan tautan tersebut langsung ke nomor WhatsApp pelanggan.
  4. Pelanggan membuka tautan yang diterima pada peramban (*browser*) ponsel/komputer:
     - Pelanggan membaca rincian Berita Acara Pemasangan.
     - Pelanggan menuliskan nama penandatangan pada kolom nama.
     - Pelanggan membubuhkan tanda tangan digital pada kotak kanvas tanda tangan.
     - Klik tombol **"Kirim Tanda Tangan"**.
  5. Sistem secara otomatis mengunci dokumen BAP menjadi berstatus telah ditandatangani (*signed*), menyimpan gambar tanda tangan, dan memperbarui status dokumen pada sistem Dekasimal V2.

---

### 2. Mengaktifkan dan Menggunakan Fitur Tiket Instalasi Opsional untuk Pelanggan Bisnis dan Dedicated Internet

- **Penjelasan Fitur**:
  - Pada kondisi tertentu seperti migrasi data dari sistem lama, aktivasi port virtual, atau kontrak layanan korporat tanpa kebutuhan kunjungan teknisi lapangan, pembuatan tiket instalasi tidak selalu diperlukan.
  - Fitur ini memberikan kendali kepada pengembang/admin untuk menonaktifkan kewajiban tiket instalasi saat mendaftarkan Pelanggan Bisnis, Mitra Bisnis, maupun Layanan Dedicated Internet.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi admin sebagai pengguna yang memiliki hak akses pengembang (*developer*).
  2. Buka menu **Pengaturan > Developer**, lalu pilih tab **Pengaturan Khusus (Special Settings)**.
  3. Temukan opsi pengaturan:
     - **"Tiket Instalasi Opsional untuk Pelanggan Bisnis"**: Aktifkan toggle jika pendaftaran Pelanggan Bisnis tidak wajib menyertakan nomor tiket instalasi.
     - **"Tiket Instalasi Opsional untuk Dedicated Internet"**: Aktifkan toggle jika pembuatan Layanan Dedicated Internet dapat dilakukan tanpa memilih tiket instalasi.
  4. Klik tombol **"Simpan Pengaturan"**.
  5. Buka menu **Pengguna > Pelanggan Bisnis > Tambah Baru** atau menu **Layanan > Dedicated Internet > Tambah Baru**:
     - Kolom tiket instalasi kini bersifat opsional (tidak lagi berlabel tanda bintang wajib).
     - Admin dapat langsung melengkapi formulir dan menyimpan akun/layanan baru secara lancar.

---

### 3. Pembuatan Faktur Tagihan dengan Potongan Pajak Berbasis DPP (Dasar Pengenaan Pajak)

- **Penjelasan Fitur**:
  - Memastikan potongan biaya persentase pajak (misalnya PPh 23 sebesar 2%) dihitung secara tepat berdasarkan Dasar Pengenaan Pajak (DPP), sehingga nilai pajak PPN dan potongan PPh selaras tanpa deviasi matematis.
  - Menampilkan ringkasan baris DPP dan memvalidasi agar total potongan tidak melebihi nominal tagihan.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Keuangan > Tagihan & Faktur**.
  2. Klik tombol **"Tambah Faktur Tagihan"** untuk membuka laci pembuatan faktur.
  3. Masukkan item layanan yang ditagihkan beserta nilai biayanya dan centang opsi kena pajak jika tagihan dikenakan PPN.
  4. Pada bagian **Potongan Biaya / Deduction**:
     - Tambahkan baris potongan baru, pilih jenis potongan persentase pajak (misalnya *PPh 23* sebesar `2%`).
     - Perhatikan baris ringkasan tagihan: sistem secara otomatis menampilkan nilai **Dasar Pengenaan Pajak (DPP)** dan menghitung nilai potongan `2% × DPP`.
  5. Pastikan total potongan tidak melebihi nilai tagihan kena pajak (sistem akan menampilkan pesan peringatan validasi jika melampaui batas).
  6. Klik tombol **"Simpan Faktur"** untuk menerbitkan tagihan dengan perhitungan pajak yang presisi.

