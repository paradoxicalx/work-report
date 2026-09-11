# 📝 Daily Work Report - Dedy S.N Putra (2026-09-11)

---

## 📅 Laporan Harian - 11 September 2026

---

## 🌿 Branch: `issue-289` — Redesign & Implementasi Dashboard Domain Terpadu: Executive, Connectivity, Problem/Tiket, Finance, Sales, dan Hotspot (Domain-Specific Operational Dashboards V2)

### 📌 Informasi Issue

- **Nomor Issue**: #289
- **Judul Issue**: Redesign & Implementasi Dashboard Domain Terpadu: Executive Overview, Connectivity & Network Health, Problem & NOC Tiket, Finance & Cash Flow, Sales & Funnel Pipeline, serta Hotspot Voucher Analytics (Domain-Specific Operational Dashboards V2)
- **Status Branch**: `Belum di-merge` (Branch aktif dalam pengembangan di lokal & remote `origin/issue-289`)

---

### 📅 Rincian Perubahan (Work in Progress & Uncommitted Files)

#### `[WIP - Active Working Tree]` - implement domain-specific operational dashboards v2 - 11 September 2026

- **Komponen yang Berubah**:
  - **Backend Core — Endpoint Statistik, Agregasi Pipeline & Kontrol Hak Akses**:
    - [`backend/src/controllers/hotspotVoucher.controller.js`](backend/src/controllers/hotspotVoucher.controller.js) & [`backend/src/routes/hotspotVoucher.route.js`](backend/src/routes/hotspotVoucher.route.js) — Penambahan controller dan endpoint `GET /api/v1/hotspot-voucher/dashboard/stats` dengan proteksi middleware privilege `hotspotVoucher.list`.
    - [`backend/src/services/hotspotVoucher.service.js`](backend/src/services/hotspotVoucher.service.js) — Implementasi pipeline agregasi MongoDB terpadu:
      - Penjualan voucher harian hari ini vs kemarin (komputasi delta performa penjualan).
      - Rincian status inventori voucher (*available*, *used/active*, *expired*, *isolir*).
      - Penghitungan sesi pelanggan aktif secara *real-time* (*online users*).
      - Daftar 5 lokasi hotspot teratas berdasarkan volume voucher.
    - [`backend/src/controllers/networkDevice.controller.js`](backend/src/controllers/networkDevice.controller.js) & [`backend/src/routes/networkDevice.route.js`](backend/src/routes/networkDevice.route.js) — Penambahan parameter query `device_type` (mis. `OLT`) dan `status` pada endpoint `GET /api/v1/network/device/stats` untuk kebutuhan filter spesifik kartu PON/OLT.
    - [`backend/src/controllers/networkIPv4.controller.js`](backend/src/controllers/networkIPv4.controller.js), [`backend/src/services/networkIPv4.service.js`](backend/src/services/networkIPv4.service.js), & [`backend/src/routes/networkIPv4.route.js`](backend/src/routes/networkIPv4.route.js) — Endpoint `GET /api/v1/network/ipv4/stats`:
      - Menghitung utilisasi pool IPv4 (kapasitas alamat IP vs IP yang sudah terpakai/terdistribusi) pada seluruh blok subnet *leaf* bertipe *usage general*.
    - [`backend/src/controllers/prospect.controller.js`](backend/src/controllers/prospect.controller.js), [`backend/src/services/prospect.service.js`](backend/src/services/prospect.service.js), & [`backend/src/routes/prospect.route.js`](backend/src/routes/prospect.route.js) — Endpoint `GET /api/v1/prospect/funnel-stages`:
      - Agregasi 5 tahap konversi penjualan (*Sales Funnel*): Leads, Survey Lokasi, Penawaran (*Quotation*), PKS/SO (*Sales Order*), dan Instalasi Selesai lintas modul `Prospect`, `CustomerPO`, dan `WorkOrder`.
    - [`backend/src/controllers/radiusAuthentication.controller.js`](backend/src/controllers/radiusAuthentication.controller.js), [`backend/src/services/radiusAuthentication.service.js`](backend/src/services/radiusAuthentication.service.js), & [`backend/src/routes/radiusAuthentication.route.js`](backend/src/routes/radiusAuthentication.route.js) — Dua endpoint analitik pelanggan broadband:
      - `GET /api/v1/broadband/dashboard/distribution`: Menghitung distribusi pelanggan aktif per POP (*Point of Presence*) dan per paket broadband/profil kecepatan.
      - `GET /api/v1/broadband/dashboard/growth`: Menghitung metrik pertumbuhan bersih pelanggan bulan berjalan (aktivasi baru vs pelanggan deaktif/proxy churn).
    - [`backend/src/controllers/ticket.controller.js`](backend/src/controllers/ticket.controller.js), [`backend/src/services/ticket.service.js`](backend/src/services/ticket.service.js), & [`backend/src/routes/ticket.route.js`](backend/src/routes/ticket.route.js) — Endpoint `GET /api/v1/ticket/dashboard/problem-stats`:
      - Agregasi komprehensif pipeline tiket gangguan: status (*unassigned*, *assigned/in-progress*, *completed*, *canceled*), deteksi tiket *overdue* (> 7 hari tanpa penutupan), top kategori permasalahan pelanggan, rata-rata durasi penyelesaian (*Mean Time to Resolve / MTTR* dalam jam dan hari), serta beban penugasan teknisi (*technician workload*).
    - [`backend/src/services/workOrder.service.js`](backend/src/services/workOrder.service.js) — Fungsi agregasi status Work Order terbagi per tipe (instalasi baru vs perbaikan/gangguan).
  - **Frontend — Navigasi RBAC, Dashboard Eksekutif & 5 Dashboard Spesifik Domain**:
    - [`frontend/src/app/navigation/dashboards.js`](frontend/src/app/navigation/dashboards.js) — Penyematan izin RBAC spesifik pada menu dashboard: Sales (`prospect.list`), Finance (`financeReport.readSensitive`), Problem (`ticketCustomer.list`), Connectivity (`networkDevice.read`), dan Hotspot (`hotspotVoucher.list`).
    - **Dashboard Eksekutif (Home Overview)**:
      - [`frontend/src/app/pages/dashboards/home/index.jsx`](frontend/src/app/pages/dashboards/home/index.jsx) — Perombakan dari sekadar *redirect* utilitas menjadi dasbor eksekutif modern yang menyatukan metrik lintas divisi (Pelanggan, Finansial, Kesehatan Jaringan, Gangguan Aktif, dan Akses Cepat).
      - [`frontend/src/app/pages/dashboards/home/hooks/useExecutiveStats.js`](frontend/src/app/pages/dashboards/home/hooks/useExecutiveStats.js) [NEW] — Hook orkestrator pengambilan data paralel lintas endpoint analitik.
      - [`frontend/src/app/pages/dashboards/home/components/CustomerSummaryCard.jsx`](frontend/src/app/pages/dashboards/home/components/CustomerSummaryCard.jsx) [NEW] — Ringkasan status pelanggan aktif, isolir, nonaktif, dan pertumbuhan bulan berjalan.
      - [`frontend/src/app/pages/dashboards/home/components/FinancialSummaryCard.jsx`](frontend/src/app/pages/dashboards/home/components/FinancialSummaryCard.jsx) [NEW] — Ringkasan arus kas bersih, saldo piutang tertunggak, dan nilai portofolio.
      - [`frontend/src/app/pages/dashboards/home/components/NetworkHealthCard.jsx`](frontend/src/app/pages/dashboards/home/components/NetworkHealthCard.jsx) [NEW] — Indikator perangkat online, offline, peringatan, dan rata-rata latensi sistem.
      - [`frontend/src/app/pages/dashboards/home/components/OpenTicketsCard.jsx`](frontend/src/app/pages/dashboards/home/components/OpenTicketsCard.jsx) [NEW] — Ringkasan tiket gangguan terbuka dan *alert* tiket *overdue*.
      - [`frontend/src/app/pages/dashboards/home/components/QuickActionsCard.jsx`](frontend/src/app/pages/dashboards/home/components/QuickActionsCard.jsx) [NEW] — Tombol navigasi cepat ke modul registrasi pelanggan, tiket gangguan, pembuatan faktur, dan manajemen perangkat.
    - **Dashboard Connectivity & Jaringan**:
      - [`frontend/src/app/pages/dashboards/connectivity/index.jsx`](frontend/src/app/pages/dashboards/connectivity/index.jsx) — Layout dasbor operasional NOC jaringan.
      - [`frontend/src/app/pages/dashboards/connectivity/hooks/useConnectivityStats.js`](frontend/src/app/pages/dashboards/connectivity/hooks/useConnectivityStats.js) [NEW] — Hook konsumsi data latensi, trafik, pool IP, dan perangkat.
      - [`frontend/src/app/pages/dashboards/connectivity/components/DeviceHealthCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/DeviceHealthCard.jsx) [NEW] — Metrik ketersediaan router, switch, dan perangkat jaringan.
      - [`frontend/src/app/pages/dashboards/connectivity/components/LatencyStatusCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/LatencyStatusCard.jsx) [NEW] — Status latensi rata-rata dan persentase *packet loss* per target probe.
      - [`frontend/src/app/pages/dashboards/connectivity/components/TrafficThroughputCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/TrafficThroughputCard.jsx) [NEW] — Pemantauan throughput *inbound* dan *outbound* real-time.
      - [`frontend/src/app/pages/dashboards/connectivity/components/IPv4PoolCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/IPv4PoolCard.jsx) [NEW] — Bar utilisasi kapasitas pool IPv4 publik/privat.
      - [`frontend/src/app/pages/dashboards/connectivity/components/OltHealthCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/OltHealthCard.jsx) [NEW] — Status kesehatan perangkat OLT fiber optik dan port PON.
    - **Dashboard Problem & NOC Tiket**:
      - [`frontend/src/app/pages/dashboards/problem/index.jsx`](frontend/src/app/pages/dashboards/problem/index.jsx) — Dasbor penanganan gangguan dan helpdesk.
      - [`frontend/src/app/pages/dashboards/problem/hooks/useProblemStats.js`](frontend/src/app/pages/dashboards/problem/hooks/useProblemStats.js) [NEW] — Hook integrasi analitik tiket dan beban teknisi.
      - [`frontend/src/app/pages/dashboards/problem/components/PipelineCard.jsx`](frontend/src/app/pages/dashboards/problem/components/PipelineCard.jsx) [NEW] — Visualisasi pipeline tiket dari baru hingga selesai disertai peringatan tiket mangkrak.
      - [`frontend/src/app/pages/dashboards/problem/components/CategoryChart.jsx`](frontend/src/app/pages/dashboards/problem/components/CategoryChart.jsx) [NEW] — Grafik kategori kendala paling sering dilaporkan pelanggan.
      - [`frontend/src/app/pages/dashboards/problem/components/ResolutionTimeCard.jsx`](frontend/src/app/pages/dashboards/problem/components/ResolutionTimeCard.jsx) [NEW] — Indikator kecepatan tim menyelesaikan tiket (*Mean Time to Resolve*).
      - [`frontend/src/app/pages/dashboards/problem/components/WorkOrderCard.jsx`](frontend/src/app/pages/dashboards/problem/components/WorkOrderCard.jsx) [NEW] — Status Surat Perintah Kerja (SPK/WO) instalasi vs perbaikan.
      - [`frontend/src/app/pages/dashboards/problem/components/TechnicianWorkloadTable.jsx`](frontend/src/app/pages/dashboards/problem/components/TechnicianWorkloadTable.jsx) [NEW] — Matriks beban tugas aktif per staf teknisi dan status absensi harian.
    - **Dashboard Finance & Billing**:
      - [`frontend/src/app/pages/dashboards/finance/index.jsx`](frontend/src/app/pages/dashboards/finance/index.jsx) — Dasbor keuangan manajerial ISP.
      - [`frontend/src/app/pages/dashboards/finance/hooks/useFinanceStats.js`](frontend/src/app/pages/dashboards/finance/hooks/useFinanceStats.js) [NEW] — Hook data kas, piutang, isolir, dan pembayaran online.
      - [`frontend/src/app/pages/dashboards/finance/components/CashFlowCard.jsx`](frontend/src/app/pages/dashboards/finance/components/CashFlowCard.jsx) [NEW] — Metrik pendapatan, beban operasional, dan estimasi laba bersih bulan berjalan.
      - [`frontend/src/app/pages/dashboards/finance/components/ReceivableAgingCard.jsx`](frontend/src/app/pages/dashboards/finance/components/ReceivableAgingCard.jsx) [NEW] — Distribusi umur piutang (*Account Receivable Aging*: 0-30, 31-60, 61-90, >90 hari).
      - [`frontend/src/app/pages/dashboards/finance/components/IsolirCard.jsx`](frontend/src/app/pages/dashboards/finance/components/IsolirCard.jsx) [NEW] — Rekapitulasi eksekusi otomasi isolir pelanggan menunggak.
      - [`frontend/src/app/pages/dashboards/finance/components/GatewayReconciliationCard.jsx`](frontend/src/app/pages/dashboards/finance/components/GatewayReconciliationCard.jsx) [NEW] — Volume transaksi gateway pembayaran, pencairan bersih, dan transaksi belum dibukukan.
      - [`frontend/src/app/pages/dashboards/finance/components/RegulatoryObligationCard.jsx`](frontend/src/app/pages/dashboards/finance/components/RegulatoryObligationCard.jsx) [NEW] — Estimasi kewajiban regulasi telekomunikasi (BHP, USO, PPh Final UMKM).
    - **Dashboard Sales & Pertumbuhan**:
      - [`frontend/src/app/pages/dashboards/sales/index.jsx`](frontend/src/app/pages/dashboards/sales/index.jsx) — Dasbor performa tim penjualan dan ekspansi pasar.
      - [`frontend/src/app/pages/dashboards/sales/hooks/useSalesStats.js`](frontend/src/app/pages/dashboards/sales/hooks/useSalesStats.js) [NEW] — Hook data pipeline prospek, paket terlaris, dan wilayah.
      - [`frontend/src/app/pages/dashboards/sales/components/SalesFunnelChart.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesFunnelChart.jsx) [NEW] — Grafik konversi prospek dari calon pelanggan hingga terpasang.
      - [`frontend/src/app/pages/dashboards/sales/components/RegionGrowthCard.jsx`](frontend/src/app/pages/dashboards/sales/components/RegionGrowthCard.jsx) [NEW] — Kepadatan dan pertumbuhan pelanggan per POP.
      - [`frontend/src/app/pages/dashboards/sales/components/TopProductsCard.jsx`](frontend/src/app/pages/dashboards/sales/components/TopProductsCard.jsx) [NEW] — Peringkat paket langganan broadband paling diminati.
      - [`frontend/src/app/pages/dashboards/sales/components/SalesPerformanceTable.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesPerformanceTable.jsx) [NEW] — Tabel peringkat pencapaian *closing* per staf sales/PIC.
    - **Dashboard Hotspot**:
      - [`frontend/src/app/pages/dashboards/hotspot/index.jsx`](frontend/src/app/pages/dashboards/hotspot/index.jsx) — Dasbor analitik operasional jaringan hotspot voucher.
      - [`frontend/src/app/pages/dashboards/hotspot/hooks/useHotspotStats.js`](frontend/src/app/pages/dashboards/hotspot/hooks/useHotspotStats.js) [NEW] — Hook data penjualan dan inventori voucher.
      - [`frontend/src/app/pages/dashboards/hotspot/components/DailySalesCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/DailySalesCard.jsx) [NEW] — Perbandingan penjualan voucher hari ini terhadap hari sebelumnya.
      - [`frontend/src/app/pages/dashboards/hotspot/components/VoucherStatusCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/VoucherStatusCard.jsx) [NEW] — Komposisi status voucher dalam sirkulasi.
      - [`frontend/src/app/pages/dashboards/hotspot/components/OnlineUsersCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/OnlineUsersCard.jsx) [NEW] — Metrik pengguna hotspot yang sedang terhubung aktif.
      - [`frontend/src/app/pages/dashboards/hotspot/components/TopLocationsCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/TopLocationsCard.jsx) [NEW] — Titik lokasi hotspot dengan traffic penjualan terbesar.
    - **Internasionalisasi (i18n)**:
      - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) — Penambahan lebih dari 200 key terjemahan mencakup seluruh label, metrik, kartu, dan status pada kelima dashboard baru.

---

## 🌿 Branch: `master` / `issue-291` — Partner API: Modul Faktur/Tagihan Mitra Bisnis POP (Partner App Invoice Management & Access Scoping)

### 📌 Informasi Issue

- **Nomor Issue**: #291
- **Judul Issue**: Partner API: Modul Faktur/Tagihan Mitra Bisnis (POP) — Endpoint Listing, Detail, Pengamanan Akses Scope Mitra, Integrasi Pembayaran Online/Manual, dan Suite Pengujian Integrasi
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `e77f2d0`)

---

### 📅 Rincian Commit

#### [`e77f2d0`](https://github.com/user/repo/commit/e77f2d0) - resolve #291 - 11 September 2026, 22:47:28 WIB (Merge to `master`)
#### [`715c174`](https://github.com/user/repo/commit/715c174) - resolve #291 - 11 September 2026, 22:46:54 WIB (Refactoring, Sanitasi Keamanan & Pengujian)
#### [`2a78d71`](https://github.com/user/repo/commit/2a78d71) - resolve #291 - 11 September 2026, 16:59:52 WIB (Implementasi Awal oleh Idham)

- **Komponen yang Berubah**:
  - **Backend Core — Controller, Rute & Service Faktur Partner**:
    - [`backend/src/controllers/partnerApiInvoice.controller.js`](backend/src/controllers/partnerApiInvoice.controller.js) [NEW] — Controller penanganan endpoint faktur pada antarmuka Partner API:
      - `listPartnerAppInvoice`: Handler `GET /p-api/v1/invoices` dengan proteksi isolasi scope. Pengguna ber-role `partner` dipaksa hanya melihat tagihan milik ID mitranya sendiri (`req.user.partner_id`), mengabaikan parameter query luar. Untuk akun admin, didukung query `partner_id` dengan validasi tipe data string ketat guna menangkal serangan *NoSQL Injection / Query Selector Bypassing*.
      - Menyaring mitra tersembunyi via helper `withPortalVisibility` (`getHiddenPortalPartnerIds()`).
      - Proyeksi field minimalis (`INVOICE_LIST_FIELDS` & `INVOICE_DETAIL_FIELDS`), meniadakan pemuatan field sensitif dan `_id` MongoDB internal.
      - Pengurutan default berbasis tanggal terbaru (`{ date: -1, created_at: -1 }`).
      - `readPartnerAppInvoice`: Handler `GET /p-api/v1/invoices/:id` untuk detail faktur lengkap dengan informasi gateway pembayaran online dan riwayat pembayaran manual (`FinancePayment`).
    - [`backend/src/routes/partnerApi.route.js`](backend/src/routes/partnerApi.route.js) — Pendaftaran rute dan dokumentasi OpenAPI/Swagger komprehensif untuk `GET /p-api/v1/invoices` dan `GET /p-api/v1/invoices/:id` lengkap dengan skema respons, parameter, dan status error.
    - [`backend/src/services/financeInvoice.service.js`](backend/src/services/financeInvoice.service.js) — Integrasi data pembayaran manual saat faktur lunas untuk pelaporan transparan pada portal mitra.
  - **Testing & Quality Assurance**:
    - [`backend/test/helpers/factories.js`](backend/test/helpers/factories.js) — Penambahan factory helper pembuatan data tiruan pembayaran faktur (`createTestPayment`).
    - [`backend/test/integration/partnerApiInvoice.test.js`](backend/test/integration/partnerApiInvoice.test.js) [NEW] — 562 baris pengujian integrasi otomatis:
      - Verifikasi listing faktur terisolasi per mitra (mitra A tidak dapat melihat tagihan mitra B).
      - Uji bypass keamanan parameter `?partner_id=` oleh akun non-admin.
      - Validasi data pembayaran terlampir pada faktur yang telah berstatus *paid*.
      - Uji penanganan error 404, validasi format ID, dan paginasi data.

---

## 🌿 Branch: `master` / `issue-280` — Modul Pengelolaan Notifikasi Broadcast & Terarah Aplikasi Mobile (Mobile Notification Broadcast & Management)

### 📌 Informasi Issue

- **Nomor Issue**: #280
- **Judul Issue**: Modul Pengelolaan Notifikasi Pelanggan Aplikasi Mobile: Broadcast, Penargetan Spesifik, Penjadwalan, Riwayat Pembacaan, dan Antarmuka Dasbor Admin
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `a876d69`)

---

### 📅 Rincian Commit

#### [`a876d69`](https://github.com/user/repo/commit/a876d69) - resolve #280 - 11 September 2026, 21:10:08 WIB (Merge to `master`)
#### [`e961197`](https://github.com/user/repo/commit/e961197) - resolve #280 - 11 September 2026, 21:09:07 WIB (Merge branch & Resolusi Konflik)
#### [`9312589`](https://github.com/user/repo/commit/9312589) - resolve #280 - 10 September 2026, 18:17:55 WIB (Implementasi Awal oleh Idham)

- **Komponen yang Berubah**:
  - **Backend Core — Model Notifikasi, Logika Broadcast & API Endpoint**:
    - [`backend/src/models/mobileNotification.model.js`](backend/src/models/mobileNotification.model.js) [NEW] — Skema Mongoose notifikasi aplikasi mobile:
      - Field `title`, `body`, `type` (*general*, *maintenance*, *invoice*, *incident*, *promo*), `target_type` (*all*, *specific*, *region*, *package*), `target_ids`, `read_by`, `status`, dan `scheduled_at`.
    - [`backend/src/controllers/mobileNotification.controller.js`](backend/src/controllers/mobileNotification.controller.js) [NEW] & [`backend/src/services/mobileNotification.service.js`](backend/src/services/mobileNotification.service.js) [NEW] — Layanan bisnis pengelolaan notifikasi:
      - Pengiriman siaran masal (*broadcast*) maupun terarah ke kelompok pelanggan tertentu.
      - Paginasi daftar notifikasi dan kalkulasi statistik pembacaan (*delivery rate*, *read count*).
    - [`backend/src/routes/mobileNotification.route.js`](backend/src/routes/mobileNotification.route.js) [NEW] & [`backend/src/routes/mobileCustomer.route.js`](backend/src/routes/mobileCustomer.route.js) — Rute admin dan rute konsumsi notifikasi sisi aplikasi mobile pelanggan.
    - [`backend/src/config/privilege.json`](backend/src/config/privilege.json) — Pendaftaran privilege baru `mobileNotification.read`, `mobileNotification.create`, `mobileNotification.delete`, dan `mobileNotification.export`.
    - [`backend/test/integration/mobileNotification.service.test.js`](backend/test/integration/mobileNotification.service.test.js) [NEW] — 414 baris uji integrasi validasi pembuatan, penargetan pelanggan, dan mutasi status notifikasi.
  - **Frontend — Antarmuka Manajemen Notifikasi Mobile**:
    - [`frontend/src/app/pages/mobileApp/notification/index.jsx`](frontend/src/app/pages/mobileApp/notification/index.jsx) [NEW] — Halaman tabel riwayat notifikasi dengan filter tipe, status, pencarian judul, dan aksi massal.
    - [`frontend/src/app/pages/mobileApp/notification/create.jsx`](frontend/src/app/pages/mobileApp/notification/create.jsx) [NEW] — Formulir perancangan notifikasi dengan pemilihan target (Semua, Per Wilayah POP, Per Paket, atau Pelanggan Tertentu) dan integrasi date picker untuk penjadwalan.
    - [`frontend/src/app/pages/mobileApp/notification/detail.jsx`](frontend/src/app/pages/mobileApp/notification/detail.jsx) [NEW] — Halaman detail notifikasi yang menampilkan metrik audiens dan daftar pelanggan yang telah membaca pesan.
    - [`frontend/src/app/pages/mobileApp/notification/schema/columns.jsx`](frontend/src/app/pages/mobileApp/notification/schema/columns.jsx) [NEW] & [`NotificationBadge.jsx`](frontend/src/app/pages/mobileApp/notification/schema/NotificationBadge.jsx) [NEW] — Konfigurasi kolom tabel TanStack dan badge visual kategori notifikasi.
    - [`frontend/src/app/navigation/mobileApp.js`](frontend/src/app/navigation/mobileApp.js) & [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx) — Penambahan item navigasi menu dan rute terproteksi RBAC.

---

## 🌿 Branch: `master` / `issue-279` — Modul Tinjauan & Persetujuan Perubahan Layanan Pelanggan Mobile (Mobile Service Change Request Review & Approval)

### 📌 Informasi Issue

- **Nomor Issue**: #279
- **Judul Issue**: Modul Tinjauan & Persetujuan Perubahan Layanan Pelanggan Mobile: Alur Approval Paket Broadband, Komparasi Visual Paket & Bandwidth, Audit Remediasi Keamanan NoSQL Injection, dan Concurrency Control
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `15472f5`)

---

### 📅 Rincian Commit

#### [`15472f5`](https://github.com/user/repo/commit/15472f5) - resolve #279 - 11 September 2026, 20:49:27 WIB (Merge to `master`)
#### [`a20ad99`](https://github.com/user/repo/commit/a20ad99) - resolve #279 - 11 September 2026, 20:48:23 WIB (Remediasi Audit Keamanan, Concurrency & Refactoring)
#### [`2422c8e`](https://github.com/user/repo/commit/2422c8e) - resolve #279 - 9 September 2026, 16:54:47 WIB (Implementasi Awal oleh Idham)

- **Komponen yang Berubah**:
  - **Backend Core — Audit Remediasi & Engine Persetujuan Perubahan Layanan**:
    - [`backend/src/controllers/mobileServiceChange.controller.js`](backend/src/controllers/mobileServiceChange.controller.js) — Implementasi controller permintaan ubah layanan pelanggan:
      - **Remediasi Keamanan NoSQL Injection [SEC-01]**: Menambahkan validasi tipe data string ketat untuk parameter `profile` saat `resp === 'accept'`, serta pembatasan karakter `notes` guna mencegah manipulasi query selector Mongoose.
      - **Remediasi Sanitasi Regex [SEC-02]**: Menerapkan `escapeRegex()` pada filter pencarian relasi sebelum diteruskan ke operator kueri regex MongoDB untuk menangkal ReDoS.
      - **Kepatuhan Status Code [ERR-01]**: Mengubah penolakan validasi bisnis ke kode HTTP `422 Unprocessable Entity` (bukan 400 generik).
    - [`backend/src/services/mobileServiceChange.service.js`](backend/src/services/mobileServiceChange.service.js) — Layanan pemrosesan permohonan:
      - **Pencegahan Race Condition [CONC-01]**: Menggunakan kueri pembaruan atomik bersyarat `{ req_change: authentication.req_change._id }` saat eksekusi keputusan admin, melempar error `409 Conflict` jika permohonan telah disetujui atau dibatalkan pihak lain secara simultan.
      - **Jejak Audit Utuh [LOG-01]**: Memastikan penolakan permohonan tanpa catatan khusus tetap tercatat rapi pada array `history_change`.
      - **Data Minimization [DATA-01]**: Menghapus populate berlebih dan menerapkan proyeksi selektif `.select()`.
    - [`backend/src/models/radiusAuthentication.model.js`](backend/src/models/radiusAuthentication.model.js) & [`backend/src/services/radiusAuthentication.service.js`](backend/src/services/radiusAuthentication.service.js) — Skema pelacakan `req_change` (paket tujuan, alasan, cap waktu pengajuan) dan riwayat perubahan profil langganan pelanggan broadband.
    - [`backend/scripts/backfill-req-change-at.js`](backend/scripts/backfill-req-change-at.js) [NEW] — Script migrasi data untuk mengisi atribut `requested_at` pada data pengajuan lampau.
    - [`backend/test/integration/mobileServiceChange.service.test.js`](backend/test/integration/mobileServiceChange.service.test.js) [NEW] — 584 baris pengujian integrasi komprehensif menguji skenario persetujuan, penolakan, tinjauan, dan proteksi konkurensi ganda.
  - **Frontend — Antarmuka Review Perubahan Paket**:
    - [`frontend/src/app/pages/mobileApp/serviceChange/index.jsx`](frontend/src/app/pages/mobileApp/serviceChange/index.jsx) [NEW] — Datatable permohonan perubahan layanan pelanggan mobile dengan filter status (*pending*, *approved*, *declined*, *review*).
    - [`frontend/src/app/pages/mobileApp/serviceChange/components/ReviewDrawer.jsx`](frontend/src/app/pages/mobileApp/serviceChange/components/ReviewDrawer.jsx) [NEW] — Drawer interaktif untuk mengevaluasi permohonan:
      - Menampilkan komparasi paket lama vs paket baru, profil batas kecepatan (*bandwidth upload/download*), selisih biaya langganan, dan riwayat permohonan pelanggan.
      - Aksi persetujuan (*Accept*), penolakan (*Decline* dengan input alasan wajib), atau penundaan untuk evaluasi teknis (*Review*).
    - [`frontend/src/app/pages/mobileApp/serviceChange/schema/columns.jsx`](frontend/src/app/pages/mobileApp/serviceChange/schema/columns.jsx) [NEW] & [`frontend/src/app/router/mobileApp/serviceChangeRoute.jsx`](frontend/src/app/router/mobileApp/serviceChangeRoute.jsx) [NEW] — Definisi kolom tabel, hook routing terproteksi, dan pembersihan header redundan sesuai arahan UX.

---

## 🌿 Branch: `origin/issue-289` / `issue-284` — Sinkronisasi Integrasi Google Drive Dynamic Redirect URI & Master Dependencies

### 📌 Informasi Issue

- **Nomor Issue**: #284
- **Judul Issue**: Sinkronisasi Pembaruan Google Drive Dynamic Redirect URI OAuth, Database Backup Tool, dan Dependensi Monorepo ke Branch Fitur
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `issue-289` via commit `b784b30`)

---

### 📅 Rincian Commit

#### [`b784b30`](https://github.com/user/repo/commit/b784b30) - resolve #284 - 11 September 2026, 11:31:25 WIB (Merge commit)
#### [`422cf54`](https://github.com/user/repo/commit/422cf54) - resolve #284 - 11 September 2026, 11:30:06 WIB (Sinkronisasi perubahan branch)

- **Komponen yang Berubah**:
  - Penyelarasan perubahan arsitektur integrasi Google Drive Backup yang telah dituntaskan sebelumnya ke dalam branch basis `issue-289`.
  - Memastikan endpoint dynamic redirect URI OAuth (`GET /api/v1/db-tools/gdrive/redirect-uri`), perbaikan modal panduan Test User di Google Cloud Console, serta dependensi modul privilege dan document manager berjalan selaras tanpa diskrepansi pustaka.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| **#289** | Redesign & Implementasi Dashboard Domain Terpadu (Executive, Connectivity, Problem, Finance, Sales, Hotspot V2) | Menyediakan pusat kendali visual operasional terpadu bagi seluruh divisi ISP (Eksekutif, NOC/Teknis, Helpdesk, Billing/Keuangan, Marketing/Sales, dan Hotspot Retail) dengan data metrik real-time berkinerja tinggi. |
| **#291** | Partner API: Modul Faktur/Tagihan Mitra Bisnis (POP) | Mitra bisnis POP kini dapat mengakses, melihat detail rincian, dan melacak status pembayaran faktur/tagihan bulanan secara mandiri via API/Portal mitra dengan isolasi keamanan data yang ketat. |
| **#280** | Pengelolaan Notifikasi Pelanggan Aplikasi Mobile | Memungkinkan tim admin menyiarkan pengumuman pemeliharaan jaringan, notifikasi tagihan, info gangguan lokal, dan promo langsung ke aplikasi mobile pelanggan dengan penargetan presisi. |
| **#279** | Tinjauan & Persetujuan Perubahan Layanan Pelanggan Mobile | Pelanggan dapat mengajukan upgrade/downgrade paket broadband langsung dari aplikasi ponsel, dan tim admin dapat meninjau serta menyetujui perubahan secara instan dengan eksekusi profil teknis otomatis. |
| **#284** | Sinkronisasi Fitur & Dynamic Redirect URI Google Drive | Menjaga konsistensi pustaka kode dan mencegah kegagalan otorisasi cadangan database cloud lintas branch pengembangan. |

### Kemampuan Baru Pengguna/Admin

- **Pemantauan Holistik Eksekutif & Operasional**: Admin dan manajemen kini memiliki akses ke 5 dashboard terfokus yang menyajikan informasi krusial tanpa harus membuka laporan terpisah:
  - Melihat utilisasi IP Pool dan kesehatan OLT/PON dalam satu layar (Connectivity).
  - Melacak rata-rata waktu penyelesaian tiket (*MTTR*) dan beban kerja teknisi lapangan (Problem).
  - Mengawasi arus kas harian, penuaan piutang (*aging*), dan status isolir otomatis (Finance).
  - Memantau tahapan prospek (*sales funnel*) dan persebaran pelanggan per POP (Sales).
  - Melihat omzet penjualan voucher harian dan jumlah pengguna hotspot aktif (*online users*) secara langsung (Hotspot).
- **Manajemen Mandiri Mitra Bisnis**: Mitra POP dapat mengunduh dan memeriksa rincian faktur biaya langganan, termasuk nomor rekening tujuan dan tautan pembayaran online tanpa perlu meminta rekapitulasi manual kepada tim finance.
- **Broadcast & Komunikasi Langsung**: Admin marketing dan operasional dapat mengirim pesan peringatan gangguan darurat ke pelanggan di wilayah POP terdampak secara cepat dengan target audiens terseleksi.
- **Workflow Persetujuan Layanan Pelanggan Cepat**: Admin dapat langsung menyetujui kenaikan paket langganan pelanggan dari antarmuka drawer modern dengan perbandingan spesifikasi kecepatan yang jelas dan perlindungan dari persetujuan ganda (*anti-race condition*).

### Bug Fix / Solusi Masalah

- **Celah Keamanan NoSQL Injection pada Modul Layanan Mobile**: Menutup potensi eksploitasi query selector MongoDB pada endpoint persetujuan layanan melalui validasi tipe string yang ketat dan sanitasi ekspresi reguler (ReDoS prevention).
- **Race Condition Persetujuan Ganda**: Menggunakan pembaruan bersyarat atomik untuk menjamin bahwa permohonan yang telah diubah atau dibatalkan oleh pihak lain akan ditolak dengan status `409 Conflict`.
- **Kebocoran Scope Data Mitra Bisnis pada API**: Mencegah mitra mengakses data tagihan mitra lain melalui penegakan penguncian filter ID mitra di tingkat middleware dan controller server.
- **Eliminasi Tiket Mangkrak (*Overdue Alert*)**: Menambahkan deteksi otomatis untuk tiket gangguan yang melewati 7 hari masa penanganan agar segera mendapat eskalasi prioritas tim NOC.

### Menu/Fitur Baru

- **Dasbor Domain Spesifik**: Menu dasbor baru pada navigasi utama:
  - `Executive Overview` (`/`)
  - `Connectivity Dashboard` (`/dashboards/connectivity`)
  - `Problem/Ticket Dashboard` (`/dashboards/problem`)
  - `Finance Dashboard` (`/dashboards/finance`)
  - `Sales Dashboard` (`/dashboards/sales`)
  - `Hotspot Dashboard` (`/dashboards/hotspot`)
- **Menu Pengelolaan Notifikasi Mobile**: Antarmuka di `/mobile-app/notifications` untuk merancang, menjadwalkan, dan melihat analitik pembacaan notifikasi broadcast.
- **Menu Permohonan Ubah Layanan**: Antarmuka di `/mobile-app/service-change` untuk meninjau pengajuan paket broadband pengguna ponsel secara interaktif.
- **Endpoint Faktur Partner API**: Rute `/p-api/v1/invoices` dan `/p-api/v1/invoices/:id` untuk integrasi portal mitra bisnis.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Dashboard Domain Spesifik (Operational Dashboards V2)
- **Penjelasan Fitur**: Dasbor domain spesifik dirancang untuk memenuhi kebutuhan analitik operasional harian tiap divisi. Setiap dasbor dilengkapi kartu metrik utama, grafik tren, dan visualisasi status yang dilindungi izin berbasis hak akses (*Role-Based Access Control*).
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu samping utama (*sidebar*), klik kelompok menu **Dashboards**.
  2. Pilih dasbor sesuai kebutuhan:
     - Klik **Connectivity** untuk melihat status jaringan, perangkat online/offline, latensi, utilisasi IPv4, dan status OLT.
     - Klik **Problem** untuk mengevaluasi beban tiket gangguan, rata-rata durasi penutupan tiket, dan penugasan teknisi.
     - Klik **Finance** untuk menganalisis arus kas bulanan, umur piutang (*AR Aging*), dan rekonsiliasi payment gateway.
     - Klik **Sales** untuk melihat konversi prospek pelanggan dari Leads hingga Instalasi serta wilayah dengan pertumbuhan tercepat.
     - Klik **Hotspot** untuk memantau performa penjualan voucher harian dan jumlah user yang sedang online.
  3. Setiap kartu metrik menyediakan tautan langsung (*Lihat detail*) untuk melompat ke tabel modul operasional terkait.

### 2. Tinjauan Permohonan Perubahan Layanan Pelanggan Mobile
- **Penjelasan Fitur**: Fitur ini memungkinkan staf operasional mengevaluasi permintaan pergantian paket langganan (upgrade/downgrade) yang diajukan pelanggan melalui aplikasi mobile DEKASIMAL.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Mobile App** > **Perubahan Layanan** (`/mobile-app/service-change`).
  2. Pada tabel daftar permintaan, cari baris dengan status **Pending** (*Menunggu Tinjauan*).
  3. Klik tombol aksi **Tinjau** pada baris pelanggan yang bersangkutan untuk membuka **Review Drawer**.
  4. Periksa perbandingan paket saat ini dengan paket baru yang diajukan, termasuk perubahan kuota bandwidth dan estimasi tagihan baru.
  5. Pilih aksi:
     - Klik **Setujui** untuk langsung menerapkan paket baru pada profil radius pelanggan secara otomatis.
     - Klik **Tolak** lalu isi alasan penolakan pada kolom catatan untuk menginformasikan pelanggan via notifikasi.
     - Klik **Tinjau Ulang** jika membutuhkan konfirmasi survei teknis lebih lanjut.
