# 📝 Daily Work Report - Dedy S.N Putra (14 September 2026)

---

## 📅 Laporan Harian - 14 September 2026

---

## 🌿 Branch: `issue-300` — Tambahan Endpoint untuk Metode Pembayaran

### 📌 Informasi Issue

- **Nomor Issue**: #300
- **Judul Issue**: Tambahan Endpoint untuk Metode Pembayaran (Akun Bank & Kas Pembayaran Mitra)
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via PR #302 pada commit `df64528`)

### 📅 Rincian Commit

#### [df64528] - resolve #300 - Senin, 14 September 2026, 19:33:00 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/partnerApiAccount.controller.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/financeAccount.service.js`
  - `backend/test/integration/partnerApiAccount.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Merge branch `issue-300` ke branch `master`.
  - Melakukan audit kode dan remediasi penuh sesuai standar `AGENTS.md` (§2.A Aturan 3, 14, 17):
    - Mencegah *error swallowing* dengan menambahkan objek error `err` dan logging terstruktur Winston (`logger.error(...)`) sebelum melempar error i18n pada fungsi `findListAccountForPartnerApi` di `financeAccount.service.js`.
    - Menerapkan isolasi data dan prinsip minimisasi data: menyaring akun agar mengecualikan akun kas personal milik admin/karyawan (`owner_admin: null`) dan akun sistem payment gateway internal (`is_system: { $ne: true }`), sehingga data operasional internal perusahaan tidak bocor ke pihak mitra eksternal.
    - Menerapkan standarisasi rute API *singular kebab-case* dengan mendukung alias rute array `['/account/list', '/accounts']` di `partnerApi.route.js`.
    - Menambahkan dukungan dan validasi query string `status` (`active`/`inactive`) untuk efisiensi transfer data.
    - Menjalankan dan memverifikasi pengujian integrasi otomatis menyeluruh (12 test case) yang mencakup uji otentikasi token, isolasi akun sensitif, filter status, dan penanganan kegagalan database.

#### [9805729] - resolve #300 - Senin, 14 September 2026, 16:47:44 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/partnerApiAccount.controller.js` [NEW]
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/financeAccount.service.js`
  - `backend/test/integration/partnerApiAccount.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Membuat controller `partnerApiAccount.controller.js` dengan fungsi `listPartnerAppAccount` untuk melayani permintaan data rekening bank dan akun kas perusahaan bagi aplikasi pelaporan mitra.
  - Menambahkan fungsi service `findListAccountForPartnerApi` pada `financeAccount.service.js` dengan proyeksi field yang aman (`_id`, `name`, `account_number`, `account_holder`, `bank_name`, `status`).
  - Menambahkan rute `/accounts` pada `partnerApi.route.js` di bawah proteksi middleware otentikasi `protectedPartnerApp`.
  - Menyusun berkas integration test `partnerApiAccount.test.js` untuk memvalidasi alur pemanggilan endpoint akun.

---

## 🌿 Branch: `issue-289` — Implementasi Dashboard

### 📌 Informasi Issue

- **Nomor Issue**: #289
- **Judul Issue**: Implementasi Dashboard (Multi-Domain Operational Dashboards)
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via PR #303 pada commit `069340d` dan `df64528`)

### 📅 Rincian Commit

#### [069340d] - resolve #289 - Senin, 14 September 2026, 19:31:00 WIB

- **Komponen yang Berubah**:
  - Sinkronisasi merge branch `issue-289` / `issue-297` dengan pembaruan master pasca-merge `issue-298`.
- **Deskripsi Perubahan & Fungsi**:
  - Menyatukan basis kode master terbaru ke dalam branch dashboard agar siap digabungkan tanpa konflik.

#### [5cc453c] - resolve #289 - Senin, 14 September 2026, 14:49:05 WIB

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/config/privilegeDictionary.json`
  - `backend/src/controllers/dashboard.controller.js` [NEW]
  - `backend/src/routes/dashboard.route.js` [NEW]
  - `backend/src/services/dashboard.service.js` [NEW]
  - `backend/src/controllers/financeInvoice.controller.js`
  - `backend/src/controllers/hotspotVoucher.controller.js`
  - `backend/src/controllers/networkDevice.controller.js`
  - `backend/src/controllers/networkIPv4.controller.js`
  - `backend/src/controllers/prospect.controller.js`
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/controllers/ticket.controller.js`
  - `backend/src/controllers/waChat.controller.js`
  - `backend/src/controllers/warehouseItem.controller.js`
  - `backend/src/controllers/warehouseType.controller.js`
  - `backend/src/models/networkDevice.model.js`
  - `backend/src/models/ticket.model.js`
  - `backend/src/models/waConversation.model.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/hotspotVoucher.service.js`
  - `backend/src/services/networkIPv4.service.js`
  - `backend/src/services/prospect.service.js`
  - `backend/src/services/radiusAuthentication.service.js`
  - `backend/src/services/ticket.service.js`
  - `backend/src/services/waBroadcast.service.js`
  - `backend/src/services/waConversation.service.js`
  - `backend/src/services/warehouseItem.service.js`
  - `backend/src/services/warehouseType.service.js`
  - `backend/src/services/workOrder.service.js`
  - `baileys-api/src/utils/logger.js`
  - `network-monitor/src/utils/logger.js`
  - `telegram-api/src/utils/logger.js`
  - `whatsapp-api/src/utils/logger.js`
  - `frontend/src/app/navigation/dashboards.js`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/pages/dashboards/home/*` [NEW: Header, Hub, KPI, QuickActions, Skeleton, SystemHealth, TicketPipeline]
  - `frontend/src/app/pages/dashboards/connectivity/*` [NEW: DeviceHealth, IPv4Pool, LatencyStatus, AlertSummary, OltHealth, TrafficThroughput, QuickActions, Skeleton]
  - `frontend/src/app/pages/dashboards/finance/*` [NEW: BillingAnalytics, BillingOverview, CashPosition, IsolirCard, ReceivableAging, RegulatoryObligation, GatewayRecon, Skeleton]
  - `frontend/src/app/pages/dashboards/hotspot/*` [NEW: SalesTrend, SessionsCard, ProfileDistribution, TopLocations, VoucherStatus, QuickActions, Skeleton]
  - `frontend/src/app/pages/dashboards/problem/*` [NEW: CategoryChart, IncidentSeverity, PipelineCard, ResolutionTime, TechnicianWorkload, WorkOrderCard, Skeleton]
  - `frontend/src/app/pages/dashboards/radius/*` [NEW: DashboardSkeleton, RadiusQuickActions, ActiveSessions, EventsChart, LiveLog, ServerList, UserActivity]
  - `frontend/src/app/pages/dashboards/sales/*` [NEW: BroadbandRevenue, BroadbandService, LostReason, PipelineStatus, RegionGrowth, SalesFunnel, SalesPerformanceTable, TopProducts, Skeleton]
  - `frontend/src/app/pages/dashboards/warehouse/*` [NEW: AssetDistribution, CategoryBreakdown, LowStockCard, RecentActivity, RequestStatus, QuickActions, Skeleton]
  - `frontend/src/app/pages/dashboards/whatsapp/*` [NEW: AgentWorkload, BroadcastReminder, ConnectionStatus, MessageVolume, ResponseTime, QuickActions, Skeleton]
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/i18n/locales/en/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - Mengembangkan sistem Dashboard Terpadu berskala besar (145 berkas, 20.000+ baris kode) yang mencakup 9 domain operasional ISP Dekasimal:
    1. **Home Dashboard (`/dashboards/home`)**: Menyajikan gambaran umum eksekutif (Executive Summary), status kesehatan platform, pipeline tiket aduan, navigasi cepat ke seluruh divisi operasional (Department Hub), dan aksi kilat.
    2. **Connectivity Dashboard (`/dashboards/connectivity`)**: Monitoring kesehatan perangkat OLT, ONT, dan router MikroTik secara real-time, latensi probe ICMP, trafik throughput antarmuka jaringan, ringkasan alert SNMP, serta utilisasi subnet IPv4 pool.
    3. **Finance & Billing Dashboard (`/dashboards/finance`)**: Visualisasi posisi arus kas operasional, overview penagihan bulanan, umur piutang (*aging report*), status batch otomatisasi isolir pelanggan, rekonsiliasi payment gateway (Tripay, Midtrans, Xendit), dan kalkulasi estimasi kewajiban regulasi BHP Telekomunikasi & USO.
    4. **Hotspot & Voucher Dashboard (`/dashboards/hotspot`)**: Analitik penjualan voucher Wi-Fi, monitoring sesi hotspot online bersamaan, profil kecepatan paling diminati, ranking lokasi hotspot terlaris, dan rasio status voucher (cetak, aktif, kedaluwarsa).
    5. **Problem & Tiket Gangguan Dashboard (`/dashboards/problem`)**: Distribusi kategori masalah gangguan, matriks keparahan insiden (*severity level*), tahapan pipeline resolusi tiket, metrik rata-rata waktu penyelesaian (MTTR), serta beban kerja teknisi dan surat perintah kerja (*Work Order*).
    6. **Radius Server Dashboard (`/dashboards/radius`)**: Visualisasi live sessions PPPoE/Hotspot, histori event autentikasi/accounting, inspeksi log aktivitas pengguna secara live, dan kesehatan node server RADIUS berbasis Go.
    7. **Sales & CRM Dashboard (`/dashboards/sales`)**: Pemantauan funnel penjualan (Prospek ➔ Survey ➔ Penawaran ➔ Kontrak PKS/SO ➔ Terpasang), tingkat pertumbuhan per wilayah/klaster ODP, analisis alasan prospek gagal (*lost reason*), dan performa tenaga sales/reseller.
    8. **Warehouse & Aset Fiber Dashboard (`/dashboards/warehouse`)**: Sebaran aset material jaringan (kabel drop core, ONT, closure, adaptor), deteksi peringatan stok kritis (*low stock alert*), dan riwayat permohonan material teknisi.
    9. **WhatsApp & Helpdesk Dashboard (`/dashboards/whatsapp`)**: Monitoring kanal komunikasi CS (Meta Cloud API resmi dan Baileys WhatsApp Web), rasio volume pesan masuk vs keluar, kecepatan rata-rata respon agen CS, skor kepuasan pelanggan (CSAT), dan performa siaran pengingat tagihan bulanan.
  - Penambahan rute `/api/v1/dashboard/*` di backend dengan controller dan service yang dioptimalkan menggunakan MongoDB Aggregation Pipeline untuk memastikan performa query tinggi dan minim latensi.
  - Implementasi hak akses presisi pada `privilegeDictionary.json` untuk menjamin setiap divisi hanya dapat melihat dashboard sesuai wewenangnya (`hakAksesData.read`, dsb).
  - Penambahan proteksi guard timeout (`AbortSignal.timeout(5000)`) pada logger microservice (`network-monitor`, `telegram-api`, `whatsapp-api`, `baileys-api`) untuk mencegah *hanging process* bila backend API sedang sibuk.
  - Lokalisasi lengkap i18n dwibahasa (Bahasa Indonesia & English) dengan penambahan lebih dari 1.000 key terjemahan.

---

## 🌿 Branch: `issue-298` — Tambahan Endpoint Faktur dan Tagihan untuk Pelanggan Mitra

### 📌 Informasi Issue

- **Nomor Issue**: #298
- **Judul Issue**: Tambahan Endpoint Faktur dan Tagihan untuk Pelanggan Mitra
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via PR #299 pada commit `b8b4079`)

### 📅 Rincian Commit

#### [b8b4079] - resolve #298 - Senin, 14 September 2026, 19:16:27 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/partnerApiInvoice.controller.js`
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/customer.service.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/partner.service.js`
  - `backend/test/integration/partnerApiInvoice.test.js`
- **Deskripsi Perubahan & Fungsi**:
  - Merge branch `issue-298` ke branch `master`.
  - Melakukan audit kode dan remediasi penuh sesuai standar `AGENTS.md`:
    - Mengatasi *error swallowing* pada `findListCustomerInvoiceForPartnerApi` di `financeInvoice.service.js` dengan menangkap parameter `err` dan mencatat log terstruktur via `logger.error(...)` sebelum melempar error i18n.
    - Menjamin integritas data pelanggan nonaktif (*soft-deleted*): Mengubah `Customer.find` menjadi `Customer.findWithDeleted` pada fungsi `findCustomerIdsByPartner` di `customer.service.js`, serta menambahkan opsi `{ options: { withDeleted: true } }` pada populate customer di controller. Hal ini memastikan riwayat transaksi dan tagihan pelanggan yang sudah dihapus tetap dapat dilihat oleh mitra tanpa memicu error 404.
    - Optimasi performa database: Menerapkan mekanisme *short-circuit* (early return) pada `listPartnerAppCustomerInvoice` bila mitra belum memiliki pelanggan terdaftar, menghindari eksekusi dua query MongoDB redundan (`countDocuments` dan `find`).
    - Memverifikasi kepatuhan seluruh skenario pengujian (35 test case) pada file integrasi `partnerApiInvoice.test.js`.

#### [19df9b6] - resolve #298 - Senin, 14 September 2026, 12:43:22 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/partnerApiInvoice.controller.js` [NEW]
  - `backend/src/routes/partnerApi.route.js`
  - `backend/src/services/customer.service.js`
  - `backend/src/services/financeInvoice.service.js`
  - `backend/src/services/partner.service.js`
  - `backend/test/integration/partnerApiInvoice.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Mengembangkan modul `partnerApiInvoice.controller.js` dengan fungsi `listPartnerAppInvoice`, `readPartnerAppInvoice`, `listPartnerAppCustomerInvoice`, dan `readPartnerAppCustomerInvoice`.
  - Menambahkan logika query dan paginasi faktur pelanggan mitra di `financeInvoice.service.js` (`findListCustomerInvoiceForPartnerApi`).
  - Mendaftarkan rute API faktur mitra pada `partnerApi.route.js` dengan autentikasi `protectedPartnerApp`.
  - Membuat rangkaian integration test komprehensif (670+ baris) untuk memverifikasi isolasi faktur antar-mitra (mencegah celah keamanan IDOR / *Insecure Direct Object Reference*), autentikasi JWT, dan format response tagihan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #300  | Tambahan Endpoint untuk Metode Pembayaran | Menyediakan endpoint REST API terisolasi dan aman bagi aplikasi mitra untuk membaca daftar akun bank dan kas pembayaran perusahaan guna keperluan transaksi pelanggan mitra. |
| #289  | Implementasi Dashboard | Menghadirkan ekosistem analitik operasional terpadu 9 divisi dengan visualisasi interaktif, monitoring real-time, metrik KPI, dan shortcut penanganan masalah bagi seluruh tim ISP. |
| #298  | Tambahan Endpoint Faktur dan Tagihan untuk Pelanggan Mitra | Menyediakan API faktur/tagihan berkemampuan paginasi, filter pencarian, dan proteksi IDOR bagi mitra bisnis untuk memantau invoice sendiri maupun pelanggan binaannya (termasuk pelanggan soft-deleted). |

### Kemampuan Baru Pengguna/Admin

- **Mitra Bisnis (Partner POP/Reseller)**:
  - Dapat melihat dan mengintegrasikan daftar rekening bank/kas resmi perusahaan secara dinamis ke dalam aplikasi mobile/portal mitra mereka melalui endpoint `GET /api/v1/partner-api/account/list`.
  - Dapat memantau riwayat tagihan faktur mereka sendiri beserta rincian pembayarannya melalui endpoint `GET /api/v1/partner-api/invoices`.
  - Dapat mengelola dan melacak status pembayaran tagihan seluruh pelanggan yang berada di bawah naungan mitra melalui endpoint `POST /api/v1/partner-api/customer-invoices/list` dan `GET /api/v1/partner-api/customer-invoices/read/:id`.
- **Manajemen & Staf Internal ISP**:
  - Memiliki visibilitas holistik 360 derajat terhadap seluruh aspek operasional ISP melalui 9 modul dashboard spesifik divisi (Executive, Jaringan, Keuangan, Hotspot, Tiket, Radius, Penjualan, Gudang, dan WhatsApp CS).
  - Mampu mendeteksi secara dini anomali jaringan, perangkat offline, lonjakan piutang tak tertagih, stok barang menipis, maupun pesan pelanggan yang belum terbalas.

### Bug Fix / Solusi Masalah

- **Pencegahan Error Swallowing & Observability**: Mengatasi masalah blok `catch` tanpa penangkapan error di `financeAccount.service.js` dan `financeInvoice.service.js`. Seluruh error teknis kini dicatat secara terstruktur ke Winston (`logger.error`) lengkap dengan context dan stack trace sebelum melempar error i18n ke client.
- **Isolasi Keamanan Akun Keuangan**: Menyaring akun bank/kas pada Partner API agar akun personal milik admin (`owner_admin`) dan akun sistem gateway internal (`is_system`) tidak bocor ke pihak luar.
- **Integritas Data Pelanggan Terhapus Lunak (*Soft-deleted*)**: Memperbaiki pencarian invoice pelanggan mitra agar faktur historis milik pelanggan berstatus soft-delete tetap dapat ditampilkan dengan nama pelanggan lengkap dan tidak memicu respon 404 Not Found.
- **Optimasi Kueri Redundan (*Short-Circuiting*)**: Mengeliminasi kueri MongoDB yang sia-sia pada endpoint tagihan ketika mitra belum memiliki pelanggan.
- **Pencegahan Hanging Logging pada Microservices**: Menambahkan pengaman `AbortSignal.timeout(5000)` pada fungsi `reportFatalErrorToBackend` di seluruh microservice pendukung (`network-monitor`, `telegram-api`, `whatsapp-api`, `baileys-api`).

### Menu/Fitur Baru

1. **Menu Dashboard Terpadu (`/dashboards/*`)**:
   - `Ringkasan Eksekutif` (`/dashboards/home`)
   - `Konektivitas & Jaringan` (`/dashboards/connectivity`)
   - `Keuangan & Billing` (`/dashboards/finance`)
   - `Hotspot & Voucher` (`/dashboards/hotspot`)
   - `Penanganan Masalah & Tiket` (`/dashboards/problem`)
   - `Server RADIUS` (`/dashboards/radius`)
   - `Penjualan & CRM` (`/dashboards/sales`)
   - `Gudang & Aset Fiber` (`/dashboards/warehouse`)
   - `Layanan WhatsApp & Helpdesk` (`/dashboards/whatsapp`)
2. **Partner API Endpoints**:
   - `GET /api/v1/partner-api/account/list` & `GET /api/v1/partner-api/accounts` (Daftar Akun Bank Pembayaran)
   - `GET /api/v1/partner-api/invoices` & `GET /api/v1/partner-api/invoices/:id` (Faktur Mitra)
   - `POST /api/v1/partner-api/customer-invoices/list` & `GET /api/v1/partner-api/customer-invoices/read/:id` (Faktur Pelanggan Mitra)

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penjelasan Fitur: Ekosistem Dashboard Multi-Divisi Dekasimal V2

Sistem Dashboard Dekasimal V2 dirancang untuk memecah silo informasi antar-departemen pada penyedia layanan internet (ISP). Setiap divisi memiliki tampilan terdedikasi yang menampilkan metrik paling krusial, indikator KPI, grafik performa, dan tombol aksi cepat (*Quick Actions*). Seluruh data disajikan secara dinamis dengan proteksi hak akses berbasis peran (*Role-Based Access Control* / RBAC).

### 2. Langkah Penggunaan (Tutorial)

#### A. Mengakses dan Menavigasi Dashboard Operasional:
1. Masuk (*Login*) ke aplikasi Dekasimal Web App menggunakan akun staf/admin yang memiliki hak akses dashboard terkait.
2. Pada bilah navigasi samping (*Sidebar*), klik menu **Dashboards** untuk membuka sub-menu analitik:
   - Pilih **Ringkasan Eksekutif** untuk melihat KPI umum perusahaan, antrean tiket, dan hub departemen.
   - Pilih **Konektivitas** untuk mengecek status kesehatan perangkat OLT, throughput bandwidth, dan sisa IP pool.
   - Pilih **Keuangan** untuk meninjau arus kas, status tagihan bulanan, serta daftar pelanggan dalam antrean isolir.
   - Pilih **Tiket Gangguan** untuk memantau antrean masalah lapangan dan meninjau beban kerja teknisi.
   - Pilih **Gudang** untuk mengecek stok material kritis (seperti drop core dan ONT) sebelum kehabisan.
   - Pilih **WhatsApp** untuk mengecek status koneksi nomor gateway CS dan memantau pesan masuk yang membutuhkan respon segera.
3. Gunakan kartu **Pintasan Aksi Cepat (*Quick Actions*)** pada setiap dashboard untuk langsung mengeksekusi tindakan umum (misalnya membuat tiket baru, mencetak voucher, atau membuka live chat) tanpa harus berpindah-pindah menu navigasi.

#### B. Mengonsumsi Partner API Akun & Faktur (Bagi Pengembang Aplikasi Mitra):
1. **Dapatkan Token Akses Mitra**: Lakukan autentikasi menggunakan kredensial mitra pada endpoint login Partner API untuk mendapatkan JWT token.
2. **Mengambil Daftar Akun Pembayaran**:
   - Kirimkan HTTP GET request ke `https://api.domain.com/api/v1/partner-api/account/list` (atau query opsional `?status=active`).
   - Sertakan header `Authorization: Bearer <token_mitra>`.
   - Response akan mengembalikan daftar akun bank/kas resmi yang dapat dipilih pelanggan untuk pembayaran tagihan.
3. **Mengecek Tagihan Pelanggan Mitra**:
   - Kirimkan HTTP POST request ke `https://api.domain.com/api/v1/partner-api/customer-invoices/list` dengan payload paginasi dan filter.
   - Ambil detail faktur spesifik menggunakan `GET /api/v1/partner-api/customer-invoices/read/:invoice_id`.
