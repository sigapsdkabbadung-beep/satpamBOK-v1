# SATPAM BOK — Paket Rebuild GitHub Pages + Google Sheets

Sistem monitoring realisasi Bantuan Operasional Kesehatan untuk UPTD Puskesmas Sawan II.

## Isi paket
- `index.html`, `style.css`, `app.js`: frontend GitHub Pages.
- `gas/Code.gs`: backend Apps Script yang sudah diperbaiki.
- `SATPAM_BOK_Template_GoogleSheets.xlsx`: template database.
- `docs/PANDUAN_INSTALASI.md`: panduan rinci dari pembuatan database sampai uji koneksi.
- `docs/STRUKTUR_DATA.md`: struktur tab dan header.

## Perbaikan utama
Backend tidak lagi membaca `SPREADSHEET_ID` dari tab KONFIGURASI. ID dibaca langsung dari **Project Settings → Script Properties**. Pesan error akan menjelaskan langkah perbaikan jika properti belum tersedia. Fungsi `testConnection()` disediakan untuk menguji ID, izin, dan nama tab sebelum deployment.

## Endpoint
URL Web App yang diberikan untuk integrasi pada frontend:
`https://script.google.com/macros/s/AKfycbwEoy6Ar6R04C0wDeIL_s_OmytdJXBpPYo6QrF6ETPzLQRXTOJv2TnzvkuEVpPuFKPD/exec`

Jika Anda membuat deployment baru dengan URL berbeda, perbarui konstanta `apiUrl` pada `app.js`.

## Mulai dari sini
Baca `docs/PANDUAN_INSTALASI.md` secara berurutan. Jangan melewati langkah Script Properties dan pengujian `testConnection()`.

## Peringatan keamanan
Jangan gunakan akses `Anyone` untuk data keuangan dan dokumen SPJ. Backend membatasi email melalui `ALLOWED_EMAILS`, tetapi kemampuan membaca email pengguna bergantung pada jenis akun dan konfigurasi Google Workspace. Uji dengan akun organisasi terlebih dahulu. Jangan unggah dokumen SPJ atau data pribadi ke repository publik.
