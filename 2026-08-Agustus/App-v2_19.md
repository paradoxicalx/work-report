# 📝 Daily Work Report - Dedy (2026-08-19)

---

## 📅 Laporan Harian - 19 Agustus 2026

---

## 🌿 Branch: `issue-227` — Network Device Management, SNMP Walk Engine & Multi-Vendor OID Diagnostics

### 📌 Informasi Issue

- **Nomor Issue**: #227
- **Judul Issue**: Network Device Management, SNMP Walk Engine & Multi-Vendor OID Diagnostics
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit & Perubahan Lokal

#### [WIP / Uncommitted] - Pekerjaan Dalam Proses - Wed Aug 19 2026

- **Komponen yang Berubah**:
  - `backend/src/controllers/networkDevice.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/networkDevice.model.js`
  - `backend/src/routes/networkDevice.route.js`
  - `backend/src/services/networkDevice.service.js`
  - `backend/test/integration/networkDevice.service.test.js`
  - `frontend/src/app/pages/network/devices/create.jsx`
  - `frontend/src/app/pages/network/devices/detail.jsx`
  - `frontend/src/app/pages/network/devices/edit.jsx`
  - `frontend/src/app/pages/network/devices/index.jsx`
  - `frontend/src/app/pages/network/devices/components/SnmpWalkSection.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/schema/columns.jsx`
  - `frontend/src/app/pages/network/devices/schema/createSchema.js`
  - `frontend/src/components/shared/table/SelectedRowsActions.jsx`
  - `frontend/src/components/shared/table/Table.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `network-monitor/src/controllers/probe.controller.js`
  - `network-monitor/src/routes/probe.route.js`
  - `network-monitor/src/services/snmp.service.js`
- **Deskripsi Perubahan & Fungsi**:
  - **Pembangunan Mesin SNMP Walk Multi-Vendor Terisolasi (`network-monitor`)**: Mengembangkan engine `snmpWalk` canggih pada microservice `network-monitor` yang mampu memindai seluruh pohon OID (*OID tree traversal*) secara rekursif. Engine ini dilengkapi pengenalan otomatis (*auto-detection*) untuk berbagai vendor perangkat jaringan terkemuka:
    - **CPU Load**: MikroTik RouterOS, Cisco IOS/Catalyst/Nexus, Huawei VRP, Ubiquiti airMAX/EdgeMAX, BDCOM OLT/Switch, VSOL/HSGQ GPON/EPON OLT, Linux UCD-SNMP & Host-Resources-MIB.
    - **Sensor Suhu (Temperature)**: MikroTik Health OID, Cisco ENV-MIB, Huawei Entity-MIB, ZTE ZXA10, VSOL, BDCOM, dan Linux LMSensors.
    - **Sensor Tegangan Listrik (Voltage)**: MikroTik Health OID, Cisco ENV-MIB, dan Standard Entity-MIB.
    - **Radio Wireless & Frekuensi**: MikroTik Wireless MIB (frekuensi & SSID per radio), Ubiquiti airMAX, Cambium ePMP, Mimosa PtP, serta TP-Link Pharos.
    - **Antarmuka Jaringan (Interfaces)**: Pemindaian IF-MIB (`1.3.6.1.2.1.2`) dan `ifXTable` (`1.3.6.1.2.1.31`), termasuk konversi format MAC address heksadesimal, status operasional antarmuka (UP / DOWN / DISABLED), serta pemetaan OID in/out octets dan error traffic.
    - **Penjelajah OID Mentah (Raw OID Tree Explorer)**: Menghasilkan pohon OID lengkap dengan konversi tipe data (buffer, number, string, hex MAC) untuk kebutuhan analisis lanjutan.
  - **Penyediaan Endpoint Internal & Route SNMP Walk (`backend`)**:
    - Menambahkan controller `walkDeviceSnmp` dan rute `POST /network-devices/snmp-walk` (dengan privilege `networkDevice.read`).
    - Menambahkan fungsi service `walkDeviceSnmp` yang berkomunikasi ke `network-monitor` via internal API key.
    - Menyesuaikan fungsi `testDeviceProbe` agar langsung memicu endpoint `/probe/ping` pada `network-monitor`.
  - **Aksi Hapus Massal Perangkat Jaringan (Bulk Soft Delete)**:
    - Menambahkan rute `DELETE /network-devices/multiple` dan service `deleteMultipleNetworkDevices` dengan audit log admin.
    - Menambahkan unit/integration test pada `backend/test/integration/networkDevice.service.test.js` untuk memverifikasi fungsionalitas penghapusan massal.
  - **Pembaruan Komponen Tabel Global (`SelectedRowsActions.jsx` & `Table.jsx`)**:
    - Menambahkan kapabilitas tombol **Hapus Massal** pada bilah aksi baris terpilih (*SelectedRowsActions*) dengan modal konfirmasi `ConfirmModal`, validasi hak akses pengguna (`multiDeletePrivilege`), serta pemanggilan fungsi mutasi dan reload tabel otomatis.
  - **Komponen Visual Baru `SnmpWalkSection.jsx` di Frontend**:
    - Membangun antarmuka interaktif pemindaian SNMP Walk yang dapat digunakan pada halaman Tambah (`create.jsx`), Ubah (`edit.jsx`), dan Detail Perangkat (`detail.jsx`).
    - Dilengkapi **Preset Vendor Cepat** (MikroTik, Ubiquiti, Cisco, Huawei, ZTE, VSOL, BDCOM, TP-Link, Linux Server) untuk mengisi OID standar dalam sekali klik.
    - Tabulasi tampilan: **Sensor Hardware**, **Wireless & Frekuensi**, **Antarmuka Jaringan**, dan **Penjelajah OID Mentah** dengan fitur pencarian OID real-time.
    - Tombol **"Terapkan Pemetaan OID"** yang langsung memetakan OID CPU, Suhu, Tegangan, Frekuensi, SSID, dan antarmuka jaringan yang dipilih ke formulir konfigurasi perangkat tanpa perlu salin-tempel manual.
  - **Penyempurnaan Skema Kolom Tabel & Form Perangkat Jaringan**:
    - Memperbaiki helper sel TanStack Table (`columns.jsx` & `rows.jsx`) agar tahan terhadap variasi schema data (`node`/`group`, `device_type`/`type`, `device_id`/`_id`, serta parsing aman untuk angka CPU, suhu, tegangan, dan latensi).
    - Menambahkan cell helper `NetworkDeviceNameCell` dan `NetworkDeviceIpCell`.
    - Menyelaraskan teks multibahasa pada file `translations.json` (ID & EN).

#### [0543087] - save #227 - Tue Aug 18 18:53:49 2026

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/internal.controller.js`
  - `backend/src/controllers/networkDevice.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/networkDevice.model.js`
  - `backend/src/routes/internal.route.js`
  - `backend/src/routes/networkDevice.route.js`
  - `backend/src/services/networkDevice.service.js`
  - `backend/test/integration/networkDevice.service.test.js`
  - `cron-worker/src/jobs/processors/networkDeviceSync.js`
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `frontend/src/app/navigation/networks.js`
  - `frontend/src/app/pages/network/devices/create.jsx`
  - `frontend/src/app/pages/network/devices/detail.jsx`
  - `frontend/src/app/pages/network/devices/discover.jsx`
  - `frontend/src/app/pages/network/devices/edit.jsx`
  - `frontend/src/app/pages/network/devices/index.jsx`
  - `frontend/src/app/pages/network/devices/schema/columns.jsx`
  - `frontend/src/app/pages/network/devices/schema/createSchema.js`
  - `frontend/src/app/pages/network/devices/schema/editSchema.js`
  - `frontend/src/app/router/network/networkDeviceRoute.jsx`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `network-monitor/.env.example`
  - `network-monitor/eslint.config.js`
  - `network-monitor/package.json`
  - `network-monitor/src/app.js`
  - `network-monitor/src/config/env.js`
  - `network-monitor/src/controllers/probe.controller.js`
  - `network-monitor/src/middlewares/auth.middleware.js`
  - `network-monitor/src/routes/probe.route.js`
  - `network-monitor/src/server.js`
  - `network-monitor/src/services/ping.service.js`
  - `network-monitor/src/services/scanner.service.js`
  - `network-monitor/src/services/snmp.service.js`
  - `network-monitor/src/utils/logger.js`
  - `package.json`
- **Deskripsi Perubahan & Fungsi**:
  - Inisialisasi microservice `network-monitor` (Express v4, `net-snmp`, `ping`, auth API Key).
  - Implementasi CRUD Perangkat Jaringan (Network Devices) dengan integrasi pemantauan Ping & SNMP berkala melalui `cron-worker`.
  - Halaman Auto-Discovery Subnet Scanner (`discover.jsx`) untuk menemukan perangkat aktif dalam rentang CIDR IP tertentu.
  - Kartu statistik pemantauan perangkat (Total, Online, Offline, Warning, Average Latency).

---

## 🌿 Branch: `issue-217` — Modul Tagihan Pembelian (Expenses / Accounts Payable), Rencana Anggaran (Budgeting) & Permintaan Belanja (Purchase Requests)

### 📌 Informasi Issue

- **Nomor Issue**: #217
- **Judul Issue**: Modul Tagihan Pembelian, Rencana Anggaran & Permintaan Belanja (Expenses / AP, Budgeting & Purchase Requests)
- **Status Branch**: `Sudah di-merge ke master`

### 📅 Rincian Commit

#### [46f6b28] - update changelog - Wed Aug 19 10:27:54 2026

- **Komponen yang Berubah**:
  - `backend/src/data/changelog/releases/issue-206.json` [NEW]
  - `backend/src/data/changelog/releases/issue-216.json` [NEW]
  - `backend/src/data/changelog/releases/issue-217.json` [NEW]
  - `backend/src/data/changelog/releases/issue-219.json` [NEW]
  - `backend/src/data/changelog/releases/issue-220.json` [NEW]
  - `backend/src/data/changelog/releases/issue-85.json`
  - `backend/src/data/changelog/releases/issue-87.json`
  - `backend/src/data/changelog/releases/issue-88.json`
  - `backend/src/data/changelog/releases/issue-90.json`
  - `backend/src/data/changelog/releases/issue-92.json`
  - `backend/src/data/changelog/releases/issue-98.json`
  - `backend/src/services/changelog.service.js`
- **Deskripsi Perubahan & Fungsi**:
  - Pembaruan berkas catatan rilis versi aplikasi (Changelog Releases) untuk rilis fitur `#206` (v1.44.1), `#220` (v1.44.2), `#219` (v1.45.0), `#216` (v1.46.0), dan `#217` (v1.47.0).
  - Standarisasi struktur data changelog per-file untuk menjaga kecepatan load data dan integrasi API catatan rilis.

#### [521787b] - resolve #217 - Wed Aug 19 10:21:39 2026

- **Komponen yang Berubah**:
  - `backend/src/config/privilege.json`
  - `backend/src/models/financeBudgeting.model.js` [NEW]
  - `backend/src/models/financeExpense.model.js` [NEW]
  - `backend/src/models/financeExpenseOrder.model.js` [NEW]
  - `backend/src/models/financeJournal.model.js`
  - `backend/src/models/financeLogs.model.js`
  - `backend/src/routes/financeBudgeting.route.js` [NEW]
  - `backend/src/routes/financeExpense.route.js` [NEW]
  - `backend/src/routes/financeExpenseOrder.route.js` [NEW]
  - `backend/src/services/financeBudgeting.service.js` [NEW]
  - `backend/src/services/financeExpense.service.js` [NEW]
  - `backend/src/services/financeExpenseOrder.service.js` [NEW]
  - `backend/src/services/financeJournal.service.js`
  - `backend/src/services/financeCoa.service.js`
  - `backend/src/utils/finance-error.js` [NEW]
  - `backend/test/integration/financeBudgeting.approve.test.js` [NEW]
  - `backend/test/integration/financeBudgeting.create.test.js` [NEW]
  - `backend/test/integration/financeBudgeting.rejectRevise.test.js` [NEW]
  - `backend/test/integration/financeExpense.auditFixes.test.js` [NEW]
  - `backend/test/integration/financeExpense.bulkPay.test.js` [NEW]
  - `backend/test/integration/financeExpense.create.test.js` [NEW]
  - `backend/test/integration/financeExpense.journalAudit.test.js` [NEW]
  - `backend/test/integration/financeExpense.payment.race.test.js` [NEW]
  - `backend/test/integration/financeExpense.payment.test.js` [NEW]
  - `backend/test/integration/financeExpense.paymentVoid.test.js` [NEW]
  - `backend/test/integration/financeExpenseAP.journal.test.js` [NEW]
  - `backend/test/integration/financeExpenseAttachments.test.js` [NEW]
  - `backend/test/integration/financeExpenseFromBudgeting.test.js` [NEW]
  - `backend/test/integration/financeExpenseFromVendorPO.test.js` [NEW]
  - `backend/test/integration/financeExpenseOrder.listDetail.test.js` [NEW]
  - `backend/test/integration/financeExpenseOrder.multiBill.test.js` [NEW]
  - `backend/test/integration/financeExpensePayableAging.test.js` [NEW]
  - `backend/test/integration/financeExpenseV1Compat.test.js` [NEW]
  - `backend/test/integration/financeJournal.model.test.js` [NEW]
  - `frontend/src/app/navigation/finance.js`
  - `frontend/src/app/pages/finance/budgeting/BudgetingDocument.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/BudgetingDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/BudgetingItemsField.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/detail.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/index.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/schema/budgetingSchema.js` [NEW]
  - `frontend/src/app/pages/finance/budgeting/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/BillPaymentDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/BulkExpensePaymentDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/ExpenseAttachmentsSection.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/ExpenseBillPaymentReceiptDocument.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/ExpenseBillPaymentReceiptModal.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/ExpenseDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/ExpenseItemsField.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/detail.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/index.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/schema/expenseSchema.js` [NEW]
  - `frontend/src/app/pages/finance/expenses/schema/paymentSchema.js` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/PurchaseRequestDrawer.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/PurchaseRequestItemsField.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/detail.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/index.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/finance/purchase-requests/schema/purchaseRequestSchema.js` [NEW]
  - `frontend/src/app/pages/finance/reports/InputTaxRecap.jsx` [NEW]
  - `frontend/src/app/pages/finance/reports/PayableAging.jsx` [NEW]
  - `frontend/src/app/router/finance/budgeting.jsx` [NEW]
  - `frontend/src/app/router/finance/expenses.jsx` [NEW]
  - `frontend/src/app/router/finance/purchaseRequests.jsx` [NEW]
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Modul Tagihan Pembelian / Utang Usaha (Expenses / Accounts Payable)**:
    - Manajemen siklus lengkap tagihan belanja operasional dan vendor: Draf, Menunggu Persetujuan, Disetujui (Approved), Dibayar Sebagian (Partially Paid), Lunas (Paid), Dibatalkan (Voided).
    - Otomatisasi pengakuan jurnal akuntansi Akrual Utang Usaha (*Debit Biaya / Aset / PPN Masukan, Kredit Utang Usaha*) seketika tagihan disetujui.
    - Pembayaran multi-termin dan pelunasan bertahap dengan integrasi mutasi kas/bank (*Debit Utang Usaha, Kredit Kas/Bank*).
    - Fitur **Pembayaran Tagihan Massal (Bulk Payment)** untuk membayar banyak tagihan vendor dalam satu transaksi kas.
    - Fitur **Pembatalan Pembayaran (Payment Void)** dengan pembalikan jurnal kas otomatis dan pengembalian sisa tagihan secara akurat.
    - Pencetakan Kuitansi Bukti Pembayaran Tagihan Pembelian (*Expense Bill Payment Receipt*).
    - Manajemen lampiran file bukti transaksi dan faktur pajak dengan pratinjau langsung.
  - **Modul Rencana Anggaran (Budgeting)**:
    - Penyusunan pos rencana anggaran biaya (RAB) departemen/proyek dengan multi-item dan akun COA terkait.
    - Alur persetujuan bertingkat (Draf → Diajukan → Disetujui / Ditolak / Perlu Revisi).
    - Pembuatan tagihan belanja langsung dari item anggaran yang telah disetujui (*Expense from Budgeting*), disertai validasi saldo pagu anggaran tersisa secara *real-time*.
    - Pratinjau dan cetak dokumen resmi Rencana Anggaran (*Budgeting Document*).
  - **Modul Permintaan Belanja (Purchase Requests)**:
    - Formulir pengajuan kebutuhan barang/jasa bagi staf lapangan/kantor sebelum diproses menjadi tagihan pembelian.
    - Pelacakan status pengajuan dari Draf, Diajukan, hingga Disetujui menjadi Tagihan Pembelian (Expense Order).
  - **Laporan Finansial Baru**:
    - **Laporan Umur Utang Usaha (Payable Aging Report)**: Analisis pengelompokan utang usaha berdasarkan rentang jatuh tempo (Belum Jatuh Tempo, 1-30 hari, 31-60 hari, 61-90 hari, >90 hari).
    - **Laporan Rekapitulasi Pajak Masukan (Input Tax Recap)**: Rekapitulasi PPN Masukan dari tagihan pembelian untuk pelaporan SPT Masa PPN.
  - **Pengujian Integrasi Komprehensif**: Menulis 13 berkas integration test untuk memvalidasi seluruh skenario bisnis kritis, termasuk pencegahan race condition saat pembayaran simultan, audit trail jurnal, dan kompatibilitas data V1.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #227  | Network Device Management, SNMP Walk Engine & Multi-Vendor OID Diagnostics | Memungkinkan teknisi memindai seluruh pohon OID SNMP perangkat secara langsung, mendeteksi sensor hardware (CPU, suhu, tegangan, frekuensi, SSID, antarmuka) multi-vendor, memetakan OID otomatis ke form konfigurasi, serta melakukan penghapusan massal perangkat jaringan. |
| #217  | Modul Tagihan Pembelian (Expenses/AP), Rencana Anggaran & Permintaan Belanja | Menyediakan sistem akuntansi utang usaha (AP) otomatis, kontrol anggaran operasional perusahaan (Budgeting), pengajuan belanja (Purchase Request), pembayaran massal tagihan, serta laporan umur utang dan rekap PPN masukan. |

### Kemampuan Baru Pengguna/Admin

- **Teknisi & Administrator Jaringan**:
  - Dapat menjalankan **SNMP Walk** langsung ke alamat IP perangkat target (Router, Switch, OLT, Radio PtP Wireless) untuk melihat seluruh OID yang aktif dan nilai bacanya.
  - Dapat memilih preset OID vendor siap pakai (MikroTik, Ubiquiti, Cisco, Huawei, ZTE, VSOL, BDCOM, TP-Link, Linux) atau memilih sensor yang terdeteksi dari hasil walk.
  - Dapat menerapkan pemetaan OID sensor (CPU, Suhu, Tegangan, Frekuensi, SSID) ke konfigurasi perangkat dalam sekali klik tanpa perlu mencari dan mengetik manual string OID yang panjang.
  - Dapat memilih beberapa perangkat sekaligus di tabel utama dan melakukan **Hapus Massal (Bulk Soft Delete)** secara aman.
- **Divisi Keuangan (Finance) & Manajemen**:
  - Dapat mencatat dan memverifikasi seluruh tagihan pembelian vendor/operasional dengan pengakuan jurnal utang usaha akrual otomatis.
  - Dapat menyusun pos rencana anggaran (Budgeting) dan memonitor realisasi serapan anggaran secara real-time saat tagihan diterbitkan.
  - Dapat melakukan pelunasan bertahap maupun pembayaran massal (*Bulk Expense Payment*) untuk banyak tagihan vendor sekaligus.
  - Dapat mencetak kuitansi pembayaran resmi, membatalkan transaksi pembayaran yang salah (*Payment Void*) dengan pembalikan jurnal aman, serta mengunduh Laporan Umur Utang (*Payable Aging*) dan Rekap Pajak Masukan.

### Bug Fix / Solusi Masalah

- **Pencegahan Error Skema Kolom Tabel Perangkat Jaringan**: Memperbaiki accessor dan cell renderer pada tabel perangkat jaringan agar kebal terhadap inkonsistensi nama field warisan (`node` vs `group`, `device_type` vs `type`, latensi timeout outlier).
- **Pencegahan Race Condition Pembayaran**: Mencegah duplikasi pencatatan transaksi kas saat tombol simpan pembayaran tagihan ditekan berulang kali secara bersamaan.
- **Penyempurnaan Tampilan Footer Drawer**: Memperbaiki tata letak, padding, dan ketinggian tombol aksi pada footer drawer transaksi keuangan agar proporsional dan mudah diakses di berbagai resolusi layar.

### Menu/Fitur Baru

- **Jaringan > Perangkat Jaringan**:
  - Tombol & Modal Interaktif **SNMP Walk & Deteksi OID** pada halaman Tambah, Edit, dan Detail Perangkat.
  - Opsi Aksi **Hapus Massal** pada bilah tabel perangkat jaringan terpilih.
- **Keuangan > Tagihan Pembelian (Expenses / Utang Usaha)**:
  - Menu pembuatan tagihan, persetujuan akrual, pembayaran bertahap, dan pembayaran massal.
- **Keuangan > Rencana Anggaran (Budgeting)**:
  - Menu penyusunan anggaran, persetujuan hierarki, dan pembuatan tagihan langsung dari pos anggaran.
- **Keuangan > Permintaan Belanja (Purchase Requests)**:
  - Menu pengajuan kebutuhan barang/jasa internal.
- **Keuangan > Laporan**:
  - Laporan Umur Utang Usaha (*Payable Aging Report*).
  - Laporan Rekapitulasi Pajak Masukan (*Input Tax Recap*).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Fitur SNMP Walk & Deteksi OID Perangkat Jaringan (#227)

- **Penjelasan Fitur**: Fitur SNMP Walk memungkinkan administrator jaringan memindai pohon OID perangkat secara langsung melalui microservice `network-monitor`. Sistem akan secara otomatis mengenali OID sensor CPU, suhu, tegangan, frekuensi wireless, SSID, dan antarmuka jaringan dari berbagai vendor (MikroTik, Ubiquiti, Cisco, Huawei, ZTE, VSOL, BDCOM, TP-Link, Linux) dan memetakannya langsung ke formulir konfigurasi perangkat.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Jaringan** > **Perangkat Jaringan**.
  2. Klik tombol **Tambah Perangkat** (atau klik tombol **Ubah** / buka halaman **Detail** pada perangkat yang sudah ada).
  3. Masukkan **Alamat IP** perangkat dan pastikan parameter SNMP (Komunitas SNMP, Port, Versi) sudah sesuai.
  4. Gulir ke seksi **SNMP Walk & Deteksi OID** lalu klik tombol **Jalankan SNMP Walk**.
  5. Sistem akan memindai perangkat dan menampilkan hasil deteksi dalam tab:
     - **Sensor Hardware**: Menampilkan sensor CPU, Suhu, dan Tegangan yang terdeteksi beserta nilai bacaannya saat ini.
     - **Wireless & Frekuensi**: Menampilkan frekuensi radio dan SSID yang terdeteksi (khusus perangkat nirkabel/PtP).
     - **Antarmuka Jaringan**: Menampilkan daftar port/antarmuka (Ethernet, SFP, Wireless) beserta status UP/DOWN, MAC address, dan OID traffic.
     - **Penjelajah OID Mentah**: Menampilkan seluruh pohon OID mentah untuk pencarian fleksibel.
  6. Pilih sensor yang diinginkan (atau klik salah satu **Preset Vendor** seperti *MikroTik RouterOS* / *Ubiquiti airMAX*).
  7. Klik tombol **Terapkan Pemetaan OID**. Nilai OID sensor akan otomatis terisi ke dalam field formulir.
  8. Klik **Simpan** untuk menyimpan data perangkat.

---

### 2. Fitur Alur Tagihan Pembelian (Expenses) & Rencana Anggaran (Budgeting) (#217)

- **Penjelasan Fitur**: Mengintegrasikan rantai pasok dan belanja operasional dari perencanaan (Budgeting), pengajuan (Purchase Request), hingga realisasi tagihan vendor (Expenses/AP) yang otomatis menjurnal pengakuan utang dan mutasi kas pelunasan.
- **Langkah Penggunaan (Tutorial)**:
  1. **Membuat Rencana Anggaran**:
     - Buka menu **Keuangan** > **Rencana Anggaran** > klik **Tambah Anggaran**.
     - Masukkan nama anggaran, periode, dan rincian alokasi biaya per pos akun COA.
     - Simpan dan ajukan persetujuan (*Submit Approval*). Setelah disetujui (*Approved*), anggaran siap digunakan.
  2. **Membuat Tagihan Pembelian dari Anggaran**:
     - Buka detail Rencana Anggaran yang telah disetujui, lalu klik opsi **Buat Tagihan (Create Expense)** pada item anggaran terkait.
     - Sistem otomatis mengisi vendor, deskripsi, pos akun biaya, dan memvalidasi pagu anggaran tersisa.
  3. **Menyetujui Tagihan & Pengakuan Jurnal Akrual**:
     - Buka menu **Keuangan** > **Tagihan Pembelian** > buka dokumen tagihan.
     - Klik **Setujui (Approve)**. Sistem secara otomatis membentuk Jurnal Akrual Utang Usaha (Biaya bertambah, Utang bertambah).
  4. **Melakukan Pembayaran Tagihan**:
     - Pada tagihan yang disetujui, klik **Catat Pembayaran** (atau gunakan **Pembayaran Massal** dari tabel utama).
     - Pilih akun Kas/Bank sumber dana, nominal bayar, dan tanggal bayar.
     - Simpan pembayaran. Sistem akan memperbarui status tagihan menjadi *Lunas (Paid)* atau *Dibayar Sebagian (Partially Paid)*, membentuk jurnal pengeluaran kas, dan menyediakan opsi cetak Kuitansi Pembayaran Resmi.
