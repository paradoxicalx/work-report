# 📝 Daily Work Report - Dedy (2026-10-16)

---

## 📌 Informasi Issue

- **Nomor Issue**: #375
- **Judul Issue**: Routing ipaymu ke App v2

## 📅 Laporan Harian - 16 Oktober 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

_Tidak ada pekerjaan yang belum di-commit (working tree bersih). Seluruh perubahan kode telah di-commit ke repositori pada branch `issue-375`._

### 📅 Rincian Commit

#### [e325791](https://github.com/paradoxicalx/ISPF_V1/commit/e325791) - resolve #375 (#375 - Routing ipaymu ke App v2)

- **Komponen yang Berubah**:
  - `[backend/helpers/migration.help.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/helpers/migration.help.js)` [NEW]
  - `[backend/routes/customer_app.route.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/routes/customer_app.route.js)`
  - `[backend/index.core.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/index.core.js)`
  - `[backend/cron/1_minute.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_minute.cron.js)`
  - `[backend/cron/1_day.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_day.cron.js)`
  - `[backend/cron/1_month.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_month.cron.js)`
  - `[backend/env.example](file:///home/dhedhy/Project/Dekasimal-V1/backend/env.example)`
- **Deskripsi Perubahan & Fungsi**:
  - **`[backend/helpers/migration.help.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/helpers/migration.help.js)`** [NEW]:
    - Membuat helper baru `isV2Active()` yang memeriksa apakah environment variable `V2_ACTIVE === 'true'`.
    - Berfungsi sebagai sentral pengecekan status migrasi sistem keuangan untuk menghentikan operasional ganda antara backend v1 dan v2.
  - **`[backend/routes/customer_app.route.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/routes/customer_app.route.js)`**:
    - Memperbarui rute pembayaran pelanggan `GET /mypay/:invoice_id/:payment_code`.
    - Menambahkan mekanisme pengalihan (HTTP 302 Redirect) secara otomatis menuju halaman pembayaran Dekasimal v2 (`${v2Base}/p/invoice/${encodeURIComponent(payment_code)}`) apabila opsi `V2_PAYMENT_REDIRECT === 'true'` aktif.
    - Menyediakan penanganan fallback aman: jika `V2_PAYMENT_REDIRECT` aktif tetapi `V2_PAYMENT_BASE_URL` belum ditentukan, sistem mencatat log error dan tetap menggunakan alur pembayaran v1.
  - **`[backend/index.core.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/index.core.js)`**:
    - Menambahkan log informasi saat startup backend jika `V2_ACTIVE=true` untuk mempermudah pemantauan bahwa background job keuangan, penagihan, dan isolir versi 1 telah dinonaktifkan.
  - **`[backend/cron/1_minute.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_minute.cron.js)`**:
    - Mengondisikan pemanggilan `sendNotifPayment()` (notifikasi pembayaran via WhatsApp) dan `starCheckIpayment()` (polling status & pembukuan otomatis IPAYmu) agar tidak berjalan ketika `isV2Active()` bernilai `true`, menghindari duplikasi pembukuan dan notifikasi ganda ke pelanggan.
  - **`[backend/cron/1_day.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_day.cron.js)`**:
    - Menonaktifkan pembaharuan status faktur kadaluwarsa (`expired`), penghapusan temporary invoice & request IPAYmu lawas, pembuatan tagihan harian (`createInvoice`), pengingat WhatsApp tagihan (`createWAReminder`), dan isolir otomatis (`offAuthIfExp`) jika `isV2Active()` aktif. Seluruh proses ini dialihkan sepenuhnya ke manajemen background job sistem v2.
  - **`[backend/cron/1_month.cron.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/cron/1_month.cron.js)`**:
    - Menonaktifkan pembuatan faktur bulanan (`createInvoice`) dan pelaporan keuangan (`financeReport`) di v1 jika `isV2Active()` bernilai `true`.
  - **`[backend/env.example](file:///home/dhedhy/Project/Dekasimal-V1/backend/env.example)`**:
    - Menambahkan dokumentasi variabel lingkungan baru:
      - `V2_PAYMENT_REDIRECT`: Toggle aktivasi redirect tautan `/mypay/...` ke UI v2.
      - `V2_PAYMENT_BASE_URL`: URL basis publik frontend aplikasi v2 (contoh: `https://app.example.com`).
      - `V2_ACTIVE`: Flag sentral migrasi modul keuangan ke v2 guna menonaktifkan cron keuangan v1.

---

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**:
  - **Pengalihan Mulus Pelanggan ke Portal Baru**: Pelanggan yang mengakses tautan pembayaran tagihan invoice lama (format `/mypay/...`) akan secara otomatis diarahkan ke halaman pembayaran faktur modern di portal Dekasimal v2 (`/p/invoice/:payment_code`) tanpa perlu mengubah link yang telah terdistribusi sebelumnya.
  - **Manajemen Transisi Sistem yang Aman**: Tim engineer/administrator dapat mengaktifkan atau menonaktifkan pengalihan pembayaran serta mematikan proses cron v1 secara fleksibel melalui konfigurasi environment variable tanpa downtime atau perubahan kode.
- **Bug Fix / Solusi Masalah**:
  - **Mencegah Konflik & Duplikasi Pembukuan (Double Bookkeeping)**: Mengeliminasi potensi race condition atau eksekusi ganda antara backend v1 dan v2 pada pengecekan pembayaran IPAYmu, pembuatan tagihan otomatis, pengiriman notifikasi/reminder WhatsApp, dan proses isolir pelanggan.
- **Menu/Tombol Baru**:
  - Tidak ada menu UI baru di sisi v1; perubahan berfokus pada routing cerdas (HTTP redirect) dan orkestrasi transisi background services antara backend v1 dan v2.
