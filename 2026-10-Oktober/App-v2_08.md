# 📝 Daily Work Report - Dedy S.N Putra (2026-10-08)

---

## 📅 Laporan Harian - 8 Oktober 2026

> Laporan ini merangkum seluruh aktivitas rekayasa perangkat lunak, perancangan sistem publik anti-spam, arsitektur alur kerja multi-tahap (Helpdesk & Accounting), orkestrasi perizinan berbasis peran (RBAC), serta rilis penggabungan fitur pada monorepo DEKASIMAL V2 per 8 Oktober 2026. Fokus dan pencapaian utama hari ini berpusat pada:
> 1. **Permintaan Perubahan Layanan via Formulir Web Publik (`issue-379`)**: Pembangunan antarmuka formulir web publik tanpa login (`/service-change`) yang aman dan elegan bagi pelanggan untuk mengajukan perubahan paket internet, dilengkapi sistem kode rujukan acak nan unik (`request_code`), dialog persetujuan Syarat & Ketentuan dinamis ber-hash versi, serta penjaminan isolasi data agar formulir publik tidak dapat memanipulasi dokumen pelanggan atau membocorkan data terdaftar (*zero data leakage*).
> 2. **Lapisan Keamanan Berlapis, Proteksi Anti-Bot Honeypot, & Sensor Data Pribadi (PII)**: Penerapan mekanisme jebakan bot (*Honeypot trap*) dengan balasan sukses palsu (HTTP 201) tanpa penulisan database, pembatasan laju pengiriman (*rate limiting* 5 per 15 menit per IP), validasi dan sanitasi ketat tipe data masukan publik, pencegahan spam permohonan ganda pada nomor HP yang sama via *Partial Unique Index* MongoDB (`open_phone_unique`), serta sensor otomatis data pribadi (nomor telepon, email, alamat) pada middleware logger Winston.
> 3. **Alur Kerja Terpadu Multi-Tahap Helpdesk & Accounting (*Process Timeline*)**: Pengembangan sistem jejak proses (*timeline*) internal berkonsep *append-only* dengan pemisahan wewenang yang tegas antara verifikasi Helpdesk (`waiting → confirmed → verified`) dan pemrosesan akhir Accounting (`verified → done`, pengembalian ke Helpdesk, atau penolakan), di mana seluruh perubahan status dan entri kronologi dieksekusi dalam satu mutasi atomik database MongoDB (`findOneAndUpdate`).
> 4. **Antarmuka Admin Terpadu, Drawer Interaktif, & Sinkronisasi Badge Real-Time**: Restrukturisasi panel admin `serviceChange` menjadi 2 tab navigasi (*Aplikasi Mobile* dan *Formulir Web*), panel *Drawer* detail lengkap dengan kartu pencocokan otomatis (*auto-lookup*) ke database pelanggan, tombol pintas hubungi WhatsApp, timeline proses interaktif, form komentar staf, serta counter antrean badge sidebar yang tersinkronisasi secara real-time via WebSocket.
> 5. **Pengujian Otomatis Menyeluruh**: Penulisan rangkaian pengujian otomatis lengkap mencakup 7 test suite integrasi backend, 3 unit test utilitas & sanitizer backend, serta 4 pengujian skema & logika frontend dengan kelulusan 100%.

---

## 🌿 Branch: `issue-379` — Permintaan Perubahan Layanan via Formulir Web Publik & Jejak Proses Multi-Tahap

### 📌 Informasi Issue

- **Nomor Issue**: #379
- **Judul Issue**: Permintaan Perubahan Layanan via Formulir Web Publik (`serviceChangeRequest`), Penjaga Anti-Spam & Honeypot, Jejak Proses Alur Kerja Helpdesk & Accounting (*Timeline*), serta Pengendalian Status Atomik
- **Status Branch**: `Sudah di-merge` (Di-merge ke branch `master` dan `issue-381` via commit [`c12419d6`](file:///home/dhedhy/Project/Dekasimal-V2), seluruh 14 test suite terverifikasi lulus 100%)

### 📅 Rincian Commit

#### [[`c12419d6`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #379 (Merge commit ke master) - 8 Oktober 2026, 21:12:45 WIB
#### [[`b0324d0e`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #379 - 8 Oktober 2026, 21:10:58 WIB
#### *Catatan Riwayat Pengembangan Pre-Squash*: Terekam pada branch cadangan `backup/issue-379-pre-squash` (27 commits: [`81b06b50`](file:///home/dhedhy/Project/Dekasimal-V2) s/d [`bff8c31e`](file:///home/dhedhy/Project/Dekasimal-V2))

- **Komponen yang Berubah**:
  - **Dokumentasi & Standar Arsitektur Monorepo (`AGENTS.md` & `docs/`)**:
    - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md):
      - Penambahan aturan baku Bab 2.A nomor 23: Standar Formulir Publik Tanpa Login (`serviceChangeRequest`) dan Jejak Proses (`timeline[]`).
      - Menetapkan kepatuhan arsitektur:
        - Kiriman publik tidak pernah menulis langsung ke dokumen langganan/pelanggan; masuk ke koleksi mandiri sebagai klaim yang diverifikasi admin.
        - Pencegahan kebocoran data: server tidak melakukan lookup untuk publik (tidak ada cara menguji apakah suatu ID atau nomor HP terdaftar dari luar).
        - Penjagaan honeypot `website` (balasan 201 palsu tanpa penyimpanan), rate limiting ketat, dan penanganan duplikasi via indeks parsial unik `open_phone_unique`.
        - Redaksi data pribadi (PII) pengirim dari log Winston.
        - Integritas jejak proses: komentar dan transisi status bersifat *append-only*, dan pembaruan status beserta entri timelinenya wajib berada dalam satu mutasi atomik `findOneAndUpdate`.
        - Pemisahan tegas endpoint status Helpdesk (`change-status`) dan Accounting (`accounting-status`).
    - [`docs/superpowers/specs/2026-10-08-public-service-change-request-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-10-08-public-service-change-request-design.md) [NEW]: Spesifikasi desain teknis halaman web publik, model data klaim, honeypot, snapshot ketentuan, dan isolasi keamanan.
    - [`docs/superpowers/specs/2026-10-08-service-change-process-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-10-08-service-change-process-design.md) [NEW]: Spesifikasi desain teknis jejak proses (*timeline*), transisi status 5-tahap, wewenang accounting, dan counter badge terisolasi.
  - **Model Data & Skema Backend (`backend/src/models/`)**:
    - [`backend/src/models/serviceChangeWebRequest.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/serviceChangeWebRequest.model.js) [NEW]:
      - Model Mongoose independen `service_change_web_requests`.
      - Identitas klaim: `name`, `phone` (kanonik via `toCanonicalPhone`), `email`, `address`, `customer_id_claimed`, `current_package_text`.
      - Kode rujukan acak unik `request_code` (misal `SCW-7K3M9QXD`) yang diindeks unik.
      - Referensi produk: `requested_product` beserta snapshot `requested_product_name` dan `requested_product_code`.
      - Snapshot persetujuan ketentuan: `terms_text`, `terms_version` (hash SHA-256 ringkas), dan `terms_accepted_at`.
      - Jejak proses (*Process Timeline*): Sub-dokumen array `timeline` (`admin_id`, `admin_name`, `action: 'comment' | 'status_change'`, `from_status`, `to_status`, `message`, `created_at`).
      - Jejak pengirim: `submitted_ip` dan `user_agent` (maksimal 200 karakter).
      - Status alur: `status` (`waiting`, `confirmed`, `verified`, `done`, `rejected`), `is_open` (boolean).
      - Indeks Parsial Unik `open_phone_unique` pada `{ phone: 1 }` dengan kondisi `{ is_open: true }`.
  - **Utilitas Keamanan, Sanitasi Input, & Sensor Logger (`backend/src/`)**:
    - [`backend/src/utils/serviceChangeRequestInput.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/serviceChangeRequestInput.js) [NEW]:
      - `parseCreateInput`: Validasi tipe ketat, eliminasi karakter berbahaya, normalisasi string nama/alamat/catatan, pembakuan format nomor telepon Indonesia (`toCanonicalPhone`), dan validasi email. Nilai non-string secara otomatis ditolak atau diubah menjadi string kosong.
      - `isHoneypotTriggered`: Deteksi pengisian bidang tersembunyi `website` oleh robot spam.
      - `normalizePhoneSearch`: Pembakuan input pencarian nomor telepon oleh admin untuk pencocokan parsial yang akurat.
    - [`backend/src/middlewares/logger.middleware.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/middlewares/logger.middleware.js):
      - Peningkatan mekanisme redaksi data sensitif (*Zero PII Logging*): Menambahkan field `phone`, `email`, `address`, `notes`, `customer_id_claimed`, dan `message` pada daftar redaksi payload HTTP log Express untuk mencegah kebocoran informasi pribadi ke file log sistem.
  - **Layanan Bisnis & Orkestrasi Backend (`backend/src/services/`)**:
    - [`backend/src/services/serviceChangeRequest.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/serviceChangeRequest.service.js) [NEW]:
      - `createServiceChangeWebRequest`: Pengecekan honeypot bot trap, penarikan snapshot teks ketentuan aktif dari database, pengambilan snapshot produk, pembuatan kode rujukan acak yang aman, dan inisialisasi permohonan baru berstatus `waiting`.
      - `listServiceChangeWebRequests`: Pengambilan daftar permohonan dengan paginasi, pencarian nomor telepon/nama/kode rujukan, dan filter status alur.
      - `getServiceChangeWebRequestDetail`: Mengambil detail permohonan sekaligus menjalankan pencocokan otomatis (*auto-lookup*) ke database pelanggan berdasarkan nomor telepon (`buildPhoneVariants`), ID pelanggan klaim, dan data langganan `RadiusAuthentication`.
      - `changeHelpdeskStatus`: Transisi status oleh tim Helpdesk (`waiting → confirmed → verified` atau `rejected`) dengan append entri timeline atomik.
      - `changeAccountingStatus`: Transisi status oleh tim Accounting (`verified → done`, `verified → confirmed` [pengembalian], atau `verified → rejected`) dengan pesan wajib dan append timeline atomik.
      - `addProcessComment`: Penambahan komentar internal staf ke timeline (dibatasi maksimal 190 komentar untuk menjamin batas ukuran dokumen MongoDB).
      - `countOpenRequests`: Perhitungan counter antrean terbuka terisolasi per peran: Helpdesk (`waiting + confirmed`) dan Accounting (`verified`), dipancarkan via WebSocket realtime `service-change-request:count-updated`.
    - [`backend/src/services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js):
      - Fungsi `getServiceChangeTerms` untuk membaca teks ketentuan perubahan layanan dengan penanganan aman *JSON-like string parsing*.
  - **Kontroler & Perutean Backend (`backend/src/controllers/`, `backend/src/routes/`, `backend/src/app.js`)**:
    - [`backend/src/controllers/serviceChangeRequest.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/serviceChangeRequest.controller.js) [NEW]:
      - Endpoint Publik: `publicCreateRequest` (dengan honeypot guard dan rate limiting) dan `publicGetTerms`.
      - Endpoint Internal: `listRequests`, `getRequestDetail`, `changeHelpdeskStatus`, `changeAccountingStatus`, `addProcessComment`, dan `getOpenCounts`.
    - [`backend/src/routes/serviceChangeRequest.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/serviceChangeRequest.route.js) [NEW]: Pendaftaran rute RESTful lengkap dengan dokumentasi OpenAPI Swagger dan batasan rate limit IP.
    - [`backend/src/app.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/app.js): Pendaftaran `serviceChangeRequestRouter` ke Express application pipeline.
    - [`backend/src/config/privilege.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilege.json) & [`privilegeDictionary.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/config/privilegeDictionary.json): Pendaftaran kunci privilege baru: `serviceChangeRequest.read`, `serviceChangeRequest.changeStatus`, `serviceChangeRequest.comment`, `serviceChangeAccounting.read`, dan `serviceChangeAccounting.changeStatus`.
    - [`backend/scripts/migrate-service-change-privileges.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/scripts/migrate-service-change-privileges.js) [NEW]: Skrip migrasi database untuk menyuntikkan privilege baru ke role Administrator dan Helpdesk.
    - [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) & [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json): Penambahan kunci terjemahan bilingual respon API dan galat validasi.
  - **Halaman Formulir Web Publik Frontend (`frontend/src/app/pages/public/`)**:
    - [`frontend/src/app/pages/public/serviceChangeRequest.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/public/serviceChangeRequest.jsx) [NEW]:
      - Halaman antarmuka publik responsif dan modern dengan tema visual premium untuk pelanggan (`/service-change`).
      - Formulir data pemohon (Nama, Nomor HP, Email, Alamat, ID Pelanggan opsional, Paket saat ini).
      - Pemilihan paket baru yang diinginkan dari daftar produk aktif.
      - Kotak persetujuan Syarat & Ketentuan dinamis dengan kotak dialog modal baca ketentuan lengkap.
      - Bidang honeypot transparan tersembunyi [`HoneypotField.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/form/HoneypotField.jsx) [NEW].
      - Layar sukses dengan kode rujukan pelanggan (`request_code`) yang dapat disalin dengan satu klik (*Copy to Clipboard*).
    - [`frontend/src/app/pages/public/schema/serviceChangeRequestSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/public/schema/serviceChangeRequestSchema.js) [NEW]: Skema validasi Yup untuk formulir publik.
    - [`frontend/src/app/router/public.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/public.jsx): Pendaftaran rute publik `/service-change`.
    - [`frontend/src/utils/axios.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/axios.js): Mendaftarkan `/service-change` ke dalam daftar putih `PUBLIC_PATHS` agar respons HTTP 401 tidak mengalihkan pengunjung publik ke halaman login/expired.
  - **Antarmuka Admin Terpadu & Panel Drawer (`frontend/src/app/pages/mobileApp/serviceChange/`)**:
    - [`frontend/src/app/pages/mobileApp/serviceChange/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/mobileApp/serviceChange/index.jsx):
      - Restrukturisasi halaman menjadi navigasi 2 tab:
        - Tab **Aplikasi Mobile** ([`MobileRequestsTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/mobileApp/serviceChange/components/MobileRequestsTab.jsx) [NEW]): Mempertahankan alur lama permintaan perubahan dari mobile app pelanggan.
        - Tab **Formulir Web** ([`WebRequestsTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/mobileApp/serviceChange/components/WebRequestsTab.jsx) [NEW]): Datatable permintaan formulir web publik dengan pencarian, filter status, dan badge warna status.
    - [`frontend/src/app/pages/mobileApp/serviceChange/components/WebRequestDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/mobileApp/serviceChange/components/WebRequestDrawer.jsx) [NEW]:
      - Panel drawer detail komprehensif:
        - Ringkasan kode permintaan, tanggal pengajuan, dan status saat ini.
        - Kartu pencocokan otomatis (*auto-match customer card*): menampilkan kecocokan data pelanggan, paket internet aktif, dan tautan profil.
        - Tombol cepat hubungi WhatsApp pelanggan.
        - Pratinjau teks ketentuan yang disetujui pelanggan beserta hash versi.
        - Jejak Proses Interaktif ([`ProcessTimeline.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/mobileApp/serviceChange/components/ProcessTimeline.jsx) [NEW]): Menampilkan kronologi seluruh riwayat perubahan status dan komentar staf.
        - Kotak tambah komentar internal.
        - Tombol transisi aksi dinamis berdasarkan hak akses pengguna (Konfirmasi, Verifikasi, Selesai, Kembalikan ke Helpdesk, Tolak).
    - [`frontend/src/app/pages/settings/sections/Application.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/Application.jsx): Antarmuka textarea di menu *Pengaturan > Aplikasi* untuk menyunting teks Ketentuan Perubahan Layanan yang tampil pada form publik.
    - [`frontend/src/features/serviceChangeRequestSlice.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/features/serviceChangeRequestSlice.js) [NEW] & [`frontend/src/hooks/useTicketBadge.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/hooks/useTicketBadge.js): Mengelola state hitungan badge menu sidebar yang tersinkronisasi via socket realtime `service-change-request:count-updated`.
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json): Penambahan 110+ pasangan kunci terjemahan bilingual.
  - **Rangkaian Pengujian Otomatis Lengkap**:
    - Backend Integration Tests:
      - [`serviceChangeRequest.create.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/serviceChangeRequest.create.test.js): Pengujian endpoint publik form web, honeypot bot trap, snapshot produk & terms.
      - [`serviceChangeRequest.admin.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/serviceChangeRequest.admin.test.js): Pengujian CRUD admin, otorisasi, pagination, filtering.
      - [`serviceChangeProcess.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/serviceChangeProcess.test.js): Pengujian alur 5-tahap status (`waiting -> confirmed -> verified -> done / rejected`), timeline append-only, dan pembatasan wewenang Helpdesk vs Accounting.
      - [`serviceChangeTerms.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/serviceChangeTerms.test.js): Pengujian persistensi terms dan hash versi.
      - [`serviceChangeRequest.rateLimit.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/serviceChangeRequest.rateLimit.test.js): Pengujian rate limiter 5 req/15 menit per IP.
      - [`serviceChangeRequest.routes.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/serviceChangeRequest.routes.test.js): Pengujian integrasi routing HTTP.
      - [`serviceChangePrivilegeMigration.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/serviceChangePrivilegeMigration.test.js): Pengujian skrip migrasi privilege.
    - Backend Unit Tests:
      - [`serviceChangeRequestInput.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/serviceChangeRequestInput.test.js) (25 test, 100% lulus): Pengujian sanitasi string, honeypot, normalisasi telepon.
      - [`loggerSanitizer.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/loggerSanitizer.test.js): Pengujian sensor data sensitif pada logger HTTP.
      - [`serviceChangePrivilegeText.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/serviceChangePrivilegeText.test.js): Pengujian konsistensi teks kamus perizinan.
    - Frontend Unit & Schema Tests:
      - [`serviceChangeRequestSchema.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/public/schema/serviceChangeRequestSchema.test.js) (11 test, 100% lulus): Pengujian skema validasi Yup form publik.
      - [`processActions.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/mobileApp/serviceChange/utils/processActions.test.js) (11 test, 100% lulus): Pengujian logika penentuan transisi status dan tombol aksi drawer.
      - [`applicationSchema.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/schema/applicationSchema.test.js): Pengujian validasi textarea ketentuan di settings.
      - [`useTicketBadge.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/hooks/useTicketBadge.test.js): Pengujian kalkulasi badge counter antrean.
- **Deskripsi Perubahan & Fungsi**:
  - Membuka kanal baru pengajuan perubahan paket bagi pelanggan internet tanpa harus memasang aplikasi mobile, sekaligus memperkuat keamanan sistem dari serangan spamming/bot dan mencegah manipulasi data langganan secara langsung.
  - Memperjelas pemisahan tanggung jawab (*Separation of Duties*) antara tim Helpdesk yang memvalidasi keaslian pengajuan pelanggan dan tim Accounting yang mengeksekusi perubahan paket pada sistem penagihan.
  - Menciptakan transparansi dan audit trail menyeluruh (*process timeline*) sehingga setiap keputusan, catatan internal, dan perubahan status tercatat secara kekal demi kepatuhan operasional perusahaan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #379  | Permintaan Perubahan Layanan via Formulir Web Publik | Halaman publik aman `/service-change` dengan honeypot anti-bot, alur kerja 5-tahap (Helpdesk & Accounting), jejak proses append-only, auto-match data pelanggan, dan badge realtime. |

### Kemampuan Baru Pengguna/Admin

- **Pengajuan Perubahan Paket via Web Publik**: Pelanggan dapat mengajukan peningkatan atau penurunan paket internet langsung melalui peramban web di alamat `/service-change` tanpa perlu login, memilih paket yang tersedia, menyetujui ketentuan layanan, dan menerima kode rujukan resmi (contoh: `SCW-7K3M9QXD`).
- **Pencocokan Otomatis Pelanggan (*Auto-Lookup*)**: Pada antarmuka admin, permohonan web yang masuk secara otomatis dicocokkan dengan data pelanggan di database berdasarkan nomor WhatsApp/HP kanonik dan nomor klaim pelanggan, menampilkan status langganan dan paket aktif saat ini tanpa perlu pencarian manual.
- **Pemisahan Wewenang Helpdesk & Accounting**:
  - Tim Helpdesk bertugas mengonfirmasi keabsahan permohonan dan menandai permohonan sebagai `verified` setelah berkomunikasi dengan pelanggan.
  - Tim Accounting bertugas memeriksa aspek finansial, mengubah paket di sistem langganan, lalu menandai permohonan sebagai `done`, atau mengembalikannya ke Helpdesk jika diperlukan klarifikasi lanjutan.
- **Kolaborasi Catatan Internal (*Process Timeline*)**: Staf dapat menambahkan catatan atau instruksi internal pada timeline permohonan yang hanya dapat dibaca oleh tim internal perusahaan.
- **Pemantauan Antrean Real-Time**: Lencana hitungan (*badge counter*) pada menu sidebar secara otomatis bertambah saat ada permohonan baru yang masuk sesuai dengan peran admin yang sedang aktif (Helpdesk memantau antrean `waiting + confirmed`, Accounting memantau antrean `verified`).

### Bug Fix / Solusi Masalah

- **Pencegahan Eksploitasi & Spam Bot (Anti-Bot Honeypot)**: Mengeliminasi risiko spam submission dari bot otomatis melalui field jebakan `website` dan pembatasan laju IP (5 kiriman per 15 menit), di mana bot akan menerima respons sukses semu tanpa mengotori database.
- **Pencegahan Permohonan Ganda (*Double Submission Prevention*)**: Menerapkan indeks parsial unik `open_phone_unique` pada basis data MongoDB, memastikan nomor telepon yang sedang memiliki permohonan aktif (`waiting`, `confirmed`, `verified`) tidak dapat mengajukan permohonan baru sampai permohonan sebelumnya selesai atau ditolak.
- **Pencegahan Kebocoran Data Pelanggan (*Zero Data Leakage*)**: Endpoint publik sama sekali tidak menyediakan API lookup; orang luar tidak dapat memanfaatkan formulir untuk memeriksa apakah suatu nomor telepon atau ID pelanggan telah terdaftar di sistem.
- **Perlindungan Privasi Data Pribadi (PII)**: Menghilangkan pencatatan data pribadi sensitif pemohon (nomor HP, alamat, email, catatan) dari berkas log server.

### Menu/Fitur Baru

- **Halaman Formulir Publik `/service-change`**: Antarmuka web publik responsif dan modern untuk pengajuan perubahan paket internet.
- **Tab Formulir Web pada Modul Perubahan Layanan**: Tab baru pada halaman admin `/mobileApp/serviceChange` khusus untuk mengelola permohonan yang diajukan dari web publik.
- **Drawer Detail Permohonan Web & Process Timeline**: Panel inspeksi detail permohonan, riwayat kronologi status, kartu kecocokan pelanggan, dan tombol tindakan operasional.
- **Pengaturan Teks Ketentuan Perubahan Layanan**: Bagian baru pada menu *Pengaturan > Aplikasi* untuk menyesuaikan dokumen syarat & ketentuan yang tampil di formulir publik.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Pengajuan Perubahan Layanan oleh Pelanggan via Formulir Web Publik

- **Penjelasan Fitur**:
  Pelanggan yang ingin mengubah paket internet dapat mengakses formulir publik tanpa perlu memasang aplikasi mobile atau memiliki akun login.
- **Langkah Penggunaan (Tutorial)**:
  1. Pelanggan membuka tautan formulir publik (contoh: `https://domain-isp.com/service-change`).
  2. Masukkan identitas pemohon: **Nama Lengkap**, **Nomor WhatsApp**, **Alamat Email**, dan **Alamat Pemasangan**.
  3. (Opsional) Masukkan **ID Pelanggan** dan **Paket Saat Ini** jika pelanggan mengingatnya.
  4. Pilih **Paket Layanan Baru** yang diinginkan dari daftar dropdown produk aktif.
  5. Baca dokumen ketentuan dengan mengklik tautan **Syarat & Ketentuan**, lalu centang kotak persetujuan.
  6. Klik tombol **Kirim Permintaan**.
  7. Sistem akan menampilkan layar sukses berisi **Kode Permintaan** (misal: `SCW-7K3M9QXD`). Simpan kode tersebut sebagai bukti pengajuan.

---

### 2. Verifikasi Permohonan Web oleh Tim Helpdesk

- **Penjelasan Fitur**:
  Tim Helpdesk memvalidasi pengajuan yang masuk, menghubungi pelanggan untuk konfirmasi, dan meneruskan permohonan ke tim Accounting jika data telah valid.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Mobile Apps > Perubahan Layanan**.
  2. Buka tab **Formulir Web**. Permohonan yang baru masuk akan berstatus **Menunggu** (*Waiting*).
  3. Klik salah satu baris permohonan untuk membuka drawer detail.
  4. Periksa kartu **Pencocokan Pelanggan**:
     - Sistem secara otomatis menampilkan nama pelanggan dan paket aktif jika nomor WhatsApp cocok dengan database.
     - Klik tombol **Hubungi WhatsApp** untuk membuka obrolan langsung dengan pelanggan jika diperlukan konfirmasi.
  5. Ubah status menjadi **Dikonfirmasi** (*Confirmed*) saat permohonan sedang ditindaklanjuti.
  6. Jika pelanggan telah setuju dan berkas lengkap, klik tombol **Verifikasi (Teruskan ke Accounting)**. Status akan berubah menjadi **Terverifikasi** (*Verified*).

---

### 3. Eksekusi Perubahan Layanan oleh Tim Accounting

- **Penjelasan Fitur**:
  Tim Accounting menerima permohonan yang telah diverifikasi oleh Helpdesk, melakukan penyesuaian paket pada sistem langganan, dan menyelesaikan permohonan.
- **Langkah Penggunaan (Tutorial)**:
  1. Staf Accounting membuka tab **Formulir Web** pada modul Perubahan Layanan (badge sidebar akan menunjukkan jumlah antrean *Verified*).
  2. Pilih permohonan yang berstatus **Terverifikasi** (*Verified*).
  3. Buka tautan profil pelanggan yang tertera pada drawer untuk melakukan penyesuaian paket pada menu langganan pelanggan secara manual.
  4. Kembali ke drawer permohonan, masukkan catatan penanganan pada kotak pesan (misal: "Paket berhasil diubah ke 50 Mbps per tanggal 9 Oktober").
  5. Klik tombol **Tandai Selesai**. Status permohonan akan berubah menjadi **Selesai** (*Done*) secara permanen.
  6. *(Alternatif)* Jika ditemukan kendala tagihan atau ketidaksesuaian administrasi, Accounting dapat mengklik **Kembalikan ke Helpdesk** beserta alasannya untuk ditindaklanjuti kembali oleh tim Helpdesk.
