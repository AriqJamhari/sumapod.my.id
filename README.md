# sumapod.my.id - n8n AI Agent RAG (Google Docs & Supabase)

Repositori ini berisi workflow otomasi **n8n** untuk implementasi **AI Agent Chatbot berbasis RAG (Retrieval-Augmented Generation)** otomatis menggunakan trigger **Google Drive**, **Google Docs**, pemecah teks **Recursive Character Text Splitter**, penyimpanan vektor **Supabase Vector Store**, model bahasa & embeddings **Google Gemini (3.1 Flash Lite)**, serta **Postgres Chat Memory**.

Proyek ini berfungsi sebagai **Asisten Pembimbing Penulisan Ilmiah Tugas Akhir (Skripsi)** yang datanya selalu tersinkronisasi otomatis dari dokumen Google Docs.

---

## 🏗️ Alur & Arsitektur Workflow

Workflow ini mencakup dua alur terintegrasi:

### 1. Ingestion Pipeline Otomatis (Google Drive Docs to Supabase)
- **Google Drive Trigger**: Memantau pembaruan file spesifik di Google Drive secara berkala (misal: dokumen *Panduan Penulisan Ilmiah Tugas Akhir Skripsi*).
- **Edit Fields**: Menyiapkan metadata file ID dan nama dokumen.
- **Download file**: Mengunduh dan mengonversi Google Docs menjadi format teks mentah (`text/plain`).
- **Extract from File**: Mengekstrak teks dari data biner hasil unduhan.
- **Recursive Character Text Splitter & Default Data Loader**: Memotong teks secara cerdas berdasarkan karakter dan paragraf agar konteks tetap terjaga.
- **Embeddings Google Gemini**: Mengubah potongan teks menjadi vektor embedding numerik.
- **Supabase Vector Store (Insert Mode)**: Menyimpan embeddings dan teks ke dalam tabel vektor Supabase (`documents`).

### 2. RAG Chatbot Pipeline (AI Agent Penjawab Pertanyaan)
- **Chat Trigger** (`When chat message received`): Antarmuka Web Chatbot publik interaktif untuk mahasiswa.
- **AI Agent**: Mengarahkan penalaran asisten penulisan ilmiah dengan instruksi ketat agar tidak mengarang jawaban di luar basis data.
- **Google Gemini Chat Model** (`models/gemini-3.1-flash-lite`): Model inferensi cepat dan responsif.
- **Supabase Vector Store Tool (Retrieve-as-Tool)**: Tool khusus AI Agent untuk mencari relevansi konteks panduan TA dari Supabase.
- **Postgres Chat Memory**: Menyimpan memori histori percakapan antar sesi ke database PostgreSQL Supabase.

---

## 📁 Berkas Proyek

- [`sumapod.my.id.json`](./sumapod.my.id.json): File workflow n8n lengkap siap pakai.

---

## 🚀 Panduan Import ke n8n

1. Buka instance **n8n** Anda.
2. Buat workflow baru atau masuk ke menu **Workflows**.
3. Klik tombol menu **Options** (ikon titik tiga di kanan atas) lalu pilih **Import from File...** (atau salin isi file JSON dan paste langsung di canvas).
4. Pilih file [`sumapod.my.id.json`](./sumapod.my.id.json).
5. Konfigurasikan kredensial yang dibutuhkan:
   - **Google Drive OAuth2 API**: Untuk memantau dan mengunduh Google Docs.
   - **Supabase API**: Untuk koneksi ke database vektor Supabase.
   - **Postgres Database Credentials**: Untuk node Postgres Chat Memory.
   - **Google Gemini (PaLM) API**: Untuk node Embeddings dan Chat Model (`gemini-3.1-flash-lite`).
6. Aktifkan workflow (`Active / Publish`) agar Google Drive Trigger dan Chat Trigger publik mulai beroperasi.
