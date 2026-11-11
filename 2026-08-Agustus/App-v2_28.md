# 📝 Daily Work Report - Dedy (2026-08-28)

---

## 📅 Laporan Harian - 28 Agustus 2026

---

## 🌿 Branch: `issue-245` — Cacti-Style Network Traffic Monitoring System & Full-Stack Audit

### 📌 Informasi Issue

- **Nomor Issue**: #245
- **Judul Issue**: Cacti-Style Network Traffic Monitoring — Integrasi RRDTool, SNMP Interface Auto-Discovery, Polling Microservice, Visual Graph Dashboard, Spike Killer, dan Audit Kualitas Kode Full-Stack
- **Status Branch**: `Belum di-merge` (Active Development / In Progress)

### 📅 Rincian Commit

#### [b9bba29] - save #245 - 28 Agustus 2026, 21:07:09 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/networkTraffic.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/networkTraffic.service.js) — Penyempurnaan pipeline agregasi datatable trafik, sanitasi parameter interval waktu, dan penanganan fallback status target monitoring.
  - [`backend/src/controllers/networkTraffic.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/networkTraffic.controller.js) — Standardisasi HTTP error status code, sanitasi body dengan `cleanFormData`, dan penanganan async handler.
  - [`backend/src/routes/networkTraffic.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/networkTraffic.route.js) — Penambahan route endpoint data rate instan dan dokumentasi Swagger query params.
  - [`backend/src/services/financeWallet.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeWallet.service.js) — Audit dan penguatan validasi mutasi saldo dompet digital mitra serta konsistensi rollback transaksi.
  - [`backend/src/services/networkDevice.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/networkDevice.service.js) — Peningkatan query relasi SNMP interface dan penanganan fallback timeout koneksi perangkat.
  - [`backend/src/services/productBroadband.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/productBroadband.service.js) & [`backend/src/services/radiusProfile.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusProfile.service.js) — Refactoring kueri relasi profil bandwidth dan pembersihan unused variable linter.
  - [`backend/src/models/financeBudgeting.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financeBudgeting.model.js) — Penyesuaian constraint index dan schema validation budgeting periode keuangan.
  - [`backend/test/integration/financeWallet.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeWallet.test.js) & [`backend/test/integration/networkDevice.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/networkDevice.service.test.js) — Penambahan unit & integration tests untuk pengujian edge-cases wallet dan device discovery.
  - [`frontend/src/app/pages/network/traffic/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/index.jsx) — Pembersihan state re-rendering, optimasi layout grid grafik trafik visual, dan integrasi filter status dinamis.
  - [`frontend/src/app/pages/network/traffic/components/InterfaceDiscoveryDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/components/InterfaceDiscoveryDrawer.jsx) — Perbaikan binding `Listbox` Headless UI untuk seleksi perangkat dan interface batch.
  - [`frontend/src/app/pages/network/traffic/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/schema/columns.jsx) — Standardisasi kolom tabel TanStack Table, cell helpers, dan badge formatter bit rate (Kbps, Mbps, Gbps).
  - [`frontend/src/components/shared/form/Listbox.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/form/Listbox.jsx) — Refactoring kompatibilitas mode single/multiple selection agar terintegrasi sempurna dengan `useController`.
  - [`network-monitor/src/services/rrd.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/rrd.service.js) — Penguatan eksekusi binary `rrdtool` CLI dengan sanitasi input path dan penanganan timeout.
- **Deskripsi Perubahan & Fungsi**:
  - Melakukan audit menyeluruh kualitas kode (Full-Stack Code Audit) untuk membersihkan peringatan ESLint, unused variables/imports, serta memastikan kepatuhan terhadap standar arsitektur AGENTS.md.
  - Memperbaiki kompatibilitas komponen form `Listbox` agar mendukung pemilihan item secara aman tanpa error null pointer saat array opsi diperbarui.
  - Memperkuat integrasi file RRD di modul `network-monitor` agar dapat menangani concurrent write/read secara stabil tanpa file locking collision.

---

#### [d8bab27] - save #245 - 28 Agustus 2026, 17:18:01 WIB

- **Komponen yang Berubah**:
  - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) — Pendaftaran hak akses terinci untuk manajemen pemantauan trafik jaringan: `networkTraffic.list`, `networkTraffic.read`, `networkTraffic.create`, `networkTraffic.update`, `networkTraffic.delete`.
  - [`backend/src/utils/snmp.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/snmp.js) — Implementasi fungsi helper baru [`walkInterfaceOid()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/snmp.js) yang mendukung penjelajahan subtree interface MIB-II (`1.3.6.1.2.1.2.2.1.2` dan `1.3.6.1.2.1.31.1.1.1.1`), pemetaan otomatis OID counter `ifHCInOctets` (download) dan `ifHCOutOctets` (upload), serta konfigurasi port SNMP & versi (`1`/`2c`) kustom.
- **Deskripsi Perubahan & Fungsi**:
  - Mengonfigurasi hak akses berbasis RBAC (Role-Based Access Control) di backend untuk mengamankan seluruh API endpoint monitoring trafik.
  - Menyediakan utilitas SNMP subtree walk yang efisien untuk membaca indeks seluruh port/interface fisik maupun VLAN pada router/switch secara instan tanpa perlu input manual nomor OID.

---

#### [54223ff] - save #245 - 28 Agustus 2026, 16:00:31 WIB

- **Komponen yang Berubah**:
  - [`backend/src/models/networkTrafficTarget.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/networkTrafficTarget.model.js) [NEW] — Skema Mongoose untuk target monitoring trafik (relasi ke `network_devices`, nama target, index interface, OID in/out, path file `.rrd`, status aktif, telemetry terakhir).
  - [`backend/src/services/networkTraffic.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/networkTraffic.service.js) [NEW] — Layanan CRUD target monitoring, orkestrasi inisialisasi RRD via microservice, sinkronisasi status polling, eksekusi pembersihan lonjakan trafik (Spike Killer), dan pemanggilan graph generator.
  - [`backend/src/controllers/networkTraffic.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/networkTraffic.controller.js) [NEW] & [`backend/src/routes/networkTraffic.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/networkTraffic.route.js) [NEW] — REST API controller dan rute dengan dokumentasi OpenAPI/Swagger lengkap.
  - [`backend/src/controllers/internal.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/internal.controller.js) & [`backend/src/routes/internal.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/internal.route.js) — Endpoint internal `/internal/network-traffic/poll` yang diakses secara aman oleh Cron Worker via `INTERNAL_API_KEY`.
  - [`cron-worker/src/jobs/processors/networkTrafficPoll.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/processors/networkTrafficPoll.js) [NEW], [`cron-worker/src/jobs/scheduler.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/scheduler.js), [`cron-worker/src/jobs/worker.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/worker.js) — Job worker BullMQ periodik untuk memicu polling trafik jaringan secara terjadwal dan idempoten.
  - [`network-monitor/src/services/rrd.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/rrd.service.js) [NEW] — Engine manajemen basis data RRD (Round Robin Database) menggunakan RRDTool: pembuatan berkas RRD dengan Round Robin Archives (RRA) beresolusi bertingkat (1 jam, 24 jam, 7 hari, 30 hari, 1 tahun), update counter, rendering grafik SVG/PNG visual Cacti-style, dan algoritma Spike Killer.
  - [`network-monitor/src/services/trafficPoller.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/trafficPoller.service.js) [NEW] — Service polling SNMP berkecepatan tinggi dengan concurrent batching untuk mengambil nilai counter traffic dan memperbarui database RRD secara efisien.
  - [`network-monitor/src/controllers/rrd.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/controllers/rrd.controller.js) [NEW] & [`network-monitor/src/routes/rrd.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/routes/rrd.route.js) [NEW] — API REST microservice untuk probe dan visualisasi RRD.
  - [`frontend/src/app/pages/network/traffic/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/index.jsx) [NEW] — Halaman antarmuka utama Network Traffic Dashboard dengan dukungan toggle view (Tabel Datatable & Cacti Visual Grid).
  - [`frontend/src/app/pages/network/traffic/components/CactiGraphCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/components/CactiGraphCard.jsx) [NEW] — Kartu grafik trafik visual interaktif Cacti-style yang menampilkan kurva bit rate download/upload, ringkasan Current/Average/Max, rentang waktu multi-resolusi, dan tombol aksi langsung.
  - [`frontend/src/app/pages/network/traffic/components/GraphZoomModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/components/GraphZoomModal.jsx) [NEW] — Modal inspeksi grafik resolusi tinggi dengan kemampuan zoom dinamis dan navigasi rentang tanggal historis.
  - [`frontend/src/app/pages/network/traffic/components/InterfaceDiscoveryDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/components/InterfaceDiscoveryDrawer.jsx) [NEW] — Drawer pencarian otomatis interface router via SNMP walk untuk penambahan target monitoring secara instan.
  - [`frontend/src/app/pages/network/traffic/components/SpikeKillerModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/components/SpikeKillerModal.jsx) [NEW] — Modal utilitas Spike Killer untuk memotong lonjakan trafik abnormal yang merusak skala grafik.
  - [`frontend/src/app/pages/network/traffic/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/schema/columns.jsx) [NEW] & [`frontend/src/app/router/network/networkTrafficRoute.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/network/networkTrafficRoute.jsx) [NEW] — Routing SPA dan konfigurasi kolom tabel trafik.
  - [`frontend/src/app/navigation/networks.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/networks.js) — Pendaftaran menu navigasi "Traffic Monitoring" di bawah modul Network.
  - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) — Penambahan key terjemahan multibahasa lengkap untuk antarmuka traffic monitoring.
- **Deskripsi Perubahan & Fungsi**:
  - Membangun ekosistem pemantauan trafik jaringan end-to-end terintegrasi berbasis RRDTool bergaya Cacti klasik namun dibalut dengan antarmuka modern yang responsif.
  - Mengisolasi pemrosesan probe SNMP dan manipulasi binary RRD ke dalam microservice `network-monitor` demi menjaga keandalan dan skalabilitas backend utama.
  - Mengotomatiskan penemuan port/interface router melalui SNMP walk, mengeliminasi kebutuhan konfigurasi OID manual oleh network engineer.

---

## 🌿 Branch: `issue-223` — Customer PKS (Perjanjian Kerja Sama) Document & Workflow Management

### 📌 Informasi Issue

- **Nomor Issue**: #223
- **Judul Issue**: Customer PKS Document Management — Pembuatan Dokumen PKS, Template Klausul Legal, Approval Workflow, Portal Publik Tanda Tangan Digital, dan Integrasi Telegram Bot Notifikasi
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `7ecb126` & `4519589`)

### 📅 Rincian Commit

#### [4519589] / [7ecb126] - resolve #223 - 28 Agustus 2026, 20:31:47 / 20:39:14 WIB

- **Komponen yang Berubah**:
  - [`backend/src/models/customerPKS.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/customerPKS.model.js) [NEW] — Skema Mongoose dokumen PKS mencakup data identitas pihak pertama (provider) & pihak kedua (pelanggan/perusahaan), nomor surat perjanjian, durasi kontrak kerja sama, pasal-pasal perjanjian (SLA, hak & kewajiban, wanprestasi, force majeure), status lifecycle (`draft`, `pendingApproval`, `approved`, `rejected`, `signed`, `active`, `expired`), serta metadata digital signature.
  - [`backend/src/services/customerPKS.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/customerPKS.service.js) [NEW] — Layanan bisnis untuk pembuatan PKS, update draft, transisi status approval, pembuatan secure token signing untuk akses publik pelanggan, dan audit log perubahan.
  - [`backend/src/controllers/customerPKS.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customerPKS.controller.js) [NEW] & [`backend/src/controllers/publicCustomerPKS.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/publicCustomerPKS.controller.js) [NEW] — Controller request handling untuk dashboard internal admin dan portal publik tanda tangan digital pelanggan tanpa autentikasi JWT (menggunakan cryptographic token validation).
  - [`backend/src/routes/customerPKS.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/customerPKS.route.js) [NEW] & [`backend/src/routes/public.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/public.route.js) — Pendaftaran rute API terproteksi hak akses dan rute publik `/public/pks/:token`.
  - [`backend/src/utils/telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js) — Penambahan integrasi notifikasi Telegram bot untuk pemberitahuan otomatis ke grup manajemen ketika dokumen PKS baru diajukan untuk ditinjau atau telah berhasil ditandatangani oleh pelanggan.
  - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) — Pendaftaran hak akses RBAC: `customerPKS.list`, `customerPKS.read`, `customerPKS.create`, `customerPKS.update`, `customerPKS.delete`, `customerPKS.changeStatus`, `customerPKS.approve`.
  - [`frontend/src/app/pages/users/customerPKS/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customerPKS/create.jsx) [NEW] & [`frontend/src/app/pages/users/customerPKS/edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customerPKS/edit.jsx) [NEW] — Halaman pembuatan dan penyuntingan dokumen PKS dengan editor pasal dinamis, generator nomor surat otomatis, dan integrasi pemilihan paket langganan.
  - [`frontend/src/app/pages/users/customerPKS/constants/pksTemplates.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customerPKS/constants/pksTemplates.js) [NEW] — Preset template pasal-pasal standar legal hukum untuk layanan Internet Dedicated, Broadband Bisnis, dan Kemitraan Reseller.
  - [`frontend/src/app/pages/users/customerPKS/CustomerPKSReviewDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customerPKS/CustomerPKSReviewDrawer.jsx) [NEW] — Drawer verifikasi dokumen PKS bagi manajer/direksi dengan opsi persetujuan (*Approve*) atau penolakan (*Reject*) disertai alasan perbaikan.
  - [`frontend/src/app/pages/users/customerPKS/CustomerPKSDocumentPreview.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customerPKS/CustomerPKSDocumentPreview.jsx) [NEW] — Komponen pratinjau dokumen PKS resmi berformat kertas legal standar (A4/Folio) siap cetak (*Print Portal*) dengan tata letak kop surat, klausul perjanjian, tabel paket, dan kolom tanda tangan kedua belah pihak.
  - [`frontend/src/app/pages/public/PublicCustomerPKSDocument.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/public/PublicCustomerPKSDocument.jsx) [NEW] & [`frontend/src/app/pages/public/ReviewCustomerPKSPage.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/public/ReviewCustomerPKSPage.jsx) [NEW] — Halaman publik eksternal berkeamanan tinggi yang memungkinkan pelanggan meninjau isi kontrak dan membubuhkan tanda tangan digital (*E-Signature pad*) secara langsung melalui browser/smartphone.
  - [`frontend/src/app/pages/users/document/pks/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/document/pks/index.jsx) [NEW] & [`frontend/src/app/pages/users/document/pks/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/document/pks/schema/columns.jsx) [NEW] — Tampilan tabel data pendaftaran dokumen PKS pada sub-menu Document Registry.
  - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) — Lokalisasi lengkap istilah kontrak hukum dan alur persetujuan PKS.
- **Deskripsi Perubahan & Fungsi**:
  - Mengotomatiskan seluruh siklus hidup dokumen Perjanjian Kerja Sama (PKS) antara penyedia layanan dan pelanggan korporat/B2B, mulai dari drafting pasal hingga penandatanganan elektronik.
  - Memungkinkan penandatanganan dokumen secara digital tanpa kertas (paperless) melalui tautan publik berenkripsi token unik yang dapat diakses langsung oleh klien.
  - Mengirimkan laporan instan ke grup Telegram ketika kontrak disetujui atau ditandatangani untuk mempercepat proses aktivasi layanan pelanggan.

---

## 🌿 Branch: `issue-228` — Customer SDN (Surat Dukungan Negosiasi) System & Network Device Improvements

### 📌 Informasi Issue

- **Nomor Issue**: #228
- **Judul Issue**: Customer SDN Document System — Surat Dukungan Lelang/Proyek, Review & Digital Signature, serta Peningkatan Manajemen Perangkat Jaringan (SNMP Walk & IPv4 Management)
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `ebb0075`)

### 📅 Rincian Commit

#### [ebb0075] - resolve #228 - 28 Agustus 2026, 14:46:44 WIB

- **Komponen yang Berubah**:
  - [`backend/src/models/customerSDN.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/customerSDN.model.js) [NEW] — Skema basis data Surat Dukungan Negosiasi/Tender (SDN) mencakup nama proyek/lelang, instansi pengguna jasa, rincian bandwidth dan perangkat yang didukung, tanggal berlaku, dan verifikasi status.
  - [`backend/src/services/customerSDN.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/customerSDN.service.js) [NEW], [`backend/src/controllers/customerSDN.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customerSDN.controller.js) [NEW], [`backend/src/controllers/publicCustomerSDN.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/publicCustomerSDN.controller.js) [NEW], [`backend/src/routes/customerSDN.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/customerSDN.route.js) [NEW] — Arsitektur REST API lengkap dokumen dukungan tender dengan dukungan akses portal publik.
  - [`frontend/src/app/pages/users/document/sdn/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/document/sdn/create.jsx) [NEW], [`frontend/src/app/pages/users/document/sdn/edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/document/sdn/edit.jsx) [NEW], [`frontend/src/app/pages/users/document/sdn/ReviewDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/document/sdn/ReviewDrawer.jsx) [NEW], [`frontend/src/app/pages/users/document/sdn/DocumentPreview.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/document/sdn/DocumentPreview.jsx) [NEW] — Form pembuatan, drawer review, dan pratinjau dokumen surat dukungan lelang resmi.
  - [`frontend/src/app/pages/public/PublicCustomerSDNDocument.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/public/PublicCustomerSDNDocument.jsx) [NEW] & [`frontend/src/app/pages/public/ReviewCustomerSDNPage.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/public/ReviewCustomerSDNPage.jsx) [NEW] — Portal verifikasi keaslian dokumen surat dukungan tender untuk instansi penyelenggara pengadaan.
  - [`frontend/src/app/pages/network/devices/components/SnmpWalkSection.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/devices/components/SnmpWalkSection.jsx) — Peningkatan antarmuka eksplorasi SNMP walk interaktif pada modul Network Device dengan visualisasi pohon MIB dan status respon live.
  - [`frontend/src/app/pages/network/devices/detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/devices/detail.jsx), [`frontend/src/app/pages/network/devices/discover.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/devices/discover.jsx), [`frontend/src/app/pages/network/devices/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/devices/create.jsx), [`frontend/src/app/pages/network/devices/edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/devices/edit.jsx) — Refactoring komponen perangkat jaringan untuk meningkatkan akurasi auto-discovery subnet dan kejelasan tampilan detail perangkat.
  - [`frontend/src/app/pages/network/ipv4Management/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/ipv4Management/schema/columns.jsx) — Penyempurnaan filter subnetting dan visualisasi status alokasi IP address.
  - [`backend/src/controllers/customerPO.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customerPO.controller.js), [`backend/src/controllers/customerSO.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customerSO.controller.js), [`backend/src/controllers/customerQuotation.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customerQuotation.controller.js) — Integrasi relasi dokumen penawaran dan pemesanan dengan dokumen pendukung SDN.
- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan sistem penerbitan Surat Dukungan Negosiasi (SDN) untuk kebutuhan lelang/tender proyek pelanggan korporat secara terstruktur dengan penomoran otomatis dan template legal resmi.
  - Meningkatkan stabilitas dan kejelasan UI modul Network Device dalam melakukan penjelajahan OID SNMP perangkat serta perbaikan filter data pada tabel alokasi IPv4.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul Modul / Fitur | Dampak Utama |
| :--- | :--- | :--- |
| **#245** | Cacti Network Traffic Monitoring & Full-Stack Audit | Memungkinkan pemantauan bandwidth interface perangkat jaringan secara real-time via RRDTool, auto-discovery interface via SNMP, visual graph multi-timeframe, utilitas pembersihan spike, dan perbaikan kualitas kode full-stack. |
| **#223** | Customer PKS Document Management | Otomatisasi drafting, review bertingkat, dan penandatanganan elektronik perjanjian kerja sama pelanggan korporat via portal publik terenkripsi dan notifikasi Telegram. |
| **#228** | Customer SDN System & Network Devices | Penerbitan dan verifikasi keaslian surat dukungan lelang/tender secara digital, didukung peningkatan fitur SNMP walk dan manajemen IP perangkat jaringan. |

### Kemampuan Baru Pengguna/Admin

- **Admin Jaringan (NOC/Engineer)**:
  - Dapat menelusuri (*SNMP Walk*) seluruh antarmuka pada router/switch secara instan dan mendaftarkannya ke sistem monitoring trafik tanpa perlu menghafal atau menyalin OID manual.
  - Dapat memantau utilisasi bandwidth antarmuka router dalam berbagai skala waktu (1 Jam, 24 Jam, 7 Hari, 30 Hari, 1 Tahun) dengan tampilan visual grafik Cacti yang presisi.
  - Dapat memperbesar grafik (*Zoom In*) pada rentang waktu tertentu untuk menganalisis lonjakan penggunaan bandwidth.
  - Dapat menjalankan utilitas **Spike Killer** untuk memotong anomali lonjakan data yang merusak proporsi grafik.
- **Admin Penjualan & Legal (Sales/Account Manager)**:
  - Dapat menyusun dokumen Perjanjian Kerja Sama (PKS) dan Surat Dukungan Negosiasi (SDN) dengan template klausul hukum siap pakai.
  - Dapat mengirimkan link peninjauan dan tanda tangan dokumen digital ke pelanggan/mitra secara aman via token unik.
  - Menerima notifikasi instan di Telegram saat dokumen selesai ditinjau atau ditandatangani oleh klien.

### Bug Fix / Solusi Masalah

- **Pembersihan Linter & Unused Variables**: Menghapus seluruh variabel, import, dan parameter yang tidak terpakai di backend dan frontend guna memenuhi standar ketat ESLint dan menjaga kebersihan basis kode.
- **Form Listbox Headless UI**: Memperbaiki masalah sinkronisasi form control pada komponen select/combobox sehingga proses seleksi single maupun multiple berjalan mulus tanpa error state `undefined`.
- **Ketahanan RRD Database File Locking**: Mencegah tabrakan proses penulisan file `.rrd` di microservice `network-monitor` saat terjadi polling batch dengan volume target monitoring yang tinggi.
- **Pengamanan Mutasi Dompet Digital**: Memperketat mekanisme verifikasi saldo dan rollback pada modul `financeWallet` untuk mencegah inkonsistensi pembukuan kas.

### Menu/Fitur Baru

- **Menu `Network > Traffic Monitoring`** (`/network/traffic`): Dashboard visual pemantauan grafik trafik antarmuka perangkat jaringan.
- **Menu `Documents > PKS (Perjanjian Kerja Sama)`** (`/document/pks`): Modul pengelolaan, drafting, dan review kontrak hukum pelanggan.
- **Menu `Documents > SDN (Surat Dukungan Negosiasi)`** (`/document/sdn`): Modul penerbitan dan pengesahan surat dukungan tender proyek.
- **Portal Publik Tanda Tangan Digital PKS & SDN** (`/public/pks/:token` & `/public/sdn/:token`): Halaman web publik responsif untuk penandatanganan dokumen oleh pelanggan eksternal.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Menambahkan Antarmuka Perangkat ke Network Traffic Monitoring

- **Penjelasan Fitur**: Fitur ini memungkinkan network engineer memindai seluruh antarmuka pada router MikroTik, Cisco, atau switch manajemen via SNMP secara otomatis, lalu memilih antarmuka yang ingin dipantau grafiknya tanpa perlu mengisi OID manual.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu navigasi **Network** → **Traffic Monitoring**.
  2. Klik tombol **"Discovery Interface"** di pojok kanan atas tabel.
  3. Pada drawer yang terbuka, pilih perangkat router dari daftar **Network Device**.
  4. Klik tombol **"Scan Interfaces"** untuk menjalankan SNMP walk otomatis ke perangkat.
  5. Sistem akan menampilkan daftar seluruh interface yang aktif beserta indeks dan namanya (misal: `ether1-WAN`, `sfp-plus1-Uplink`).
  6. Centang satu atau beberapa interface yang ingin dipantau, lalu klik **"Add to Monitoring"**.
  7. Sistem akan secara otomatis menginisialisasi basis data RRD dan grafik visual akan langsung muncul pada tampilan **Graph View**.

### 2. Menggunakan Fitur Spike Killer pada Grafik Trafik

- **Penjelasan Fitur**: Ketika router mengalami reboot mendadak atau anomali counter 64-bit, file RRD terkadang mencatat lonjakan trafik abnormal yang sangat tinggi (misal: lonjakan semu ratusan Gbps), menyebabkan skala grafik normal menjadi tidak terbaca. Fitur Spike Killer membersihkan nilai anomali tersebut secara otomatis.
- **Langkah Penggunaan (Tutorial)**:
  1. Pada halaman **Traffic Monitoring**, arahkan ke kartu grafik antarmuka yang mengalami lonjakan data.
  2. Klik ikon menu aksi pada kartu grafik, lalu pilih **"Spike Killer"**.
  3. Masukkan batas atas nilai lonjakan maksimum (*Outlier Threshold / Max Limit*) atau biarkan default otomatis.
  4. Klik tombol **"Kill Spikes"**.
  5. Sistem akan memindai database RRD, memotong nilai ekstrim di atas ambang batas, meregenerasi grafik, dan mengembalikan proporsi visual grafik ke kondisi normal.

### 3. Alur Pembuatan & Tanda Tangan Digital PKS (Perjanjian Kerja Sama)

- **Penjelasan Fitur**: Alur penyusunan kontrak hukum B2B digital mulai dari pemilihan template pasal, penyesuaian klausul khusus, persetujuan manajerial internal, hingga penandatanganan basah/elektronik oleh perwakilan pelanggan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Documents** → **PKS**.
  2. Klik tombol **"Buat PKS Baru"**.
  3. Pilih data pelanggan/partner, nomor penawaran (quotation), masa berlaku kontrak, dan template pasal yang sesuai.
  4. Sesuaikan klausul SLA atau pasal tambahan jika diperlukan, lalu simpan sebagai draft atau klik **"Ajukan Approval"**.
  5. Manajer/Direksi yang berwenang akan meninjau draf dokumen via **Review Drawer** dan memberikan persetujuan (*Approve*).
  6. Setelah disetujui, salin tautan publik dokumen (*Public Signing Link*) dan kirimkan ke klien.
  7. Klien membuka tautan tersebut pada browser atau smartphone, membaca seluruh isi perjanjian, dan membubuhkan tanda tangan digital pada kotak tanda tangan yang disediakan.
  8. Setelah klien mengirimkan tanda tangan, status PKS otomatis berubah menjadi **"Signed / Active"** dan sistem mengirimkan notifikasi konfirmasi ke grup Telegram manajemen.
