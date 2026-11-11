# 📝 Daily Work Report - Dedy S.N Putra (2026-10-06)

---

## 📅 Laporan Harian - 6 Oktober 2026

> Laporan ini merangkum seluruh aktivitas rekayasa perangkat lunak, integrasi alur kerja operasional, orkestrasi asisten kecerdasan buatan (AI), dan pembuatan dokumen pelanggan pada monorepo DEKASIMAL V2 per 6 Oktober 2026. Fokus dan pencapaian utama hari ini berpusat pada:
> 1. **Integrasi Tiket & Asisten AI dari Obrolan WhatsApp (`issue-373`)**: Pembangunan arsitektur terpadu berupa *Drawer Tiket* multi-tab langsung di dalam panel Obrolan WhatsApp (*Chat Header*), menghilangkan friksi perpindahan halaman bagi admin layanan pelanggan (*Customer Service* & *Helpdesk*).
> 2. **Otomasi Draf Tiket & Rangkuman Komentar berbasis AI**: Pemanfaatan *LLM Adapter* untuk membaca konteks obrolan pelanggan secara aman, menyusun draf form tiket lengkap (kategori *Pelanggan* dan *Lainnya*), merangkum pesan-pesan obrolan terpilih menjadi draf komentar tiket, dengan mitigasi *prompt injection*, perlindungan data sensitif (*censorForAdmin*), dan pembatasan laju (*rate limiting*) in-memory.
> 3. **Pembuat & Pratinjau Dokumen PDF Vektor Laporan Tiket (`jspdf` & `react-pdf`)**: Transisi dari teks laporan bebas menjadi dokumen laporan resmi berbentuk PDF vektor (teks asli yang dapat dicari/diseleksi, bukan gambar raster) dengan tata letak korporat profesional (kop surat, logo perusahaan, tabel detail tiket bergaris, bagian hasil pekerjaan, daftar catatan penanganan bernomor, serta footer dinamis "Halaman X dari Y"). Dokumen diverifikasi melalui komponen pratinjau terisolasi sebelum dikirimkan ke pelanggan via WhatsApp bersama pesan pengantar (*caption*) dalam satu kiriman lampiran.
> 4. **Penjaminan Kualitas & Pengujian Otomatis Menyeluruh**: Penulisan rangkaian pengujian otomatis lengkap mencakup unit test utilitas AI & laporan, integrasi controller/service backend, pengujian generator PDF, dan pengujian komponen/hook frontend dengan 100% kelulusan (43 test backend & 46 test frontend).

---

## 🌿 Branch: `issue-373` — Tiket & Asisten AI dari Obrolan WhatsApp

### 📌 Informasi Issue

- **Nomor Issue**: #373
- **Judul Issue**: Tiket & Asisten AI dari Obrolan WhatsApp (Drawer Tiket Multi-Tab, Draf AI Form Tiket, Rangkuman Pesan Obrolan ke Komentar Tiket, Generator & Pratinjau Laporan PDF Vektor untuk Pelanggan)
- **Status Branch**: `Belum di-merge` (Branch aktif lokal `issue-373`, 28 commits terverifikasi, 89 test suite lulus 100%, siap untuk peninjauan dan penggabungan ke master)

### 📅 Rincian Commit

#### [[`cfadbce3`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: rapikan format Prettier blok terjemahan revisi tiket obrolan - 6 Oktober 2026, 20:06:20 WIB
#### [[`4b5889f8`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: perbarui aturan AGENTS untuk laporan tiket berbentuk PDF - 6 Oktober 2026, 20:06:01 WIB
#### [[`13052c21`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: ukur lebar pratinjau PDF lewat komponen sendiri, kunci pilihan tiket saat sibuk, beri batas waktu unduh logo, dan samakan hitungan keterangan - 6 Oktober 2026, 20:03:38 WIB
#### [[`4e6f87ed`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tab tiket selesai membuat PDF, menampilkan pratinjau, dan mengirimnya sebagai lampiran - 6 Oktober 2026, 20:00:10 WIB
#### [[`06cf6f05`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: cegah judul bagian tertinggal di dasar halaman PDF, jaga angka pecahan dan tab, dan pertahankan rasio logo - 6 Oktober 2026, 19:56:33 WIB
#### [[`7f639ef7`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah pembuat PDF vektor laporan tiket dengan tata letak dokumen profesional - 6 Oktober 2026, 19:51:31 WIB
#### [[`f4255160`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah pilih semua pesan dan daftar tiket satu kotak dengan ikon buka tiket di tab terbuka - 6 Oktober 2026, 19:47:13 WIB
#### [[`db62299f`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: judul per tab, pemilih jenis Listbox, footer AI dan Simpan, serta kolom lengkap tiket pelanggan - 6 Oktober 2026, 19:43:43 WIB
#### [[`5e8697bf`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah logika pilih semua, syarat kirim PDF, dan hook kirim lampiran laporan - 6 Oktober 2026, 19:40:02 WIB
#### [[`088475d7`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah detail daftar-putih pada daftar tiket selesai untuk PDF pelanggan - 6 Oktober 2026, 19:36:18 WIB
#### [[`9a0f13e2`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: hapus alur laporan teks dan AI laporan dari backend tiket obrolan - 6 Oktober 2026, 19:33:09 WIB
#### [[`50910b68`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah spec desain tiket dan asisten AI dari obrolan WhatsApp - 6 Oktober 2026, 17:04:21 WIB
#### [[`171518a3`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: rapikan format Prettier blok terjemahan tiket obrolan - 6 Oktober 2026, 17:02:37 WIB
#### [[`6854e7ef`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: pertahankan pemisah item daftar dan lewati tanggal laporan tak valid - 6 Oktober 2026, 17:02:37 WIB
#### [[`c3af215c`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah test pencegahan spoofing asal obrolan, gerbang Baileys, dan kuota AI - 6 Oktober 2026, 17:02:37 WIB
#### [[`64e663cb`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: dokumentasikan aturan tiket dan asisten AI dari obrolan WhatsApp di AGENTS.md - 6 Oktober 2026, 14:27:00 WIB
#### [[`cbe53a1f`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah tab tiket selesai untuk pratinjau dan kirim laporan ke pelanggan - 6 Oktober 2026, 14:18:29 WIB
#### [[`96bb8ccd`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah tab tiket terbuka untuk komentar dari pesan obrolan terpilih - 6 Oktober 2026, 14:13:41 WIB
#### [[`1208d8b6`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: reset form setelah tiket dibuat dan ambil status AI tanpa syarat hak baca tiket - 6 Oktober 2026, 14:10:38 WIB
#### [[`6695bb60`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah drawer tiket di header obrolan dengan tab buat tiket dan draf AI - 6 Oktober 2026, 14:06:15 WIB
#### [[`da6756d2`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah logika murni, hook API, privilege, dan i18n drawer tiket obrolan - 6 Oktober 2026, 14:03:00 WIB
#### [[`1f1dc967`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah controller dan route tiket dari obrolan WhatsApp (draf AI, daftar, komentar, laporan) - 6 Oktober 2026, 13:58:47 WIB
#### [[`ace2fd7f`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: jalur komentar tanpa AI mempertahankan tanda sudut (escape oleh textToHtml) - 6 Oktober 2026, 13:55:49 WIB
#### [[`b1f80757`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah servis waTicket (draf AI, daftar tiket, draf komentar, pratinjau laporan) - 6 Oktober 2026, 13:51:39 WIB
#### [[`d5bdb094`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah penaut wa_conversation/wa_id pada tiket dan validasi asal di createTicket - 6 Oktober 2026, 13:48:13 WIB
#### [[`c99135a0`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah util murni laporan tiket dan penyaring komentar manusia - 6 Oktober 2026, 13:45:07 WIB
#### [[`ad1429d1`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: perkuat penyaring pembatas prompt dan tahan timestamp tidak valid - 6 Oktober 2026, 13:42:32 WIB
#### [[`7b7e4183`](file:///home/dhedhy/Project/Dekasimal-V2)] - save #373: tambah util murni AI untuk tiket dari obrolan WhatsApp - 6 Oktober 2026, 13:40:07 WIB

- **Komponen yang Berubah**:
  - **Dokumentasi & Standar Arsitektur Monorepo (`AGENTS.md` & `docs/`)**:
    - [`AGENTS.md`](file:///home/dhedhy/Project/Dekasimal-V2/AGENTS.md):
      - Penambahan aturan baku Bab 2.A nomor 22: Standar Tiket & Asisten AI dari Obrolan WhatsApp (`waTicket.service.js`, drawer `TicketDrawer.jsx`).
      - Menetapkan 7 sub-aturan ketat:
        - (a) AI dipanggil satu kali jalan (*one-shot*) tanpa tool/function calling melalui `llmAdapter`, transkrip disensor `censorForAdmin` dan dibungkus tag data `<transkrip>`, validasi struktural JSON wajib, zero-logging untuk transkrip/prompt/output.
        - (b) Tiket mencatat jejak `wa_conversation` dan nomor kanonik `wa_id` yang diturunkan oleh server; pencarian tiket relasional berbasis record pelanggan atau `wa_id` lintas sesi.
        - (c) Laporan pelanggan wajib berupa PDF vektor murni (teks riil yang dapat diseleksi/dicari via `jspdf`, bukan screenshot raster); hanya mengekspos field daftar-putih tanpa nama teknisi, biaya, perangkat internal, lampiran, atau CC.
        - (d) Komentar manusia pada tiket disaring melalui daftar kunci resmi `HUMAN_COMMENT_KEYS` dan dirujuk berdasarkan indeks aslinya.
        - (e) Pengiriman PDF memanfaatkan endpoint lampiran obrolan yang sudah ada (`POST .../attachment`) dengan keterangan teks maksimal 1.024 karakter.
        - (f) Validasi hak akses dinamis berbasis tipe tiket (`ticket.customer.*`, `ticket.other.*`), tiket tidak berhak dijawab HTTP 404.
        - (g) Pembatasan kuota AI in-memory (10 panggilan/menit/admin).
    - [`docs/superpowers/specs/2026-10-06-wa-chat-ticket-ai-design.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-10-06-wa-chat-ticket-ai-design.md) [NEW]: Spesifikasi teknis awal arsitektur tiket dan asisten AI dari obrolan WhatsApp.
    - [`docs/superpowers/specs/2026-10-06-wa-chat-ticket-ai-revisi-1.md`](file:///home/dhedhy/Project/Dekasimal-V2/docs/superpowers/specs/2026-10-06-wa-chat-ticket-ai-revisi-1.md) [NEW]: Spesifikasi teknis revisi antarmuka pengguna drawer, transisi laporan akhir ke PDF vektor profesional dengan pratinjau langsung, dan pembersihan endpoint teks redundan.
  - **Skema Model Data & Konstanta Backend (`backend/src/models/` & `backend/src/constants/`)**:
    - [`backend/src/models/ticket.model.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/models/ticket.model.js):
      - Penambahan field `wa_conversation` (`ObjectId`, ref `wa_conversations`, indeks *sparse*) untuk melacak asal percakapan pembuatan tiket.
      - Penambahan field `wa_id` (`String`, nomor kanonik internasional E.164, indeks *sparse*) untuk mengaitkan tiket ke identitas nomor WhatsApp pengirim.
    - [`backend/src/constants/waTicket.constant.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/constants/waTicket.constant.js) [NEW]:
      - Definisi tipe tiket yang didukung (`customer`, `other`), kuota batasan laju AI (10 request/menit), batas maksimal seleksi pesan (50 pesan), daftar-putih kunci komentar manusia `HUMAN_COMMENT_KEYS` (`admin_id`, `message`, `date`, `_id`), dan daftar-putih field laporan `TICKET_REPORT_WHITELIST_FIELDS`.
  - **Utilitas Murni, Keamanan, & Adapter AI Backend (`backend/src/utils/`)**:
    - [`backend/src/utils/waTicketAi.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waTicketAi.js) [NEW]:
      - Modul prompt engineering dan validasi LLM:
        - `buildTicketDraftPrompt`: Menyusun prompt instruksi ekstraksi entitas tiket (topik, prioritas, deskripsi masalah, nomor alternatif) dari transkrip obrolan.
        - `buildCommentDraftPrompt`: Menyusun prompt perangkuman pesan obrolan terpilih menjadi draf komentar teknis ringkas dan padat.
        - `formatTranscriptForAi`: Mengompilasi pesan obrolan, menjalankan sensor kata kasar `censorForAdmin`, dan membungkus transkrip dalam tag pembatas data `<transkrip>` dengan penangkal injeksi prompt (menghapus tag penutup tiruan `</transkrip>`).
        - `validateDraft` & `validateCommentDraft`: Validator skema keluaran JSON dari AI untuk menjamin integritas data sebelum disajikan ke antarmuka formulir.
        - Rate Limiter in-memory: Menghitung frekuensi pemanggilan AI per ID admin dalam jendela geser (*sliding window*) 60 detik.
    - [`backend/src/utils/waTicketAccess.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waTicketAccess.js) [NEW]:
      - Pengecekan otorisasi privilege tiket granular per tipe (`ticket.customer.create`, `ticket.customer.read`, `ticket.customer.update`, `ticket.other.*`), validasi keanggotaan channel WhatsApp (Meta/Baileys), dan validasi jendela balasan pesan 24 jam.
    - [`backend/src/utils/waTicketReport.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/utils/waTicketReport.js) [NEW]:
      - Utilitas pemfilteran komentar manusia murni `filterHumanComments` (memisahkan komentar staf dari log status sistem otomatis) dan kurasi detail laporan tiket.
  - **Layanan Bisnis Backend (`backend/src/services/`)**:
    - [`backend/src/services/waTicket.service.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/services/waTicket.service.js) [NEW]:
      - `generateTicketDraft`: Membaca 50 pesan terakhir percakapan, mengeksekusi panggilan AI sekali-jalan tanpa tools, dan mengembalikan draf formulir tiket.
      - `listOpenTickets`: Mengambil seluruh tiket aktif (`open`, `process`, `pending`, `investigation`) yang relevan dengan pelanggan obrolan (berdasarkan kecocokan data master atau nomor kanonik `wa_id`).
      - `listClosedTickets`: Mengambil riwayat tiket terselesaikan (`resolved`, `closed`), dilengkapi dengan objek `detail` terkurasi (hanya field publik yang diizinkan untuk konsumen).
      - `generateCommentDraft`: Merangkum sekumpulan pesan obrolan yang dicentang admin menjadi draf komentar tiket.
      - `applyTicketOrigin`: Menyematkan referensi `wa_conversation` dan nomor kanonik `wa_id` yang diverifikasi server ke dokumen tiket baru.
  - **Kontroler & Perutean Backend (`backend/src/controllers/`, `backend/src/routes/`, `backend/src/app.js`)**:
    - [`backend/src/controllers/waTicket.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/waTicket.controller.js) [NEW]:
      - Kontroler HTTP terstandarisasi dengan `asyncHandler`:
        - `POST /whatsapp/conversations/:conversationId/ticket/ai-draft`: Generator draf form tiket via AI.
        - `GET /whatsapp/conversations/:conversationId/ticket/open-tickets`: Daftar tiket aktif terkait percakapan.
        - `GET /whatsapp/conversations/:conversationId/ticket/closed-tickets`: Daftar tiket selesai berobjek detail untuk PDF.
        - `POST /whatsapp/conversations/:conversationId/ticket/comment-draft`: Generator draf komentar dari pesan terpilih.
        - `GET /whatsapp/conversations/:conversationId/ticket/ai-status`: Pengecekan status ketersediaan layanan AI global.
    - [`backend/src/routes/waTicket.route.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/routes/waTicket.route.js) [NEW]: Pendaftaran endpoint RESTful tiket obrolan WhatsApp dengan validasi parameter ID percakapan.
    - [`backend/src/controllers/ticket.controller.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/controllers/ticket.controller.js): Penyesuaian `createTicket` untuk menyematkan jejak `wa_conversation` dan `wa_id` bila tiket dibuat dari drawer obrolan.
    - [`backend/src/app.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/app.js): Pendaftaran `waTicketRouter` ke Express application pipeline.
    - [`backend/src/locales/id/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/id/translation.json) & [`backend/src/locales/en/translation.json`](file:///home/dhedhy/Project/Dekasimal-V2/backend/src/locales/en/translation.json): Penambahan kunci terjemahan bilingual pesan sistem tiket obrolan.
  - **Komponen Antarmuka & Drawer Obrolan Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/customerService/whatsappChat/components/TicketDrawer.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/TicketDrawer.jsx) [NEW]:
      - Komponen panel geser samping (*Drawer*) multi-tab elegan di panel obrolan dengan judul adaptif per tab ("Buat tiket", "Tiket terbuka", "Tiket selesai") dan navigasi tab responsif.
    - [`frontend/src/app/pages/customerService/whatsappChat/components/ticket/CreateTicketTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/CreateTicketTab.jsx) [NEW]:
      - Antarmuka pembuatan tiket langsung di dalam obrolan:
        - Pemilih tipe tiket berbasis Headless UI `Listbox` (**Pelanggan** dan **Lainnya**).
        - Kolom form lengkap setara halaman Buat Tiket standar: Divisi penanganan, CC Notifikasi staf, prioritas, topik masalah, deskripsi keluhan, dan komponen unggah lampiran berkas.
        - Area footer terpisah dengan tombol **Isi dengan AI** di kiri dan tombol **Simpan Tiket** di kanan.
    - [`frontend/src/app/pages/customerService/whatsappChat/components/ticket/OpenTicketsTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/OpenTicketsTab.jsx) [NEW]:
      - Antarmuka tiket aktif:
        - Daftar tiket dalam kotak terpadu dengan status badge dan ikon navigasi cepat untuk membuka detail tiket di tab peramban baru.
        - Komponen seleksi pesan obrolan (`MessagePicker`) untuk memilih pesan pelanggan yang relevan.
        - Tombol **Rangkum dengan AI** untuk menyusun rangkuman percakapan menjadi teks komentar yang dapat disunting sebelum disimpan ke tiket.
    - [`frontend/src/app/pages/customerService/whatsappChat/components/ticket/ClosedTicketsTab.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/ClosedTicketsTab.jsx) [NEW]:
      - Antarmuka pelaporan tiket selesai:
        - Daftar tiket selesai dan pemilih komentar teknis yang akan diikutsertakan ke dalam laporan.
        - Tombol **Buat PDF Laporan** untuk mengompilasi data ke dokumen PDF vektor di sisi klien.
        - Integrasi komponen pratinjau dokumen dan kotak pesan pengantar (*caption*, maksimal 1.024 karakter).
        - Tombol **Kirim ke Pelanggan** yang mengirimkan file PDF sebagai lampiran WhatsApp resmi saat jendela percakapan aktif.
    - [`frontend/src/app/pages/customerService/whatsappChat/components/ticket/MessagePicker.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/MessagePicker.jsx) [NEW]:
      - Komponen visual pemilihan pesan obrolan dengan opsi **Pilih Semua** (otomatis membatasi maksimal 50 pesan terbaru demi stabilitas API) dan pembeda arah pesan masuk/keluar.
    - [`frontend/src/app/pages/customerService/whatsappChat/components/ticket/PdfPreview.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ticket/PdfPreview.jsx) [NEW]:
      - Komponen pratinjau dokumen PDF berbasis `react-pdf` dengan pengukuran lebar kontainer otomatis agar pratinjau pas di dalam drawer tanpa merusak layout.
    - [`frontend/src/app/pages/customerService/whatsappChat/components/ChatHeader.jsx`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/components/ChatHeader.jsx):
      - Penambahan tombol aksi "Tiket" dengan badge indikator jumlah tiket terbuka aktif di samping info kontak.
  - **Generator Dokumen PDF Vektor & Algoritma Murni Frontend (`frontend/src/`)**:
    - [`frontend/src/app/pages/customerService/whatsappChat/utils/ticketReportPdf.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/utils/ticketReportPdf.js) [NEW]:
      - Engine pembuatan dokumen PDF vektor profesional murni berbasis `jspdf`:
        - Kop dokumen resmi dengan logo perusahaan, alamat, judul dokumen, dan garis pemisah tebal.
        - Tabel informasi tiket bergaris (Nomor Tiket, Tanggal Selesai, Nama Pelanggan/Mitra, ID Pelanggan, Layanan, Kategori).
        - Kotak hasil pekerjaan (Akar Masalah dan Tindakan Penanganan).
        - Daftar kronologi catatan & komentar penanganan bernomor urut dengan pembatasan teks rapi.
        - Footer berulang otomatis di setiap halaman: penomoran format "Halaman X dari Y", stempel waktu pembuatan, dan catatan kerahasiaan dokumen.
        - Pencegahan *orphan headings* di dasar halaman (otomatis berpindah ke halaman baru jika ruang tidak mencukupi) dan sanitasi teks ke karakter Latin-1.
    - [`frontend/src/app/pages/customerService/whatsappChat/utils/ticketAssist.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/utils/ticketAssist.js) [NEW]:
      - Logika murni penentuan kelayakan pengiriman (validasi jendela 24 jam, batas panjang caption, ketersediaan PDF), nama file PDF otomatis, dan pemfilteran pesan.
    - Custom Hooks:
      - [`useTicketAssist.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/hooks/useTicketAssist.js) [NEW]: Manajemen state reaktif drawer tiket, mutasi API draf AI, pembuatan blob PDF, dan pengiriman lampiran.
      - [`useTicketPrivileges.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/hooks/useTicketPrivileges.js) [NEW]: Validasi hak akses pengguna per tipe tiket.
    - [`frontend/src/i18n/locales/id/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/id/translations.json) & [`frontend/src/i18n/locales/en/translations.json`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/i18n/locales/en/translations.json):
      - Penambahan 75+ pasangan kunci terjemahan bilingual untuk seluruh elemen antarmuka tiket obrolan.
  - **Rangkaian Pengujian Otomatis Komprehensif (Unit & Integration Tests)**:
    - Backend Unit Tests (3 berkas, 43 test, 100% lulus):
      - [`waTicketAi.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/waTicketAi.test.js) (31 test): Pengujian pembentukan prompt AI, pembersihan tag `<transkrip>`, validasi skema JSON, dan pembatasan laju pemanggilan.
      - [`waTicketReport.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/waTicketReport.test.js) (7 test): Pengujian penyaringan komentar manusia dan seleksi data detail tiket.
      - [`waTicketAccess.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/unit/waTicketAccess.test.js) (5 test): Pengujian kalkulasi privilege per tipe tiket.
    - Backend Integration Tests (4 berkas, 100% lulus):
      - [`waTicket.service.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waTicket.service.test.js): Pengujian pemanggilan AI dan query relasi tiket terbuka/selesai.
      - [`waTicket.controller.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waTicket.controller.test.js): Pengujian status code HTTP, validasi payload, dan otorisasi.
      - [`waTicket.createTicket.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waTicket.createTicket.test.js): Pengujian penyematan asal tiket `wa_conversation` dan `wa_id`.
      - [`waTicketAccess.resolve.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/backend/test/integration/waTicketAccess.resolve.test.js): Pengujian resolusi hak akses dan proteksi channel.
    - Frontend Unit & Logic Tests (3 berkas, 46 test, 100% lulus):
      - [`ticketReportPdf.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/utils/ticketReportPdf.test.js) (18 test): Pengujian kompilasi PDF vektor di Node environment (struktur dokumen, pagination, sanitasi data sensitif, teks yang dapat dicari).
      - [`ticketAssist.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/utils/ticketAssist.test.js) (24 test): Pengujian logika seleksi pesan, batas 50 pesan, nama berkas PDF, dan syarat kirim lampiran.
      - [`contactKinds.test.js`](file:///home/dhedhy/Project/Dekasimal-V2/frontend/src/app/pages/customerService/whatsappChat/utils/contactKinds.test.js) (4 test): Pengujian pemetaan relasi entitas kontak.
- **Deskripsi Perubahan & Fungsi**:
  - Menyatukan alur penanganan komplain pelanggan: agen CS kini dapat membuat tiket resmi, memperbarui tiket yang sedang ditangani dengan pesan obrolan, dan mengirimkan laporan penyelesaian pekerjaan langsung dari layar Obrolan WhatsApp tanpa perlu membuka menu tiket secara terpisah.
  - Memanfaatkan kecerdasan buatan (*AI Assistant*) untuk mengotomatisasi penulisan ringkasan keluhan dan catatan teknis, menghemat waktu agen dan meningkatkan konsistensi format tiket di seluruh tim operasional.
  - Menghasilkan dokumen laporan pekerjaan berbentuk PDF vektor resmi dengan tampilan visual yang rapi dan elegan, memberikan impresi profesional kepada pelanggan sekaligus menjaga kerahasiaan data internal perusahaan.

---

## 📢 Ringkasan Dampak Perubahan & Fungsionalitas Baru

| Issue | Judul | Dampak Utama |
| ----- | ----- | ------------ |
| #373  | Tiket & Asisten AI dari Obrolan WhatsApp | Integrasi drawer tiket multi-tab di layar chat WhatsApp, otomasi draf form tiket via AI, rangkuman pesan ke komentar tiket, serta generator & pratinjau dokumen laporan PDF vektor untuk pelanggan. |

### Kemampuan Baru Pengguna/Admin

- **Membuat Tiket Cepat Berbantuan AI**: Admin dapat mengklik tombol "Tiket" pada header obrolan dan menekan tombol "Isi dengan AI". Sistem secara cerdas akan mengekstrak topik masalah, rincian keluhan, nomor alternatif, dan prioritas tiket langsung dari percakapan WhatsApp ke dalam form pembuatan tiket.
- **Pembaruan Komentar Tiket dari Obrolan**: Untuk pelanggan yang sudah memiliki tiket penanganan terbuka, admin dapat mencentang pesan-pesan obrolan WhatsApp pelanggan (dengan tombol "Pilih Semua" hingga 50 pesan terbaru) lalu merangkumnya menggunakan AI atau memasukkannya langsung sebagai komentar resmi pada tiket tersebut.
- **Penerbitan Dokumen Laporan Pekerjaan Pelanggan (PDF Vektor)**: Admin dapat memilih tiket yang telah berstatus selesai, memilih catatan pekerjaan yang ingin dibagikan, dan mengompilasi dokumen laporan berformat PDF resmi ber-kop perusahaan dalam hitungan detik.
- **Pratinjau Dokumen Sebelum Pengiriman**: Admin dapat melihat hasil dokumen PDF secara langsung di dalam drawer obrolan sebelum mengirimkannya, memastikan tidak ada kekeliruan data atau catatan yang tidak diinginkan.
- **Pengiriman Laporan Terintegrasi via WhatsApp**: Admin dapat mengirimkan berkas PDF hasil kompilasi beserta pesan pengantar (maksimal 1.024 karakter) langsung ke nomor WhatsApp pelanggan dalam satu aksi kirim lampiran, selama jendela pesan 24 jam masih terbuka.

### Bug Fix / Solusi Masalah

- **Eliminasi Friksi Konteks (Context Switching)**: Memecahkan kendala agen CS yang sebelumnya harus berpindah-pindah menu antara obrolan WhatsApp dan modul tiket, yang sering menyebabkan hilangnya fokus percakapan atau keterlambatan penanganan komplain.
- **Pencegahan Kebocoran Data Internal Perusahaan**: Menerapkan arsitektur daftar-putih (*whitelist*) ganda di backend dan generator PDF; informasi sensitif seperti nama teknisi internal, rincian biaya perbaikan, nomor seri perangkat internal, dan catatan internal staf secara otomatis dihilangkan dari dokumen laporan pelanggan.
- **Pencegahan Injeksi Prompt AI (AI Security & Guardrails)**: Membersihkan transkrip pesan obrolan pelanggan dari karakter manipulatif dan tag penutup buatan sebelum dikirimkan ke model AI, serta menyensor kata-kata kotor menggunakan engine sensor kata kasar `censorForAdmin`.
- **Kepatuhan Terhadap Kebijakan Jendela Pesan WhatsApp (24-Hour Window)**: Tombol pengiriman laporan PDF secara otomatis dinonaktifkan dengan penjelasan status bila batas jendela interaksi 24 jam telah berakhir, mencegah pesan gagal kirim atau penolakan dari penyedia API WhatsApp.

### Menu/Fitur Baru

- **Drawer Tiket Obrolan WhatsApp**: Panel samping interaktif pada halaman Obrolan WhatsApp dengan 3 tab navigasi: **Buat tiket**, **Tiket terbuka**, dan **Tiket selesai**.
- **Tombol Tiket & Badge Indikator pada Chat Header**: Tombol akses cepat tiket di bagian atas layar obrolan yang menampilkan badge jumlah tiket terbuka pelanggan.
- **Komponen Pemilih Pesan Obrolan (`MessagePicker`)**: Antarmuka seleksi pesan obrolan dengan batasan aman 50 pesan dan aksi toggle "Pilih Semua".
- **Generator Dokumen Laporan Pekerjaan PDF Vektor**: Modul pembentukan dokumen PDF vektor profesional di peramban web lengkap dengan kop korporat, tabel data bergaris, dan penomoran halaman otomatis.
- **Komponen Pratinjau PDF Terintegrasi (`PdfPreview`)**: Viewer PDF interaktif di dalam drawer untuk memeriksa tata letak dan isi dokumen sebelum proses pengiriman.

---

## 📖 Informasi & Tutorial Singkat Fitur Utama

### 1. Membuat Tiket Baru dari Obrolan dengan Bantuan AI

- **Penjelasan Fitur**:
  Fitur ini memungkinkan agen CS membuat tiket keluhan pelanggan tanpa meninggalkan layar obrolan WhatsApp. Asisten AI membaca riwayat obrolan terkini dan mengisi form tiket secara otomatis.
- **Langkah Penggunaan (Tutorial)**:
  1. Buka menu **WhatsApp > Obrolan** di Web Admin.
  2. Buka percakapan pelanggan yang menyampaikan keluhan.
  3. Klik tombol **Tiket** pada bagian kanan atas (*Chat Header*). Panel drawer tiket akan terbuka pada tab **Buat tiket**.
  4. Pilih jenis tiket (**Pelanggan** atau **Lainnya**) melalui menu dropdown *Listbox*.
  5. Klik tombol **Isi dengan AI** pada bagian footer drawer. Sistem akan menganalisis percakapan dan mengisi kolom *Judul*, *Topik Masalah*, *Deskripsi Keluhan*, dan *Prioritas*.
  6. Tinjau dan sesuaikan isi form jika diperlukan (pilih *Divisi Penanganan*, tambahkan *CC Notifikasi*, atau unggah lampiran jika ada).
  7. Klik tombol **Simpan Tiket**. Tiket resmi akan dibuat di sistem dengan jejak asal percakapan WhatsApp yang tersimpan otomatis.

---

### 2. Menambahkan Komentar ke Tiket Terbuka dari Pesan Obrolan

- **Penjelasan Fitur**:
  Jika pelanggan mengirimkan informasi tambahan untuk tiket yang sedang berjalan, admin dapat langsung menyalin atau merangkum pesan tersebut ke dalam riwayat tiket tanpa membuat tiket baru.
- **Langkah Penggunaan (Tutorial)**:
  1. Pada percakapan obrolan pelanggan yang bersangkutan, klik tombol **Tiket** pada header obrolan.
  2. Buka tab **Tiket terbuka**. Daftar tiket pelanggan yang sedang dalam proses penanganan akan ditampilkan.
  3. Pilih tiket yang ingin diperbarui.
  4. Pada daftar pesan obrolan di bawahnya, centang pesan-pesan yang berisi informasi penting dari pelanggan (atau klik **Pilih Semua** untuk memilih hingga 50 pesan terakhir).
  5. Klik tombol **Rangkum dengan AI** jika ingin menyusun poin-poin penting secara otomatis, atau ketik langsung pada kotak komentar.
  6. Periksa draf komentar, lalu klik tombol **Tambah Komentar ke Tiket**.

---

### 3. Mengompilasi & Mengirimkan Laporan Tiket Berformat PDF ke Pelanggan

- **Penjelasan Fitur**:
  Setelah penanganan gangguan selesai, admin dapat menerbitkan dokumen laporan hasil pekerjaan berformat PDF resmi ber-kop perusahaan dan langsung mengirimkannya ke nomor WhatsApp pelanggan.
- **Langkah Penggunaan (Tutorial)**:
  1. Pada percakapan obrolan pelanggan, klik tombol **Tiket** pada header obrolan.
  2. Buka tab **Tiket selesai**. Riwayat tiket yang telah diselesaikan akan ditampilkan.
  3. Pilih tiket yang ingin dibuatkan laporannya.
  4. Centang catatan-catatan teknis / komentar penanganan yang relevan untuk disertakan dalam dokumen.
  5. Klik tombol **Buat PDF Laporan**. Sistem akan mengompilasi data ke format PDF vektor.
  6. Komponen pratinjau PDF akan muncul di layar. Periksa tampilan dokumen, tabel detail, dan catatan penanganan.
  7. Tulis pesan pengantar pada kotak keterangan (*caption*, maksimal 1.024 karakter).
  8. Klik tombol **Kirim ke Pelanggan**. Berkas PDF beserta pesan pengantar akan langsung terkirim ke obrolan WhatsApp pelanggan sebagai lampiran resmi.
