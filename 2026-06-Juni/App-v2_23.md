# 📝 Daily Work Report - Dedy Putra (2026-06-23)

---

## 📌 Informasi Issue
- **Nomor Issue**: #116
- **Judul Issue**: Fiber Optic Management & OTDR Tracing

## 📅 Laporan Harian - 23 Juni 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `[backend/src/controllers/fiberCable.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberCable.controller.js)`
  - **Deskripsi**: Penyesuaian pada respons controller untuk mendukung i18n tanpa *default value* yang tidak sesuai standar arsitektur.
- `[backend/src/controllers/locationPoint.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/locationPoint.controller.js)`
  - **Deskripsi**: Penyesuaian untuk mengembalikan data lokasi poin dan *node* secara optimal bagi tampilan map.
- `[backend/src/models/fiberCable.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/fiberCable.model.js)`
  - **Deskripsi**: Memperbarui struktur *schema* kabel *fiber optic* untuk menyimpan referensi *splice* dan mendukung pencarian *core*.
- `[backend/src/routes/fiberCable.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/fiberCable.route.js)`
  - **Deskripsi**: Memperbarui dan menambahkan rute *endpoint* untuk penelusuran (*tracing*) kabel optik dan operasi node.
- `[backend/src/routes/locationPoint.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/locationPoint.route.js)`
  - **Deskripsi**: Menyesuaikan parameter *endpoint* pencarian lokasi *node*.
- `[backend/src/services/fiberCable.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberCable.service.js)`
  - **Deskripsi**: Menambah layanan logika bisnis untuk mendapatkan informasi kabel beserta detail *core* dan sambungan kabel (*splices*).
- `[backend/src/services/fiberTrace.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberTrace.service.js)`
  - **Deskripsi**: Membuat dan memperbaiki algoritma penelusuran (tracing) OTDR untuk menavigasi topologi jaringan *fiber* dari node awal secara proporsional, serta memisahkan jarak fisik rute dari asimilasi jarak gulungan (*slack*).
- `[backend/src/services/locationPoint.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/locationPoint.service.js)`
  - **Deskripsi**: Penyempurnaan pada logika perolehan *location points*.
- `[backend/src/locales/id/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/id/translation.json)` & `[backend/src/locales/en/translation.json](file:///d:/Project/DEKASIMAL_V2/backend/src/locales/en/translation.json)`
  - **Deskripsi**: Menambahkan string lokalisasi bahasa untuk pesan *error* dan *response tracing* dari sisi *backend*.
- `[frontend/src/app/pages/network/fiberCable/index.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/index.jsx)`
  - **Deskripsi**: Integrasi ID pada penampung (container) elemen utama modul *fiber* agar dapat mendukung fitur mode layar penuh (*fullscreen*).
- `[frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/FiberMap.jsx)`
  - **Deskripsi**: Render visual garis *polyline* rute hasil *tracing* di atas peta Leaflet menggunakan kalkulasi geometris Turf.js. Garis disesuaikan hingga titik putusnya.
- `[frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx)`
  - **Deskripsi**: Mengimplementasikan antarmuka interaktif pada *Sidebar* untuk tombol dan form pelacakan OTDR, sinkronisasi state *Node Terlewati*, perbaikan komponen tooltip yang terstandarisasi, penambahan fitur layar penuh (*fullscreen API*), serta integrasi *translation tool* (i18n).
- `[frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx)`
  - **Deskripsi**: Perbaikan visualisasi pemotongan dan pengelolaan sambungan kabel / *core*.
- `[frontend/src/features/fiberSlice.js](file:///d:/Project/DEKASIMAL_V2/frontend/src/features/fiberSlice.js)`
  - **Deskripsi**: Penambahan dan manajemen *state* pada Redux *Store* untuk mengatur aliran data *tracing*, jarak optik yang dimasukkan, dan koordinat *routing* antar komponen.
- `[frontend/src/i18n/locales/id/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)` & `[frontend/src/i18n/locales/en/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)`
  - **Deskripsi**: Menambahkan seluruh teks *User Interface* baru (tombol, *tooltip*, status node, error log) pada fitur manajemen *fiber optic* untuk dukungan bahasa Indonesia (ID) dan Bahasa Inggris (EN).
- `[AGENTS.md](file:///d:/Project/DEKASIMAL_V2/AGENTS.md)`
  - **Deskripsi**: Pembaruan aturan arsitektur bagi *AI Agent* dengan referensi dan batasan baru yang lebih terstruktur.

### 📅 Rincian Commit

*(Tidak ada riwayat commit baru hari ini; semua perubahan status pekerjaannya berada pada status Work In Progress (WIP) di working directory).*

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)
- **Kemampuan Pengguna/Admin**: Administrator jaringan kini dapat melakukan pelacakan akurat lokasi dan permasalahan kabel fisik (Tracing OTDR) melalui antarmuka peta interaktif yang juga mendukung fungsionalitas Layar Penuh (*Fullscreen*).
- **Bug Fix / Solusi Masalah**: Memperbaiki kejanggalan matematis di mana nilai gulungan (slack) pada node memengaruhi panjang rute visual (visual polyline) pada peta. Mengatasi masalah urutan arah trace yang sebelumnya terbalik dari node awal ke tujuan. Juga memperbaiki masalah terjemahan yang menggunakan *hardcoded defaults* sehingga seluruh i18n sekarang bekerja standar.
- **Menu/Tombol Baru**: Penambahan tab form **Tracing** pada sidebar, tombol fungsi layang (**Layar Penuh**) di bilah judul, *input field* dinamis **Node Terlewati**, serta tombol **Lacak OTDR**.

## 📖 Informasi & Tutorial Singkat Fitur
- **Penjelasan Fitur**: Fitur *Tracing OTDR* ini meniru pelacakan alat Optik asli. Fitur mendeteksi letak pasti kabel terputus di lapangan. Secara *backend*, fitur ini menggabungkan pencarian sambungan core antar node (lintas graf) dan manipulasi koordinat pemetaan (*turf.js*) untuk merepresentasikan seberapa jauh rute dapat dijelajahi dengan mengkalkulasi pengurangan parameter jarak masukkan (otdr distance) dengan jarak rute nyata dan *slack* cadangan.
- **Langkah Penggunaan (Tutorial)**:
  1. Akses modul **Manajemen Fiber Optik** via menu navigasi utama.
  2. Klik ikon maksimalkan layar (*Maximize*) di sebelah kanan judul untuk meluaskan bidang peta.
  3. Buka tab opsi **Tracing** pada panel instrumen (sidebar kanan).
  4. Pilih **Node Awal** yang diinginkan dan pilih **Arah Tracing (Kabel Tujuan)**.
  5. Masukkan parameter **Jarak OTDR** yang didapat dari indikator perangkat optik fisik.
  6. Masukkan asumsi nilai **Gulungan (m)** untuk sisa kabel tiap perlintasan *node* yang dilalui oleh core.
  7. Tekan tombol **Lacak OTDR**. Garis polyline pada peta akan menampilkan seberapa jauh *core* tersebut dapat dirunut sampai ke titik persis kabel terputus.
