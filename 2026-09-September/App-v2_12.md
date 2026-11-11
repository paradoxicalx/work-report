# 📝 Daily Work Report - Dedy S.N Putra (2026-09-12)

---

## 📅 Laporan Harian - 12 September 2026

---

## 🌿 Branch: `issue-289` — Redesign & Implementasi Dashboard Domain Terpadu: Executive/Home, Connectivity, Problem/Tiket, Finance & Billing, Sales & Prospek, Hotspot, Warehouse, dan WhatsApp (Domain-Specific Operational Dashboards V2)

### 📌 Informasi Issue

- **Nomor Issue**: #289
- **Judul Issue**: Redesign & Implementasi Dashboard Domain Terpadu: Executive Overview, Connectivity & Network Health, Problem & NOC Tiket, Finance & Billing Analytics, Sales & Funnel Pipeline, Hotspot Voucher Analytics, Warehouse & Logistik, serta WhatsApp Customer Service (Domain-Specific Operational Dashboards V2)
- **Status Branch**: `Belum di-merge` (Branch aktif dalam pengembangan di lokal `issue-289`)

---

### 📅 Rincian Perubahan (Work in Progress & Uncommitted Files)

#### `[WIP - Active Working Tree]` - implement domain-specific operational dashboards v2 & landing page revamp - 12 September 2026

- **Komponen yang Berubah**:
  - **1. Backend Core — Endpoint Agregasi Universal, Service Baru, dan Penanganan Database**:
    - [`backend/src/routes/dashboard.route.js`](backend/src/routes/dashboard.route.js) [NEW] & [`backend/src/controllers/dashboard.controller.js`](backend/src/controllers/dashboard.controller.js) [NEW] — Pembuatan rute dan controller `GET /api/v1/dashboard/home-overview` yang dibungkus `asyncHandler` dan diproteksi `protectedAdmin`.
    - [`backend/src/services/dashboard.service.js`](backend/src/services/dashboard.service.js) [NEW] — Agregasi MongoDB berkinerja tinggi (<50ms) untuk dashboard landing page:
      - Menghitung status operasional platform dan kesehatan 5 microservices inti (Database MongoDB, Redis Queue, RADIUS Daemon, WhatsApp Gateway, Network Monitor).
      - Menghitung statistik pelanggan broadband non-sensitif (total, aktif, nonaktif, online saat ini).
      - Menghitung ketersediaan perangkat jaringan (`UP`, `DOWN`, ketersediaan persentase).
      - Menghitung pipeline tiket gangguan, tiket melewati SLA target 7 hari (*overdue*), dan tiket terselesaikan hari ini.
      - Menghitung sesi hotspot aktif dan antrean obrolan WhatsApp belum dibaca (*unread count*).
      - **Zero Sensitive Data:** Tidak memuat data keuangan apa pun (bebas dari kas, MRR, omzet, piutang) sehingga aman diakses oleh staf dari semua tingkatan tanpa error 403.
    - [`backend/src/app.js`](backend/src/app.js) — Mendaftarkan `DashboardRoute` ke `/api/v1`.
    - [`backend/src/controllers/financeInvoice.controller.js`](backend/src/controllers/financeInvoice.controller.js), [`backend/src/routes/financeInvoice.route.js`](backend/src/routes/financeInvoice.route.js), & [`backend/src/services/financeInvoice.service.js`](backend/src/services/financeInvoice.service.js) — Endpoint `GET /api/v1/finance/invoice/summary`:
      - Agregasi `$facet` MongoDB untuk penagihan/faktur: total terbit, faktur lunas, sisa tagihan aktif, tagihan lewat jatuh tempo (*overdue*), tingkat efektivitas penagihan (*collection rate %*), distribusi per kategori penerima (Retail, Bisnis, Mitra, Umum), dan tren komparasi terbit vs tertagih 6 bulan terakhir.
    - [`backend/src/services/hotspotVoucher.service.js`](backend/src/services/hotspotVoucher.service.js) & [`backend/src/controllers/hotspotVoucher.controller.js`](backend/src/controllers/hotspotVoucher.controller.js) — Endpoint `GET /api/v1/hotspot-voucher/dashboard/stats`:
      - Agregasi performa penjualan voucher harian hari ini vs kemarin (delta nominal & volume), tren penjualan harian 7 hari vs bulanan 6 bulan, siklus 4 status voucher, popularitas paket kecepatan (*profile distribution*), sesi aktif real-time (*live sessions*), dan ranking lokasi titik hotspot teratas.
    - [`backend/src/services/warehouseType.service.js`](backend/src/services/warehouseType.service.js) & [`backend/src/controllers/warehouseType.controller.js`](backend/src/controllers/warehouseType.controller.js) — Perbaikan bug Mongoose `MissingSchemaError` pada pemanggilan `WarehouseLogs.populate()` dengan mendaftarkan import model `WarehouseItem` dan `Admin` secara eksplisit, serta pengayaan metrik `getWarehouseOverviewReport()` untuk mencakup 3.200+ unit aset fisik, 700+ riwayat mutasi, komposisi kategori material, dan distribusi kondisi unit.
    - [`backend/src/services/waConversation.service.js`](backend/src/services/waConversation.service.js) & [`backend/src/controllers/waChat.controller.js`](backend/src/controllers/waChat.controller.js) — Endpoint `GET /api/v1/whatsapp/dashboard/stats`:
      - Agregasi pipeline MongoDB paralel (<20ms) menghitung statistik sesi percakapan, distribusi channel (Meta Cloud API resmi vs Baileys multi-device), status pesan masuk/keluar (*inbound* vs *outbound*), kecepatan rata-rata respon admin CS, antrean pesan pelanggan belum terbalas (*unreplied queue*), dan riwayat kampanye siaran pesan (*broadcast*).
    - [`backend/src/services/ticket.service.js`](backend/src/services/ticket.service.js), [`backend/src/services/workOrder.service.js`](backend/src/services/workOrder.service.js), & [`backend/src/controllers/ticket.controller.js`](backend/src/controllers/ticket.controller.js) — Endpoint `GET /api/v1/ticket/dashboard/problem-stats` & metrik SLA, MTTR (*Mean Time to Resolve*), pipeline tiket 4 status, perbandingan jenis gangguan pelanggan vs infrastruktur, beban kerja teknisi lapangan, dan ketercapaian target penyelesaian 7 hari.
    - [`backend/src/services/networkIPv4.service.js`](backend/src/services/networkIPv4.service.js) & [`backend/src/controllers/networkIPv4.controller.js`](backend/src/controllers/networkIPv4.controller.js) — Endpoint `GET /api/v1/network/ipv4/stats` menghitung kapasitas alokasi host, IP terpakai, dan ketersediaan blok subnet.
    - [`backend/src/services/radiusAuthentication.service.js`](backend/src/services/radiusAuthentication.service.js), [`backend/src/controllers/radiusAuthentication.controller.js`](backend/src/controllers/radiusAuthentication.controller.js), & [`backend/src/routes/radiusAuthentication.route.js`](backend/src/routes/radiusAuthentication.route.js) — Endpoint `GET /api/v1/broadband/dashboard/distribution` dan `GET /api/v1/broadband/dashboard/growth`.

  - **2. Stabilitas Microservices — Timeout Protection pada Logging Fatal Error**:
    - [`baileys-api/src/utils/logger.js`](baileys-api/src/utils/logger.js), [`network-monitor/src/utils/logger.js`](network-monitor/src/utils/logger.js), [`telegram-api/src/utils/logger.js`](telegram-api/src/utils/logger.js), & [`whatsapp-api/src/utils/logger.js`](whatsapp-api/src/utils/logger.js) — Menambahkan `signal: AbortSignal.timeout(5000)` pada fungsi `reportFatalErrorToBackend` agar panggilan fetch pelaporan error ke backend tidak menggantung selamanya (*hanging socket*) bila backend sedang tidak merespon.

  - **3. Frontend — Navigasi & Routing Protected**:
    - [`frontend/src/app/navigation/dashboards.js`](frontend/src/app/navigation/dashboards.js) — Mendaftarkan menu `Home`, pembatas divider, penyesuaian hak akses RBAC per menu, serta penambahan menu baru `Warehouse` (`warehouseType.list|warehouseItem.list`) dan `WhatsApp` (`whatsappChat.list`).
    - [`frontend/src/app/router/protected.jsx`](frontend/src/app/router/protected.jsx) — Mengaktifkan rute utama `/dashboards` ke modul Home secara langsung (bukan redirect ke changelog) serta mendaftarkan lazy-loaded routes untuk `/dashboards/warehouse` dan `/dashboards/whatsapp`.

  - **4. Frontend — Halaman Utama Dashboard (`/dashboards`) Bergaya Tailux**:
    - [`frontend/src/app/pages/dashboards/home/index.jsx`](frontend/src/app/pages/dashboards/home/index.jsx) — Portal pendaratan utama operasional 360°.
    - [`frontend/src/app/pages/dashboards/home/hooks/useHomeStats.js`](frontend/src/app/pages/dashboards/home/hooks/useHomeStats.js) [NEW] — Hook konsumsi data dari endpoint universal `home-overview`.
    - [`frontend/src/app/pages/dashboards/home/components/HomeDashboardHeader.jsx`](frontend/src/app/pages/dashboards/home/components/HomeDashboardHeader.jsx) [NEW] — Hero Banner bergradien modern (`from-primary-600 via-primary-700 to-indigo-700`) dengan ambient blur, indikator live platform, wadah pedestal kaca kontras tinggi untuk ilustrasi SVG [`dashboard-meet.svg`](frontend/src/assets/illustrations/dashboard-meet.svg), dan tombol navigasi terintegrasi.
    - [`frontend/src/app/pages/dashboards/home/components/HomeKpiStrip.jsx`](frontend/src/app/pages/dashboards/home/components/HomeKpiStrip.jsx) [NEW] — 5 Metrik operasional bergaya Tailux dengan avatar squircle (`mask is-squircle rounded-none shadow-xs`) dan token warna semantik (`this:primary`, `this:success`, `this:warning`, `this:info`, `this:secondary`).
    - [`frontend/src/app/pages/dashboards/home/components/DepartmentHubCard.jsx`](frontend/src/app/pages/dashboards/home/components/DepartmentHubCard.jsx) [NEW] — Ubin hub operasional divisi ke 8 modul departemen dengan indikator privilege dinamis (`Tersedia` vs `Akses Terbatas`).
    - [`frontend/src/app/pages/dashboards/home/components/SystemHealthCard.jsx`](frontend/src/app/pages/dashboards/home/components/SystemHealthCard.jsx) [NEW] — Monitoring kesehatan 5 microservices dan distribusi perangkat online UP vs offline DOWN.
    - [`frontend/src/app/pages/dashboards/home/components/TicketPipelineCard.jsx`](frontend/src/app/pages/dashboards/home/components/TicketPipelineCard.jsx) [NEW] — Stat chips pipeline tiket helpdesk dan callout peringatan SLA.
    - [`frontend/src/app/pages/dashboards/home/components/HomeQuickActions.jsx`](frontend/src/app/pages/dashboards/home/components/HomeQuickActions.jsx) [NEW] — Pintasan aksi cepat harian berwarna cerah semantik berbasis hak akses staf (`useHasPrivilege`).
    - [`frontend/src/app/pages/dashboards/home/components/HomeDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/home/components/HomeDashboardSkeleton.jsx) [NEW] — Placeholder shimmer loader presisi tanpa layout shift.

  - **5. Frontend — Elevasi UI Dashboard Sales (`/dashboards/sales`) Mengacu Template Tailux**:
    - [`frontend/src/app/pages/dashboards/sales/index.jsx`](frontend/src/app/pages/dashboards/sales/index.jsx) — Tata letak simetris 2-kolom (`xl:grid-cols-2`) dengan live beacon timestamp.
    - [`frontend/src/app/pages/dashboards/sales/components/SalesKpiStrip.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesKpiStrip.jsx) [NEW] — 4 Metrik sales (`Prospek Masuk`, `Broadband Aktif`, `Win Rate %`, `Nilai MRR Berjalan`) dengan squircle avatar Tailux.
    - [`frontend/src/app/pages/dashboards/sales/components/SalesFunnelChart.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesFunnelChart.jsx) [NEW] — Visualisasi funnel horizontal ApexCharts dan segmented stage cards.
    - [`frontend/src/app/pages/dashboards/sales/components/PipelineStatusCard.jsx`](frontend/src/app/pages/dashboards/sales/components/PipelineStatusCard.jsx) [NEW] — Progress alur 5 tahapan prospek dan peringatan stagnasi penawaran >14 hari.
    - [`frontend/src/app/pages/dashboards/sales/components/BroadbandServiceCard.jsx`](frontend/src/app/pages/dashboards/sales/components/BroadbandServiceCard.jsx) [NEW] — Grid stat chips status pelanggan online, nonaktif, dan isolir.
    - [`frontend/src/app/pages/dashboards/sales/components/BroadbandRevenueCard.jsx`](frontend/src/app/pages/dashboards/sales/components/BroadbandRevenueCard.jsx) [NEW] — Analitik neraca pendapatan MRR & ARPU serta kontribusi paket produk terlaris.
    - [`frontend/src/app/pages/dashboards/sales/components/RegionGrowthCard.jsx`](frontend/src/app/pages/dashboards/sales/components/RegionGrowthCard.jsx) [NEW] — Tab filter wilayah (Area, Provinsi, Kabupaten/Kota) dengan ranking pill pelanggan.
    - [`frontend/src/app/pages/dashboards/sales/components/TopProductsCard.jsx`](frontend/src/app/pages/dashboards/sales/components/TopProductsCard.jsx) [NEW] — Daftar katalog paket internet broadband paling banyak diminati.
    - [`frontend/src/app/pages/dashboards/sales/components/SalesPerformanceTable.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesPerformanceTable.jsx) [NEW] — Leaderboard klasemen kinerja tim sales dengan medali ranking (Emas, Perak, Perunggu).
    - [`frontend/src/app/pages/dashboards/sales/components/LostReasonCard.jsx`](frontend/src/app/pages/dashboards/sales/components/LostReasonCard.jsx) [NEW] — Analisis faktor kegagalan dan alasan pembatalan prospek.
    - [`frontend/src/app/pages/dashboards/sales/components/SalesQuickActions.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesQuickActions.jsx) [NEW] & [`SalesDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesDashboardSkeleton.jsx) [NEW].

  - **6. Frontend — Revamp Dashboard Gangguan & Penanganan Tiket (`/dashboards/problem`)**:
    - [`frontend/src/app/pages/dashboards/problem/index.jsx`](frontend/src/app/pages/dashboards/problem/index.jsx) — Tata letak simetris 2-kolom (`xl:grid-cols-2`, `items-stretch`).
    - [`frontend/src/app/pages/dashboards/problem/components/ProblemKpiStrip.jsx`](frontend/src/app/pages/dashboards/problem/components/ProblemKpiStrip.jsx) [NEW] — 4 Metrik (`Tiket Aktif`, `Tingkat Selesai %`, `MTTR Durasi`, `Work Order Lapangan`).
    - [`frontend/src/app/pages/dashboards/problem/components/PipelineCard.jsx`](frontend/src/app/pages/dashboards/problem/components/PipelineCard.jsx) [NEW] & [`CategoryChart.jsx`](frontend/src/app/pages/dashboards/problem/components/CategoryChart.jsx) [NEW] — Pipeline tiket dan grafik horizontal kategori kendala terbanyak.
    - [`frontend/src/app/pages/dashboards/problem/components/ResolutionTimeCard.jsx`](frontend/src/app/pages/dashboards/problem/components/ResolutionTimeCard.jsx) [NEW] & [`WorkOrderCard.jsx`](frontend/src/app/pages/dashboards/problem/components/WorkOrderCard.jsx) [NEW] — Waktu penanganan rata-rata MTTR vs progress Surat Perintah Kerja teknisi.
    - [`frontend/src/app/pages/dashboards/problem/components/TechnicianWorkloadCard.jsx`](frontend/src/app/pages/dashboards/problem/components/TechnicianWorkloadCard.jsx) [NEW] & [`IncidentSeverityCard.jsx`](frontend/src/app/pages/dashboards/problem/components/IncidentSeverityCard.jsx) [NEW] — Beban kerja teknisi (tiket, WO, presensi harian) vs pemantauan SLA kepatuhan 7 hari dan tiket overdue.
    - [`frontend/src/app/pages/dashboards/problem/components/ProblemQuickActions.jsx`](frontend/src/app/pages/dashboards/problem/components/ProblemQuickActions.jsx) [NEW] & [`ProblemDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/problem/components/ProblemDashboardSkeleton.jsx) [NEW].

  - **7. Frontend — Revamp Dashboard Konektivitas & Jaringan (`/dashboards/connectivity`)**:
    - [`frontend/src/app/pages/dashboards/connectivity/index.jsx`](frontend/src/app/pages/dashboards/connectivity/index.jsx) — Tata letak simetris 2-kolom seimbang.
    - [`frontend/src/app/pages/dashboards/connectivity/components/ConnectivityKpiStrip.jsx`](frontend/src/app/pages/dashboards/connectivity/components/ConnectivityKpiStrip.jsx) [NEW] — 4 Metrik (`Perangkat Online Rate`, `Throughput Agregat`, `Stabilitas Latency & Loss`, `Utilisasi Pool IPv4`).
    - [`frontend/src/app/pages/dashboards/connectivity/components/DeviceHealthCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/DeviceHealthCard.jsx) [NEW] & [`OltHealthCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/OltHealthCard.jsx) [NEW] — Donut chart kesehatan unit perangkat dan status GPON OLT.
    - [`frontend/src/app/pages/dashboards/connectivity/components/TrafficThroughputCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/TrafficThroughputCard.jsx) [NEW] & [`LatencyStatusCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/LatencyStatusCard.jsx) [NEW] — Monitor aliran Inbound vs Outbound dan target probe latensi.
    - [`frontend/src/app/pages/dashboards/connectivity/components/IPv4PoolCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/IPv4PoolCard.jsx) [NEW] & [`NetworkAlertSummaryCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/NetworkAlertSummaryCard.jsx) [NEW] — Progress alokasi IP publik/privat dan sintesis peringatan kesehatan jaringan operasional (mengisi slot kosong layout sebelumnya).
    - [`frontend/src/app/pages/dashboards/connectivity/components/ConnectivityQuickActions.jsx`](frontend/src/app/pages/dashboards/connectivity/components/ConnectivityQuickActions.jsx) [NEW] & [`ConnectivityDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/connectivity/components/ConnectivityDashboardSkeleton.jsx) [NEW].

  - **8. Frontend — Revamp & Modernisasi Dashboard Hotspot (`/dashboards/hotspot`)**:
    - [`frontend/src/app/pages/dashboards/hotspot/index.jsx`](frontend/src/app/pages/dashboards/hotspot/index.jsx) — Pusat kendali analitik operasional voucher hotspot.
    - [`frontend/src/app/pages/dashboards/hotspot/components/HotspotKpiStrip.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotKpiStrip.jsx) [NEW] — 5 Metrik (`Penjualan Hari Ini`, `Voucher Aktif`, `Pengguna Online Real-time`, `Stok Siap Jual`, `Kedaluwarsa/Isolir`).
    - [`frontend/src/app/pages/dashboards/hotspot/components/HotspotSalesTrendCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotSalesTrendCard.jsx) [NEW] — Area chart ApexCharts dengan tombol toggle Nilai (Rp) vs Volume (Pcs) dan periode 7 Hari vs 6 Bulan.
    - [`frontend/src/app/pages/dashboards/hotspot/components/VoucherStatusCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/VoucherStatusCard.jsx) [NEW] & [`ProfileDistributionCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/ProfileDistributionCard.jsx) [NEW] — Donut chart inventaris voucher dan bar popularitas profil paket kecepatan.
    - [`frontend/src/app/pages/dashboards/hotspot/components/HotspotSessionsCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotSessionsCard.jsx) [NEW] & [`TopLocationsCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/TopLocationsCard.jsx) [NEW] — Tabel live sesi koneksi pengguna hotspot dan tab peringkat lokasi titik hotspot serta kelompok batch voucher.
    - [`frontend/src/app/pages/dashboards/hotspot/components/HotspotQuickActions.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotQuickActions.jsx) [NEW] & [`HotspotDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotDashboardSkeleton.jsx) [NEW].

  - **9. Frontend — Revamp Dashboard Gudang & Logistik (`/dashboards/warehouse`)**:
    - [`frontend/src/app/pages/dashboards/warehouse/index.jsx`](frontend/src/app/pages/dashboards/warehouse/index.jsx) [NEW] & [`hooks/useWarehouseStats.js`](frontend/src/app/pages/dashboards/warehouse/hooks/useWarehouseStats.js) [NEW] — Halaman dashboard terpadu logistik material dan unit aset fisik ISP.
    - [`frontend/src/app/pages/dashboards/warehouse/components/WarehouseKpiStrip.jsx`](frontend/src/app/pages/dashboards/warehouse/components/WarehouseKpiStrip.jsx) [NEW] — 5 Metrik (`Katalog Varian`, `Total Unit Fisik`, `Stok Siap Pakai`, `Peringatan Stok Kritis`, `Permintaan Menunggu`).
    - [`frontend/src/app/pages/dashboards/warehouse/components/AssetDistributionCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/AssetDistributionCard.jsx) [NEW] & [`LowStockCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/LowStockCard.jsx) [NEW] — Donut chart status aset di gudang/pelanggan dan deteksi jenis barang di bawah batas minimum (*low stock alert*).
    - [`frontend/src/app/pages/dashboards/warehouse/components/CategoryBreakdownCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/CategoryBreakdownCard.jsx) [NEW], [`RequestStatusCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/RequestStatusCard.jsx) [NEW] (alur tiket permohonan & fulfillment rate), [`RecentActivityCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/RecentActivityCard.jsx) [NEW] (riwayat mutasi unit terkini), [`WarehouseQuickActions.jsx`](frontend/src/app/pages/dashboards/warehouse/components/WarehouseQuickActions.jsx) [NEW], & [`WarehouseDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/warehouse/components/WarehouseDashboardSkeleton.jsx) [NEW].

  - **10. Frontend — Revamp Dashboard WhatsApp & Layanan Pesan (`/dashboards/whatsapp`)**:
    - [`frontend/src/app/pages/dashboards/whatsapp/index.jsx`](frontend/src/app/pages/dashboards/whatsapp/index.jsx) [NEW] & [`hooks/useWhatsappStats.js`](frontend/src/app/pages/dashboards/whatsapp/hooks/useWhatsappStats.js) [NEW] — Monitoring layanan pesan multi-channel (Meta Cloud API & Baileys multi-device).
    - [`frontend/src/app/pages/dashboards/whatsapp/components/WhatsappKpiStrip.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/WhatsappKpiStrip.jsx) [NEW] — 5 Metrik (`Koneksi Akun WhatsApp`, `Pesan Menunggu Balasan`, `Rata-rata Respon CS`, `Total Sesi Obrolan`, `Efektivitas Siaran Pesan`).
    - [`frontend/src/app/pages/dashboards/whatsapp/components/ConnectionStatusCard.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/ConnectionStatusCard.jsx) [NEW] & [`ResponseTimeCard.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/ResponseTimeCard.jsx) [NEW] — Detail status akun Meta & Baileys, kecepatan respon CS, dan antrean pesan unreplied.
    - [`frontend/src/app/pages/dashboards/whatsapp/components/MessageVolumeCard.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/MessageVolumeCard.jsx) [NEW] & [`BroadcastReminderCard.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/BroadcastReminderCard.jsx) [NEW] — Donut chart volume pesan masuk vs keluar dan status pengingat tagihan bulanan otomatis.
    - [`frontend/src/app/pages/dashboards/whatsapp/components/WhatsappQuickActions.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/WhatsappQuickActions.jsx) [NEW] & [`WhatsappDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/WhatsappDashboardSkeleton.jsx) [NEW].

  - **11. Frontend — Analisis Tagihan & Billing Finance (`/dashboards/finance`)**:
    - [`frontend/src/app/pages/dashboards/finance/index.jsx`](frontend/src/app/pages/dashboards/finance/index.jsx) & [`components/FinanceKpiStrip.jsx`](frontend/src/app/pages/dashboards/finance/components/FinanceKpiStrip.jsx) — Penambahan metrik ke-5 pada KPI Strip: Tagihan Diterbitkan bulan berjalan & % Collection Rate.
    - [`frontend/src/app/pages/dashboards/finance/components/BillingOverviewCard.jsx`](frontend/src/app/pages/dashboards/finance/components/BillingOverviewCard.jsx) [NEW] — Ringkasan total tagihan, progress bar efektivitas penagihan, tagihan lunas, sisa tagihan, dan piutang jatuh tempo.
    - [`frontend/src/app/pages/dashboards/finance/components/BillingAnalyticsCard.jsx`](frontend/src/app/pages/dashboards/finance/components/BillingAnalyticsCard.jsx) [NEW] — Tab interaktif komposisi penerima (Retail, Bisnis, Mitra, Umum) dan tren terbit vs tertagih 6 bulan ApexCharts.

  - **12. Internasionalisasi (i18n)**:
    - [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) — Penambahan lebih dari 1.300 baris kunci terjemahan mencakup seluruh teks UI pada seluruh dashboard tanpa string default hardcoded di kode komponen.

---

## 🌿 Branch: `master` — Issue #294: Finance Cash Account Ownership, Cashier Payment Multi-Account & Transfer Approval Workflow

### 📌 Informasi Issue

- **Nomor Issue**: #294
- **Judul Issue**: Finance: Rekening Kas Owner/Admin, Kasir Multi-Akun Kas Pembayaran, dan Alur Persetujuan Mutasi Saldo Kas/Bank (Finance Cash Account Ownership & Transfer Approval Workflow)
- **Status Branch**: `Sudah di-merge` (Commit `077e562` & `efae30f` di `origin/master`)

### 📅 Rincian Commit

#### `[077e562]` & `[efae30f]` - resolve #294 - 12 September 2026

- **Komponen yang Berubah**:
  - **Backend Core**:
    - [`backend/src/models/financeAccount.model.js`](backend/src/models/financeAccount.model.js) — Penambahan relasi kepemilikan admin (`owner_admin`), penanda tipe kepemilikan, dan flag penguncian akun kas.
    - [`backend/src/controllers/financeAccount.controller.js`](backend/src/controllers/financeAccount.controller.js) & [`backend/src/services/financeAccount.service.js`](backend/src/services/financeAccount.service.js) — Logika filtering akun kas berdasarkan hak kepemilikan admin kasir / finance officer.
    - [`backend/src/models/financeTransfer.model.js`](backend/src/models/financeTransfer.model.js) — Penambahan state approval mutasi saldo kas/bank: `status` (`pending`, `approved`, `rejected`), `approved_by`, `approved_at`, dan `rejection_reason`.
    - [`backend/src/routes/financeTransfer.route.js`](backend/src/routes/financeTransfer.route.js), [`backend/src/controllers/financeTransfer.controller.js`](backend/src/controllers/financeTransfer.controller.js), & [`backend/src/services/financeTransfer.service.js`](backend/src/services/financeTransfer.service.js) — Implementasi alur persetujuan mutasi saldo transfer internal kas/bank (`approveTransfer` dan `rejectTransfer`) dengan transaksi atomik Mongoose, update saldo rekening asal dan tujuan, serta audit trail.
    - [`backend/src/routes/financePayment.route.js`](backend/src/routes/financePayment.route.js), [`backend/src/models/financePayment.model.js`](backend/src/models/financePayment.model.js), & [`backend/src/services/financePayment.service.js`](backend/src/services/financePayment.service.js) — Dukungan pemilihan akun kas spesifik kasir saat pencatatan pelunasan faktur/tagihan pelanggan secara tunai maupun transfer.
    - [`backend/src/utils/finance-error.js`](backend/src/utils/finance-error.js) [NEW] — Standardisasi class error dan kode status HTTP finansial.
    - [`backend/src/locales/id/translation.json`](backend/src/locales/id/translation.json) & [`backend/src/locales/en/translation.json`](backend/src/locales/en/translation.json) — Kunci terjemahan respon backend untuk approval transfer dan rekening kasir.
  - **Automated Integration Tests**:
    - [`backend/test/integration/financeAccount.ownerAdmin.test.js`](backend/test/integration/financeAccount.ownerAdmin.test.js) [NEW] — Suite uji integritas kepemilikan akun admin kasir.
    - [`backend/test/integration/financePayment.cashAccount.test.js`](backend/test/integration/financePayment.cashAccount.test.js) [NEW] — Suite uji pencatatan pembayaran faktur ke akun kas spesifik.
    - [`backend/test/integration/financeTransfer.approval.test.js`](backend/test/integration/financeTransfer.approval.test.js) [NEW] — Suite uji alur persetujuan transfer mutasi saldo, penolakan, dan verifikasi mutasi saldo rekening.
  - **Frontend UI**:
    - [`frontend/src/app/pages/finance/accounts/AccountDrawer.jsx`](frontend/src/app/pages/finance/accounts/AccountDrawer.jsx), [`schema/accountSchema.js`](frontend/src/app/pages/finance/accounts/schema/accountSchema.js), & [`schema/columns.jsx`](frontend/src/app/pages/finance/accounts/schema/columns.jsx) — Form dan kolom penugasan admin pemilik akun kas.
    - [`frontend/src/app/pages/finance/invoices/PaymentDrawer.jsx`](frontend/src/app/pages/finance/invoices/PaymentDrawer.jsx) & [`BulkPaymentDrawer.jsx`](frontend/src/app/pages/finance/invoices/BulkPaymentDrawer.jsx) — Seleksi akun kas tujuan pelunasan tagihan.
    - [`frontend/src/app/pages/finance/transfers/index.jsx`](frontend/src/app/pages/finance/transfers/index.jsx), [`detail.jsx`](frontend/src/app/pages/finance/transfers/detail.jsx), & [`schema/columns.jsx`](frontend/src/app/pages/finance/transfers/schema/columns.jsx) — Antarmuka daftar mutasi kas dengan tombol approval, modal penolakan, dan badge status persetujuan.
    - [`frontend/src/components/shared/table/rows.jsx`](frontend/src/components/shared/table/rows.jsx) & [`status.js`](frontend/src/components/shared/table/status.js) — Komponen badge status transfer approval (`Pending`, `Approved`, `Rejected`).

---

## 🌿 Branch: `master` — Issue #293: Standardisasi Komponen Tabel & Format Kolom

### 📌 Informasi Issue

- **Nomor Issue**: #293
- **Judul Issue**: Standardisasi Format Komponen Tabel, Status Badges, dan Kolom Data Relasional Layanan Broadband & Penagihan
- **Status Branch**: `Sudah di-merge` (Commit `27440df` & `131f6ea` di `origin/master`)

### 📅 Rincian Commit

#### `[27440df]` & `[131f6ea]` - resolve #293 - 12 September 2026

- **Komponen yang Berubah**:
  - [`frontend/src/components/shared/table/Table.jsx`](frontend/src/components/shared/table/Table.jsx) & [`rows.jsx`](frontend/src/components/shared/table/rows.jsx) — Peningkatan cell helper dan wrapper status badge standar TanStack Table.
  - [`frontend/src/app/pages/services/broadband/schema/columns.jsx`](frontend/src/app/pages/services/broadband/schema/columns.jsx) & [`columnsLogs.jsx`](frontend/src/app/pages/services/broadband/schema/columnsLogs.jsx) — Penyelarasan kolom status pelanggan aktif, alamat IP, MAC address, dan log riwayat akun broadband.
  - Penyesuaian kolom pada modul invoices, payments, radius session, tiket dismantle, dan riwayat gudang.

---

## 🌿 Branch: `master` — Dokumentasi & Changelog

### 📌 Informasi Issue

- **Nomor Issue**: Dokumentasi Rilis
- **Judul Issue**: Pembaruan Berkas Changelog Rilis dan README.md Repositori
- **Status Branch**: `Sudah di-merge` (Commit `1c207e3` di `origin/master`)

### 📅 Rincian Commit

#### `[1c207e3]` - update changelog - 12 September 2026

- **Komponen yang Berubah**:
  - [`README.md`](README.md) — Pembaruan deskripsi arsitektur monorepo dan fitur modul rilis terkini.
  - [`backend/src/data/changelog/index.json`](backend/src/data/changelog/index.json) & berkas rilis `issue-244.json` s/d `issue-291.json` — Dokumentasi log rilis berkala.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| :--- | :--- | :--- |
| **#289** | Domain-Specific Operational Dashboards V2 | 7 Modul dashboard dirombak total menjadi pusat kendali operasional simetris berbasis Tailux, landing page universal bebas 403 & non-sensitif, penambahan analitik tagihan finance, serta penambahan dashboard Gudang & WhatsApp. |
| **#294** | Finance Cash Account & Transfer Approval | Manajemen kepemilikan akun kas per staf admin, seleksi rekening kas saat pelunasan tagihan, serta alur persetujuan (*approval workflow*) mutasi saldo transfer internal kas/bank yang aman dengan transaksi atomik. |
| **#293** | Standardisasi Komponen Tabel & Schema Kolom | Konsistensi format visual sel tabel, penanganan badge status baku, dan penyelarasan kolom pada tabel data broadband dan invoice. |
| **DOC** | Update Changelog & Dokumentasi Monorepo | Pencatatan rekam jejak rilis modul sistem dan pembaruan arsitektur pada panduan repositori. |

---

### Kemampuan Baru Pengguna/Admin

- **Seluruh Staf (Universal Access):** Dapat membuka halaman utama `/dashboards` secara instan tanpa hambatan error 403 Forbidden dan memantau kesehatan sistem, performa perangkat, tiket gangguan, serta mengakses modul yang diizinkan melalui Department Hub yang cerdas.
- **Pimpinan & Sales Officer:** Dapat memantau funnel konversi penjualan dari leads hingga terpasang, mendeteksi prospek stagnan >14 hari, menganalisis paket internet terlaris, dan melihat peringkat kinerja sales per wilayah.
- **Tim NOC & Helpdesk:** Dapat memantau SLA kepatuhan 7 hari secara real-time, mendeteksi tiket overdue, mengevaluasi kecepatan rata-rata MTTR, dan memantau beban kerja serta absensi tim teknisi lapangan.
- **Tim Logistik & Gudang:** Memiliki dashboard dedicated untuk memantau 3.200+ unit aset fisik, mendeteksi barang kritis di bawah batas minimum (*low stock*), dan melacak alur permintaan material teknisi.
- **Tim Customer Service:** Memiliki dashboard khusus untuk memantau koneksi akun WhatsApp Meta resmi & Baileys, memantau antrean pesan belum dibalas, dan mengukur kecepatan respon CS.
- **Finance Officer / Manager:** Dapat menyetujui atau menolak permohonan mutasi transfer dana internal kas/bank, mengelola akun kasir per admin, dan menganalisis efektivitas penagihan piutang (*collection rate*) serta komposisi kategori pelanggan.

---

### Bug Fix / Solusi Masalah

- **Penghapusan Kerentanan Data Finansial pada Landing Page:** Menghilangkan seluruh data keuangan sensitif (MRR, kas, piutang) dari halaman utama `/dashboards` sehingga staf teknisi, CS, dan gudang dapat mengakses landing page tanpa melihat angka keuangan perusahaan dan tanpa memicu error 403.
- **Mongoose MissingSchemaError Gudang:** Mengatasi kegagalan populate log mutasi gudang pada `warehouseType.service.js` dengan menyertakan side-effect import model `WarehouseItem` dan `Admin`.
- **Pencegahan Hanging Socket pada Microservices:** Menambahkan `AbortSignal.timeout(5000)` pada seluruh modul `logger.js` di microservice `baileys-api`, `network-monitor`, `telegram-api`, dan `whatsapp-api` sehingga kegagalan fetch error tidak membekukan worker service.
- **Penghapusan Slot Asimetris Layout:** Mengeliminasi kartu ganjil yang menyisakan ruang kosong pada dashboard Connectivity dan Problem dengan penambahan kartu sintesis kesehatan `NetworkAlertSummaryCard` dan penataan grid 2-kolom seimbang.

---

### Menu/Fitur Baru

- **Dashboard Gudang & Logistik:** `/dashboards/warehouse` (Hak akses: `warehouseType.list` atau `warehouseItem.list`).
- **Dashboard WhatsApp & Layanan Pesan:** `/dashboards/whatsapp` (Hak akses: `whatsappChat.list`).
- **Billing & Collection Analytics:** Terintegrasi di `/dashboards/finance` untuk analisis efektivitas penagihan faktur.
- **Approval Mutasi Kas/Bank:** Terintegrasi pada halaman `/finance/transfers` dan `/finance/transfers/:id`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penjelasan Fitur: Portal Pendaratan Utama (Home Operational Dashboard)
Dashboard utama `/dashboards` dirancang sebagai ruang kendali bersama bagi seluruh staf operasional ISP. Halaman ini menyajikan wawasan 360° operasional secara netral tanpa menampilkan data nominal finansial, dilengkapi penanda kesehatan koneksi microservice, antrean tiket gangguan kritis yang membutuhkan eskalasi, dan navigasi modular berbasis hak akses staf.

### 2. Langkah Penggunaan (Tutorial):
1. **Masuk ke Aplikasi:** Login menggunakan akun staf apa pun (Teknisi, NOC, CS, Gudang, atau Billing).
2. **Buka Dashboard Utama:** Sistem akan langsung mengarahkan Anda ke portal pendaratan utama `/dashboards` (atau klik menu **Home** di navigasi sidebar).
3. **Periksa Status Sistem:**
   - Amati hero banner di bagian atas; badge hijau berdenyut menandakan seluruh layanan backend dan microservices berjalan normal.
   - Periksa kartu **Status Sistem & Jaringan** untuk melihat ketersediaan koneksi database, antrean Redis, RADIUS, WhatsApp, dan Network Monitor.
4. **Pantau Antrean Operasional:**
   - Periksa kartu **Alur Penanganan Masalah & Tiket** untuk melihat apakah terdapat tiket gangguan terbuka yang berstatus *overdue* (> 7 hari).
   - Klik langsung tombol pintasan tiket untuk menuju ke antrean pengerjaan teknisi.
5. **Navigasi ke Modul Divisi:**
   - Gulir ke bagian **Hub Operasional Divisi**.
   - Setiap kartu divisi menampilkan status hak akses akun Anda (`Tersedia` atau `Akses Terbatas`).
   - Klik kartu divisi yang berstatus tersedia untuk langsung berpindah ke dashboard spesifik departemen terkait.
