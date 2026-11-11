# 📝 Daily Work Report - Dedy (2026-08-30)

---

## 📅 Laporan Harian - 30 Agustus 2026

---

## 🌿 Branch: `issue-183` — Comprehensive Financial Module Enhancement, G3 Accounting Reports & Audit Fixes

### 📌 Informasi Issue

- **Nomor Issue**: #183
- **Judul Issue**: Financial Module Audit & G3 Accounting Standards Compliance — Laporan Arus Kas (Metode Tidak Langsung & Rekonsiliasi Kas Mandiri), Laporan Umur Piutang (AR Aging) & Umur Hutang (AP Aging) Agregasi Berpaginasi, Buku Besar Multi-Akun Siap Cetak (Printable General Ledger), Manajemen Draf Transaksi Terisolasi (Tab & Drawer), Klasifikasi Kategori Arus Kas & Hierarki Kaskade COA, serta Penyelesaian Audit Temuan Akuntansi (B1, B4, B5, B6, B10, C7, C8, C9, C10, A6, G3)
- **Status Branch**: `Belum di-merge` (Active Development / In Progress)

### 📅 Rincian Perubahan Terkini

#### [Working Tree Update] - implement #183 - 30 Agustus 2026, 23:45:00 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/financeReport.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeReport.service.js) — Implementasi Laporan Arus Kas (*Cash Flow Statement*) dan perbaikan struktur Laba-Rugi:
    - [`buildCashFlowStatement()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeReport.service.js): Penyusunan laporan arus kas berbasis metode tidak langsung (*indirect method*). Titik awal adalah Laba Bersih periode berjalan (dari [`buildIncomeStatement()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeReport.service.js)), disesuaikan dengan perubahan saldo akun Neraca non-kas selama rentang tanggal (Aset bertambah = arus kas keluar, Liabilitas/Modal bertambah = arus kas masuk). Dikelompokkan ke dalam 3 aktivitas utama: **Aktivitas Operasi** (*Operating*), **Aktivitas Investasi** (*Investing*), dan **Aktivitas Pendanaan** (*Financing*), serta ember pengaman *Unclassified* untuk akun yang belum terpetakan.
    - **Rekonsiliasi Kas Mandiri**: Menghitung selisih saldo kas awal dan akhir secara independen dari akun `cash_bank` (`actual_cash_change`), lalu memverifikasinya terhadap total perubahan arus kas bersih (`net_cash_change`). Selisih rekonsiliasi (`reconciliation_difference`) dan flag `reconciled` dilaporkan langsung ke UI untuk mendeteksi ketidakseimbangan atau mutasi kas yang lolos klasifikasi.
    - [`buildIncomeStatement()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeReport.service.js): Perbaikan struktur laporan Laba-Rugi dengan memisahkan `other_income` (Pendapatan Lain-lain) dari pendapatan operasional, sehingga tidak mendistorsi Laba Kotor (*Gross Profit*) maupun Laba Operasional (*Operating Profit*) dan baru ditambahkan sebelum Laba Bersih (*Net Profit*) (Temuan C9).
  - [`backend/src/services/financeInvoice.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeInvoice.service.js) — Implementasi engine Laporan Umur Piutang (*Receivable Aging* / AR Aging):
    - [`findReceivableAgingSummary()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeInvoice.service.js): Agregasi ringkasan saldo piutang tak terbayar (`status: 'unpaid'`, `to_wallet: { $ne: true }`, `deleted: { $ne: true }`) langsung di tingkat MongoDB pipeline ke dalam 4 ember umur berdasarkan `due_date`: `0-30 hari`, `31-60 hari`, `61-90 hari`, dan `>90 hari`.
    - [`findReceivableAgingForTable()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeInvoice.service.js): Penyajian daftar detail faktur piutang berpaginasi menggunakan utilitas `dataTable`, mengeliminasi pemuatan ribuan dokumen ke memori Node.js dan menghitung sisa hari jatuh tempo (`days_overdue`) secara efisien per halaman.
  - [`backend/src/services/financeExpense.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeExpense.service.js) — Refactoring Laporan Umur Hutang (*Payable Aging* / AP Aging):
    - [`findPayableAgingSummary()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeExpense.service.js): Penggantian mekanisme in-memory loop lama dengan agregasi MongoDB murni untuk pengelompokan sisa hutang tagihan pembelian berstatus `approved`/`partial` ke dalam ember umur `0-30`, `31-60`, `61-90`, dan `90+`.
    - [`findPayableAgingForTable()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeExpense.service.js): Endpoint tabel berpaginasi dengan pencarian vendor dan pengurutan dinamis.
  - [`backend/src/utils/aging-bucket.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/aging-bucket.js) [NEW] — Utilitas terpusat untuk klasifikasi ember umur piutang dan hutang (`agingBucket`), memastikan keseragaman batas interval waktu antara query agregasi database dan formatting pada antarmuka.
  - [`backend/src/services/financeCoa.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeCoa.service.js) — Penyempurnaan buku besar multi-akun, hierarki COA, dan audit integritas:
    - [`buildMultiCoaLedger()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeCoa.service.js): Penyusunan laporan Buku Besar gabungan untuk banyak akun perkiraan sekaligus dalam satu rentang tanggal, mendukung normalisasi seragam antara mutasi akun kas (`finance_logs`) dan non-kas (`lines` jurnal), dengan batasan kuota baris (`MULTI_LEDGER_MAX_LINES_PER_ACCOUNT = 2000`) dan indikator pemotongan data (`truncated: true`).
    - [`findOpeningBalanceConflicts()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeCoa.service.js): Alat audit pendeteksi saldo awal ganda (*dual opening balance conflict*) antara field `opening_balance` pada master COA dan jurnal saldo awal (`source: opening`), ditampilkan sebagai banner peringatan pada laporan Neraca (Temuan C8).
    - [`updateCoaById()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeCoa.service.js) & [`resolveParentCoa()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeCoa.service.js): Perhitungan ulang dan kaskade otomatis field `path` dan `level` ke seluruh akun turunan saat kode akun (`code`) atau induk (`parent`) diperbarui, serta validasi pencegahan struktur hierarki melingkar (*circular parent reference*) (Temuan B4 & B5).
    - [`summarizeCoa()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeCoa.service.js): Penghapusan filter `status: 'active'` agar akun nonaktif yang masih memiliki sisa saldo tetap tercermin akurat dan selaras dengan laporan Neraca (Temuan C7).
    - [`assertCoaNotPendingReferenced()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeCoa.service.js): Validasi pengecekan relasi sebelum menghapus atau menonaktifkan akun COA terhadap transaksi belum dijurnal, jadwal transaksi berulang (`FinanceRecurring`), dan draf transaksi pending (Temuan C10).
  - [`backend/src/services/financeLedger.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeLedger.service.js) & [`backend/src/services/financeJournal.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeJournal.service.js):
    - Penegakan kueri `activeLogsMatch` (`deleted: { $ne: true }`) di seluruh fungsi kalkulasi saldo kas (`getAccountBalance`, `getAccountBalanceMap`, `sumInOutForAccounts`, `sumUnjournaledCash`, `findCashJournalDrift`, `buildLedgerWithRunningBalance`) untuk menjamin konsistensi satu definisi saldo buku besar (Temuan B1).
    - Ekstraksi validasi `assertJournalDateAllowed()` untuk penegakan larangan posting ke periode akuntansi yang terkunci (*closed/locked period*) serta larangan pencatatan tanggal masa depan pada jurnal manual, saldo awal, dan jurnal pembalikan (Temuan B6).
    - Penerapan penolakan pembentukan jurnal saldo awal untuk COA yang sudah memiliki saldo awal terdaftar (`COA_OPENING_BALANCE_ALREADY_SET`) (Temuan C8).
    - Pemetaan pembatalan jurnal `reversed_by` berbasis `leg_key` deterministik untuk mencegah inkonsistensi penunjukan baris jurnal (Temuan A6).
  - [`backend/src/services/financeRecurring.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeRecurring.service.js) — Penggantian syntax `$set: { last_error: undefined }` menjadi operator `$unset: { last_error: '' }` yang valid di Mongoose/MongoDB (Temuan B10).
  - [`backend/src/models/financeCoa.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financeCoa.model.js) & [`backend/src/data/financeCoaSeed.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/financeCoaSeed.json) — Penambahan field `cash_flow_category` (`operating`, `investing`, `financing`) pada skema COA dan pembaruan seed bagan akun standar.
  - [`backend/src/controllers/financeReport.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeReport.controller.js), [`backend/src/controllers/financeInvoice.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeInvoice.controller.js), [`backend/src/controllers/financeExpense.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeExpense.controller.js), [`backend/src/controllers/financeCoa.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeCoa.controller.js), [`backend/src/routes/*`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/):
    - Penambahan rute dan endpoint: `GET /finance/report/cash-flow`, `POST /finance/invoice/receivable-aging/table`, `GET /finance/invoice/receivable-aging/summary`, `POST /finance/expense/payable-aging/table`, `GET /finance/expense/payable-aging/summary`, `POST /finance/coa/multi-ledger`, dan `GET /finance/coa/opening-balance-conflicts`.
  - [`frontend/src/app/pages/finance/reports/CashFlowStatement.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/reports/CashFlowStatement.jsx) [NEW] — Komponen antarmuka Laporan Arus Kas interaktif dengan pemilih rentang tanggal, breakdown per kategori aktivitas, kartu total arus kas bersih, serta status rekonsiliasi kas riil.
  - [`frontend/src/app/pages/finance/reports/ReceivableAging.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/reports/ReceivableAging.jsx) [NEW] — Komponen antarmuka Laporan Umur Piutang dengan kartu metrik ringkasan nominal per rentang jatuh tempo (0-30, 31-60, 61-90, >90 hari) dan tabel detail faktur pelanggan berpaginasi.
  - [`frontend/src/app/pages/finance/reports/PayableAging.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/reports/PayableAging.jsx) — Pembaruan tampilan Umur Hutang dengan kartu ringkasan berbasis endpoint agregasi backend dan integrasi tabel `Datatables`.
  - [`frontend/src/app/pages/finance/reports/LedgerPrint.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/reports/LedgerPrint.jsx) [NEW] — Fitur cetak Buku Besar multi-akun (*General Ledger*) yang memungkinkan admin memilih satu atau seluruh akun perkiraan, menentukan periode tanggal, dan mencetak laporan resmi via stylesheet cetak (`@media print`).
  - [`frontend/src/app/pages/finance/reports/BalanceSheet.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/reports/BalanceSheet.jsx) & [`frontend/src/app/pages/finance/reports/IncomeStatement.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/reports/IncomeStatement.jsx) — Penyempurnaan tampilan Neraca dengan banner deteksi konflik saldo awal ganda dan pemisahan baris Pendapatan Lain-lain pada Laba-Rugi.
  - [`frontend/src/app/pages/finance/transactions/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/transactions/index.jsx), [`frontend/src/app/pages/finance/transactions/TransactionDraftTable.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/transactions/TransactionDraftTable.jsx) [NEW], [`frontend/src/app/pages/finance/transactions/TransactionDraftDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/transactions/TransactionDraftDetailDrawer.jsx) [NEW], [`frontend/src/app/pages/finance/transactions/DraftAccountCell.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/transactions/DraftAccountCell.jsx) [NEW], [`frontend/src/app/pages/finance/transactions/schema/draftColumns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/transactions/schema/draftColumns.jsx) [NEW]:
    - Pemisahan antarmuka draf transaksi dari tabel transaksi utama menjadi struktur tab terdedikasi (`Transaksi` dan `Draf Tertunda`) dengan badge counter jumlah draf pending secara real-time.
    - Drawer detail draf transaksi untuk peninjauan baris akun debit/kredit dan eksekusi persetujuan (*Approve*) atau penolakan (*Reject*) draf transaksi.
  - [`frontend/src/app/pages/finance/coa/CoaDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/coa/CoaDrawer.jsx) & [`frontend/src/app/pages/finance/coa/schema/coaSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/coa/schema/coaSchema.js) — Penambahan dropdown pilihan `cash_flow_category` pada form pembuatan dan pengubahan akun perkiraan.
  - [`backend/test/integration/*`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/):
    - [`financeReport.cashFlow.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeReport.cashFlow.test.js) [NEW] — Pengujian integrasi perhitungan arus kas operasi, investasi, pendanaan, dan verifikasi rekonsiliasi kas.
    - [`financeInvoiceReceivableAging.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeInvoiceReceivableAging.test.js) [NEW] — Pengujian ringkasan agregasi umur piutang dan filtering tabel berpaginasi.
    - [`financeCoa.multiLedger.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeCoa.multiLedger.test.js) [NEW] — Pengujian kompilasi buku besar multi-akun kas dan non-kas siap cetak.
    - [`financeCoa.pathCascade.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeCoa.pathCascade.test.js) [NEW] — Pengujian kaskade perubahan path dan level akun turunan secara otomatis.
    - [`financeExpensePayableAging.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeExpensePayableAging.test.js) & [`financeLedger.postEntries.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/financeLedger.postEntries.test.js) — Pembaruan suite pengujian umur hutang dan pencegahan duplikasi buku besar.
  - [`FINANCE_AUDIT.md`](file:///home/dhedhy/Project/Dekasimal-V2/FINANCE_AUDIT.md) — Pembaruan status checklist temuan audit akuntansi: penyelesaian B1, B4, B5, B6, B10, C7, C8, C9, C10, A6, dan G3 (Arus Kas, Umur Piutang, Buku Besar Cetak).
- **Deskripsi Perubahan & Fungsi**:
  - Menyempurnakan modul keuangan Dekasimal V2 menuju kepatuhan Standar Akuntansi Keuangan (SAK) Indonesia dengan menghadirkan Laporan Arus Kas metode tidak langsung yang terverifikasi mandiri terhadap mutasi kas riil, Laporan Umur Piutang Usaha, dan modul pencetakan Buku Besar komprehensif.
  - Meningkatkan skalabilitas dan performa sistem pada data bervolume tinggi dengan mengalihkan kalkulasi umur piutang/hutang ke agregasi native MongoDB berpaginasi, menggantikan pemrosesan loop di memori backend yang lambat dan memboroskan RAM.
  - Mengisolasi pengelolaan draf transaksi ke dalam tab terpisah agar tidak mengaburkan transaksi resmi di buku besar, serta menutup celah audit terkait saldo ganda, pembalikan jurnal, kaskade hierarki akun, dan penguncian periode akuntansi.

---

## 🌿 Branch: `master` / `issue-245` — Network Traffic Polling, RRD Tool Integration, Interface Discovery & Developer System Logs

### 📌 Informasi Issue

- **Nomor Issue**: #245
- **Judul Issue**: Network Traffic Polling & Monitoring System — Integrasi RRDtool & SNMP Poller, Interface Auto-Discovery, Grafik Trafik Interaktif Multi-Rentang Waktu (1 Jam hingga 1 Tahun), Spike Killer Modal, Simulator Fake Traffic, Tab Developer System Logs dengan Analisis AI Gemini, dan Ekstensi Privilege RBAC
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `31d93de` & `9275fb0`)

### 📅 Rincian Commit

#### [31d93de] / [9275fb0] - resolve #245 - 30 Agustus 2026, 21:43:52 WIB

- **Komponen yang Berubah**:
  - [`network-monitor/src/services/rrd.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/rrd.service.js) & [`network-monitor/src/controllers/rrd.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/controllers/rrd.controller.js) — Implementasi engine penyimpanan data time-series Round Robin Database (RRDtool) untuk monitoring trafik jaringan:
    - Pembuatan dan pemeliharaan file `.rrd` dinamis per target interface perangkat.
    - Struktur arsip RRA bertingkat (resolusi 5 menit untuk 48 jam, 30 menit untuk 14 hari, 2 jam untuk 90 hari, 1 hari untuk 1 tahun) dengan fungsi konsolidasi `AVERAGE` dan `MAX`.
    - Rendering grafik gambar PNG performa tinggi menggunakan binary RRDtool graph dengan kustomisasi warna, batas canvas, label legenda, dan auto-scaling.
    - Implementasi utilitas `spikeKiller` untuk membersihkan data lonjakan (*spike anomaly*) abnormal pada database RRD.
  - [`network-monitor/src/services/trafficPoller.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/trafficPoller.service.js) & [`network-monitor/src/services/snmp.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/snmp.service.js) — Layanan polling SNMP terdistribusi yang mendukung pembacaan counter 64-bit HC (`ifHCInOctets`/`ifHCOutOctets`) dan fallback 32-bit (`ifInOctets`/`ifOutOctets`), deteksi link speed port (`ifHighSpeed`/`ifSpeed`), dan status port operasional/administratif (`ifOperStatus`/`ifAdminStatus`).
  - [`network-monitor/src/services/fakeTraffic.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/services/fakeTraffic.service.js) & [`network-monitor/src/controllers/fakeTraffic.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/network-monitor/src/controllers/fakeTraffic.controller.js) — Generator data trafik simulasi (*Fake Traffic*) dengan berbagai profil beban (Normal Sinusoidal, Work Hours Peak, Video Streaming Burst, DDoS Attack, Heavy Download, Network Outage, Weekend Spike) untuk pengujian beban sistem dan verifikasi visual grafik tanpa memerlukan perangkat fisik aktif.
  - [`cron-worker/src/jobs/processors/networkTrafficPoll.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/processors/networkTrafficPoll.js) & [`cron-worker/src/jobs/scheduler.js`](file:///home/dhedhy/Project/Dekasimal-V2/cron-worker/src/jobs/scheduler.js) — Penjadwalan job polling periodik BullMQ (interval 5 menit) untuk memicu pembaruan metrik trafik seluruh perangkat jaringan yang aktif secara otomatis.
  - [`backend/src/models/networkTrafficTarget.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/networkTrafficTarget.model.js), [`backend/src/services/networkTraffic.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/networkTraffic.service.js), [`backend/src/controllers/networkTraffic.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/networkTraffic.controller.js), [`backend/src/routes/networkTraffic.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/networkTraffic.route.js) — Skema database target trafik, layanan penemuan antarmuka (*interface auto-discovery* via SNMP walk), manajemen konfigurasi grafik, dan perutean API.
  - [`frontend/src/app/pages/network/traffic/*`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/traffic/) — Antarmuka visualisasi monitoring trafik jaringan:
    - Dashboard kartu grafik interaktif (`TrafficGraphCard.jsx`, `RrdZoomableGraph.jsx`) dengan filter rentang waktu cepat (1 jam, 6 jam, 12 jam, 24 jam, 7 hari, 30 hari, 1 tahun).
    - Modal pembesar grafik (`GraphZoomModal.jsx`) untuk inspeksi detail titik data trafik.
    - Drawer auto-discovery interface (`InterfaceDiscoveryDrawer.jsx`) untuk memindai dan menambahkan port jaringan perangkat secara instan.
    - Modal kustomisasi tema grafik (`GraphStyleFields.jsx`, `TrafficSettingsModal.jsx`) dan modal pembersihan spike data (`SpikeKillerModal.jsx`).
  - [`frontend/src/app/pages/settings/sections/developer/logs/*`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/logs/) — Modul log sistem developer terpusat:
    - Tab log sistem (`SystemLogsTab.jsx`) dengan tabel riwayat log Winston, filter level (error, warn, info, debug), dan drawer penampil payload JSON (`LogDetailDrawer.jsx`).
    - Modal analisis akar masalah (*Root Cause Analysis* / RCA) berbasis kecerdasan buatan Gemini AI (`AiAnalysisModal.jsx`) yang memberikan diagnosis teknis instan dan rekomendasi perbaikan saat sistem mencatat log error.
    - Tab kontrol simulator fake traffic (`FakeTrafficTab.jsx`).
  - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json), [`frontend/src/i18n/locales/*`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/), [`backend/src/locales/*`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/) — Ekstensi hak akses RBAC (`networkTraffic.read`, `networkTraffic.create`, `networkTraffic.update`, `networkTraffic.delete`, `developerLog.read`, `developerLog.analyze`) serta lokalisasi kamus bahasa lengkap.
- **Deskripsi Perubahan & Fungsi**:
  - Membangun ekosistem pemantauan trafik jaringan real-time berbasis RRDtool yang mampu merekam throughput data port router/switch (inbound & outbound) secara presisi dengan visualisasi grafik interaktif yang ringan dan cepat.
  - Mempermudah engineer jaringan dalam mendaftarkan port monitoring melalui fitur Interface Auto-Discovery berbasis SNMP walk otomatis.
  - Menyediakan pusat observabilitas sistem melalui Developer System Logs yang terintegrasi dengan Analisis AI Gemini untuk mempercepat penanganan insiden dan debugging error aplikasi.

---

## 🌿 Branch: `issue-247` — Partner API Customer & Partner Documents Upload & Management

### 📌 Informasi Issue

- **Nomor Issue**: #247
- **Judul Issue**: Partner API Document Management — Pengelolaan & Unggah Berkas Dokumen Pelanggan dan Mitra (KTP, NPWP, NIB, Surat Kuasa, Dokumen Legalitas), Validasi Tipe & Ukuran File, Integrasi Storage MinIO, dan Automated Integration Test Suite
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `94fad72` & `30cec4b`)

### 📅 Rincian Commit

#### [94fad72] / [30cec4b] - resolve #247 - 30 Agustus 2026, 12:25:20 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/partnerApiCustomer.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiCustomer.controller.js) & [`backend/src/controllers/partnerApiPartner.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiPartner.controller.js) — Implementasi controller Partner API untuk manajemen dokumen pelanggan dan profil mitra:
    - Endpoint pengunggahan berkas dokumen identitas pelanggan (KTP, NPWP, foto rumah/lokasi pelanggan) dan berkas legalitas mitra (KTP PIC, NPWP Perusahaan, NIB / Izin Usaha, Surat Kuasa).
    - Mekanisme penimpaan berkas lama di MinIO saat dokumen baru diunggah untuk menghemat ruang penyimpanan.
    - Penegakan isolasi akses (*tenant isolation*): partner hanya diizinkan mengunggah atau melihat dokumen milik pelanggan yang terdaftar di bawah naungannya.
  - [`backend/src/routes/partnerApi.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/partnerApi.route.js) — Penambahan rute dokumen pelanggan (`POST /partner-api/customers/:customer_id/documents`) dan dokumen mitra (`POST /partner-api/partners/documents`) yang dilindungi middleware autentikasi API Key Partner dan upload handler Multer.
  - [`backend/src/models/customer.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/customer.model.js) & [`backend/src/models/partner.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/partner.model.js) — Penambahan struktur field dokumen (KTP, NPWP, NIB, Surat Kuasa) lengkap dengan metadata URL MinIO, ukuran berkas, dan tanggal pengunggahan.
  - [`backend/src/controllers/files.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/files.controller.js) — Penguatan penanganan streaming file dan proteksi akses berkas dokumen privat melalui signed URL.
  - [`backend/test/integration/partnerApiCustomer.documents.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/partnerApiCustomer.documents.test.js) [NEW] & [`backend/test/integration/partnerApiPartner.uploadDocuments.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/partnerApiPartner.uploadDocuments.test.js) [NEW] — Rangkaian automated integration tests komprehensif (700+ baris pengujian) yang memvalidasi skenario upload berkas legalitas, validasi format gambar/PDF, penolakan akses lintas mitra (*cross-tenant violation*), dan penghapusan dokumen.
- **Deskripsi Perubahan & Fungsi**:
  - Membuka kapabilitas bagi sistem pihak ketiga / aplikasi mitra (Partner API) untuk mengunggah dan mengelola dokumen verifikasi KYC pelanggan serta berkas legalitas perusahaan secara otomatis dan aman.
  - Menjamin keamanan dan kepatuhan privasi data pelanggan melalui pembatasan akses data berbasis *tenant partner* dan validasi tipe berkas yang ketat.

---

## 🌿 Branch: `issue-233` — Automated Employee Payroll System & Financial Integration

### 📌 Informasi Issue

- **Nomor Issue**: #233
- **Judul Issue**: Automated Employee Payroll System — Engine Perhitungan Gaji Modular, Integrasi BPJS Ketenagakerjaan & BPJS Kesehatan, PPh 21 TER, Lembur & Kehadiran, Posting Jurnal Akuntansi Double-Entry, Slip Gaji Karyawan Siap Cetak, dan Notifikasi Transisi Status Real-time
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `e05a83d`)

### 📅 Rincian Commit

#### [e05a83d] - resolve #233 - 30 Agustus 2026, 01:43:28 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/payrollCalculation.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollCalculation.service.js) — Engine kalkulasi penggajian terintegrasi: gaji pokok, tunjangan multi-komponen, kalkulasi lembur Depnaker (1.5x jam pertama, 2x jam berikutnya), potongan absensi, iuran BPJS TK (JKK, JKM, JHT, JP dengan batas upah maksimum), BPJS Kesehatan, opsi tunjangan BPJS perusahaan (`bpjs_company_borne`), serta kalkulasi pajak penghasilan PPh 21 TER (Kategori A, B, C) sesuai regulasi DJP terbaru.
  - [`backend/src/services/payrollRun.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollRun.service.js) & [`backend/src/services/payrollLedger.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollLedger.service.js) — Alur verifikasi batch penggajian (*Maker-Checker*), mekanisme *Emergency Slip Removal*, dan otomatisasi pencatatan jurnal akuntansi buku besar double-entry seimbang saat gaji dibayarkan (Debet: Beban Gaji & BPJS Perusahaan; Kredit: Kas/Bank, Utang Pajak PPh 21, Utang BPJS, Piutang Kasbon).
  - [`backend/src/services/payrollSlip.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollSlip.service.js) & [`backend/src/services/notification.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/notification.service.js) — Pengelolaan slip individual, pembatalan slip berstatus paid (*void* pembalik kas), serta notifikasi in-app dan Socket.io pada setiap transisi status penggajian.
  - [`frontend/src/app/pages/finance/payroll/*`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/) & [`frontend/src/app/pages/profile/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/profile/index.jsx) — Antarmuka pengguna lengkap: pengaturan payroll dengan simulasi biaya BPJS, dashboard batch payroll run, dokumen slip gaji resmi siap cetak, dan portal mandiri slip gaji pada halaman profil karyawan.
- **Deskripsi Perubahan & Fungsi**:
  - Mengotomatiskan seluruh alur penggajian bulanan karyawan dari kalkulasi komponen gaji, pajak, dan jaminan sosial ketenagakerjaan, hingga pembukuan jurnal akuntansi dan penerbitan slip gaji resmi bagi karyawan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul Modul / Fitur | Dampak Utama |
| :--- | :--- | :--- |
| **#183** | Financial Module Audit & G3 Accounting Standards | Menghadirkan Laporan Arus Kas metode tidak langsung dengan verifikasi rekonsiliasi kas mandiri, Laporan Umur Piutang (AR Aging) & Umur Hutang (AP Aging) berbasis agregasi MongoDB berpaginasi, Buku Besar Multi-Akun siap cetak, tab Draf Transaksi terisolasi, kaskade hierarki COA, dan penutupan 11 temuan audit keuangan (B1, B4, B5, B6, B10, C7, C8, C9, C10, A6, G3). |
| **#245** | Network Traffic Polling & Developer Logs | Monitoring bandwidth port jaringan real-time berbasis RRDtool & SNMP poller otomatis, visualisasi grafik interaktif multi-periode (1h-1y), Interface Auto-Discovery, simulator Fake Traffic, dan dashboard Developer System Logs dengan diagnosis insiden berbasis AI Gemini. |
| **#247** | Partner API Document Management | Penyediaan antarmuka REST API bagi mitra/partner untuk mengunggah dan mengelola dokumen identitas pelanggan (KTP, NPWP, foto rumah) dan legalitas mitra (NIB, Surat Kuasa) dengan validasi tipe berkas dan isolasi akses per-tenant. |
| **#233** | Automated Employee Payroll System | Solusi penggajian karyawan otomatis end-to-end: kalkulasi BPJS & PPh 21 TER, lembur absensi, alur approval maker-checker, posting jurnal akuntansi double-entry seimbang, dokumen slip gaji siap cetak, dan portal mandiri karyawan. |

### Kemampuan Baru Pengguna/Admin

- **Finance & Accounting Administrator**:
  - Dapat menganalisis pergerakan likuiditas perusahaan secara komprehensif melalui **Laporan Arus Kas** (*Cash Flow Statement*) yang membagi arus kas ke aktivitas operasi, investasi, dan pendanaan, serta memantau selisih rekonsiliasi kas secara instan.
  - Dapat memantau kesehatan penagihan piutang pelanggan melalui **Laporan Umur Piutang** (*Receivable Aging*) dan kewajiban tagihan belanja melalui **Laporan Umur Hutang** (*Payable Aging*) dengan kartu metrik ringkasan per ember jatuh tempo dan tabel detail yang responsif.
  - Dapat mencetak dokumen **Buku Besar** (*General Ledger*) resmi untuk seluruh atau sebagian akun perkiraan terpilih dalam satu periode pembukuan tanpa terpotong paginasi.
  - Dapat meninjau dan menyetujui/menolak draf transaksi dari tab terpisah **Draf Tertunda** pada menu Transaksi Keuangan dengan indikator badge draf aktif.
  - Dapat mengklasifikasikan kategori arus kas (*Operating*, *Investing*, *Financing*) pada setiap akun perkiraan (COA).
- **Network Engineers & NOC**:
  - Dapat memantau utilisasi bandwidth router/switch secara visual melalui grafik RRDtool interaktif dengan zoom modal dan rentang waktu dinamis.
  - Dapat memindai dan mendaftarkan interface jaringan baru ke sistem monitoring secara otomatis hanya dengan memasukkan kredensial SNMP perangkat.
  - Dapat membersihkan lonjakan data abnormal pada database RRD menggunakan fitur *Spike Killer*.
- **Developers & System Administrators**:
  - Dapat memeriksa riwayat log aplikasi Winston secara terpusat pada menu Pengaturan Developer dan memanfaatkan fitur **AI Root Cause Analysis** untuk mendapatkan analisis teknis dan saran pemecahan masalah secara instan.
  - Dapat melakukan simulasi berbagai pola beban trafik data menggunakan panel *Fake Traffic Generator*.
- **Partner / Mitra Bisnis (via Partner API)**:
  - Dapat mengunggah berkas identitas calon pelanggan (KTP, NPWP) dan berkas legalitas usaha (NIB, Surat Kuasa) secara terprogram melalui REST API.

### Bug Fix / Solusi Masalah

- **Eliminasi Masalah Memori & N+1 pada Laporan Umur Piutang dan Hutang**: Menggantikan pemrosesan loop di memori Node.js yang memuat seluruh dokumen tagihan dengan agregasi native MongoDB berpaginasi, menjamin performa cepat dan bebas lonjakan konsumsi RAM pada dataset berukuran besar.
- **Pemisahan Pendapatan Lain-lain pada Laba-Rugi (Temuan C9)**: Memperbaiki kalkulasi Laba Kotor dan Laba Operasional agar tidak tercampur dengan akun Pendapatan Lain-lain (*Other Income*).
- **Pencegahan Saldo Awal Dobel (Temuan C8)**: Menolak pembuatan jurnal saldo awal baru untuk COA yang sudah memiliki nilai saldo awal terdaftar dan menyematkan detektor konflik saldo pada laporan Neraca.
- **Konsistensi Definisi Saldo Buku Besar Kas (Temuan B1)**: Menyeragamkan filter `deleted: { $ne: true }` pada seluruh agregasi buku besar kas untuk mencegah pergeseran saldo akibat log yang telah dihapus (*soft-deleted*).
- **Kaskade Otomatis Path & Level Hierarki COA (Temuan B4 & B5)**: Memastikan pembaruan kode akun atau induk mengkaskadekan path seluruh akun turunan secara atomik serta menolak relasi hierarki melingkar (*circular reference*).
- **Penegakan Penguncian Periode Akuntansi pada Jurnal Non-Kas (Temuan B6)**: Memastikan jurnal manual, saldo awal, dan jurnal pembalikan tunduk pada aturan penguncian periode dan larangan tanggal masa depan.
- **Perbaikan Operator Update Mongoose (Temuan B10)**: Menggantikan operator `$set: undefined` dengan `$unset` pada layanan jadwal transaksi berulang.

### Menu/Fitur Baru

- **Menu Finance > Reports (`/finance/reports`)**:
  - Tab **Laporan Arus Kas** (*Cash Flow Statement*)
  - Tab **Umur Piutang** (*Receivable Aging*)
  - Tab **Buku Besar Cetak** (*Printable General Ledger*)
- **Menu Finance > Transactions (`/finance/transactions`)**:
  - Tab **Draf Tertunda** (*Pending Transaction Drafts*) dengan badge counter dan drawer persetujuan.
- **Menu Network > Traffic Monitoring (`/network/traffic`)**:
  - Dashboard monitoring grafik trafik RRDtool, filter rentang waktu, drawer interface auto-discovery, dan modal kustomisasi style.
- **Menu Settings > Developer > System Logs (`/settings/developer`)**:
  - Tab log sistem terpusat dan modal analisis AI Gemini.
- **Menu Settings > Developer > Fake Traffic (`/settings/developer`)**:
  - Panel generator simulasi trafik jaringan.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Analisis Laporan Arus Kas & Rekonsiliasi Kas (#183)

- **Penjelasan Fitur**: Laporan Arus Kas menyajikan arus kas masuk dan keluar yang dikelompokkan ke dalam aktivitas operasi, investasi, dan pendanaan menggunakan metode tidak langsung. Sistem secara otomatis memverifikasi kebenaran perhitungan dengan membandingkan total arus kas bersih terhadap selisih saldo kas fisik riil di akun kas & bank.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Finance > Reports** dan pilih tab **Arus Kas**.
  2. Tentukan periode tanggal pembukuan (contoh: awal bulan hingga akhir bulan berjalan).
  3. Periksa rincian arus kas pada seksi **Aktivitas Operasi**, **Aktivitas Investasi**, dan **Aktivitas Pendanaan**.
  4. Periksa kartu **Rekonsiliasi Kas**: jika badge berstatus *Terekonsiliasi (Selisih Rp 0)*, maka seluruh mutasi kas terpetakan sempurna dengan laporan neraca.

---

### 2. Pemantauan Umur Piutang & Penagihan Faktur (#183)

- **Penjelasan Fitur**: Modul Umur Piutang (*Receivable Aging*) mengelompokkan seluruh faktur pelanggan yang belum lunas ke dalam ember waktu jatuh tempo (0-30 hari, 31-60 hari, 61-90 hari, dan >90 hari) untuk mempermudah identifikasi piutang macet dan prioritas penagihan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Finance > Reports** dan pilih tab **Umur Piutang**.
  2. Tinjau kartu ringkasan nominal di bagian atas untuk melihat total eksposur piutang per kelompok usia.
  3. Gunakan filter pencarian pada tabel di bawahnya untuk mencari faktur berdasarkan nama pelanggan, nomor invoice, atau rentang tanggal jatuh tempo tertentu.

---

### 3. Monitoring Trafik Antarmuka Jaringan & Interface Auto-Discovery (#245)

- **Penjelasan Fitur**: Modul pemantauan trafik jaringan real-time yang membaca bandwidth masuk (*inbound*) dan keluar (*outbound*) interface router/switch melalui SNMP poller dan menyajikannya dalam bentuk grafik time-series RRDtool interaktif.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Network > Traffic Monitoring**.
  2. Untuk menambahkan target port baru, klik tombol **Discover Interface**, pilih perangkat router yang dituju, lalu pilih port jaringan yang ingin dipantau dari hasil pemindaian SNMP.
  3. Pada dashboard utama, amati pergerakan grafik trafik. Gunakan tombol filter rentang waktu (contoh: *1h*, *24h*, *7d*) atau klik pada kartu grafik untuk membuka modal zoom interaktif.
