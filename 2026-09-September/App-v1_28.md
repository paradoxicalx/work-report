# 📝 Daily Work Report - Dedy (2026-09-28)

---

## 📌 Informasi Issue

- **Nomor Issue**: #373
- **Judul Issue**: Optimasi Pembayaran Invoice dengan Saldo Dompet & Penambahan Informasi Admin Pembuat Tagihan

## 📅 Laporan Harian - 28 September 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

_Tidak ada pekerjaan yang belum di-commit (working tree bersih). Seluruh perubahan kode telah di-commit ke repositori dan di-merge ke branch utama (`main`)._

### 📅 Rincian Commit

#### [65132b6](https://github.com/paradoxicalx/ISPF_V1/commit/65132b6) & [92d141b](https://github.com/paradoxicalx/ISPF_V1/commit/92d141b) - resolve #373 (#373 - Optimasi Pembayaran Invoice dengan Saldo Dompet & Penambahan Informasi Admin Pembuat Tagihan)

- **Komponen yang Berubah**:
  - [backend/routes/finance_invoice.route.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/routes/finance_invoice.route.js)
  - [backend/routes/ipay.route.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/routes/ipay.route.js)
  - [frontend/views/finance/invoice/detail.ejs](file:///home/dhedhy/Project/Dekasimal-V1/frontend/views/finance/invoice/detail.ejs)
  - [frontend/views/finance/invoice/payment.ejs](file:///home/dhedhy/Project/Dekasimal-V1/frontend/views/finance/invoice/payment.ejs)
  - [frontend/views/finance/ipay/index.ejs](file:///home/dhedhy/Project/Dekasimal-V1/frontend/views/finance/ipay/index.ejs)
- **Deskripsi Perubahan & Fungsi**:
  - **[backend/routes/finance_invoice.route.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/routes/finance_invoice.route.js)**:
    - Memperbarui query `.populate()` pada endpoint `POST /finance-invoice/read/:id` untuk menyertakan referensi relasi `created_by` (Model `Admin`) beserta field `admin_id`, `name`, `email`, dan `phone`.
    - Memungkinkan frontend mengakses data pembuat tagihan asli untuk ditampilkan pada rincian invoice.
  - **[frontend/views/finance/invoice/detail.ejs](file:///home/dhedhy/Project/Dekasimal-V1/frontend/views/finance/invoice/detail.ejs)**:
    - Menambahkan blok informasi pembuat tagihan (`#inv-created-by-alert`) yang terletak tepat di bawah card utama invoice.
    - Menampilkan nama admin beserta tautan langsung ke halaman profil admin (`/admin/profile/${admin_id}`).
    - Menyediakan fallback teks `"Sistem"` apabila tagihan dibuat secara otomatis oleh sistem (cronjob/scheduler berkala).
    - Memastikan elemen pembuat tagihan ini dilengkapi class `hide-on-print` sehingga tidak akan ikut tercetak saat lembar invoice dicetak/di-print ke kertas atau PDF.
  - **[frontend/views/finance/invoice/payment.ejs](file:///home/dhedhy/Project/Dekasimal-V1/frontend/views/finance/invoice/payment.ejs)**:
    - Memperbaiki logika deteksi saldo dompet (`wallet`) milik pelanggan atau mitra: menghapus pengondisian `itm.customer.wallet >= itm.total ? itm.customer.wallet : 0` yang sebelumnya membuat saldo pelanggan seolah 0 jika nilainya di bawah total tagihan.
    - Menampilkan informasi saldo dompet secara transparan:
      - Jika saldo mencukupi (`wallet >= itm.total`): Checkbox pembayaran dengan saldo dompet aktif dan dapat dicentang oleh petugas/admin.
      - Jika saldo tidak mencukupi (`wallet < itm.total`): Checkbox dibuat nonaktif (`disabled`) disertai indikator badge peringatan `Saldo kurang`.
    - Menambahkan pembersihan status pilihan dompet (`delete payData.usewallet`) pada proses iterasi pembayaran batch/massal ketika checkbox tidak tercentang, guna mencegah status dompet bocor ke invoice berikutnya.
  - **[backend/routes/ipay.route.js](file:///home/dhedhy/Project/Dekasimal-V1/backend/routes/ipay.route.js)**:
    - Menambahkan data `total: invoice.total` pada payload `invoice` saat query log riwayat transaksi IPAYmu dipanggil.
  - **[frontend/views/finance/ipay/index.ejs](file:///home/dhedhy/Project/Dekasimal-V1/frontend/views/finance/ipay/index.ejs)**:
    - Merapikan struktur pemanggilan API pembacaan saldo IPAYmu dan modal riwayat transaksi log.
    - Menambahkan fallback nilai kolom harga (`SubTotal`) ke `b.invoice.total` jika data subtotal kosong pada DataTable IPAYmu.

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**:
  - Admin kini dapat melihat dengan jelas siapa pembuat faktur tagihan (nama admin pembuat dengan tautan ke profilnya, atau "Sistem" jika otomatis) pada halaman detail invoice tanpa khawatir informasi internal tersebut ikut tercetak saat faktur diserahkan ke pelanggan.
  - Petugas kasir/keuangan dapat melihat informasi saldo dompet pelanggan/mitra secara akurat di tabel pembayaran invoice, serta mengetahui secara langsung apakah saldo mencukupi atau kurang untuk melunasi tagihan yang dipilih.
- **Bug Fix / Solusi Masalah**:
  - **Bug Hilangnya Opsi Saldo Dompet di Halaman Pembayaran**: Memperbaiki bug di mana opsi dompet tidak muncul sama sekali ketika saldo dompet pelanggan lebih kecil dari total tagihan invoice.
  - **Mencegah Kebocoran State Pembayaran Batch**: Mencegah penggunaan flag `usewallet` secara tidak sengaja pada faktur berikutnya saat proses batch payment berlangsung.
  - **Informasi Admin Pembuat Tagihan**: Menyelesaikan kebutuhan pelacakan pembuat tagihan pada halaman detail invoice dengan tetap menjaga kerapian cetak dokumen resmi (tidak ikut tercetak).
- **Menu/Tombol Baru**:
  - Tautan nama admin pembuat tagihan pada halaman detail invoice (`/finance/invoice/detail/{id}`) yang dapat diklik langsung untuk menuju ke profil admin terkait (`/admin/profile/{admin_id}`).
