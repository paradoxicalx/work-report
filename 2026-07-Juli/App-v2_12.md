# 📝 Daily Work Report - Dedy Putra (2026-07-12)

---

## 📅 Laporan Harian - 12 Juli 2026

---

## 🌿 Branch: `issue-137` — Modul Dokumen PIR (Post Incident Report)

### 📌 Informasi Issue
- **Nomor Issue**: #137
- **Judul Issue**: Pembuatan Modul Baru Dokumen PIR (Post Incident Report)
- **Status Branch**: `Belum di-merge` *(dalam pengerjaan — WIP)*

---

### 📅 Rincian Pekerjaan Hari Ini

> ⚠️ Belum di-commit. Pekerjaan hari ini masih dalam bentuk perubahan lokal (unstaged) di atas commit `d6355c8` (save #137 dari kemarin).
>
> **Statistik perubahan:** `8 files changed, 494 insertions(+), 67 deletions(-)`

---

#### 🔧 Pekerjaan Belum Di-commit (WIP — 12 Juli 2026)

- **Komponen yang Berubah**:
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/ticket.controller.js`
  - `backend/src/services/ticket.service.js`
  - `backend/src/utils/generateTicketReport.js` *(diperluas signifikan)*
  - `frontend/src/app/pages/tickets/partner/PartnerReport.jsx` *(diperluas signifikan)*
  - `frontend/src/app/pages/tickets/partner/close.jsx`
  - `frontend/src/app/pages/tickets/partner/schema/closeSchema.js`
  - `frontend/src/i18n/locales/id/translations.json`

---

- **Deskripsi Perubahan & Fungsi**:

  **Backend — `generateTicketReport.js` (Kalkulasi Metrik PIR):**
  Penambahan fungsi kalkulasi metrik PIR yang kompleks pada utility `generateTicketReport`. Logika baru yang ditambahkan:

  - **Fungsi `getPendingDuration(messages, resolvedAt, closedAt)`**: Fungsi kalkulator durasi *pending* yang canggih. Bekerja dengan:
    1. Menelusuri seluruh riwayat pesan tiket secara kronologis
    2. Mengidentifikasi pasangan event `suspend` → `resume` untuk membentuk interval waktu pending
    3. Jika tiket masih suspended saat ditutup, interval dihitung hingga waktu `closedAt`
    4. Menambahkan interval tambahan dari `resolved_time` → `closedAt` (waktu antara selesai dan tutup tiket)
    5. Melakukan **merge interval yang overlap** agar tidak ada perhitungan ganda
    6. Menjumlahkan total durasi semua interval (dalam milidetik)

  - **Kalkulasi Metrik PIR pada Penutupan Tiket Partner**:
    - `pending_duration_ms` — Total durasi tiket dalam kondisi suspended (di-hold)
    - `incident_duration_ms` — Durasi total kejadian dari waktu insiden (`time_of_incident` atau `created_at`) sampai waktu resolved
    - `ticket_duration_ms` — Durasi total tiket dari dibuat hingga ditutup
    - `mttr_ms` (**Mean Time To Repair**) — Waktu perbaikan efektif = (resolved_time - time_of_incident) dikurangi total durasi pending
    - `root_cause`, `corrective_action`, `preventive_action` — Field analisis insiden disimpan ke database saat menutup tiket

  - **Integrasi `ticket` object**: Controller dan service diperbarui agar meneruskan data tiket lengkap (termasuk `messages`) ke fungsi `generateTicketReport` supaya kalkulasi interval pending dapat dilakukan dengan akurat.

  **Backend — `ticket.service.js`**: Penambahan field `messages` ke dalam populate/select sehingga data riwayat pesan tiket tersedia di controller untuk diteruskan ke `generateTicketReport`.

  **Backend — `ticket.controller.js`**: Penambahan passing `ticket` object ke fungsi `generateTicketReport` agar kalkulasi metrik PIR dapat mengakses `messages`, `time_of_incident`, dan `created_at`.

  ---

  **Frontend — `close.jsx` (Form Penutupan Tiket Partner — Laporan PIR):**
  Perombakan halaman penutupan tiket partner untuk mendukung pengisian laporan PIR secara lengkap:

  - **Field baru `resolved_time` (Waktu Terselesaikan)**: Input `InputDatePicker` dengan format `Y-m-d H:i` dan `time_24hr: true` ditambahkan di bagian **Laporan Kejadian**
  - **Field baru `root_cause` (Analisis Akar Masalah)**: Textarea untuk mengisi akar penyebab insiden
  - **Field baru `corrective_action` (Tindakan Perbaikan)**: Textarea untuk mengisi tindakan korektif yang sudah dilakukan
  - **Field baru `preventive_action` (Tindakan Pencegahan)**: Textarea untuk mengisi tindakan preventif agar insiden tidak terulang
  - **Pemisahan seksi**: Formulir kini dibagi menjadi dua seksi terpisah: "**Laporan Kejadian**" (berisi field-field PIR) dan "**Keterangan**" (catatan umum/worknotes)
  - **`ticketData` state**: Fetch data tiket (`time_of_incident`) saat komponen mount dan disimpan ke state, lalu diteruskan sebagai `context` ke Yup resolver untuk validasi silang antar-field

  **Frontend — `closeSchema.js` (Validasi Form Penutupan):**
  Penambahan aturan validasi Yup untuk field-field PIR baru:

  - **`resolved_time`**:
    - Test `is-after-incident`: Waktu terselesaikan **tidak boleh kurang** dari `time_of_incident` (validasi silang menggunakan Yup context)
    - Test `is-not-future`: Waktu terselesaikan **tidak boleh melebihi** waktu sekarang
  - **`root_cause`**, **`corrective_action`**, **`preventive_action`**: Didaftarkan sebagai string nullable (opsional)

  **Frontend — `PartnerReport.jsx` (Halaman Laporan / Preview Dokumen PIR):**
  Penambahan logika kalkulasi dan tampilan metrik PIR secara langsung di halaman laporan tiket partner (preview dokumen):

  - **Fungsi `getPendingDuration()`**: Implementasi ulang kalkulasi pending duration di sisi frontend (sama dengan backend) agar laporan tetap dapat dihitung secara real-time dari data tiket yang ada, tidak bergantung penuh pada nilai tersimpan di database
  - **Fungsi `formatDuration(ms)`**: Fungsi formatter durasi dari milidetik menjadi teks human-readable, misalnya `"2 hari 3 jam 15 menit"`
  - **Kalkulasi semua metrik PIR** ditampilkan di preview dokumen:
    - `pendingDurationMs` — menggunakan nilai dari DB jika sudah tersimpan, fallback ke kalkulasi real-time
    - `incidentDurationMs` — durasi dari `time_of_incident` / `created_at` hingga `resolved_time`
    - `ticketDurationMs` — durasi total tiket dari dibuat hingga ditutup
    - `mttrMs` — MTTR efektif setelah dikurangi durasi pending
  - **Label baru** pada dokumen PIR yang ditampilkan: MTTR, Waktu Tiket Dibuat, Waktu Tiket Ditutup, Waktu Kejadian, Waktu Terselesaikan, Durasi Pending, Durasi Kejadian

  **Frontend — `translations.json` (Bahasa Indonesia):**
  Penambahan key terjemahan baru untuk antarmuka PIR:

  - `ticket.report.mttr` → "MTTR (Mean Time To Repair)"
  - `ticket.report.ticketCreatedAt` → "Waktu Tiket Dibuat"
  - `ticket.report.ticketClosedAt` → "Waktu Tiket Ditutup"
  - `ticket.report.timeOfIncident` → "Waktu Kejadian"
  - `ticket.report.resolvedTime` → "Waktu Terselesaikan"
  - `ticket.report.pendingDuration` → "Durasi Pending"
  - `ticket.report.incidentDuration` → "Durasi Kejadian"
  - `form.rootCause` → "Analisis Akar Masalah"
  - `form.correctiveAction` → "Tindakan Perbaikan"
  - `form.preventiveAction` → "Tindakan Pencegahan"
  - `form.resolvedTime` → "Waktu Terselesaikan"
  - `form.incidentReport` → "Laporan Kejadian"
  - `form.invalidIncidentTime` → "Waktu tidak boleh kurang dari waktu kejadian"
  - `form.invalidFutureResolvedTime` → "Waktu terselesaikan tidak boleh melebihi waktu sekarang"

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
|-------|-------|--------------|
| #137  | Modul PIR (Post Incident Report) | Sistem kini dapat menghitung dan menyimpan metrik insiden otomatis (MTTR, durasi kejadian, durasi pending) saat tiket partner ditutup |

### Kemampuan Baru Pengguna/Admin

- **Admin/Operator**: Saat menutup tiket partner, kini dapat mengisi **Laporan Kejadian (PIR)** secara lengkap: Waktu Terselesaikan, Analisis Akar Masalah, Tindakan Perbaikan, dan Tindakan Pencegahan.
- **Admin/Operator**: Dokumen laporan tiket partner kini menampilkan metrik insiden secara otomatis: **MTTR**, **Durasi Kejadian**, **Durasi Pending**, dan **Durasi Total Tiket** — dihitung dari data riwayat tiket.
- **Sistem**: Metrik PIR tersimpan secara permanen ke database saat tiket ditutup (`pending_duration_ms`, `incident_duration_ms`, `ticket_duration_ms`, `mttr_ms`).

### Bug Fix / Solusi Masalah

- Validasi silang antar-field tanggal PIR: sistem kini mencegah input `resolved_time` yang lebih awal dari `time_of_incident` atau yang melebihi waktu saat ini.
- Algoritma merge interval overlap pada kalkulasi pending duration mencegah penghitungan ganda jika interval suspend/resume saling bertumpuk.

### Menu/Fitur Baru

- **Form Penutupan Tiket Partner** — Seksi baru "Laporan Kejadian" berisi field: Waktu Terselesaikan, Analisis Akar Masalah, Tindakan Perbaikan, Tindakan Pencegahan.
- **Dokumen PIR (PartnerReport)** — Preview laporan tiket partner kini menampilkan tabel metrik insiden lengkap dengan MTTR dan durasi-durasi yang dihitung otomatis.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 🎯 Fitur Utama Hari Ini: Kalkulasi Metrik PIR & Form Penutupan Tiket Partner

**Penjelasan Fitur**:
Saat tiket partner ditutup, sistem kini secara otomatis menghitung empat metrik insiden kunci dan menyimpannya ke database: (1) **MTTR** — waktu efektif perbaikan setelah dikurangi periode pending, (2) **Durasi Kejadian** — lamanya insiden berlangsung dari waktu terjadi hingga selesai, (3) **Durasi Pending** — total waktu tiket berada dalam status on-hold dihitung dari riwayat pesan suspend/resume, (4) **Durasi Tiket** — total umur tiket dari dibuat hingga ditutup. Metrik ini juga tampil secara real-time di halaman preview dokumen PIR.

**Langkah Penggunaan (Tutorial)**:

**Untuk Menutup Tiket Partner dengan Mengisi Laporan PIR:**
1. Login ke Frontend sebagai Admin atau Operator
2. Navigasi ke **Tiket → Partner** dan buka tiket yang akan ditutup
3. Klik tombol **Tutup Tiket / Close Ticket**
4. Pada seksi **"Laporan Kejadian"**, isi:
   - **Waktu Terselesaikan** — pilih tanggal & jam kapan insiden berhasil diperbaiki *(tidak boleh sebelum waktu kejadian atau melebihi waktu sekarang)*
   - **Analisis Akar Masalah** — jelaskan penyebab utama insiden
   - **Tindakan Perbaikan** — jelaskan langkah-langkah yang sudah diambil untuk memperbaiki
   - **Tindakan Pencegahan** — jelaskan langkah untuk mencegah insiden serupa di masa depan
5. Isi seksi **"Keterangan"** dengan catatan umum penutupan
6. Lengkapi data teknisi, peralatan, dan upload file bukti
7. Submit — sistem otomatis menghitung dan menyimpan semua metrik PIR ke database

**Untuk Melihat Dokumen PIR / Laporan Tiket Partner:**
1. Buka detail tiket partner yang sudah ditutup
2. Klik tab atau tombol **"Laporan"** / **"Dokumen PIR"**
3. Dokumen menampilkan tabel metrik insiden dengan nilai yang dihitung otomatis:
   - MTTR, Durasi Kejadian, Durasi Pending, Durasi Total Tiket
4. Dokumen dapat dicetak atau diexport sesuai kebutuhan
