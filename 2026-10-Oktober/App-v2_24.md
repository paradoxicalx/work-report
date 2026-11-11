# 📝 Daily Work Report - Dedy S.N Putra (2026-10-24)

---

## 📅 Laporan Harian - 24 Oktober 2026

---

## 🌿 Branch: `master` / `issue-481` — Peningkatan Manajemen Toast Notifikasi, Status Socket, dan Navigasi Panel Pengaturan Pengembang

### 📌 Informasi Issue

- **Nomor Issue**: #481
- **Judul Issue**: Manajemen Notifikasi Toast Sonner, Status Socket Toast, dan Restrukturisasi Navigasi Panel Pengaturan Pengembang
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [5ad87da5] - resolve #481 - 24 Oktober 2026, 23:00:56 WIB

- **Komponen yang Berubah**:
  - Menggabungkan pembaruan branch fitur `issue-481` ke dalam branch utama `master`.
- **Deskripsi Perubahan & Fungsi**:
  - Commit penggabungan (merge commit) seluruh implementasi fitur perbaikan navigasi pengaturan developer dan optimasi sistem toast notifikasi Sonner ke branch `master`.

#### [b787d468] - resolve #481 - 24 Oktober 2026, 23:00:20 WIB

- **Komponen yang Berubah**:
  - `frontend/src/app/layouts/MainLayout/Header/index.jsx`
  - `frontend/src/app/layouts/MainLayout/Sidebar/MainPanel/index.jsx`
  - `frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/DeveloperMenu.jsx` [NEW]
  - `frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/index.jsx`
  - `frontend/src/app/navigation/developerTabs.js` [NEW]
  - `frontend/src/app/navigation/developerTabs.test.js` [NEW]
  - `frontend/src/app/pages/settings/sections/Developer.jsx`
  - `frontend/src/components/template/DismissAllToasts.jsx` [NEW]
  - `frontend/src/components/template/Toaster.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/utils/socketStatusToast.js` [NEW]
  - `frontend/src/utils/socketStatusToast.test.js` [NEW]
  - `frontend/src/utils/toastStack.js` [NEW]
  - `frontend/src/utils/toastStack.test.js` [NEW]
- **Deskripsi Perubahan & Fungsi**:
  - **Restrukturisasi Navigasi Tab Pengembang ke Sidebar Panel**: Memindahkan daftar tab horizontal panjang pada halaman Pengaturan Pengembang (`/settings/developer`) ke panel sekunder sidebar (`PrimePanel/DeveloperMenu.jsx`). Menu dikelompokkan ke dalam 4 kategori terstruktur: *Sistem & Layanan*, *Log & Debug*, *Data*, dan *Konfigurasi*. Mendukung sinkronisasi state query parameter URL (`?tab=id`) agar tab aktif tetap persisten saat di-refresh dan dapat dibagikan langsung via tautan URL.
  - **Fitur Dismiss All Toasts (Tutup Semua Notifikasi)**: Mengembangkan komponen floating pintar `DismissAllToasts.jsx` dan utilitas kalkulasi geometri `toastStack.js`. Tombol tutup semua notifikasi muncul secara otomatis di ujung tumpukan notifikasi Sonner saat terdapat minimal 2 notifikasi aktif, menghitung posisi koordinat secara real-time via `MutationObserver` dan `getBoundingClientRect`, serta mendukung posisi tumpukan di atas/bawah/tengah layar maupun layout LTR dan RTL.
  - **Reposisi Tombol Tutup Per-Toast**: Memperbaiki letak tombol close bawaan Sonner dari pojok kiri atas ke pojok kanan atas melalui style injection variabel CSS `--toast-close-button-*` tanpa merusak spesifisitas gaya antarmuka, dengan penyesuaian otomatis untuk dokumen bertipe RTL.
  - **Socket Status Toast Throttling & Debounce**: Mengimplementasikan `createSocketStatusNotifier` pada `socketStatusToast.js` untuk mengontrol pesan konektivitas websocket di `Header`. Memberikan jeda 5 detik (`disconnectDelayMs`) sebelum memunculkan toast "Terputus dari server" guna meredam spam toast saat terjadi reconnect singkat/sesaat, menggunakan toast ID tetap (`socket-status`) agar status diperbarui di tempat yang sama, dan hanya menampilkan toast "Terhubung ke server" apabila sebelumnya sistem telah menampilkan pesan terputus.
  - **Pengujian Unit Frontend Komprehensif**: Menambahkan test suite Vitest untuk `developerTabs.test.js`, `socketStatusToast.test.js`, dan `toastStack.test.js` dengan cakupan pengujian logika penempatan koordinat, penanganan resize/mutasi DOM, dan skenario reconnection websocket.

---

## 🌿 Branch: `issue-447` / `issue-480` — Pemulihan dan Sinkronisasi Koordinat Layanan Broadband dari Data Pelanggan

### 📌 Informasi Issue

- **Nomor Issue**: #447 (Referensi: #477 / #480)
- **Judul Issue**: Pemulihan dan Sinkronisasi Koordinat Layanan Internet Broadband dari Data Pelanggan (Recovery Tool)
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [ece07bb9] - resolve #477 - 24 Oktober 2026, 11:10:34 WIB

- **Komponen yang Berubah**:
  - Menggabungkan perbaikan fitur pemulihan koordinat layanan broadband dari branch `issue-447` ke branch integrasi `origin/issue-480`.
- **Deskripsi Perubahan & Fungsi**:
  - Commit penggabungan (merge commit) branch pemulihan database koordinat layanan internet ke branch kerja integrasi sebelum digabungkan ke `master`.

#### [7c8fd3db] - resolve #447 - 24 Oktober 2026, 11:09:21 WIB

- **Komponen yang Berubah**:
  - `backend/src/controllers/radiusAuthentication.controller.js`
  - `backend/src/routes/radiusAuthentication.route.js`
  - `backend/src/services/recovery.service.js`
  - `backend/test/integration/authenticationCoordinateRecovery.test.js` [NEW]
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `partner-frontend/src/i18n/locales/en/translations.json`
  - `partner-frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - **Refaktorisasi & Penguatan Service Pemulihan Koordinat**: Memperbarui fungsi `setAuthenticationCoordinates` di `recovery.service.js` untuk menyalin data koordinat geografis pelanggan (`Customer.coordinate`) ke akun layanan internet broadband (`RadiusAuthentication.coordinate`) yang kosong/belum terisi akibat migrasi data V1.
  - **Pemberlakuan Guard Pelanggan Multi-Layanan**: Menambahkan filter agregasi MongoDB untuk memastikan koordinat pelanggan **HANYA** disalin jika pelanggan memiliki tepat 1 layanan broadband aktif/belum terhapus (`deleted: { $ne: true }`). Jika pelanggan memiliki 2 atau lebih layanan broadband, proses pemulihan otomatis melewatinya (`ignored_multiple_services`) demi mencegah kerancuan titik lokasi layanan.
  - **Dukungan Mode Simulasi (Dry-Run)**: Menambahkan opsi `dryRun` pada endpoint (baik melalui parameter query `?dryRun=true` maupun body payload `{ dryRun: true }`) sehingga developer dapat mempratinjau jumlah kandidat, data yang eligible, dan data yang diabaikan tanpa melakukan perubahan fisik pada database.
  - **Optimalisasi Batch Write Database**: Menggunakan `bulkWrite` dengan batching per 1.000 dokumen untuk memastikan eksekusi pemulihan data berjalan cepat, efisien, dan ramah memori server.
  - **Penyempurnaan Endpoint & Dokumentasi OpenAPI**: Memperluas rute `/api/v1/broadband/recovery-db` untuk menerima metode `GET` dan `POST` dengan proteksi middleware `protectedDeveloper`, mencatat log terstruktur Winston di setiap tahapan, serta memperbarui deskripsi i18n dan Swagger API.
  - **Pengujian Integrasi Backend Komprehensif**: Menambahkan berkas test Vitest `authenticationCoordinateRecovery.test.js` yang menguji skenario pelanggan tunggal, pelanggan multi-layanan, koordinat berupa spasi kosong, hingga verifikasi flag `dryRun`.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #481  | Manajemen Notifikasi Sonner, Socket Toast & Navigasi Pengaturan Developer | Meningkatkan kenyamanan antarmuka admin dengan tombol "Tutup Semua Notifikasi", mencegah spam toast saat socket terputus sesaat, dan merapikan akses modul pengaturan developer ke sidebar sekunder. |
| #447  | Pemulihan Koordinat Layanan Broadband dari Data Pelanggan | Memungkinkan pengembang/admin melakukan sinkronisasi koordinat pelanggan ke layanan internet broadband secara aman (dengan perlindungan multi-layanan dan mode simulasi dry-run). |

### Kemampuan Baru Pengguna/Admin

- **Tutup Semua Notifikasi Sekaligus**: Ketika sistem memunculkan beberapa notifikasi bersamaan (misalnya saat bulk approval, perubahan status, atau pesan error berurutan), admin kini dapat menutup seluruh notifikasi dengan satu klik tombol floating "Tutup semua notifikasi".
- **Navigasi Cepat Pengaturan Pengembang**: Developer dapat langsung mengakses modul spesifik di menu Pengaturan Pengembang melalui sidebar sekunder serta dapat menyalin/membagikan URL tab langsung (misal: `/settings/developer?tab=cron-worker`).
- **Simulasi & Sinkronisasi Koordinat Broadband Aman**: Melalui endpoint pemulihan database, developer dapat menjalankan audit koordinat broadband dengan mode simulasi (`dryRun=true`) sebelum benar-benar menerapkannya ke database.

### Bug Fix / Solusi Masalah

- **Peredam Notifikasi Socket Disconnect (Anti-Spam)**: Mengatasi masalah munculnya toast "Terputus dari server" yang berlebihan saat jaringan mengalami kedipan singkat (intermittent/reconnect otomatis socket.io). Toast terputus kini memiliki jeda toleransi 5 detik dan toast tersambung hanya muncul bila notifikasi terputus sempat tampil.
- **Perbaikan Posisi Tombol Close Toast Sonner**: Tombol silang tutup notifikasi yang sebelumnya berada di sisi kiri (menutupi awal kalimat teks notifikasi) telah dipindahkan secara rapi ke sudut kanan atas.
- **Pencegahan Salah Salin Koordinat Multi-Layanan**: Memperbaiki kelemahan pada skrip pemulihan lama yang berpotensi menyalin koordinat rumah pelanggan ke seluruh layanan broadband miliknya meskipun lokasi fisik pemasangan layanan kedua/ketiga berbeda.

### Menu/Fitur Baru

- **Panel Menu Pengembang di Sidebar Sekunder (`PrimePanel`)**: Menyajikan daftar navigasi modul developer terkelompok (Sistem & Layanan, Log & Debug, Data, Konfigurasi) yang bersih dan terpadu dengan desain layout utama.
- **Tombol Floating Dismiss All Toasts**: Tombol interaktif dinamis yang muncul otomatis saat tumpukan toast lebih dari satu.
- **Dukungan REST API Pemulihan Koordinat dengan Dry-Run**: Endpoint `POST /api/v1/broadband/recovery-db` dan `GET /api/v1/broadband/recovery-db?dryRun=true`.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**:
  1. **Tombol Tutup Semua Notifikasi Sonner (`DismissAllToasts`)**:
     Komponen ini memantau kontainer notifikasi Sonner secara dinamis menggunakan `MutationObserver`. Ketika terdeteksi ada 2 atau lebih toast yang sedang aktif/terlihat, tombol mengambang bundar dengan ikon silang akan dirender tepat di atas atau di bawah tumpukan notifikasi (menyesuaikan orientasi dan posisi toast). Mengklik tombol ini akan memanggil `toast.dismiss()` untuk membersihkan seluruh notifikasi dari layar dalam sekali tindakan.
  2. **Alat Pemulihan Koordinat Layanan Broadband (`/broadband/recovery-db`)**:
     Fitur ini dirancang untuk developer dan teknisi database guna melengkapi field `coordinate` pada koleksi `radius_authentications` yang masih kosong dari migrasi data versi sebelumnya. Skrip mengevaluasi seluruh akun internet aktif, mencari relasi ke data pelanggan (`Customer`), dan memastikan pelanggan hanya memiliki 1 layanan internet sebelum menyalin koordinat pelanggan.
- **Langkah Penggunaan (Tutorial)**:
  - **Mencoba Fitur Dismiss All Toasts**:
    1. Buka dashboard web admin DEKASIMAL V2.
    2. Lakukan tindakan yang memicu munculnya beberapa notifikasi toast secara bersamaan (atau buka DevTools Console dan panggil `toast.info('Pesan 1'); toast.info('Pesan 2');`).
    3. Perhatikan tombol bundar kecil yang muncul otomatis di samping/atas tumpukan toast.
    4. Klik tombol tersebut untuk menutup seluruh notifikasi sekaligus.
  - **Menjalankan Pemulihan Koordinat Layanan Broadband**:
    1. Login sebagai akun developer atau gunakan bearer token developer aktif.
    2. Jalankan simulasi terlebih dahulu menggunakan cURL atau Postman:
       ```bash
       curl -X GET "https://api.dekasimal.local/api/v1/broadband/recovery-db?dryRun=true" \
            -H "Authorization: Bearer <TOKEN_DEVELOPER>"
       ```
    3. Periksa respons JSON untuk melihat statistik (`candidates`, `would_update`, `ignored_multiple_services`, `skipped_no_customer_coordinate`).
    4. Jika hasil simulasi sudah sesuai, jalankan eksekusi pemulihan sebenarnya:
       ```bash
       curl -X POST "https://api.dekasimal.local/api/v1/broadband/recovery-db" \
            -H "Authorization: Bearer <TOKEN_DEVELOPER>" \
            -H "Content-Type: application/json" \
            -d '{"dryRun": false}'
       ```
