# 📝 Daily Work Report - Dedy (2026-08-03)

---

## 📅 Laporan Harian - 3 Agustus 2026

---

## 🌿 Branch: `master` — Fitur Isolir Batch

### 📌 Informasi Issue

- **Nomor Issue**: #173
- **Judul Issue**: Fitur Isolir Batch (Pelajaran Modul Isolir Massal)
- **Status Branch**: `Sudah di-merge`

### 📅 Rincian Commit

#### [`6d24a06`](origin/master) - resolve #173 - 3 Agustus 2026 10:20

- **Komponen yang Berubah**:
  - `AGENTS.md`
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/controllers/isolirBatch.controller.js` [NEW]
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/isolirBatch.model.js` [NEW]
  - `backend/src/routes/attendanceDevice.route.js`
  - `backend/src/routes/isolirBatch.route.js` [NEW]
  - `backend/src/services/isolirBatch.service.js` [NEW]
  - `backend/src/services/radiusAuthentication.service.js`
  - `frontend/src/app/navigation/utilities.js`
  - `frontend/src/app/pages/utilities/attendanceDevice/schema/mappingColumns.jsx`
  - `frontend/src/app/pages/utilities/isolirBatch/create.jsx` [NEW]
  - `frontend/src/app/pages/utilities/isolirBatch/detail.jsx` [NEW]
  - `frontend/src/app/pages/utilities/isolirBatch/helpers.js` [NEW]
  - `frontend/src/app/pages/utilities/isolirBatch/index.jsx` [NEW]
  - `frontend/src/app/pages/utilities/isolirBatch/schema/columns.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/components/shared/form/Combobox.jsx`
  - `frontend/src/components/shared/table/rows.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - Membuat modul **Isolir Batch** yang memungkinkan Admin mengisolir (nonaktifkan akses internet) beberapa pelanggan sekaligus dalam satu aksi massal.
  - **Backend**: Membuat model `isolirBatch.model.js` untuk menyimpan data batch isolir, controller `isolirBatch.controller.js` untuk request handling, service `isolirBatch.service.js` untuk business logic, dan routes `isolirBatch.route.js` dengan privilege `isolirBatch.*`.
  - Menambahkan integrasi ke `radiusAuthentication.service.js` untuk melakukan disconnect session PPPoE/Hotspot pelanggan yang diisolir secara massal.
  - **Frontend**: Membuat halaman `index.jsx` (daftar batch), `create.jsx` (form pembuatan batch isolir baru), `detail.jsx` (detail hasil eksekusi batch), `helpers.js` (utilitas), dan `schema/columns.jsx` (konfigurasi kolom tabel).
  - Menambahkan `Combobox` reusable di `frontend/src/components/shared/form/Combobox.jsx` untuk input multi-select pelanggan.
  - Memperbarui navigasi `utilities.js` dan routing `protected.jsx` untuk menambahkan akses ke halaman Isolir Batch.
  - Menambahkan translation key i18n di kedua sisi (Bahasa Indonesia & Inggris).

---

## 🌿 Branch: `issue-184` — Integrasi AI Agent (Percakapan Cerdas dengan Data Nyata)

### 📌 Informasi Issue

- **Nomor Issue**: #184
- **Judul Issue**: Integrasi AI Agent — Percakapan Cerdas dengan Data Nyata
- **Status Branch**: `Belum di-merge`

### 📅 Rincian Commit

#### [`dc643f8`](HEAD) - resolve #184 - 3 Agustus 2026 17:59

- **Komponen yang Berubah**:
  - `backend/src/app.js`
  - `backend/src/config/privilege.json`
  - `backend/src/constants/aiAgent.constant.js`
  - `backend/src/controllers/aiAgent.controller.js`
  - `backend/src/controllers/settings.controller.js`
  - `backend/src/locales/en/translation.json`
  - `backend/src/locales/id/translation.json`
  - `backend/src/models/aiConversation.model.js` [NEW]
  - `backend/src/models/knowledgeBase.model.js` [NEW]
  - `backend/src/routes/aiAgent.route.js` [NEW]
  - `backend/src/services/aiAgent.service.js` [NEW]
  - `backend/src/services/aiConversation.service.js` [NEW]
  - `backend/src/services/appIndex.service.js` [NEW]
  - `backend/src/services/codeInspector.service.js` [NEW]
  - `backend/src/services/knowledgeBase.service.js` [NEW]
  - `backend/src/services/llmAdapter.service.js` [NEW]
  - `backend/src/services/option.service.js`
  - `docs/panduan-cara-menambahkan-pelanggan-baru-a03c4ac8.md` [NEW]
  - `frontend/src/app/navigation/settings.js`
  - `frontend/src/app/pages/settings/sections/AiAgent.jsx` [NEW]
  - `frontend/src/app/router/protected.jsx`
  - `frontend/src/app/router/settings/settingsRoute.jsx` [NEW]
  - `frontend/src/components/template/RightSidebar/Header.jsx`
  - `frontend/src/components/template/RightSidebar/MarkdownMessage.jsx` [NEW]
  - `frontend/src/components/template/RightSidebar/index.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
- **Deskripsi Perubahan & Fungsi**:
  - Membangun modul **AI Agent** penuh — asisten berbasis LLM (OpenAI GPT / Google Gemini) yang terintegrasi ke dalam aplikasi Dekasimal, memungkinkan Admin berinteraksi secara percakapan untuk mendapatkan panduan navigasi, penjelasan schema/model, dan bantuan prosedur.
  - **Backend — Service Layer**:
    - `aiAgent.service.js`: Orkestrator utama alur percakapan (chat), mengelola loop tool-calling, menjalankan tool `get_model_schema` (query schema MongoDB via Mongoose), `get_app_map` (navigasi menu), dan menyimpan hasil percakapan ke cache pengetahuan.
    - `llmAdapter.service.js`: Adapter universal untuk berbagai provider LLM (OpenAI, Gemini) dengan fallback otomatis dan handling streaming.
    - `appIndex.service.js`: Mengindeks seluruh file source code untuk menyediakan peta menu dan model kepada LLM.
    - `codeInspector.service.js`: Membaca dan menganalisis file kode secara dinamis untuk menjawab pertanyaan tentang implementasi teknis.
    - `knowledgeBase.service.js`: Menyimpan dan mengelola cache jawaban AI untuk mengurangi panggilan API LLM yang berulang.
    - `aiConversation.service.js`: Mengelola riwayat percakapan per sesi admin, termasuk summarization otomatis saat konteks terlalu panjang.
  - **Backend — Model**: `aiConversation.model.js` (riwayat chat), `knowledgeBase.model.js` (cache pengetahuan).
  - **Backend — Controller & Route**: `aiAgent.controller.js` (chat dan streaming SSE), `aiAgent.route.js` dengan privilege `aiAgent.*`.
  - **Frontend**: Halaman settings `AiAgent.jsx` untuk konfigurasi API key, model, temperature, max tokens, dan system prompt. Komponen `RightSidebar/index.jsx` (sidebar chat AI), `Header.jsx` (header sidebar), dan `MarkdownMessage.jsx` (rendering pesan markdown).

### ⚠️ Pekerjaan Belum Di-commit (Work In Progress)

Terdapat 13 file yang dimodifikasi dan 5 file baru yang belum di-commit di branch `issue-184`:

- **Komponen yang Berubah**:
  - `.gitignore`
  - `backend/src/constants/aiAgent.constant.js`
  - `backend/src/controllers/aiAgent.controller.js`
  - `backend/src/services/aiAgent.service.js`
  - `backend/src/services/aiConversation.service.js`
  - `backend/src/services/appIndex.service.js`
  - `backend/src/services/llmAdapter.service.js`
  - `backend/src/services/option.service.js`
  - `frontend/src/app/pages/settings/sections/AiAgent.jsx`
  - `frontend/src/components/template/RightSidebar/Header.jsx`
  - `frontend/src/components/template/RightSidebar/index.jsx`
  - `frontend/src/i18n/locales/en/translations.json`
  - `frontend/src/i18n/locales/id/translations.json`
  - `backend/src/services/endpointCatalog.service.js` [NEW]
  - `backend/src/services/selfApiClient.service.js` [NEW]
  - `backend/src/utils/summarize-list-payload.js` [NEW]
  - `backend/.admintest2.tmp.mjs`
  - `AI_AGENT_MAINTENANCE.md`
- **Deskripsi Perubahan & Fungsi**:
  - **Tool `get_module_data`**: Menambahkan tool baru yang memungkinkan AI Agent mengambil data nyata dari database lewat endpoint API yang sudah ada (self-call via HTTP internal). Ini adalah peningkatan signifikan — sebelumnya AI Agent hanya bisa menjawab berdasarkan pengetahuan umum, sekarang bisa menampilkan data spesifik seperti "tampilkan SO nomor X" atau "daftar tiket terbuka".
  - **`endpointCatalog.service.js`**: Service baru yang memindai seluruh route files secara otomatis, membangun katalog endpoint berdasarkan privilege yang terdaftar, dan menentukan kategori (LIST/DETAIL) serta parameter yang dibutuhkan. Hanya endpoint dengan privilege `.read`/`.list` yang masuk katalog; endpoint dengan efek samping (preview, send, approve, dll.) dikecualikan demi keamanan.
  - **`selfApiClient.service.js`**: HTTP client untuk memanggil backend sendiri ATAS NAMA admin yang sedang chat, membawa token JWT asli — sehingga `protectedAdmin` + `checkPrivilege` di endpoint tujuan berlaku otomatis tanpa modifikasi endpoint yang sudah ada.
  - **`summarize-list-payload.js`**: Utilitas untuk merangkum respons API berbentuk daftar menjadi ringkasan yang padat dan mudah dibaca LLM, membatasi jumlah item dan panjang string agar tidak melebihi batas token context.
  - **Custom Persona**: Admin kini bisa menyesuaikan nama persona AI (mis. "Bejo") dan karakter/gaya bicaranya melalui form di halaman Settings AI Agent. Nama persona ditampilkan di seluruh antarmuka (tooltip tombol, pesan selamat datang, identitas percakapan).
  - **Admin Identity Injection**: Identitas Admin (nama, posisi, divisi) otomatis disuntikkan ke system prompt sehingga AI Agent bisa menyapa Admin dengan namanya secara alami — bukan sebutan generik "Admin" berulang-ulang.
  - **Keamanan Error**: Error dari LLM tidak lagi membocorkan detail internal (MongoDB driver, stack trace, dsb) ke Admin; pola error yang tidak aman difilter secara otomatis.
  - **Sanitasi Jawaban**: Tool name (mis. `get_model_schema`, `get_module_data`) yang bocor ke teks jawaban dihapus secara selektif tanpa membuang data lain di kalimat yang sama.
  - **Cache Cerdas**: Jawaban yang mengandung data transaksional nyata (`get_module_data`) TIDAK disimpan ke cache karena datanya berubah setiap saat.
  - **Riwayat Percakapan Aman**: History chat dari server hanya dimuat ke state jika Admin belum mulai mengirim pesan — mencegah kehilangan pesan yang sedang di-stream.
  - **Pagination Cache**: Tabel knowledge cache di halaman Settings kini mendukung pagination (20 item per halaman) dengan kontrol navigasi sebelumnya/berikutnya.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul              | Dampak Utama                                                                             |
| ----- | ------------------ | ---------------------------------------------------------------------------------------- |
| #173  | Fitur Isolir Batch | Admin dapat mengisolir pelanggan secara massal dalam satu aksi                           |
| #184  | Integrasi AI Agent | AI Agent kini dapat mengambil data nyata dari database dan menyapa Admin secara personal |

### Kemampuan Baru Pengguna/Admin

- **Isolir Massal**: Admin dapat memilih beberapa pelanggan sekaligus dan menjalankan isolir (disconnect session PPPoE/Hotspot) dalam satu batch, menghemat waktu dibanding isolir satu per satu.
- **AI Agent dengan Data Nyata**: AI Agent tidak lagi terbatas pada panduan umum — Admin dapat meminta data spesifik seperti "tampilkan data SO nomor 123" atau "daftar tiket terbuka" dan AI akan mengambil data langsung dari database.
- **Custom Persona AI**: Admin dapat memberi nama dan karakter khusus pada AI Agent (mis. "Bejo, asisten yang ramah dan suka bercanda") untuk pengalaman percakapan yang lebih personal.
- **Sapaan Personal**: AI Agent kini menyapa Admin dengan nama aslinya berdasarkan data identitas di sistem.

### Bug Fix / Solusi Masalah

- **Error Bocoran Internal**: Detail teknis seperti nama driver MongoDB, stack trace, dan error code E-series tidak lagi terlihat oleh Admin saat terjadi kesalahan — diganti dengan pesan ramah pengguna.
- **Tool Name Bocor ke Teks**: Nama tool seperti `get_model_schema` yang kadang muncul di teks jawaban kini dibersihkan secara selektif tanpa menghapus data penting lain di kalimat yang sama.
- **Riwayat Chat Overwrite**: Mencegah riwayat percakapan dari server menimpa pesan yang sedang di-stream saat Admin mengirim pesan sebelum riwayat selesai dimuat.

### Menu/Fitur Baru

- **Menu Isolir Batch**: Menu baru di bawah navigasi Utilities untuk mengelola isolir massal pelanggan (create, detail, daftar).
- **Settings AI Agent**: Halaman konfigurasi AI Agent di Settings — termasuk pengaturan persona (nama & karakter), API key, model LLM, temperature, max tokens, dan knowledge cache.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### Fitur AI Agent dengan `get_module_data`

- **Penjelasan Fitur**: AI Agent kini memiliki kemampuan untuk mengambil data nyata dari database melalui tool `get_module_data`. Tool ini bekerja dengan memindai katalog endpoint yang sudah ada di backend, menentukan endpoint mana yang sesuai dengan permintaan Admin, lalu memanggil endpoint tersebut atas nama Admin yang sedang berinteraksi. Hanya endpoint dengan privilege `read`/`list` yang tersedia; endpoint yang memiliki efek samping (kirim notifikasi, approve, submit, dll.) dikecualikan demi keamanan.

- **Langkah Penggunaan (Tutorial)**:
  1. Klik ikon AI Agent (✨) di pojok kanan atas untuk membuka sidebar percakapan.
  2. Ketik pertanyaan yang membutuhkan data nyata, misalnya:
     - "Tampilkan data SO nomor 123"
     - "Daftar tiket terbuka saat ini"
     - "Siapa pelanggan dengan ID C-001?"
  3. AI Agent akan secara otomatis memanggil tool `get_module_data`, mencari endpoint yang sesuai, dan mengambil data dari database.
  4. Hasil data ditampilkan dalam format yang mudah dibaca langsung di chat.
  5. Jika endpoint tidak ditemukan atau Admin tidak memiliki hak akses, AI akan memberitahu secara terus terang tanpa mengarang data.

### Fitur Isolir Batch

- **Penjelasan Fitur**: Modul Isolir Batch memungkinkan Admin mengisolir (memutus akses internet) beberapa pelanggan sekaligus dalam satu aksi. Setelah batch dibuat, sistem akan secara otomatis melakukan disconnect session PPPoE/Hotspot untuk setiap pelanggan yang terdaftar dalam batch.

- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **Utilities → Isolir Batch** dari sidebar navigasi.
  2. Klik tombol **Buat Batch Baru** untuk membuka form pembuatan.
  3. Pilih pelanggan yang ingin diisolir menggunakan field multi-select (Combobox).
  4. Isi keterangan/alasan isolir (opsional).
  5. Klik **Submit** untuk menjalankan batch isolir.
  6. Halaman detail akan menampilkan hasil eksekusi — pelanggan mana yang berhasil diisolir dan mana yang gagal (dengan alasan).
  7. Daftar semua batch yang pernah dibuat dapat dilihat di halaman index.
