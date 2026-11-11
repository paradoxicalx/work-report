# 📝 Daily Work Report - Dedy (2026-08-29)

---

## 📅 Laporan Harian - 29 Agustus 2026

---

## 🌿 Branch: `master` / `issue-248` — Telegram Bot Ticket Closing Rich Notification & System-wide Audit Fixes

### 📌 Informasi Issue

- **Nomor Issue**: #248
- **Judul Issue**: Telegram Bot Ticket Closing Rich Notification — Integrasi Detail Penutupan Tiket Multi-Kategori (Incident, Installation, Survey, Dismantle, NOC, Payment), Timeline MTTR, Daftar Perangkat, Proteksi Format HTML, dan Audit Perbaikan Modul Finansial
- **Status Branch**: `Sudah di-merge` (Telah di-merge ke branch `master` via commit `17af094` & `2bc019a`)

### 📅 Rincian Commit

#### [17af094] / [2bc019a] - resolve #248 - 29 Agustus 2026, 15:34:16 / 15:33:35 WIB

- **Komponen yang Berubah**:
  - [`backend/src/utils/telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js) — Implementasi fungsi helper baru [`buildTicketCloseReportSection()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js) untuk menyusun payload notifikasi Telegram berformat HTML kaya saat tiket ditutup, dengan penyesuaian format dinamis per kategori tiket:
    - **Incident**: Menampilkan penyebab masalah (*root cause*), tindakan perbaikan, timeline gangguan (waktu gangguan, waktu pulih), dan durasi gangguan / Mean Time to Recovery (MTTR) dalam format durasi jam/menit yang mudah dibaca.
    - **Installation**: Menampilkan catatan instalasi, parameter teknis (sinyal optik, redaman Rx/Tx), daftar perangkat terpasang (nama, serial number, MAC address), serta lokasi dan koordinat POP terkait.
    - **Survey**: Menampilkan hasil survei lapangan, topologi jalur optik/kabel, dan lokasi POP target.
    - **Dismantle**: Menampilkan catatan pelepasan layanan dan inventaris perangkat yang ditarik/dilepas.
    - **NOC & Payment**: Menampilkan keterangan dan parameter teknis/pembayaran terkait.
    - **Teknisi Lapangan**: Menampilkan daftar nama teknisi yang bertugas menyelesaikan tiket.
    - **Sanitasi HTML**: Penambahan utilitas [`escapeHtml()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js) untuk mencegah kegagalan pengiriman akibat karakter khusus HTML (`<`, `>`, `&`).
  - [`backend/test/unit/ticketTelegramClose.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/ticketTelegramClose.test.js) [NEW] — Unit test suite komprehensif untuk pengujian fungsionalitas notifikasi Telegram penutupan tiket di seluruh kategori dan skenario edge-cases (formatting MTTR, koordinat Google Maps, escaping HTML).
  - [`backend/src/models/financeBudgeting.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financeBudgeting.model.js) — Standarisasi skema basis data anggaran periode keuangan, penguatan validasi tipe data, dan constraint index.
  - [`backend/src/controllers/financeGateway.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/financeGateway.controller.js), [`backend/src/services/financeGateway.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeGateway.service.js), [`backend/src/services/financeWallet.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeWallet.service.js), [`backend/src/services/financeAutoInvoice.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeAutoInvoice.service.js), [`backend/src/services/financeLedger.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeLedger.service.js), [`backend/src/services/financePayment.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financePayment.service.js), [`backend/src/services/paymentGateways/ipaymu.gateway.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/paymentGateways/ipaymu.gateway.js) — Audit dan penguatan transaksi dompet mitra, perbaikan kalkulasi auto-invoice prorata, verifikasi callback payment gateway, dan konsistensi posting jurnal keuangan.
  - [`backend/src/controllers/partnerApiCustomer.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiCustomer.controller.js), [`backend/src/controllers/partnerApiNetworkDevice.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiNetworkDevice.controller.js), [`backend/src/controllers/partnerApiPartner.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiPartner.controller.js), [`backend/src/controllers/partnerApiRadiusProfile.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiRadiusProfile.controller.js), [`backend/src/routes/partnerApi.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/partnerApi.route.js) — Pembersihan dan standardisasi response HTTP controller Partner API serta refactoring penamaan rute.
  - [`backend/src/services/networkDevice.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/networkDevice.service.js), [`backend/src/services/productBroadband.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/productBroadband.service.js), [`backend/src/services/radiusProfile.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/radiusProfile.service.js) — Penguatan null-safety, perbaikan sanitasi input kueri, dan penambahan error logging sebelum re-throw.
  - [`backend/test/integration/*`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/) — Pembaruan dan perbaikan suite integration test untuk finance wallet, partner API documents, radius profile API, recon alert, expense audit fixes, dan batch settlement.
- **Deskripsi Perubahan & Fungsi**:
  - Mengubah format notifikasi penutupan tiket di Telegram Bot menjadi laporan terstruktur yang informatif dan kaya detail sesuai jenis tiket yang dikerjakan teknisi, mempermudah tim manajemen memantau kualitas layanan dan MTTR tanpa harus membuka dashboard web.
  - Mengeliminasi potensi pesan error *bad request* dari Telegram API dengan menerapkan sanitasi tag HTML otomatis pada input deskripsi dan catatan teknisi yang dinamis.
  - Melakukan perbaikan menyeluruh pada modul keuangan dan Partner API untuk menjaga integritas mutasi saldo dompet digital mitra, penagihan prorata otomatis, dan rekonsiliasi gateway pembayaran.

---

## 🌿 Branch: `issue-233` — Automated Employee Payroll System & Financial Integration

### 📌 Informasi Issue

- **Nomor Issue**: #233
- **Judul Issue**: Automated Employee Payroll System — Engine Perhitungan Gaji Modular, Integrasi BPJS Ketenagakerjaan & BPJS Kesehatan, PPh 21 TER, Lembur & Kehadiran, Posting Jurnal Akuntansi Double-Entry, Slip Gaji Karyawan Siap Cetak, dan Notifikasi Transisi Status Real-time
- **Status Branch**: `Belum di-merge` (Active Development / In Progress)

### 📅 Rincian Commit & Perubahan Terkini

#### [66f7a98] / [Working Tree Update] - save #233 - 29 Agustus 2026, 23:44:00 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/payrollCalculation.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollCalculation.service.js) & [`backend/test/unit/payrollCalculation.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/payrollCalculation.test.js) — Implementasi engine kalkulasi payroll komprehensif:
    - Perhitungan gaji pokok dan tunjangan (tunjangan jabatan, makan, transport, kehadiran, komunikasi, kinerja).
    - Perhitungan lembur (*overtime*) per jam sesuai regulasi Depnaker (1.5x upah jam pertama, 2x upah jam berikutnya) berbasis kehadiran.
    - Potongan ketidakhadiran (*unpaid leave*) dan keterlambatan (*late penalty*) yang dihitung otomatis dari data absensi.
    - Perhitungan BPJS Ketenagakerjaan (JKK, JKM, JHT 2% karyawan / 3.7% perusahaan, JP 1% karyawan / 2% perusahaan dengan batasan upah maksimum / *wage cap*).
    - Perhitungan BPJS Kesehatan (1% porsi karyawan, 4% porsi perusahaan dengan batasan upah maksimum).
    - Dukungan konfigurasi `bpjs_company_borne`: jika aktif, porsi iuran BPJS karyawan ditanggung penuh oleh perusahaan dan ditambahkan ke beban perusahaan tanpa memotong gaji karyawan.
    - Perhitungan PPh 21 TER (Tarif Efektif Rata-Rata Kategori A, B, C berdasarkan status PTKP) sesuai peraturan perpajakan terbaru DJP.
  - [`backend/src/services/payrollRun.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollRun.service.js), [`backend/src/controllers/payrollRun.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/payrollRun.controller.js), [`backend/src/routes/payrollRun.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/payrollRun.route.js) — Layanan orkestrasi siklus hidup Payroll Run dengan state machine andal:
    - **Draft**: Pratinjau kalkulasi (*preview*) dan pembentukan (*generate*) periode penggajian massal maupun seleksi karyawan tertentu. Mendukung regenerasi periode yang sama tanpa duplikasi run.
    - **Submit**: Pengajuan draft penggajian ke level verifikator/manajer dengan validasi kelengkapan data.
    - **Approve**: Persetujuan penggajian dengan penegakan aturan *Maker-Checker* (penyetuju tidak boleh sama dengan pembuat run).
    - **Reject & Reopen**: Penolakan run dengan pencatatan alasan audit (*reason*), serta kemampuan pembukaan kembali (*reopen*) dari status rejected ke draft untuk perbaikan tanpa merusak riwayat.
    - **Post**: Eksekusi pencairan gaji dengan pemilihan akun kas/bank (`disbursement_account`), mendukung posting parsial (`slipIds`), dan isolasi *race condition*.
    - **Emergency Recovery / Slip Removal**: Fitur `removeSlipFromApprovedRun` untuk mengeluarkan slip bermasalah dari run berstatus approved sebelum dicairkan, dengan pencatatan alasan perbaikan formal.
  - [`backend/src/services/payrollLedger.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollLedger.service.js) — Integrasi akuntansi double-entry otomatis saat slip/run diposting:
    - **Debet**: Beban Gaji Pokok & Tunjangan Karyawan, Beban BPJS Ketenagakerjaan & Kesehatan (Porsi Perusahaan).
    - **Kredit**: Kas/Bank Pencairan, Utang PPh 21 Karyawan, Utang BPJS Ketenagakerjaan & Kesehatan, Piutang Pemotongan Kasbon Karyawan. Jurnal dijamin selalu seimbang (*balanced*).
  - [`backend/src/services/payrollAttendance.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollAttendance.service.js) — Sinkronisasi otomatis data presensi/kehadiran karyawan (total kehadiran, keterlambatan, cuti, izin, lembur) per rentang tanggal periode penggajian.
  - [`backend/src/services/payrollSlip.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollSlip.service.js), [`backend/src/controllers/payrollSlip.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/payrollSlip.controller.js), [`backend/src/routes/payrollSlip.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/payrollSlip.route.js) — Pengelolaan slip gaji individual, pembatalan slip berstatus paid melalui mekanisme jurnal pembalik kas (*void*), dan pembatasan hak akses (*readSensitive* vs *find.mine* scoping).
  - [`backend/src/services/notification.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/notification.service.js) — Notifikasi in-app real-time dan broadcast Socket.io terarah berdasarkan privilege `notification.payroll` pada setiap perubahan status alur kerja penggajian.
  - [`backend/src/services/payrollMigration.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollMigration.service.js) — Utilitas migrasi data penggajian legacy (V1) ke struktur dokumen V2 yang idempoten dan aman.
  - [`backend/test/integration/payrollRun.workflow.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/payrollRun.workflow.test.js) [NEW], [`backend/test/integration/payrollRun.notification.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/payrollRun.notification.test.js) [NEW], [`backend/test/integration/payrollRun.tableFilter.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/payrollRun.tableFilter.test.js) [NEW] — Rangkaian automated integration tests komprehensif mencakup 37+ skenario pengujian siklus hidup payroll, maker-checker, penolakan & reopen, partial posting, recovery kegagalan slip, pemulihan void, notifikasi privilege, dan filter data table.
  - [`frontend/src/app/pages/finance/payroll/settings/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/settings/index.jsx) & [`frontend/src/app/pages/finance/payroll/settings/BpjsSimulationModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/settings/BpjsSimulationModal.jsx) [NEW] — Antarmuka pengaturan payroll lengkap (komponen gaji, tarif BPJS, wage cap, PPh 21, akun COA) dengan modal simulasi interaktif untuk melihat kalkulasi biaya BPJS karyawan dan perusahaan secara real-time sebelum menyimpan formulir.
  - [`frontend/src/app/pages/finance/payroll/runs/GenerateRunDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/GenerateRunDrawer.jsx), [`frontend/src/app/pages/finance/payroll/runs/RunDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/RunDetailDrawer.jsx), [`frontend/src/app/pages/finance/payroll/runs/PostRunModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/PostRunModal.jsx), [`frontend/src/app/pages/finance/payroll/runs/RejectRunModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/RejectRunModal.jsx) [NEW], [`frontend/src/app/pages/finance/payroll/runs/RemoveApprovedSlipModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/RemoveApprovedSlipModal.jsx) [NEW] — Drawer dan modal manajemen batch penggajian: generate dengan preview instan, tampilan detail ringkasan total biaya (gaji kotor, potongan, take-home pay, beban perusahaan), modal penolakan bersyarat alasan, modal pengeluaran slip approved, dan modal posting pencairan kas.
  - [`frontend/src/app/pages/finance/payroll/slips/SlipDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/slips/SlipDetailDrawer.jsx), [`frontend/src/app/pages/finance/payroll/slips/SlipPreviewModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/slips/SlipPreviewModal.jsx) [NEW], [`frontend/src/app/pages/finance/payroll/slips/VoidSlipModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/slips/VoidSlipModal.jsx) [NEW], [`frontend/src/app/pages/finance/payroll/slips/PayrollSlipDocument.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/slips/PayrollSlipDocument.jsx) — Komponen pratinjau slip gaji resmi siap cetak (*Print Portal*) dengan tata letak kop perusahaan, rincian komponen pendapatan, potongan BPJS & PPh 21, tunjangan perusahaan, informasi absensi, serta modal pembatalan slip berstatus paid.
  - [`frontend/src/app/pages/finance/payroll/slips/MySlipsTabContent.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/slips/MySlipsTabContent.jsx), [`frontend/src/app/pages/users/components/PayrollProfileTabContent.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/components/PayrollProfileTabContent.jsx), [`frontend/src/app/pages/profile/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/profile/index.jsx) — Portal mandiri slip gaji pada halaman profil karyawan (`find.mine`), memastikan setiap pegawai hanya dapat melihat slip gajinya sendiri yang sudah berstatus `paid`.
  - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json), [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json), [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json), [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) — Lokalisasi lengkap kamus bahasa untuk modul payroll, komponen gaji, status workflow, pesan validasi, dan notifikasi.
- **Deskripsi Perubahan & Fungsi**:
  - Mengembangkan sistem penggajian karyawan otomatis end-to-end yang mengintegrasikan kalkulasi gaji pokok, tunjangan, lembur berbasis absensi, iuran BPJS Ketenagakerjaan/Kesehatan, dan pajak PPh 21 TER secara presisi dan sesuai regulasi ketenagakerjaan Indonesia.
  - Menerapkan alur persetujuan bertingkat (*Approval Workflow*) dengan prinsip *Maker-Checker*, penanganan pembatalan (*Reopen/Reject*), serta mekanisme pemulihan darurat untuk mengeluarkan slip bermasalah dari batch yang sudah disetujui.
  - Mengotomatiskan pencatatan akuntansi double-entry saat gaji dibayarkan, menghasilkan jurnal keuangan yang seimbang secara instan ke dalam buku besar perusahaan tanpa perlu input manual oleh staf akuntansi.
  - Menyediakan fitur cetak dokumen slip gaji berformat profesional dan portal mandiri bagi karyawan untuk melihat riwayat slip gaji pribadi mereka dengan jaminan keamanan data yang ketat.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul Modul / Fitur | Dampak Utama |
| :--- | :--- | :--- |
| **#248** | Telegram Bot Ticket Close & Financial Audit | Mengirimkan ringkasan penutupan tiket Telegram yang terstruktur dan detail (timeline gangguan, MTTR, teknisi, perangkat terpasang/ditarik, koordinat POP), sanitasi HTML, audit budgeting, dan penguatan transaksi keuangan & Partner API. |
| **#233** | Automated Employee Payroll System | Sistem otomatisasi penggajian karyawan terpadu: kalkulasi gaji multi-komponen, BPJS & PPh 21 TER, integrasi presensi/lembur, alur persetujuan maker-checker, posting jurnal akuntansi double-entry, dokumen slip gaji siap cetak, dan portal mandiri karyawan. |

### Kemampuan Baru Pengguna/Admin

- **Finance & HR Administrator**:
  - Dapat melakukan pratinjau kalkulasi gaji seluruh karyawan secara instan sebelum membentuk batch penggajian periode berjalan.
  - Dapat mengonfigurasi skema tarif BPJS (JKK, JKM, JHT, JP, Kesehatan) dan batasan upah maksimum (*wage cap*), serta menguji hasilnya langsung melalui modal simulasi interaktif tanpa perlu menyimpan formulir terlebih dahulu.
  - Dapat memposting pencairan gaji karyawan dengan memilih akun kas/bank pembayaran tertentu, baik sekaligus maupun bertahap (*partial posting*).
  - Dapat menolak batch penggajian dengan menyertakan catatan alasan revisi, atau mengeluarkan karyawan/slip tertentu dari batch yang telah disetujui jika ditemukan kekeliruan data (*emergency slip removal*).
- **Manajer / Direksi (Approver)**:
  - Menerima notifikasi otomatis saat batch penggajian diajukan untuk ditinjau, dengan rincian total beban perusahaan, take-home pay, dan potongan pajak/BPJS.
  - Dapat menyetujui (*Approve*) atau menolak (*Reject*) batch penggajian dengan penegakan integritas *Maker-Checker*.
- **Karyawan / Pegawai**:
  - Dapat mengakses tab "Slip Gaji Saya" pada halaman profil pribadi untuk melihat rincian pendapatan, potongan, dan mencetak dokumen slip gaji resmi berformat kertas standar.
- **Tim Manajemen & Teknisi Lapangan (NOC/Helpdesk)**:
  - Menerima notifikasi penutupan tiket di grup Telegram yang lengkap dengan durasi MTTR, data teknis, perangkat yang dipasang/ditarik, dan lokasi koordinat titik POP.

### Bug Fix / Solusi Masalah

- **Telegram Bot Special Character Error**: Memperbaiki kegagalan pengiriman pesan Telegram bot akibat karakter khusus HTML (`<`, `>`, `&`) pada catatan tiket dengan menerapkan sanitasi `escapeHtml()`.
- **Integrasi Akuntansi Penggajian Otomatis**: Menghilangkan kebutuhan pencatatan manual beban gaji ke buku besar dengan integrasi posting jurnal double-entry otomatis yang seimbang pada saat penggajian dicairkan.
- **Penanganan Kesalahan Slip Tanpa Menggagalkan Batch**: Menyediakan mekanisme pemulihan darurat untuk membatalkan atau mengeluarkan satu slip gaji yang bermasalah tanpa harus membatalkan seluruh penggajian karyawan lainnya dalam batch yang sama.
- **Pencegahan Double-Posting**: Menerapkan validasi status dan penanganan *race condition* pada saat persetujuan maupun posting pembayaran gaji agar tidak terjadi pencatatan jurnal ganda.

### Menu/Fitur Baru

- **Menu Finance > Payroll > Runs (`/finance/payroll/runs`)**: Dashboard pengelolaan siklus hidup batch penggajian bulanan (Draft, Submitted, Approved, Posted, Rejected).
- **Menu Finance > Payroll > Slips (`/finance/payroll/slips`)**: Tabel master slip gaji seluruh karyawan dengan fitur filter status dan pencarian data.
- **Menu Finance > Payroll > Settings (`/finance/payroll/settings`)**: Konfigurasi komponen gaji, BPJS, PPh 21, pemetaan akun COA buku besar, dan simulasi biaya BPJS.
- **Menu Profil > Slip Gaji Saya (`/profile`)**: Portal mandiri slip gaji bagi setiap karyawan terdaftar.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Penutupan Tiket & Notifikasi Telegram Interaktif (#248)

- **Penjelasan Fitur**: Ketika teknisi lapangan atau admin menyelesaikan pengerjaan tiket dan mengubah statusnya menjadi *Closed*, sistem secara otomatis mengekstrak seluruh rincian teknis pengerjaan (analisis gangguan, tindakan perbaikan, kalkulasi MTTR, daftar perangkat yang dipasang/ditarik, parameter sinyal, dan koordinat POP) lalu memformatnya menjadi laporan resmi di grup Telegram.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka modul **Tickets** dan pilih tiket yang sedang dikerjakan (contoh: *Incident* atau *Installation*).
  2. Klik tombol **Resolve / Close Ticket** pada drawer detail tiket.
  3. Isi laporan penutupan (catatan teknisi, parameter teknis, perangkat terpasang/dilepas, dan waktu penanganan).
  4. Simpan perubahan. Sistem akan otomatis memposting ringkasan lengkap ke grup Telegram dengan format yang rapi dan aman dari error formatting.

---

### 2. Siklus Penggajian Karyawan & Integrasi Akuntansi (#233)

- **Penjelasan Fitur**: Modul penggajian otomatis yang memproses penghitungan gaji pokok, tunjangan, lembur berbasis absensi, BPJS, dan PPh 21 TER, dilanjutkan dengan alur verifikasi *Maker-Checker* hingga pencatatan jurnal akuntansi buku besar secara otomatis.
- **Langkah Penggunaan (Tutorial)**:
  1. **Konfigurasi Sistem**: Buka menu **Finance > Payroll > Settings**, sesuaikan tarif BPJS, batas upah (*wage cap*), dan akun kas pencairan gaji. Gunakan tombol **Simulasi BPJS** untuk menguji perhitungan biaya.
  2. **Generate Payroll Run**: Buka menu **Finance > Payroll > Runs**, klik tombol **Generate Run**. Pilih periode bulan penggajian (contoh: `2026-08`), lalu klik **Preview** untuk meninjau kalkulasi gaji seluruh karyawan sebelum membentuk draft run.
  3. **Pengajuan & Persetujuan**: Klik **Submit for Approval** pada run yang telah dibuat. Manajer/Approver yang berwenang kemudian meninjau rincian biaya pada **Run Detail Drawer** dan menekan tombol **Approve**.
  4. **Pencairan & Posting Jurnal**: Setelah disetujui, admin finance menekan tombol **Post Payroll**, memilih akun kas/bank pencairan, dan mengonfirmasi pembayaran. Sistem akan mengubah status slip menjadi `paid` dan mencatat jurnal akuntansi double-entry secara otomatis.
  5. **Melihat & Mencetak Slip**: Karyawan dapat masuk ke menu **Profil > Slip Gaji Saya** untuk melihat rincian gaji mereka dan mengunduh/mencetak dokumen slip gaji resmi.
