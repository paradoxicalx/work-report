# 📝 Daily Work Report - Dedy Putra (2026-07-09)

---

## 📌 Informasi Issue
- **Nomor Issue**: #129
- **Judul Issue**: Optimasi Manajemen Kabel Fiber (Fiber Cable Management)

## 📅 Laporan Harian - 9 Juli 2026

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `[CoreTopologyCustomEdge.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/CoreTopologyCustomEdge.jsx)`
  - **Deskripsi**: Penyesuaian format penulisan (formatting/Prettier) pada tampilan tooltip kapasitas core dan panjang kabel.
- `[CoreTopologyCustomNode.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/CoreTopologyCustomNode.jsx)`
  - **Deskripsi**: Penyesuaian format penulisan (formatting/Prettier) dan render styling tabel sambungan *splice* untuk merapikan pembacaan kode.

### 📅 Rincian Commit

#### [e04eee9] - save #129 (#129 - Optimasi Manajemen Kabel Fiber)

- **Komponen yang Berubah**:
  - `[NodeEquipment.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/NodeEquipment.jsx)`
  - `[NodeInfoDrawer.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx)`
  - `[SpliceTray.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/SpliceTray.jsx)`
- **Deskripsi Perubahan & Fungsi**:
  - Memperbaiki interaksi antar tab pada antarmuka detail node jaringan.
  - Menyempurnakan logika efek visual (*highlight/shining*) pada port perangkat saat diklik dari tautan pada *core*, serta menghilangkan garis koneksi tambahan (*arrow*) yang merender secara acak dan mengganggu visibilitas.
  - Mengubah penanganan pesan *error* "Core element not found" (saat transisi tab terlalu cepat) dari peringatan *toast* di UI menjadi sekadar informasi di *console* (console.warn).
  - Melakukan penyempurnaan `dependency array` (resolusi *linting*) pada hook *useEffect*.

#### [2385b2a] - save #129 (#129 - Optimasi Manajemen Kabel Fiber)

- **Komponen yang Berubah**:
  - `[fiberCable.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/fiberCable.controller.js)`
  - `[locationPoint.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/locationPoint.model.js)`
  - `[fiberCable.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/fiberCable.service.js)`
  - `[locationPoint.service.js](file:///d:/Project/DEKASIMAL_V2/backend/src/services/locationPoint.service.js)`
  - `[DropCoreModal.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/DropCoreModal.jsx)` [NEW]
  - `[SidebarTools.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/SidebarTools.jsx)`
  - `[translations.json (en)](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)`
  - `[translations.json (id)](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)`
- **Deskripsi Perubahan & Fungsi**:
  - Penambahan struktur model database dan penyesuaian layanan *backend* (API) untuk sinkronisasi operasi pemotongan kabel dan manajemen port/splice (*Drop Core*).
  - Pembaruan bahasa dan lokalisasi antarmuka untuk mencakup terminologi perangkat keras fiber optik baru.
  - Implementasi komponen UI React pertama untuk menangani antarmuka drop core.

#### [ad03f92] - save #129 (#129 - Optimasi Manajemen Kabel Fiber)

- **Komponen yang Berubah**:
  - `[fiberCable.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/fiberCable.model.js)`
  - `[locationPoint.model.js](file:///d:/Project/DEKASIMAL_V2/backend/src/models/locationPoint.model.js)`
  - `[DropCoreModal.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/DropCoreModal.jsx)`
  - `[NodeEquipment.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/NodeEquipment.jsx)` [NEW]
  - `[NodeInfoDrawer.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/network/fiberCable/components/NodeInfoDrawer.jsx)`
- **Deskripsi Perubahan & Fungsi**:
  - Modifikasi tahap awal (*bootstraping*) untuk skema struktur data *Equipment* di dalam Node, menyimpan referensi port perangkat yang aktif.
  - Mempersiapkan tata letak modular (layouting awal) untuk komponen tab *Node Equipment* pada layar *Drawer* informasi.

## 📢 Dampak Perubahan & Fungsionalitas Baru (User Capabilities & Bug Fixes)
- **Kemampuan Pengguna/Admin**: Admin jaringan kini memiliki akses ke tab **Equipment** di informasi node yang menyediakan daftar detail perangkat pasif/aktif (seperti ODC dan ODP). Proses *tracing* koneksi antar port perangkat dengan core optik menjadi lebih mulus tanpa perlu pindah tab secara manual mencari-cari core.
- **Bug Fix / Solusi Masalah**: Terselesaikannya *glitch* garis visual `Xarrow` yang tidak diizinkan tergambar acak saat membuka Equipment. Hilangnya pesan *toast* error peringatan (berulang-ulang) yang sebelumnya mengganggu navigasi perpindahan layar.
- **Menu/Tombol Baru**: Penambahan fitur klik-interaktif. Port pada daftar perangkat di Equipment dapat diklik sehingga layar otomatis mengarah dan menyalakan indikator lampu sorot (animasi *shining/highlight*) di *core* warna yang terkait di dalam kotak *Splice Tray*.

## 📖 Informasi & Tutorial Singkat Fitur
- **Penjelasan Fitur**: Fitur *Jump-To-Core* pada Node Information Drawer menghubungkan relasi fisik (Port Equipment) ke relasi logikal penyambungan kabel (*Splice Tray*). Relasi ini digambar secara mandiri dan interaktif demi menyederhanakan *troubleshooting* kabel jaringan.
- **Langkah Penggunaan (Tutorial)**:
  1. Klik sebuah titik Node (Tiang/Lokasi) yang memiliki peralatan fiber optik di tampilan Peta (Map).
  2. Buka "Lihat Detail Node" untuk membuka *Drawer* di layar kanan Anda.
  3. Buka tab **Perangkat (Equipment)**.
  4. Gulir atau temukan perangkat, lalu jika ada port yang sudah diidentifikasi tersambung dengan core tertentu, klik nama relasi/port tersebut.
  5. Sistem akan otomatis memindahkan Anda ke tab **Splice Tray** dan menyoroti kotak warna kabel/core yang terhubung.
