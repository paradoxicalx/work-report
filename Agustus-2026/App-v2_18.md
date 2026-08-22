# 📝 Daily Work Report - Dedy S.N Putra (2026-08-18)

---

## 📅 Laporan Harian - 18 Agustus 2026

---

## 🌿 Branch: `issue-227` — Duplikasi & Upgrade Modul Perangkat Jaringan (Network Devices) dengan Microservice Monitoring Probe (Ping, SNMP, Subnet Scanner) & Auto-Sync Cron

### 📌 Informasi Issue

- **Nomor Issue**: #227
- **Judul Issue**: Duplikasi & Upgrade Modul Perangkat Jaringan (Network Devices) dengan Microservice Monitoring Probe (Ping, SNMP, Subnet Scanner) & Auto-Sync Cron
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [0543087] - save #227 - 18 Agt 2026 18:53

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `package.json`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/internal.controller.js` [NEW]
  - `backend/src/controllers/networkDevice.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/networkDevice.model.js` [NEW]
  - `backend/src/routes/internal.route.js` [NEW]
  - `backend/src/routes/networkDevice.route.js` [NEW]
  - `backend/src/services/networkDevice.service.js` [NEW]
  - `backend/test/integration/networkDevice.service.test.js` [NEW]
  - `cron-worker/src/jobs/processors/networkDeviceSync.js` [NEW]
  - `cron-worker/src/jobs/scheduler.js`
  - `cron-worker/src/jobs/worker.js`
  - `cron-worker/src/services/api.service.js`
  - `frontend/src/app/navigation/networks.js`
  - `frontend/src/app/pages/network/devices/create.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/detail.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/discover.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/edit.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/index.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/network/devices/schema/createSchema.js` [NEW]
  - `frontend/src/app/pages/network/devices/schema/editSchema.js` [NEW]
  - `frontend/src/app/router/network/networkDeviceRoute.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
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
- **Deskripsi Perubahan & Fungsi**:
  - **Pembuatan Microservice `network-monitor`**:
    - Membangun service Node.js mandiri khusus untuk eksekusi probing jaringan agar tidak membebani Backend utama maupun Frontend.
    - Mengimplementasikan `ping.service.js` untuk pengukuran latensi, jitter, dan packet loss perangkat secara real-time.
    - Mengimplementasikan `snmp.service.js` (mendukung SNMP v1, v2c, dan v3) untuk mengambil informasi sistem (sysDescr, sysUptime, sysName), daftar interface jaringan, traffic rate, dan CPU/RAM usage.
    - Mengimplementasikan `scanner.service.js` untuk discovery pemindaian subnet IP (CIDR range) guna mendeteksi perangkat baru yang aktif di jaringan.
    - Menambahkan middleware autentikasi API Key (`auth.middleware.js`), logging terstruktur Winston (`logger.js`), serta dokumentasi Swagger/OpenAPI lengkap pada `probe.route.js`.
  - **Backend Core & Database Modul Perangkat Jaringan**:
    - Mendefinisikan schema Mongoose `networkDevice.model.js` dengan relasi ke Node/Site (`locationPoint`), IP Address, MAC Address, kredensial SNMP, metrik latensi, status operasional (`online`, `offline`, `warning`, `maintenance`), dan riwayat probe terakhir.
    - Mengembangkan `networkDevice.service.js` dan `networkDevice.controller.js` sesuai kaidah pemisahan logika controller-service ISPF V2, mencakup manajemen CRUD, query datatable dengan filter multi-kolom, trigger probe on-demand, dan eksekusi subnet discovery.
    - Menambahkan endpoint internal di `internal.route.js` dan `internal.controller.js` yang diamankan `INTERNAL_API_KEY` untuk sinkronisasi otomatis dari background worker.
    - Mendaftarkan hak akses RBAC (`networkDevice.list`, `networkDevice.read`, `networkDevice.create`, `networkDevice.update`, `networkDevice.delete`, `networkDevice.probe`) pada `privilege.json` dan melengkapi deskripsi privilege ID & EN.
    - Menulis unit/integration test `networkDevice.service.test.js` untuk menjamin keandalan endpoint dan service perangkat jaringan.
  - **Background Worker & Sinkronisasi Periodik (`cron-worker`)**:
    - Membuat processor job `networkDeviceSync.js` di `cron-worker` yang secara berkala memicu probe massal ke microservice `network-monitor` dan memperbarui status perangkat di Backend.
    - Mendaftarkan jadwal cron di `scheduler.js` dan worker handler di `worker.js`.
  - **Frontend Antarmuka Manajemen & Monitoring Perangkat (`frontend`)**:
    - Membuat halaman daftar perangkat `/networks/devices` (`index.jsx`) berbasis TanStack Table dengan filter status, pencarian multi-kolom, quick stats, dan badge status indikator latensi.
    - Membuat formulir tambah & ubah perangkat (`create.jsx`, `edit.jsx`) dengan validasi Yup, dropdown tipe perangkat dinamis yang otomatis mengagregasi data jenis yang sudah ada, serta konfigurasi parameter SNMP.
    - Membuat halaman detail perangkat (`detail.jsx`) yang menampilkan ringkasan metrik kesehatan, grafik latensi/uptime, tabel interface SNMP, serta tombol trigger probe langsung.
    - Membuat fitur pemindaian jaringan (`discover.jsx`) untuk menemukan perangkat aktif dalam rentang IP/subnet dan mengimpornya langsung ke sistem.
    - Mendaftarkan rute proteksi, menu navigasi `networks.js`, dan melengkapi seluruh dictionary i18n Bahasa Indonesia dan Bahasa Inggris.

---

## 🌿 Branch: `issue-217` — Pengembangan Modul Pengadaan & Pengeluaran Keuangan (RAB / Budgeting, Permintaan Belanja / Purchase Requests, Tagihan Pembelian & Pembayaran Beban, Jurnal Hutang Usaha / AP, Rekap PPN Masukan & Umur Hutang)

### 📌 Informasi Issue

- **Nomor Issue**: #217
- **Judul Issue**: Pengembangan Modul Pengadaan & Pengeluaran Keuangan (RAB / Budgeting, Permintaan Belanja / Purchase Requests, Tagihan Pembelian & Pembayaran Beban, Jurnal Hutang Usaha / AP, Rekap PPN Masukan & Umur Hutang)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [Work In Progress] - Penyempurnaan Alur Approval & Revisi Budgeting (RAB), Kwitansi Pembayaran Beban (Receipt), dan Integrasi Test - 18 Agt 2026

- **Komponen yang Berubah**:
  - `backend/src/controllers/admin.controller.js`
  - `backend/src/controllers/financeBudgeting.controller.js`
  - `backend/src/controllers/financeExpense.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/financeBudgeting.model.js`
  - `backend/src/routes/admin.route.js`
  - `backend/src/routes/financeExpense.route.js`
  - `backend/src/services/admin.service.js`
  - `backend/src/services/financeBudgeting.service.js`
  - `backend/src/services/financeExpense.service.js`
  - `backend/src/services/financeJournal.service.js`
  - `backend/src/utils/data-table.js`
  - `backend/src/utils/finance-error.js`
  - `backend/test/integration/financeBudgeting.approve.test.js` [NEW]
  - `backend/test/integration/financeBudgeting.create.test.js` [NEW]
  - `backend/test/integration/financeBudgeting.rejectRevise.test.js` [NEW]
  - `backend/test/integration/financeExpense.create.test.js`
  - `backend/test/integration/financeExpense.payment.race.test.js`
  - `backend/test/integration/financeExpense.payment.test.js`
  - `backend/test/integration/financeExpenseAP.journal.test.js`
  - `backend/test/integration/financeExpenseFromBudgeting.test.js` [NEW]
  - `backend/test/integration/financeExpensePayableAging.test.js`
  - `frontend/src/app/navigation/finance.js`
  - `frontend/src/app/pages/finance/budgeting/BudgetingDocument.jsx` [NEW]
  - `frontend/src/app/pages/finance/budgeting/BudgetingDrawer.jsx`
  - `frontend/src/app/pages/finance/budgeting/BudgetingItemsField.jsx`
  - `frontend/src/app/pages/finance/budgeting/detail.jsx`
  - `frontend/src/app/pages/finance/budgeting/schema/budgetingSchema.js`
  - `frontend/src/app/pages/finance/budgeting/schema/columns.jsx`
  - `frontend/src/app/pages/finance/expenses/ExpenseBillPaymentReceiptDocument.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/ExpenseBillPaymentReceiptModal.jsx` [NEW]
  - `frontend/src/app/pages/finance/expenses/ExpenseDrawer.jsx`
  - `frontend/src/app/pages/finance/expenses/ExpenseItemsField.jsx`
  - `frontend/src/app/pages/finance/expenses/detail.jsx`
  - `frontend/src/app/pages/finance/expenses/index.jsx`
  - `frontend/src/app/pages/finance/expenses/schema/columns.jsx`
  - `frontend/src/app/pages/finance/purchase-requests/PurchaseRequestDrawer.jsx`
  - `frontend/src/app/pages/finance/purchase-requests/PurchaseRequestItemsField.jsx`
  - `frontend/src/app/pages/finance/purchase-requests/detail.jsx`
  - `frontend/src/components/shared/ApprovePOModal.jsx`
  - `frontend/src/components/shared/DocumentPreviewModal.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Penyempurnaan Alur Approval, Revisi, dan Penolakan RAB (Budgeting)**:
    - Menambahkan logika bisnis di `financeBudgeting.service.js` dan endpoint controller terkait untuk transisi status RAB: pengajuan (`submit`), persetujuan bertingkat (`approve`), permintaan revisi dengan catatan (`requestRevision`), penolakan (`reject`), dan pembatalan (`cancel`).
    - Menghubungkan RAB yang telah disetujui dengan pembuatan Tagihan Beban / Pembelian (`Expense`) secara otomatis atau parsial dengan validasi alokasi kuantitas/anggaran item.
    - Membuat komponen cetak dan pratinjau dokumen RAB resmi `BudgetingDocument.jsx` dengan tata letak header perusahaan, rincian anggaran item, tanda tangan bertingkat, dan status watermark dokumen.
  - **Fitur Kwitansi Pembayaran Tagihan Pembelian/Beban (Payment Receipt)**:
    - Membuat komponen dokumen kwitansi cetak `ExpenseBillPaymentReceiptDocument.jsx` dan modal pratinjau `ExpenseBillPaymentReceiptModal.jsx` untuk mencetak bukti pembayaran resmi atas transaksi pelunasan beban atau tagihan vendor.
    - Mengintegrasikan tombol cetak bukti bayar pada setiap baris riwayat pembayaran di halaman detail tagihan beban (`frontend/src/app/pages/finance/expenses/detail.jsx`).
  - **Penyempurnaan Form, Validasi, dan Komponen Shared**:
    - Menambahkan opsi pemilihan sumber RAB (`budgeting_id`) pada pembuatan Expense di `ExpenseItemsField.jsx` dan `ExpenseDrawer.jsx`.
    - Memperbarui `DocumentPreviewModal.jsx` dan `ApprovePOModal.jsx` agar mendukung dokumen RAB dan kwitansi pembayaran beban secara dinamis.
    - Menambahkan helper cell `rows.jsx` untuk status approval RAB dan status pembayaran beban.
  - **Penambahan Test Suite Integrasi Modul Keuangan**:
    - Membuat berkas integration test baru: `financeBudgeting.create.test.js`, `financeBudgeting.approve.test.js`, `financeBudgeting.rejectRevise.test.js`, dan `financeExpenseFromBudgeting.test.js` untuk memastikan keutuhan validasi bisnis dan idempotensi status.
    - Memperbarui pengujian integrasi jurnal hutang usaha (AP), race condition pembayaran, dan laporan umur hutang (_payable aging_).
  - **Pembaruan Kamus Bahasa (i18n)**:
    - Melengkapi terjemahan ID & EN untuk status approval RAB, aksi cetak dokumen/kwitansi, pesan konfirmasi persetujuan/penolakan, dan keterangan tagihan beban.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul                                                                                                                | Dampak Utama                                                                                                                                                                                  |
| ----- | -------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #227  | Duplikasi & Upgrade Modul Perangkat Jaringan (Network Devices) dengan Microservice Monitoring Probe & Auto-Sync Cron | Menghadirkan modul manajemen perangkat jaringan end-to-end yang dilengkapi microservice khusus probe (ping, SNMP, scanner) dan auto-sync status berkala tanpa membebani server backend utama. |
| #217  | Pengembangan Modul Pengadaan & Pengeluaran Keuangan (RAB, Purchase Requests, Purchase Bills, Jurnal AP, Umur Hutang) | Melengkapi alur persetujuan/revisi anggaran (RAB), pencetakan dokumen resmi RAB, serta pencetakan bukti bayar/kwitansi pelunasan tagihan pembelian/beban.                                     |

### Kemampuan Baru Pengguna/Admin

- **Monitoring Perangkat Jaringan Real-Time**: Admin jaringan dapat memantau status online/offline, latensi ping, pemakaian bandwidth/interface, dan informasi hardware perangkat secara langsung atau otomatis via cron.
- **Discovery Subnet Jaringan**: Admin dapat memindai segmen IP/subnet untuk mendeteksi dan mendaftarkan perangkat baru secara cepat ke dalam sistem ISPF V2.
- **Approval & Revisi Rencana Anggaran (RAB)**: Manajemen keuangan dapat menyetujui, meminta revisi dengan catatan, atau menolak pengajuan RAB dari berbagai departemen, serta mencetak dokumen resmi RAB.
- **Cetak Bukti Bayar / Kwitansi Beban**: Tim finance dapat menerbitkan dan mencetak kwitansi pembayaran resmi atas setiap cicilan atau pelunasan tagihan pembelian/beban operasional kepada vendor.

### Bug Fix / Solusi Masalah

- **Pemisahan Beban Monitoring Jaringan**: Beban komputasi eksekusi raw ping, SNMP walk/poll, dan pemindaian IP dipindahkan sepenuhnya ke microservice terpisah (`network-monitor`), menjaga stabilitas performa API backend utama.
- **Penguncian & Validasi RAB**: Mencegah pembuatan tagihan beban dari RAB yang belum berstatus `approved` serta mencegah revisi pada anggaran yang sudah ditutup/dieksekusi.
- **Konsistensi UI & i18n**: Memperbaiki duplikasi identifier divider pada navigasi dan melengkapi missing translation keys pada modul jaringan dan keuangan.

### Menu/Fitur Baru

- **Menu Jaringan > Devices (`/networks/devices`)**: Dashboard tabel data perangkat jaringan dengan filter komprehensif, halaman detail metrik perangkat, formulir tambah/edit perangkat, dan fitur subnet scanner.
- **Pratinjau & Cetak Dokumen RAB**: Fitur cetak PDF/dokumen resmi rencana anggaran biaya dengan rincian approval flow.
- **Cetak Kwitansi Pembayaran Beban**: Modal dan layout pratinjau bukti bayar untuk setiap transaksi pelunasan pada detail tagihan beban.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur Modul Perangkat Jaringan & Probe Microservice**:
  - Modul Perangkat Jaringan di ISPF V2 terbagi menjadi tiga lapisan: Frontend UI untuk interaksi admin, Backend Core untuk penyimpanan dan otorisasi RBAC, serta Microservice `network-monitor` yang bertugas melakukan komunikasi jaringan tingkat rendah (ICMP Ping, SNMP v1/v2c/v3, dan ARP/Port Scanner). Sinkronisasi kesehatan perangkat dijalankan secara otomatis oleh `cron-worker` pada interval yang dapat dikonfigurasi.

- **Langkah Penggunaan (Tutorial - Menambah & Memonitor Perangkat Jaringan)**:
  1. Masuk ke Dashboard Admin, buka menu **Network** > **Devices**.
  2. Klik tombol **Tambah Perangkat** (atau gunakan tombol **Discover** untuk memindai subnet IP).
  3. Masukkan nama perangkat, alamat IP/host, tipe perangkat, lokasi node/site terkait, dan konfigurasi SNMP (community string / versi).
  4. Klik **Simpan**. Perangkat akan muncul pada tabel dengan status awal.
  5. Buka detail perangkat untuk melihat grafik latensi, informasi sistem (sysDescr, Uptime), daftar interface jaringan, atau klik tombol **Probe Sekarang** untuk memeriksa status terkini secara langsung.

- **Langkah Penggunaan (Tutorial - Cetak Kwitansi Pembayaran Tagihan Beban)**:
  1. Masuk ke menu **Keuangan** > **Tagihan Pembelian / Beban**.
  2. Pilih salah satu tagihan beban yang sudah memiliki riwayat pembayaran, lalu buka halaman detailnya.
  3. Pada tabel **Riwayat Pembayaran**, klik ikon/tombol **Kwitansi / Cetak Bukti Bayar** di kolom aksi transaksi terkait.
  4. Modal pratinjau kwitansi resmi akan muncul lengkap dengan nomor bukti, nominal pembayaran, nama penerima/vendor, dan rincian alokasi. Klik tombol **Cetak** untuk mencetak fisik atau mengunduh PDF.
