# 📝 Daily Work Report - Dedy (2026-08-27)

---

## 📅 Laporan Harian - 27 Agustus 2026

---

## 🌿 Branch: `issue-233` — Finance Payroll System ⚡ WIP

### 📌 Informasi Issue

- **Nomor Issue**: #233
- **Judul Issue**: Finance Payroll System — Sistem Penggajian Karyawan, Kalkulasi BPJS/Lembur/Pph21, Alur Persetujuan Maker-Checker, dan Integrasi Jurnal Akuntansi
- **Status Branch**: `Belum di-merge` (Work In Progress — perubahan dalam working tree / uncommitted)

### 📝 Rincian Pekerjaan (Uncommitted Changes)

> Seluruh perubahan di bawah ini masih dalam status **unstaged & untracked** di branch `issue-233`.
> Total: **55 berkas** (19 berkas termodifikasi, 36 berkas baru), **+7.566 baris kode**.

#### A. Arsitektur Model Data & Skema Database Penggajian

- **Komponen yang Berubah**:
  - [`backend/src/models/employeePayrollProfile.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/employeePayrollProfile.model.js) [NEW] — Skema konfigurasi nominal gaji per karyawan (gaji pokok, insentif kehadiran, nilai poin, transportasi, uang makan, tunjangan jabatan, fee marketing, kepesertaan BPJS Kesehatan/Ketenagakerjaan, upah resmi terdaftar BPJS, dan rekening bank).
  - [`backend/src/models/payrollRun.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/payrollRun.model.js) [NEW] — Skema batch proses penggajian satu periode bulanan dengan lifecycle status (`draft`, `pendingApproval`, `approved`, `rejected`, `posted`), ringkasan agregat keuangan (total gross, total net, total biaya BPJS perusahaan), audit trail (`generated_by`, `reviewed_by`, `posted_by`, `rejected_by`), serta penguncian akun pencairan kas/bank.
  - [`backend/src/models/payrollSlip.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/payrollSlip.model.js) [NEW] — Skema rincian slip gaji individual per karyawan mencakup seluruh breakdown penerimaan, potongan alpha, lembur, porsi iuran BPJS (karyawan vs perusahaan), PPh 21, baris penerimaan/potongan tambahan, snapshot tarif BPJS, referensi jurnal akuntansi, dan audit status slip (`draft`, `pendingApproval`, `approved`, `rejected`, `paid`, `cancelled`).
  - [`backend/src/models/payrollSettings.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/payrollSettings.model.js) [NEW] — Skema konfigurasi global penggajian (tarif BPJS Kesehatan & Ketenagakerjaan JHT/JP/JKK/JKM, plafon upah/wage cap BPJS, parameter lembur Kemenaker, sakelar potongan alpha, penegakan maker-checker, serta pemetaan akun perkiraan COA akuntansi).
  - [`backend/src/models/financeJournal.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financeJournal.model.js) — Penambahan tipe transaksi jurnal `'payroll'` untuk pembukuan otomatis pencairan slip gaji.
  - [`backend/src/models/financeLogs.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financeLogs.model.js) — Pendaftaran modul `'payroll'` pada audit log keuangan.
- **Deskripsi Perubahan & Fungsi**:
  - Merancang skema basis data Mongoose terstruktur untuk modul penggajian menyeluruh. Seluruh nominal desimal dibulatkan secara presisi dan divalidasi dengan constraint Mongoose.
  - Snapshot tarif BPJS (`bpjs_rates_snapshot`) disimpan langsung pada setiap dokumen `PayrollSlip` saat generate agar histori slip masa lalu tidak berubah jika di kemudian hari admin mengubah persentase/plafon BPJS di pengaturan.
  - Penguncian akun kas/bank (`disbursement_account`) diterapkan pada level `PayrollRun` untuk memastikan idempotensi saat proses pencairan/posting batch.

#### B. Mesin Kalkulasi Gaji Murni (Pure Calculation Engine)

- **Komponen yang Berubah**:
  - [`backend/src/services/payrollCalculation.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollCalculation.service.js) [NEW] — Modul kalkulasi gaji independen tanpa ketergantungan database untuk pengujian deterministik dan mencegah disparitas formula antara pratinjau (preview) dan pembuatan nyata (generate).
- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan penghitungan hari kerja resmi (Senin-Jumat) dalam satu bulan kalender via [`countWorkingDaysInPeriod()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollCalculation.service.js).
  - Mengimplementasikan kalkulasi lembur harian berbasis regulasi Ketenagakerjaan via [`calculateOvertime()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollCalculation.service.js): upah per jam dihitung dari `base_salary / overtime_hourly_divisor` (standar 173), dengan tarif 1.5x untuk jam pertama dan 2.0x untuk jam-jam berikutnya di setiap hari hadir.
  - Mengimplementasikan kalkulasi komponen komprehensif via [`calculatePayrollSlipComponents()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollCalculation.service.js):
    - **Penerimaan Kotor (Gross)**: Gaji pokok + Insentif Kehadiran (hadir * insentif harian) + Insentif Poin (poin * nilai per poin) + Transportasi (hadir * transport harian) + Uang Makan (hadir * makan harian) + Tunjangan Jabatan + Fee Marketing (jumlah tiket instalasi * nominal per tiket) + Upah Lembur + Baris Penerimaan Tambahan.
    - **Potongan (Deductions)**: Potongan Alpha (proporsional terhadap hari kerja: `(alpha_days / working_days) * base_salary`), BPJS Kesehatan Karyawan (1%, dibatasi wage cap), BPJS JHT Karyawan (2%), BPJS JP Karyawan (1%, dibatasi wage cap JP), PPh 21, serta Baris Potongan Tambahan (misal pelunasan kasbon).
    - **Beban Perusahaan (Company BPJS Cost)**: BPJS Kesehatan Perusahaan (4%), BPJS JHT Perusahaan (3.7%), BPJS JP Perusahaan (2%), BPJS JKK Perusahaan (0.24%), dan BPJS JKM Perusahaan (0.3%).
    - **Basis Upah BPJS**: Memprioritaskan `bpjs_registered_wage` (upah resmi yang dilaporkan ke kantor BPJS) sebagai basis tunggal iuran; jika belum disetel, fallback otomatis ke `base_salary + position_allowance`.

#### C. Integrasi Akuntansi Double-Entry & Pembukuan Otomatis

- **Komponen yang Berubah**:
  - [`backend/src/services/payrollLedger.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollLedger.service.js) [NEW] — Layanan integrasi akuntansi satu pintu untuk mem-posting slip gaji ke buku besar kas dan jurnal umum secara seimbang (*balanced double-entry*), serta pembatalan/void slip dengan reversal jurnal.
  - [`backend/src/data/financeCoaSeed.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/financeCoaSeed.json) — Seeding akun Chart of Accounts (COA) standar penggajian: `5101` (Beban Gaji & Upah), `5102` (Beban BPJS Karyawan), `2105` (Hutang PPh 21), `2106` (Hutang BPJS Kesehatan), dan `2107` (Hutang BPJS Ketenagakerjaan).
- **Deskripsi Perubahan & Fungsi**:
  - Mengotomatiskan penjurnalan keuangan saat status `PayrollRun` diposting (`postPayrollSlipLedger`):
    - **Debet**: Beban Gaji & Upah (`gross_salary - alpha_deduction`)
    - **Debet**: Beban BPJS (`total_company_bpjs_cost`)
    - **Kredit**: Kas / Bank Pencairan (`net_salary`) via [`postEntries()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeLedger.service.js)
    - **Kredit**: Piutang Karyawan (`total_additional_deductions`)
    - **Kredit**: Hutang PPh 21 (`pph21`)
    - **Kredit**: Hutang BPJS Kesehatan (`bpjs_kesehatan employee + company`)
    - **Kredit**: Hutang BPJS Ketenagakerjaan (`bpjs_jht + bpjs_jp employee + company + jkk + jkm`)
  - Menangani pembatalan slip berstatus terbayar via [`voidPayrollSlipLedger()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollLedger.service.js) yang membalikkan jurnal secara otomatis (*reversal entries*) dan mengembalikan saldo rekening kas.

#### D. Orkestrasi Batch Penggajian & Alur Persetujuan Maker-Checker

- **Komponen yang Berubah**:
  - [`backend/src/services/payrollRun.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollRun.service.js) [NEW] — Orkestrasi menyeluruh pembuatan run bulanan, agregasi data kehadiran & profil semua karyawan aktif, transisi status approval, serta posting pencairan batch atomik.
  - [`backend/src/controllers/payrollRun.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/payrollRun.controller.js) [NEW] — Controller request handler untuk preview, generate, list datatable, detail, submit approval, approve, reject, post pencairan, ringkasan metrics, dan trigger migrasi data historis V1.
  - [`backend/src/routes/payrollRun.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/payrollRun.route.js) [NEW] — Definisi endpoint REST API dan dokumentasi Swagger interaktif lengkap untuk seluruh operasi `PayrollRun`.
- **Deskripsi Perubahan & Fungsi**:
  - Mengimplementasikan alur verifikasi bertingkat (*Maker-Checker*): ketika sakelar `maker_checker_enforced` aktif di pengaturan, admin yang men-generate run dilarang menyetujui (`approve`) run buatannya sendiri.
  - Menjamin eksekusi posting batch secara atomik dan *resumable*: slip yang sudah diposting tidak akan diposting ganda jika terjadi kegagalan jaringan di tengah batch.
  - Menyediakan endpoint pratinjau (`POST /payroll/run/preview`) agar manajer keuangan dapat memverifikasi simulasi nominal gaji seluruh karyawan sebelum run resmi dibuat ke basis data.

#### E. Manajemen Slip Gaji & Kontrol Privasi Data Sensitif

- **Komponen yang Berubah**:
  - [`backend/src/services/payrollSlip.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollSlip.service.js) [NEW] — Logika data access untuk membaca daftar slip, detail slip, penyesuaian nominal/koreksi pada status draft, dan pembatalan slip paid.
  - [`backend/src/controllers/payrollSlip.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/payrollSlip.controller.js) [NEW] — Controller slip gaji dengan *dual-access scoping*: karyawan biasa otomatis dibatasi hanya bisa melihat slip miliknya sendiri, sedangkan manajer dengan hak akses `payroll.readSensitive` dapat mengakses seluruh slip.
  - [`backend/src/routes/payrollSlip.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/payrollSlip.route.js) [NEW] — Route endpoint table, detail, update koreksi draft, dan void slip gaji.
- **Deskripsi Perubahan & Fungsi**:
  - Mengizinkan penyesuaian manual (*adjustments*) berupa baris penerimaan tambahan, potongan tambahan, maupun koreksi PPh 21 selama status slip masih dalam tahap `draft`. Setiap kali baris diubah, seluruh nominal *gross*, *net*, dan potongan dikalkulasi ulang secara otomatis.
  - Menerapkan perlindungan privasi data sensitif: endpoint membaca slip tidak mengekspos rincian slip karyawan lain kepada pengguna tanpa hak akses khusus.

#### F. Integrasi Presensi Karyawan & Tiket Pemasangan

- **Komponen yang Berubah**:
  - [`backend/src/services/payrollAttendance.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollAttendance.service.js) [NEW] — Layanan agregasi rekaman presensi bulanan, izin, dan cuti berbayar per karyawan untuk menentukan jumlah hari hadir, total durasi kerja harian, dan jumlah hari alpha.
  - [`backend/src/services/ticket.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/ticket.service.js) — Fungsi baru [`countCompletedInstallationsByMarketing()`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/ticket.service.js) untuk menghitung jumlah tiket instalasi pelanggan baru yang selesai dikerjakan oleh staf marketing terkait dalam periode penggajian.
- **Deskripsi Perubahan & Fungsi**:
  - Menghubungkan modul penggajian secara langsung dengan data presensi aktual di modul Attendance, mencegah input kehadiran manual yang rawan manipulasi.
  - Menghubungkan komisi marketing dengan data tiket instalasi yang berstatus selesai (*completed*) pada modul Ticketing.

#### G. Migrasi Histori Slip Gaji V1 (Backfill Service)

- **Komponen yang Berubah**:
  - [`backend/src/services/payrollMigration.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollMigration.service.js) [NEW] — Layanan migrasi satu kali untuk mengimpor seluruh arsip slip gaji dari tabel legasi V1 (`salary_slips`) ke struktur koleksi V2 `payroll_slips` dan `payroll_runs`.
- **Deskripsi Perubahan & Fungsi**:
  - Bersifat idempoten dan aman dijalankan berkali-kali (`dryRun` default `true`). Slip legasi yang telah termigrasi ditandai dengan `legacy_salary_slip_id` sehingga tidak akan diduplikasi.
  - Data migrasi diperlakukan murni sebagai arsip historis (*read-only*) tanpa memicu penjurnalan akuntansi susulan.

#### H. Pengaturan Global, Profil Karyawan, & Hak Akses

- **Komponen yang Berubah**:
  - [`backend/src/services/payrollSettings.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/payrollSettings.service.js) [NEW] — CRUD konfigurasi global penggajian dengan inisialisasi default otomatis (*auto-seed*).
  - [`backend/src/services/employeePayrollProfile.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/employeePayrollProfile.service.js) [NEW] — CRUD profil gaji karyawan dengan audit logging keuangan.
  - [`backend/src/controllers/payrollSettings.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/payrollSettings.controller.js) [NEW] & [`backend/src/routes/payrollSettings.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/payrollSettings.route.js) [NEW]
  - [`backend/src/controllers/employeePayrollProfile.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/employeePayrollProfile.controller.js) [NEW] & [`backend/src/routes/employeePayrollProfile.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/employeePayrollProfile.route.js) [NEW]
  - [`backend/src/services/employee.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/employee.service.js) — Penambahan penghapusan bertingkat (*cascade delete*) profil payroll saat data karyawan dihapus dari sistem.
  - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) — Pendaftaran hak akses terinci: `payroll.create`, `payroll.readSensitive`, `payroll.list`, `payroll.read`, `payroll.update`, `payroll.changeStatus`, `payroll.delete`, `payrollSettings.read`, `payrollSettings.update`, `payrollProfile.readSensitive`, `payrollProfile.update`.
  - [`backend/src/utils/payroll-error.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/payroll-error.js) [NEW] — Standardisasi kode error dan helper pembuatan error i18n khusus payroll.
  - [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) & [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) — Penambahan seluruh translation keys pesan error dan deskripsi operasi backend.

#### I. Antarmuka Frontend Penggajian (Payroll UI)

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/finance/payroll/runs/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/index.jsx) [NEW] — Halaman utama daftar proses penggajian bulanan dengan TanStack Table terintegrasi.
  - [`frontend/src/app/pages/finance/payroll/runs/GenerateRunDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/GenerateRunDrawer.jsx) [NEW] — Drawer interaktif untuk memilih periode penggajian, menjalankan kalkulasi pratinjau instan (*live preview table*), serta men-generate batch payroll resmi.
  - [`frontend/src/app/pages/finance/payroll/runs/RunDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/RunDetailDrawer.jsx) [NEW] — Drawer detail proses penggajian yang memuat ringkasan kartu metrik total, aksi workflow approval (*Submit*, *Approve*, *Reject*, *Post & Bayar*), dan tabel seluruh slip gaji pada periode tersebut.
  - [`frontend/src/app/pages/finance/payroll/runs/PostRunModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/PostRunModal.jsx) [NEW] — Modal konfirmasi pencairan gaji dengan dropdown pemilihan rekening sumber kas/bank.
  - [`frontend/src/app/pages/finance/payroll/runs/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/runs/schema/columns.jsx) [NEW] — Definisi kolom tabel proses penggajian lengkap dengan badge status dan aksi drawer.
  - [`frontend/src/app/pages/finance/payroll/slips/SlipDetailDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/slips/SlipDetailDrawer.jsx) [NEW] — Drawer rincian slip gaji individual yang mendukung penambahan/penghapusan baris tunjangan dan potongan tambahan, pengubahan PPh 21, tombol cetak/unduh, dan tombol pembatalan slip.
  - [`frontend/src/app/pages/finance/payroll/slips/PayrollSlipDocument.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/slips/PayrollSlipDocument.jsx) [NEW] — Komponen dokumen slip gaji resmi berformat standar A4 dengan kop logo perusahaan untuk keperluan cetak langsung maupun ekspor PDF.
  - [`frontend/src/app/pages/finance/payroll/slips/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/slips/schema/columns.jsx) [NEW] — Kolom tabel daftar slip gaji individual.
  - [`frontend/src/app/pages/finance/payroll/settings/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/settings/index.jsx) [NEW] — Halaman pengaturan payroll komprehensif terbagi dalam 5 seksi: BPJS Kesehatan, BPJS Ketenagakerjaan, Lembur & Jam Kerja, Aturan Lainnya, dan Pemetaan Akun COA Akuntansi, lengkap dengan tooltip penjelasan regulasi di setiap field.
  - [`frontend/src/app/pages/finance/payroll/settings/schema/settingsSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/settings/schema/settingsSchema.js) [NEW] — Skema validasi Yup untuk formulir pengaturan penggajian.
  - [`frontend/src/app/navigation/finance.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/finance.js) — Pendaftaran menu navigasi baru: **Finance → Payroll** dan **Finance → Payroll Settings**.
  - [`frontend/src/app/router/finance/payroll.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/finance/payroll.jsx) [NEW] & [`frontend/src/app/router/protected.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/protected.jsx) — Konfigurasi routing halaman penggajian.

#### J. Integrasi Profil Karyawan & Portal Self-Service

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/users/employee/components/EmployeePayrollProfileCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/employee/components/EmployeePayrollProfileCard.jsx) [NEW] — Kartu konfigurasi gaji dan rekening bank karyawan pada halaman profil karyawan (hanya terlihat oleh pemegang hak akses `payrollProfile.readSensitive`).
  - [`frontend/src/app/pages/users/employee/profile.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/employee/profile.jsx) — Pemasangan kartu konfigurasi penggajian pada halaman detail profil karyawan.
  - [`frontend/src/app/pages/finance/payroll/slips/MySlipsTabContent.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/payroll/slips/MySlipsTabContent.jsx) [NEW] — Konten tab portal mandiri untuk melihat dan mencetak slip gaji pribadi.
  - [`frontend/src/app/pages/profile/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/profile/index.jsx) & [`frontend/src/app/pages/users/components/UserAttendanceTabs.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/components/UserAttendanceTabs.jsx) — Penambahan tab **Slip Gaji Saya** pada halaman profil pengguna untuk memudahkan setiap staf melihat riwayat gajinya tanpa harus meminta ke bagian HRD/Finance.
  - [`frontend/src/components/shared/form/FormInput.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/form/FormInput.jsx) — Penyempurnaan komponen form input: perbaikan resolusi label & placeholder, serta penambahan komponen [`InputPercent`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/form/FormInput.jsx).
  - [`frontend/src/components/shared/table/status.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/status.js) & [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx) — Helper resolusi badge warna dan label status proses penggajian (`PayrollRunStatusBadgeCell`, `PayrollSlipStatusBadgeCell`).
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) — Penambahan lebih dari 160 translation keys lengkap untuk antarmuka pengguna penggajian.

#### K. Pengujian Komprehensif (Unit & Integration Tests)

- **Komponen yang Berubah**:
  - [`backend/test/unit/payrollCalculation.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/payrollCalculation.test.js) [NEW] — 16 unit test komprehensif menguji ketepatan penghitungan hari kerja kalender, lembur harian (1.5x/2x), pemisahan porsi BPJS, wage cap plafon upah, potongan alpha proporsional, serta integritas snapshot tarif.
  - [`backend/test/integration/payrollRun.workflow.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/payrollRun.workflow.test.js) [NEW] — Pengujian integrasi alur kerja penuh (*end-to-end workflow*): generate → submit → approve → post pencairan → pembuatan jurnal double-entry otomatis → void reversal jurnal.
- **Hasil Pengujian**:
  - Seluruh 16 unit test pada `payrollCalculation.test.js` **lulus 100% (16 passed)**.

---

## 🌿 Branch: `issue-119` — Structured Logging, Observability & Developer Tools

### 📌 Informasi Issue

- **Nomor Issue**: #119
- **Judul Issue**: Structured Logging, Observability & Developer Tools — Overhaul logging terstruktur lintas seluruh microservice, manajemen penjadwal tugas latar belakang (Cron Worker), dan panel Developer Settings
- **Status Branch**: `Sudah di-merge` (ke master)

### 📅 Rincian Commit

#### [`8a81b57`](file:///home/dhedhy/Project/Dekasimal-V2) - resolve #119 - Kamis, 27 Agustus 2026, 00:15

- **Komponen yang Berubah**:
  - `backend/src/services/apiLog.service.js` [NEW] — Service terpusat normalisasi, pembersihan ANSI codes, dan filter log
  - `backend/src/controllers/log.controller.js` [NEW] — Handler API log, ignored paths, Redis flush, dan AI log analysis
  - `backend/src/routes/log.route.js` [NEW] — Route manajemen log sistem
  - `backend/src/services/systemEnvironment.service.js` [NEW] — Service pembacaan informasi sistem dan environment
  - `backend/src/services/cronWorkerControl.service.js` [NEW] — Service kontrol dan pemicu restart Cron Worker
  - `backend/src/utils/telegramAlert.js` [NEW] — Pengiriman alert otomatis saat terjadi error kritis
  - `backend/src/utils/sanitize.js` [NEW] — Sanitasi data sensitif pada metadata log
  - `backend/src/utils/correlation.js` [NEW] — Pelacakan request ID lintas service
  - `frontend/src/app/pages/settings/sections/Developer.jsx` [NEW] — Halaman utama Pengaturan Developer
  - `frontend/src/app/pages/settings/sections/developer/CronWorkerTab.jsx` [NEW] — Tab manajemen tugas periodik Cron Worker
  - `frontend/src/app/pages/settings/sections/developer/IgnoredPathsCard.jsx` [NEW] — Manajemen URL yang diabaikan dari pencatatan log
  - `frontend/src/app/pages/settings/sections/developer/OtherTab.jsx` [NEW] — Informasi environment dan tombol Flush Redis
  - `cron-worker/src/utils/logger.js` [NEW] & `cron-worker/src/controllers/cron.controller.js` [NEW] — Winston logger dan handler restart cron-worker
  - `network-monitor/src/utils/logger.js` & `telegram-api/src/utils/logger.js` & `whatsapp-api/src/utils/logger.js` [NEW] — Winston structured logging pada seluruh service satelit
  - 124 berkas diperbarui (+11.307 / -1.163 baris).
- **Deskripsi Perubahan & Fungsi**:
  - Menyelesaikan implementasi sistem observabilitas terpadu lintas seluruh microservice monorepo (Backend, Telegram API, WhatsApp API, Network Monitor, dan Cron Worker).
  - Menyediakan panel Pengaturan Developer terpusat di UI untuk memantau performa, membersihkan cache Redis, mengontrol tugas periodik Cron Worker, menyaring URL log yang diabaikan secara real-time, serta memanfaatkan AI Agent untuk menganalisis pola error sistem.

#### [`b7b1d65`](file:///home/dhedhy/Project/Dekasimal-V2) - fix: hapus import duplikat resolvePurchaseRequestDisplayStatus/resolveLegacyExpenseBillStatus di rows.jsx - Kamis, 27 Agustus 2026, 00:29

- **Komponen yang Berubah**:
  - `frontend/src/components/shared/table/rows.jsx`
- **Deskripsi Perubahan & Fungsi**:
  - Menghapus sisa impor fungsi duplikat dari refactoring status helper pada `rows.jsx` yang sempat memicu error kompilasi Vite (*Identifier has already been declared*).

#### [`8063e6e`](file:///home/dhedhy/Project/Dekasimal-V2) - hotfix #119 - Kamis, 27 Agustus 2026, 00:46

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/routes/financeGateway.route.js`
  - `backend/src/routes/partnerApi.route.js`
  - `cron-worker/src/services/api.service.js`
  - `frontend/src/app/pages/finance/expenses/detail.jsx`
  - `frontend/src/app/pages/settings/sections/developer/IgnoredPathsCard.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/i18n/config.js`
- **Deskripsi Perubahan & Fungsi**:
  - Menambahkan mekanisme deduplikasi peringatan *missing translation keys* pada `frontend/src/i18n/config.js` agar tidak membanjiri konsol browser.
  - Memperbaiki penanganan error logging pada rute sistem internal dan menyelaraskan styling cell tabel log pada tema gelap (*dark mode*).

#### [`9145f5a`](file:///home/dhedhy/Project/Dekasimal-V2) - update changelog - Kamis, 27 Agustus 2026, 00:49

- **Komponen yang Berubah**:
  - `backend/src/data/changelog/index.json`
  - `backend/src/data/changelog/releases/issue-119.json` [NEW]
  - `backend/src/data/changelog/releases/issue-231.json` [NEW]
  - `backend/src/data/changelog/releases/issue-234.json` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - Memperbarui catatan rilis resmi (*changelog*) pada basis data sistem untuk rilis versi v1.51.0 (Issue #119), v1.50.0 (Issue #231), dan v1.49.1 (Issue #234).

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| :--- | :--- | :--- |
| **#233** | **Finance Payroll System (WIP)** | Modul penggajian karyawan terpadu: kalkulasi otomatis BPJS Kesehatan & Ketenagakerjaan (5 program), lembur harian Kemenaker, potongan alpha, persetujuan Maker-Checker, posting jurnal double-entry otomatis, manajemen slip gaji, serta portal mandiri (*self-service*) karyawan. |
| **#119** | **Structured Logging & Developer Tools** | Observabilitas terpusat lintas seluruh service, AI Log Analysis, panel manajemen Cron Worker di UI, pembersihan cache Redis instan, kontrol URL log yang diabaikan, dan sanitasi data sensitif. |

---

### 💼 Kemampuan Baru Pengguna & Admin

- **Generate Batch Penggajian Otomatis**: Admin/HRD dapat membuat proses penggajian bulanan untuk seluruh karyawan yang telah dikonfigurasi dalam sekali klik, dengan simulasi pratinjau (*live preview*) sebelum data disimpan.
- **Kalkulasi Akurat Sesuai Regulasi**: Sistem secara otomatis menghitung iuran BPJS Kesehatan (1% karyawan, 4% perusahaan), BPJS JHT (2% karyawan, 3.7% perusahaan), BPJS JP (1% karyawan, 2% perusahaan dengan wage cap), BPJS JKK (0.24%), BPJS JKM (0.3%), lembur harian (1.5x jam pertama, 2x jam berikutnya), potongan alpha berbasis presensi, dan fee marketing berbasis tiket instalasi.
- **Persetujuan Bertingkat (Maker-Checker)**: Mencegah kecurangan dalam penggajian dengan melarang pembuat proses payroll menyetujui pengajuannya sendiri.
- **Pembukuan Keuangan Otomatis**: Pencairan gaji langsung mencatat jurnal akuntansi seimbang (Beban Gaji, Beban BPJS, Kas/Bank, Hutang PPh21, Hutang BPJS Kesehatan & Ketenagakerjaan) tanpa perlu input manual di modul Jurnal.
- **Penyesuaian Manual Fleksibel**: Admin dapat menambahkan baris penerimaan tambahan (bonus, tunjangan khusus) atau potongan tambahan (pelunasan kasbon) pada slip berstatus draft.
- **Portal Mandiri Slip Gaji (Self-Service)**: Setiap karyawan dapat melihat dan mencetak slip gajinya secara mandiri melalui halaman profil akun mereka.
- **Manajemen Developer & Observabilitas**: Developer dapat mengelola jadwal tugas latar belakang, merestart Cron Worker, memicu analisis log via AI Agent, dan membersihkan cache Redis langsung dari antarmuka web.

---

### 🐛 Bug Fix & Solusi Masalah

- **Pencegahan Disparitas Rumus Gaji**: Menyatukan seluruh logika kalkulasi gaji ke dalam satu berkas mesin murni (`payrollCalculation.service.js`) sehingga hasil kalkulasi pratinjau (*preview*) selalu 100% identik dengan hasil saat disimpan (*generate*).
- **Penanganan Konflik Impor Vite**: Memperbaiki duplikasi impor status resolver pada `rows.jsx` yang sebelumnya menyebabkan kegagalan build Vite.
- **Pencegahan Banjir Log i18n**: Menerapkan mekanisme deduplikasi pelaporan missing key pada `i18n/config.js` untuk mencegah spamming request ke server.

---

### 📂 Menu & Rute Baru

- **Finance → Payroll (`/finance/payroll/runs`)**: Halaman pemantauan proses penggajian, pembuatan batch run, dan alur persetujuan.
- **Finance → Payroll Settings (`/finance/payroll/settings`)**: Halaman konfigurasi tarif BPJS, aturan lembur, potongan alpha, dan pemetaan akun perkiraan COA.
- **Users → Employee Profile → Konfigurasi Penggajian**: Kartu pengaturan gaji pokok, tunjangan, kepesertaan BPJS, dan nomor rekening per karyawan.
- **Profile → Slip Gaji Saya**: Tab mandiri bagi seluruh staf untuk mengakses dan mencetak slip gaji pribadi.
- **Settings → Developer (`/settings/developer`)**: Panel kontrol teknis pengembang (Cron Worker, System Logs, Ignored Paths, Other Utilities).

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Alur Proses Penggajian Bulanan (Payroll Run Workflow)

- **Penjelasan Fitur**: Modul penggajian dirancang dengan alur kontrol internal yang ketat (*Draft* ➔ *Pending Approval* ➔ *Approved* ➔ *Posted & Paid*).
- **Langkah Penggunaan**:
  1. Buka menu **Finance → Payroll**.
  2. Klik tombol **Generate Penggajian** di kanan atas.
  3. Pilih periode bulan penggajian (contoh: `2026-08`), lalu klik **Simulasikan** untuk melihat pratinjau kalkulasi gaji seluruh karyawan.
  4. Klik **Generate** untuk menyimpan batch penggajian ke sistem (status: `draft`).
  5. Buka detail run: jika ada penyesuaian khusus (bonus/kasbon), klik baris slip karyawan terkait untuk menambah baris penerimaan/potongan tambahan.
  6. Klik tombol **Ajukan Persetujuan** untuk memindahkan status ke `pendingApproval`.
  7. Manajer berwenang (berbeda dari pembuat jika maker-checker aktif) meninjau dan mengklik **Setujui**.
  8. Setelah disetujui, klik **Post & Bayar**, pilih akun kas/bank yang digunakan untuk pencairan, lalu konfirmasi. Sistem akan otomatis memperbarui status slip menjadi `paid` dan mencatat jurnal akuntansi double-entry.

---

### 2. Konfigurasi Profil Gaji Karyawan & Pengaturan BPJS

- **Penjelasan Fitur**: Setiap karyawan memiliki profil gaji tersendiri yang menjadi dasar perhitungan bulanan. Upah yang didaftarkan ke BPJS dipisahkan secara eksplisit dari gaji aktual agar penyesuaian gaji internal tidak merusak perhitungan iuran resmi BPJS sebelum dilaporkan.
- **Langkah Penggunaan**:
  1. Buka menu **Users → Employee**, pilih salah satu karyawan, lalu buka halaman profilnya.
  2. Gulir ke kartu **Konfigurasi Penggajian** (hanya terlihat oleh staf dengan hak akses `payrollProfile.readSensitive`).
  3. Masukkan nominal Gaji Pokok, Tunjangan Jabatan, Uang Transportasi, Uang Makan, dan Insentif.
  4. Aktifkan sakelar kepesertaan **BPJS Kesehatan** dan/atau **BPJS Ketenagakerjaan**, lalu isi nilai **Upah Terdaftar BPJS**.
  5. Lengkapi data rekening bank karyawan (Nama Bank, Nomor Rekening, Atas Nama).
  6. Klik **Simpan** untuk memperbarui profil.

---

### 3. Akses Mandiri Slip Gaji Karyawan (Self-Service)

- **Penjelasan Fitur**: Memberikan kemudahan bagi setiap staf untuk melihat rincian gaji yang telah dibayarkan tanpa memerlukan intervensi tim keuangan.
- **Langkah Penggunaan**:
  1. Klik foto profil pengguna di pojok kanan atas, lalu pilih **Profile**.
  2. Buka tab **Slip Gaji Saya**.
  3. Klik salah satu periode slip gaji yang ingin dilihat untuk membuka rincian lengkapnya.
  4. Klik tombol **Cetak Slip** untuk mencetak langsung atau menyimpannya sebagai dokumen PDF.
