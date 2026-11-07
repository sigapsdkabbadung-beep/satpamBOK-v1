# PANDUAN INSTALASI ULANG SATPAM BOK

Panduan ini memperbaiki error `Atur Script Property SPREADSHEET_ID terlebih dahulu`.

## 1. Siapkan database Google Sheets
1. Unggah `SATPAM_BOK_Template_GoogleSheets.xlsx` ke Google Drive.
2. Klik kanan file > **Buka dengan Google Spreadsheet**.
3. Di Google Sheets, pastikan nama tab berikut sama persis (huruf besar dan underscore):
   - `REGISTER_BELANJA`
   - `ANGGARAN`
   - `JADWAL_KEGIATAN`
   - `SPJ`
   - `PENGGUNA`
   - `KONFIGURASI` (opsional untuk informasi unit)
4. Salin ID Spreadsheet dari alamat browser. Contoh URL:
   `https://docs.google.com/spreadsheets/d/1AbCDefGhijkLMNopQRstuVWxyz123456789/edit#gid=0`
   ID adalah bagian di antara `/d/` dan `/edit`, yaitu `1AbCDefGhijkLMNopQRstuVWxyz123456789`.
5. Pastikan akun Google yang memiliki proyek Apps Script juga mempunyai akses Editor ke spreadsheet tersebut.
6. Isi pagu di tab `ANGGARAN` berdasarkan DPA/RKA resmi. Jangan memakai angka contoh sebagai pagu sah.

## 2. Pasang backend Apps Script
1. Buka proyek Apps Script yang menghasilkan URL Web App SATPAM BOK Anda.
2. Buka file `Code.gs`. Hapus kode lama dan tempel seluruh isi `gas/Code.gs` dari paket ini.
3. Klik **Simpan**.
4. Di sisi kiri buka **Project Settings** (ikon roda gigi).
5. Cari **Script Properties** lalu klik **Add script property**.
6. Tambahkan properti pertama:
   - Property: `SPREADSHEET_ID`
   - Value: tempel ID spreadsheet dari langkah 1 (hanya ID, bukan seluruh URL).
7. Tambahkan properti kedua:
   - Property: `ALLOWED_EMAILS`
   - Value: email Google petugas yang boleh menggunakan aplikasi. Untuk beberapa email, pisahkan dengan koma, contoh `petugas1@instansi.go.id,petugas2@instansi.go.id`.
8. Klik **Save script properties**.
9. Kembali ke editor, pilih fungsi `testConnection` pada dropdown fungsi, lalu klik **Run**.
10. Saat pertama kali, Google meminta izin. Pilih akun pemilik dan izinkan akses Spreadsheet. Setelah selesai, buka **Execution log**. Harus muncul `Koneksi OK: ...`.

> Jika `testConnection` gagal, cek kembali ID (tanpa spasi), nama tab, dan izin Editor akun pemilik script.

## 3. Perbarui deployment Web App
1. Klik **Deploy > Manage deployments**.
2. Pilih deployment Web App yang URL-nya berakhiran `/exec`.
3. Klik ikon pensil **Edit**.
4. Pada Version pilih **New version**.
5. Untuk penggunaan internal, pilih **Execute as: User accessing the web app** dan batasi **Who has access** ke akun/domain organisasi yang sesuai (misalnya domain Google Workspace instansi).
6. Klik **Deploy** dan selesaikan otorisasi bila diminta.
7. Salin URL Web App `/exec`. URL tersebut harus sama dengan URL yang tertanam pada `app.js` paket ini. Jika URL berubah, ganti nilai `apiUrl` di `app.js` sebelum upload GitHub.

Catatan penting: Apps Script harus dapat membaca email pengguna melalui `Session.getActiveUser().getEmail()`. Pada akun Gmail pribadi atau konfigurasi deployment tertentu, email dapat kosong. Bila kosong, backend menolak akses. Gunakan akun Google Workspace instansi dan uji dahulu. Jangan mengubah akses menjadi `Anyone` untuk mengakali error karena itu dapat membuka data keuangan kepada publik.

## 4. Upload ke GitHub Pages
1. Ekstrak ZIP.
2. Buat repository GitHub, misalnya `satpam-bok`.
3. Unggah isi folder `SATPAM_BOK_REBUILD` ke root repository (bukan folder ZIP-nya).
4. Pastikan `index.html`, `app.js`, `style.css`, dan folder `gas` berada di root sesuai struktur paket.
5. Buka **Settings > Pages**.
6. Pilih **Deploy from a branch**, branch `main`, folder `/ (root)`, lalu **Save**.
7. Tunggu URL GitHub Pages aktif, kemudian buka situs.

## 5. Uji koneksi setelah publikasi
1. Login ke browser menggunakan email Google yang sudah tercantum pada `ALLOWED_EMAILS`.
2. Buka situs GitHub Pages.
3. Tekan tombol muat ulang/sinkronisasi di dashboard.
4. Jika koneksi sukses, status berubah menjadi **Terhubung ke Google Sheets**.
5. Coba input satu data uji dengan keterangan `UJI COBA`, lalu periksa apakah baris baru muncul pada tab yang sesuai. Hapus data uji setelah pemeriksaan.
6. Buka menu SPJ dan pastikan status dokumen tampil sesuai data pada tab `SPJ`.

## 6. Jika masih muncul error
| Pesan | Pemeriksaan |
|---|---|
| `SPREADSHEET_ID belum diatur` | Script Properties harus bernama persis `SPREADSHEET_ID`; simpan lalu deploy New version. |
| `Spreadsheet tidak dapat dibuka` | ID salah atau akun pemilik Apps Script belum diberi akses Editor. |
| `Tab ... tidak ditemukan` | Nama tab harus sama persis dengan template. |
| `ALLOWED_EMAILS belum diisi` | Tambahkan email pada Script Properties. |
| `Email Google tidak terbaca` | Periksa akun dan deployment; gunakan Google Workspace dan Execute as User accessing. |
| `Akun ... belum terdaftar` | Tambahkan email tersebut ke `ALLOWED_EMAILS`, pisahkan dengan koma. |
| Koneksi tidak dapat terhubung | Pastikan URL `/exec` di `app.js` benar, deployment sudah New version, dan pengguna sudah login Google. |

## 7. Struktur dan fungsi data
- `REGISTER_BELANJA`: header ada di baris ke-3.
- `ANGGARAN`, `JADWAL_KEGIATAN`, `SPJ`: header ada di baris pertama.
- `PENGGUNA`: disiapkan sebagai daftar referensi pengguna; validasi akses API saat ini menggunakan `ALLOWED_EMAILS` di Script Properties.
- Data dokumen SPJ disimpan di Google Drive. Aplikasi mencatat tautan; unggah file langsung melalui aplikasi belum diaktifkan.

## 8. Keamanan dan batasan
- Jangan menaruh token rahasia di `app.js` atau repository GitHub.
- Jangan mengatur akses Web App ke `Anyone` untuk data operasional, transaksi, atau SPJ.
- GitHub Pages adalah frontend statis; pembatasan akses wajib ditegakkan di backend dan Google Drive.
- Fitur WhatsApp otomatis memerlukan WhatsApp Business Platform/provider resmi. Tombol berbagi pesan bukan pengiriman otomatis.
- Lakukan uji akses dengan akun petugas dan akun yang tidak berwenang sebelum memasukkan data riil.
