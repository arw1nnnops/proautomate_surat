# Otomatisasi Surat NSD (Memo Masuk ke S-Seg Wil)

Aplikasi web untuk mengubah memo masuk (`.doc`, `.docx`, `.pdf` teks, maupun PDF hasil scan/foto) menjadi surat resmi **Segenap Kantor Wilayah (`S-Seg Wil`)** Divisi Network Strategy & Development (NSD) PT Bank Negara Indonesia (Persero) Tbk.

## Struktur Proyek
```
otomatisasi-surat/
├── public/
│   └── index.html      # Halaman Login, Registrasi, Unggah Memo, Pratinjau Surat Resmi BNI (Arial 10 pt), Unduh Word & PDF
├── src/
│   └── worker.js       # Backend Cloudflare Worker (/api/compose + static assets)
├── wrangler.toml       # Konfigurasi resmi Cloudflare Workers with Assets
├── package.json        # Skrip dev & deploy lokal
├── .gitignore
└── README.md
```

## Fitur Utama
- **Sesi Login & Daftar Akun** sebelum mengakses halaman utama.
- **Template Resmi BNI NSD (`S-Seg Wil`) utuh dari atas sampai bawah**: logo BNI kanan atas, header `Jakarta` tanpa tanggal, `No. : NSD / 2.2 /`, `Kepada` rata kiri tebal, `Perihal` sesuai memo, baris referensi tebal bergaris bawah (`Memo Divisi [Singkatan] No. [Nomor] Tanggal [Tanggal]`), penutup, tanda tangan NSD, `CC` tebal, dan footer Kantor Pusat BNI.
- **Format Rapi & Baku KBBI**: font **Arial 10 pt**, rata kanan-kiri (`justify`), poin & tabel mengikuti memo, cetak miring (*italic*) istilah asing, dan otomatis mengabaikan halaman lampiran setelah tanda tangan.
- **AI Terpusat di Server Cloudflare**: semua pengguna yang login langsung memakai `GEMINI_API_KEY` dan `GEMINI_MODEL` (`gemini-4-hydrogen`) dari Cloudflare Secrets tanpa perlu menempel API Key di browser.

## Cara Unggah ke GitHub & Deploy Cloudflare
1. Ekstrak seluruh isi ZIP ini ke satu folder.
2. Unggah folder `public/`, folder `src/`, `wrangler.toml`, `package.json`, `.gitignore`, dan `README.md` ke repository GitHub `otomatisasi_surat` (seret langsung folder `public` dan `src` ke kotak unggah GitHub agar struktur foldernya terbawa).
3. Di Cloudflare Dashboard -> proyek `otomatisasi-surat` -> **Settings** -> **Runtime variables and secrets** (Production), pastikan sudah terpasang:
   - `GEMINI_API_KEY` (Secret)
   - `GEMINI_MODEL` (Text: `gemini-4-hydrogen`)
4. Cloudflare akan otomatis melakukan *deploy* dan website tayang di alamat `.workers.dev` milikmu.
