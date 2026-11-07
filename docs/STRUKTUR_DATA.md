# STRUKTUR DATA SATPAM BOK

| Tab | Header | Fungsi |
|---|---|---|
| REGISTER_BELANJA | Baris 3 | Register transaksi belanja; mengikuti kolom sumber Excel dan kolom monitoring tambahan. |
| ANGGARAN | Baris 1 | Tahun, program, kegiatan, kode rekening, pagu, target bulanan, penanggung jawab. |
| JADWAL_KEGIATAN | Baris 1 | Kalender kegiatan, lokasi, PIC, status dan catatan. |
| SPJ | Baris 1 | ID, transaksi terkait, checklist, tautan Drive, status verifikasi, tenggat, verifikator, catatan. |
| PENGGUNA | Baris 1 | Daftar referensi pengguna dan peran. |
| KONFIGURASI | Baris 1 | Informasi umum unit; bukan tempat menyimpan rahasia API. |

Pagu harus bersumber dari dokumen anggaran resmi. Target realisasi bulanan merupakan target pengendalian, bukan realisasi aktual. Tautan Drive harus memiliki izin terbatas sesuai kewenangan.
