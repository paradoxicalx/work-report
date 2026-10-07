# 📝 Daily Work Report - Dedy S.N Putra (2026-10-07)

---

## 📅 Laporan Harian - 7 Oktober 2026

> Laporan ini merangkum seluruh aktivitas rekayasa perangkat lunak, audit kode komprehensif, remediasi arsitektur keuangan & akuntansi, integrasi notifikasi omnichannel, serta pemutakhiran riwayat rilis produksi pada monorepo DEKASIMAL V2 per 7 Oktober 2026. Fokus dan pencapaian utama hari ini berpusat pada:
> 1. **Audit Kode Menyeluruh & Remediasi Kritis Modul "Ubah Tanggal Terbit Tagihan Penjualan" (`issue-378`)**: Pelaksanaan audit kode mendalam terhadap implementasi branch `issue-378` yang mencakup integritas data pajak `tax: 0`, eliminasi potensi memory leak pada komponen formulir, optimalisasi *short-circuit* query database kebijakan tanggal terbit, ketahanan evaluasi periode akuntansi terhadap perbedaan zona waktu server (UTC vs lokal), serta penyelarasan skema validasi form dan dokumentasi OpenAPI. Seluruh 10 temuan audit ([BE-01–BE-06] & [FE-01–FE-04]) berhasil diremediasi 100% dan diverifikasi melalui 38 unit test.
> 2. **Pencatatan Riwayat Notifikasi Tagihan WhatsApp & Standardisasi Kamus Hak Akses (`issue-377`)**: Penambahan model data `finance_invoice_whatsapps` untuk pelacakan otomatis riwayat pengiriman invoice ke pelanggan via WhatsApp, pembangunan kartu riwayat notifikasi pada halaman detail faktur, serta sinkronisasi masif kamus hak akses terpusat (*Privilege Dictionary*) mencakup ribuan endpoint backend.
> 3. **Penyempurnaan Pendaftaran Topik Forum Telegram & Resolusi Callback Dinamis (`issue-375`)**: Perbaikan penanganan dokumen pengaturan topik forum Telegram saat inisialisasi awal (mencegah *TypeError* hening), pendaftaran topik atomik bersamaan multi-replika, serta pembacaan URL callback teranyar pada `telegram-api`.
> 4. **Penggabungan Penuh Modul Tiket & Asisten AI WhatsApp ke Master (`issue-373`)**: Penyelesaian form tiket penagihan (*BillingTicketForm*), skrip seeder data sampel, serta penggabungan resmi fitur drawer tiket dan generator PDF vektor ke branch `master`.
> 5. **Pembaruan Berkas Changelog Rilis Sistem Monorepo (`production`)**: Pemutakhiran riwayat rilis terpusat di `backend/src/data/changelog/` untuk 5 rilis sistem (`issue-365`, `issue-368`, `issue-370`, `issue-373`, dan `issue-375`) dan dipublikasikan ke branch `production`.

---

## 🌿 Branch: `issue-378` — Audit Kode & Remediasi Kritis Modul "Ubah Tanggal Terbit Tagihan Penjualan"

### 📌 Informasi Issue

- **Nomor Issue**: #378
- **Judul Issue**: Kebijakan dan Validasi Pengubahan Tanggal Terbit Tagihan Penjualan (Audit Kode Menyeluruh, Remediasi Integritas Data Pajak, Pencegahan Memory Leak, Optimasi Kueri Kebijakan Tanggal, & Ketahanan Zona Waktu Akuntansi)
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-378`, 10 butir tindakan remediasi audit terselesaikan 100%, 38 pengujian otomatis lulus)

### 📅 Rincian Aktivitas & Audit Remediasi

#### [[`3d077024`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #378: remediasi temuan audit tanggal terbit tagihan penjualan - 7 Oktober 2026, 18:06:16 WIB

- **Dokumen Laporan Audit & Checklist Remediasi**:
  - [`_work-report/audit-report-issue-378.md`](file:///home/dhedhy/Project/Dekasimal-V2/_work-report/audit-report-issue-378.md) [NEW]: Dokumen laporan audit kode menyeluruh (417 baris) yang mengidentifikasi temuan integritas data, efisiensi query DB, memory leak, ketahanan timezone, data overfetching, dan OpenAPI.
  - [`_work-report/audit-task-issue-378.md`](file:///home/dhedhy/Project/Dekasimal-V2/_work-report/audit-task-issue-378.md) [NEW]: Checklist tindakan perbaikan 10 butir temuan prioritas tinggi, sedang, dan rendah yang telah diselesaikan 100%.

- **Komponen yang Berubah & Diremediasi**:
  - **Integritas Data & Sanitasi Form Controller (`backend/src/controllers/`)**:
    - [`backend/src/controllers/financeInvoice.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeInvoice.controller.js):
      - **[BE-01] Penyelamatan Nilai `tax: 0`**: Memperbaiki pelanggaran aturan AGENTS.md §2.A Rule 8 di mana pemanggilan `cleanFormData(req.body)` membuang nilai `0`. Menyimpan `rawTax` sebelum sanitasi dan memulihkan `tax = 0` jika nilai aslinya `0` atau `'0'`, sehingga admin dapat menghapus komponen pajak pada faktur tanpa tertimpa nilai pajak lama.
      - **[BE-04] Penerapan Data Minimization**: Mengubah payload kembalian respons HTTP 200 pada `financeInvoiceUpdate` agar tidak mengekspos 35+ field dokumen internal `INVOICE_NON_SENSITIVE`, melainkan DTO ringkas (`invoice_id`, `name`, `date`, `status`, `updated_at`).
  - **Layanan Bisnis & Efisiensi Database (`backend/src/services/` & `backend/src/utils/`)**:
    - [`backend/src/services/financeInvoice.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeInvoice.service.js):
      - **[BE-02] Short-Circuit Evaluasi Status `findIssueDatePolicy`**: Menambahkan pengecekan awal pada status faktur (`status !== 'unpaid'`, `canceling`, `to_wallet`, `paid_total > 0`, `paid_date`). Melewati query database ke `FinancePeriod.findOne`, `Payment.findOne`, dan `findFinanceSettings` jika faktur sudah pasti berstatus terkunci, menghemat 2 hingga 3 query DB per panggilan detail pada lebih dari 90% riwayat faktur.
    - [`backend/src/services/financePeriod.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financePeriod.service.js):
      - **[BE-03] Ketahanan Timezone `toAppMoment`**: Mengganti pemanggilan `moment(date).format('YYYY-MM')` mentah dengan `toAppMoment(date).format('YYYY-MM')` pada fungsi evaluasi periode tutup buku `isPeriodOpen` dan `assertPeriodOpen`. Mencegah pergeseran evaluasi bulan tutup buku di sekitar batas pergantian hari pada server dengan konfigurasi timezone runtime UTC.
    - [`backend/src/utils/invoiceIssueDatePolicy.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/invoiceIssueDatePolicy.js):
      - Engine evaluasi kebijakan kelayakan tanggal terbit faktur: memvalidasi tanggal masa depan, faktur ber-Nomor Faktur Pajak (dikunci ke bulan kalender yang sama), faktur berlangganan, faktur sebelum/sesudah tanggal mulai akrual, dan batas tanggal pembayaran pertama.
  - **Dokumentasi API & Pembersihan Dead Code (`backend/src/routes/` & `locales/`)**:
    - [`backend/src/routes/financeInvoice.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/financeInvoice.route.js):
      - **[BE-05] Sinkronisasi OpenAPI/Swagger**: Memperbaiki contoh `same_month_reason`, melengkapi skema respons 200 pada `PATCH /finance/invoice/{invoice_id}`, serta mendokumentasikan kode status HTTP 401 dan 403.
    - [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) & [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json):
      - **[BE-06] Pembersihan String Dead Code**: Menghapus entri yatim `"issueDateMonthLocked"`.
  - **Antarmuka & Komponen Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/finance/invoices/InvoiceDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/InvoiceDrawer.jsx):
      - **[FE-01] Pencegahan Memory Leak & State Mutation Unmounted**: Menerapkan boolean flag `isSubscribed` pada `useEffect` pengambilan detail invoice dan menempatkan `setLoading(false)` sebelum penutupan drawer `onClose()`.
      - **[FE-03] Stabilisasi Opsi Datepicker**: Menggunakan `baseDate` stabil (`watchedDate || originalIssueDate || new Date()`) untuk evaluasi pemilihan hari pada datepicker jatuh tempo, menghindari passing string kosong.
    - [`frontend/src/app/pages/finance/invoices/schema/invoiceSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/schema/invoiceSchema.js):
      - **[FE-02] Perbaikan Validasi Jatuh Tempo Client-Side**: Menambahkan fallback tanggal saat validasi tanggal jatuh tempo vs tanggal terbit pada mode pembuatan faktur baru, mencegah kelolosan payload tidak valid ke backend.
    - [`frontend/src/app/pages/finance/invoices/utils/issueDate.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/utils/issueDate.js) [NEW] & [`issueDate.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/utils/issueDate.test.js) [NEW]:
      - Logika murni deteksi perubahan tanggal terbit, pembentukan payload perubahan, dan evaluasi hari yang dapat dipilih pada datepicker.
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json):
      - **[FE-04] Pembersihan Kunci Terjemahan Yatim**: Menghapus kunci string `"auto_billing"`.
  - **Rangkaian Pengujian Otomatis**:
    - Backend: [`invoiceIssueDatePolicy.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/invoiceIssueDatePolicy.test.js) (28 test, 100% lulus) dan [`financeInvoice.issueDate.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeInvoice.issueDate.test.js).
    - Frontend: [`issueDate.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/utils/issueDate.test.js) (10 test, 100% lulus).
- **Deskripsi Perubahan & Fungsi**:
  - Menjamin keamanan dan konsistensi data finansial saat administrator melakukan koreksi tanggal terbit faktur tagihan penjualan. Faktur yang sudah memiliki pembayaran, sudah berstatus batal, atau berada pada periode akuntansi yang telah ditutup tidak dapat dimanipulasi tanggalnya.
  - Memastikan faktur yang telah diterbitkan Nomor Faktur Pajak terkunci pada bulan kalender yang sama untuk mematuhi regulasi perpajakan, sementara faktur tagihan langganan reguler tetap memiliki fleksibilitas penyesuaian tanggal.

---

## 🌿 Branch: `issue-377` — Riwayat Notifikasi Faktur Penjualan WhatsApp & Standardisasi Kamus Privilege

### 📌 Informasi Issue

- **Nomor Issue**: #377
- **Judul Issue**: Riwayat Notifikasi Faktur Penjualan WhatsApp (`finance_invoice_whatsapps`) & Standardisasi Kamus Privilege Sistem Terpusat
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`cb27795b`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`cb27795b`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #377 (Merge commit ke master) - 7 Oktober 2026, 15:00:31 WIB
#### [[`e4b1ffe5`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #377 - 7 Oktober 2026, 14:59:32 WIB

- **Komponen yang Berubah**:
  - **Model Data & Skema Backend (`backend/src/models/`)**:
    - [`backend/src/models/financeInvoiceWhatsapp.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financeInvoiceWhatsapp.model.js) [NEW]: Model Mongoose untuk menyimpan log riwayat pengiriman notifikasi faktur WhatsApp (`invoice_id`, nomor tujuan `phone`, isi pesan/template, status kirim `sent`/`delivered`/`read`/`failed`, waktu kirim, ID pesan WhatsApp, dan rincian kegagalan).
  - **Layanan Bisnis & Kontroler Backend (`backend/src/`)**:
    - [`backend/src/services/financeInvoiceWhatsapp.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeInvoiceWhatsapp.service.js) [NEW]: Layanan pencatatan event notifikasi invoice dan query daftar riwayat per faktur `findListInvoiceWhatsappHistory`.
    - [`backend/src/controllers/financeInvoice.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeInvoice.controller.js) & [`routes/financeInvoice.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/financeInvoice.route.js): Pengeksposan endpoint RESTful `GET /api/v1/finance/invoice/:invoice_id/whatsapp-history`.
    - [`backend/src/controllers/waInternal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/waInternal.controller.js) & [`services/waBroadcast.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waBroadcast.service.js): Pemicuan pencatatan otomatis riwayat saat invoice disiarkan melalui webhook Baileys / Meta API.
  - **Standardisasi Kamus Hak Akses (*Privilege Dictionary*)**:
    - [`backend/src/utils/sync-privilege-dictionary.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/sync-privilege-dictionary.js) [NEW] & [`privilege-endpoints.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/privilege-endpoints.js) [NEW]: Perkakas otomatis untuk memetakan seluruh route Express ke kamus privilege.
    - [`backend/src/config/privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json), [`frontend/src/constants/privilegeDescriptions.id.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/constants/privilegeDescriptions.id.json), [`frontend/src/constants/privilegeDescriptions.en.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/constants/privilegeDescriptions.en.json): Standardisasi dan penambahan ribuan deskripsi privilege bilingual untuk seluruh modul monorepo.
  - **Komponen Antarmuka Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/finance/invoices/InvoiceWhatsappHistory.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/InvoiceWhatsappHistory.jsx) [NEW]: Kartu tab riwayat notifikasi WhatsApp pada halaman detail faktur penjualan (`detail.jsx`), menampilkan timeline pengiriman pesan, status centang, nomor tujuan, dan tombol refresh.
    - [`frontend/src/app/pages/finance/invoices/detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/detail.jsx): Integrasi tab riwayat pengiriman WhatsApp.
    - [`frontend/src/app/navigation/customerService.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/customerService.js): Penyelarasan privilege menu customer service.
  - **Pengujian Otomatis Sistem**:
    - [`financeInvoiceWhatsappHistory.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeInvoiceWhatsappHistory.test.js) [NEW]: Pengujian integrasi pencatatan dan query riwayat pengiriman WhatsApp faktur.
    - [`privilegeDictionary.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/privilegeDictionary.test.js) [NEW] & [`privilegeDescriptions.coverage.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/constants/privilegeDescriptions.coverage.test.js) [NEW]: Pengujian kelengkapan cakupan kamus privilege.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan visibilitas penuh bagi staf keuangan terkait status pengiriman invoice ke WhatsApp pelanggan (apakah tagihan sudah terkirim, diterima, dibaca, atau gagal karena nomor tidak aktif), memudahkan proses follow-up penagihan.
  - Memastikan konsistensi penamaan dan deskripsi hak akses pengguna di seluruh sistem perizinan berbasis peran (RBAC) frontend dan backend.

---

## 🌿 Branch: `issue-375` — Pendaftaran Topik Forum Telegram Bersamaan & Resolusi Callback Dinamis

### 📌 Informasi Issue

- **Nomor Issue**: #375
- **Judul Issue**: Peningkatan Registrasi Topik Forum Telegram Bersamaan & Resolusi Callback Dinamis Microservice Bot
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`bf29749f`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`bf29749f`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #375 (Merge commit ke master) - 7 Oktober 2026, 13:20:42 WIB
#### [[`5c5a7ac9`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #375 - 7 Oktober 2026, 13:19:00 WIB

- **Komponen yang Berubah**:
  - **Layanan Backend & Utilitas Telegram (`backend/src/`)**:
    - [`backend/src/services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js): Peningkatan operasi atomik MongoDB saat mendaftarkan atau menghapus pemetaan topik forum Telegram, mencegah *race condition* antar instance server backend.
    - [`backend/src/utils/telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js):
      - Penanganan regresi produksi: dokumen `telegram_topic` yang belum ada kini diinisialisasi otomatis tanpa melempar *TypeError*.
      - Dukungan perintah pendaftaran topik `/regtopic` dan pencabutan `/unregtopic` dengan verifikasi izin grup, serta penyampaian pesan balasan ke chat jika operasi gagal (tidak senyap).
      - Fungsi `resolveTelegramDestinations` kini membaca konfigurasi langsung dari database untuk mendukung pendaftaran dinamis tanpa perlu me-reload proses backend.
  - **Microservice Bot Telegram (`telegram-api/`)**:
    - [`telegram-api/src/botManager.js`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-api/src/botManager.js): Membaca URL callback aktif terkini dari memori pemetaan bot saat meneruskan pesan perintah Telegram, memastikan perubahan URL webhook backend langsung aktif tanpa harus me-restart service `telegram-api`.
  - **Rangkaian Pengujian Integrasi**:
    - [`telegramTopicRegistration.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/telegramTopicRegistration.test.js) [NEW]: Pengujian integrasi komprehensif pendaftaran topik forum Telegram (pendaftaran pertama kali, pencegahan penimpaan topik tipe lain, konkurensi pendaftaran bersamaan, penghapusan `/unregtopic`, dan penolakan chat tidak sah).
- **Deskripsi Perubahan & Fungsi**:
  - Menghilangkan kegagalan pendaftaran topik grup Telegram yang sebelumnya gagal diam-diam tanpa pesan balasan saat dokumen konfigurasi belum terbentuk di database.
  - Memungkinkan perutean notifikasi sistem otomatis (seperti tiket, komplain gudang, presensi, alert NOC) ke sub-topik forum Telegram yang spesifik secara dinamis dan stabil.

---

## 🌿 Branch: `issue-373` — Penggabungan Modul Tiket & Asisten AI WhatsApp ke Master

### 📌 Informasi Issue

- **Nomor Issue**: #373
- **Judul Issue**: Tiket & Asisten AI dari Obrolan WhatsApp (Penyelesaian Form Tiket Penagihan, Seeder Sampel Simulasi, & Penggabungan Penuh ke Master)
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`d5bb66c0`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`d5bb66c0`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #373 (Merge commit ke master) - 7 Oktober 2026, 12:02:33 WIB
#### [[`d900932e`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #373 - 6 Oktober 2026, 13:40:07 WIB

- **Komponen yang Berubah**:
  - Penambahan skrip pembibitan data simulasi pengujian: [`backend/scripts/seedWaTicketSample.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/scripts/seedWaTicketSample.js) [NEW] dan [`seedWaTicketSample.data.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/scripts/seedWaTicketSample.data.js) [NEW].
  - Penyelesaian form tiket penagihan terintegrasi: [`BillingTicketForm.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/BillingTicketForm.jsx) [NEW], [`TicketExtrasFields.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/TicketExtrasFields.jsx) [NEW], [`TicketFormFooter.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/TicketFormFooter.jsx) [NEW], [`LinkNumberHint.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/LinkNumberHint.jsx) [NEW], dan [`AiDraftNotice.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/AiDraftNotice.jsx) [NEW].
  - Pengujian integrasi tambahan: [`waSendAttachment.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waSendAttachment.test.js) [NEW] dan [`billingTicketForm.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/utils/billingTicketForm.test.js) [NEW].
- **Deskripsi Perubahan & Fungsi**:
  - Menggabungkan seluruh fungsionalitas drawer tiket WhatsApp, draf formulir AI, rangkuman komentar obrolan, dan generator laporan PDF vektor ke branch `master`.

---

## 🌿 Lingkungan Produksi: Pembaruan Berkas Changelog Rilis Monorepo

### 📅 Rincian Commit

#### [[`ca9250b6`](file:///home/dhedhy/Project/Dekasimal-V2)] - update changelog - 7 Oktober 2026, 13:24:16 WIB (Branch `production`)

- **Komponen yang Berubah**:
  - [`backend/src/data/changelog/index.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/index.json): Penambahan indeks rilis terbaru.
  - [`backend/src/data/changelog/releases/issue-365.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-365.json) [NEW]: Catatan rilis Manajemen Jaringan Fiber Optik.
  - [`backend/src/data/changelog/releases/issue-368.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-368.json) [NEW]: Catatan rilis Penalaan RRD Counter Cap & Latency RRA MAX.
  - [`backend/src/data/changelog/releases/issue-370.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-370.json) [NEW]: Catatan rilis Buku Telepon WhatsApp & Sensor Kata Kasar Masuk.
  - [`backend/src/data/changelog/releases/issue-373.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-373.json) [NEW]: Catatan rilis Tiket & Asisten AI WhatsApp dengan PDF Vektor.
  - [`backend/src/data/changelog/releases/issue-375.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-375.json) [NEW]: Catatan rilis Registrasi Topik Forum Telegram.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #378  | Kebijakan & Validasi Pengubahan Tanggal Terbit Tagihan Penjualan | Audit kode dan remediasi penuh integritas pajak `tax: 0`, eliminasi memory leak, short-circuit query database, dan proteksi tanggal akuntansi. |
| #377  | Riwayat Notifikasi Tagihan WhatsApp & Standardisasi Privilege | Visibilitas status pengiriman WhatsApp pada detail faktur penjualan dan sinkronisasi kamus ribuan hak akses RBAC sistem. |
| #375  | Pendaftaran Topik Forum Telegram Bersamaan & Callback Dinamis | Stabilitas notifikasi grup forum Telegram multi-replika dan eliminasi error saat inisialisasi dokumen topik. |
| #373  | Penggabungan Tiket & Asisten AI WhatsApp ke Master | Rilis resmi drawer tiket di panel obrolan, draf form tiket AI, dan pembuatan dokumen laporan PDF vektor ke branch utama. |

### Kemampuan Baru Pengguna/Admin

- **Koreksi Aman Tanggal Terbit Faktur Penjualan**: Administrator keuangan dapat mengoreksi tanggal terbit faktur tagihan penjualan yang salah input dengan penjaminan kepatuhan perpajakan (Nomor Faktur Pajak terkunci di bulan kalender yang sama) dan proteksi periode tutup buku akuntansi.
- **Pemantauan Riwayat WhatsApp pada Faktur**: Admin dapat langsung melihat apakah notifikasi tagihan atau kwitansi faktur sudah terkirim, diterima, atau dibaca oleh nomor WhatsApp pelanggan dari tab *Riwayat WhatsApp* di halaman detail faktur.
- **Konfigurasi Topik Forum Telegram Otomatis**: Administrator dapat mendaftarkan topik forum grup Telegram secara langsung menggunakan perintah bot (`/regtopic <tujuan>`) tanpa risiko kegagalan sistem hening.

### Bug Fix / Solusi Masalah

- **Masalah Hilangnya Pajak 0 (`tax: 0`) pada Edit Faktur**: Memperbaiki sanitasi `cleanFormData` yang membuang angka `0`, sehingga admin kini dapat mengubah atau membebaskan pajak faktur menjadi 0% tanpa tereset ke nilai lama.
- **Pencegahan Memory Leak Komponen Faktur**: Mencegah pembaruan state pada unmounted component di `InvoiceDrawer.jsx` saat drawer ditutup sebelum request HTTP selesai.
- **Inefisiensi Query Detail Faktur**: Menghindari 2–3 query database yang tidak perlu pada faktur berstatus non-unpaid melalui mekanisme short-circuit di service faktur.
- **Pergeseran Evaluasi Periode Akuntansi Lintas Timezone**: Mencegah kesalahan pembacaan periode tutup buku bulanan pada server runtime UTC dengan standardisasi `toAppMoment`.
- **Kegagalan Registrasi Topik Telegram**: Memperbaiki crash *TypeError* pada pendaftaran awal topik forum Telegram ketika dokumen pengaturan belum terbentuk di MongoDB.

### Menu/Fitur Baru

- **Kartu Riwayat Pengiriman WhatsApp Faktur**: Komponen timeline dan tabel riwayat pengiriman notifikasi WhatsApp pada halaman detail faktur penjualan.
- **Opsi Penyesuaian Tanggal Terbit Faktur di Drawer**: Antarmuka pemilih tanggal terbit dengan datepicker adaptif yang membatasi pilihan hari hanya pada tanggal yang diizinkan secara bisnis dan akuntansi.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Aturan & Kebijakan Pengubahan Tanggal Terbit Tagihan Penjualan

- **Penjelasan Fitur**:
  Sistem menerapkan serangkaian aturan validasi cerdas saat admin mengoreksi tanggal terbit faktur tagihan penjualan (`invoice.date`):
  1. **Status Terkunci**: Faktur berstatus `paid` (lunas), `canceled` (batal), `refund`, atau faktur dalam proses pembatalan/dompet tidak dapat diubah tanggal terbitnya.
  2. **Batas Masa Depan**: Tanggal terbit tidak boleh melebihi hari ini (tidak boleh tanggal masa depan).
  3. **Batas Pembayaran Pertama**: Jika sudah ada pembayaran tercatat, tanggal terbit tidak boleh melewati tanggal pembayaran pertama tersebut.
  4. **Faktur Pajak**: Faktur yang telah memiliki Nomor Faktur Pajak wajib berada di dalam bulan kalender yang sama.
  5. **Periode Akuntansi**: Tanggal terbit tidak boleh berada pada periode tutup buku akuntansi yang telah dikunci.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Keuangan > Tagihan Penjualan**.
  2. Klik ikon edit (pensil) pada faktur yang berstatus *Belum Dibayar* (*Unpaid*).
  3. Pada panel drawer edit faktur, periksa bidang **Tanggal Tagihan**:
     - Jika faktur memenuhi syarat, kolom tanggal akan aktif dan datepicker hanya menampilkan rentang hari yang sah.
     - Jika faktur terkunci, sistem akan menampilkan alasan mengapa tanggal tidak dapat diubah (misal: "Periode akuntansi telah ditutup" atau "Faktur sudah memiliki pembayaran").
  4. Pilih tanggal baru yang diinginkan dan sesuaikan tanggal jatuh tempo jika perlu.
  5. Klik **Simpan**.

---

### 2. Memeriksa Riwayat Notifikasi Pengiriman WhatsApp pada Tagihan

- **Penjelasan Fitur**:
  Setiap notifikasi tagihan yang dikirimkan ke pelanggan (baik melalui siaran tagihan otomatis, pengingat jatuh tempo, atau kiriman manual) tercatat secara terperinci.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Keuangan > Tagihan Penjualan**.
  2. Klik nomor faktur untuk membuka halaman **Detail Faktur**.
  3. Gulir ke bagian bawah halaman pada kartu **Riwayat WhatsApp**.
  4. Periksa daftar log pengiriman: nomor ponsel tujuan, waktu pengiriman, isi pesan, serta status terkirim (*Terkirim*, *Diterima*, *Dibaca*, atau *Gagal*).
  5. Jika terdapat kegagalan pengiriman, arahkan kursor ke lencana status untuk membaca detail galat teknis.

---

### 3. Mendaftarkan Topik Forum Telegram untuk Notifikasi Sistem

- **Penjelasan Fitur**:
  Sistem mendukung pengiriman notifikasi operasional terarah ke sub-topik tertentu pada grup Telegram bertipe *Supergroup Forum*.
- **Langkah Penggunaan (Tutorial)**:
  1. Pastikan Bot Telegram sistem telah dimasukkan ke dalam Supergroup Telegram perusahaan dan memiliki hak administrator.
  2. Buat atau buka sub-topik forum yang diinginkan (misal topik "Komplain Pelanggan" atau "Notifikasi NOC").
  3. Ketikkan perintah berikut langsung di dalam sub-topik tersebut:
     ```text
     /regtopic <tujuan>
     ```
     *(Contoh: `/regtopic ticket` atau `/regtopic noc`)*.
  4. Bot akan segera membalas: `Topic '<tujuan>' now registered.`
  5. Seluruh notifikasi yang berhubungan dengan tujuan tersebut akan otomatis dialirkan ke sub-topik forum yang telah didaftarkan.
