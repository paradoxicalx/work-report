# Tutorial Lengkap — Manajemen Kabel Fiber Optik

**ISPF V2 — Modul GIS Fiber Optic Management**

---

## Daftar Isi

1. [Tujuan Fitur](#tujuan-fitur)
2. [Aturan Penggunaan](#aturan-penggunaan)
3. [Prasyarat](#prasyarat)
4. [Sesi 1: Membuka Halaman Fiber Optik dan Memahami Antarmuka](#sesi-1-membuka-halaman-fiber-optik-dan-memahami-antarmuka)
5. [Sesi 2: Menambahkan Node Baru](#sesi-2-menambahkan-node-baru)
6. [Sesi 3: Mengedit Node](#sesi-3-mengedit-node)
7. [Sesi 4: Menghapus Node](#sesi-4-menghapus-node)
8. [Sesi 5: Menambahkan Rute Kabel Secara Manual](#sesi-5-menambahkan-rute-kabel-secara-manual)
9. [Sesi 6: Menambahkan Rute Kabel via Import KML](#sesi-6-menambahkan-rute-kabel-via-import-kml)
10. [Sesi 7: Mengedit Rute Kabel](#sesi-7-mengedit-rute-kabel)
11. [Sesi 8: Memotong Kabel (Split) — Menyisipkan Node Baru](#sesi-8-memotong-kabel-split--menyisipkan-node-baru)
12. [Sesi 9: Memotong Kabel (Split) — Menggunakan Node yang Sudah Ada](#sesi-9-memotong-kabel-split--menggunakan-node-yang-sudah-ada)
13. [Sesi 10: Menyambung Kembali Kabel yang Terpotong (Merge)](#sesi-10-menyambung-kembali-kabel-yang-terpotong-merge)
14. [Sesi 11: Menghapus Kabel](#sesi-11-menghapus-kabel)
15. [Sesi 12: Menyambungkan Core (Splicing)](#sesi-12-menyambungkan-core-splicing)
16. [Sesi 13: Memutus Sambungan Core (Unsplicing)](#sesi-13-memutus-sambungan-core-unsplicing)
17. [Sesi 14: Melihat dan Menambah Catatan pada Kabel dan Core](#sesi-14-melihat-dan-menambah-catatan-pada-kabel-dan-core)
18. [Sesi 15: Tracing Core (Pelacakan Jalur Sambungan)](#sesi-15-tracing-core-pelacakan-jalur-sambungan)
19. [Sesi 16: Tracing OTDR (Simulasi Deteksi Putus)](#sesi-16-tracing-otdr-simulasi-deteksi-putus)
20. [Sesi 17: Melihat dan Mencetak Diagram Topologi Core](#sesi-17-melihat-dan-mencetak-diagram-topologi-core)
21. [Sesi 18: Menggunakan Filter Node dan Kapasitas Kabel](#sesi-18-menggunakan-filter-node-dan-kapasitas-kabel)
22. [Sesi 19: Mode Layar Penuh](#sesi-19-mode-layar-penuh)
23. [Sesi 20: Memahami Sistem Clustering dan Zoom](#sesi-20-memahami-sistem-clustering-dan-zoom)
24. [Sesi 21: Memahami Perhitungan Redaman Optik (Optical Budget)](#sesi-21-memahami-perhitungan-redaman-optik-optical-budget)
25. [Sesi 22: Mengelola Konflik Data (Optimistic Locking)](#sesi-22-mengelola-konflik-data-optimistic-locking)
26. [Pintasan Keyboard](#pintasan-keyboard)
27. [Struktur Data Referensi](#struktur-data-referensi)

---

## Tujuan Fitur

Manajemen Kabel Fiber Optik adalah modul GIS (_Geographic Information System_) yang dirancang untuk:

1. **Pemetaan Jaringan Fiber Optik** — Memvisualisasikan rute kabel fiber optik di atas peta satelit secara interaktif, lengkap dengan titik-titik node/POP yang menghubungkannya.

2. **Manajemen Inventaris Kabel** — Mencatat properti fisik setiap kabel: kapasitas core, panjang rute, panjang _slack_ (gulungan cadangan), warna tampilan peta, jenis garis, dan catatan operasional.

3. **Manajemen Sambungan Core** — Melacak koneksi logis antar core kabel fiber optik pada setiap node menggunakan standar warna IEC 60304, membantu teknisi memahami alur sinyal optik dari ujung ke ujung.

4. **Auto-Routing & Koreksi Rute** — Menghasilkan rute kabel secara otomatis mengikuti jalan raya nyata (via OSRM), dengan koreksi manual menggunakan _waypoint_ yang dapat digeser.

5. **Import Data Pihak Ketiga** — Mendukung impor file KML (Google Earth) untuk membuat rute kabel dari data survei kontraktor.

6. **Simulasi OTDR** — Memperkirakan titik putus kabel berdasarkan jarak yang terukur dari perangkat OTDR (Optical Time Domain Reflectometer).

7. **Diagram Topologi Core** — Menampilkan diagram alur koneksi core antar node yang dapat dicetak untuk dokumentasi lapangan.

8. **Pencegahan Bentrok Data** — Menerapkan _Optimistic Locking_ untuk mencegah dua admin mengedit data sambungan yang sama secara bersamaan.

---

## Aturan Penggunaan

### Aturan Umum

| No  | Aturan                             | Penjelasan                                                                                                                       |
| --- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Node wajib ada lebih dulu          | Kabel hanya dapat dibuat jika sudah ada minimal 2 node (POP, ODP, ODC, Joint Closure, dll).                                      |
| 2   | Minimal 2 waypoint per rute        | Setiap rute kabel membutuhkan minimal 2 titik koordinat (waypoint).                                                              |
| 3   | Nama kabel wajib diisi             | Setiap kabel harus memiliki nama unik untuk identifikasi.                                                                        |
| 4   | Core capacity sesuai spesifikasi   | Kapasitas core hanya dapat dipilih dari opsi standar: 1, 2, 4, 6, 8, 12, 24, 48, 96, 144, 288.                                   |
| 5   | Pemotongan butuh node              | Memotong kabel (split) memerlukan pemilihan node yang akan menjadi titik potong (misalnya Joint Closure).                        |
| 6   | Penyambungan butuh 2 kabel berbeda | Menyambung core (splice) hanya bisa dilakukan dari core kabel A ke core kabel B — tidak bisa menyambung core ke dirinya sendiri. |
| 7   | Tidak bisa hapus node berkabel     | Node yang masih terhubung dengan kabel tidak dapat dihapus. Putuskan/hapus kabel terlebih dahulu.                                |
| 8   | Merge butuh node bersama           | Dua kabel hanya dapat digabung (merge) jika keduanya terhubung di node yang sama.                                                |
| 9   | Merge tidak bisa jika ada splice   | Kabel yang sudah memiliki sambungan core tidak dapat digabung. Putuskan splice terlebih dahulu.                                  |
| 10  | Optimistic Locking aktif           | Jika dua admin mengedit splice bersamaan, admin kedua akan mendapat error dan harus memuat ulang.                                |

### Aturan Auto-Routing

| No  | Aturan                            | Penjelasan                                                                                            |
| --- | --------------------------------- | ----------------------------------------------------------------------------------------------------- |
| 1   | Auto-routing mengikuti jalan      | Mode `auto` memanggil API OSRM untuk menghasilkan rute mengikuti jalan raya terdekat.                 |
| 2   | Mode `straight` untuk garis lurus | Mode `straight` menghasilkan garis lurus antar waypoint — berguna untuk kabel udara atau rute khusus. |
| 3   | Kombinasi auto + straight         | Kamu bisa mencampur mode auto dan straight dalam satu rute kabel (setiap segmen independen).          |
| 4   | Toggle Shift untuk inversi        | Menahan tombol `Shift` saat toggle auto-routing akan membalik logika toggle (temporary override).     |

### Aturan Hak Akses

| Privilege            | Dibutuhkan Untuk                               |
| -------------------- | ---------------------------------------------- |
| `networkSite.read`   | Melihat peta dan data kabel                    |
| `networkSite.create` | Menambah node baru, kabel baru, splice baru    |
| `networkSite.update` | Mengedit node, kabel, splice, dan catatan core |
| `networkSite.delete` | Menghapus node dan kabel                       |

---

## Prasyarat

Sebelum memulai, pastikan:

1. Anda telah **login** ke sistem ISPF V2.
2. Anda memiliki hak akses `networkSite` (minimal `.read`).
3. Untuk aksi penambahan/pengeditan, Anda memerlukan hak akses `.create`, `.update`, atau `.delete`.
4. Halaman diakses melalui menu navigasi: **Node/Sites → Fiber Optic Management**.

---

## Sesi 1: Membuka Halaman Fiber Optik dan Memahami Antarmuka

### 1.1 Membuka Halaman

1. Dari sidebar kiri, klik menu **"Node/Sites"**.
2. Pilih sub-menu **"Fiber Optic Management"**.
3. Halaman akan memuat peta interaktif dengan dua panel:
   - **Panel kiri (utama):** Peta GIS lengkap dengan marker node dan polyline kabel.
   - **Panel kanan (sidebar):** Panel alat (_SidebarTools_) untuk semua tindakan.

### 1.2 Memahami Antarmuka Peta

| Elemen                     | Deskripsi                                                                                |
| -------------------------- | ---------------------------------------------------------------------------------------- |
| **Marker Node**            | Lingkaran berwarna dengan ikon yang mewakili POP, ODP, ODC, Joint Closure, dll.          |
| **Garis Kabel (Polyline)** | Garis berwarna mewakili rute kabel fiber optik. Garis putus-putus = kabel tipe `dashed`. |
| **Cluster Marker**         | Lingkaran biru berangka — menggabungkan beberapa node yang berdekatan pada zoom rendah.  |
| **Layer Peta**             | Pojok kanan atas peta — dapat diganti ke berbagai layer (default: Google Satellite).     |
| **Zoom Control**           | Pojok kiri atas peta — tombol `+` dan `-`.                                               |

### 1.3 Memahami Panel Sidebar

Panel sidebar memiliki dua tab:

- **Tab "Kabel"** — Untuk menambah, mengedit, memotong, menyambung, dan menghapus kabel. Juga berisi filter node dan kapasitas kabel.
- **Tab "Tracing"** — Untuk melakukan tracing core dan simulasi OTDR.

Di bawah panel terdapat **tip kontekstual** yang berubah sesuai mode aktif.

### 1.4 Interaksi Dasar di Peta

- **Klik node:** Membuka kartu informasi melayang (_floating card_) dengan opsi lanjutan (tambah kabel, edit node, detail node).
- **Klik kabel:** Membuka kartu informasi kabel dengan opsi potong, edit, tracing core.
- **Klik area kosong:** Membuka kartu lokasi dengan opsi buat node baru di titik tersebut atau mulai tambah kabel.

---

## Sesi 2: Menambahkan Node Baru

Node adalah titik referensi geografis tempat kabel fiber optik dimulai, berakhir, atau melewati sambungan.

### 2.1 Menambahkan Node via Peta

1. **Klik kanan** (atau klik biasa di mode normal) pada area kosong di peta.
2. Kartu lokasi melayang akan muncul menampilkan koordinat (Lat/Lng).
3. Klik tombol **"Node Baru"**.
4. Formulir pembuatan node akan terbuka di dalam drawer.
5. Isi data node:
   - **Nama:** Identifikasi node (contoh: `POP-Ciganitri-01`).
   - **Tipe:** Pilih tipe node (POP, ODP, ODC, Joint Closure, dll).
   - **Grup:** Kategori grup node.
   - **Kapasitas & Tipe Kapasitas:** Spesifikasi node.
   - **Radius:** Radius jangkauan node dalam meter.
   - **Koordinat:** Otomatis terisi dari titik yang diklik — dapat disesuaikan.
   - **PIC:** Nama dan kontak penanggung jawab.
   - **Catatan:** Informasi tambahan.
6. Klik **"Simpan"**.
7. Node baru akan muncul di peta. Data node otomatis diperbarui.

### 2.2 Menambahkan Node via Sidebar

1. Pada sidebar tab **"Kabel"**, klik tombol **"Node Baru"** (ikon +).
2. Formulir pembuatan node akan terbuka tanpa koordinat awal.
3. Isi data node secara manual termasuk koordinatnya.
4. Klik **"Simpan"**.
5. Node baru akan muncul di peta setelah data diperbarui.

> **Tips:** Menambahkan node via peta lebih mudah karena koordinat otomatis terisi dari titik yang Anda klik.

---

## Sesi 3: Mengedit Node

1. **Klik marker node** di peta.
2. Kartu informasi node melayang akan muncul.
3. Klik tombol **"Edit Node"**.
4. Formulir edit node akan terbuka di dalam drawer.
5. Ubah data yang diperlukan (nama, tipe, grup, kapasitas, koordinat, PIC, catatan, dll).
6. Klik **"Simpan"**.
7. Data node diperbarui dan marker di peta ikut berubah.

> **Catatan:** Ikon dan warna marker node ditentukan oleh tipe dan warna yang dipilih.

---

## Sesi 4: Menghapus Node

### 4.1 Syarat Penghapusan

Node hanya dapat dihapus jika **tidak ada kabel** yang terhubung ke node tersebut. Jika masih ada kabel, tombol hapus akan dinonaktifkan dengan pesan informasi.

### 4.2 Langkah Menghapus Node

1. **Klik marker node** di peta.
2. Kartu informasi node melayang akan muncul.
3. Klik tombol **"Detail Node"** untuk membuka drawer informasi node.
4. Di bagian bawah drawer, klik tombol **"Hapus"** (ikon tong sampah, berwarna merah).
5. Modal konfirmasi akan muncul.
6. Klik **"Konfirmasi"** untuk melanjutkan penghapusan.
7. Node akan dihapus dari peta dan database setelah berhasil.

> **Peringatan:** Penghapusan node bersifat permanen dan tidak dapat dibatalkan.

---

## Sesi 5: Menambahkan Rute Kabel Secara Manual

### 5.1 Cara 1: Memulai dari Node yang Sudah Ada

1. **Klik marker node** di peta (node awal).
2. Kartu informasi node melayang muncul.
3. Klik tombol **"Mulai Tambah Kabel"**.
4. Sidebar akan beralih ke mode **"Tambah Kabel"**.
5. Waypoint pertama otomatis ditambahkan di lokasi node tersebut.
6. **Klik node tujuan** (node kedua) di peta.
7. Kartu informasi node tujuan muncul.
8. Klik tombol **"Tetapkan Node Tujuan"**.
9. Waypoint kedua ditambahkan, dan rute dihitung secara otomatis.
10. Kembali ke sidebar untuk melengkapi data kabel.

### 5.2 Cara 2: Memulai dari Area Kosong

1. **Klik area kosong** di peta.
2. Kartu lokasi melayang muncul.
3. Klik tombol **"Mulai Tambah Kabel"**.
4. Waypoint pertama ditambahkan di titik tersebut.
5. Lanjutkan menambah waypoint dengan **mengklik peta** di titik-titik selanjutnya.
6. Setiap klik menambah waypoint baru dan menghitung ulang rute.

### 5.3 Menambah Waypoint Tambahan

Selama mode tambah/edit kabel, kamu bisa menambah waypoint dengan dua cara:

- **Klik titik tengah (midpoint):** Setiap segmen kabel memiliki marker kecil putih di tengahnya. Klik marker tersebut untuk menyisipkan waypoint baru di posisi tengah segmen.
- **Geser titik tengah (midpoint):** Drag marker tengah ke posisi baru — waypoint baru akan otomatis dibuat di posisi akhir drag.

### 5.4 Menggeser Waypoint

Setiap waypoint (marker merah) dapat **di-drag** (ditarik) ke posisi baru. Rute akan otomatis dihitung ulang setelah 500ms (_debounce_).

### 5.5 Mode Auto-Routing vs Garis Lurus

Di sidebar, terdapat **toggle Auto-Routing**:

- **ON (default):** Setiap segmen waypoint baru akan dihitung menggunakan OSRM untuk mengikuti jalan raya. Rute mengikuti jalan nyata (_snap to road_).
- **OFF:** Setiap segmen waypoint baru akan menggunakan garis lurus (rute udara/straight line).

> **Toggle dengan Shift:** Tahan tombol `Shift` sambil mengklik toggle untuk membalik sementara logika auto-routing. Misalnya, jika auto-routing ON, tahan Shift + toggle OFF akan mengaktifkan mode straight hanya untuk satu segmen.

Setiap waypoint memiliki properti `routing` (auto atau straight). Segmen yang berbeda bisa menggunakan mode berbeda.

### 5.6 Menghapus Waypoint

1. Klik marker waypoint (merah).
2. Popup akan muncul dengan label waypoint.
3. Klik tombol **tong sampah** di dalam popup.
4. Waypoint akan dihapus dan rute dihitung ulang.

### 5.7 Membersihkan Semua Waypoint (Clear Draft)

Di sidebar, klik tombol **"Clear Draft"** (berwarna oranye) untuk menghapus semua waypoint dan memulai ulang.

### 5.8 Mengisi Data Kabel dan Menyimpan

Setelah waypoint selesai ditentukan, isi data kabel di sidebar:

| Field               | Deskripsi                                                       |
| ------------------- | --------------------------------------------------------------- |
| **Nama Kabel**      | Identifikasi kabel (wajib). Contoh: `FO-Ciganitri-Pasirjati-01` |
| **Node**            | Pilih node awal dan akhir. Gunakan Combobox multi-select.       |
| **Kapasitas Core**  | Pilih dari dropdown: 1, 2, 4, 6, 8, 12, 24, 48, 96, 144, 288    |
| **Warna Rute**      | Warna garis kabel di peta (color picker).                       |
| **Jenis Garis**     | Solid (garis penuh) atau Dashed (garis putus-putus).            |
| **Ketebalan Garis** | 1–10 (semakin besar semakin tebal).                             |
| **Catatan**         | Informasi tambahan (opsional).                                  |

Klik tombol **"Simpan"** (berwarna hijau). Kabel baru akan muncul di peta setelah berhasil disimpan.

### 5.9 Membatalkan Mode Tambah

Klik tombol **"Batal Tambah"** (berwarna merah) di sidebar untuk keluar dari mode tambah kabel. Semua waypoint akan dihapus.

---

## Sesi 6: Menambahkan Rute Kabel via Import KML

Jika kamu memiliki file KML dari Google Earth (hasil survei, kontraktor, dll.), kamu bisa mengimpornya langsung.

### 6.1 Format KML yang Didukung

- File `.kml` Google Earth.
- Sistem akan mengekstrak elemen `LineString` atau `MultiLineString` pertama yang ditemukan.
- Minimal 2 titik koordinat dalam satu LineString.

### 6.2 Langkah Import KML

1. Pastikan sidebar dalam mode **normal** (bukan mode tambah/edit/split).
2. Pada sidebar tab **"Kabel"**, gulir ke bagian **"Import KML"**.
3. Di area upload FilePond, klik atau drag file `.kml` ke dalamnya.
4. Sistem akan otomatis:
   - Memproses file KML.
   - Mengekstrak koordinat rute.
   - Membuat waypoint dari koordinat tersebut.
   - Menggeser tampilan peta ke area rute.
5. Sidebar beralih ke mode **"Tambah Kabel"** dengan waypoint dari KML.
6. Lengkapi data kabel (nama, kapasitas core, dll.) seperti biasa.
7. Klik **"Simpan"**.

### 6.3 Validasi Error

Jika file KML tidak valid:

- Pesan error **"Hanya file .kml yang diizinkan"** — file bukan KML.
- Pesan error **"Tidak ada rute valid di dalam file KML"** — tidak ditemukan elemen LineString.
- Pesan error **"Gagal membaca file KML"** — format file tidak dapat dibaca.

---

## Sesi 7: Mengedit Rute Kabel

### 7.1 Membuka Mode Edit

1. **Klik garis kabel** di peta yang ingin diedit.
2. Kartu informasi kabel melayang muncul.
3. Klik tombol **"Edit Kabel"** (berwarna biru).
4. Sidebar beralih ke mode **"Edit Kabel"**:
   - Formulir terisi data kabel yang sedang diedit.
   - Waypoint ditampilkan di peta.
   - Mode drag waypoint aktif kembali.

### 7.2 Mengedit Rute

Selama mode edit, kamu dapat:

- **Menambah waypoint baru** — klik peta atau drag midpoint.
- **Menggeser waypoint** — drag marker merah.
- **Menghapus waypoint** — buka popup waypoint dan klik tong sampah.

### 7.3 Mengedit Properti Kabel

Ubah data di sidebar sesuai kebutuhan:

- Nama kabel
- Node
- Kapasitas core
- Warna
- Jenis garis
- Ketebalan garis
- Catatan

### 7.4 Menyimpan Perubahan

Klik tombol **"Simpan"** (berwarna hijau). Data kabel diperbarui.

### 7.5 Membatalkan Edit

Klik tombol **"Batal Edit"** (berwarna merah) di sidebar untuk keluar dari mode edit tanpa menyimpan perubahan.

---

## Sesi 8: Memotong Kabel (Split) — Menyisipkan Node Baru

Pemotongan kabel digunakan untuk menambahkan titik sambungan baru di tengah rute kabel (misalnya memasang Joint Closure).

### 8.1 Langkah Memotong dan Menyisipkan Node Baru

1. **Klik garis kabel** di peta — titik klik menentukan lokasi pemotongan.
2. Kartu informasi kabel melayang muncul.
3. Klik tombol **"Potong Kabel"** (berwarna abu-abu).
4. Sidebar beralih ke **mode Split** dengan panel berwarna kuning/amber.
5. Di sidebar, kamu akan melihat:
   - **Pilih Node** — Combobox untuk memilih node titik potong.
   - **Buat Node Baru** — Tombol untuk membuat node baru di titik potong.
6. Klik **"Buat Node Baru"** (ikon +).
7. Formulir pembuatan node terbuka dengan koordinat otomatis terisi dari titik potong.
8. Isi data node (nama, tipe — biasanya **Joint Closure**, grup, dll.).
9. Klik **"Simpan"**.
10. Node baru akan dibuat, dan kabel otomatis terpotong menjadi dua segmen:
    - Segmen 1: Node Awal → Node Baru.
    - Segmen 2: Node Baru → Node Akhir.
11. Peta diperbarui dengan dua kabel baru.

### 8.2 Hasil Pemotongan

- Satu kabel dipecah menjadi dua kabel berbeda.
- Node baru berada di titik potong sebagai penghubung.
- Setiap kabel baru memiliki ID dan nama baru (nama asli + suffix segmen).
- Panjang kabel dihitung ulang untuk masing-masing segmen.

---

## Sesi 9: Memotong Kabel (Split) — Menggunakan Node yang Sudah Ada

Jika node tujuan potong sudah ada di database (misalnya Joint Closure yang sudah terdaftar), gunakan cara ini.

### 9.1 Langkah Memotong dengan Node Eksisting

1. **Klik garis kabel** di peta pada titik yang diinginkan.
2. Klik tombol **"Potong Kabel"** pada kartu informasi.
3. Sidebar beralih ke **mode Split**.
4. Pada Combobox **"Pilih Node"**:
   - Ketik nama node yang sudah ada.
   - Pilih node dari hasil pencarian.
5. Klik tombol **"Lakukan Pemotongan"** (berwarna hijau).
6. Kabel akan terpotong menjadi dua segmen yang terhubung di node yang dipilih.

### 9.2 Preview Pemotongan

Saat mode Split aktif, peta akan menampilkan **preview garis putus-putus oranye** yang menunjukkan dua segmen kabel hasil pemotongan. Ini membantu Anda memvisualisasikan hasil sebelum eksekusi.

---

## Sesi 10: Menyambung Kembali Kabel yang Terpotong (Merge)

Jika dua kabel terpisah sebenarnya berasal dari satu kabel yang sama, atau jika Anda ingin menggabungkan dua segmen kabel yang terhubung di satu node.

### 10.1 Syarat Merge

- Kedua kabel harus terhubung di **node yang sama** (memiliki node bersama).
- Kedua kabel **tidak boleh** memiliki sambungan core (splice) aktif.
- Kabel tidak bisa di-merge dengan dirinya sendiri.

### 10.2 Langkah Merge

1. **Klik kabel pertama** di peta.
2. Klik tombol **"Edit Kabel"** pada kartu informasi.
3. Sidebar beralih ke mode **Edit Kabel**.
4. Di bawah tombol Simpan, klik tombol **"Gabung Kabel"** (berwarna biru).
5. Panel merge akan muncul dengan:
   - Dropdown **"Pilih Kabel untuk Digabung"** — otomatis terfilter hanya kabel yang terhubung di node yang sama.
6. Pilih kabel kedua dari dropdown.
7. Klik **"Konfirmasi"**.
8. Kedua kabel akan digabung menjadi satu kabel baru:
   - Rute: menggabungkan path dari kedua kabel.
   - Node: menggabungkan node dari kedua kabel.
   - Node bersama di tengah dihapus.
9. Peta diperbarui.

### 10.3 Membatalkan Mode Merge

Klik ikon **X** di pojok kanan atas panel merge untuk keluar dari mode merge tanpa melakukan penggabungan.

---

## Sesi 11: Menghapus Kabel

### 11.1 Langkah Menghapus

1. **Klik garis kabel** di peta.
2. Klik tombol **"Edit Kabel"** pada kartu informasi.
3. Sidebar beralih ke mode **Edit Kabel**.
4. Di samping tombol Simpan, klik tombol **tong sampah** (berwarna merah).
5. Modal konfirmasi akan muncul.
6. Klik **"Konfirmasi"** untuk melanjutkan.
7. Kabel akan dihapus dari peta dan database.

### 11.2 Jika Penghapusan Gagal

Jika terjadi error (misalnya kabel masih memiliki splice aktif), pesan error akan ditampilkan di modal konfirmasi. Perbaiki dependensi terlebih dahulu (putuskan semua splice), lalu coba lagi.

---

## Sesi 12: Menyambungkan Core (Splicing)

Splicing adalah proses menyambungkan satu core kabel fiber optik ke core kabel lain pada suatu node (misalnya di Joint Closure atau ODP).

### 12.1 Membuka Splice Tray

1. **Klik marker node** di peta tempat penyambungan ingin dilakukan.
2. Klik tombol **"Detail Node"**.
3. Drawer informasi node akan terbuka.
4. Gulir ke bagian **"Splice Trays"**.
5. Setiap kabel yang terhubung ke node tersebut akan ditampilkan sebagai **SpliceTray** (kartu splice).

### 12.2 Memahami Struktur SpliceTray

Setiap SpliceTray menampilkan:

| Elemen             | Deskripsi                                                                      |
| ------------------ | ------------------------------------------------------------------------------ |
| **Nama Kabel**     | Header kartu dengan nama dan kapasitas core                                    |
| **Tube Selector**  | Tombol-tombol berwarna (T1, T2, T3, ...) — mewakili grup Tube (selubung) kabel |
| **Grid Core**      | Grid 6 kolom berisi lingkaran core berwarna sesuai standar IEC 60304           |
| **Ikon Link (🔗)** | Core yang sudah tersambung menampilkan ikon link                               |
| **Badge Angka**    | Jumlah sambungan pada core tersebut                                            |
| **Efek Glow**      | Core tersambung memiliki efek cahaya                                           |

### 12.3 Standar Warna IEC 60304

Setiap Tube berisi 12 core dengan urutan warna tetap:

| No  | Warna           | No  | Warna           |
| --- | --------------- | --- | --------------- |
| 1   | Biru (Blue)     | 7   | Merah (Red)     |
| 2   | Oranye (Orange) | 8   | Hitam (Black)   |
| 3   | Hijau (Green)   | 9   | Kuning (Yellow) |
| 4   | Cokelat (Brown) | 10  | Ungu (Violet)   |
| 5   | Abu-abu (Slate) | 11  | Pink (Rose)     |
| 6   | Putih (White)   | 12  | Toska (Aqua)    |

### 12.4 Langkah Menyambung Core (Drag-and-Drop)

1. Buka drawer detail node.
2. Pada SpliceTray kabel sumber, cari core yang ingin disambung.
3. **Drag** (klik-tahan-seret) lingkaran core dari kabel sumber.
4. **Drop** (lepas) di atas lingkaran core pada kabel tujuan.
5. Sistem akan:
   - Memvalidasi tidak ada duplikasi sambungan.
   - Menyimpan sambungan ke backend.
   - Menampilkan animasi loading (spinner) pada kedua core.
   - Setelah berhasil, core akan berubah menjadi ikon link (🔗) dengan efek glow.
6. Toast sukses akan muncul: **"Sambungan berhasil disimpan"**

### 12.5 Visualisasi Hasil Sambungan

- **Core tersambung (1 sambungan):** Lingkaran dengan ikon link, efek glow biru, ring biru tipis.
- **Core tersambung (2+ sambungan):** Lingkaran dengan ikon link, badge angka hijau, efek glow hijau, ring hijau.
- **Tooltip:** Hover pada core tersambung menampilkan detail: Core X → [Nama Kabel Tujuan] Core Y.

### 12.6 Validasi Duplikasi

Sistem mencegah:

- Sambungan dari core ke dirinya sendiri (kabel dan core yang sama).
- Sambungan duplikat (in_cable + in_core + out_cable + out_core sudah ada).

### 12.7 Optimistic Locking

Jika dua admin menyambung core di node yang sama secara bersamaan:

- Admin pertama: sukses.
- Admin kedua: mendapat error **"Data telah diubah oleh admin lain. Silakan muat ulang halaman."** (HTTP 409).

---

## Sesi 13: Memutus Sambungan Core (Unsplicing)

### 13.1 Langkah Memutus Sambungan

1. Buka drawer detail node.
2. Pada SpliceTray, **hover** di atas core yang sudah tersambung (ikon link).
3. Ikon **X merah** akan muncul di pojok kanan atas lingkaran core.
4. Klik ikon X tersebut.
5. Modal konfirmasi muncul dengan pesan **"Putus Sambungan?"**.
6. Klik **"Konfirmasi"**.
7. Sambungan akan dihapus, core kembali ke tampilan normal (angka saja).

### 13.2 Core dengan Banyak Sambungan (Splitter)

Jika satu core memiliki lebih dari 1 sambungan (splitter), setiap klik X akan menghapus satu sambungan. Ulangi untuk menghapus sambungan lainnya.

---

## Sesi 14: Melihat dan Menambah Catatan pada Kabel dan Core

### 14.1 Catatan pada Kabel (Cable Notes)

1. Klik garis kabel di peta.
2. Kartu informasi kabel menampilkan bagian **"Catatan"** jika ada.
3. Untuk menambah/mengedit: klik **"Edit Kabel"** → isi field **"Catatan"** di sidebar → Simpan.

### 14.2 Catatan pada Core (Core Notes)

Setiap core individual bisa memiliki catatan sendiri, berguna untuk mencatat kondisi core, hasil pengukuran, dll.

#### Menambah/Mengedit Catatan Core:

1. Buka drawer detail node → SpliceTray.
2. **Hover** di atas lingkaran core.
3. Ikon **pensil biru** muncul di pojok kanan bawah core.
4. Klik ikon pensil.
5. Modal **"Catatan Core [nomor]"** terbuka.
6. Tulis catatan (contoh: "Redaman tinggi, perlu investigasi").
7. Klik **"Simpan"**.
8. Setelah tersimpan, indikator titik biru akan muncul di core yang memiliki catatan.

#### Melihat Catatan Core:

- **Hover** pada core yang memiliki titik biru. Tooltip akan menampilkan isi catatan.

---

## Sesi 15: Tracing Core (Pelacakan Jalur Sambungan)

Tracing core digunakan untuk melacak jalur yang dilalui oleh satu core tertentu dari kabel awal melewati semua sambungan.

### 15.1 Memulai Tracing dari Peta

1. **Klik garis kabel** di peta.
2. Kartu informasi kabel menampilkan **panel Core Map** — grid core kecil dengan tube selector.
3. Pilih tube yang diinginkan, lalu **klik lingkaran core** yang ingin dilacak.
4. Sistem akan:
   - Mencari jalur sambungan dari core tersebut melalui semua kabel yang tersambung.
   - Menampilkan jalur di peta dengan warna sesuai core.
   - Menampilkan tombol **"Keluar Tracing"** (X merah) di atas peta.
   - Sidebar beralih ke tab **"Tracing"** menampilkan hasil.

### 15.2 Membaca Hasil Tracing

Di sidebar tab **"Tracing"**, informasi yang ditampilkan:

| Informasi          | Deskripsi                                                     |
| ------------------ | ------------------------------------------------------------- |
| **Warna Core**     | Warna core yang dilacak                                       |
| **Kabel Dilewati** | Daftar nama kabel yang dilewati beserta panjang masing-masing |
| **Node Dilewati**  | Daftar node yang dilewati                                     |
| **Total Panjang**  | Total panjang rute (termasuk estimasi jarak antar-node)       |
| **Panjang Rute**   | Panjang rute kabel saja (tanpa jarak antar-node)              |
| **Jumlah Node**    | Total node yang dilewati                                      |

### 15.3 Keluar dari Mode Tracing

Klik tombol **"Keluar Tracing"** (X merah) di atas peta, atau buka tab **"Kabel"** di sidebar. Peta akan kembali ke tampilan normal.

---

## Sesi 16: Tracing OTDR (Simulasi Deteksi Putus)

OTDR (_Optical Time Domain Reflectometer_) adalah alat yang mengukur jarak ke titik putus fiber berdasarkan pantulan cahaya. Fitur ini mensimulasikan hasil pengukuran OTDR.

### 16.1 Prasyarat

- Tracing core harus sudah aktif (ada jalur yang dilacak).
- Tab **"Tracing"** di sidebar aktif.

### 16.2 Langkah Melakukan Tracing OTDR

1. Setelah tracing core aktif, buka tab **"Tracing"** di sidebar.
2. Isi form OTDR:
   - **Node Awal:** Pilih node tempat pengukuran OTDR dilakukan.
   - **Arah Kabel:** Pilih kabel arah pengukuran (muncul jika ada >1 kabel di node).
   - **Jarak OTDR (m):** Masukkan jarak terukur dari perangkat OTDR (dalam meter).
   - **Panjang Slack (m):** Masukkan estimasi panjang gulungan kabel di sekitar node (default: 10m).
3. Klik tombol **"Lacak OTDR"**.
4. Sistem akan:
   - Menelusuri jalur dari node awal sejauh jarak OTDR.
   - Memperhitungkan panjang setiap kabel, slack, dan jarak antar-node.
   - Menjatuhkan **pin kuning** di perkiraan titik putus.
   - Memperbarui tampilan hasil tracing.

### 16.3 Membaca Hasil OTDR

Pin kuning di peta menunjukkan estimasi lokasi putus. Sidebar menampilkan rute yang dilewati dari node awal hingga titik putus.

> **Catatan:** Slack length (panjang gulungan) penting karena kabel biasanya memiliki gulungan cadangan di dekat node yang harus diperhitungkan dalam kalkulasi jarak.

---

## Sesi 17: Melihat dan Mencetak Diagram Topologi Core

Diagram topologi core menampilkan alur sambungan antar node dalam bentuk diagram alir (flowchart) menggunakan React Flow.

### 17.1 Membuka Diagram Topologi

1. Buka drawer detail node (klik node → Detail Node).
2. Gulir ke bagian **"Topologi Core"**.
3. Atur parameter:
   - **Hop:** Jumlah lompatan node yang ditampilkan (1–5). Semakin besar, diagram semakin luas.
   - **Arah Diagram:** Horizontal (LR — Left to Right) atau Vertikal (TB — Top to Bottom).
4. Klik tombol **"Lihat Topologi"**.
5. Modal diagram topologi akan terbuka.

### 17.2 Membaca Diagram Topologi

| Elemen           | Deskripsi                                                                   |
| ---------------- | --------------------------------------------------------------------------- |
| **Node (Kotak)** | Menampilkan nama node, tipe, dan daftar sambungan (splice) di node tersebut |
| **Garis (Edge)** | Menampilkan nama kabel, kapasitas core, dan panjang kabel                   |
| **Panah**        | Menunjukkan arah alur sambungan                                             |
| **Minimap**      | Pojok kanan bawah — peta mini untuk navigasi diagram besar                  |
| **Controls**     | Pojok kiri bawah — zoom in/out, fit view, lock                              |

### 17.3 Mencetak Diagram

1. Pada modal diagram, klik tombol **"Cetak"** (ikon printer) di header.
2. Sistem akan:
   - Menghitung ukuran diagram.
   - Menyesuaikan tampilan agar semua node terlihat.
   - Membuka dialog cetak browser.
3. Sesuaikan pengaturan cetak (ukuran kertas, orientasi, margin) dan klik **Print**.

> **Tips:** Untuk diagram besar, gunakan orientasi **Landscape** dan atur skala cetak agar sesuai.

---

## Sesi 18: Menggunakan Filter Node dan Kapasitas Kabel

### 18.1 Filter Tipe Node

Di sidebar tab **"Kabel"** (mode normal), terdapat panel **"Filter Tipe Node"**:

1. Panel menampilkan checkbox untuk setiap tipe node yang tersedia (POP, ODP, ODC, Joint Closure, dll).
2. Centang/ hapus centang untuk menampilkan/menyembunyikan node di peta.
3. Gunakan toggle switch di kanan atas panel untuk **Pilih Semua / Hapus Semua**.

### 18.2 Filter Kapasitas Kabel

Di sidebar tab **"Kabel"** (mode normal), terdapat panel **"Filter Kapasitas Kabel"**:

1. Panel menampilkan checkbox untuk setiap kapasitas core yang tersedia (12 Core, 24 Core, 48 Core, dll).
2. Centang/ hapus centang untuk menampilkan/menyembunyikan kabel dengan kapasitas tertentu.
3. Gunakan toggle switch di kanan atas panel untuk **Pilih Semua / Hapus Semua**.

### 18.3 Manfaat Filter

- Mengurangi beban peta dengan hanya menampilkan data yang relevan.
- Membantu fokus pada jenis infrastruktur tertentu.
- Meningkatkan performa peta terutama untuk jaringan besar.

---

## Sesi 19: Mode Layar Penuh

### 19.1 Mengaktifkan Mode Layar Penuh

Klik tombol **ikon expand** (dua panah diagonal) di pojok kanan atas sidebar.

### 19.2 Menonaktifkan Mode Layar Penuh

Klik tombol **ikon shrink** (dua panah diagonal ke dalam) di pojok kanan atas sidebar, atau tekan `Esc` pada keyboard.

### 19.3 Manfaat

- Memaksimalkan area peta untuk analisis visual.
- Berguna saat presentasi atau analisis rute yang panjang.

---

## Sesi 20: Memahami Sistem Clustering dan Zoom

### 20.1 Level Zoom Rendah (≤12)

- **Node marker:** Digabungkan menjadi **cluster** (lingkaran biru berangka). Angka menunjukkan jumlah node dalam cluster.
- **Kabel:** Ditampilkan dengan detail terbatas untuk performa.

### 20.2 Level Zoom Tinggi (>12)

- **Node cluster:** Dipecah menjadi marker individu yang bisa diklik.
- **Kabel:** Ditampilkan lengkap dengan tooltip dan interaksi klik.

### 20.3 Kabel Overlap (Bertumpuk)

Jika dua kabel melewati rute yang hampir sama, sistem otomatis:

- Mengelompokkan kabel yang tumpang tindih.
- Menampilkan **offset lateral** — kabel digeser sedikit ke samping agar tidak bertumpuk persis.
- Menampilkan kabel **bayangan** (abu-abu putus-putus) sebagai referensi.
- Menampilkan **navigasi carousel** (panah kiri/kanan) di kartu informasi untuk berpindah antar kabel dalam grup overlap.

### 20.4 Klik pada Kabel Overlap

Saat mengklik area dengan beberapa kabel bertumpuk, kartu informasi menampilkan:

- Panah navigasi kiri/kanan.
- Indikator "1 / 3" (kabel ke-1 dari 3 kabel di lokasi ini).

---

## Sesi 21: Memahami Perhitungan Redaman Optik (Optical Budget)

### 21.1 Rumus Redaman

Sistem menghitung estimasi redaman optik secara teoritis:

```
Redaman Total = (Total Panjang Kabel (km) × 0.35 dB/km) + (Jumlah Splice × 0.1 dB)
```

| Komponen       | Konstanta     | Deskripsi                                        |
| -------------- | ------------- | ------------------------------------------------ |
| Redaman kabel  | 0.35 dB/km    | Estimasi redaman per kilometer kabel fiber optik |
| Redaman splice | 0.1 dB/splice | Estimasi redaman per titik sambungan             |

### 21.2 Di Mana Redaman Ditampilkan

Hasil perhitungan redaman ditampilkan di:

- Drawer detail node (saat tracing core).
- Sidebar tab "Tracing" (total panjang rute).

### 21.3 Catatan Penting

- Nilai redaman adalah **estimasi teoritis**. Pengukuran aktual di lapangan bisa berbeda.
- **Slack length** (gulungan cadangan) diperhitungkan dalam total panjang.
- Setiap sambungan (splice) memiliki field `loss_db` yang dapat diisi nilai aktual dari pengukuran di lapangan.

---

## Sesi 22: Mengelola Konflik Data (Optimistic Locking)

### 22.1 Apa Itu Optimistic Locking?

Mekanisme yang mencegah dua admin mengedit data yang sama secara bersamaan. Setiap dokumen splice memiliki versi (`__v`) yang bertambah setiap kali perubahan disimpan.

### 22.2 Skenario Konflik

1. Admin A membuka SpliceTray Node X.
2. Admin B juga membuka SpliceTray Node X.
3. Admin A menyambung core dan menyimpan — sukses, versi dokumen bertambah.
4. Admin B mencoba menyimpan — **ditolak** karena versi dokumen sudah berubah.

### 22.3 Pesan Error

Admin B akan menerima toast error:

> **"Data telah diubah oleh admin lain. Silakan muat ulang halaman."**

### 22.4 Cara Mengatasi

1. Tutup drawer detail node.
2. Buka kembali drawer detail node (data akan dimuat ulang dengan perubahan terbaru).
3. Lakukan kembali penyambungan yang diinginkan.

---

## Pintasan Keyboard

| Tombol          | Fungsi                                                    |
| --------------- | --------------------------------------------------------- |
| `Shift` (tahan) | Saat toggle auto-routing: membalik sementara mode routing |
| `Esc`           | Keluar dari mode layar penuh (fullscreen)                 |

---

## Struktur Data Referensi

### Kabel Fiber Optik (FiberCable)

```
{
  cable_id: "FC-1001",
  name: "FO-Ciganitri-Pasirjati-01",
  nodes: [ObjectId, ObjectId],       // Minimal 2 node
  path: {
    type: "LineString",
    coordinates: [[lng, lat], ...]
  },
  length_m: 1250.5,
  slack_length_m: 15.0,
  core_capacity: 24,                 // 1-288
  direction: "BI-DIRECTIONAL",       // IN | OUT | BI-DIRECTIONAL
  status: "ACTIVE",                  // ACTIVE | INACTIVE | CUT | MAINTENANCE
  is_routing_auto: true,
  color: "#3b82f6",
  line_type: "solid",                // solid | dashed
  line_weight: 3,                    // 1-10
  notes: "Kabel feeder utama",
  core_notes: { "0": "Core biru - OK", "5": "Redaman tinggi" },
  splices: {
    "0": [{ in_cable, in_core, out_cable, out_core, loss_db, notes }],
    ...
  },
  slack_loops: [{ node_id, length_m, notes }]
}
```

### Sambungan Core (Splice Entry)

```
{
  in_cable: ObjectId,       // Kabel sumber
  in_core: 0,               // Nomor core sumber (0-based)
  out_cable: ObjectId,      // Kabel tujuan
  out_core: 5,              // Nomor core tujuan (0-based)
  is_cut: false,            // Status putus
  loss_db: 0.1,             // Redaman aktual (dB)
  notes: "Sambungan ODP-03"
}
```

---

## Penutup

Modul Manajemen Kabel Fiber Optik adalah alat komprehensif untuk mengelola infrastruktur jaringan fiber optik secara visual dan terstruktur. Dengan memahami setiap sesi dalam tutorial ini, Anda dapat:

- Memetakan jaringan kabel secara akurat di peta GIS.
- Mengelola inventaris kabel dan sambungan core.
- Melacak jalur sinyal optik dari ujung ke ujung.
- Mensimulasikan deteksi titik putus dengan OTDR.
- Mendokumentasikan topologi core untuk keperluan lapangan.

Untuk pertanyaan atau bantuan lebih lanjut, hubungi administrator sistem.

---

_Dokumen ini dibuat berdasarkan fitur Fiber Optic Management ISPF V2._  
_Standar warna mengacu pada IEC 60304._  
_Terakhir diperbarui: Juni 2026._
