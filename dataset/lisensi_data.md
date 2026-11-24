# Ketentuan pemakaian data MUARA (dicek 24 Nov 2026)

## Open-Meteo (gelombang MFWAM, ECMWF, ERA5-Ocean)
- API gratis hanya untuk non-komersial: tanpa langganan, tanpa iklan. Lomba dan aplikasi non-profit masuk.
- Lisensi CC BY 4.0, jadi wajib atribusi. Contoh: "Data gelombang: Open-Meteo.com (CC BY 4.0)".
- Batas gratis: < 10.000 panggilan/hari, 5.000/jam, 600/menit. Dengan cache per jam, MUARA cuma butuh puluhan panggilan per hari.
- Sumber: https://open-meteo.com/en/terms

## BMKG (prakiraan perairan, peringatan gelombang, InaWaves)
- Gratis untuk penggunaan pribadi dan non-komersial, dengan atribusi "Sumber: BMKG" (poin 3.2 dan 9.1).
- Kutipan harus memuat judul konten, tanggal akses, dan tautan langsung ke konten asli (9.1).
- Produk harus tampil utuh dan tidak boleh diubah maknanya (9.2, 9.4).
- PENTING:
  - Integrasi ke aplikasi pihak ketiga butuh izin tertulis resmi (9.3).
  - Scraping atau bot dilarang (8.1).
  - Akses otomatis harus lewat API resmi (4.5).
- Untuk izin API atau kerja sama, hubungi Biro Hukum, Hubungan Masyarakat, dan Kerja Sama BMKG lewat Contact Center 196, cc196@bmkg.go.id, atau WA 0852-8019-6196.
- Konsekuensi untuk MUARA:
  - Sebelum izin keluar, aplikasi TIDAK mengambil data BMKG otomatis. Banner pengaman memakai tombol di halaman Admin plus tautan ke halaman resmi BMKG.
  - Perbandingan BMKG di halaman /bukti boleh, karena itu penggunaan penelitian dengan atribusi. Datanya diambil manual.
  - Kirim surat permohonan izin. Kalau dibalas, itu bisa jadi poin "kerja sama dengan lembaga" di depan juri.
- Sumber: https://www.bmkg.go.id/ketentuan-penggunaan

## BIG (prediksi dan datum pasut SRGI, BATNAS)
- Ketentuan khusus untuk aplikasi publik belum ditemukan.
- Kontak di header file prediksi: srgi@big.go.id. Kirim email singkat untuk menanyakan izin menampilkan prediksi pasut di aplikasi non-komersial.
- Sementara itu, wajib mencantumkan atribusi "Prediksi pasang surut: BIG (SRGI)" dan "Batimetri: BATNAS, BIG".

## Copernicus Sentinel-2
- Data terbuka dan gratis. Atribusi: "Contains modified Copernicus Sentinel data [tahun]".
