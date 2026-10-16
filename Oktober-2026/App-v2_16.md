# 📝 Daily Work Report - Dedy S.N Putra (2026-10-16)

---

## 📅 Laporan Harian - 16 Oktober 2026

> Laporan ini merangkum seluruh aktivitas rekayasa perangkat lunak, penyempurnaan sistem jaringan dan autentikasi RADIUS, penguatan portal integrasi mitra (*Partner API*), perbaikan modul keuangan faktur publik, penyelarasan pelaporan layanan pelanggan, hingga pemutakhiran riwayat *changelog* rilis serta panduan Semantic Versioning pada monorepo DEKASIMAL V2 per 16 Oktober 2026.
> 
> Fokus dan pencapaian utama hari ini meliputi:
> 1. **Kepemilikan IP Pool & Alokasi IP Statis PPPoE Mitra (`issue-412`)**: Mengatribusikan blok subnet IP Pool Radius ke mitra tertentu (`owner_partner`), mengisolasi pemakaian pool internal vs mitra, serta memperluas Partner API dan frontend manajemen IPv4 agar mitra dapat memilih IP Pool dan IP statis yang tersedia saat mengelola akun PPPoE pelanggan secara mandiri.
> 2. **Penyempurnaan Halaman Pembayaran Faktur Publik (`issue-429`)**: Menampilkan modal pop-up interaktif otomatis untuk peringatan tagihan tertunggak yang lebih lama, catatan peringatan biaya admin/transaksi per metode pembayaran, pemuatan ulang halaman bersih saat beralih tagihan, serta perhitungan nominal transfer yang akurat.
> 3. **Ganti Sesi Lama Otomatis & Pembatasan Pelanggan Menunggak (`issue-430`)**: Implementasi fitur *Auto Replace Stale Session* pada Go `radius-server` dan Pengaturan Sistem untuk mencegah akun PPPoE terkunci (*stuck online*), pengalihan otomatis sesi isolir tanpa pemutusan berulang, serta pemulihan seketika (*instant restore*) ke paket normal saat tagihan dilunasi.
> 4. **Perbaikan Nilai Nol pada Tipe Barang Gudang (`issue-423`)**: Menangani hilangnya nilai `0` pada stok minimum dan harga barang gratis saat pembaruan satuan maupun massal, serta standardisasi label tombol aksi tiket dan gudang.
> 5. **Penyempurnaan Pengaturan Pelaporan Layanan Pelanggan (`issue-407`)**: Transformasi istilah "Lapor Pajak" menjadi "Pelaporan", penambahan tombol aksi ubah status pelaporan langsung pada baris tabel pelanggan, serta validasi ketat format alamat gRPC daemon Radius.
> 6. **Pengelolaan Lokasi & Node Fiber untuk Aplikasi Mitra (`issue-359`)**: Menghadirkan endpoint mandiri bagi aplikasi mitra untuk mengelola titik lokasi POP/Site dan node perangkat fiber optik miliknya lengkap dengan kontrol akses terisolasi dan *optimistic locking*.
> 7. **Perbaikan Riwayat Pembayaran Faktur Migrasi & Data Pelanggan (`issue-426`)**: Pemulihan data riwayat pembayaran pada faktur hasil migrasi sistem lama, validasi kepemilikan mitra pada pendaftaran pelanggan, dan pembersihan status pasif pelanggan secara tuntas.
> 8. **Perbaikan Responsif Halaman Pembayaran Faktur di Ponsel (`issue-424`)**: Mengoptimasi tata letak bilah ringkasan nominal dan tombol pembayaran melayang (*sticky bottom bar*) agar selalu terlihat di layar perangkat seluler.
> 9. **Pemutakhiran Berkas Riwayat Changelog & Standar SemVer**: Dokumentasi rilis v1.97.2 hingga v1.100.0 pada katalog changelog dan penegasan aturan bump versi SemVer seminimal mungkin pada `CHANGELOG_INSTRUCTION.md`.

---

## 🌿 Branch: `master` — Pemutakhiran Berkas Riwayat Rilis Changelog

### 📌 Informasi Issue

- **Nomor Issue**: N/A (Release Maintenance)
- **Judul Issue**: Pemutakhiran Berkas Riwayat Rilis Changelog (v1.97.2 s/d v1.100.0) dan Pengetatan Panduan SemVer
- **Status Branch**: `Sudah di-merge` (Tersinkronisasi penuh pada `origin/master`)

### 📅 Rincian Commit

#### [[`d69b359a`](file:///home/dhedhy/Project/Dekasimal-V2)] - update changelog - 16 Oktober 2026, 20:17:59 WIB

- **Komponen yang Berubah**:
  - [`backend/src/data/changelog/index.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/index.json):
    - Pendaftaran rilis baru: `v1.100.0` (#412), `v1.98.5` (#429), `v1.98.3` (#430), `v1.98.2` (#407), `v1.98.1` (#423), `v1.98.0` (#359), `v1.97.3` (#426), dan `v1.97.2` (#424) ke dalam indeks pencarian changelog.
  - [`backend/src/data/changelog/releases/issue-412.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-412.json) [NEW]:
    - Catatan rilis v1.100.0: kepemilikan IP Pool mitra, pemilihan IP Pool & IP statis di aplikasi mitra, serta pengetatan validasi alokasi subnet.
  - [`backend/src/data/changelog/releases/issue-429.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-429.json) [NEW]:
    - Catatan rilis v1.98.5: penyempurnaan halaman pembayaran faktur publik, modal peringatan tagihan lama, rincian biaya admin, dan nominal transfer akurat.
  - [`backend/src/data/changelog/releases/issue-430.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-430.json) [NEW]:
    - Catatan rilis v1.98.3: penggantian sesi lama otomatis (*stale session replacement*), penanganan isolir pelanggan menunggak, dan pemulihan sesi seketika.
  - [`backend/src/data/changelog/releases/issue-423.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-423.json) [NEW]:
    - Catatan rilis v1.98.1: perbaikan nilai 0 pada stok minimum dan harga barang gudang, serta penyesuaian label aksi tombol.
  - [`backend/src/data/changelog/releases/issue-407.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-407.json) [NEW]:
    - Catatan rilis v1.98.2: penyempurnaan istilah pelaporan layanan pelanggan, tombol aksi pelaporan di baris tabel, dan validasi host gRPC Radius.
  - [`backend/src/data/changelog/releases/issue-359.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-359.json) [NEW]:
    - Catatan rilis v1.98.0: pengelolaan lokasi (POP/Site) dan node fiber untuk aplikasi mitra melalui Partner API.
  - [`backend/src/data/changelog/releases/issue-426.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-426.json) [NEW]:
    - Catatan rilis v1.97.3: pemulihan riwayat pembayaran faktur migrasi lama, validasi data pelanggan mitra, dan pembersihan status pasif.
  - [`backend/src/data/changelog/releases/issue-424.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-424.json) [NEW]:
    - Catatan rilis v1.97.2: perbaikan tampilan ringkasan dan tombol bayar faktur publik pada layar ponsel.
  - [`documentations/CHANGELOG_INSTRUCTION.md`](file:///home/dhedhy/Project/Dekasimal-V2/documentations/CHANGELOG_INSTRUCTION.md):
    - Penambahan prinsip baku SemVer ketat: bump versi seminimal mungkin, larangan AI menaikkan versi MAJOR tanpa konfirmasi developer, serta pedoman pembedaan MINOR vs PATCH.
- **Deskripsi Perubahan & Fungsi**:
  - Menjaga transparansi riwayat rilis bagi seluruh pengguna dan menetapkan panduan baku Semantic Versioning agar penomoran versi di masa mendatang tetap terkendali dan konsisten.

---

## 🌿 Branch: `issue-429` — Penyempurnaan Halaman Pembayaran Faktur Publik

### 📌 Informasi Issue

- **Nomor Issue**: #429
- **Judul Issue**: Penyempurnaan Halaman Pembayaran Faktur Publik: Modal Peringatan Tagihan Lama, Rincian Biaya Admin, dan Akurasi Nominal Transfer
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `8dfbbbf5`)

### 📅 Rincian Commit

#### [[`8dfbbbf5`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #429 (Merge issue-429 into master) - 16 Oktober 2026, 20:11:32 WIB
#### [[`087ebbf4`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #429 - 16 Oktober 2026, 20:10:54 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/publicPayment.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/publicPayment.controller.js):
    - Penyesuaian kalkulasi dan pengembalian rincian nominal pembayaran, memastikan total biaya admin yang ditanggung pelanggan tercantum akurat pada payload transaksi gateway.
  - [`backend/test/integration/publicPayment.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/publicPayment.test.js):
    - Pengujian integrasi endpoint pembayaran publik untuk memvalidasi akurasi perhitungan tagihan dan respons instruksi transfer.
  - [`frontend/src/app/pages/public/PublicInvoicePayment.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/public/PublicInvoicePayment.jsx):
    - Peringatan keberadaan tagihan tertunggak yang lebih lama kini otomatis terbuka dalam modal pop-up interaktif saat halaman dibuka, lengkap dengan tombol untuk beralih langsung ke tagihan terlama.
    - Penambahan bilah pengingat ringkas (*sticky banner*) di bagian bawah layar setelah modal peringatan ditutup.
    - Navigasi perpindahan ke faktur lama kini memuat ulang halaman (*full page reload*) agar state formulir dan sesi pembayaran sebelumnya tereset bersih.
    - Menampilkan catatan peringatan biaya admin/biaya transaksi tambahan sebelum pelanggan memilih metode pembayaran.
    - Memperbaiki nominal transfer pada instruksi pembayaran agar selalu menampilkan jumlah pasti yang harus dikirim pelanggan.
  - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json):
    - Penambahan teks lokalisasi dwibahasa untuk pesan modal peringatan faktur lama dan catatan biaya administrasi transaksi.
- **Deskripsi Perubahan & Fungsi**:
  - Mencegah kekeliruan pembayaran tagihan oleh pelanggan yang memiliki beberapa tagihan tertunggak, serta menjamin nominal yang ditransfer sesuai dengan instruksi perbankan.

---

## 🌿 Branch: `issue-412` — Kepemilikan IP Pool dan IP Statis Akun PPPoE Mitra

### 📌 Informasi Issue

- **Nomor Issue**: #412
- **Judul Issue**: Atribusi Kepemilikan IP Pool Radius ke Mitra dan Alokasi IP Statis Akun PPPoE pada Partner API
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `9bf5f615`)

### 📅 Rincian Commit

#### [[`9bf5f615`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #412 (Merge issue-412 into master) - 16 Oktober 2026, 20:09:11 WIB
#### [[`b8cf2fa7`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #412 - 16 Oktober 2026, 20:06:55 WIB

- **Komponen yang Berubah**:
  - [`backend/src/models/networkIPv4.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/networkIPv4.model.js) & [`models/networkIPv4Used.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/networkIPv4Used.model.js):
    - Penambahan atribut `owner_partner` pada skema database blok IP Pool untuk menandai mitra pemilik alokasi subnet Radius.
  - [`backend/src/controllers/partnerApiIPv4.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiIPv4.controller.js) [NEW] & [`routes/partnerApi.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/partnerApi.route.js):
    - Endpoint baru Partner API (`/partner-api/ipv4/pools` dan `/partner-api/ipv4/pools/:id/available-ips`) untuk melihat daftar IP Pool milik mitra dan daftar IP yang belum terpakai.
  - [`backend/src/controllers/partnerApiRadius.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiRadius.controller.js):
    - Dukungan pengisian parameter `ipv4_pool_id` dan `static_ip` pada pembuatan dan pembaruan akun PPPoE pelanggan mitra via Partner API.
  - [`backend/src/controllers/networkIPv4.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/networkIPv4.controller.js), [`productDataAccess.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/productDataAccess.controller.js), [`productDedicatedInternet.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/productDedicatedInternet.controller.js), & [`radiusAuthentication.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusAuthentication.controller.js):
    - Penyelarasan validasi pemilihan IP Pool di akun Broadband, Dedicated Internet, dan Akses Data agar IP Pool mitra terisolasi dari layanan internal.
  - [`backend/src/utils/radiusPoolOwner.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/radiusPoolOwner.js) [NEW] & [`utils/staticIpInput.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/staticIpInput.js) [NEW]:
    - Modul utilitas validasi ketat penetapan IP statis: pengecekan format IP, verifikasi IP berada di dalam subnet pool, pencegahan duplikasi pemakaian, dan mekanisme pembatalan reservasi otomatis jika penyimpanan akun gagal.
  - [`backend/test/integration/networkIPv4Owner.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/networkIPv4Owner.test.js) [NEW], [`partnerApiIPv4.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/partnerApiIPv4.test.js) [NEW], & [`unit/staticIpInput.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/staticIpInput.test.js) [NEW]:
    - Rangkaian unit test dan integration test komprehensif untuk pengujian hak akses IP Pool mitra, isolasi subnet, dan validasi IP statis.
  - [`frontend/src/app/pages/network/ipv4Management/components/PoolOwnerSelect.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/ipv4Management/components/PoolOwnerSelect.jsx) [NEW]:
    - Komponen dropdown pemilihan mitra pemilik IP Pool dengan pencarian interaktif.
  - [`frontend/src/app/pages/network/ipv4Management/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/ipv4Management/create.jsx), [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/ipv4Management/edit.jsx), & [`index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/ipv4Management/index.jsx):
    - Penambahan kolom Mitra Pemilik pada tabel daftar IP Pool, serta bidang atribusi mitra pada formulir tambah dan ubah IP Pool.
  - [`frontend/src/app/pages/network/ipv4Management/utils/poolOwner.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/ipv4Management/utils/poolOwner.js) [NEW] & [`poolOwner.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/ipv4Management/utils/poolOwner.test.js) [NEW]:
    - Utilitas penanganan data kepemilikan pool dan verifikasi pengujian frontend.
  - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md):
    - Dokumentasi aturan arsitektur terkait kepemilikan IP Pool mitra dan pembatasan akses subnet.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan hak kepada mitra ISP untuk mengelola blok IP Pool miliknya sendiri dan memberikan IP statis khusus kepada pelanggan mereka secara tertib tanpa khawatir bentrok dengan alokasi IP pelanggan lain.

---

## 🌿 Branch: `issue-430` — Ganti Sesi Lama Otomatis dan Pembatasan Pelanggan Menunggak

### 📌 Informasi Issue

- **Nomor Issue**: #430
- **Judul Issue**: Implementasi Fitur Ganti Sesi Lama Otomatis (*Auto Replace Stale Session*) dan Penyempurnaan Pembatasan Isolir pada Daemon Radius Server
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `7d31e09c`)

### 📅 Rincian Commit

#### [[`7d31e09c`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #430 (Merge issue-430 into master) - 16 Oktober 2026, 19:14:41 WIB
#### [[`3e783957`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #430 - 16 Oktober 2026, 19:07:17 WIB

- **Komponen yang Berubah**:
  - [`radius-server/internal/domain/auth/authenticate.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/domain/auth/authenticate.go) & [`transport/pppoe/auth_listener.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/transport/pppoe/auth_listener.go):
    - Penanganan logika ganti sesi otomatis: saat akun meminta autentikasi baru namun masih tercatat online di database, server mengevaluasi tanda hidup (*liveness*) sesi lama. Jika sesi lama sudah melewati batas waktu tidak aktif atau berasal dari MAC address yang sama, sesi lama segera digantikan dan diputus.
  - [`radius-server/internal/domain/isolir/restriction.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/domain/isolir/restriction.go) [NEW] & [`restriction_test.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/domain/isolir/restriction_test.go) [NEW]:
    - Modul evaluasi status pembatasan (isolir/tunggakan/kuota habis): pelanggan yang dibatasi tetap diberikan akses IP isolir (Access-Accept dengan atribut isolir) agar browser dapat mengakses portal pembayaran, alih-alih ditolak (*Access-Reject*).
  - [`radius-server/internal/domain/accounting/handlers.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/domain/accounting/handlers.go):
    - Pencegahan pemutusan berulang (*disconnect loop*) pada paket data accounting; pemutusan hanya dipicu saat terjadi transisi status pembatasan.
    - Pemulihan otomatis ke paket broadband normal via CoA/Disconnect saat tagihan terbayar.
  - [`radius-server/pkg/config/options.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/pkg/config/options.go) & [`DOCUMENTATION.md`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/DOCUMENTATION.md):
    - Parameter konfigurasi baru: `radius_auto_replace_stale_session`, `radius_replace_session_stale_threshold_seconds`, dan `radius_replace_session_instant_same_mac`.
  - [`radius-server/internal/domain/auth/replace_session_test.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/domain/auth/replace_session_test.go) [NEW] & [`transport/pppoe/replace_session_test.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/transport/pppoe/replace_session_test.go) [NEW]:
    - Pengujian unit dan simulasi skenario penggantian sesi ganda pada level protokol RADIUS.
  - [`backend/src/controllers/settings.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/settings.controller.js) & [`services/option.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/option.service.js):
    - Penyimpanan dan sinkronisasi opsi konfigurasi ganti sesi pada pengaturan sistem backend.
  - [`backend/test/integration/settingsReplaceSession.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/settingsReplaceSession.test.js) [NEW]:
    - Pengujian integrasi endpoint pengaturan sistem untuk parameter ganti sesi.
  - [`frontend/src/app/pages/settings/sections/System.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/System.jsx) & [`schema/systemSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/schema/systemSchema.js):
    - Penambahan kontrol sakelar *Ganti Sesi Lama Otomatis*, *Batas Ganti Sesi (detik)*, dan *Ganti Seketika bila MAC Sama* pada halaman Pengaturan Sistem.
- **Deskripsi Perubahan & Fungsi**:
  - Mengeliminasi komplain teknis pelanggan gagal koneksi dengan pesan "Akun sudah online" setelah listrik padam atau modem restart mendadak, serta memastikan pelanggan terisolir tetap diarahkan ke halaman informasi tagihan dengan tertib.

---

## 🌿 Branch: `issue-423` — Perbaikan Nilai Nol pada Tipe Barang Gudang

### 📌 Informasi Issue

- **Nomor Issue**: #423
- **Judul Issue**: Penanganan Nilai Angka Nol pada Stok Minimum dan Harga Tipe Barang Gudang serta Standardisasi Label
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `a6c58645`)

### 📅 Rincian Commit

#### [[`a6c58645`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #423 (Merge issue-423 into master) - 16 Oktober 2026, 18:31:04 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/warehouseType.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/warehouseType.controller.js):
    - Perbaikan penanganan nilai `0` pada bidang `min_stock` dan `price` saat pembaruan data tipe barang (satuan maupun massal); nilai `0` kini dipertahankan dan tidak tereliminasi oleh pembersihan data form.
  - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json):
    - Perubahan label tombol pada detail tiket menjadi "Lihat Tiket" agar lebih intuitif.
    - Penggantian label tombol "Buang & Hancurkan" menjadi "Hapus" pada tabel barang gudang.
    - Penyesuaian catatan kaki laporan insiden tiket agar membaca nama perusahaan dinamis dari pengaturan sistem.
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan staf gudang dapat menonaktifkan peringatan stok minimum dengan mengisikan nilai `0`, serta mencatat barang inventaris tanpa nilai beli (gratis/hibah) dengan harga `0`.

---

## 🌿 Branch: `issue-407` — Penyempurnaan Pengaturan Pelaporan Layanan Pelanggan

### 📌 Informasi Issue

- **Nomor Issue**: #407
- **Judul Issue**: Standarisasi Istilah Pelaporan Layanan, Aksi Cepat pada Baris Tabel Pelanggan, dan Validasi Host gRPC Radius
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `199bdcfb`)

### 📅 Rincian Commit

#### [[`199bdcfb`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #407 (Merge issue-407 into master) - 16 Oktober 2026, 18:24:23 WIB
#### [[`cecca348`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #407 - 16 Oktober 2026, 18:23:06 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/taxReport.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/taxReport.service.js), [`controllers/customer.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customer.controller.js), & [`controllers/business.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/business.controller.js):
    - Penyempurnaan logika pembaruan status pelaporan layanan, pemisahan mutasi data per layanan, dan penyesuaian hak akses operasional.
  - [`backend/test/integration/settingsGrpcAddr.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/settingsGrpcAddr.test.js) [NEW]:
    - Pengujian integrasi validasi format alamat gRPC daemon Radius Server (`host:port`).
  - [`frontend/src/app/pages/users/components/TaxReportRowActions.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/components/TaxReportRowActions.jsx) [NEW] & [`taxReportConfig.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/components/taxReportConfig.js) [NEW]:
    - Tombol aksi cepat baru pada baris tabel Pelanggan dan Pelanggan Bisnis untuk mengubah status pelaporan secara langsung disertai dialog konfirmasi.
  - [`frontend/src/app/pages/users/components/taxReportConfig.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/components/taxReportConfig.test.js) [NEW]:
    - Unit test komponen logika aksi baris pelaporan.
  - [`frontend/src/app/pages/users/customer/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/customer/schema/columns.jsx) & [`users/business/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/users/business/schema/columns.jsx):
    - Penyederhanaan kolom pelaporan pada tabel menjadi lencana ringkasan status visual.
  - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json):
    - Perubahan seluruh istilah antarmuka dari "Lapor Pajak" menjadi "Pelaporan" dan status "Terlaporkan".
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan fleksibilitas bagi staf operasional untuk menandai status pelaporan layanan pelanggan langsung dari tabel tanpa harus membuka formulir edit lengkap.

---

## 🌿 Branch: `issue-359` — Pengelolaan Lokasi dan Node Fiber untuk Aplikasi Mitra

### 📌 Informasi Issue

- **Nomor Issue**: #359
- **Judul Issue**: Pengelolaan Mandiri Titik Lokasi (POP/Site) dan Node Perangkat Fiber Optik pada Partner API
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `c450151e`)

### 📅 Rincian Commit

#### [[`c450151e`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #359 (Merge issue-359 into master) - 16 Oktober 2026, 16:20:36 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/partnerApiSite.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiSite.controller.js) [NEW] & [`partnerApiNode.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/partnerApiNode.controller.js) [NEW]:
    - Pengendali baru Partner API untuk manajemen titik lokasi (Site/POP) dan node perangkat fiber optik milik mitra: daftar, ringkasan statistik, detail, penambahan, dan pengubahan data.
  - [`backend/src/routes/partnerApi.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/partnerApi.route.js):
    - Pendaftaran rute API terproteksi mitra: `/api/v1/partner-api/sites/*` dan `/api/v1/partner-api/nodes/*`.
  - [`backend/src/services/locationPoint.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/locationPoint.service.js) & [`fiberEquipment.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/fiberEquipment.service.js):
    - Integrasi isolasi kepemilikan mitra pada layanan lokasi dan perangkat fiber optik, penyembunyian nomor kontak PIC internal pada site bersama, dan validasi kapasitas perangkat.
  - [`backend/src/utils/partner-api-scope.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/partner-api-scope.js) [NEW] & [`utils/location-error.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/location-error.js) [NEW]:
    - Utilitas penegakan ruang lingkup akses mitra (*scope guard*) dan penanganan galat spesifik modul geolokasi.
  - [`backend/test/integration/partnerApiSite.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/partnerApiSite.test.js) [NEW] & [`partnerApiNode.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/partnerApiNode.test.js) [NEW]:
    - Pengujian integrasi menyeluruh mencakup skenario otentikasi mitra, batas akses data pihak ketiga, pencegahan modifikasi site bersama, dan pengujian *optimistic locking* (`__v`).
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan kemampuan mandiri (*self-service*) bagi mitra jaringan untuk memetakan infrastruktur fiber optik mereka sendiri secara aman dan terintegrasi langsung dengan core sistem.

---

## 🌿 Branch: `issue-426` — Perbaikan Pembayaran Faktur Lama dan Data Pelanggan

### 📌 Informasi Issue

- **Nomor Issue**: #426
- **Judul Issue**: Pemulihan Riwayat Pembayaran Faktur Migrasi, Validasi Pelanggan Mitra, dan Pembersihan Status Pasif
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `66cf276b`)

### 📅 Rincian Commit

#### [[`66cf276b`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #426 (Merge / final commit #426) - 16 Oktober 2026, 16:10:00 WIB
#### [[`3ad33370`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #426 - 16 Oktober 2026, 15:31:55 WIB
#### [[`459a809d`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #426 - 16 Oktober 2026, 15:30:57 WIB

- **Komponen yang Berubah**:
  - [`backend/src/utils/invoicePaymentEntries.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/invoicePaymentEntries.js) [NEW] & [`test/unit/invoicePaymentEntries.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/invoicePaymentEntries.test.js) [NEW]:
    - Utilitas normalisasi entri riwayat pembayaran pada backend untuk membaca faktur lunas warisan migrasi yang tidak memiliki struktur pembayaran modern.
  - [`backend/src/controllers/publicInvoice.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/publicInvoice.controller.js):
    - Penyajian riwayat pembayaran faktur publik lengkap dengan tanggal, metode, dan nominal yang valid.
  - [`frontend/src/app/pages/finance/invoices/utils/paymentEntries.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/utils/paymentEntries.js) [NEW]:
    - Modul utilitas frontend untuk menyusun daftar riwayat pembayaran faktur dan penamaan metode pembayaran dompet saldo yang benar.
  - [`frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/InvoiceDocument.jsx) & [`detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/finance/invoices/detail.jsx):
    - Penyempurnaan cetak faktur PDF dan halaman detail faktur agar menampilkan rincian pelunasan secara transparan.
  - [`backend/src/controllers/customerPartner.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/customerPartner.controller.js) & [`services/customer.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/customer.service.js):
    - Validasi wajib isi untuk parameter mitra (`partner_id`) pada pendaftaran pelanggan mitra, serta pembersihan tuntas atribut pasif saat pembatalan status nonaktif pelanggan.
  - [`backend/test/integration/customerPid.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/customerPid.test.js) [NEW]:
    - Pengujian integrasi integritas atribut mitra pada pendaftaran pelanggan.
- **Deskripsi Perubahan & Fungsi**:
  - Menghilangkan anomali faktur berstatus lunas namun tanpa riwayat transaksi pembayaran, serta menjamin kebersihan data master pelanggan mitra dan status aktif pelanggan.

---

## 🌿 Branch: `issue-424` — Perbaikan Tampilan Pembayaran Faktur di Ponsel

### 📌 Informasi Issue

- **Nomor Issue**: #424
- **Judul Issue**: Optimasi Tata Letak Ringkasan Pembayaran dan Tombol Aksi Faktur Publik pada Layar Seluler
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `940be6ee`)

### 📅 Rincian Commit

#### [[`940be6ee`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #424 (Merge issue-424 into master) - 16 Oktober 2026, 14:41:57 WIB
#### [[`f1bb9b0b`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #424 - 16 Oktober 2026, 14:40:58 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/public/PublicInvoicePayment.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/public/PublicInvoicePayment.jsx):
    - Penataan ulang CSS dan tata letak responsif pada tampilan ponsel (*mobile viewport*). Ringkasan nominal tagihan dan tombol "Bayar Sekarang" disematkan pada bilah melayang bawah (*sticky footer bar*) sehingga selalu terlihat tanpa menutupi rincian item tagihan.
  - [`backend/test/integration/capacitySpeed.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/capacitySpeed.controller.test.js):
    - Penyesuaian data pengujian integrasi terkait format kecepatan dan respon controller.
- **Deskripsi Perubahan & Fungsi**:
  - Memudahkan pelanggan melakukan pembayaran tagihan dari smartphone dengan tombol bayar yang selalu siap diakses tanpa perlu menggulir (*scrolling*) panjang ke bawah.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #412  | Kepemilikan IP Pool & IP Statis Mitra | IP Pool dapat dimiliki oleh mitra tertentu dan mitra dapat mengalokasikan IP statis ke akun PPPoE miliknya via Partner API. |
| #429  | Penyempurnaan Halaman Faktur Publik | Modal peringatan tagihan lama interaktif, rincian biaya admin transparan, dan nominal pembayaran akurat. |
| #430  | Ganti Sesi Otomatis & Pembatasan Isolir | Mengeliminasi galat "Akun sudah online" saat modem reboot, isolir pelanggan tertib tanpa pemutusan berulang, dan pemulihan otomatis saat lunas. |
| #423  | Perbaikan Nilai Nol Tipe Barang Gudang | Nilai 0 pada stok minimum dan harga gratis tersimpan aman tanpa terhapus sanitasi form. |
| #407  | Pengaturan Pelaporan Layanan | Istilah netral Pelaporan, tombol aksi status pelaporan per baris tabel, dan validasi alamat server Radius gRPC. |
| #359  | Lokasi & Node Fiber untuk Mitra | Mitra dapat memetakan POP/Site dan perangkat node fiber optik miliknya secara mandiri via API. |
| #426  | Riwayat Faktur Migrasi & Data Pelanggan | Faktur migrasi lama menampilkan riwayat pelunasan secara utuh, validasi mitra diperketat, dan pembatalan status pasif tuntas. |
| #424  | Tampilan Pembayaran Faktur di Ponsel | Bilah ringkasan nominal dan tombol pembayaran melayang (*sticky*) di bagian bawah layar ponsel. |

### Kemampuan Baru Pengguna/Admin

- **Admin Jaringan / NOC**:
  - Mengaktifkan fitur *Ganti Sesi Lama Otomatis* pada Pengaturan Sistem untuk membebaskan modem pelanggan yang terkunci akibat sesi gantung tanpa perlu intervensi manual kick sesi.
  - Mengatribusikan blok IP Pool Radius kepada mitra tertentu melalui menu Manajemen IPv4 sehingga alokasi IP internal dan mitra terisolasi dengan rapi.
  - Mengubah status pelaporan layanan pelanggan secara instan langsung dari baris tabel pelanggan atau bisnis tanpa perlu membuka form edit.
- **Mitra Bisnis / ISP Partner**:
  - Mengelola titik lokasi (POP/Site) dan node fiber optik miliknya sendiri secara langsung melalui integrasi Partner API.
  - Melihat daftar IP Pool miliknya dan memilih IP statis kosong yang tersedia untuk akun PPPoE pelanggan mitranya.
- **Pelanggan Publik**:
  - Memperoleh peringatan visual yang jelas jika memiliki tagihan yang lebih lama sebelum membayar tagihan berjalan.
  - Mengetahui rincian biaya administrasi pembayaran secara transparan sebelum bertransaksi.
  - Mengakses tombol pembayaran dengan mudah di layar smartphone melalui bilah aksi melayang di bagian bawah.

### Bug Fix / Solusi Masalah

- **Sesi RADIUS PPPoE Terkunci**: Mengatasi kendala modem pelanggan tidak bisa masuk kembali karena sesi lama masih tercatat online setelah pemadaman listrik atau gangguan link transmisi.
- **Disconnect Loop pada Isolir**: Memperbaiki pemutusan sesi isolir yang terus-menerus terulang pada setiap interval accounting; kini pemutusan hanya dipicu saat status pembatasan berubah.
- **Data Tipe Barang Gudang Hilang**: Menghilangkan bug di mana pengisian stok minimum 0 dan harga barang 0 dianggap kosong (*falsy*) dan terhapus saat disimpan.
- **Riwayat Pembayaran Faktur Migrasi Kosong**: Memperbaiki tampilan cetak dan detail faktur lama hasil migrasi yang sebelumnya tampil tanpa keterangan pembayaran meskipun statusnya sudah lunas.
- **Alamat Host Radius Salah Format**: Mencegah salah ketik konfigurasi alamat gRPC Radius yang dapat memutus komunikasi backend dengan daemon server.

### Menu/Fitur Baru

- **Pengaturan Sistem > Jaringan & Radius**:
  - Sakelar *Ganti Sesi Lama Otomatis* (`radius_auto_replace_stale_session`).
  - Kolom input *Batas Ganti Sesi (detik)* (`radius_replace_session_stale_threshold_seconds`).
  - Sakelar *Ganti Seketika bila MAC Sama* (`radius_replace_session_instant_same_mac`).
- **Manajemen Jaringan > IPv4 Management**:
  - Pilihan *Mitra Pemilik* pada pembuatan dan pengeditan IP Pool Radius.
- **Partner API**:
  - Endpoint `/partner-api/ipv4/pools` dan `/partner-api/ipv4/pools/:id/available-ips`.
  - Endpoint `/partner-api/sites/*` dan `/partner-api/nodes/*`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Fitur Ganti Sesi Lama Otomatis (*Auto Replace Stale Session*)

- **Penjelasan Fitur**:
  - Ketika router/modem pelanggan mati mendadak atau mengalami gangguan sinyal optik sesaat, router NAS Mikrotik terkadang terlambat mengirimkan paket *Acct-Stop* ke server RADIUS. Akibatnya, saat modem menyala kembali dan mencoba *dial-in*, permintaan koneksi ditolak dengan alasan "Akun sudah online".
  - Dengan fitur ini, server RADIUS secara cerdas memeriksa sesi lama: jika sesi lama tidak lagi mengirimkan tanda hidup (*accounting packet*) melebihi batas waktu yang ditentukan (misalnya 120 detik), atau jika permintaan login baru berasal dari MAC address perangkat yang sama persis, sistem akan otomatis memutuskan sesi lama dan langsung menerima koneksi baru pelanggan tanpa penundaan.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi web admin dengan akun berhak akses Pengaturan Sistem.
  2. Buka menu **Pengaturan** > sub-menu **Pengaturan Sistem** > buka tab atau seksi **Sistem / Jaringan**.
  3. Temukan kelompok pengaturan **Autentikasi RADIUS / Ganti Sesi**.
  4. Aktifkan sakelar **Ganti Sesi Lama Otomatis**.
  5. Tentukan **Batas Ganti Sesi (detik)**, misalnya `120` detik (waktu toleransi sesi tanpa aktivitas sebelum boleh digantikan).
  6. Aktifkan sakelar **Ganti Seketika bila MAC Sama** agar modem pelanggan yang sama langsung diterima seketika tanpa menunggu hitungan detik batas waktu.
  7. Klik tombol **Simpan Pengaturan**. Konfigurasi akan segera disinkronkan ke daemon `radius-server`.

### 2. Fitur Atribusi IP Pool & Pemilihan IP Statis untuk Mitra

- **Penjelasan Fitur**:
  - Admin pusat kini dapat menyerahkan satu atau beberapa blok subnet IP Pool Radius secara eksklusif kepada Mitra tertentu. Subnet tersebut dikunci dan hanya dapat digunakan untuk akun PPPoE milik mitra yang bersangkutan.
  - Melalui integrasi Partner API, aplikasi mitra dapat melihat kapasitas IP yang tersedia di dalam pool tersebut dan menetapkan IP statis tertentu kepada pelanggan mereka secara aman.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Jaringan** > **Manajemen IPv4**.
  2. Klik tombol **Tambah IP Pool** (atau klik tombol **Ubah** pada IP Pool yang ingin dialokasikan).
  3. Pada isian form, pilih tipe pool sebagai **Radius**.
  4. Pada bidang **Mitra Pemilik**, pilih nama mitra yang berhak atas IP Pool tersebut. Biarkan kosong jika IP Pool diperuntukkan bagi pelanggan internal perusahaan.
  5. Simpan data IP Pool.
  6. Melalui Partner API atau portal mitra, mitra kini dapat memanggil endpoint `/api/v1/partner-api/ipv4/pools` untuk mendapatkan daftar pool mereka dan `/api/v1/partner-api/ipv4/pools/:pool_id/available-ips` untuk melihat IP yang siap dialokasikan.
  7. Saat membuat atau mengubah akun PPPoE via Partner API, sertakan `ipv4_pool_id` dan `static_ip` yang valid dari daftar IP yang tersedia.
