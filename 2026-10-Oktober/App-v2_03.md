# 📝 Daily Work Report - Dedy S.N Putra (2026-10-03)

---

## 📅 Laporan Harian - 3 Oktober 2026

> Laporan ini mencakup seluruh sesi pekerjaan dan integrasi per 3 Oktober 2026, yang berfokus pada lima pilar peningkatan sistem monorepo DEKASIMAL V2:
> 1. **Modul Pengelolaan Arsip Dokumen Administrasi Perusahaan (`issue-366`)**: Implementasi sistem pengarsipan surat, memo, dan dokumen legal administrasi internal dengan penomoran unik otomatis berformat romawi, pratinjau berkas PDF langsung di peramban, penyimpanan berkas MinIO, serta manajemen hak akses berbasis peran granular.
> 2. **Dialog & Catatan Validasi Alasan Pelanggan Pasif (`issue-348`)**: Implementasi modal dialog interaktif berbasis Headless UI Dialog untuk pencatatan wajib alasan penonaktifan pelanggan pada saat pembaruan status ke "Pasif", pencegahan kehilangan fokus kursor pada textarea, serta audit trail alasan pasif di profil pelanggan dan tabel data.
> 3. **Dashboard Operasional Tiket & Analitik Performa Jaringan/Layanan (`issue-339`)**: Perancangan menyeluruh modul dashboard analitik operasional (`/dashboards/operational`) dengan agregasi MongoDB untuk 8 kategori tiket, metrik waktu resolusi (MTTR & SLA), peta sebaran geografis titik insiden, komposisi aktivitas teknisi, pola akar masalah gangguan (*root cause*), rekap perubahan paket internet, drawer rincian daftar tiket interaktif, ekspor laporan (CSV, Excel, PDF), dan pengaturan ambang batas KPI di halaman Pengaturan Aplikasi.
> 4. **Sistem Backup Database Cloudflare R2, Retensi Dinamis, Notifikasi Telegram & Pembersihan Berkas Yatim (`issue-358`)**: Penambahan Cloudflare R2 sebagai tujuan pencadangan database berbasis protokol S3, batas retensi pencadangan dinamis (1–365 berkas) untuk Google Drive dan R2 dengan rotasi otomatis, penamaan berkas terstandarisasi berbasis zona waktu aplikasi, notifikasi hasil pencadangan ke channel debug Telegram, utilitas pembersihan berkas sementara yatim piatu (*orphan temp sweep*), dan antarmuka manajemen di menu Pengembang.
> 5. **Peninjauan & Persiapan Integrasi Partner API Scope, Node & Site (`issue-359`)**: Branch aktif saat ini yang menyediakan endpoint RESTful cakupan spasial Node (ODP/JB) dan Site (Tower/POP) bagi mitra, pembatasan hak akses berbasis scope kontrak, penanganan error titik koordinat lokasi, dan rangkaian pengujian integrasi.

---

## 🌿 Branch: `origin/issue-366` — Modul Pengelolaan Arsip Dokumen Administrasi Perusahaan (Penomoran Romawi Otomatis, Pratinjau PDF, & Akses Granular)

### 📌 Informasi Issue

- **Nomor Issue**: #366
- **Judul Issue**: Pengelolaan Arsip Dokumen Administrasi Perusahaan (Arsip > Administrasi)
- **Status Branch**: `Belum di-merge` (Branch remote `origin/issue-366`, commit `8a553e9c` siap untuk proses pengujian & penggabungan ke master)

### 📅 Rincian Commit

#### [[`8a553e9c`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #366 - 3 Oktober 2026, 18:05:29 WIB

- **Komponen yang Berubah**:
  - **Skema & Model Backend (`backend/src/`)**:
    - [`backend/src/models/administrationDocument.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/administrationDocument.model.js) [NEW]:
      - Mendefinisikan skema Mongoose `AdministrationDocumentSchema` untuk dokumen administrasi yang diunggah pengguna: `title` (maks. 200 karakter), `document_number` (unik, otomatis tersusun dari format `{urutan}/{kode}/{bulanRomawi}/{tahun}`), `document_date`, `category`, `description`, `document` (menggunakan `DocumentFileSchema` terintegrasi dengan MinIO Storage), dan referensi akun pembuat `created_by`.
      - Menerapkan plugin `autoIncrementPlugin` bertumpu pada field `document_number` berdasar tahun berjalan dan plugin `softDelete` untuk audit trail.
  - **Kontroler & Logika Bisnis Service Backend (`backend/src/`)**:
    - [`backend/src/controllers/administrationDocument.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/administrationDocument.controller.js) [NEW]:
      - Controller terstandarisasi dengan `asyncHandler`, validasi batas panjang karakter teks (`TEXT_LIMITS`), sanitasi payload dengan `cleanFormData`, penanganan HTTP status code presisi (400, 404, 409, 422), dan response i18n via `req.t()`.
    - [`backend/src/services/administrationDocument.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/administrationDocument.service.js) [NEW]:
      - Logika bisnis terisolasi: pembuatan dokumen dengan resolusi nomor urut tahunan dan konversi bulan ke angka Romawi, pengambilan dokumen dengan paginasi datatable dan penyaringan teks pencarian, pembaruan metadata, serta penghapusan berkas fisik di MinIO Storage saat dokumen dihapus permanen.
  - **Routing & Hak Akses Backend (`backend/src/`)**:
    - [`backend/src/routes/administrationDocument.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/administrationDocument.route.js) [NEW]:
      - Mendaftarkan rute RESTful `/api/v1/administration-document` (list datatable, detail, create, update, delete) lengkap dengan dokumentasi OpenAPI Swagger dan validasi hak akses JWT privilege.
    - [`backend/src/routes/files.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/files.route.js) & [`backend/src/controllers/files.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/files.controller.js):
      - Menambahkan penanganan rute unduh berkas administrasi `/api/v1/file/administration-document/:name` dengan streaming dari MinIO.
    - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) & [`backend/src/config/privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json):
      - Mendaftarkan grup hak akses baru `administrationDocument` (`read`, `create`, `update`, `delete`) beserta pemetaan peran administrator dan deskripsi kamus hak akses.
    - [`backend/src/app.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/app.js):
      - Memasang rute `administrationDocument.route.js` ke dalam pipeline Express v5 aplikasi.
    - [`backend/test/integration/administrationDocument.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/administrationDocument.test.js) [NEW]:
      - 291 baris pengujian integrasi Vitest menguji seluruh siklus CRUD, pengurutan nomor romawi tahunan, upload berkas, dan validasi duplikasi.
  - **Antarmuka Pengguna Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/archive/administration/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/administration/index.jsx) [NEW]:
      - Halaman utama tabel Arsip Administrasi dengan dukungan datatable TanStack Table, tombol Tambah Dokumen (terproteksi `useHasPrivilege('administrationDocument.create')`), pencarian instan, dan penyaringan kategori.
    - [`frontend/src/app/pages/archive/administration/AdministrationDocumentDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/administration/AdministrationDocumentDrawer.jsx) [NEW]:
      - Drawer panel pembuatan dan pengeditan dokumen administrasi: formulir input judul, tanggal dokumen, kategori, kode klasifikasi nomor surat, deskripsi, serta zona drag-and-drop unggah berkas PDF.
    - [`frontend/src/app/pages/archive/administration/AdministrationDocumentPreviewModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/administration/AdministrationDocumentPreviewModal.jsx) [NEW]:
      - Modal pratinjau dokumen PDF terintegrasi langsung di browser tanpa perlu mengunduh file terlebih dahulu.
    - [`frontend/src/app/pages/archive/administration/downloadAdministrationFile.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/administration/downloadAdministrationFile.js) [NEW]:
      - Utilitas unduh berkas PDF terautentikasi melalui Blob stream Axios dengan penamaan file asli.
    - [`frontend/src/app/pages/archive/administration/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/administration/schema/columns.jsx) [NEW] & [`administrationSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/administration/schema/administrationSchema.js) [NEW]:
      - Definisi kolom tabel terstandarisasi (Nomor Dokumen, Judul, Kategori, Tanggal, Pengunggah, Aksi) dan skema validasi form Yup.
    - [`frontend/src/utils/romanNumeral.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/romanNumeral.js) [NEW]:
      - Utilitas murni konversi angka bulan kalender (1–12) ke angka Romawi baku (I–XII) untuk penyusunan nomor surat.
    - [`frontend/src/app/navigation/archive.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/archive.js), [`frontend/src/app/pages/archive/ArchiveIndexRedirect.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/archive/ArchiveIndexRedirect.jsx), & [`frontend/src/app/router/protected.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/protected.jsx):
      - Mendaftarkan entri navigasi Arsip Administrasi (`/archive/administration`) dan routing proteksi halaman.
    - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json):
      - Terjemahan lengkap bilingual untuk seluruh label, validasi, dan notifikasi pesan modul arsip administrasi.
- **Deskripsi Perubahan & Fungsi**:
  - Memfasilitasi penyimpanan terpusat, pengindeksan digital, dan pratinjau dokumen-dokumen administrasi non-kontrak (surat tugas, memo internal, berita acara serah terima, permohonan, dsb.) secara aman dengan penomoran surat otomatis yang konsisten dengan standar kearsipan perusahaan.

---

## 🌿 Branch: `master` / `issue-348` — Dialog & Catatan Validasi Alasan Penonaktifan Pelanggan (Pasif Reason Modal)

### 📌 Informasi Issue

- **Nomor Issue**: #348
- **Judul Issue**: Penambahan Modal Alasan Penonaktifan Pelanggan (Pasif Reason Modal) & Audit Catatan Status Pasif
- **Status Branch**: `Sudah di-merge` (Digabungkan ke branch `master` melalui merge commit `cb6dec77` pada 3 Oktober 2026, 15:53:14 WIB)

### 📅 Rincian Commit

#### [[`cb6dec77`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #348 - 3 Oktober 2026, 15:53:14 WIB
#### [[`064c963c`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve 348 - 30 September 2026, 17:57:52 WIB

- **Komponen yang Berubah**:
  - **Antarmuka Pelanggan Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/users/customer/PasifReasonModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customer/PasifReasonModal.jsx) [NEW]:
      - Komponen modal dialog mandiri berbasis Headless UI Dialog untuk mengonfirmasi perubahan status pelanggan dari aktif menjadi pasif.
      - Dirancang khusus secara independen tanpa memakai `ConfirmModal` global, guna menghindari bug hilangnya fokus kursor (*focus loss*) saat mengetik pada textarea akibat penggabungan objek `description` berulang oleh Lodash.
      - Dilengkapi penghitung karakter dinamis (maksimal 500 karakter), validasi alasan wajib diisi, serta pesan peringatan dampak penonaktifan layanan internet pelanggan.
    - [`frontend/src/app/pages/users/customer/edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customer/edit.jsx):
      - Menghubungkan dropdown perubahan status pelanggan pada formulir edit dengan `PasifReasonModal`; saat admin memilih status "Pasif", modal akan otomatis terbuka sebelum perubahan dikirim ke server.
  - **Kontroler & Validasi Backend (`backend/src/`)**:
    - [`backend/src/controllers/customer.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customer.controller.js) & [`backend/src/routes/customer.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/customer.route.js):
      - Memperketat validasi endpoint penonaktifan pelanggan: parameter alasan pasif (`pasif`) menjadi wajib disertakan dalam request body dengan batas panjang teks aman.
      - Menyimpan riwayat alasan penonaktifan ke dokumen pelanggan untuk keperluan audit dan peninjauan kembali oleh tim penagihan/layanan pelanggan.
  - **Internasionalisasi (`frontend/src/i18n/`)**:
    - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json):
      - Mendaftarkan teks terjemahan judul modal, label masukan alasan penonaktifan, pesan batas karakter, dan teks konfirmasi aksi.
- **Deskripsi Perubahan & Fungsi**:
  - Mencegah perubahan status pelanggan menjadi pasif tanpa dokumentasi alasan yang jelas (misal berhenti berlangganan karena pindah rumah, tarif kompetitor, gangguan berulang, atau masalah keuangan).
  - Meningkatkan kualitas data operasional dan memudahkan analisis churn pelanggan oleh manajemen.

---

## 🌿 Branch: `master` / `issue-339` — Dashboard Operasional Tiket & Analitik Performa Jaringan/Layanan

### 📌 Informasi Issue

- **Nomor Issue**: #339
- **Judul Issue**: Dashboard Operasional Tiket & Analitik Performa Jaringan/Layanan (KPI Strip, Sebaran Wilayah, Peta Insiden, Komposisi Aktivitas, Pola Akar Masalah, Perubahan Paket, & Ekspor Multi-Format)
- **Status Branch**: `Sudah di-merge` (Digabungkan ke branch `master` melalui merge commit `a2022c8c` pada 3 Oktober 2026, 15:03:50 WIB)

### 📅 Rincian Commit

#### [[`a2022c8c`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #339 - 3 Oktober 2026, 15:03:50 WIB
#### [[`7129ffbf`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #339 - 30 September 2026, 12:02:51 WIB

- **Komponen yang Berubah**:
  - **Konstanta & Arsitektur Bisnis Backend (`backend/src/`)**:
    - [`backend/src/constants/operationalDashboard.constant.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/constants/operationalDashboard.constant.js) [NEW]:
      - Definisi terpusat untuk 8 kategori tiket operasional (`survey`, `installation`, `customer`, `partner`, `dismantle`, `payment`, `backbone`, `other`), pemetaan privilege peran pengguna, filter kategori gangguan (`INCIDENT_TICKET_TYPES`), dan konfigurasi default target SLA resolusi waktu.
    - [`backend/src/controllers/operationalDashboard.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/operationalDashboard.controller.js) [NEW]:
      - Controller endpoint analitik: memverifikasi hak akses per tipe tiket pengguna, memproses parameter rentang tanggal, filter cabang, dan menyajikan ringkasan KPI metrik teragregasi.
    - [`backend/src/services/operationalDashboard.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/operationalDashboard.service.js) [NEW]:
      - 973 baris pipeline agregasi MongoDB performa tinggi untuk menghitung:
        - Metrik KPI utama: total tiket masuk, tiket terselesaikan, tiket terbuka, serta rata-rata Mean Time to Resolution (MTTR).
        - Distribusi tiket per area operasional (wilayah coverage / site).
        - Titik koordinat geografis insiden gangguan untuk rendering pada peta.
        - Komposisi tipe aktivitas dan kategori penanganan teknisi.
        - Analisis akar masalah (*root cause analysis*) gangguan terbanyak (kabel optik putus, perangkat terbakar, gangguan daya, dsb.).
        - Rekapitulasi tiket perubahan paket langganan (upgrade vs downgrade).
        - Ekstraksi data mentah untuk kebutuhan ekspor laporan.
    - [`backend/src/routes/ticket.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/ticket.route.js) & [`backend/src/models/ticket.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/ticket.model.js):
      - Menghubungkan rute-rute analitik dashboard operasional `/api/v1/tickets/dashboard/operational/*` dengan autentikasi JWT dan Swagger documentation.
    - [`backend/test/integration/operationalDashboard.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/operationalDashboard.test.js) [NEW]:
      - 516 baris pengujian integrasi Vitest memvalidasi keakuratan agregasi metrik, penyaringan privilege tiket, rentang tanggal, dan perhitungan persentase SLA.
  - **Komponen Visual & Antarmuka Dashboard Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/dashboards/operational/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/index.jsx) [NEW]:
      - Halaman utama Dashboard Operasional dengan layout responsif modern, filter rentang tanggal terpadu, selector area/cabang, dan navigasi lompat cepat antar seksi visual (`SectionNav`).
    - [`frontend/src/app/pages/dashboards/operational/components/OperationalKpiStrip.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/components/OperationalKpiStrip.jsx) [NEW]:
      - Strip kartu metrik KPI dengan indikator tren perbandingan periode sebelumnya, waktu rata-rata penanganan, dan target SLA.
    - [`frontend/src/app/pages/dashboards/operational/components/AreaDistributionTable.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/components/AreaDistributionTable.jsx) [NEW]:
      - Tabel interaktif persebaran tiket per wilayah/area dengan persentase beban kerja dan status penanganan.
    - [`frontend/src/app/pages/dashboards/operational/components/AreaMapCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/components/AreaMapCard.jsx) [NEW]:
      - Komponen visual peta Leaflet interaktif yang memetakan titik-titik lokasi insiden gangguan berdasarkan koordinat pelanggan/site.
    - [`frontend/src/app/pages/dashboards/operational/components/ActivityCompositionCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/components/ActivityCompositionCard.jsx) [NEW]:
      - Visualisasi diagram donat ApexCharts interaktif untuk komposisi 8 jenis aktivitas operasional lapangan.
    - [`frontend/src/app/pages/dashboards/operational/components/RootCauseCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/components/RootCauseCard.jsx) [NEW]:
      - Grafik batang horizontal peringkat akar masalah gangguan untuk memudahkan identifikasi titik rapuh jaringan.
    - [`frontend/src/app/pages/dashboards/operational/components/PackageChangeCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/components/PackageChangeCard.jsx) [NEW]:
      - Kartu ringkasan aktivitas upgrade dan downgrade paket kecepatan internet pelanggan.
    - [`frontend/src/app/pages/dashboards/operational/components/TicketListDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/components/TicketListDrawer.jsx) [NEW]:
      - Drawer interaktif daftar tiket yang terbuka saat pengguna mengklik angka metrik atau baris tabel untuk melihat detail tiket yang berkontribusi pada angka tersebut.
    - [`frontend/src/app/pages/dashboards/operational/components/ExportMenu.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/components/ExportMenu.jsx) [NEW] & [`exportReport.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/dashboards/operational/utils/exportReport.js) [NEW]:
      - Fitur ekspor laporan analitik operasional ke format file CSV, Microsoft Excel (.xlsx), dan dokumen PDF siap cetak.
    - [`frontend/src/app/pages/settings/sections/OperationalSettings.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/OperationalSettings.jsx) [NEW] & [`frontend/src/app/pages/settings/sections/Application.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/Application.jsx):
      - Form konfigurasi ambang batas target SLA (dalam jam) dan target penyelesaian tiket operasional pada Pengaturan Aplikasi.
    - [`frontend/src/app/navigation/dashboards.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/dashboards.js) & [`frontend/src/app/router/protected.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/protected.jsx):
      - Mendaftarkan rute navigasi `/dashboards/operational` di bawah menu Dashboard.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan visibilitas menyeluruh bagi manajemen operasional, Network Operation Center (NOC), dan tim teknis mengenai beban kerja tiket harian, kecepatan respons penanganan insiden, persebaran geografis titik masalah, dan efektivitas tim lapangan.

---

## 🌿 Branch: `production` / `issue-358` — Sistem Backup Database Cloudflare R2, Retensi Dinamis, Notifikasi Telegram & Pembersihan Berkas Yatim

### 📌 Informasi Issue

- **Nomor Issue**: #358
- **Judul Issue**: Sistem Backup Database Cloudflare R2, Retensi Dinamis (1–365 Berkas), Notifikasi Debug Telegram, Standarisasi Waktu Penamaan Berkas, & Pembersihan Berkas Yatim (`DbTools`)
- **Status Branch**: `Sudah di-merge` (Digabungkan ke branch `production` via merge commit `371d5ef6` pada 3 Oktober 2026, 01:34:12 WIB)

### 📅 Rincian Commit

#### [[`371d5ef6`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #358 - 3 Oktober 2026, 01:34:12 WIB
#### [[`20211dce`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #358 - 3 Oktober 2026, 00:49:03 WIB

- **Komponen yang Berubah**:
  - **Spesifikasi & Panduan Desain (`docs/`)**:
    - [`docs/superpowers/specs/2026-10-03-db-backup-r2-retention-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-10-03-db-backup-r2-retention-design.md) [NEW]:
      - Dokumen arsitektur lengkap spesifikasi teknis integrasi Cloudflare R2, rotasi retensi cadangan database, format penamaan berkas dengan zona waktu, notifikasi Telegram, dan pembersihan berkas yatim.
  - **Klien & Utilitas Penyimpanan Backend (`backend/src/utils/`)**:
    - [`backend/src/utils/r2.util.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/r2.util.js) [NEW]:
      - Klien S3-compatible Cloudflare R2 bertumpu pada `Minio.Client`: konfigurasi endpoint `<account_id>.r2.cloudflarestorage.com`, port 443, SSL aktif, region 'auto'.
      - Menyediakan fungsi murni `uploadFileToR2({ filePath, filename })`, `deleteFileFromR2(objectName)`, dan `testR2Connection({ accountId, accessKeyId, secretAccessKey, bucket })`.
      - Penegakan keamanan kredensial: zero-secrets pada log dan pesan error.
    - [`backend/src/utils/backupNaming.util.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/backupNaming.util.js) [NEW]:
      - Standarisasi penamaan berkas cadangan: `<nama_db>_YYYY-MM-DD_HH-mm-ss.archive.gz` menggunakan format waktu lokal sesuai preferensi zona waktu aplikasi (`application_settings.timezone` via `Intl.DateTimeFormat`, fallback `Asia/Jakarta`).
    - [`backend/src/utils/gdriveRedirect.util.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/gdriveRedirect.util.js) [NEW]:
      - Penanganan perbaikan skema protokol redirect URI Google Drive OAuth2 agar selalu dipaksa memakai HTTPS di balik proxy produksi.
    - [`backend/src/utils/googleDrive.util.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/googleDrive.util.js):
      - Integrasi penyesuaian rotasi retensi dinamis pada storage Google Drive.
  - **Layanan Bisnis & Background Worker Backend (`backend/src/services/` & `jobs/`)**:
    - [`backend/src/services/dbBackupRetention.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/dbBackupRetention.service.js) [NEW]:
      - Service rotasi cadangan otomatis (`rotateOldBackups(destination)`): mencari job `minio_backup` berstatus `success` pada database, melewati berkas aktif sejumlah nilai retensi yang dikonfigurasi admin (1–365 berkas, default 10), lalu menghapus fisik berkas cadangan tertua dari storage tujuan (Google Drive atau Cloudflare R2).
    - [`backend/src/services/dbBackupUpload.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/dbBackupUpload.service.js) [NEW]:
      - Abstraksi pengunggahan arsip cadangan ke storage tujuan (MinIO lokal, Google Drive, atau Cloudflare R2).
    - [`backend/src/services/dbToolsR2.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/dbToolsR2.service.js) [NEW]:
      - Service pengelolaan kredensial R2 di `system_settings`: status koneksi (`getR2Status`), simpan konfigurasi dengan enkripsi rahasia (`saveR2Config`), putuskan koneksi (`disconnectR2`), dan pembaruan batas retensi (`updateBackupRetention`).
    - [`backend/src/services/dbToolsTempSweep.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/dbToolsTempSweep.service.js) [NEW]:
      - Fungsi `sweepOrphanTempFiles()` yang dieksekusi saat worker pertama kali berjalan untuk membersihkan file arsip sementara di direktori `temp/db-tools/` yang ditinggalkan oleh job yang gagal/usang, tanpa mengganggu job yang sedang berjalan.
    - [`backend/src/services/dbBackupNotify.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/dbBackupNotify.service.js) [NEW]:
      - Notifikasi Telegram terstruktur ke channel `debug`: melaporkan status backup berhasil atau gagal, nama database, tujuan penyimpanan, ukuran berkas (dihitung sebelum file dihapus), durasi eksekusi, serta jumlah berkas lama yang dirotasi.
    - [`backend/src/jobs/dbToolsWorker.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/jobs/dbToolsWorker.js):
      - Mengintegrasikan seluruh siklus backup baru: pembersihan berkas yatim saat startup, proses upload ke R2, eksekusi rotasi retensi, dan pengiriman notifikasi Telegram.
    - [`backend/src/services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js):
      - Mendaftarkan key pengaturan baru: `r2_account_id`, `r2_access_key_id`, `r2_bucket`, `r2_secret_access_key` (terenkripsi dan dilindungi dari eksposur API publik), `gdrive_backup_retention`, dan `r2_backup_retention`.
    - [`backend/src/controllers/dbTools.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/dbTools.controller.js) & [`backend/src/routes/dbTools.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/dbTools.route.js):
      - Endpoint RESTful baru `/db-tools/r2/*` dan `/db-tools/backup-retention` terproteksi hak akses developer dengan dokumentasi Swagger komprehensif.
  - **Pengujian Unit & Integrasi Backend (`backend/test/`)**:
    - [`backend/test/integration/dbToolsR2.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/dbToolsR2.controller.test.js) [NEW] (116 baris)
    - [`backend/test/integration/dbToolsR2.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/dbToolsR2.service.test.js) [NEW] (181 baris)
    - [`backend/test/integration/dbToolsR2Destination.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/dbToolsR2Destination.test.js) [NEW] (125 baris)
    - [`backend/test/integration/dbBackupRetention.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/dbBackupRetention.service.test.js) [NEW] (237 baris)
    - [`backend/test/integration/dbBackupUpload.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/dbBackupUpload.service.test.js) [NEW] (155 baris)
    - [`backend/test/integration/dbToolsTempSweep.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/dbToolsTempSweep.service.test.js) [NEW] (81 baris)
    - [`backend/test/integration/googleDriveDelete.util.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/googleDriveDelete.util.test.js) [NEW] (62 baris)
    - [`backend/test/integration/r2.util.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/r2.util.test.js) [NEW] (178 baris)
    - [`backend/test/integration/settingsSystemDedicatedKeys.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/settingsSystemDedicatedKeys.test.js) [NEW] (73 baris)
    - [`backend/test/unit/backupNaming.util.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/backupNaming.util.test.js) [NEW] (95 baris)
    - [`backend/test/unit/dbBackupNotify.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/dbBackupNotify.service.test.js) [NEW] (161 baris)
    - [`backend/test/unit/gdriveRedirect.util.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/gdriveRedirect.util.test.js) [NEW] (32 baris)
  - **Antarmuka Pengembang Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/R2ConnectionCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/dbTools/R2ConnectionCard.jsx) [NEW]:
      - Kartu konfigurasi Cloudflare R2 pada tab DbTools: form input Account ID, Access Key ID, Secret Access Key (`InputPassword`), Bucket, tombol Tes Koneksi, tombol Simpan, dan tombol Putuskan.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/BackupRetentionInput.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/dbTools/BackupRetentionInput.jsx) [NEW]:
      - Komponen masukan angka batas retensi berkas cadangan (1–365) untuk kartu Google Drive dan Cloudflare R2.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/BackupNowModal.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/dbTools/BackupNowModal.jsx) & [`BackupScheduleForm.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/dbTools/BackupScheduleForm.jsx):
      - Menambahkan Cloudflare R2 sebagai opsi tujuan penyimpanan pencadangan instan dan pencadangan terjadwal.
    - [`frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectionCard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/dbTools/GdriveConnectionCard.jsx), [`JobProgressPanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/dbTools/JobProgressPanel.jsx), [`MinioBackupPanel.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/dbTools/MinioBackupPanel.jsx), & [`DbToolsTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/developer/dbTools/DbToolsTab.jsx):
      - Pembaruan status konektivitas, label tujuan, dan progres bar unggah backup.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan solusi cadangan database sekunder yang sangat andal dan hemat biaya melalui Cloudflare R2 tanpa biaya egress data.
  - Memastikan media penyimpanan remote tidak membengkak berkat rotasi retensi otomatis, serta memberikan pemberitahuan dini bagi tim DevOps/developer melalui Telegram setiap kali proses backup berjalan.

---

## 🌿 Branch: `issue-359` — Partner API Scope, Node & Site Location Integration

### 📌 Informasi Issue

- **Nomor Issue**: #359
- **Judul Issue**: Integrasi Partner API Scope, Node ODP/JB & Site POP/Tower Location
- **Status Branch**: `Belum di-merge` (Branch aktif lokal saat ini `issue-359`, tracking `origin/issue-359`, dalam status peninjauan & verifikasi integrasi)

### 📅 Rincian Commit

#### [[`58bc5b3a`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #359 - 2 Oktober 2026, 16:38:04 WIB

- **Komponen yang Berubah**:
  - **Middleware & Keamanan Partner API (`backend/src/utils/`)**:
    - [`backend/src/utils/partner-api-scope.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/partner-api-scope.js) [NEW]:
      - Utilitas validasi cakupan (*scope*) otorisasi partner API: memastikan token mitra hanya dapat mengakses resource (node, site, titik lokasi) yang tercakup dalam izin kemitraan mereka.
    - [`backend/src/utils/location-error.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/location-error.js) [NEW]:
      - Standardisasi penanganan error titik lokasi: validasi rentang koordinat latitude/longitude, duplikasi titik, dan error resolusi geospasial.
  - **Kontroler & Rute Partner API (`backend/src/controllers/` & `routes/`)**:
    - [`backend/src/controllers/partnerApiNode.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiNode.controller.js) [NEW]:
      - Controller penyedia endpoint RESTful manajemen data Node jaringan bagi mitra.
    - [`backend/src/controllers/partnerApiSite.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiSite.controller.js) [NEW]:
      - Controller penyedia endpoint RESTful manajemen data Site/Tower/POP bagi mitra.
    - [`backend/src/routes/partnerApi.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/partnerApi.route.js):
      - 839 baris definisi rute Partner API terproteksi API Key & Scope dengan dokumentasi OpenAPI Swagger.
    - [`backend/src/services/locationPoint.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/locationPoint.service.js):
      - Peningkatan query spasial dan agregasi data koordinat titik jaringan.
  - **Pengujian Integrasi (`backend/test/integration/`)**:
    - [`backend/test/integration/partnerApiNode.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/partnerApiNode.test.js) [NEW] (301 baris)
    - [`backend/test/integration/partnerApiSite.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/partnerApiSite.test.js) [NEW] (259 baris)
- **Deskripsi Perubahan & Fungsi**:
  - Membuka kapabilitas integrasi B2B yang aman bagi mitra penyedia infrastruktur untuk mengakses dan memelihara data titik koordinat Node serta Site secara terprogram tanpa membuka data internal sistem yang tidak relevan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #366  | Arsip Dokumen Administrasi | Penyimpanan, pencarian, dan pratinjau PDF terpusat dokumen administrasi internal dengan penomoran Romawi otomatis. |
| #348  | Modal Alasan Pelanggan Pasif | Pencatatan wajib alasan penonaktifan pelanggan pada UI/API dengan perbaikan masalah kehilangan fokus kursor textarea. |
| #339  | Dashboard Operasional Tiket | Visibilitas analitik menyeluruh performa tiket operasional, peta sebaran gangguan, analisis akar masalah, dan ekspor data multi-format. |
| #358  | Backup R2, Retensi & Notifikasi | Penyimpanan cadangan database hemat biaya ke Cloudflare R2, rotasi retensi otomatis, notifikasi Telegram debug, & sweep berkas yatim. |
| #359  | Partner API Scope, Node & Site | Akses RESTful terproteksi scope bagi mitra untuk query dan kelola data spasial Node serta Site infrastruktur jaringan. |

### Kemampuan Baru Pengguna/Admin

- **Admin Kearsipan / Sekretariat**: Dapat mengunggah dokumen administrasi (format PDF), mengelompokkannya per kategori, melihat pratinjau dokumen langsung di peramban, serta nomor surat tersusun otomatis dengan urutan tahunan dan bulan Romawi.
- **Admin Layanan Pelanggan (CS/Billing)**: Saat mengubah status pelanggan menjadi "Pasif", diwajibkan mengisi alasan secara eksplisit pada modal dialog khusus yang memvalidasi batas karakter dan menyimpan alasan tersebut ke profil pelanggan.
- **Manajemen Operasional & NOC**: Dapat memonitor performa layanan pelanggan secara *realtime* di Dashboard Operasional (`/dashboards/operational`), melihat sebaran gangguan pada peta geografis, menganalisis jenis gangguan paling sering terjadi (*root cause*), dan mengekspor laporan kinerja ke format Excel, PDF, atau CSV.
- **DevOps & Developer**: Dapat menghubungkan penyimpanan cadangan database ke Cloudflare R2, menguji koneksi secara instan dari antarmuka Pengaturan Pengembang, menentukan jumlah batas berkas backup (retensi), serta menerima notifikasi otomatis di grup Telegram debug setiap kali proses pencadangan selesai.

### Bug Fix / Solusi Masalah

- **Penyelesaian Masalah Fokus Input Teks (`PasifReasonModal`)**: Menghilangkan bug hilangnya fokus kursor (*cursor unmount/focus drop*) pada saat mengetik alasan penonaktifan pelanggan dengan memisahkan dialog dari `ConfirmModal` global.
- **Pembersihan Berkas Yatim Piatu (`sweepOrphanTempFiles`)**: Mengatasi penumpukan file arsip sementara `.archive.gz` yang tertinggal di folder `temp/db-tools/` akibat job backup yang terputus atau gagal di masa lalu.
- **Normalisasi Skema Redirect URI HTTPS (`gdriveRedirect.util.js`)**: Memperbaiki kegagalan pertukaran token Google OAuth2 akibat skema HTTP yang tidak sesuai saat aplikasi berada di balik reverse-proxy SSL/HTTPS.

### Menu/Fitur Baru

- **Menu Arsip > Administrasi** (`/archive/administration`): Halaman tabel pengarsipan dokumen surat/memo internal.
- **Menu Dashboard > Operasional** (`/dashboards/operational`): Halaman dasbor analitik tiket dan metrik operasional lapangan.
- **Pengaturan Aplikasi > Operasional**: Konfigurasi target waktu resolusi SLA tiket pada Pengaturan.
- **Pengaturan Pengembang > DbTools > Cloudflare R2**: Kartu koneksi R2 dan input pengaturan retensi backup Google Drive & R2.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Menghubungkan dan Menggunakan Cloudflare R2 untuk Cadangan Database

- **Penjelasan Fitur**: Sistem pencadangan database kini mendukung Cloudflare R2 sebagai tujuan sekunder selain MinIO lokal dan Google Drive. R2 menyediakan kompatibilitas API S3 dengan efisiensi biaya penyimpanan tinggi tanpa biaya transfer data keluar (*zero egress fee*). Berkas cadangan yang disimpan otomatis dirotasi sesuai batas retensi yang ditentukan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Pengaturan** > tab **Pengembang** (`Developer`) > sub-tab **Database Tools** (`DbTools`).
  2. Temukan kartu **Koneksi Cloudflare R2**:
     - Masukkan **Account ID** Cloudflare Anda (32 karakter hex).
     - Masukkan **Access Key ID** dan **Secret Access Key** R2 yang memiliki izin *Object Read & Write*.
     - Masukkan nama **Bucket** penyimpanan yang telah dibuat di Cloudflare.
  3. Klik tombol **Tes Koneksi**. Pastikan muncul notifikasi hijau bahwa koneksi ke bucket berhasil diverifikasi.
  4. Masukkan batas **Retensi Berkas Cadangan** (misalnya `15` berkas) agar sistem otomatis merotasi cadangan lama.
  5. Klik **Simpan Pengaturan**.
  6. Untuk melakukan pencadangan manual segera: klik tombol **Backup Database Sekarang**, pilih tujuan **Cloudflare R2**, lalu konfirmasi.
  7. Pantau progres pencadangan pada panel **Status Job Terakhir** dan periksa pesan notifikasi ringkasan yang dikirim ke grup Telegram channel debug.

### 2. Mengakses dan Menganalisis Dashboard Operasional Tiket

- **Penjelasan Fitur**: Modul Dashboard Operasional menyatukan data operasional dari 8 kategori tiket ke dalam satu antarmuka terintegrasi yang menyajikan metrik KPI SLA, sebaran wilayah, peta insiden geospasial, komposisi aktivitas teknisi, hingga rekap perubahan paket internet pelanggan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu navigasi utama > klik **Dashboard** > pilih **Operasional** (`/dashboards/operational`).
  2. Gunakan **Filter Bar** di bagian atas untuk memilih rentang tanggal analisis (misalnya: *7 Hari Terakhir*, *Bulan Ini*, atau *Kustom*) dan saring berdasarkan Cabang/Area tertentu jika diinginkan.
  3. Perhatikan **KPI Strip** untuk melihat rasio penyelesaian tiket dan rata-rata durasi penyelesaian (MTTR) dibandingkan dengan target SLA.
  4. Gulir ke bawah menuju **Peta Sebaran Insiden** untuk melihat klaster gangguan di lapangan. Klik salah satu penanda pada peta atau baris pada **Tabel Sebaran Area** untuk membuka drawer rincian daftar tiket terkait.
  5. Periksa grafik **Akar Masalah Gangguan** untuk melihat faktor teknis apa yang paling sering menimbulkan gangguan pada periode tersebut.
  6. Untuk mengunduh rekapitulasi data, klik tombol **Ekspor** di pojok kanan atas, lalu pilih format yang dibutuhkan: **Ekspor Excel (.xlsx)**, **Ekspor CSV**, atau **Cetak Dokumen PDF**.
