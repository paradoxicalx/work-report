# 📝 Daily Work Report - Dedy Putra (2026-07-04)

---

## 📌 Informasi Issue

- **Nomor Issue**: #365
- **Judul Issue**: Perbaikan Sistem Privilege Akses dan Penyesuaian Antarmuka Sidebar & Profil

## 📅 Laporan Harian - 4 Juli 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `[profile.ejs](file:///d:/Project/DEKASIMAL/frontend/views/admin/profile.ejs)`
  - **Deskripsi**: Melakukan penyesuaian logika validasi (kondisi `if`) dalam menampilkan data daftar slip gaji di profil. Akses khusus yang awalnya membiarkan pemilik profil (`user.admin_id == id`) dapat melihat slip tanpa izin khusus kini telah dicabut, sehingga akses membaca sepenuhnya bergantung pada konfigurasi hak akses `finance-salary/read` secara murni.

### 📅 Rincian Commit

#### [bf637d1] - resolve #365 (#365 - Perbaikan Sistem Privilege Akses dan Penyesuaian Antarmuka Sidebar & Profil)

- **Komponen yang Berubah**:
  - `[config.js](file:///d:/Project/DEKASIMAL/frontend/public/assets/js/config.js)`
  - `[config.min.js](file:///d:/Project/DEKASIMAL/frontend/public/assets/js/config.min.js)`
  - `[create.ejs](file:///d:/Project/DEKASIMAL/frontend/views/admin/privilege/create.ejs)`
  - `[edit.ejs](file:///d:/Project/DEKASIMAL/frontend/views/admin/privilege/edit.ejs)`
  - `[privilege.model.js](file:///d:/Project/DEKASIMAL/backend/models/privilege.model.js)`
- **Deskripsi Perubahan & Fungsi**:
  - **Auto-Increment ID**: Mengimplementasikan penomoran otomatis ID Privilege dengan `mongoose-auto-increment` dan menyematkannya ke bentuk *string* di `pre('save')`. Hal ini mencegah MongoDB menampilkan *Duplicate Key Error E11000* ketika menyimpan *privilege* baru dengan nilai `null`.
  - **Sinkronisasi Sidebar**: Memasukkan logika DOM manipulation pada JavaScript konfigurasi utama agar sejumlah submenu (seperti *customer/blacklist*, *network/ssh_config*, dll) disembunyikan sepenuhnya dari *sidebar* jika peran (role) yang sedang masuk tidak mempunyai *parent privilege* yang sesuai.
  - **Salin Hak Akses**: Menyematkan *Dropdown Select2* untuk menarik daftar nama tipe *privilege* dari server (*AJAX*). Ketika pengguna mengeklik tombol Salin, sistem secara *real-time* melakukan rekonstruksi *checkbox* di tampilan sehingga meniru persis konfigurasi *privilege* template sumber tersebut.
  - **Grup & Penghitungan Badge Dinamis**: Menambahkan fungsionalitas centang massal (*Select All*) per grup modul melalui form *switch*. Selain itu, di setiap baris *header* tersebut kini diberikan atribut *badge count* interaktif yang tidak hanya memperlihatkan progres tercentang (misal `2/4`), tapi juga berubah warna secara proaktif (abu-abu saat 0, biru saat sebagian, oranye/kuning saat penuh).

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)

- **Kemampuan Pengguna/Admin**: Superadmin (atau pengelola sistem) kini dapat menduplikasi (*copy-paste*) struktur hak akses puluhan fungsi dari satu jenis *role* ke jenis lainnya dalam sekejap tanpa kebingungan mengulangi proses centang manual satu per satu.
- **Bug Fix / Solusi Masalah**:
  - Menyelesaikan dan menghilangkan masalah kebocoran visual menu rahasia di mana sebelumnya admin terbatas masih bisa menekan tombol menu sensitif di *sidebar*.
  - Menyelesaikan galat kegagalan pembuatan peran baru di *backend* akibat konflik unik indeks di database NoSQL MongoDB.
- **Menu/Tombol Baru**: Penambahan *combo-box Dropdown Privilege*, fitur saklar/pemilih grup massal *Select All*, *Badge Count* dinamis pada antarmuka *Privilege*, serta Tombol "Salin Hak Akses" dengan pembaruan reaktif.
