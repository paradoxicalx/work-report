# 📝 Daily Work Report - Dedy S.N Putra (2026-10-15)

---

## 📅 Laporan Harian - 15 Oktober 2026

> Laporan ini merangkum seluruh aktivitas rekayasa perangkat lunak, inisialisasi arsitektur frontend aplikasi mitra (*Partner Frontend*), porting modul pelanggan dan layanan PPPoE ke portal mitra, standardisasi dan perbaikan penanganan batas kecepatan kosong (*empty speed limit*) pada profil dan akun layanan jaringan, penyempurnaan modul tiket terbuka pada Telegram Mini App dengan editor komentar kaya (*Rich Text Editor*) dan kompresi foto otomatis, hingga pemutakhiran riwayat *changelog* rilis serta sinkronisasi penuh ke branch `production` pada monorepo DEKASIMAL V2 per 15 Oktober 2026.
> 
> Fokus dan pencapaian utama hari ini meliputi:
> 1. **Inisialisasi Partner Frontend (`issue-401`)**: Pembangunan aplikasi web baru terdedikasi untuk mitra bisnis (*partner-frontend*) berbasis React 19, Vite 6, Tailwind CSS v4, Redux Toolkit, React Router v7, dan Vitest. Mengonfigurasi guard otentikasi token mitra, manajemen state global, layout responsif (Sideblock & MainLayout), sistem tema, dukungan internasionalisasi dwibahasa (ID/EN), hingga utilitas jaringan terisolasi.
> 2. **Porting Modul Pelanggan & Layanan PPPoE Partner Frontend (`issue-402`)**: Implementasi penuh manajemen data pelanggan mitra (`customers/customer`) dan operasional akun PPPoE (`services/pppoe`) di portal mitra. Menyediakan formulir pembuatan dan pembaruan pelanggan, skema validasi Yup, tabel interaktif dengan filter dan paginasi, kartu detail pelanggan, formulir akun PPPoE dengan pemilihan profil bandwidth, pemantauan status koneksi/IP/uptime, serta tampilan log sesi RADIUS.
> 3. **Perbaikan Batas Kecepatan Kosong & Pembersihan Data (`issue-419`)**: Mengatasi kerentanan sistem di mana profil broadband, profil hotspot, dan akun layanan yang tidak ditentukan kecepatannya tersimpan sebagai `0K/0K` sehingga menonaktifkan pembatasan kecepatan di router Mikrotik. Menerapkan pertahanan berlapis di backend (`stripEmptySpeedLimit` / `unsetEmptySpeedLimit`), form frontend, mesin penentu batas kecepatan efektif di Go `radius-server` (`internal/domain/limits`), serta menghadirkan alat pemulihan data (*Empty Speed Limit Recovery Tool*) di dashboard Developer.
> 4. **Penyempurnaan Tiket Terbuka Telegram Mini App (`issue-347`)**: Penyelesaian antarmuka tiket terbuka untuk teknisi lapangan di Telegram Mini App. Menghadirkan laci editor komentar (*CommentEditorDrawer.jsx*) dengan bilah format teks kaya (tebal, miring, garis bawah, coret, heading, list, quote, code), lampiran foto dari galeri/kamera dengan kompresi otomatis HTML5 Canvas (100–300 KB), integrasi umpan balik haptik Telegram WebApp, serta pengamanan transport logger Telegram untuk mencegah perulangan galat (*infinite alert loop*).
> 5. **Pemutakhiran Changelog & Rilis Produksi**: Penyusunan catatan rilis terstruktur versi v1.96.0 (#415), v1.97.0 (#347), dan v1.97.1 (#419), pembaruan panduan changelog, serta penggabungan dan sinkronisasi berkas rilis ke branch `master` dan `production`.

---

## 🌿 Branch: `master` & `production` — Pemutakhiran Changelog Sistem & Sinkronisasi Rilis Produksi

### 📌 Informasi Issue

- **Nomor Issue**: N/A (Release Maintenance)
- **Judul Issue**: Pemutakhiran Berkas Riwayat Rilis Changelog (v1.96.0, v1.97.0, v1.97.1) dan Sinkronisasi Branch Master ke Produksi
- **Status Branch**: `Sudah di-merge` (Tersinkronisasi penuh pada `origin/master` dan `origin/production`)

### 📅 Rincian Commit

#### [[`7dd4e71b`](file:///home/dhedhy/Project/Dekasimal-V2)] - Merge branch 'master' into production - 15 Oktober 2026, 19:24:22 WIB

- **Deskripsi Perubahan & Fungsi**:
  - Penggabungan pemutakhiran changelog dan seluruh rilis fitur harian dari branch `master` langsung ke branch `production`.

#### [[`772ce931`](file:///home/dhedhy/Project/Dekasimal-V2)] - update changelog - 15 Oktober 2026, 19:24:11 WIB

- **Komponen yang Berubah**:
  - [`backend/src/data/changelog/index.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/index.json):
    - Pendaftaran rilis baru `v1.97.1` (#419), `v1.97.0` (#347), dan `v1.96.0` (#415) ke dalam indeks katalog pencarian changelog aplikasi.
  - [`backend/src/data/changelog/releases/issue-419.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-419.json) [NEW]:
    - Catatan rilis v1.97.1: rincian perbaikan batas kecepatan kosong, validasi kapasitas dedicated internet, dan alat pemulihan data pada menu Developer.
  - [`backend/src/data/changelog/releases/issue-347.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-347.json) [NEW]:
    - Catatan rilis v1.97.0: dokumentasi modul tiket Telegram Mini App, kartu tiket ringkas, halaman detail dengan quick-action WhatsApp/Telepon/Peta, smart pinning, dan laci editor komentar teks kaya dengan kompresi foto.
  - [`backend/src/data/changelog/releases/issue-415.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-415.json) [NEW]:
    - Catatan rilis v1.96.0: dokumentasi aksi pasca-pelepasan tiket pelepasan layanan, pembatalan tagihan terintegrasi, dan pemantauan latensi jaringan.
  - [`documentations/CHANGELOG_INSTRUCTION.md`](file:///home/dhedhy/Project/Dekasimal-V2/documentations/CHANGELOG_INSTRUCTION.md):
    - Penambahan instruksi baku panduan penulisan entri changelog untuk AI Agent dan pengembang monorepo.
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan seluruh pengguna dan staf dapat membaca histori pembaruan fitur secara transparan melalui menu Bantuan & Changelog di aplikasi web.

---

## 🌿 Branch: `issue-402` — Porting Modul Pelanggan & Layanan PPPoE pada Partner Frontend

### 📌 Informasi Issue

- **Nomor Issue**: #402
- **Judul Issue**: Porting dan Implementasi Modul Manajemen Pelanggan dan Layanan Akun PPPoE pada Partner Frontend
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `f3eb9a67` dan dirilis ke `production`)

### 📅 Rincian Commit

#### [[`f3eb9a67`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #402 (Merge issue-402 into master) - 15 Oktober 2026, 19:19:11 WIB
#### [[`ca175a63`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #402 - 15 Oktober 2026, 19:14:48 WIB

- **Komponen yang Berubah**:
  - [`partner-frontend/src/app/pages/customers/customer/components/CustomerForm.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/customers/customer/components/CustomerForm.jsx) [NEW]:
    - Formulir terstruktur pendaftaran dan pengeditan data pelanggan mitra: nama lengkap, nomor telepon (otomatis format kanonik), nomor identitas KTP/SIM, alamat pemasangan, titik koordinat peta GPS, dan penanda wilayah layanan.
  - [`partner-frontend/src/app/pages/customers/customer/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/customers/customer/create.jsx) [NEW] & [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/customers/customer/edit.jsx) [NEW]:
    - Kontainer halaman tambah pelanggan baru dan ubah pelanggan yang mengonsumsi endpoint API mitra backend dengan umpan balik visual (*toast* pemberitahuan dan penanganan galat).
  - [`partner-frontend/src/app/pages/customers/customer/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/customers/customer/index.jsx) [NEW]:
    - Halaman daftar tabel pelanggan mitra dilengkapi bilah pencarian cerdas (*debounced*), filter status langganan, dan tombol tambah pelanggan.
  - [`partner-frontend/src/app/pages/customers/customer/schema/columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/customers/customer/schema/columns.jsx) [NEW] & [`customerSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/customers/customer/schema/customerSchema.js) [NEW]:
    - Definisi kolom tabel TanStack Table (ID pelanggan, nama, kontak, alamat, status) dan skema validasi Yup untuk menjamin integritas data sebelum dikirimkan ke server.
  - [`partner-frontend/src/app/pages/customers/customer/view.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/customers/customer/view.jsx) [NEW]:
    - Halaman profil detail pelanggan: informasi identitas, ringkasan layanan aktif, koordinat lokasi pada peta interaktif, serta tautan cepat ke akun layanan.
  - [`partner-frontend/src/app/pages/services/pppoe/components/AccountForm.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/services/pppoe/components/AccountForm.jsx) [NEW]:
    - Formulir komprehensif manajemen akun PPPoE pelanggan mitra: username, password, pemilihan profil paket broadband, router NAS Mikrotik, IP statis opsional, MAC address binding, dan opsi batas kuota/waktu.
  - [`partner-frontend/src/app/pages/services/pppoe/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/services/pppoe/create.jsx) [NEW] & [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/services/pppoe/edit.jsx) [NEW]:
    - Halaman registrasi akun koneksi PPPoE baru dan peremajaan konfigurasi akun pelanggan mitra.
  - [`partner-frontend/src/app/pages/services/pppoe/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/services/pppoe/index.jsx) [NEW]:
    - Tabel ringkasan akun PPPoE dengan lencana status online/offline realtime, alamat IP yang sedang digunakan, router NAS terhubung, dan waktu koneksi aktif.
  - [`partner-frontend/src/app/pages/services/pppoe/schema/accountSchema.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/services/pppoe/schema/accountSchema.js) [NEW], [`columns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/services/pppoe/schema/columns.jsx) [NEW], & [`logColumns.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/services/pppoe/schema/logColumns.jsx) [NEW]:
    - Aturan validasi formulir dan konfigurasi kolom tabel akun serta tabel riwayat log autentikasi RADIUS.
  - [`partner-frontend/src/app/pages/services/pppoe/view.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/pages/services/pppoe/view.jsx) [NEW]:
    - Tampilan detail akun PPPoE: status koneksi langsung, rincian paket, riwayat sesi koneksi (*accounting logs*), pemakaian kuota upload/download, dan riwayat alamat IP.
  - [`partner-frontend/src/app/router/customers/customerRoute.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/router/customers/customerRoute.jsx) [NEW] & [`services/pppoeRoute.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/router/services/pppoeRoute.jsx) [NEW]:
    - Pendaftaran rute URL `/customers/customer/*` dan `/services/pppoe/*` pada React Router v7 aplikasi mitra dengan proteksi `AuthGuard`.
  - [`partner-frontend/src/utils/radiusAccount.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/utils/radiusAccount.js) [NEW] & [`radiusAccount.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/utils/radiusAccount.test.js) [NEW]:
    - Utilitas parsing data akun RADIUS, pemformatan status sesi koneksi, normalisasi parameter teknis, dan verifikasi unit test dengan cakupan 100%.
  - [`partner-frontend/src/app/contexts/auth/Provider.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/contexts/auth/Provider.jsx):
    - Penyesuaian context otentikasi agar data identitas mitra tersimpan akurat dan membatasi data hanya untuk pelanggan milik mitra yang bersangkutan.
  - [`partner-frontend/src/components/shared/form/Combobox.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/components/shared/form/Combobox.jsx) & [`table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/components/shared/table/rows.jsx):
    - Peningkatan komponen pemilihan dropdown combobox dan baris tabel dinamis untuk mendukung interaktivitas form pelanggan dan akun.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan kemampuan mandiri (*self-service*) bagi mitra ISP untuk mendaftarkan pelanggan mereka dan mengonfigurasi akun PPPoE secara langsung melalui portal mitra tanpa perlu bergantung pada admin pusat.

---

## 🌿 Branch: `issue-419` — Perbaikan Batas Kecepatan Kosong pada Profil dan Layanan

### 📌 Informasi Issue

- **Nomor Issue**: #419
- **Judul Issue**: Perbaikan Batas Kecepatan Kosong pada Profil dan Layanan (Standardisasi Bandwidth, Validasi Kapasitas, & Tool Pemulihan Data)
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `0b466039` dan dirilis ke `production` melalui commit `a7ffd7d4`)

### 📅 Rincian Commit

#### [[`0b466039`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #419 (Merge issue-419 into master) - 15 Oktober 2026, 18:22:49 WIB
#### [[`e8beb67e`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #419 - 15 Oktober 2026, 18:22:03 WIB

- **Komponen yang Berubah**:
  - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md):
    - Dokumentasi arsitektur baku: penegasan bahwa batas kecepatan (`speed_limit`) yang tidak diatur adalah field **TIDAK ADA** (bukan string `0K/0K`). Menguraikan pertahanan 4 lapis sistem dari form frontend, sanitasi backend, sanitasi encode atribut Go RADIUS, hingga tool pemulihan database.
  - [`backend/src/utils/validation-data.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/validation-data.js):
    - Implementasi helper `stripEmptySpeedLimit` (untuk create) dan `unsetEmptySpeedLimit` (untuk update dengan operator Mongoose `$unset` pada ketiga field `speed_limit`, `speed_option`, dan `use_burst` secara atomik). Nilai nol satu arah (misal `0K/10M`) tetap sah dan tidak dihapus.
  - [`backend/src/utils/capacitySpeed.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/capacitySpeed.js) [NEW] & [`unit/capacitySpeed.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/capacitySpeed.test.js) [NEW]:
    - Modul utilitas validasi angka kapasitas bandwidth untuk layanan Dedicated Internet dan Akses Data, memastikan nilai bernilai numerik positif (> 0 Mbps) dan bukan nilai kosong/falsy.
  - [`backend/src/controllers/productDedicatedInternet.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/productDedicatedInternet.controller.js) & [`productDataAccess.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/productDataAccess.controller.js):
    - Validasi ketat kapasitas layanan: menolak nilai kosong atau nol dengan pesan i18n terstruktur `req.t('capacityMustBeNumber')` dengan status HTTP 422.
  - [`backend/src/controllers/radiusProfile.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/radiusProfile.controller.js) & [`hotspotProfile.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/hotspotProfile.controller.js):
    - Pengintegrasian `stripEmptySpeedLimit` dan `unsetEmptySpeedLimit` pada seluruh alur create dan update profil broadband dan profil hotspot.
  - [`backend/src/services/recovery.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/recovery.service.js) & [`routes/radiusProfile.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/radiusProfile.route.js):
    - Pembangunan servis dan endpoint `POST /broadband-profile/recovery-speed-limit` untuk memindai dokumen profil dan akun lama yang memiliki `speed_limit` bernilai `0K/0K` (termasuk dokumen soft-deleted), menyajikan simulasi *dry-run*, dan membersihkan field tersebut secara permanen dengan aman (idempoten).
  - [`backend/test/integration/radiusProfile.speedLimit.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/radiusProfile.speedLimit.test.js) [NEW] & [`capacitySpeed.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/capacitySpeed.controller.test.js) [NEW]:
    - Pengujian integrasi komprehensif (41 pengujian) yang memverifikasi penolakan kapasitas tidak valid, penghapusan field kosong pada update, dan integritas atribut RADIUS.
  - [`frontend/src/utils/mikrotikBurst.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/mikrotikBurst.js) & [`partner-frontend/src/utils/mikrotikBurst.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/utils/mikrotikBurst.js):
    - Modifikasi fungsi `buildSpeedLimit`: jika nilai rx/tx tidak diisi, kembalikan string kosong `""` alih-alih merakit `0K/0K`.
  - Formulir Profil dan Layanan Frontend:
    - [`broadbandProfile/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadbandProfile/create.jsx), [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/broadbandProfile/edit.jsx), [`hotspotProfile/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/hotspotProfile/create.jsx), [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/services/hotspotProfile/edit.jsx), [`radiusNonCustomer/create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/radiusNonCustomer/create.jsx), [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/network/radiusNonCustomer/edit.jsx), serta versi pada `partner-frontend`.
    - Hanya mengirimkan payload `speed_limit` apabila pengguna benar-benar memasukkan nilai kecepatan.
  - [`radius-server/internal/domain/limits/effective.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/domain/limits/effective.go) [NEW] & [`effective_test.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/domain/limits/effective_test.go) [NEW]:
    - Logika penentuan batas efektif di Go: memastikan nilai kosong/nol pada akun tidak menutupi (*override*) nilai valid yang ada di profil, dan sebaliknya (paritas V1).
  - [`radius-server/pkg/radiusproto/ratelimit.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/pkg/radiusproto/ratelimit.go) [NEW] & [`encode.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/pkg/radiusproto/encode.go):
    - Fungsi `IsEmptyRateLimit` untuk memeriksa apakah atribut rate-limit bernilai kosong atau bernilai `0K/0K` murni sebelum di-encode ke paket RADIUS Access-Accept PPPoE. Jika kosong, atribut `Mikrotik-Rate-Limit` tidak dikirimkan ke NAS Mikrotik.
  - [`radius-server/internal/transport/pppoe/auth_listener.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/transport/pppoe/auth_listener.go) & [`repository/mongo/authentication_repo.go`](file:///home/dhedhy/Project/Dekasimal-V2/radius-server/internal/repository/mongo/authentication_repo.go):
    - Penyertaan field pembatasan akun pada proyeksi kueri `authSnapshotProjection` dan integrasi evaluator limit efektif saat autentikasi PPPoE berlangsung.
- **Deskripsi Perubahan & Fungsi**:
  - Menghilangkan bug bypass pembatasan kecepatan router Mikrotik akibat pengiriman nilai `0K/0K`, serta memberikan mekanisme pembersihan data lama secara otomatis bagi administrator jaringan.

---

## 🌿 Branch: `issue-401` — Inisialisasi Arsitektur Monorepo Partner Frontend (`partner-frontend`)

### 📌 Informasi Issue

- **Nomor Issue**: #401
- **Judul Issue**: Inisialisasi dan Pembangunan Pondasi Arsitektur Partner Frontend (`partner-frontend`) pada Monorepo DEKASIMAL V2
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `1de31c5c` dan dirilis ke `production`)

### 📅 Rincian Commit

#### [[`1de31c5c`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #401 (Merge issue-401 into master) - 15 Oktober 2026, 12:53:39 WIB
#### [[`1302c4ab`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #401 - 15 Oktober 2026, 12:50:47 WIB

- **Komponen yang Berubah**:
  - Direktori [`partner-frontend/`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend) [NEW] (2.449 berkas, +375.008 baris kode):
    - **Konfigurasi Project**: [`package.json`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/package.json), [`vite.config.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/vite.config.js), [`vitest.config.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/vitest.config.js), [`vercel.json`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/vercel.json), dan konfigurasi ESLint/Prettier.
    - **Manajemen State & Store**: [`src/store.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/store.js) mengonfigurasi Redux Toolkit untuk state aplikasi mitra.
    - **Otorisasi & Guard**: [`src/middleware/AuthGuard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/middleware/AuthGuard.jsx) dan [`GhostGuard.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/middleware/GhostGuard.jsx) untuk mengamankan halaman internal dari akses pengguna tanpa sesi aktif.
    - **Sistem Context React**: Otentikasi mitra ([`contexts/auth`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/contexts/auth)), koneksi Socket.io ([`contexts/socket`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/contexts/socket)), tema gelap/terang ([`contexts/theme`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/contexts/theme)), konfigurasi ([`contexts/config`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/contexts/config)), dan pelacak ukuran layar ([`contexts/breakpoint`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/contexts/breakpoint)).
    - **Layout Responsif Modern**: [`MainLayout`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/layouts/MainLayout) (navigasi atas dan drawer) dan [`Sideblock`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/layouts/Sideblock) (sidebar navigasi modular).
    - **Navigasi & Menu Katalog**: [`src/app/navigation/index.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/navigation/index.js) menyusun menu khusus mitra (Dashboard, Pelanggan, Layanan, Keuangan, Jaringan, Tiket, Customer Service).
    - **Router Scaffolding**: [`src/app/router/router.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/app/router/router.jsx) dan modul rute per fitur berbasis React Router v7.
    - **Internasionalisasi (i18n)**: [`src/i18n/config.js`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/i18n/config.js) dan kamus terjemahan Bahasa Indonesia & Inggris lengkap ([`translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/partner-frontend/src/i18n/locales/id/translations.json)).
    - **Pustaka Komponen UI**: Komponen tabel TanStack Table v8, form input, modal, lencana status, grafik ApexCharts, kartu metrik, dan peta Leaflet.
    - **Suite Pengujian Vitest**: 54 file pengujian unit dan integrasi dengan 617 test case yang lulus 100%.
- **Deskripsi Perubahan & Fungsi**:
  - Memisahkan portal frontend mitra bisnis dari dashboard admin inti monorepo guna meningkatkan skalabilitas, independensi deployment, dan privasi hak akses.

---

## 🌿 Branch: `issue-347` — Modul Tiket Terbuka pada Telegram Mini App, Rich Text Editor Drawer, & Pengamanan Logger

### 📌 Informasi Issue

- **Nomor Issue**: #347
- **Judul Issue**: Modul Tiket Terbuka Telegram Mini App, Rich Text Editor Drawer, Lampiran Foto Terkompresi, dan Pengamanan Alert Telegram
- **Status Branch**: `Sudah di-merge` (Digabungkan ke `master` melalui commit `6c1b4999` dan dirilis ke `production`)

### 📅 Rincian Commit

#### [[`6c1b4999`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #347 (Merge issue-347 into master) - 15 Oktober 2026, 11:54:38 WIB
#### [[`fcb4653d`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #347 - 15 Oktober 2026, 11:53:50 WIB

- **Komponen yang Berubah**:
  - [`telegram-apps/src/pages/tickets/CommentEditorDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/tickets/CommentEditorDrawer.jsx) [NEW]:
    - Komponen laci bawah (*bottom drawer*) interaktif untuk menulis tanggapan dan komentar tiket di ponsel pintar.
    - Dilengkapi bilah alat (*toolbar*) formatting teks kaya: Tebal (*Bold*), Miring (*Italic*), Garis Bawah (*Underline*), Coret (*Strikethrough*), Judul H1/H2, Daftar Poin (*Bullet List*), Daftar Angka (*Numbered List*), Kutipan (*Blockquote*), dan Format Kode (*Inline Code*).
    - Fitur lampiran gambar cerdas: mengambil foto dari kamera ponsel langsung atau memilih dari galeri, dengan fungsi kompresi gambar berbasis HTML5 Canvas (`compressImageFile`, dimensi maksimal 1200×1200 px, kualitas JPEG 75%) yang mereduksi ukuran berkas menjadi 100–300 KB sebelum dikirim ke server.
    - Integrasi umpan balik getaran (*haptic feedback*) Telegram SDK saat tombol aksi ditekan atau komentar berhasil terkirim.
  - [`telegram-apps/src/pages/tickets/TicketDetail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/tickets/TicketDetail.jsx):
    - Integrasi tombol "Tulis Komentar" di bilah bawah tetap (*fixed bottom bar*) yang memicu pembukaan `CommentEditorDrawer`.
    - Peningkatan visualisasi linimasa komentar: render tag HTML aman, tampilan kartu lampiran foto dengan pratinjau resolusi tinggi, dan identitas penulis komentar.
    - Penambahan tombol aksi cepat di bagian atas: Telepon (`tel:`), Chat WhatsApp (`https://wa.me/`), dan Buka Lokasi di Google Maps dari titik GPS pelanggan.
  - [`telegram-apps/src/pages/tickets/TicketList.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/tickets/TicketList.jsx):
    - Penyempurnaan penyaringan jenis tiket: semua tiket terbuka, pelanggan, survei, pemasangan, mitra, pelepasan, pembayaran, backbone, NOC, dan tiket umum.
    - Pemisahan visual bagian tiket yang disematkan (*Pinned Tickets*) di bagian paling atas.
    - Penanganan status kosong (*empty state*) dan galat jaringan lengkap dengan tombol coba lagi (*retry*).
  - [`backend/src/utils/logger.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/logger.js), [`telegram.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegram.js), & [`telegramAlert.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/telegramAlert.js):
    - **Pencegahan Infinite Alert Loop**: Pemberian penanda khusus `fromTelegramTransport` pada Winston logger agar kegagalan koneksi saat bot Telegram mengirim pesan galat tidak memicu pengiriman log peringatan baru secara berulang tanpa henti.
  - [`backend/test/unit/ticketMobile.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/ticketMobile.service.test.js) [NEW]:
    - Penambahan 124 baris pengujian unit untuk endpoint backend `findMobileOpenTickets`: verifikasi kriteria filter tiket belum selesai, sanitasi tag HTML cuplikan pesan, pagination, dan sanitasi karakter pencarian.
  - [`telegram-apps/src/pages/Home.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/telegram-apps/src/pages/Home.jsx):
    - Pemasangan kartu menu akses cepat "Tiket Terbuka" dengan verifikasi izin hak akses multi-tipe tiket (`ticketCustomer.list|ticketSurvey.list|...`).
- **Deskripsi Perubahan & Fungsi**:
  - Memaksimalkan mobilitas teknisi lapangan dalam menyelesaikan gangguan pelanggan dan melaporkan progres langsung dari tempat kejadian melalui Telegram.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #402  | Porting Pelanggan & PPPoE Partner Frontend | Mitra dapat mendaftarkan pelanggan dan mengonfigurasi akun PPPoE secara mandiri melalui antarmuka modern yang cepat dan terisolasi. |
| #419  | Perbaikan Batas Kecepatan Kosong | Meniadakan celah bypass limitasi bandwidth Mikrotik akibat nilai `0K/0K`, validasi ketat kapasitas dedicated, dan pembersihan otomatis data lama. |
| #401  | Inisialisasi Arsitektur Partner Frontend | Pemisahan aplikasi frontend mitra ke dalam project tersendiri berbasis React 19 & Tailwind v4 untuk skalabilitas monorepo yang lebih tinggi. |
| #347  | Tiket Terbuka Telegram Mini App | Teknisi lapangan dapat memantau tiket aktif, smart-pinning, membaca rincian kendala, membuka peta GPS, dan menulis komentar teks kaya berlampiran foto langsung dari ponsel. |
| #415 / Maint | Changelog & Sinkronisasi Produksi | Seluruh perubahan sistem terarsip rapi pada katalog changelog v1.96.0 s/d v1.97.1 dan tersinkronisasi mulus ke server produksi. |

### Kemampuan Baru Pengguna/Admin/Mitra

- **Mitra Bisnis**: Kini memiliki portal mandiri (*Partner Frontend*) untuk mengelola pendaftaran pelanggan mereka sendiri, memantau daftar layanan aktif, membuat akun koneksi PPPoE pelanggan, serta memantau status online/offline dan pemakaian kuota secara *real-time*.
- **Teknisi Lapangan**: Dapat merespons tiket gangguan langsung dari aplikasi Telegram ponsel pintar mereka, memformat pesan teks laporan secara rapi (poin, tebal, kutipan kode), serta mengunggah foto bukti perbaikan atau dokumentasi redaman kabel optik yang otomatis dikompresi sehingga hemat kuota internet.
- **Administrator Jaringan / Developer**: Memiliki alat pemulihan data (*Empty Speed Limit Recovery Tool*) pada menu Pengaturan Developer untuk mengaudit dan membersihkan profil maupun akun jaringan lama yang tidak memiliki konfigurasi batas kecepatan.

### Bug Fix / Solusi Masalah

- **Pemberian Bandwidth Nol (Bypass Rate-Limit)**: Diatasi sepenuhnya dengan menganggap nilai batas kecepatan kosong sebagai "field tidak ada" (bukan `0K/0K`), sehingga router Mikrotik tidak membuat *simple queue* tanpa batas kecepatan.
- **Pencegahan Infinite Alert Loop Telegram**: Diatasi dengan memasang pelindung penanda konteks pada transport Winston `TelegramAlertTransport`, sehingga kegagalan jaringan saat mengirim notifikasi bot Telegram tidak memicu banjir notifikasi error sistem.
- **Validasi Nilai Kapasitas Falsy**: Memperbaiki endpoint dedicated internet dan akses data agar menolak nilai kapasitas yang kosong atau bernilai 0 dengan kode status HTTP 422 dan pesan terjemahan yang jelas.

### Menu/Fitur Baru

- **Portal Aplikasi Partner Frontend (`partner-frontend`)**:
  - Modul Manajemen Pelanggan (`/customers/customer`): Tambah, ubah, hapus, detail profil, dan integrasi peta lokasi.
  - Modul Layanan PPPoE (`/services/pppoe`): Konfigurasi akun, pemantauan status koneksi, log autentikasi RADIUS, dan statistik sesi.
- **Telegram Mini App**:
  - Menu "Tiket Terbuka" pada dashboard utama Telegram Mini App (`/tickets/list`).
  - Halaman Detail Tiket (`/tickets/detail/:type/:id`) dengan aksi cepat Panggilan Telepon, Pesan WhatsApp, dan Google Maps.
  - Laci Editor Komentar (*Comment Editor Drawer*) lengkap dengan pemformat teks kaya dan kompresi foto kamera/galeri.
- **Pengaturan Developer (Web Admin)**:
  - Kartu utilitas *Pembersihan Batas Kecepatan Kosong* untuk simulasi (*dry-run*) dan eksekusi pembersihan data profil/layanan.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Mengelola Tiket dan Menulis Komentar via Telegram Mini App

- **Penjelasan Fitur**: Fitur ini memungkinkan staf lapangan atau teknisi piket mengakses daftar tiket gangguan operasional langsung dari ponsel pintar mereka melalui bot Telegram tanpa harus login ke dashboard web komputer.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka bot Telegram resmi perusahaan di ponsel dan ketuk tombol menu **Buka Aplikasi** (*Open Mini App*).
  2. Pada halaman Beranda aplikasi, pilih menu kartu **Daftar Tiket Terbuka**.
  3. Gunakan kolom pencarian di bagian atas untuk mencari nomor tiket atau nama pelanggan, atau ketuk tab kategori (*Pelanggan, Pemasangan, Survei, dll.*) untuk menyaring daftar.
  4. Ketuk salah satu kartu tiket untuk membuka halaman **Detail Tiket**.
  5. Anda dapat mengetuk tombol aksi cepat: **Telepon** untuk menghubungi pelanggan, **WhatsApp** untuk mengirim pesan instan, atau **Maps** untuk panduan rute navigasi menuju lokasi pelanggan.
  6. Untuk menambahkan laporan progres: ketuk tombol **Tulis Komentar** di bagian bawah layar.
  7. Laci bawah editor akan terbuka. Tuliskan pesan Anda, gunakan tombol pemformat teks (Tebal, Poin, dsb.) jika diperlukan.
  8. Ketuk ikon **Kamera** untuk memotret bukti pengerjaan atau ikon **Galeri** untuk memilih foto (foto otomatis dikompresi agar hemat data).
  9. Ketuk tombol **Kirim** (ikon pesawat kertas). Komentar dan foto akan langsung tersimpan dan tercatat di riwayat linimasa tiket.

### 2. Mengoperasikan Portal Mandiri Mitra (*Partner Frontend*)

- **Penjelasan Fitur**: Antarmuka terpisah khusus mitra bisnis ISP untuk mengelola siklus hidup pelanggan dan akun layanan broadband PPPoE mereka secara independen.
- **Langkah Penggunaan (Tutorial)**:
  1. Akses alamat web portal mitra melalui peramban web dan lakukan login menggunakan kredensial akun mitra.
  2. **Mendaftarkan Pelanggan Baru**:
     - Buka menu navigasi **Pelanggan** > **Daftar Pelanggan**, lalu klik tombol **Tambah Pelanggan**.
     - Lengkapi identitas pelanggan (Nama, Nomor Telepon WhatsApp, NIK KTP, Alamat Lengkap, dan pin koordinat peta GPS).
     - Klik **Simpan**. Data pelanggan baru akan langsung terdaftar di bawah akun mitra Anda.
  3. **Membuat Akun Layanan PPPoE**:
     - Buka menu **Layanan** > **Akun PPPoE**, lalu klik tombol **Tambah Akun**.
     - Pilih nama pelanggan yang telah didaftarkan sebelumnya.
     - Tentukan *Username* dan *Password* koneksi PPPoE.
     - Pilih **Paket Profil Broadband** yang sesuai dengan kecepatan yang diinginkan pelanggan.
     - Pilih router **NAS Mikrotik** penanggung jawab area pelanggan tersebut.
     - Klik **Simpan**. Akun PPPoE akan segera tersinkronisasi ke server RADIUS dan siap digunakan pada perangkat modem/router ONT pelanggan.

### 3. Menjalankan Pembersihan Batas Kecepatan Kosong (*Developer Recovery Tool*)

- **Penjelasan Fitur**: Utilitas pemeliharaan database untuk menghapus atribut batas kecepatan kosong (`0K/0K`) pada dokumen profil dan akun layanan lama agar tidak memicu simple queue tanpa batas di router Mikrotik.
- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke dashboard web utama menggunakan akun administrator yang memiliki hak akses Developer / Super Admin.
  2. Masuk ke menu **Pengaturan** (*Settings*) > tab **Developer**.
  3. Cari kartu bagian **Alat Pemulihan Data & Jaringan**.
  4. Temukan opsi **Pembersihan Batas Kecepatan Kosong** (*Clean Empty Speed Limits*).
  5. Klik tombol **Simulasi (Dry Run)** terlebih dahulu untuk melihat berapa banyak dokumen profil broadband, profil hotspot, dan akun non-pelanggan yang terdampak.
  6. Setelah meninjau ringkasan data, klik tombol **Jalankan Pembersihan** dan konfirmasi dialog.
  7. Sistem akan secara atomik melakukan `$unset` terhadap field `speed_limit`, `speed_option`, dan `use_burst` pada dokumen yang tidak memiliki nilai kecepatan aktif.
