# 📝 Daily Work Report - dedy (2026-06-20)

---

## 📌 Informasi Issue

- **Nomor Issue**: #363
- **Judul Issue**: Fitur Pelanggan Pasif

## 📅 Laporan Harian - 20 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

_(Tidak ada file yang sedang dikerjakan secara lokal, semua pekerjaan telah di-commit)_

### 📅 Rincian Commit

#### [65bd64b] - resolve #363 (#363 - Fitur Pelanggan Pasif)

- **Komponen yang Berubah**:
  - `[backend/models/customer.model.js](file:///d:/Project/DEKASIMAL/backend/models/customer.model.js)`
  - `[backend/routes/customer.route.js](file:///d:/Project/DEKASIMAL/backend/routes/customer.route.js)`
  - `[frontend/views/customer/pasif.ejs](file:///d:/Project/DEKASIMAL/frontend/views/customer/pasif.ejs)` [NEW]
  - `[frontend/views/customer/profile.ejs](file:///d:/Project/DEKASIMAL/frontend/views/customer/profile.ejs)`
  - `[frontend/views/customer/update.ejs](file:///d:/Project/DEKASIMAL/frontend/views/customer/update.ejs)`
  - `[frontend/views/include/sidebar.ejs](file:///d:/Project/DEKASIMAL/frontend/views/include/sidebar.ejs)`
- **Deskripsi Perubahan & Fungsi**:
  - **backend/models/customer.model.js**: Menambahkan properti field `pasif` (berupa String) pada schema model _Customer_ untuk mengakomodasi penyimpanan data alasan/catatan pelanggan pasif.
  - **backend/routes/customer.route.js**: Membuat endpoint API backend baru (`/update/pasif`, `/update/unpasif`, `/read/table-pasif`) yang dirancang untuk dapat mendeteksi masukan parameter berupa `_id` maupun `customer_id`. Selain itu, menambahkan instruksi filter pada route pembacaan Datatables pelanggan utama agar tidak lagi menampilkan daftar pelanggan pasif.
  - **frontend/views/customer/pasif.ejs**: Menerapkan pembuatan berkas antarmuka pengguna halaman web baru secara terpisah untuk memuat kerangka Datatables yang khusus me-list/menampilkan daftar pelanggan berstatus pasif.
  - **frontend/views/customer/update.ejs**: Mengintegrasikan blok elemen visual UI dan logika interaksi fungsi Javascript (berbasis _popup_ SweetAlert2) bagi level admin untuk dapat mengeksekusi penandaan sebuah akun menjadi pasif, atau sebaliknya membatalkannya.
  - **frontend/views/customer/profile.ejs**: Menyesuaikan tampilan detil profil pelanggan dengan mengembalikan elemen _alert_ informasi (tanpa tombol aksi) yang otomatis terpicu/muncul untuk mengabarkan bahwa pelanggan bersangkutan kini sedang berstatus pasif.
  - **frontend/views/include/sidebar.ejs**: Mengikutsertakan sebuah entri menu navigasi _sidebar_ yang baru agar admin bisa menavigasikan diri menuju halaman daftar Pelanggan Pasif.

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**: Admin sistem kini memiliki kemampuan dan kontrol penuh untuk memanipulasi status kelangsungan akun pelanggan ke dalam posisi "Pasif" dengan menyertakan alasannya secara deskriptif melalui layar form _Edit Pelanggan_ (update.ejs). Para admin terkait juga dapat senantiasa meninjau dan melacak siapa saja yang berada pada status pasif tersebut melalui kehadiran menu rute khusus bernama "Pelanggan Pasif".
- **Bug Fix / Solusi Masalah**: Telah diselesaikan isu kompatibilitas tipe format _ID_ (String vs ObjectId) saat melakukan penyimpanan backend yang menyebabkan _error crash_, dengan merevisi route _update_ untuk selalu menerima kedua format tersebut secara dinamis tanpa mengganggu dependensi yang lain.
- **Menu/Tombol Baru**: Penambahan interaksi aksi berupa tombol "Tandai Pasif" dan "Batalkan Status Pasif" pada halaman pembaharuan Edit Profil, ditambah pula dukungan navigasi menu permanen berjudul "Pelanggan Pasif" yang bertengger pada menu navigasi sebelah kiri (Sidebar).
