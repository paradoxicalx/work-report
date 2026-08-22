# 📝 Daily Work Report - Dedy S.N Putra (2026-08-20)

---

## 📅 Laporan Harian - 20 Agustus 2026

---

## 🌿 Branch: `issue-230` — Modul Payment Gateway iPaymu (Pemantauan Transaksi Online, Saldo Merchant, Kanal Pembayaran, Rekonsiliasi Otomatis, Settlement Kas, dan Background Sweep)

### 📌 Informasi Issue

- **Nomor Issue**: #230
- **Judul Issue**: Modul Payment Gateway iPaymu (Pemantauan Transaksi Online, Saldo Merchant, Kanal Pembayaran, Rekonsiliasi Otomatis, Settlement Kas, dan Background Sweep)
- **Status Branch**: `Belum di-merge` (Branch aktif: `origin/issue-230`)

### 📅 Rincian Commit

#### [bee4ba4] - resolve #230 - 20 Agt 2026 23:00

- **Komponen yang Berubah**:
  - `FINANCE_AUDIT.md`
  - `backend/.env.example`
  - `backend/package-lock.json`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/financeGateway.controller.js` [NEW]
  - `backend/src/controllers/financePayment.controller.js`
  - `backend/src/controllers/financeSettings.controller.js`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-230.json` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeGatewayTransaction.model.js` [NEW]
  - `backend/src/models/financePayment.model.js`
  - `backend/src/routes/financeGateway.route.js` [NEW]
  - `backend/src/routes/financeSettings.route.js`
  - `backend/src/routes/internal.route.js`
  - `backend/src/routes/public.route.js`
  - `backend/src/services/financeGateway.service.js` [NEW]
  - `backend/src/services/financeLedger.service.js`
  - `backend/src/services/financePayment.service.js`
  - `backend/src/services/invoiceFreeze.service.js`
  - `backend/src/services/paymentGateway.service.js`
  - `backend/test/integration/financeGateway.callback.test.js` [NEW]
  - `backend/test/integration/financeGateway.settle.test.js` [NEW]
  - `cron-worker/src/jobs/processors/financeGatewaySweep.js` [NEW]
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `frontend/src/app/navigation/finance.js`
  - `frontend/src/app/pages/finance/gateway/GatewayChannelsPanel.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/GatewayReconPanel.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/GatewaySummary.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/detail.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/index.jsx` [NEW]
  - `frontend/src/app/pages/finance/gateway/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/settings/sections/Finance.jsx`
  - `frontend/src/app/router/finance/gateway.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Model Transaksi Payment Gateway (`financeGatewayTransaction.model.js`)**:
    - Merancang skema Mongoose untuk merekam seluruh riwayat transaksi pembayaran online dari iPaymu secara mendalam: Session ID, Transaksi ID iPaymu, referensi nomor tagihan (`invoice_id`), nominal bruto, biaya potongan gateway (fee), dana bersih yang masuk kas, metode pembayaran (VA, QRIS, gerai ritel, e-wallet), status transaksi di sisi gateway, dan status pembukuan kas/buku besar internal (`unbooked`, `settled`, `mismatched`).
  - **Logika Bisnis & Pembukuan Otomatis Webhook Callback (`financeGateway.service.js` & `paymentGateway.service.js`)**:
    - Mengamankan penerimaan webhook callback iPaymu dengan secret token validasi dan verifikasi status ulang langsung ke API iPaymu guna mencegah pemalsuan status transaksi.
    - Pembukuan otomatis saat pembayaran sukses diterima: otomatis membuat record pembayaran tagihan (`financePayment`), memotong sisa tagihan, membuat jurnal mutasi buku besar kas/bank (pendapatan diterima dan beban biaya administrasi bank/gateway), serta secara otomatis membuka kembali layanan pelanggan yang terisolir (_auto-unblock/restore service_).
    - Memperbaiki penanganan tagihan cicilan: tagihan yang telah dicicil sebagian secara manual kini dapat dilunasi secara online dengan benar tanpa merusak saldo buku besar.
    - Tombol sinkronisasi dan cek ulang status transaksi (_re-check status_) yang aman dan idempotent tanpa risiko pencatatan ganda.
  - **Rekonsiliasi Pembayaran Online Terpadu (`GatewayReconPanel.jsx`)**:
    - Mengembangkan panel rekonsiliasi data: sistem menarik seluruh mutasi mutakhir dari server iPaymu lalu membandingkannya dengan database internal, mengelompokkannya menjadi transaksi cocok (_matched_), belum terbukukan (_unbooked_), atau tidak ditemukan di iPaymu (_missing_), dilengkapi tombol aksi pembukuan langsung (_one-click settle_).
  - **Background Worker Sweep 15 Menit (`cron-worker` - `financeGatewaySweep.js`)**:
    - Membuat background job processor `financeGatewaySweep.js` yang berjalan setiap 15 menit untuk menyisir transaksi berstatus pending/unbooked ke API iPaymu sebagai jaring pengaman jika webhook notifikasi dari gateway sempat terhambat atau gagal terkirim.
  - **Frontend Modul Payment Gateway (`frontend`)**:
    - Menambahkan menu navigasi baru **Payment Gateway** (`/finance/gateway`) pada kelompok menu Keuangan.
    - Halaman utama (`index.jsx`) dengan TanStack Table, kartu statistik ringkasan (`GatewaySummary.jsx`) yang menampilkan saldo merchant iPaymu, total potongan fee gateway, dan peringatan transaksi belum terbukukan.
    - Panel daftar kanal pembayaran (`GatewayChannelsPanel.jsx`) yang menampilkan status keaktifan, status gangguan saluran bank/gerai, dan persentase biaya potongan per kanal.
    - Halaman detail transaksi (`detail.jsx`) dengan jejak audit webhook callback iPaymu, rincian alokasi tagihan, dan tautan ke entri jurnal buku besar.
    - Pengaturan webhook secret dan kredensial API iPaymu pada menu **Pengaturan** > **Keuangan** (`Finance.jsx`).
    - Suite integration test komprehensif `financeGateway.callback.test.js` dan `financeGateway.settle.test.js` serta kelengkapan kamus bahasa ID & EN.

---

## 🌿 Branch: `issue-227` — Modul Manajemen & Pemantauan Perangkat Jaringan (Network Devices) dengan Microservice Monitoring Probe (Ping, SNMP, Subnet Scanner) & Auto-Sync Cron

### 📌 Informasi Issue

- **Nomor Issue**: #227
- **Judul Issue**: Modul Manajemen & Pemantauan Perangkat Jaringan (Network Devices) dengan Microservice Monitoring Probe (Ping, SNMP, Subnet Scanner) & Auto-Sync Cron
- **Status Branch**: `Sudah di-merge` (ke `master` via commit `d21c4a0` / `cef1189`)

### 📅 Rincian Commit

#### [d21c4a0] / [cef1189] - resolve #227 - 20 Agt 2026 10:48

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `package.json`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/internal.controller.js` [NEW]
  - `backend/src/controllers/networkDevice.controller.js` [NEW]
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-227.json` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/locationPoint.model.js`
  - `backend/src/models/networkDevice.model.js` [NEW]
  - `backend/src/routes/internal.route.js` [NEW]
  - `backend/src/routes/networkDevice.route.js` [NEW]
  - `backend/src/services/locationPoint.service.js`
  - `backend/src/services/networkDevice.service.js` [NEW]
  - `backend/src/utils/data-table.js`
  - `backend/test/integration/networkDevice.service.test.js` [NEW]
  - `cron-worker/src/jobs/processors/networkDeviceSync.js` [NEW]
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `frontend/src/app/navigation/networks.js`
  - `frontend/src/app/pages/network/devices/components/SnmpWalkSection.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/create.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/detail.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/discover.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/edit.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/index.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/schema/createSchema.js` [NEW]
  - `frontend/src/app/pages/network/devices/schema/editSchema.js` [NEW]
  - `frontend/src/app/pages/network/sites/schema/SiteDetailDrawer.jsx`
  - `frontend/src/app/router/network/networkDeviceRoute.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/Badge.jsx`
  - `frontend/src/components/shared/form/Combobox.jsx`
  - `frontend/src/components/shared/table/SelectedRowsActions.jsx`
  - `frontend/src/components/shared/table/Table.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `network-monitor/.env.example` [NEW]
  - `network-monitor/eslint.config.js` [NEW]
  - `network-monitor/package.json` [NEW]
  - `network-monitor/package-lock.json` [NEW]
  - `network-monitor/src/app.js` [NEW]
  - `network-monitor/src/config/env.js` [NEW]
  - `network-monitor/src/controllers/probe.controller.js` [NEW]
  - `network-monitor/src/middlewares/auth.middleware.js` [NEW]
  - `network-monitor/src/routes/probe.route.js` [NEW]
  - `network-monitor/src/server.js` [NEW]
  - `network-monitor/src/services/ping.service.js` [NEW]
  - `network-monitor/src/services/scanner.service.js` [NEW]
  - `network-monitor/src/services/snmp.service.js` [NEW]
  - `network-monitor/src/utils/logger.js` [NEW]
  - `network-monitor/src/utils/validate.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Penyelesaian & Integrasi Microservice `network-monitor`**:
    - Membangun dan menguji service Node.js mandiri untuk mengeksekusi ICMP Ping, SNMP polling (v1, v2c, v3), dan subnet crawler/scanner secara terisolasi.
    - Mengintegrasikan pengukuran performa perangkat (latensi ping, packet loss, bandwidth antarmuka, beban CPU/RAM, suhu mesin, voltase, frekuensi & sinyal wireless).
    - Menjamin keamanan endpoint probe dengan API Key (`x-api-key`), validasi skema input, logging Winston, dan Swagger JSDoc.
  - **Backend Core & Database Modul Perangkat Jaringan**:
    - Schema Mongoose `networkDevice.model.js` dengan asosiasi ke Node/Site POP (`locationPoint`), IP Address, MAC Address, kredensial SNMP, dan sensor telemetri.
    - Service `networkDevice.service.js` & controller `networkDevice.controller.js` untuk CRUD, query datatable dengan filter status & tipe dinamis, trigger probe on-demand, subnet auto-discovery, dan inspeksi pohon OID SNMP Walk.
    - Endpoint internal `internal.route.js` untuk auto-sync dari background worker `cron-worker` (`networkDeviceSync.js`).
    - Hak akses RBAC `networkDevice.*` pada `privilege.json` serta penambahan integration test `networkDevice.service.test.js` dan rilis changelog `issue-227.json` (v1.48.0).
  - **Frontend UI & Fitur Interaktif (`frontend`)**:
    - Menu navigasi **Network** > **Devices** (`/networks/devices`) berbasis TanStack Table dengan kartu status ringkasan dan aksi massal.
    - Formulir tambah/edit dengan dropdown tipe dinamis dan konfigurasi kredensial SNMP.
    - Dashboard detail perangkat (`detail.jsx`) dengan visualisasi uptime, riwayat latensi, sensor mesin, status port interface, dan komponen interaktif `SnmpWalkSection.jsx`.
    - Fitur Auto-Discovery (`discover.jsx`) untuk memindai subnet IP dan mengimpor perangkat secara massal.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                                                                   | Dampak Utama                                                                                                                                                                                |
| ----- | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #230  | Modul Payment Gateway iPaymu (Pemantauan Transaksi, Saldo Merchant, Rekonsiliasi, Settlement & Sweep)                   | Menghadirkan otomatisasi pencatatan pembayaran online iPaymu, verifikasi keamanan webhook, rekonsiliasi mutasi gateway, pembukuan kas/biaya bank, dan pemulihan otomatis layanan terisolir. |
| #227  | Modul Manajemen & Pemantauan Perangkat Jaringan (Network Devices) dengan Microservice Monitoring Probe & Auto-Sync Cron | Menyediakan modul inventarisasi dan monitoring perangkat jaringan lengkap dengan microservice khusus probe (ping, SNMP, scanner) dan auto-sync status berkala.                              |

### Kemampuan Baru Pengguna/Admin

- **Pemantauan Pembayaran Online iPaymu Real-Time**: Tim Keuangan dapat memantau seluruh transaksi masuk dari iPaymu, melihat saldo dana merchant yang siap ditarik, total potongan fee gateway, dan memeriksa kanal pembayaran yang aktif/terganggu.
- **Rekonsiliasi Otomatis & One-Click Settlement**: Menarik mutasi dari iPaymu secara langsung untuk mencocokkan data tagihan dan mengeksekusi pembukuan kas pada pembayaran yang belum terbukukan dalam satu klik.
- **Auto-Restore Layanan Terisolir**: Pelanggan yang melunasi tagihan tertunggak melalui kanal pembayaran online akan otomatis dipulihkan status layanannya seketika tanpa memerlukan tindakan manual admin.
- **Monitoring & Discovery Perangkat Jaringan**: Teknisi NOC dapat mendata perangkat jaringan, memindai subnet IP (Auto-Discovery), memantau sensor temperatur/CPU/RAM/sinyal radio, dan menelusuri pohon OID SNMP secara langsung dari web.

### Bug Fix / Solusi Masalah

- **Pencegahan Transaksi Terlewat (iPaymu)**: Pembayaran online kini otomatis dibukukan melalui webhook callback yang diverifikasi serta disisir ulang oleh background sweep setiap 15 menit, mengeliminasi masalah pembayaran yang tidak tercatat saat pelanggan menutup halaman sebelum redirect.
- **Pencatatan Beban Administrasi Gateway**: Biaya potongan fee gateway kini otomatis dicatat sebagai Beban Administrasi Bank, mencegah timbulnya selisih piutang yang tidak dapat ditutup.
- **Penanganan Pelunasan Tagihan Cicilan**: Pembayaran online untuk tagihan yang sebelumnya telah dicicil sebagian secara manual kini teralokasi dan tercatat secara akurat pada buku besar.
- **Isolasi Beban Jaringan**: Beban komputasi eksekusi raw ping, SNMP walk, dan pemindaian subnet dialihkan ke microservice mandiri `network-monitor`, menjaga stabilitas performa API backend utama.

### Menu/Fitur Baru

- **Menu Keuangan > Payment Gateway (`/finance/gateway`)**: Halaman pemantauan transaksi online, saldo merchant, kanal pembayaran, dan rekonsiliasi iPaymu.
- **Menu Jaringan > Devices (`/networks/devices`)**: Halaman inventarisasi perangkat jaringan, dashboard kesehatan perangkat, penelusuran SNMP Walk, dan Auto-Discovery scanner.
- **Fitur Auto-Discovery Subnet (`/networks/devices/discover`)**: Pemindaian subnet IP (CIDR) dan impor massal perangkat aktif.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Modul Payment Gateway iPaymu (Rekonsiliasi & Pemantauan)

- **Penjelasan Fitur**:
  - Modul Payment Gateway mengintegrasikan layanan iPaymu dengan sistem akuntansi dan penagihan ISPF V2. Setiap transaksi yang masuk melalui Virtual Account, QRIS, gerai ritel, atau e-wallet akan diverifikasi keasliannya dan dibukukan langsung ke buku besar, memotong piutang pelanggan, mencatat beban fee, dan menyalakan kembali akun yang sempat diisolir.

- **Langkah Penggunaan (Tutorial - Melakukan Rekonsiliasi & Settlement Transaksi Online)**:
  1. Masuk ke Dashboard Admin, buka menu **Keuangan** > **Payment Gateway**.
  2. Periksa kartu ringkasan di bagian atas untuk melihat **Saldo Merchant iPaymu**, **Total Biaya Gateway**, dan status **Transaksi Belum Terbukukan**.
  3. Buka tab **Rekonsiliasi**, pilih rentang tanggal transaksi, lalu klik **Tarik Data iPaymu**.
  4. Sistem akan menampilkan perbandingan transaksi antara database internal dan data server iPaymu:
     - **Cocok (Matched)**: Transaksi yang sudah terbukukan sempurna di kas dan tagihan.
     - **Belum Terbukukan (Unbooked)**: Dana yang sudah diterima iPaymu namun pembukuannya belum tercatat di sistem (misal karena jaringan webhook terputus).
     - **Tidak Ditemukan (Missing)**: Transaksi di sistem yang tidak tercatat di server iPaymu.
  5. Pada daftar **Belum Terbukukan**, klik tombol **Settle / Bukukan Sekarang** untuk mengeksekusi pencatatan kas, pelunasan tagihan, dan jurnal secara instan.

### 2. Modul Manajemen Perangkat Jaringan (Network Devices & Probe)

- **Penjelasan Fitur**:
  - Modul Perangkat Jaringan menghubungkan inventaris fisik perangkat dengan monitoring telemetri real-time via ICMP Ping dan SNMP melalui microservice `network-monitor`.

- **Langkah Penggunaan (Tutorial - Menambah & Memindai Perangkat)**:
  1. Buka menu **Network** > **Devices**.
  2. Untuk menambah manual: klik **Tambah Perangkat**, masukkan nama, IP, tipe, lokasi POP/Node, dan kredensial SNMP, lalu klik **Simpan**.
  3. Untuk memindai jaringan: klik **Discover**, masukkan subnet target (contoh `192.168.10.0/24`), klik **Mulai Pindai**, lalu pilih perangkat yang ditemukan dan klik **Impor Terpilih**.
  4. Buka detail perangkat untuk melihat grafik latensi ping, status port interface, sensor suhu/CPU, atau lakukan inspeksi OID pada bagian **SNMP Walk**.
