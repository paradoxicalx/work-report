# 📝 Daily Work Report - Dedy Putra (2026-06-07)

---

## 📅 Laporan Harian - 7 Juni 2026

> [!NOTE]
> Seluruh pembaruan kode yang telah di-commit secara resmi di branch ini (commit `387b4a8` dan `a89b7b3`) telah berhasil dideploy dan dijalankan di lingkungan produksi (**production**).

### 🛠️ Pekerjaan Belum Di-commit / Work in Progress (WIP)

- `[backend/src/controllers/document.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/document.controller.js)`
  - **Deskripsi**: Menambahkan controller `getBAD` untuk mengambil data draft dokumen BAD (Berita Acara Penghentian Layanan) dengan memuat nomor dokumen terformat dinamis, detail identitas pelanggan, data teknis layanan, serta daftar perangkat yang dilepas/diambil dari tiket dismantle terkait.
- `[backend/src/controllers/publicDocument.controller.js](file:///d:/Project/DEKASIMAL_V2/backend/src/controllers/publicDocument.controller.js)`
  - **Deskripsi**: Menambahkan controller `updateBADByCode` untuk menyimpan tanda tangan digital pelanggan pada dokumen BAD bertipe `'BAD'` dan menyimpannya secara permanen ke MongoDB dengan nama berkas template `"BAD Penghentian Layanan [Nama Pelanggan]"`.
- `[backend/src/routes/document.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/document.route.js)`
  - **Deskripsi**: Mendaftarkan rute internal `/documents/bad/read/:ticketId` untuk membaca dokumen BAD dengan hak akses `ticketDismantle.read` serta menyertakan Swagger JSDoc.
- `[backend/src/routes/public.route.js](file:///d:/Project/DEKASIMAL_V2/backend/src/routes/public.route.js)`
  - **Deskripsi**: Mendaftarkan rute publik `/public-docs/bad/:ticketId` dan `/public-docs/bad/update` untuk halaman penandatanganan pelanggan tanpa autentikasi beserta Swagger JSDoc-nya.
- `[frontend/src/components/shared/DocumentPreviewModal.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/components/shared/DocumentPreviewModal.jsx)`
  - **Deskripsi**: Mengintegrasikan pratinjau visual BAD (tipe `'bad'`) di dalam modal pratinjau dokumen dan membenahi tombol share agar selalu aktif pada dokumen yang tidak membutuhkan approval (seperti BAP dan BAD).
- `[frontend/src/app/pages/tickets/dismantle/detail.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/tickets/dismantle/detail.jsx)`
  - **Deskripsi**: Menambahkan tombol "Cetak Berita Acara Penghentian Layanan" pada panel rincian tiket dismantle yang memicu penampilan modal pratinjau dokumen BAD.
- `[frontend/src/app/pages/tickets/dismantle/components/BADDocumentPreview.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/tickets/dismantle/components/BADDocumentPreview.jsx)` [NEW]
  - **Deskripsi**: Membuat komponen visual dokumen cetak BAD (Berita Acara Penghentian Layanan) yang memuat kop surat perusahaan, nomor surat, identitas para pihak, daftar perangkat yang dilepas, pernyataan serah terima perangkat/layanan, serta kolom tanda tangan digital.
- `[frontend/src/app/pages/public/PublicBADDocument.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/pages/public/PublicBADDocument.jsx)` [NEW]
  - **Deskripsi**: Membuat halaman publik responsive untuk penandatanganan digital dokumen BAD oleh pelanggan dengan tanda tangan berbasis kanvas (`DrawerSign`).
- `[frontend/src/app/router/public.jsx](file:///d:/Project/DEKASIMAL_V2/frontend/src/app/router/public.jsx)`
  - **Deskripsi**: Mendaftarkan rute publik `/p/bad/:code` ke halaman penandatanganan `PublicBADDocument` secara lazy-loaded.
- `[frontend/src/i18n/locales/id/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/id/translations.json)`
  - **Deskripsi**: Memperbarui teks pelabelan dan terjemahan lokalisasi Bahasa Indonesia agar secara konsisten merujuk ke nama dokumen `"Berita Acara Penghentian Layanan"`.
- `[frontend/src/i18n/locales/en/translations.json](file:///d:/Project/DEKASIMAL_V2/frontend/src/i18n/locales/en/translations.json)`
  - **Deskripsi**: Memperbarui teks pelabelan dan terjemahan lokalisasi Bahasa Inggris agar secara konsisten merujuk ke nama dokumen `"Service Termination Document"`.

---

### 📅 Rincian Commit

#### [387b4a8] & [a89b7b3] - resolve #98

- **Komponen yang Berubah**:
  - `frontend/src/app/pages/tickets/other/index.jsx` [NEW]
  - `frontend/src/app/pages/tickets/other/create.jsx` [NEW]
  - `frontend/src/app/pages/tickets/other/detail.jsx` [NEW]
  - `frontend/src/app/pages/tickets/other/edit.jsx` [NEW]
  - `frontend/src/app/pages/tickets/other/close.jsx` [NEW]
  - `frontend/src/app/pages/tickets/other/OtherReport.jsx` [NEW]
  - `frontend/src/app/pages/tickets/other/schema/columns.jsx` [NEW]
  - `frontend/src/app/pages/tickets/other/schema/createSchema.js` [NEW]
  - `frontend/src/app/pages/tickets/other/schema/closeSchema.js` [NEW]
  - `frontend/src/app/router/tickets/otherRoute.jsx` [NEW]
  - `frontend/src/app/navigation/tickets.js`
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/i18n/locales/id/translations.json`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/app/pages/tickets/backbone/BackboneReport.jsx`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/ticket.controller.js`
  - `backend/src/services/document.service.js`
  - `backend/src/utils/generateTicketReport.js`
  - `backend/src/utils/setAdminPoint.js`

- **Deskripsi Perubahan & Fungsi**:
  - **Menu Tiket Lainnya (Other)**: Membuat submodul baru lengkap untuk tiket jenis `other` (lainnya), mulai dari halaman daftar, form pembuatan, pengeditan, rincian detail tiket, formulir penutupan tiket (tanpa parameter hasil ukur), hingga komponen visualisasi laporan penutupan. Menghubungkan pembacaan data laporan tiket lainnya ke field `maintenance_report` di database.
  - **Tiket Backbone**: Mengubah pembacaan field data laporan tiket backbone di frontend ke properti `maintenance_report` (sebelumnya `backbone_report`) agar selaras dengan skema default database.
  - **Sistem Pembagian Poin Admin (Backend)**: Membenahi logika `setAdminPoint` untuk mengintegrasikan pembagian poin secara dinamis berdasarkan formula `max`, `min`, `base`, dan `dropTime` yang dihitung secara adil dibagi rata ke seluruh teknisi pelaksana. Menambahkan validasi protektif (`?.`) untuk menghindari kegagalan eksekusi (unhandled promise rejection).
  - **Integrasi Menu**: Mendaftarkan tautan navigasi menu Tiket Lainnya di sidebar utama dan mendaftarkan konfigurasi routing frontend dengan proteksi privilege.
