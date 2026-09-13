# 📝 Daily Work Report - Dedy S.N Putra (2026-09-13)

---

## 📅 Laporan Harian - 13 September 2026

---

## 🌿 Branch: `issue-289` — Redesign & Implementasi Dashboard Domain Terpadu: Executive/Home, Connectivity, Problem/Tiket, Finance & Billing, Sales & Prospek, Hotspot, Warehouse, dan WhatsApp (Domain-Specific Operational Dashboards V2)

### 📌 Informasi Issue

- **Nomor Issue**: #289
- **Judul Issue**: Redesign & Implementasi Dashboard Domain Terpadu: Executive Overview, Connectivity & Network Health, Problem & NOC Tiket, Finance & Billing Analytics, Sales & Funnel Pipeline, Hotspot Voucher Analytics, Warehouse & Logistik, serta WhatsApp Customer Service (Domain-Specific Operational Dashboards V2)
- **Status Branch**: `Belum di-merge` (Branch aktif dalam pengembangan di lokal `issue-289`)

---

### 📅 Rincian Perubahan (Work in Progress & Uncommitted Files)

#### `[WIP - Active Working Tree]` - Finalisasi & Penuntasan Overhaul Seluruh 9 Dashboard ke Standar Estetika Premium Tailux - 13 September 2026

- **Komponen yang Berubah**:
  - **1. Frontend — Finalisasi Dashboard WhatsApp & Layanan Pesan (`/dashboards/whatsapp`) ke Standar Tailux**:
    - [`frontend/src/app/pages/dashboards/whatsapp/index.jsx`](frontend/src/app/pages/dashboards/whatsapp/index.jsx) — Penataan tata letak simetris 12-kolom, integrasi tombol perbarui dengan animasi spin, pulsing beacon live realtime, dan penanganan status error/loading terpadu.
    - [`frontend/src/app/pages/dashboards/whatsapp/components/WhatsappKpiStrip.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/WhatsappKpiStrip.jsx) [NEW] — 5 Kartu metrik eksekutif berarsitektur Tailux (`xl:grid-cols-5`):
      - **Koneksi Akun WhatsApp** (`this:success` / `this:error`): Rasio akun terhubung vs total akun dengan badge status online.
      - **Pesan Menunggu Balasan** (`this:error` / `this:success`): Jumlah antrean pesan masuk pelanggan yang belum direspon CS dengan badge evaluasi.
      - **Rata-rata Waktu Respon** (`this:info`): Format dinamis durasi penanganan pesan (menit/jam) dengan badge kecepatan (*Cepat*, *Standar*, *Lambat*).
      - **Total Sesi Obrolan** (`this:primary`): Akumulasi percakapan multi-channel dengan subteks komparasi Meta Cloud API resmi vs Baileys.
      - **Efektivitas Siaran Pesan** (`this:warning`): Total pesan siaran terkirim dengan badge persentase *Delivery Rate*.
      - Dilengkapi squircle avatar (`mask is-squircle rounded-none shadow-xs`), elevasi mikro hover, dan border berbayang `shadow-2xs`.
    - [`frontend/src/app/pages/dashboards/whatsapp/components/ConnectionStatusCard.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/ConnectionStatusCard.jsx) [NEW] (7 Kolom):
      - Header dengan squircle avatar `SignalIcon`, judul formal, dan tombol link sirkular Tailux ke `/customer-service/whatsapp-accounts`.
      - Kartu status akun WhatsApp resmi Meta Cloud API dengan penanda verifikasi dan status terhubung.
      - Daftar kartu akun WhatsApp Web (Baileys) dengan timestamp aktif terformat i18n dan badge semantik (`connected`, `connecting`, `pending_qr`, dll.).
      - Segmented bar distribusi proporsi volume percakapan Meta vs Baileys.
    - [`frontend/src/app/pages/dashboards/whatsapp/components/ResponseTimeCard.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/ResponseTimeCard.jsx) [NEW] (5 Kolom):
      - Header dengan squircle avatar `ClockIcon` dan tombol link sirkular ke `/customer-service/whatsapp-chat`.
      - Kartu hero kecepatan rata-rata penanganan pesan CS dengan angka besar dan badge evaluasi kecepatan.
      - Alert box antrean pesan belum dibalas berlatar lembut dinamis (aksen rose jika terdapat antrean, emerald jika kotak masuk bersih).
      - Tombol aksi cepat Tailux ke ruang obrolan CS.
    - [`frontend/src/app/pages/dashboards/whatsapp/components/MessageVolumeCard.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/MessageVolumeCard.jsx) [NEW] (6 Kolom):
      - Donut chart ApexCharts modern komparasi Pesan Masuk (*Inbound* dari pelanggan) vs Pesan Keluar (*Outbound* dari admin).
      - Total pesan di tengah donut dan kartu ringkasan volume pesan dengan rasio persentase terformat i18n.
    - [`frontend/src/app/pages/dashboards/whatsapp/components/BroadcastReminderCard.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/BroadcastReminderCard.jsx) [NEW] (6 Kolom):
      - Monitoring jadwal pengingat tagihan bulanan otomatis (H-N jatuh tempo) dengan status Terkirim, Menunggu, dan Gagal.
      - Progress bar tingkat keberhasilan pengiriman (*Delivery Rate*) dan grid 3 kartu metrik siaran dengan soft badge Tailux.
      - Riwayat 3 kampanye siaran terkini dengan status badge dinamis.
    - [`frontend/src/app/pages/dashboards/whatsapp/components/WhatsappQuickActions.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/WhatsappQuickActions.jsx) [NEW] — 5 Pintasan cepat: Obrolan CS (Live Chat), Siaran Pesan (Broadcast), Akun WhatsApp & QR, Template Balasan Cepat, Riwayat Percakapan dengan tombol baku `Button component={Link} variant="flat" color="neutral"`, squircle avatar solid tematik, animasi hover slide chevron, dan proteksi hak akses `useHasPrivilege`.
    - [`frontend/src/app/pages/dashboards/whatsapp/components/WhatsappDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/whatsapp/components/WhatsappDashboardSkeleton.jsx) [NEW] — Placeholder shimmer loader presisi diselaraskan dengan geometri kartu KPI dan squircle avatar.

  - **2. Frontend — Finalisasi Dashboard Gudang & Logistik (`/dashboards/warehouse`) ke Standar Tailux**:
    - [`frontend/src/app/pages/dashboards/warehouse/index.jsx`](frontend/src/app/pages/dashboards/warehouse/index.jsx) — Portal kendali logistik terpadu dengan pulsing beacon live, tombol perbarui beranimasi putar, dan tata letak simetris 12-kolom.
    - [`frontend/src/app/pages/dashboards/warehouse/components/WarehouseKpiStrip.jsx`](frontend/src/app/pages/dashboards/warehouse/components/WarehouseKpiStrip.jsx) [NEW] — 5 Kartu metrik eksekutif (`xl:grid-cols-5`):
      - **Katalog Varian Barang** (`this:primary`): Total varian tipe barang aktif terdaftar dengan badge status katalog.
      - **Unit Fisik & Material** (`this:info`): 3.200+ unit aset tercatat di gudang dan lapangan dengan badge aset fisik.
      - **Stok Siap Pakai** (`this:success`): Total unit dan meter material siap deploy dengan badge stok sehat.
      - **Peringatan Stok Kritis** (`this:error` / `this:success`): Hitungan jenis barang di bawah ambang minimum dengan badge restock.
      - **Permintaan Menunggu** (`this:warning` / `this:neutral`): Jumlah tiket permohonan material teknisi yang butuh alokasi.
    - [`frontend/src/app/pages/dashboards/warehouse/components/AssetDistributionCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/AssetDistributionCard.jsx) [NEW] (7 Kolom):
      - Donut chart ApexCharts modern menampilkan status keberadaan aset: Stok Siap Pakai, Di Pelanggan/Lapangan, dan Cadangan/Perbaikan.
      - Progress bar ketersediaan stok 5 kelompok material terbesar (Fiber Dropcore, Router OLT, Patch Cord, dll.).
    - [`frontend/src/app/pages/dashboards/warehouse/components/LowStockCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/LowStockCard.jsx) [NEW] (5 Kolom):
      - Visualisasi progress bar per item untuk jenis barang yang berada di bawah `min_stock` dengan indikator warna semantik merah (stok habis) atau amber (menipis).
      - State kosong ramah (*All Stocks Healthy*) jika seluruh stok berada dalam batas aman.
    - [`frontend/src/app/pages/dashboards/warehouse/components/CategoryBreakdownCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/CategoryBreakdownCard.jsx) [NEW] (4 Kolom):
      - Komposisi 7 kategori inventaris teratas dengan progress bar horizontal terhadap kelompok produk terbesar.
    - [`frontend/src/app/pages/dashboards/warehouse/components/RequestStatusCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/RequestStatusCard.jsx) [NEW] (4 Kolom):
      - Pipeline 4 status permohonan pengambilan barang: Menunggu, Siap Diambil, Selesai, Dibatalkan dengan chip berlatar lembut.
      - Progress bar **Tingkat Pemenuhan Permintaan (Fulfillment Rate)**.
    - [`frontend/src/app/pages/dashboards/warehouse/components/RecentActivityCard.jsx`](frontend/src/app/pages/dashboards/warehouse/components/RecentActivityCard.jsx) [NEW] (4 Kolom):
      - Riwayat 6 mutasi dan pergerakan unit terkini lengkap dengan squircle avatar arah mutasi, info admin, dan timestamp terformat.
    - [`frontend/src/app/pages/dashboards/warehouse/components/WarehouseQuickActions.jsx`](frontend/src/app/pages/dashboards/warehouse/components/WarehouseQuickActions.jsx) [NEW] — 6 Pintasan navigasi terproteksi hak akses pengguna (`useHasPrivilege`): Katalog Barang, Unit Fisik & SN, Mutasi Antar Lokasi, Permintaan Barang, Lokasi Gudang, dan Laporan Cepat.
    - [`frontend/src/app/pages/dashboards/warehouse/components/WarehouseDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/warehouse/components/WarehouseDashboardSkeleton.jsx) [NEW] — Placeholder shimmer state pemuatan data awal.

  - **3. Frontend — Finalisasi Dashboard Hotspot & Voucher (`/dashboards/hotspot`) ke Standar Tailux**:
    - [`frontend/src/app/pages/dashboards/hotspot/index.jsx`](frontend/src/app/pages/dashboards/hotspot/index.jsx) — Pusat kendali analitik operasional voucher hotspot dengan tata letak simetris 12-kolom dan pulsing live beacon.
    - [`frontend/src/app/pages/dashboards/hotspot/components/HotspotKpiStrip.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotKpiStrip.jsx) [NEW] — 5 Kartu metrik eksekutif (`xl:grid-cols-5`):
      - **Penjualan Hari Ini** (`this:success`): Omzet terformat rupiah + kuantitas voucher terjual & badge delta harian vs kemarin.
      - **Voucher Terjual & Aktif** (`this:primary`): Jumlah voucher beredar aktif dengan badge status penggunaan.
      - **Pengguna Online Real-time** (`this:info`): Sesi aktif saat ini dengan indikator denyut hijau (*Live Sessions*).
      - **Stok Siap Jual** (`this:warning`): Total voucher siap edar dengan badge kesehatan stok (*Stock Healthy* vs *Stock Low*).
      - **Kedaluwarsa / Isolir** (`this:error`): Total voucher habis masa aktif dan terisolir dengan badge isolir.
    - [`frontend/src/app/pages/dashboards/hotspot/components/HotspotSalesTrendCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotSalesTrendCard.jsx) [NEW] (8 Kolom):
      - Area Chart ApexCharts interaktif dengan kurva *smooth*, gradien lembut, dan dark mode tooltip terformat.
      - Tombol toggle pil (*pill buttons*) Tailux untuk metrik (**Nominal Rp** vs **Volume Pcs**) dan periode (**7 Hari Terakhir** vs **Bulanan 6 Bln**).
    - [`frontend/src/app/pages/dashboards/hotspot/components/VoucherStatusCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/VoucherStatusCard.jsx) [NEW] (4 Kolom):
      - Donut chart ApexCharts modern dengan total inventaris di tengah dan warna semantik adaptif (Tersedia, Terjual/Aktif, Kedaluwarsa, Terisolir).
      - Callout status ketersediaan stok siap jual.
    - [`frontend/src/app/pages/dashboards/hotspot/components/ProfileDistributionCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/ProfileDistributionCard.jsx) [NEW] (5 Kolom):
      - Daftar paket kecepatan hotspot terpopuler dengan progress bar dinamis Tailux, kuantitas voucher, dan badge omzet.
    - [`frontend/src/app/pages/dashboards/hotspot/components/HotspotSessionsCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotSessionsCard.jsx) [NEW] (7 Kolom):
      - Monitoring sesi pengguna hotspot (Username, Profil, IP/NAS, Lokasi, Status Online / Selesai) menggunakan komponen tabel terstandarisasi (`Table`, `THead`, `TBody`, `Tr`, `Th`, `Td`) tanpa elemen HTML mentah.
    - [`frontend/src/app/pages/dashboards/hotspot/components/TopLocationsCard.jsx`](frontend/src/app/pages/dashboards/hotspot/components/TopLocationsCard.jsx) [NEW] — Tab interaktif beralih antara **Peringkat Lokasi Hotspot Teratas** dan **Distribusi Kelompok Batch Voucher**.
    - [`frontend/src/app/pages/dashboards/hotspot/components/HotspotQuickActions.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotQuickActions.jsx) [NEW] & [`HotspotDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/hotspot/components/HotspotDashboardSkeleton.jsx) [NEW] — 5 Pintasan navigasi cepat berpelindung `useHasPrivilege` dan shimmer skeleton loader.

  - **4. Frontend — Finalisasi Dashboard Autentikasi RADIUS (`/dashboards/radius`) ke Standar Tailux**:
    - [`frontend/src/app/pages/dashboards/radius/index.jsx`](frontend/src/app/pages/dashboards/radius/index.jsx) — Integrasi WebSocket live streaming (`useRadiusStream`), denyut hijau pulsing beacon ke daemon Go `radiusd`, tombol muat ulang, dan status sinkronisasi.
    - [`frontend/src/app/pages/dashboards/radius/components/StatsRow.jsx`](frontend/src/app/pages/dashboards/radius/components/StatsRow.jsx) — 4 Kartu metrik eksekutif Tailux: Sesi Aktif PPPoE/Hotspot, Server Terhubung, Autentikasi Berhasil 12 Jam, Autentikasi Gagal 12 Jam dengan squircle avatar dan micro-elevation.
    - [`frontend/src/app/pages/dashboards/radius/components/EventsChart.jsx`](frontend/src/app/pages/dashboards/radius/components/EventsChart.jsx) — Area Chart ApexCharts tren autentikasi 12 jam dengan kurva halus dan badge komparasi total berhasil vs gagal.
    - [`frontend/src/app/pages/dashboards/radius/components/ServerList.jsx`](frontend/src/app/pages/dashboards/radius/components/ServerList.jsx) — Monitoring node server Go RADIUS daemon, IP peer, durasi koneksi gRPC, chip sesi aktif, memori goroutines, serta tombol Restart berpelindung `ConfirmModal` dan `radiusControl.update`.
    - [`frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx`](frontend/src/app/pages/dashboards/radius/components/LiveLogPanel.jsx) (8 Kolom) — Terminal log streaming realtime, kontrol play/pause/clear, filter kategori event (`auth-success`, `auth-failed`, `session`, `system`), dan auto-scroll console berestetika gelap.
    - [`frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx`](frontend/src/app/pages/dashboards/radius/components/UserActivityPanel.jsx) (4 Kolom) — Tab aktivitas Semua / Berhasil / Gagal dengan tabel terstandarisasi Tailux dan link username berpelindung privilege (`broadband.read`, `customer.read`).
    - [`frontend/src/app/pages/dashboards/radius/components/ActiveSessionsTable.jsx`](frontend/src/app/pages/dashboards/radius/components/ActiveSessionsTable.jsx), [`RadiusQuickActions.jsx`](frontend/src/app/pages/dashboards/radius/components/RadiusQuickActions.jsx) [NEW], & [`RadiusDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/radius/components/RadiusDashboardSkeleton.jsx) [NEW].

  - **5. Frontend — Finalisasi Dashboard Konektivitas & Jaringan (`/dashboards/connectivity`) ke Standar Tailux**:
    - [`frontend/src/app/pages/dashboards/connectivity/index.jsx`](frontend/src/app/pages/dashboards/connectivity/index.jsx) — Tata letak simetris 2-kolom (`xl:grid-cols-2`, `items-stretch`) seimbang tanpa ruang kosong asimetris.
    - [`frontend/src/app/pages/dashboards/connectivity/components/ConnectivityKpiStrip.jsx`](frontend/src/app/pages/dashboards/connectivity/components/ConnectivityKpiStrip.jsx) [NEW] — 4 Metrik: Perangkat Jaringan Aktif, Throughput Trafik Agregat, Stabilitas Latency & Loss, Utilisasi Pool IPv4.
    - [`frontend/src/app/pages/dashboards/connectivity/components/DeviceHealthCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/DeviceHealthCard.jsx) [NEW] & [`OltHealthCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/OltHealthCard.jsx) [NEW] — Donut chart kesehatan perangkat dan status kesiapan Optical Line Terminal (GPON OLT).
    - [`frontend/src/app/pages/dashboards/connectivity/components/TrafficThroughputCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/TrafficThroughputCard.jsx) [NEW] & [`LatencyStatusCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/LatencyStatusCard.jsx) [NEW] — Aliran Inbound vs Outbound dinamis dan target probe latensi.
    - [`frontend/src/app/pages/dashboards/connectivity/components/IPv4PoolCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/IPv4PoolCard.jsx) [NEW] & [`NetworkAlertSummaryCard.jsx`](frontend/src/app/pages/dashboards/connectivity/components/NetworkAlertSummaryCard.jsx) [NEW] — Progress alokasi pool IP publik/privat dan sintesis peringatan kesehatan jaringan operasional.
    - [`frontend/src/app/pages/dashboards/connectivity/components/ConnectivityQuickActions.jsx`](frontend/src/app/pages/dashboards/connectivity/components/ConnectivityQuickActions.jsx) [NEW] & [`ConnectivityDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/connectivity/components/ConnectivityDashboardSkeleton.jsx) [NEW].

  - **6. Frontend — Finalisasi Dashboard Gangguan & Helpdesk Tiket (`/dashboards/problem`) ke Standar Tailux**:
    - [`frontend/src/app/pages/dashboards/problem/index.jsx`](frontend/src/app/pages/dashboards/problem/index.jsx) — Tata letak simetris 2-kolom (`xl:grid-cols-2`, `items-stretch`) dengan live beacon timestamp.
    - [`frontend/src/app/pages/dashboards/problem/components/ProblemKpiStrip.jsx`](frontend/src/app/pages/dashboards/problem/components/ProblemKpiStrip.jsx) [NEW] — 4 Metrik: Tiket Aktif Berjalan, Tingkat Penyelesaian %, Durasi Rata-rata MTTR, Work Order Lapangan.
    - [`frontend/src/app/pages/dashboards/problem/components/PipelineCard.jsx`](frontend/src/app/pages/dashboards/problem/components/PipelineCard.jsx) [NEW] & [`CategoryChart.jsx`](frontend/src/app/pages/dashboards/problem/components/CategoryChart.jsx) [NEW] — 4 Tahapan pipeline tiket dengan alert overdue >7 hari dan grafik horizontal ApexCharts kategori kendala.
    - [`frontend/src/app/pages/dashboards/problem/components/ResolutionTimeCard.jsx`](frontend/src/app/pages/dashboards/problem/components/ResolutionTimeCard.jsx) [NEW] & [`WorkOrderCard.jsx`](frontend/src/app/pages/dashboards/problem/components/WorkOrderCard.jsx) [NEW] — Evaluasi MTTR per tipe jaringan vs progress status Surat Perintah Kerja teknisi.
    - [`frontend/src/app/pages/dashboards/problem/components/TechnicianWorkloadCard.jsx`](frontend/src/app/pages/dashboards/problem/components/TechnicianWorkloadCard.jsx) [NEW] & [`IncidentSeverityCard.jsx`](frontend/src/app/pages/dashboards/problem/components/IncidentSeverityCard.jsx) [NEW] — Leaderboard teknisi lapangan dengan squircle avatar, beban kerja, dan status presensi harian vs kepatuhan SLA 7 hari dan tiket overdue.
    - [`frontend/src/app/pages/dashboards/problem/components/ProblemQuickActions.jsx`](frontend/src/app/pages/dashboards/problem/components/ProblemQuickActions.jsx) [NEW] & [`ProblemDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/problem/components/ProblemDashboardSkeleton.jsx) [NEW].

  - **7. Frontend — Finalisasi Dashboard Keuangan & Analisis Tagihan (`/dashboards/finance`) ke Standar Tailux**:
    - [`frontend/src/app/pages/dashboards/finance/index.jsx`](frontend/src/app/pages/dashboards/finance/index.jsx) — Grid simetris 2-kolom komprehensif.
    - [`frontend/src/app/pages/dashboards/finance/components/FinanceKpiStrip.jsx`](frontend/src/app/pages/dashboards/finance/components/FinanceKpiStrip.jsx) — 5 Metrik eksekutif (`xl:grid-cols-5`): Total Kas & Bank, Laba Bersih Berjalan, Tagihan Diterbitkan & % Collection Rate, Piutang Usaha (AR), Gateway Belum Terbuku.
    - [`frontend/src/app/pages/dashboards/finance/components/IncomeStatementCard.jsx`](frontend/src/app/pages/dashboards/finance/components/IncomeStatementCard.jsx) & [`CashPositionCard.jsx`](frontend/src/app/pages/dashboards/finance/components/CashPositionCard.jsx) — Marjin laba-rugi vs arus mutasi kas masuk/keluar.
    - [`frontend/src/app/pages/dashboards/finance/components/BillingOverviewCard.jsx`](frontend/src/app/pages/dashboards/finance/components/BillingOverviewCard.jsx) [NEW] & [`BillingAnalyticsCard.jsx`](frontend/src/app/pages/dashboards/finance/components/BillingAnalyticsCard.jsx) [NEW] — Realisasi tagihan diterbitkan, progress bar efektivitas penagihan, komposisi penerima (Retail, Bisnis, Mitra, Umum), dan tren komparatif 6 bulan ApexCharts.
    - [`frontend/src/app/pages/dashboards/finance/components/ReceivableAgingCard.jsx`](frontend/src/app/pages/dashboards/finance/components/ReceivableAgingCard.jsx) & [`IsolirCard.jsx`](frontend/src/app/pages/dashboards/finance/components/IsolirCard.jsx) — Distribusi umur piutang (0-30, 31-60, 61-90, >90 hari) vs status batch isolir penunggak tagihan.
    - [`frontend/src/app/pages/dashboards/finance/components/GatewayReconciliationCard.jsx`](frontend/src/app/pages/dashboards/finance/components/GatewayReconciliationCard.jsx) & [`RegulatoryObligationCard.jsx`](frontend/src/app/pages/dashboards/finance/components/RegulatoryObligationCard.jsx) — Rekonsiliasi transaksi payment gateway vs kewajiban regulasi telekomunikasi (BHP, USO, PPh Final).
    - [`frontend/src/app/pages/dashboards/finance/components/FinanceQuickActions.jsx`](frontend/src/app/pages/dashboards/finance/components/FinanceQuickActions.jsx) & [`FinanceDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/finance/components/FinanceDashboardSkeleton.jsx).

  - **8. Frontend — Finalisasi Dashboard Penjualan & Prospek (`/dashboards/sales`) ke Standar Tailux**:
    - [`frontend/src/app/pages/dashboards/sales/index.jsx`](frontend/src/app/pages/dashboards/sales/index.jsx) — Tata letak simetris 2-kolom dengan live beacon timestamp.
    - [`frontend/src/app/pages/dashboards/sales/components/SalesKpiStrip.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesKpiStrip.jsx) [NEW] — 4 Metrik: Prospek Masuk, Layanan Broadband Aktif, Win Rate %, Nilai MRR Berjalan dengan squircle avatar Tailux.
    - [`frontend/src/app/pages/dashboards/sales/components/SalesFunnelChart.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesFunnelChart.jsx) [NEW] & [`PipelineStatusCard.jsx`](frontend/src/app/pages/dashboards/sales/components/PipelineStatusCard.jsx) [NEW] — Visualisasi funnel horizontal ApexCharts dan 5 tahapan prospek dengan peringatan deal stagnan >14 hari.
    - [`frontend/src/app/pages/dashboards/sales/components/BroadbandServiceCard.jsx`](frontend/src/app/pages/dashboards/sales/components/BroadbandServiceCard.jsx) [NEW] & [`BroadbandRevenueCard.jsx`](frontend/src/app/pages/dashboards/sales/components/BroadbandRevenueCard.jsx) [NEW] — Status pelanggan online/nonaktif/isolir dan neraca pendapatan MRR/ARPU serta 3 paket terlaris.
    - [`frontend/src/app/pages/dashboards/sales/components/RegionGrowthCard.jsx`](frontend/src/app/pages/dashboards/sales/components/RegionGrowthCard.jsx) [NEW] & [`TopProductsCard.jsx`](frontend/src/app/pages/dashboards/sales/components/TopProductsCard.jsx) [NEW] — Filter wilayah (Area, Provinsi, Kota) dan katalog paket broadband terpopuler.
    - [`frontend/src/app/pages/dashboards/sales/components/SalesPerformanceTable.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesPerformanceTable.jsx) [NEW] & [`LostReasonCard.jsx`](frontend/src/app/pages/dashboards/sales/components/LostReasonCard.jsx) [NEW] — Leaderboard PIC sales dengan medali ranking (Emas, Perak, Perunggu) dan analisis faktor pembatalan prospek.
    - [`frontend/src/app/pages/dashboards/sales/components/SalesQuickActions.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesQuickActions.jsx) [NEW] & [`SalesDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/sales/components/SalesDashboardSkeleton.jsx) [NEW].

  - **9. Frontend — Finalisasi Landing Page Portal Pendaratan Utama (`/dashboards`) ke Standar Tailux**:
    - [`frontend/src/app/pages/dashboards/home/index.jsx`](frontend/src/app/pages/dashboards/home/index.jsx) — Portal pendaratan utama operasional 360°.
    - [`frontend/src/app/pages/dashboards/home/components/HomeDashboardHeader.jsx`](frontend/src/app/pages/dashboards/home/components/HomeDashboardHeader.jsx) [NEW] — Executive Welcome Hero Banner bergradien modern dengan ambient blur, wadah pedestal berkontras tinggi untuk ilustrasi SVG [`dashboard-meet.svg`](frontend/src/assets/illustrations/dashboard-meet.svg), live platform beacon, dan tombol navigasi terintegrasi.
    - [`frontend/src/app/pages/dashboards/home/components/HomeKpiStrip.jsx`](frontend/src/app/pages/dashboards/home/components/HomeKpiStrip.jsx) [NEW] — 5 Metrik operasional universal (*zero sensitive financial data*) aman diakses semua staf tanpa error 403 Forbidden.
    - [`frontend/src/app/pages/dashboards/home/components/DepartmentHubCard.jsx`](frontend/src/app/pages/dashboards/home/components/DepartmentHubCard.jsx) [NEW] — Ubin hub operasional divisi ke seluruh modul dengan indikator hak akses dinamis (`Tersedia` vs `Akses Terbatas`).
    - [`frontend/src/app/pages/dashboards/home/components/SystemHealthCard.jsx`](frontend/src/app/pages/dashboards/home/components/SystemHealthCard.jsx) [NEW] & [`TicketPipelineCard.jsx`](frontend/src/app/pages/dashboards/home/components/TicketPipelineCard.jsx) [NEW] — Monitoring kesehatan 5 microservices, status perangkat UP vs DOWN, dan pipeline tiket dengan peringatan SLA.
    - [`frontend/src/app/pages/dashboards/home/components/HomeQuickActions.jsx`](frontend/src/app/pages/dashboards/home/components/HomeQuickActions.jsx) [NEW] & [`HomeDashboardSkeleton.jsx`](frontend/src/app/pages/dashboards/home/components/HomeDashboardSkeleton.jsx) [NEW].

  - **10. Frontend — Internasionalisasi Komprehensif (i18n)**:
    - [`frontend/src/i18n/locales/id/translations.json`](frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](frontend/src/i18n/locales/en/translations.json) — Penambahan dan pembersihan lebih dari 1.400 baris kunci terjemahan mencakup seluruh teks, label metrik, badge status, dan deskripsi pada seluruh 9 dashboard tanpa ada string default hardcoded di kode komponen.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| :--- | :--- | :--- |
| **#289** | Domain-Specific Operational Dashboards V2 | Seluruh 9 modul dashboard operasional monorepo Dekasimal V2 (Home/Executive, Sales, Finance, Problem, Connectivity, Radius, Hotspot, Warehouse, WhatsApp) telah selesai 100% dirombak ke standar estetika premium Tailux, bebas tag HTML mentah, terlindungi RBAC presisi (`useHasPrivilege`), simetris responsif, dan siap produksi. |

---

### Kemampuan Baru Pengguna/Admin

- **Seluruh Karyawan & Staf (Universal Landing Experience):** Membuka halaman utama `/dashboards` secara instan tanpa hambatan error 403 Forbidden, melihat ringkasan kebugaran sistem 360°, dan melompat ke dashboard departemen yang diizinkan melalui Department Hub cerdas.
- **Tim CS & WhatsApp Support:** Memiliki pusat kendali obrolan WhatsApp multi-channel untuk memantau status koneksi Meta Cloud API & Baileys secara real-time, mendeteksi antrean pesan belum dibalas, mengukur kecepatan respon CS, memantau rasio volume pesan masuk vs keluar, dan mengevaluasi jadwal pengingat tagihan bulanan otomatis.
- **Tim Gudang & Logistik:** Memiliki dashboard mandiri untuk memantau inventaris 3.200+ unit aset fisik di gudang dan lapangan, mendeteksi barang kritis yang berada di bawah batas minimum (*low stock alert*), mengontrol pergerakan mutasi unit terkini, dan melacak persentase pemenuhan permohonan barang teknisi (*fulfillment rate*).
- **Pengelola Hotspot & Voucher:** Menganalisis pendapatan voucher hari ini vs kemarin, memantau live sessions pengguna online real-time, mengontrol siklus 4 status voucher (tersedia, aktif, expired, isolir), mengevaluasi popularitas paket profil kecepatan, dan memantau peringkat titik hotspot terlaris.
- **Administrator Server RADIUS:** Mengawasi live sessions PPPoE & Hotspot secara streaming WebSocket langsung ke daemon Go `radiusd`, menganalisis tren autentikasi 12 jam, membaca console live log berfilter kategori event, dan memantau kesehatan node server serta goroutines Go runtime.
- **Tim NOC & Teknisi Jaringan:** Memantau ketersediaan perangkat jaringan online UP vs DOWN secara simetris, mengevaluasi throughput trafik gabungan Inbound/Outbound, memantau latensi dan packet loss, serta memantau utilisasi pool IPv4 publik/privat.
- **Tim Helpdesk & Lapangan:** Mengawasi pipeline tiket dengan peringatan eskalasi darurat tiket *overdue* (>7 hari), mengevaluasi rata-rata waktu penyelesaian MTTR, memantau progress Surat Perintah Kerja (WO), dan memantau beban kerja serta presensi harian tim teknisi.
- **Finance Officer & Manajer:** Menganalisis kinerja laba-rugi, arus kas likuid, ringkasan efektivitas penagihan piutang (*collection rate %*), komposisi penerima faktur, distribusi umur piutang (AR aging), serta rekonsiliasi gerbang pembayaran dan kewajiban regulasi telekomunikasi.
- **Pimpinan & Tim Sales:** Melacak funnel konversi penjualan secara horizontal, mengidentifikasi deal prospek yang stagnan >14 hari, menganalisis pelanggan broadband per wilayah, dan mengevaluasi klasemen kinerja PIC sales dengan sistem medali.

---

### Bug Fix / Solusi Masalah

- **Penuntasan 100% Standar Komponen & Eliminasi Tag HTML Mentah:** Menghapus seluruh penggunaan elemen HTML native (`<a>`, `<button>`, `<table>`, `<tr>`, `<td>`) pada seluruh modul dashboard, menggantikannya secara konsisten dengan komponen desain sistem terstandarisasi Tailux (`Button component={Link}`, `Card`, `Badge`, `Avatar`, `Table`, `Progress`).
- **Pembersihan String Hardcoded & Kepatuhan i18n:** Menghilangkan seluruh string teks dan badge bahasa Inggris inline (*Online & Ready*, *Attention Needed*, *Fast*, *Stock Healthy*, *Delivered*, dll.) dan menggantinya dengan key terjemahan baku pada `translations.json` tanpa teks default pada pemanggilan `t()`.
- **Penyelarasan Layout Grid Simetris:** Memperbaiki layout asimetris pada dashboard Connectivity, Problem, Hotspot, Warehouse, dan WhatsApp menjadi grid simetris 12-kolom (`xl:grid-cols-2`, `xl:grid-cols-5`, `items-stretch`) yang mengisi ruang vertikal secara merata.
- **Verifikasi Kualitas Kode & Build:** Seluruh rangkaian dashboard berhasil divalidasi dengan ESLint (`0 errors, 0 warnings`) dan lolos build produksi Vite (`npm run build --prefix frontend`) dalam 35.63s.

---

### Menu/Fitur Baru

- **Dashboard WhatsApp & Layanan Pesan:** `/dashboards/whatsapp` (Hak akses: `whatsappChat.list`).
- **Dashboard Gudang & Logistik:** `/dashboards/warehouse` (Hak akses: `warehouseType.list` atau `warehouseItem.list`).
- **Billing & Collection Analytics:** Terintegrasi di `/dashboards/finance` untuk analisis efektivitas penagihan faktur.
- **Live Stream Log Console & Server Node Monitoring:** Terintegrasi di `/dashboards/radius` untuk monitoring daemon Go RADIUS.
- **Department Hub & Executive Welcome Hero:** Terintegrasi di portal utama `/dashboards` sebagai pintu masuk operasional 360°.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penjelasan Fitur: Ekosistem 9 Dashboard Operasional Berstandar Tailux
Seluruh 9 dashboard operasional pada sistem Dekasimal-V2 kini mengadopsi standar visual terpadu berarsitektur **Tailux**:
- **Squircle Avatars & Modern Micro-Elevation:** Penggunaan avatar berbentuk squircle (`mask is-squircle rounded-none shadow-xs`) berpadu dengan efek elevasi hover interaktif (`hover:-translate-y-1 hover:shadow-lg transition-all duration-300`) memberikan kedalaman visual modern.
- **Pulsing Live Beacons & Seamless Refresh:** Setiap halaman dilengkapi penanda denyut hijau (*pulsing beacon* `animate-ping`) yang menandakan aliran data realtime, dipadukan dengan tombol perbarui berbingkai halus dengan animasi putar halus tanpa merusak layout (*zero layout shift*).
- **Proteksi Akses Granular (RBAC):** Seluruh kartu modul, tombol pintasan aksi cepat, dan tautan rincian dilindungi oleh `useHasPrivilege` selaras dengan aturan backend.

---

### 2. Langkah Penggunaan (Tutorial):

1. **Masuk ke Portal Pendaratan Utama (`/dashboards`):**
   - Login ke dalam sistem. Anda akan langsung disambut oleh **Executive Hero Banner** yang menampilkan status operasional platform secara real-time.
   - Periksa kartu **Status Sistem & Jaringan** untuk memastikan seluruh microservices (MongoDB, Redis, RADIUS, WhatsApp, Network Monitor) berjalan normal.
   - Pantau kartu **Alur Penanganan Masalah & Tiket** untuk memastikan tidak ada tiket gangguan kritis yang melewati batas SLA 7 hari (*overdue*).

2. **Berpindah Antar Divisi Menggunakan Department Hub:**
   - Gulir ke bawah menuju seksi **Hub Operasional Divisi** (atau klik tombol *Hub Operasional Divisi* pada Hero Banner).
   - Perhatikan badge pada setiap ubin divisi: badge hijau **Tersedia** menandakan modul dapat diakses sesuai peran akun Anda, sedangkan badge abu-abu **Akses Terbatas** menandakan modul berada di luar wewenang peran akun.
   - Klik kartu divisi yang tersedia untuk langsung berpindah ke dashboard tujuan.

3. **Memantau Layanan Pesan Pelanggan (`/dashboards/whatsapp`):**
   - Buka menu **WhatsApp** di navigasi sidebar atau klik kartu WhatsApp di Hub.
   - Amati kartu **Koneksi Akun WhatsApp**; pastikan nomor Meta Cloud API dan seluruh sesi Baileys berstatus terhubung.
   - Amati kartu **Pesan Menunggu Balasan**; jika badge berwarna merah, segera klik tombol pintasan **Buka Obrolan CS** untuk merespon pesan pelanggan.
   - Evaluasi kartu **Pengingat Tagihan Otomatis** untuk memastikan siaran reminder jatuh tempo berhasil terkirim.

4. **Memantau Logistik & Peringatan Stok Gudang (`/dashboards/warehouse`):**
   - Buka menu **Warehouse** di navigasi sidebar atau klik kartu Gudang di Hub.
   - Amati kartu **Peringatan Stok Kritis**; jika terdeteksi barang di bawah batas minimum, periksa kartu **Barang Mendekati Batas Minimum** untuk melihat material apa yang membutuhkan restock pengadaan.
   - Periksa alur **Permintaan Barang Teknisi** dan persentase *Fulfillment Rate* untuk mempercepat alokasi material instalasi baru.

5. **Memantau Pendapatan & Stok Voucher Hotspot (`/dashboards/hotspot`):**
   - Buka menu **Hotspot** di navigasi sidebar atau klik kartu Hotspot di Hub.
   - Gunakan toggle pada kartu **Tren Penjualan** untuk menganalisis performa berdasarkan nominal rupiah atau volume fisik voucher dalam kurun waktu 7 hari atau 6 bulan.
   - Amati tabel **Monitoring Sesi Pengguna** untuk melihat pengguna yang sedang online secara real-time di berbagai titik lokasi hotspot.

6. **Memantau Streaming Daemon RADIUS (`/dashboards/radius`):**
   - Buka menu **Radius** di navigasi sidebar.
   - Periksa panel **Server Terhubung** untuk melihat kesehatan daemon Go `radiusd` dan jumlah memori goroutines.
   - Gunakan panel **Terminal Live Log** dengan kontrol Pause/Play dan filter kategori event untuk menganalisis kegagalan autentikasi pengguna PPPoE/Hotspot secara instan.
