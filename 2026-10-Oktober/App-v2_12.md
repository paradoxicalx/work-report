# 📝 Daily Work Report - Dedy S.N Putra (2026-10-12)

---

## 📅 Laporan Harian - 12 Oktober 2026

> Laporan ini merangkum seluruh aktivitas rekayasa perangkat lunak, perancangan modul analitik performa layanan pelanggan (Customer Service WhatsApp), penguatan kontrol akses dan keamanan transaksi keuangan sisi server, peningkatan fleksibilitas komponen tabel (*Datatables Client-Side Mode*), hingga rilis penggabungan produksi pada monorepo DEKASIMAL V2 per 12 Oktober 2026. Fokus dan pencapaian utama hari ini meliputi:
> 1. **Rekap Harian Obrolan WhatsApp & Analisis KPI Layanan (`issue-387`)**: Perancangan dan pembangunan modul komprehensif *Rekap Obrolan WhatsApp* (`/customer-service/whatsapp-report`) untuk mengukur performa objektif tim CS di seluruh channel (Resmi/Meta Cloud API dan Pribadi/Baileys). Sistem dilengkapi snapshot harian terarsip (`wa_daily_reports`), tab percakapan dengan TTL retensi otomatis (`wa_daily_conversations`), metrik First Response Time (FRT median/p90/rata-rata), tingkat ketercapaian SLA (%), rasio pesan masuk/keluar, pelacakan backlog/antrean harian, presenter terisolasi dengan proteksi data sensitif admin (`whatsappReport.readSensitive`), filter sensor kata kasar otomatis (*fail-closed*), serta fitur ekspor laporan multi-sheet XLSX dan PDF vektor.
> 2. **Orkestrasi Background Job Terjadwal BullMQ (`cron-worker`)**: Integrasi job harian `waDailyReport` yang berjalan otomatis setiap pukul 00:10 WIB (17:10 UTC) untuk membekukan (*freeze*) snapshot metrik hari kemarin ke database dan mengisi dokumen percakapan harian secara idempoten via endpoint internal Backend yang terproteksi API key.
> 3. **Penguatan Keamanan Akses Akun Pembayaran Manual Sisi Server (`issue-396`)**: Penegakan otorisasi ketat di lapisan backend controller (`assertPaymentAccountAccess`) pada seluruh transaksi pembayaran manual (pembayaran invoice tunggal, pembayaran invoice massal / bulk payment, dan pengisian saldo wallet pelanggan). Akun bank/kas kantor kini divalidasi wajib memiliki privilege `financeAccount.list|financeAccount.read`, sedangkan akun kas personal admin hanya dapat dipilih dan digunakan oleh pemiliknya masing-masing (`adminId === owner_admin`), menghasilkan proteksi HTTP 403 Forbidden bila terjadi manipulasi payload.
> 4. **Peningkatan Kemampuan Komponen Reusable Datatables (`frontend`)**: Penambahan mode data lokal/klien (`isClientData = !apiUrl && Array.isArray(data)`) pada komponen TanStack `Datatables` sehingga mendukung pengurutan, pencarian fuzzy, penyaringan rentang angka (`numberRange`), dan paginasi langsung di memori browser tanpa harus bergantung pada API backend. Dilengkapi utilitas sel baru `summaryCells.jsx` (`EmptyDashCell`, `BadgeValueCell`, `CountBadgeCell`) yang langsung diintegrasikan pada tabel ringkasan rekap WhatsApp.
> 5. **Penyempurnaan Filter Kolom Tabel Pembayaran Keuangan**: Penambahan filter dinamis pada tabel pembayaran kas dan transfer (`findListPaymentForTable`) untuk kolom `name` (nama invoice), `recipient` / `recipient_name` (nama pelanggan/mitra), dan `created_by` (nama staf pencatat pembayaran) dengan integrasi kueri lookup Mongoose yang efisien.
> 6. **Pengujian Otomatis & Sinkronisasi Rilis Produksi**: Seluruh 182 pengujian otomatis di backend (17 test files) dan 51 pengujian di frontend (9 test files) terverifikasi lulus 100%. Branch `issue-387` dan `issue-396` telah di-merge ke `master` dan diteruskan secara penuh ke branch `production`.

---

## 🌿 Branch: `issue-396` — Penegakan Hak Akses Akun Pembayaran Sisi Server, Filter Kolom Pembayaran Dinamis, & Komponen Datatables Mode Client-Side

### 📌 Informasi Issue

- **Nomor Issue**: #396
- **Judul Issue**: Penegakan Otorisasi Akun Tujuan Pembayaran Manual Sisi Server (`financeAccount`), Filter Kolom Pembayaran Dinamis (`name`, `recipient`, `created_by`), dan Peningkatan Fleksibilitas Komponen `Datatables` Mode Client-Side
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`e3665271`](file:///home/dhedhy/Project/Dekasimal-V2) dan dideploy ke branch `production` via commit [`77141ac0`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`e3665271`](file:///home/dhedhy/Project/Dekasimal-V2)] & [[`6c7ddad8`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #396 (Merge branch ke master) - 12 Oktober 2026, 22:00:33 WIB
#### [[`b52f14ce`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #396 - 12 Oktober 2026, 21:55:41 WIB
#### [[`441ef271`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #396 - 12 Oktober 2026, 18:48:26 WIB

- **Komponen yang Berubah**:
  - **Keamanan & Kontrol Akses Akun Keuangan Backend (`backend/src/`)**:
    - [`backend/src/utils/finance-account-access.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/finance-account-access.js) [NEW]:
      - Utilitas pembantu `assertPaymentAccountAccess(req, accountId)` yang memeriksa hak akses admin terhadap akun tujuan pembayaran.
      - Mengevaluasi apakah pemanggil memiliki privilege `financeAccount.list|financeAccount.read` untuk akun kantor, atau kepemilikan akun kas pribadi melalui `assertAccountUsableByAdmin`.
    - [`backend/src/services/financeAccount.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeAccount.service.js):
      - Fungsi `assertAccountUsableByAdmin({ accountId, adminId, canViewOfficeAccounts })`:
        - Memastikan akun ada dan aktif.
        - Menolak penggunaan akun bertipe kantor jika admin tidak memiliki wewenang `canViewOfficeAccounts`.
        - Menolak penggunaan akun kas personal milik admin lain jika `owner_admin` tidak cocok dengan ID admin pemanggil (`adminId`).
        - Menolak pemilihan akun gateway untuk transaksi manual di luar alur gateway resmi.
    - [`backend/src/controllers/financePayment.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financePayment.controller.js):
      - Memasang pengawal `assertPaymentAccountAccess` sebelum pemanggilan `financePaymentCreate` (pembayaran invoice tunggal) dan `financePaymentBulkCreate` (pembayaran invoice massal).
      - Menangkap galat otorisasi akun dan meneruskannya via `rethrowFinanceError` ke status HTTP 403 Forbidden.
    - [`backend/src/controllers/financeWallet.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeWallet.controller.js):
      - Menerapkan pengawal `assertPaymentAccountAccess` pada endpoint isi ulang dompet pelanggan (`financeWalletTopUp`).
    - [`backend/src/controllers/financeAccount.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeAccount.controller.js) & [`backend/src/routes/financeAccount.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/financeAccount.route.js):
      - Penyesuaian endpoint `financeAccountOptions` agar menyaring daftar akun kantor dan kas personal sesuai privilege admin yang sedang masuk.
    - [`backend/src/utils/finance-error.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/finance-error.js):
      - Penambahan pemetaan error code `ACCOUNT_FORBIDDEN` ke HTTP 403 Forbidden.
  - **Layanan Pembayaran & Filter Kolom Dinamis (`backend/src/services/`)**:
    - [`backend/src/services/financePayment.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financePayment.service.js):
      - Peningkatan fungsi datatable `findListPaymentForTable`, `findListCashPaymentForTable`, dan `findListTransferPaymentForTable`:
        - Filter kolom `name`: Mencari ID invoice berdasarkan regex nama tagihan (`Invoice.find({ name: regex })`), lalu memfilter pembayaran dengan `{ invoice: { $in: invoiceIds } }`.
        - Filter kolom `recipient` / `recipient_name`: Mencari relasi pelanggan dan mitra via nama, lalu menyaring transaksi terkait.
        - Filter kolom `created_by`: Mencari ID admin pencatat berdasarkan nama (`Admin.find({ name: regex })`), lalu memfilter field `created_by: { $in: adminIds }`.
    - [`backend/src/models/financePayment.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financePayment.model.js):
      - Indeks dan penyesuaian schema pendukung kueri filter pembayaran.
  - **Peningkatan Komponen Reusable Datatables (`frontend/src/components/shared/table/`)**:
    - [`frontend/src/components/shared/table/Table.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/Table.jsx):
      - Dukungan penuh mode klien (`isClientData = !apiUrl && Array.isArray(data)`):
        - Mengaktifkan pengurutan lokal TanStack Table saat `isClientData` bernilai true (`manualSorting: !isClientData`).
        - Mengaktifkan penyaringan lokal TanStack Table (`manualFiltering: !isClientData`) dengan fungsi penyaring kustom:
          - `numberRange`: Filter rentang angka `[min, max]` untuk sel numerik.
          - `select`: Filter pilihan berganda peka nilai string.
          - `fuzzy`: Pencarian teks fuzzy.
        - Mengaktifkan paginasi lokal TanStack Table (`manualPagination: !isClientData`, `getPaginationRowModel()`).
      - Dukungan properti UI fleksibel: `isCard`, `minHeight`, `hideToolbar`, `hidePagination`, `hideViewType`, `loading`, dan callback `onReload`.
      - Penambahan metode ref `getTable()` pada imperative handle untuk integrasi kontrol eksternal.
    - [`frontend/src/components/shared/table/summaryCells.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/summaryCells.jsx) [NEW]:
      - Koleksi komponen helper sel tabel ringkasan standar:
        - `EmptyDashCell`: Render penanda sel kosong `-` abu-abu seragam.
        - `BadgeValueCell`: Render badge lembut berwarna dengan format fungsi kustom dan awalan (prefix).
        - `CountBadgeCell`: Render badge berwarna untuk angka > 0 dan angka biasa abu-abu untuk 0.
    - [`frontend/src/components/shared/table/summaryCells.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/summaryCells.test.jsx) [NEW]:
      - Unit test komprehensif menguji skenario render null, undefined, format angka, dan badge warna.
    - [`frontend/src/components/shared/table/Table.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/Table.test.jsx) [NEW]:
      - Unit test TanStack Table menguji mode data klien, interaksi sorting, pencarian lokal, dan pagination.
  - **Penyempurnaan Tabel Ringkasan Rekap WhatsApp (`whatsappReport`)**:
    - [`frontend/src/app/pages/customerService/whatsappReport/components/ReportAdminTable.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/components/ReportAdminTable.jsx):
      - Migrasi ke komponen `Datatables` mode klien dengan sorting lokal, filter angka, dan badge helper.
    - [`frontend/src/app/pages/customerService/whatsappReport/components/ReportChannelTable.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/components/ReportChannelTable.jsx):
      - Migrasi ke `Datatables` mode klien untuk analisis kinerja per nomor/channel WhatsApp.
    - [`frontend/src/app/pages/customerService/whatsappReport/components/ReportDayTable.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/components/ReportDayTable.jsx):
      - Migrasi tabel harian ke `Datatables` klien dengan pengurutan tanggal, kalkulasi SLA, dan klik baris menuju drawer hari.
    - [`frontend/src/app/pages/customerService/whatsappReport/components/ReportDayDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/components/ReportDayDrawer.jsx):
      - Refactoring drawer detail harian menggunakan tabel-tabel ringkasan baru.
    - [`frontend/src/app/pages/customerService/whatsappReport/components/ReportTables.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/components/ReportTables.test.jsx) [NEW]:
      - Unit test memvalidasi render tabel admin, channel, dan drawer harian.
    - [`frontend/src/app/pages/customerService/whatsappReport/components/ReportConversationTable.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/components/ReportConversationTable.jsx):
      - Penyesuaian filter kolom percakapan, indikator status waktu respon, pemotongan pratinjau pesan teks, dan navigasi obrolan langsung.
  - **Skema Kolom Tabel Pembayaran Invoice (`frontend/`)**:
    - [`frontend/src/app/pages/finance/invoices/payments/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/payments/schema/columns.jsx):
      - Penambahan konfigurasi filter kolom yang tepat untuk `name`, `recipient_name`, dan `created_by`.
    - [`frontend/src/app/pages/finance/invoices/payments/schema/columns.test.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/payments/schema/columns.test.jsx) [NEW]:
      - Pengujian render kolom tabel pembayaran dan ketersediaan opsi filter.
  - **Higienitas Repositori & Pembersihan Dokumen Usang**:
    - Pembersihan dokumen spesifikasi teknis usang di direktori `docs/superpowers/specs/` agar repositori tetap bersih dan terfokus pada dokumen arsitektur aktif di `AGENTS.md`.
  - **Rangkaian Pengujian Otomatis**:
    - [`financeAccount.optionsPrivilege.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeAccount.optionsPrivilege.test.js) (10 tests, 100% lulus): Validasi pemisahan hak akses opsi akun kantor vs kas personal.
    - [`financePayment.accountAccess.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financePayment.accountAccess.test.js) (7 tests, 100% lulus): Validasi penolakan pembayaran jika admin mengakses kas milik orang lain atau akun kantor tanpa izin.
    - [`financePayment.tableFilter.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financePayment.tableFilter.test.js) (9 tests, 100% lulus): Validasi filter kolom tabel pembayaran `name`, `recipient`, dan `created_by`.

---

## 🌿 Branch: `issue-387` — Rekap Harian Obrolan WhatsApp (Customer Service) — KPI Tim, Analisis Channel & Admin, Snapshot Harian, serta Ekspor Excel & PDF

### 📌 Informasi Issue

- **Nomor Issue**: #387
- **Judul Issue**: Rekap Harian Obrolan WhatsApp (`wa_daily_reports` & `wa_daily_conversations`), Analisis Performa Layanan Pelanggan (FRT, SLA, Volume, Channel, Admin), Otomasi Cron BullMQ, serta Ekspor XLSX & PDF Vektor
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` via commit [`4b191b1b`](file:///home/dhedhy/Project/Dekasimal-V2) dan dideploy ke branch `production` via commit [`77141ac0`](file:///home/dhedhy/Project/Dekasimal-V2))

### 📅 Rincian Commit

#### [[`4b191b1b`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #387 (Merge commit ke master) - 12 Oktober 2026, 17:52:37 WIB
#### [[`be901752`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #387 - 12 Oktober 2026, 11:59:00 WIB

- **Komponen yang Berubah**:
  - **Panduan Arsitektur Monorepo (`AGENTS.md`)**:
    - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md):
      - Penambahan aturan baku Bab 2.A nomor 25: Standar Rekap Harian Obrolan WhatsApp (`wa_daily_reports` & `wa_daily_conversations`).
      - Menetapkan kepatuhan arsitektur:
        - SATU fungsi `computeDailyReport(date)` sebagai *single source of truth* untuk cron BullMQ, rekalkulasi manual, dan tampilan langsung (*live*) hari ini.
        - Penghitungan batas hari berbasis zona waktu Asia/Jakarta (00:00–24:00 WIB) yang dieksekusi deterministik di atas container backend UTC via `utils/waReportDate.js`.
        - Metrik First Response Time (FRT) dan `sessions_new` dihitung murni dari urutan sekuensial pesan di database (bukan timestamp denormalisasi yang rentan bergeser).
        - Balasan admin dari ponsel fisik (Baileys sync) dihitung sebagai kontribusi tim (`via_device`), tidak dibebankan keliru ke admin tertentu.
        - Data individual per-admin (`by_admin`, `first_reply_admin`, `handled_by`) difilter secara ketat di lapisan presenter (`waReportPresenter.js`) dan HANYA disajikan bila pemanggil memiliki hak `whatsappReport.readSensitive`.
        - Tab Percakapan Harian disimpan ke koleksi terpisah `wa_daily_conversations` dengan masa retensi TTL otomatis (`sessionRetentionDays`, bawaan 365 hari).
        - Sensor kata kasar otomatis terpasang pada pratinjau pesan (`censorForAdmin`) secara *fail-closed*.
  - **Model Data Mongoose Backend (`backend/src/models/`)**:
    - [`backend/src/models/waDailyReport.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/waDailyReport.model.js) [NEW]:
      - Model data laporan harian `wa_daily_reports` dengan indeks unik pada `{ date: 1 }` (format `YYYY-MM-DD` WIB).
      - Menyimpan metrik teragregasi: `period_start`, `period_end`, `sla_target_minutes`, `delayed_threshold_minutes`, `sessions_total`, `sessions_new`, `sessions_resolved`, `sessions_waiting`, `inbound_messages`, `outbound_messages`, `frt` (median, p90, avg, sum, count, histogram buckets), `sla_attainment`, `by_channel`, `by_admin`, `backlog_end`, `waiting_end`, dan versi formula `calc_version`.
    - [`backend/src/models/waDailyConversation.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/waDailyConversation.model.js) [NEW]:
      - Model percakapan harian `wa_daily_conversations` dengan indeks unik gabungan `{ date: 1, conversation: 1 }` dan indeks TTL pada `expires_at`.
      - Menyimpan snapshot rincian sesi: `customer`, `partner`, `channel_name`, `channel_type`, `first_reply_admin`, `handled_by`, `frt_seconds`, `inbound_count`, `outbound_count`, `resolution_status`, `waiting_end`, dan pratinjau pesan terakhir (maksimal 160 karakter).
    - [`backend/src/models/waConversation.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/waConversation.model.js) & [`backend/src/models/ticket.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/ticket.model.js):
      - Indeks penunjang pencarian riwayat obrolan dan relasi tiket.
  - **Layanan Bisnis, Metrik, & Presenter Backend (`backend/src/`)**:
    - [`backend/src/services/waReport.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waReport.service.js) [NEW]:
      - `computeDailyReport(date, options)`: Algoritma penarikan pesan mentah dan komputasi metrik performa tim CS.
      - `computeAndSave(date)`: Pengeksekusian kalkulasi dan penyimpanan snapshot harian secara atomik dan idempoten.
      - `getDailyReportsRange(startDate, endDate, req)`: Pengambilan daftar rekap harian dalam rentang waktu yang difilter presenter hak akses.
      - `getDailyReportDetail(date, req)`: Mengambil laporan rincian lengkap untuk satu tanggal tertentu.
      - `listDailyConversations(query, req)`: Menyajikan daftar percakapan harian dengan filter paginasi, pencarian teks/nomor HP kanonik, filter channel, dan resolusi status.
      - `rebuildDailyReports(startDate, endDate)`: Fitur pemulihan snapshot masa lampau dengan proteksi rentang maksimal 31 hari.
    - [`backend/src/utils/waReportDate.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waReportDate.js) [NEW]:
      - Utilitas konversi dan kalkulasi tanggal presisi WIB (UTC+7).
    - [`backend/src/utils/waReportMetrics.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waReportMetrics.js) [NEW]:
      - Utilitas pembentukan histogram rentang respon, perhitungan persentil 50 (median) dan persentil 90 (p90), serta persentase ketercapaian SLA target.
    - [`backend/src/utils/waReportPresenter.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waReportPresenter.js) [NEW]:
      - Lapisan gerbang keamanan respon API yang menyaring dan membersihkan data sensitif staf jika pemanggil tidak memiliki privilege `whatsappReport.readSensitive`, serta menyensor teks kata kasar via `censorForAdmin`.
    - [`backend/src/utils/waReportConversations.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waReportConversations.js) [NEW]:
      - Utilitas pembentukan data baris percakapan dan penyaringan channel Baileys per akun yang diizinkan (`allowed_admins`).
    - [`backend/src/services/cronSettings.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/cronSettings.service.js) & [`backend/src/services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js):
      - Pendaftaran konfigurasi cron job dan parameter SLA sistem.
  - **Kontroler & Perutean Backend (`backend/src/controllers/`, `backend/src/routes/`, `backend/src/app.js`)**:
    - [`backend/src/controllers/waReport.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/waReport.controller.js) [NEW]:
      - Controller Express menangani endpoint `summary`, `day-detail`, `conversations`, `export`, dan `rebuild`.
    - [`backend/src/routes/waReport.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/waReport.route.js) [NEW]:
      - Definisi rute RESTful dengan middleware hak akses `whatsappReport.read`, `whatsappReport.export`, dan `whatsappReport.rebuild`.
    - [`backend/src/controllers/internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internal.controller.js) & [`backend/src/routes/internal.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/internal.route.js):
      - Endpoint internal `POST /internal/cron/wa-daily-report` untuk eksekusi oleh cron worker.
    - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) & [`backend/src/config/privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json):
      - Pendaftaran privilege baru: `whatsappReport.read`, `whatsappReport.readSensitive`, `whatsappReport.rebuild`, `whatsappReport.export`.
    - [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) & [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json):
      - Penambahan kunci terjemahan bilingual pesan error dan validasi laporan WhatsApp.
  - **Layanan Cron Worker Terjadwal (`cron-worker/`)**:
    - [`cron-worker/src/jobs/processors/waDailyReport.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/processors/waDailyReport.js) [NEW]:
      - Job processor BullMQ yang memanggil endpoint internal backend untuk mengeksekusi pembuatan rekapitulasi harian pukul 00:10 WIB setiap hari.
    - [`cron-worker/src/jobs/scheduler.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/scheduler.js):
      - Penjadwalan berulang cron BullMQ (`10 17 * * *` UTC = 00:10 WIB).
    - [`cron-worker/src/jobs/worker.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/worker.js) & [`cron-worker/src/services/api.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/services/api.service.js):
      - Registrasi processor job dan client HTTP API internal dengan token autentikasi rahasia.
  - **Antarmuka Pengguna & Navigasi Frontend (`frontend/src/`)**:
    - [`frontend/src/app/navigation/customerService.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/customerService.js):
      - Pendaftaran menu navigasi baru *Rekap Obrolan* (`whatsappReport`) di bawah grup Layanan Pelanggan.
    - [`frontend/src/app/router/customerService/whatsappReportRoute.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/customerService/whatsappReportRoute.jsx) [NEW] & [`frontend/src/app/router/protected.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/protected.jsx):
      - Rute terproteksi `/customer-service/whatsapp-report`.
    - [`frontend/src/app/pages/customerService/whatsappReport/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/index.jsx) [NEW]:
      - Halaman utama komprehensif memuat:
        - Bar navigasi filter tanggal (Hari ini, 7 Hari Terakhir, 30 Hari Terakhir, Kustom).
        - Strip metrik kartu KPI interaktif (`ReportKpiStrip.jsx`).
        - Grafik ApexCharts tren percakapan harian dan pencapaian SLA (`ReportCharts.jsx`).
        - Tabulasi tampilan: Ringkasan Harian (`ReportDayTable`), Kinerja Admin (`ReportAdminTable`), Analisis Channel (`ReportChannelTable`), dan Daftar Percakapan (`ReportConversationTable`).
    - [`frontend/src/app/pages/customerService/whatsappReport/hooks/useWhatsappReport.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/hooks/useWhatsappReport.js) [NEW] & [`useWhatsappConversations.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/hooks/useWhatsappConversations.js) [NEW]:
      - React hooks untuk fetching dan sinkronisasi data rekapitulasi serta percakapan harian.
    - [`frontend/src/app/pages/customerService/whatsappReport/utils/reportExport.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/utils/reportExport.js) [NEW]:
      - Generator ekspor spreadsheet XLSX multi-sheet (Ringkasan Harian, Kinerja Admin, Analisis Channel, dan Log Percakapan).
    - [`frontend/src/app/pages/customerService/whatsappReport/utils/reportPdf.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/utils/reportPdf.js) [NEW]:
      - Generator dokumen PDF vektor profesional berbasis `jspdf` yang dapat dicetak atau diunduh.
    - [`frontend/src/app/pages/customerService/whatsappReport/utils/reportMath.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/utils/reportMath.js) [NEW] & [`conversationFilters.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappReport/utils/conversationFilters.js) [NEW]:
      - Utilitas kalkulasi persentil, agregasi metrik, dan logika filter percakapan.
    - [`frontend/src/app/pages/settings/sections/System.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/System.jsx) & [`systemSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/schema/systemSchema.js):
      - Form pengaturan SLA WhatsApp: Target Respons (menit), Ambang Delay (menit), dan Masa Retensi Sesi Percakapan (hari).
    - Integrasi Chat WhatsApp (`frontend/src/app/pages/customerService/whatsappChat/`):
      - Peningkatan daftar obrolan dengan indikator status balasan (`replyStatus.js`), pengurutan percakapan (`conversationSort.js`), dan notifikasi tiket (`ticketNotify.js`).
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json):
      - Penambahan 188+ pasangan teks terjemahan antarmuka, judul kolom tabel, kartu KPI, dan dialog ekspor/rebuild.
  - **Rangkaian Pengujian Otomatis**:
    - Backend Tests (8 test suite): `waReport.controller.test.js`, `waReport.service.test.js`, `waReportConversations.controller.test.js`, `waReportConversations.service.test.js`, `waReportConversations.test.js`, `waReportDate.test.js`, `waReportMetrics.test.js`, `waReportPresenter.test.js`.
    - Frontend Tests (8 test suite): `conversationSort.test.js`, `replyStatus.test.js`, `ticketNotify.test.js`, `conversationFilters.test.js`, `reportExport.test.js`, `reportMath.test.js`, `reportPdf.test.js`, `ReportConversationTable.test.jsx`.

---

## 🌿 Branch: `production` — Sinkronisasi Rilis Produksi Monorepo DEKASIMAL V2

### 📌 Informasi Rilis

- **Branch**: `production`
- **Tujuan**: Rilis penggabungan cabang produksi dari seluruh kode teruji di cabang `master`
- **Status Branch**: `Sudah di-merge` (Sinkron dengan `origin/production` dan `origin/master`)

### 📅 Rincian Commit

#### [[`77141ac0`](file:///home/dhedhy/Project/Dekasimal-V2)] - Merge branch 'master' into production - 12 Oktober 2026, 22:03:24 WIB

- **Deskripsi Perubahan**:
  - Menggabungkan seluruh pembaruan dari branch `master` ke branch `production`.
  - Memastikan seluruh fitur baru dari `issue-387` (Modul Rekap Harian WhatsApp, otomatisasi BullMQ, ekspor laporan) dan `issue-396` (penegakan kontrol akses akun keuangan sisi server, filter dinamis pembayaran invoice, dan pembaruan komponen TanStack Datatables mode klien) telah terdistribusi ke branch produksi.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #396  | Penegakan Otorisasi Akun Pembayaran Sisi Server & Datatables Client-Side | Mencegah celah manipulasi akun pembayaran manual di level server (HTTP 403 jika akun bukan milik staf/bukan akun kantor yang sah), mempermudah pelacakan pembayaran via filter dinamis kolom `name`/`recipient`/`created_by`, dan menyediakan komponen `Datatables` mode data klien yang fleksibel untuk data tabel lokal. |
| #387  | Rekap Harian Obrolan WhatsApp & Analisis KPI Customer Service | Supervisor dan manajemen kini memiliki visibilitas penuh terhadap performa objektif layanan pelanggan (FRT, SLA, volume pesan, channel, per-admin) dengan snapshot harian otomatis dari BullMQ serta dukungan ekspor laporan resmi XLSX dan PDF vektor. |

### Kemampuan Baru Pengguna/Admin

- **Supervisor & Manajemen Layanan Pelanggan**:
  - Dapat meninjau dashboard KPI analitik WhatsApp secara komprehensif (`/customer-service/whatsapp-report`), memilih rentang tanggal fleksibel (Hari ini, 7 hari, 30 hari, atau tanggal kustom), dan memantau rata-rata, median, serta persentil 90 First Response Time (FRT).
  - Dapat melihat perincian performa per channel (nomor resmi Meta Cloud API vs nomor pribadi Baileys) dan performa kontribusi per admin (jika memiliki hak akses `whatsappReport.readSensitive`).
  - Dapat membuka tab Percakapan Harian untuk membaca transkrip singkat sesi yang telah disensor dari kata kasar secara otomatis, melihat admin yang merespons pertama, serta membuka obrolan langsung di panel WhatsApp Chat.
  - Dapat mengunduh dokumen laporan resmi dalam format spreadsheet Excel (.xlsx) dengan 4 lembar kerja terstruktur, atau format dokumen cetak PDF vektor.
- **Staf Kasir & Keuangan**:
  - Mendapatkan proteksi otomatis di formulir pembayaran manual: staf kasir hanya dapat memilih kas personal miliknya sendiri dan akun kantor yang memang diizinkan untuk rolenya.
  - Dapat memfilter riwayat pembayaran di tabel keuangan berdasarkan nama tagihan tertentu, nama pelanggan/mitra pembayar, atau staf pencatat pembayaran secara cepat dan akurat.

### Bug Fix / Solusi Masalah

- **Pencegahan Celah Keamanan Otorisasi Akun Pembayaran**: Memperbaiki kerentanan di mana sebelumnya validasi pemilihan akun hanya berada di form antarmuka frontend (UI) sehingga payload API yang dimodifikasi berpotensi mencatatkan pembayaran ke akun kantor atau kas staf lain. Penegakan server-side `assertPaymentAccountAccess` kini menolak tegas dengan HTTP 403 Forbidden.
- **Penyaringan Kolom Relasi Pembayaran**: Menyelesaikan kendala pencarian pada tabel riwayat pembayaran di mana pencarian kolom nama invoice, nama penerima, atau admin pembuat pembayaran sebelumnya tidak dapat difilter langsung karena perbedaan relasi ObjectId di database.
- **Ketidakkonsistenan Tampilan Sel & Sorting Data Lokal**: Menyelesaikan keterbatasan komponen `Datatables` lama yang hanya dirancang untuk endpoint backend paginasi server, kini mendukung mode data klien mandiri dengan sorting, filtering rentang angka, dan pagination instan tanpa re-fetching.

### Menu/Fitur Baru

- **Menu Rekap Obrolan WhatsApp**: Menu baru di *Layanan Pelanggan > Rekap Obrolan* (`/customer-service/whatsapp-report`) lengkap dengan kartu KPI, grafik visual tren, tabel rincian harian, tabel admin, tabel channel, dan log percakapan harian.
- **Pengaturan SLA WhatsApp**: Pengaturan baru di menu *Pengaturan > Sistem* untuk mengonfigurasi Target Respon (menit), Ambang Delay (menit), dan Masa Retensi Sesi Percakapan (hari).
- **Background Worker BullMQ `waDailyReport`**: Job cron otomatis di `cron-worker` yang membekukan rekapan harian setiap pukul 00:10 WIB.
- **Sel Ringkasan Reusable (`summaryCells.jsx`)**: Utilitas komponen sel `EmptyDashCell`, `BadgeValueCell`, dan `CountBadgeCell` untuk standardisasi sel tabel monorepo.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Modul Rekap Harian Obrolan WhatsApp (`/customer-service/whatsapp-report`)

- **Penjelasan Fitur**:
  Modul ini menghadirkan visibilitas objektif terhadap kecepatan dan kualitas pelayanan tim CS dalam merespons pesan WhatsApp pelanggan. Metrik dihitung secara deterministik dari urutan pesan di database per hari WIB (00:00–24:00). Supervisor dapat mengevaluasi seberapa cepat pesan pertama dibalas (FRT), berapa persen pesan yang memenuhi target SLA, berapa banyak pesan masuk/keluar, dan berapa beban antrean yang tertunda (*backlog*).

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi web Dekasimal V2 dengan akun yang memiliki hak akses `whatsappReport.read`.
  2. Pada menu navigasi sebelah kiri, buka grup **Layanan Pelanggan** lalu pilih sub-menu **Rekap Obrolan**.
  3. Gunakan filter bar di bagian atas:
     - Pilih tombol rentang tanggal cepat: **Hari Ini** (data live terhitung langsung), **7 Hari Terakhir**, **30 Hari Terakhir**, atau tentukan tanggal manual via pemilih tanggal.
     - Pilih filter channel jika ingin meninjau nomor WhatsApp tertentu saja.
  4. Amati kartu **KPI Strip** di bagian atas untuk melihat rangkuman Total Percakapan, Sesi Baru, FRT Rata-rata & Median, serta Pencapaian SLA.
  5. Jelajahi tab navigasi di bawah grafik:
     - **Tabel Harian**: Klik salah satu baris tanggal untuk membuka panel *Drawer* detail hari tersebut.
     - **Tabel Admin**: Lihat perbandingan waktu respon dan jumlah sesi yang ditangani masing-masing staf CS (hanya tampil jika memiliki izin `whatsappReport.readSensitive`).
     - **Tabel Channel**: Periksa volume dan respon di masing-masing nomor WhatsApp.
     - **Tab Percakapan**: Cari nomor telepon pelanggan atau nama untuk melihat riwayat sesi hari tersebut, waktu respon pertamanya, dan pratinjau pesan terakhir.
  6. Untuk mengunduh laporan, klik tombol **Ekspor** di sudut kanan atas dan pilih format **Excel (.xlsx)** atau **Cetak Dokumen PDF**.

### 2. Proteksi Akun Pembayaran Manual Sisi Server

- **Penjelasan Fitur**:
  Sistem menjamin integritas pembukuan kas dan rekening bank perusahaan. Setiap staf yang melakukan pencatatan pembayaran manual pada invoice pelanggan (baik melalui *Payment Drawer* tunggal, *Bulk Payment Drawer* massal, maupun *Wallet Top Up*) kini diverifikasi hak aksesnya di server. Staf tanpa wewenang akuntansi kantor hanya dapat menyetor ke kas personal miliknya sendiri.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Keuangan > Tagihan** atau **Keuangan > Dompet**.
  2. Buka laci pembayaran dengan menekan tombol **Bayar** pada salah satu tagihan pelanggan.
  3. Pada pilihan **Akun Tujuan**, sistem secara otomatis hanya menampilkan daftar akun yang sah untuk role admin yang sedang masuk (kas personal admin bersangkutan, atau rekening kantor bila admin memiliki izin `financeAccount.read`).
  4. Bila terdapat usaha memanipulasi request dengan mengirimkan ID akun kas staf lain atau akun kantor yang tidak berhak, server akan menolak mutasi secara aman dengan respons `403 Forbidden` dan mencatat peringatan keamanan pada log audit sistem.
