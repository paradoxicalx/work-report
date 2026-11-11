# 📝 Daily Work Report - Dedy S.N Putra (2026-09-09)

---

## 📅 Laporan Harian - 9 September 2026

---

## 🌿 Branch: `issue-278` — Modul Pengelolaan Berita & Promo Aplikasi Mobile (Mobile News & Promotion Management)

### 📌 Informasi Issue

- **Nomor Issue**: #278
- **Judul Issue**: Modul Beranda Aplikasi Mobile: Pengelolaan Banner Promosi & Artikel Informasi Pelanggan (Mobile News & Promotion Management)
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [4915703] - resolve #278 - Rabu, 9 September 2026, 23:21:55 WIB

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/files.controller.js`
  - `backend/src/controllers/news.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/news.model.js`
  - `backend/src/routes/files.route.js`
  - `backend/src/routes/news.route.js` [NEW]
  - `backend/src/services/news.service.js`
  - `frontend/src/app/navigation/index.js`
  - `frontend/src/app/navigation/mobileApp.js` [NEW]
  - `frontend/src/app/pages/mobileApp/news/create.jsx` [NEW]
  - `frontend/src/app/pages/mobileApp/news/edit.jsx` [NEW]
  - `frontend/src/app/pages/mobileApp/news/index.jsx` [NEW]
  - `frontend/src/app/pages/mobileApp/news/schema/GridCard.jsx` [NEW]
  - `frontend/src/app/pages/mobileApp/news/schema/NewsTypeBadge.jsx` [NEW]
  - `frontend/src/app/pages/mobileApp/news/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/mobileApp/news/schema/createSchema.js` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/table/GridView.jsx`
  - `frontend/src/components/shared/table/Table.jsx`
  - `frontend/src/components/shared/table/status.js`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Arsitektur Backend REST API & Koleksi Berita Mobile (`news.route.js`, `news.controller.js`, `news.service.js`, `news.model.js`)**:
    - Membangun endpoint REST API lengkap untuk mengelola konten berita dan banner promosi pada aplikasi mobile pelanggan di rute `/news/*` (`createNews`, `updateNews`, `deleteNews`, `findNewsById`, `findAllNewsForTable`, `getNewsStats`, dan `findMultipleBanner`).
    - Model database Mongoose `MobileNews` (`news.model.js`) menyimpan field: `news_id`, `title`, `type` (`news` / `article`), `content`, `image` (nama berkas di MinIO), `show` (status publikasi aktif/draf), `created_at`, dan `created_by`.
    - Mendaftarkan permission privilege baru pada `backend/src/config/privilege.json`: `mobileNews.list`, `mobileNews.read`, `mobileNews.create`, `mobileNews.update`, dan `mobileNews.delete`.
  - **Penguatan Keamanan & Pencegahan IDOR pada Berkas Publik (`files.controller.js`, `files.route.js`)**:
    - Memperbaiki kerentanan *Arbitrary File Read / Information Disclosure* pada endpoint `GET /api/v1/file/news/:name` dengan menambahkan validasi kepemilikan berkas `await NewsModel.exists({ image: req.params.name })`.
    - Mencegah akses tidak terotorisasi ke dokumen identitas sensitif (seperti foto KTP pelanggan dan dokumen vendor) yang tersimpan di dalam bucket bersama yang sama (`buckets.appFiles`).
  - **Pencegahan Kebocoran Disk & Siklus Upload Berkas (`news.controller.js`)**:
    - Menambahkan pembersihan berkas temporer lokal `await fs.unlink(imageFile.tempFilePath).catch(() => {})` pada validasi tipe/ukuran gambar yang gagal serta setelah pengunggahan ke MinIO berhasil, guna mencegah penumpukan berkas sampah pada disk server (*disk exhaustion*).
    - Memperbaiki alur eksekusi pada `createNews`: validasi kelengkapan data wajib dan keunikan judul dilakukan sebelum operasi upload ke MinIO dijalankan, guna mencegah terciptanya *orphaned files* di penyimpanan objek MinIO.
    - Menstandarisasi penanganan error duplikasi judul berita (`E11000 duplicate key error`) dengan mengembalikan status HTTP `409 Conflict` yang informatif.
  - **Optimasi Kueri Database & Format Response (`news.service.js`, `news.controller.js`)**:
    - Menambahkan optimasi alokasi memori Mongoose dengan menyertakan `.lean()` dan pembatasan proyeksi field eksplisit `.select('news_id title type content image show created_at created_by')` pada pembacaan detail berita (`findNewsById`) serta banner publik (`findMultipleBanner`).
    - Mengoptimalkan fungsi ringkasan statistik `getNewsStats` dari 4x pemanggilan `countDocuments` menjadi kueri agregasi tunggal berbasis `$facet`.
    - Menghapus pembungkus respons `{ success: true, data }` pada endpoint `statsNews` dan `readNews` agar mengembalikan data mentah langsung sesuai panduan baku monorepo AGENTS.md §2.A Aturan 2.
    - Menangani konversi ID numerik vs MongoDB ObjectId secara aman pada `findNewsById` guna mencegah crash `CastError` Mongoose.
  - **Antarmuka Pengguna Beranda Aplikasi Mobile (`frontend/src/app/pages/mobileApp/news/`)**:
    - Membangun halaman dashboard utama manajemen konten beranda mobile (`index.jsx`) dengan dukungan tampilan ganda (*Dual View*): **Table View** (TanStack Datatable) dan **Grid Card View** (`GridCard.jsx`).
    - Menyediakan 4 kartu KPI ringkasan statistik berita di atas tabel (Total Konten, Aktif Tayang, Berita Promo, dan Artikel Informasi) yang terhubung langsung ke endpoint `GET /news/stats`.
    - Halaman tambah konten baru (`create.jsx`) dan drawer pengubahan konten (`edit.jsx`) dengan pratinjau gambar, input tipe konten, toggle publikasi, serta validasi form schema Yup.
    - Komponen visual `NewsTypeBadge.jsx` untuk membedakan badge tipe Berita Promo dan Artikel Informasi.
  - **Optimasi Frontend & Lokalisasi (i18n)**:
    - Membungkus skema kolom tabel dengan `useMemo` guna mencegah kalkulasi ulang berulang pada setiap render tabel.
    - Mengeliminasi siklus *re-fetch* data berulang pada `NewsEditDrawer` dengan membersihkan dependensi `useEffect`.
    - Mendaftarkan deskripsi privilege pada `privilegeDescriptions.id.json` dan `en.json`.
    - Melengkapi seluruh string terjemahan bahasa Indonesia dan bahasa Inggris pada namespace `news`.

---

## 🌿 Branch: `issue-244` — Modul Dokumen Direktur & Surat Keputusan (SK) Direksi / Persetujuan Digital (Digital Signature Approval)

### 📌 Informasi Issue

- **Nomor Issue**: #244
- **Judul Issue**: Modul Arsip Ketetapan Direktur & Surat Keputusan (SK) Direksi / Alur Persetujuan & Penandatanganan Digital (Digital Signature Placement & Approval)
- **Status Branch**: `Sudah di-merge` (Di-merge ke `master` via commit `aabb7b6`)

### 📅 Rincian Commit

#### [aabb7b6] - resolve #244 (Merge commit to master) - Rabu, 9 September 2026, 21:33:09 WIB

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/package-lock.json`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/direkturDocument.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/customerPO.model.js`
  - `backend/src/models/direkturDocument.model.js` [NEW]
  - `backend/src/models/shared/documentFile.schema.js` [NEW]
  - `backend/src/models/shared/signaturePosition.schema.js` [NEW]
  - `backend/src/models/vendorSO.model.js`
  - `backend/src/routes/directorDocument.route.js` [NEW]
  - `backend/src/routes/files.route.js`
  - `backend/src/services/direkturDocument.service.js` [NEW]
  - `backend/src/utils/roman-numeral.js` [NEW]
  - `backend/src/utils/telegram.js`
  - `frontend/src/app/navigation/archive.js` [NEW]
  - `frontend/src/app/navigation/index.js`
  - `frontend/src/app/pages/archive/decree/direktur/DirekturGeneratedDocumentPreview.jsx` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/DocumentPreview.jsx` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/ReviewDrawer.jsx` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/SignPositionStep.jsx` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/constants/direkturTemplates.js` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/create.jsx` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/edit.jsx` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/index.jsx` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/schema/direkturSchema.js` [NEW]
  - `frontend/src/app/pages/archive/decree/direktur/useDirekturSignFlow.js` [NEW]
  - `frontend/src/app/pages/archive/decree/index.jsx` [NEW]
  - `frontend/src/app/pages/public/ReviewDirekturDocumentPage.jsx` [NEW]
  - `frontend/src/app/pages/users/document/index.jsx`
  - `frontend/src/app/pages/users/document/pks/index.jsx`
  - `frontend/src/app/pages/users/document/sdn/index.jsx`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/DocumentPreviewModal.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/constants/privilegeDescriptions.en.json`
  - `frontend/src/constants/privilegeDescriptions.id.json`
  - `frontend/src/hooks/index.js`
  - `frontend/src/hooks/useDocumentApproval.js`
  - `frontend/src/hooks/usePdfSignaturePlacement.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`

- **Deskripsi Perubahan & Fungsi**:
  - **Audit Mutu Perangkat Lunak, Keamanan & Remediasi Menyeluruh**:
    - Melakukan audit komprehensif terhadap seluruh berkas branch `issue-244` yang menghasilkan dokumen temuan `audit-report-issue-244.md` dan checklist perbaikan `audit-task-issue-244.md`.
    - Menyelesaikan 100% temuan audit meliputi keamanan otorisasi RBAC, penguncian integritas dokumen legal, proteksi penimpaan file storage MinIO, serta eliminasi celah race condition sebelum digabungkan ke branch utama `master`.
  - **Sistem Manajemen Dokumen Direktur & Surat Keputusan (`direkturDocument.model.js`, `direkturDocument.controller.js`, `direkturDocument.service.js`)**:
    - Merancang schema Mongoose `DirekturDocument` yang mendukung dua mode pembuatan dokumen resmi:
      1. **Mode Upload**: Menyimpan metadata berkas PDF hasil unggahan pengguna beserta koordinat penempatan tanda tangan digital (koordinat X, Y, nomor halaman, dan rasio dimensi).
      2. **Mode Generated**: Menyimpan struktur dokumen formal terstruktur (nomor surat resmi, perihal, konsideran Menimbang, konsideran Mengingat, data penunjukan PIC/organisasi, serta rangkaian pasal/klausul keputusan).
    - Membangun utilitas format penomoran surat resmi berbasis angka romawi (`utils/roman-numeral.js`) untuk standardisasi nomor ketetapan dinamis (contoh: `001/SK-DIR/IX/2026`).
    - Menyediakan endpoint lengkap untuk pembuatan draft, pengubahan informasi, pratinjau dokumen, pengajuan persetujuan, persetujuan dan pembubuhan tanda tangan digital, serta penolakan dokumen.
  - **Pengamanan Hak Akses & Integritas Dokumen Legal (Security & Legal Integrity)**:
    - Memproteksi rute kategori `POST /director-document/category-select` dengan otorisasi `checkPrivilege('direkturDocument.read')` agar data klasifikasi internal tidak dapat diakses secara bebas.
    - Menjamin kekebalan dokumen legal: dokumen yang telah disetujui atau ditandatangani (`signed`/`complete`/`approval`) dikunci secara permanen dari aksi pengubahan (`updateDirekturDocument`) maupun penghapusan (`deleteDirekturDocument`) dengan respon status HTTP `422 Unprocessable Entity` / `409 Conflict`.
    - Mencegah penimpaan tanda tangan akibat *race condition* atau klik ganda (*double approval*) dengan menerapkan kunci kondisional atomik Mongoose pada kueri pembaruan data.
    - Menjaga keutuhan berkas MinIO: berkas lama hanya dihapus setelah pembaruan rekaman di database berhasil tersimpan (*fail-safe storage update*).
  - **Integrasi Notifikasi Telegram Bot (`utils/telegram.js`)**:
    - Menghubungkan alur pengajuan persetujuan dokumen Direktur dengan bot Telegram: secara otomatis mengirimkan ringkasan nomor dokumen, perihal, nama pembuat, dan tautan langsung untuk peninjauan persetujuan oleh Direktur.
  - **Antarmuka Arsip Ketetapan Direktur & Visual Penempatan Tanda Tangan (`frontend`)**:
    - Menambahkan modul baru pada sidebar navigasi: **Arsip > Ketetapan Direktur** (`archive/decree/direktur/index.jsx`).
    - Menyediakan form pembuatan dokumen fleksibel (`create.jsx`) dengan fitur pemilihan template siap pakai (`direkturTemplates.js`), seperti template *SK Penunjukan PIC POP & Marketing*.
    - Mengembangkan komponen interaktif penempatan tanda tangan berbasis visual drag-and-drop (`SignPositionStep.jsx` dan hook `usePdfSignaturePlacement.js`) di mana pengguna dapat menggeser kotak tanda tangan pada halaman PDF yang diinginkan.
    - Mengembangkan antarmuka pratinjau dokumen terstruktur berbasis web (`DirekturGeneratedDocumentPreview.jsx`) lengkap dengan format kop surat resmi, penomoran, tabel penunjukan, dan stempel tanda tangan.
    - Halaman eksekutif peninjauan dan persetujuan digital (`ReviewDirekturDocumentPage.jsx`) yang mendukung tanda tangan sentuh/stylus pada perangkat tablet maupun desktop.
    - Komponen drawer detail dokumen (`ReviewDrawer.jsx`) untuk meninjau riwayat penerbitan, status persetujuan, dan unduh berkas yang sudah disahkan.

#### [c988c3d] - resolve #244 - Rabu, 9 September 2026, 21:31:58 WIB

- **Komponen yang Berubah**:
  - Identik dengan ringkasan commit [aabb7b6] di atas (commit squash fitur utama #244 sebelum proses merge).
- **Deskripsi Perubahan & Fungsi**:
  - Konsolidasi seluruh implementasi fitur Ketetapan Direktur, penyusunan model bersama (`documentFile.schema.js`, `signaturePosition.schema.js`), integrasi template Surat Keputusan, serta penyelesaian seluruh checklist perbaikan audit perangkat lunak.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #278  | Modul Beranda Aplikasi Mobile (Berita & Promo) | Memungkinkan admin mengelola konten promosi dan artikel informasi pelanggan secara terpusat, dengan dukungan tampilan visual ganda (tabel & grid card) serta pengamanan ketat terhadap kebocoran berkas privat di penyimpanan MinIO. |
| #244  | Modul Ketetapan Direktur & Persetujuan Digital | Mendigitalkan proses penerbitan Surat Keputusan (SK) Direksi dari penyusunan draft berbasis template hingga penandatanganan digital interaktif oleh Direktur, disertai penguncian integritas dokumen sah agar tidak dapat dimanipulasi atau terhapus. |

### Kemampuan Baru Pengguna/Admin

- **Manajemen Konten Aplikasi Mobile**: Admin kini dapat menambah, mengubah, menonaktifkan, dan menghapus banner promo maupun artikel berita yang tampil pada beranda aplikasi mobile pelanggan, serta memantau metrik ringkasan konten aktif melalui KPI dashboard card.
- **Penyusunan SK Direktur Tanpa PDF Eksternal**: Pengguna dapat menyusun Surat Keputusan resmi langsung dari browser menggunakan form terstruktur (konsideran Menimbang, Mengingat, Data Penunjukan, dan Diktum Putusan) tanpa perlu menyusun dokumen di Microsoft Word terlebih dahulu.
- **Penempatan Tanda Tangan Visual**: Pengguna yang mengunggah dokumen PDF dapat menentukan posisi koordinat tanda tangan Direktur secara visual dengan mekanisme drag-and-drop pada halaman yang dipilih.
- **Persetujuan & Tanda Tangan Digital Direktur**: Direktur dapat langsung meninjau dokumen melalui link notifikasi Telegram atau halaman web, membubuhkan tanda tangan digital langsung di layar sentuh/desktop, dan dokumen bertanda tangan langsung terkunci dan tersimpan otomatis.

### Bug Fix / Solusi Masalah

- **Perbaikan Celah IDOR / Arbitrary File Read (#278)**: Menutup celah keamanan pada `GET /api/v1/file/news/:name` dengan memverifikasi kepemilikan nama berkas pada koleksi berita sebelum mengalirkan data, mencegah kebocoran foto KTP pelanggan pada bucket bersama `buckets.appFiles`.
- **Pencegahan Disk Leakage & Orphaned Files (#278)**: Menjamin pembersihan berkas temporer lokal pada Express file-upload saat validasi gagal serta pasca unggah MinIO, dan memvalidasi keunikan data sebelum mengunggah objek ke MinIO.
- **Eliminasi Infinite Re-fetch pada Edit Drawer (#278)**: Memperbaiki dependensi `useEffect` pada `NewsEditDrawer` sehingga tidak memicu pemanggilan API terus-menerus saat tabel di-render ulang.
- **Perlindungan Dokumen Sah dari Penghapusan (#244)**: Menambahkan pengecekan status ketat pada controller dan service dokumen Direktur sehingga berkas yang sudah sah/ditandatangani tidak dapat dihapus atau ditimpa oleh pengguna mana pun.
- **Pencegahan Race Condition Penandatanganan (#244)**: Menerapkan kunci kondisional atomik pada kueri Mongoose sehingga penandatanganan simultan tidak menyebabkan inkonsistensi data atau penimpaan tanda tangan.
- **Penanganan Duplikasi Kunci HTTP 409 (#278 & #244)**: Mengganti respon generik HTTP 400/500 menjadi status HTTP `409 Conflict` dengan pesan terjemahan i18n yang jelas saat terjadi bentrok judul berita atau nomor dokumen.

### Menu/Fitur Baru

- **Menu Integrasi Aplikasi > Beranda Aplikasi Mobile** (`/mobile-app/news`): Halaman pengelolaan banner promosi dan artikel berita aplikasi mobile dengan pilihan tampilan tabel dan kartu grid.
- **Menu Arsip > Ketetapan Direktur** (`/archive/decree/direktur`): Modul arsip dan tata kelola Surat Keputusan Direksi.
- **Halaman Tambah Dokumen Direktur** (`/archive/decree/direktur/create`): Wizard pembuatan dokumen dengan opsi Upload PDF atau Dokumen Terstruktur (Generated).
- **Halaman Peninjauan & Tanda Tangan Publik/Eksekutif** (`/public/decree/direktur/review/:id`): Antarmuka peninjauan dan pembubuhan tanda tangan elektronik responsif untuk pimpinan.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Manajemen Konten Beranda Aplikasi Mobile (#278)

- **Penjelasan Fitur**:
  Modul ini digunakan oleh tim pemasaran dan operasional untuk mengelola konten yang disajikan kepada pelanggan pada beranda aplikasi mobile DEKASIMAL. Konten terbagi menjadi dua kategori: **Berita** (banner promosi, pengumuman event) dan **Artikel** (edukasi teknis jaringan, panduan layanan, informasi tips & trik).
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu sidebar **Integrasi Aplikasi** lalu klik submenu **Beranda Aplikasi Mobile**.
  2. Perhatikan 4 kartu ringkasan di bagian atas untuk melihat total konten, konten yang aktif tayang, serta sebaran tipe konten.
  3. Untuk beralih antar mode tampilan, gunakan tombol toggle di pojok kanan atas tabel: pilih **Tampilan Tabel** untuk daftar terpaginasi standar atau **Tampilan Grid** untuk kartu visual dengan pratinjau gambar.
  4. Klik tombol **Tambah Konten** untuk membuka formulir pembuatan konten baru:
     - Masukkan **Judul Konten**.
     - Pilih **Tipe Konten** (*Berita* atau *Artikel*).
     - Unggah gambar banner berformat JPG, PNG, atau WebP (maks. 5 MB).
     - Tuliskan isi konten pada kolom yang disediakan.
     - Tentukan switch status penayangan (**Aktifkan Tayang** agar langsung muncul di aplikasi pelanggan).
  5. Klik tombol **Simpan**. Konten akan langsung tersimpan dan dapat diedit sewaktu-waktu melalui tombol aksi **Ubah** pada baris tabel atau kartu grid.

---

### 2. Pembuatan & Penandatanganan Dokumen Ketetapan Direktur (#244)

- **Penjelasan Fitur**:
  Modul Ketetapan Direktur menyediakan alur kerja terpadu untuk menerbitkan regulasi internal perusahaan atau Surat Keputusan penunjukan pejabat/staf. Sistem mendukung pembuatan dokumen dari berkas PDF eksternal maupun pembuatan otomatis (*generated document*) berbasis template standar hukum perusahaan.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Arsip** lalu pilih **Ketetapan Direktur**.
  2. Klik tombol **Buat Ketetapan Baru** di pojok kanan atas.
  3. Pilih salah satu dari dua mode pembuatan:
     - **Mode Upload Dokumen**:
       - Masukkan Nomor Dokumen, Judul, Kategori, dan Tanggal Penetapan.
       - Unggah berkas PDF Surat Keputusan (maks. 15 MB).
       - Pada langkah penempatan tanda tangan, pilih halaman dokumen dan geser (*drag*) kotak tanda tangan visual ke posisi kolom tanda tangan yang diinginkan.
     - **Mode Buat Dokumen Baru (Generated)**:
       - Pilih template dokumen (misal: *SK Penunjukan PIC POP & Marketing*).
       - Isi data pihak yang ditunjuk (Nama, NIK, Jabatan, Alamat, Wilayah Penugasan).
       - Sesuaikan butir pertimbangan *Menimbang*, dasar hukum *Mengingat*, dan pasal *Klausul Keputusan*.
       - Lihat tab **Preview** untuk memeriksa hasil tampilan dokumen berstempel kop resmi perusahaan.
  4. Klik tombol **Simpan & Ajukan**. Sistem akan mengirimkan notifikasi interaktif ke bot Telegram Direktur.
  5. **Penandatanganan oleh Direktur**:
     - Buka tautan peninjauan yang dikirimkan via Telegram atau klik tombol **Tinjau** pada tabel arsip.
     - Periksa isi dokumen pada layar pratinjau interaktif.
     - Bubuhkan tanda tangan digital pada kotak tanda tangan atau pilih tanda tangan tersimpan.
     - Klik tombol **Konfirmasi & Tandatangani**. Dokumen akan digabungkan dengan tanda tangan, diberi cap sah, dan dikunci secara permanen.
