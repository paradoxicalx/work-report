# 📝 Daily Work Report - Dedy S.N Putra (2026-10-01)

---

## 📅 Laporan Harian - 1 Oktober 2026

> Laporan ini mencakup seluruh sesi pekerjaan per 1 Oktober 2026, yang berfokus pada tiga domain utama:
> 1. **Perancangan & Implementasi Menyeluruh Fitur Menu Sidebar Dinamis Global (`worktree-dynamic-sidebar-menu`)**: Membangun sistem tata letak menu sidebar yang sepenuhnya dapat disesuaikan oleh administrator (hingga kedalaman 3 tingkat hierarki), engine drag-and-drop berbasis `@atlaskit/pragmatic-drag-and-drop`, validasi dan service backend dengan *optimistic locking*, auto-healing pohon menu frontend, serta migrasi seluruh layout (`main-layout` dan `sideblock`) dengan cakupan unit & integrasi test yang luas.
> 2. **Penyelesaian & Rilis Gateway Pembayaran & Rekonsiliasi Otomatis (`issue-360`)**: Mengimplementasikan mekanisme kontrol batas laju (*rate limiting* HTTP 429), *exponential backoff* dengan *jitter*, parser header `Retry-After` (standar RFC 7231 & detik), pembagian batch rekonsiliasi (*chunking*), hingga pencatatan rilis resmi v1.83.4 di sistem changelog.
> 3. **Peningkatan Resiliensi Komponen Avatar & Normalisasi Petugas Scheduler (`issue-346`)**: Menghadirkan mekanisme fallback tangguh avatar pengguna/staf/pelanggan ke Dicebear WebP dan inisial teks pada UI, perbaikan resolusi ID petugas rencana kerja scheduler, serta sinkronisasi opsi topik tiket Telegram (Bisnis, Backbone, Backhaul).

---

## 🌿 Branch: `worktree-dynamic-sidebar-menu` — Desain, Engine, & Editor Tata Letak Menu Sidebar Dinamis Global (Multi-Layout, Optimistic Locking, & Drag-and-Drop)

### 📌 Informasi Issue

- **Nomor Issue**: Feature / Superpowers (Dynamic Sidebar Menu)
- **Judul Issue**: Implementasi Sistem Tata Letak Menu Sidebar Dinamis Global (Editor Drag-and-Drop Pragmatic, Penyimpanan Dokumen Opsi, Optimistic Locking, Normalisasi Pohon 3-Level, dan Migrasi Komponen Sidebar)
- **Status Branch**: `Belum di-merge` (Branch aktif pada worktree `worktree-dynamic-sidebar-menu`, seluruh 14 commit telah selesai diimplementasikan bersama test suite dan siap untuk proses review/merge)

### 📅 Rincian Commit

#### [[`361142cf`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE docs(menu): pola menu sidebar dinamis di AGENTS.md - 1 Oktober 2026, 21:42:28 WIB

- **Komponen yang Berubah**:
  - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md):
    - Mendokumentasikan pola baku arsitektur menu sidebar dinamis ke dalam panduan monorepo: aturan penyimpanan dokumen option `menu_layout`, kedalaman maksimal 3 level, fallback item yang belum terdaftar ke grup default "Lainnya", dan isolasi menu Settings di luar pohon yang dapat diedit.
- **Deskripsi Perubahan & Fungsi**:
  - Menjaga kepatuhan standar rekayasa perangkat lunak monorepo bagi AI Agent dan developer di masa mendatang saat berinteraksi dengan sistem navigasi.

#### [[`5a7d542b`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE fix(menu): reset susunan menu selalu membuang draf - 1 Oktober 2026, 21:36:06 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/settings/sections/MenuLayout.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/MenuLayout.jsx):
    - Memperbaiki alur reset tata letak menu agar segera membuang dan membersihkan draf lokal (`clearDraftLayout`), lalu memuat ulang susunan default dari backend secara bersih.
- **Deskripsi Perubahan & Fungsi**:
  - Mencegah sisa perubahan yang belum disimpan (stale draft state) tetap muncul kembali setelah admin menekan tombol "Reset ke Default".

#### [[`42ca33a7`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE feat(menu): editor susunan menu di Settings - 1 Oktober 2026, 21:31:13 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/settings/sections/MenuLayout.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/MenuLayout.jsx) [NEW]:
    - Halaman utama manajemen susunan menu navigasi pada Pengaturan Aplikasi: menampilkan pohon hierarki menu, status simpan/draf, deteksi konflik versi (HTTP 409), tombol Tambah Grup Root, tombol Reset, dan tombol Simpan Perubahan.
  - [`frontend/src/app/pages/settings/menuEditor/MenuTree.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/menuEditor/MenuTree.jsx) [NEW]:
    - Komponen visual interaktif pohon menu dengan drag-and-drop `@atlaskit/pragmatic-drag-and-drop`: mendukung drag reordering antar item/grup, visual drop indicator (garis biru dan kotak penampung), expand/collapse sub-grup, aksi cepat tambah sub-grup, ubah grup, hapus grup, dan pindah item.
  - [`frontend/src/app/pages/settings/menuEditor/GroupDialog.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/menuEditor/GroupDialog.jsx) [NEW]:
    - Modal dialog pembuatan dan pengeditan grup/sub-grup menu: input nama bilingual (Bahasa Indonesia & Bahasa Inggris), pemilihan ikon dari registri ikon sistem, dan pemilihan divider pemisah.
  - [`frontend/src/app/pages/settings/menuEditor/MoveDialog.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/menuEditor/MoveDialog.jsx) [NEW]:
    - Modal pemindahan item alternatif yang aksesibel bagi pengguna non-drag-and-drop: memilih grup tujuan secara langsung dari daftar dropdown.
  - [`frontend/src/app/navigation/settings.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/settings.js) & [`frontend/src/app/router/settings/settingsRoute.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/router/settings/settingsRoute.jsx):
    - Mendaftarkan rute navigasi dan tab pengaturan `settings.menuLayout` (`/settings/menu-layout`).
  - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json):
    - Mendaftarkan seluruh label terjemahan bilingual untuk antarmuka editor susunan menu.
- **Deskripsi Perubahan & Fungsi**:
  - Memberikan antarmuka visual yang modern dan intuitif bagi admin untuk menata struktur menu aplikasi sesuai alur kerja operasional perusahaan tanpa perlu menyentuh kode program.

#### [[`156e605b`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE feat(menu): operasi pohon editor susunan menu - 1 Oktober 2026, 21:26:17 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/pages/settings/menuEditor/layoutOps.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/menuEditor/layoutOps.js) [NEW]:
    - Modul utilitas murni untuk manipulasi immutability struktur data pohon menu: `addGroup`, `updateGroup`, `deleteGroup`, `moveNode`, `reorderSiblings`, serta validasi pencegahan siklus atau nesting berlebih (> 3 level).
  - [`frontend/src/app/pages/settings/menuEditor/layoutOps.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/menuEditor/layoutOps.test.js) [NEW]:
    - 324 baris pengujian unit komprehensif menguji seluruh operasi pohon, mutasi node, pergeseran posisi, dan kasus batas hierarki.
- **Deskripsi Perubahan & Fungsi**:
  - Menjamin setiap mutasi pohon menu di browser berjalan murni secara fungsional (*pure immutable operations*), bebas efek samping (*side-effects*), dan aman dari inkonsistensi struktur data.

#### [[`4361e63b`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE refactor(menu): sideblock dan Search memakai useMenuTree - 1 Oktober 2026, 21:23:50 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/layouts/Sideblock/Sidebar/Menu/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/Sideblock/Sidebar/Menu/index.jsx):
    - Mengganti pembacaan array statis dengan `useMenuTree()` yang reaktif terhadap tata letak kustom backend.
  - [`frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/index.jsx) & [`CollapsibleItem/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/Sideblock/Sidebar/Menu/Group/CollapsibleItem/index.jsx):
    - Menyederhanakan logika rendering grup dan sub-grup lipat (*collapsible*) berdasar pohon menu terpadu.
  - [`frontend/src/components/template/Search/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/template/Search/index.jsx):
    - Mengintegrasikan dialog pencarian global cepat (Ctrl+K) agar mengindeks item menu langsung dari pohon menu aktif yang sudah disaring hak akses penggunanya.
- **Deskripsi Perubahan & Fungsi**:
  - Menyelaraskan tampilan tata letak `Sideblock` dan komponen `Search` agar otomatis mengikuti struktur dinamis yang diatur admin, serta mengeliminasi item yang tidak berhak diakses user dari hasil pencarian cepat.

#### [[`bc744f5c`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE refactor(menu): sidebar main-layout memakai useMenuTree - 1 Oktober 2026, 21:20:21 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/app/layouts/MainLayout/Sidebar/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/MainLayout/Sidebar/index.jsx):
    - Migrasi navigasi panel utama (`MainLayout`) untuk mengonsumsi `useMenuTree()`.
  - [`frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/Menu/index.jsx) & [`PrimePanel/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/MainLayout/Sidebar/PrimePanel/index.jsx):
    - Menghapus duplikasi kode filter privilege manual dan menghubungkan grup navigasi vertikal ke pohon menu hasil resolver.
  - [`frontend/src/app/layouts/MainLayout/Sidebar/MainPanel/Menu/index.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/MainLayout/Sidebar/MainPanel/Menu/index.jsx) & [`Item.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/MainLayout/Sidebar/MainPanel/Menu/Item.jsx):
    - Penyesuaian rendering ikon dinamis dan badge total per-grup pada ikon sidebar yang sedang dilipat.
- **Deskripsi Perubahan & Fungsi**:
  - Memastikan mode tampilan layout utama (`main-layout`) merefleksikan susunan kustom menu, termasuk perlakuan dinamis pada panel primer dan sub-menu sekunder.

#### [[`b7572fd3`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE feat(menu): slice susunan menu, registri ikon, dan hook useMenuTree - 1 Oktober 2026, 21:17:31 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/features/menuLayoutSlice.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/features/menuLayoutSlice.js) [NEW] & [`frontend/src/features/menuLayoutSlice.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/features/menuLayoutSlice.test.js) [NEW]:
    - Redux Toolkit slice untuk mengelola state susunan menu: async thunk `fetchMenuLayout`, `saveMenuLayout`, dan `resetMenuLayout`, dilengkapi penanganan loading, error, dan sinkronisasi versi.
  - [`frontend/src/hooks/useMenuTree.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/hooks/useMenuTree.js) [NEW]:
    - Custom React hook terpusat yang menggabungkan state layout Redux, katalog navigasi statis, hak akses privilege pengguna saat ini, dan penghitungan badge tiket menjadi struktur pohon menu siap render.
  - [`frontend/src/app/navigation/menuIcons.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/menuIcons.js) [NEW] & [`frontend/src/app/navigation/menuIcons.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/navigation/menuIcons.test.js) [NEW]:
    - Registri ikon menu sidebar terpusat (Heroicons & Tabler Icons) untuk mendukung pemilihan ikon dinamis pada grup/sub-grup menu.
  - [`frontend/src/app/layouts/Root.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/layouts/Root.jsx) & [`frontend/src/store.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/store.js):
    - Mendaftarkan `menuLayoutReducer` ke Redux store dan melakukan inisialisasi pemanggilan layout saat aplikasi pertama kali dimuat.
- **Deskripsi Perubahan & Fungsi**:
  - Menghubungkan lapisan data backend dengan komponen visual antarmuka secara reaktif dan terkelola rapi melalui Redux Toolkit.

#### [[`6a065d20`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE refactor(menu): badge sidebar jadi fungsi murni dan badge grup - 1 Oktober 2026, 21:15:13 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/hooks/useTicketBadge.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/hooks/useTicketBadge.js):
    - Merefaktor `useSidebarBadge` menjadi fungsi murni `calculateMenuBadge(menuId, badgeState)` dan menambahkan `calculateGroupBadge(node, badgeState)`.
  - [`frontend/src/hooks/useTicketBadge.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/hooks/useTicketBadge.test.js) [NEW]:
    - 289 baris pengujian unit memvalidasi akurasi perhitungan badge individual dan akumulasi badge grup untuk seluruh kategori tiket operasional.
- **Deskripsi Perubahan & Fungsi**:
  - Memungkinkan penampil angka notifikasi tiket (badge) tetap aktif secara akurat bahkan saat item tiket dipindahkan ke dalam sub-grup baru yang dibuat oleh admin.

#### [[`81742779`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE fix(menu): completeLayout tidak membuat grup Lainnya ganda - 1 Oktober 2026, 21:13:12 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/utils/menuTree.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/menuTree.js) & [`frontend/src/utils/menuTree.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/menuTree.test.js):
    - Memperbaiki helper normalisasi `completeLayout` agar mendeteksi keberadaan grup default "Lainnya" yang sudah ada dalam dokumen tata letak, sehingga tidak tercipta duplikasi grup penampung saat ada item baru di kode.
- **Deskripsi Perubahan & Fungsi**:
  - Mencegah inkonsistensi visual di mana grup "Lainnya" muncul lebih dari satu kali pada navigasi sidebar.

#### [[`ac8a2826`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE feat(menu): resolver pohon menu sidebar dan test runner frontend - 1 Oktober 2026, 21:10:43 WIB

- **Komponen yang Berubah**:
  - [`frontend/src/utils/menuTree.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/menuTree.js) [NEW]:
    - Engine resolver pohon menu sidebar (`resolveMenuTree`, `completeLayout`, `filterTreeByPrivilege`): memvalidasi referensi item menu terhadap katalog kode, mengisolasi menu Settings, menampung item yatim (*orphan items*) ke grup cadangan, serta menyaring node pohon berdasarkan hak akses pengguna secara rekursif.
  - [`frontend/src/utils/menuTree.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/utils/menuTree.test.js) [NEW]:
    - 552 baris pengujian unit komprehensif untuk pengujian pohon menu (resolusi referensi, normalisasi draf, filter hak akses, dan struktur hierarki).
  - [`frontend/vitest.config.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/vitest.config.js) [NEW] & [`frontend/package.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/package.json):
    - Mengonfigurasi test runner Vitest modern untuk modul frontend Dekasimal V2 dengan dukungan lingkungan Node & jsdom.
- **Deskripsi Perubahan & Fungsi**:
  - Membangun fondasi engine resolver yang kokoh di sisi klien untuk memastikan struktur menu selalu valid dan teruji secara otomatis tanpa risiko breaking changes.

#### [[`5625314e`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE feat(menu): endpoint susunan menu sidebar - 1 Oktober 2026, 21:07:47 WIB

- **Komponen yang Berubah**:
  - [`backend/src/controllers/menuLayout.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/menuLayout.controller.js) [NEW]:
    - Controller penanganan HTTP request: `getMenuLayout`, `updateMenuLayout`, dan `resetMenuLayout` dibungkus dengan `asyncHandler`.
  - [`backend/src/routes/menuLayout.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/menuLayout.route.js) [NEW]:
    - Mendefinisikan endpoint API singular kebab-case: `GET /api/v1/menu-layout`, `PUT /api/v1/menu-layout`, `POST /api/v1/menu-layout/reset`, diproteksi autentikasi JWT dan hak akses `option.read` serta `option.update`.
  - [`backend/src/app.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/app.js):
    - Mendaftarkan rute `menuLayout.route.js` ke dalam aplikasi server Express backend.
  - [`backend/test/integration/menuLayout.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/menuLayout.controller.test.js) [NEW]:
    - Pengujian integrasi endpoint controller mencakup respons 200, penolakan input invalid (400), konflik versi (409), dan reset (200).
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan antarmuka REST API yang aman, berstandar OpenAPI/Swagger, dan tervalidasi bagi frontend untuk membaca dan memperbarui tata letak navigasi.

#### [[`3c7285df`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE feat(menu): service susunan menu dengan optimistic locking - 1 Oktober 2026, 21:05:49 WIB

- **Komponen yang Berubah**:
  - [`backend/src/services/menuLayout.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/menuLayout.service.js) [NEW]:
    - Layanan manipulasi database pada koleksi `options` (key: `menu_layout`): mendukung pembacaan layout dengan fallback default katalog, pembaruan layout berbasis kontrol konkurensi optimistik (`version`), dan reset layout ke konfigurasi awal sistem.
  - [`backend/test/integration/menuLayout.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/menuLayout.service.test.js) [NEW]:
    - 173 baris pengujian integrasi service database menguji operasi CRUD, pencegahan penimpaan data konkuren, dan mekanisme fallback data kosong.
  - [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json) & [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json):
    - Menambahkan pesan i18n untuk status update susunan menu dan deteksi konflik versi.
- **Deskripsi Perubahan & Fungsi**:
  - Mengamankan persistensi data tata letak menu dari risiko *race condition* atau saling timpa ketika beberapa admin membuka halaman editor secara bersamaan.

#### [[`02d93fc2`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE feat(menu): validator susunan menu sidebar - 1 Oktober 2026, 21:03:25 WIB

- **Komponen yang Berubah**:
  - [`backend/src/utils/menuLayoutValidator.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/menuLayoutValidator.js) [NEW]:
    - Validator rekursif struktur skema tata letak menu: memverifikasi tipe node (`group`, `item`, `divider`), batas kedalaman maksimum 3 level, format key dan transKey, kelengkapan nama bilingual, integritas string referensi menu, serta proteksi isolasi menu Settings.
  - [`backend/test/unit/menuLayoutValidator.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/menuLayoutValidator.test.js) [NEW]:
    - 193 baris pengujian unit mencakup pengujian payload valid, payload melampaui kedalaman 3 tingkat, node rusak, referensi hilang, dan payload non-array.
- **Deskripsi Perubahan & Fungsi**:
  - Menjadi dinding pertahanan pertama di sisi server untuk menolak struktur data cacat atau manipulasi payload sebelum disimpan ke database.

#### [[`51f7feb3`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #ISSUE docs(menu): spec menu sidebar dinamis - 1 Oktober 2026, 21:01:38 WIB

- **Komponen yang Berubah**:
  - [`docs/superpowers/specs/2026-10-01-dynamic-sidebar-menu-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-10-01-dynamic-sidebar-menu-design.md) [NEW]:
    - Dokumen spesifikasi teknis dan perancangan arsitektur lengkap menu sidebar dinamis: latar belakang, sasaran fitur, keputusan arsitektur (maksimal 3 level, satu susunan global, penyimpanan referensi katalog), model data option, algoritma resolver, penanganan hak akses, hingga strategi pengujian.
- **Deskripsi Perubahan & Fungsi**:
  - Meletakkan landasan konseptual dan panduan arsitektur yang jelas sebelum implementasi kode dilakukan.

---

## 🌿 Branch: `master` / `issue-360` — Pengendalian Batas Laju (Rate Limiting) & Ketahanan Gateway Pembayaran (iPaymu), Mekanisme Retry Cerdas, dan Rekonsiliasi Otomatis

### 📌 Informasi Issue

- **Nomor Issue**: #360
- **Judul Issue**: Pengendalian Batas Laju (Rate Limiting) & Ketahanan Gateway Pembayaran (iPaymu), Mekanisme Retry Cerdas, dan Rekonsiliasi Otomatis
- **Status Branch**: `Sudah di-merge` (Telah diselesaikan dan di-merge ke branch utama `master` & `production` melalui merge commit [`834d01f3`](file:///home/dhedhy/Project/Dekasimal-V2) pada 1 Oktober 2026, 21:36:52 WIB, serta dirilis resmi pada changelog melalui commit [`f8629ac1`](file:///home/dhedhy/Project/Dekasimal-V2) pada 1 Oktober 2026, 21:46:55 WIB)

### 📅 Rincian Commit

#### [[`f8629ac1`](file:///home/dhedhy/Project/Dekasimal-V2)] - update changelog - 1 Oktober 2026, 21:46:55 WIB

- **Komponen yang Berubah**:
  - [`backend/src/data/changelog/releases/issue-360.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-360.json) [NEW]:
    - Mendokumentasikan catatan rilis resmi versi **v1.83.4** untuk Issue #360: ringkasan fitur pengendalian laju transaksi, pencegahan pemblokiran HTTP 429 pada rekonsiliasi berkala, pemisahan batch transaksi, dan peningkatan akurasi riwayat rekonsiliasi.
  - [`backend/src/data/changelog/index.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/index.json):
    - Memperbarui indeks rilis sistem Dekasimal V2 dengan menambahkan rilis v1.83.4 di baris teratas.
  - [`backend/src/data/changelog/releases/issue-175.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-175.json) [NEW], [`issue-345.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-345.json) [NEW], [`issue-352.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-352.json) [NEW], [`issue-354.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/data/changelog/releases/issue-354.json) [NEW]:
    - Mengarsipkan berkas deskripsi rilis historis yang mendetail untuk modul ACS TR-069, skema multi-bulan, Redis multi-instance, dan validasi tanggal.
- **Deskripsi Perubahan & Fungsi**:
  - Menyediakan transparansi riwayat pembaruan sistem yang dapat dibaca oleh administrator melalui widget atau halaman "Tentang & Pembaruan Sistem".

#### [[`834d01f3`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #360 - 1 Oktober 2026, 21:36:52 WIB

- **Deskripsi**: Merge commit integrasi final penyelesaian issue #360 dari branch kerja ke branch `master` dan `production`.

#### [[`bd7e3745`](file:///home/dhedhy/Project/Dekasimal-V2)] - resolve #360 - 1 Oktober 2026, 21:27:44 WIB

- **Komponen yang Berubah**:
  - **Backend Modul Utilitas Retry Tangguh (`backend/src/`)**:
    - [`backend/src/utils/callWithRetry.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/callWithRetry.js) [NEW]:
      - Membangun fungsi eksekusi HTTP dengan *exponential backoff* dan *full jitter*: parameter `maxRetries`, `baseDelayMs`, `maxDelayMs`, serta jeda default antar-panggilan (`DEFAULT_INTER_REQUEST_DELAY_MS = 250ms`).
      - Pengurai cerdas header `Retry-After`: mengenali format detik murni (`"5"` / `5000ms`) maupun format tanggal RFC 7231 (`HTTP-Date`), serta memastikan sistem menaati anjuran jeda waktu dari gateway eksternal sebelum mencoba kembali.
      - Detektor kegagalan jaringan yang dapat di-retry (`isRetryableError`): mendeteksi error HTTP 429 Too Many Requests, error 5xx, timeout koneksi (`ETIMEDOUT`), dan pemutusan socket tiba-tiba (`ECONNRESET`).
      - Pembantu pemecah array (`chunkArray`) untuk pemrosesan batch bertahap.
  - **Integrasi Service Gateway Pembayaran (`backend/src/services/`)**:
    - [`backend/src/services/paymentGateways/ipaymu.gateway.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/paymentGateways/ipaymu.gateway.js):
      - Membungkus seluruh komunikasi eksternal HTTP API iPaymu (pengecekan transaksi, riwayat mutasi/statement, pembuatan VA) ke dalam mekanisme `callWithRetry` dengan jeda yang terkontrol.
    - [`backend/src/services/paymentGateway.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/paymentGateway.service.js):
      - Mengekspor ulang fungsi pembantu retry dan konstanta jeda antar-request.
    - [`backend/src/services/financeGateway.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/financeGateway.service.js):
      - Memecah proses sapuan transaksi pembayaran menggantung (`sweepPendingTransactions`) ke dalam kelompok-kelompok kecil (`RECON_CHUNK_SIZE = 5`) menggunakan `chunkArray`, mencegah gelombang *burst requests* ke server pembayaran.
      - Meningkatkan pelacakan status rekonsiliasi: mencatat status `recon_status: 'SUCCESS'` saat transaksi berhasil dibukukan, serta memberikan penanda `MANUAL_REVIEW` pada transaksi yang terindikasi ganjil.
  - **Model Data Keuangan (`backend/src/models/`)**:
    - [`backend/src/models/financeGatewayReconRun.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financeGatewayReconRun.model.js) & [`backend/src/models/financeGatewayTransaction.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/financeGatewayTransaction.model.js):
      - Menambahkan field pelacakan error dan status rekonsiliasi yang lebih terperinci.
  - **Pengujian Unit Komprehensif (`backend/test/`)**:
    - [`backend/test/unit/paymentGatewayRetry.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/paymentGatewayRetry.test.js) [NEW]:
      - 473 baris pengujian unit menguji algoritma retry, parser header Retry-After, toleransi jitter, pemecahan chunking array, dan simulasi penanganan lonjakan HTTP 429.
- **Deskripsi Perubahan & Fungsi**:
  - Mengatasi masalah kritis di mana proses rekonsiliasi massal sering gagal atau terblokir oleh server iPaymu akibat memicu pembatasan laju (*rate limiting*). Kini komunikasi berjalan teratur, memiliki jeda cerdas, dan secara otomatis pulih saat terjadi gangguan jaringan sesaat.

---

## 🌿 Branch: `issue-346` — Penambahan Opsi Topik Tiket (Bisnis, Backbone, Backhaul) pada Notifikasi Telegram, Penanganan Avatar Fleksibel & Resilien (Dicebear WebP Fallback), serta Normalisasi ID Petugas Scheduler

### 📌 Informasi Issue

- **Nomor Issue**: #346
- **Judul Issue**: Penambahan Opsi Topik Tiket (Bisnis, Backbone, Backhaul) pada Notifikasi Telegram, Penanganan Avatar Fleksibel & Resilien (Dicebear WebP Fallback), serta Normalisasi ID Petugas Scheduler
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-346`, pekerjaan dalam status _working directory changes_ / persiapan commit)

### 📅 Rincian Commit

#### [Work In Progress] - Penyelarasan Resiliensi Avatar (Dicebear WebP Fallback & ID Agnostik), Normalisasi Petugas Scheduler, dan Opsi Topik Telegram - 1 Oktober 2026

- **Komponen yang Berubah**:
  - **Backend Files Controller (`backend/src/controllers/files.controller.js`)**:
    - *Pencegahan Error Invalid ID*: Menambahkan guard ketat di awal endpoint avatar admin, customer, partner, dan business: jika `id` kosong, bernilai `"undefined"`, atau `"null"`, controller langsung merespons HTTP 404 tanpa mengeksekusi query database atau pemanggilan MinIO.
    - *Resolusi ID Agnostik*: Mendukung pencarian dokumen pengguna baik berdasarkan identifier bisnis (`admin_id`, `customer_id`, `partner_id`) maupun `ObjectId` MongoDB (`_id`).
    - *Pengecekan File Bertingkat di MinIO*: Menguji beberapa kandidat key file di MinIO (misalnya perbandingan antara custom ID dengan ObjectId atau field `image`) sebelum menyimpulkan file tidak ada.
    - *Fallback Tangguh ke Dicebear WebP*: Jika file gambar fisik tidak ditemukan di MinIO, sistem tidak lagi mengembalikan 404 mentah, melainkan mengenerate avatar SVG Dicebear berdasarkan nama pengguna lalu mengonversinya secara dinamis ke buffer WebP menggunakan pustaka `sharp`.
  - **Backend Scheduler Service (`backend/src/services/scheduler.service.js`)**:
    - Membuat helper `normalizeTeamOfficers`: memastikan array `officer` pada setiap tim di rencana kerja scheduler selalu terisi `admin_id` yang valid (dikonversi dari MongoDB `_id` bila staf dipilih menggunakan ObjectId), diterapkan pada `getDraftTeamService`, `saveDraftTeamService`, dan `getScheduleByIdService`.
  - **Antarmuka Form Scheduler (`frontend/src/app/pages/activities/scheduler/`)**:
    - [`create.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/create.jsx), [`edit.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/edit.jsx), & [`detail.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/activities/scheduler/detail.jsx):
      - Sanitasi data petugas saat menyimpan draf atau jadwal kerja agar selalu mempertahankan identifier valid (`admin_id || id || _id`).
      - Memperbaiki rendering tag petugas dan passing key React (`key={adminId || offIdx}`) untuk mencegah duplikasi key warning.
  - **Komponen Tampilan Avatar & Tabel Frontend (`frontend/src/`)**:
    - [`frontend/src/components/ui/Avatar/Avatar.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/ui/Avatar/Avatar.jsx):
      - Menambahkan state `imageError` dan event handler `onError={() => setImageError(true)}` pada tag `<img>`. Jika gambar gagal di-load oleh browser, komponen otomatis beralih menampilkan inisial nama dengan warna latar belakang dinamis (`initialColor="auto"`), mencegah ikon *broken image* tampil pada halaman.
    - [`frontend/src/components/shared/table/rows.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/components/shared/table/rows.jsx):
      - `AdminLink` dan `PartyDisplay`: menambahkan prop `name={name}` ke komponen `Avatar` agar fallback inisial teks memiliki nama yang valid, serta memastikan URL request avatar hanya dibuat jika `id` valid (`isValidId`).
  - **Antarmuka Pengaturan Grup Telegram (`frontend/src/`)**:
    - [`frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/settings/sections/TelegramGroupsManager.jsx):
      - Menambahkan tipe topik tiket baru: `business` (`Tiket Bisnis`), `backbone` (`Tiket Backbone`), dan `backhaul` (`Tiket Backhaul`) ke dalam pemetaan objek `ticketTypeLabels`.
    - [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json) & [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json):
      - Mendaftarkan terjemahan bilingual untuk tipe topik baru tersebut.
- **Deskripsi Perubahan & Fungsi**:
  - Mengeliminasi secara menyeluruh kendala tampilan avatar pecah (*broken image*) dan request 404 pada tabel dan kartu profil pengguna.
  - Memastikan pembuatan tim kerja rencana lapangan (scheduler) berjalan mulus tanpa kegagalan identifikasi profil petugas, serta melengkapi opsi pemetaan notifikasi topik Telegram.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| Feature / #ISSUE | Sistem Manajemen Tata Letak Menu Sidebar Dinamis Global | Memberikan kendali penuh kepada admin untuk mengatur susunan menu sidebar secara visual (drag-and-drop hingga 3 tingkat hierarki), dengan sinkronisasi global, proteksi optimistic locking, dan pemulihan otomatis (auto-healing). |
| #360  | Pengendalian Batas Laju (Rate Limiting) & Ketahanan Gateway Pembayaran | Mencegah kegagalan pembukuan/rekonsiliasi transaksi pembayaran akibat pemblokiran HTTP 429 iPaymu melalui mekanisme jeda cerdas (*exponential backoff*, *full jitter*, *Retry-After* header, dan *batch chunking*). |
| #346  | Resiliensi Avatar (Dicebear WebP Fallback), Normalisasi Petugas Jadwal, & Topik Telegram | Menghilangkan ikon gambar pecah dan error 404 pada profil pengguna/staf, menstandarkan ID petugas rencana kerja, serta melengkapi opsi routing topik tiket Telegram untuk divisi Bisnis, Backbone, dan Backhaul. |

### Kemampuan Baru Pengguna/Admin

- **Kustomisasi Menu Sidebar Mandiri**: Admin dapat menambah grup menu baru, membuat sub-grup hierarkis, memindahkan item menu antar-grup, serta mengubah nama dan ikon grup langsung dari halaman Pengaturan tanpa rilis kode baru.
- **Pemulihan Cepat Susunan Default**: Admin dapat mengembalikan susunan menu ke konfigurasi standar pabrik kapan saja dengan sekali klik tombol "Reset ke Default".
- **Rekonsiliasi Keuangan Tanpa Batas**: Staf keuangan dapat menjalankan proses rekonsiliasi dan sapuan transaksi (*sweep*) secara massal dengan tenang, karena sistem secara otomatis mengatur ritme panggilan API ke gateway iPaymu agar tidak terkena blokir batas laju.
- **Tampilan Profil Pengguna yang Mulus**: Seluruh avatar admin, teknisi, mitra, dan pelanggan yang belum memiliki foto fisik akan langsung ditampilkan dalam bentuk avatar Dicebear WebP atau inisial nama yang elegan tanpa tampilan error visual.

### Bug Fix / Solusi Masalah

- **Struktur Menu Sidebar Kaku & Tersebar**: Menghapus duplikasi kode filter hak akses di 4 berkas layout berbeda dan menyatukannya ke dalam engine `useMenuTree` yang konsisten.
- **Error Pemblokiran HTTP 429 pada iPaymu**: Implementasi modul `callWithRetry.js` dan pemecahan antrean transaksi ke dalam ukuran *chunk* kecil (5 transaksi per batch) menyelesaikan kendala pemblokiran laju transaksi secara tuntas.
- **Avatar Pengguna Menghasilkan Error 404 & Broken Image**: Pengecekan multi-kandidat ID (ObjectId vs custom ID) di backend dan penambahan listener `onError` di frontend memastikan tidak ada lagi gambar profil yang gagal ditampilkan.
- **Petugas Lapangan Tak Terpilih pada Rencana Kerja Scheduler**: Normalisasi array petugas pada backend scheduler service menjamin data petugas tersimpan dan terbaca dengan ID seragam.

### Menu/Fitur Baru

- **Editor Susunan Menu Sidebar**: Menu baru di `Pengaturan > Aplikasi > Susunan Menu` (`/settings/menu-layout`) lengkap dengan pohon hierarki visual dan mekanisme drag-and-drop.
- **Dialog Pembuatan & Pemindahan Grup Menu**: Fitur interaktif untuk mengatur label dwibahasa dan memilih ikon grup menu.
- **Opsi Topik Tiket Bisnis, Backbone, & Backhaul**: Pilihan tipe topik baru pada konfigurasi routing notifikasi grup Telegram perusahaan.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

- **Penjelasan Fitur**: **Editor Tata Letak Menu Sidebar Dinamis Global**
  Fitur ini dirancang untuk memberikan fleksibilitas penuh kepada manajemen dan administrator sistem Dekasimal V2 dalam menyusun tata letak navigasi sidebar aplikasi. Susunan menu yang diatur berlaku secara global untuk seluruh staf dan bekerja secara konsisten pada mode tampilan *Main Layout* maupun *Sideblock*. Sistem mendukung hingga 3 tingkat hierarki (**Grup Menu Utama > Sub-Grup > Item Menu**). Pengaturan ini sepenuhnya aman: menu Pengaturan (*Settings*) diisolasi agar tidak dapat dipindahkan atau dikunci secara tidak sengaja, dan jika ada modul/item menu baru yang ditambahkan oleh developer di masa mendatang, sistem *auto-healing* akan otomatis menempatkannya ke dalam grup default "Lainnya" hingga admin mengaturnya kembali.

- **Langkah Penggunaan (Tutorial)**:
  1. Masuk ke aplikasi Dekasimal V2 sebagai Administrator yang memiliki hak akses pengaturan (`option.read` & `option.update`).
  2. Buka menu **Pengaturan** melalui sidebar atau pintasan profil, lalu pilih tab **Susunan Menu** (`/settings/menu-layout`).
  3. Pada halaman editor, Anda akan melihat susunan menu aktif dalam format pohon hierarki visual:
     - **Memindahkan Posisi**: Klik dan tahan ikon seret (*drag handle*) pada item atau grup yang ingin dipindah, lalu geser ke atas/bawah untuk mengubah urutan, atau geser ke dalam grup lain untuk menjadikannya sub-menu. Anda juga dapat menggunakan tombol titik tiga (...) lalu memilih **Pindahkan** untuk memilih grup tujuan melalui dialog dropdown.
     - **Membuat Grup Baru**: Klik tombol **Tambah Grup Menu** di bagian atas untuk grup tingkat utama, atau klik ikon **Tambah Sub-Grup** pada baris grup untuk membuat sub-kategori di dalamnya. Masukkan nama grup dalam Bahasa Indonesia dan Bahasa Inggris, lalu pilih ikon yang sesuai.
     - **Mengubah / Menghapus Grup**: Klik tombol aksi pada grup untuk mengganti nama/ikon atau menghapus grup. Saat grup dihapus, item-item menu di dalamnya akan secara aman dipindahkan ke grup penampung dan tidak akan hilang.
  4. Perhatikan tombol status di pojok kanan atas: jika terdapat perubahan yang belum disimpan, tombol **Simpan Perubahan** akan aktif dan menampilkan indikator perubahan draf.
  5. Klik tombol **Simpan Perubahan**. Sistem akan memvalidasi hierarki data dan menyimpannya ke server dengan proteksi versi *optimistic locking*.
  6. Bila sewaktu-waktu ingin membatalkan semua perubahan kustom dan kembali ke tata letak bawaan sistem, klik tombol **Reset ke Default** lalu konfirmasikan pada dialog yang muncul.
