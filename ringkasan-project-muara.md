# MUARA — Ringkasan Project

*Diperbarui 24 November 2026 sore (setelah pengumpulan data). Web app untuk lomba Web Application TECHCOMFEST 2027 (UKM PCC, Politeknik Negeri Semarang).*

**Tagline kerja:** perkiraan kondisi plawangan (mulut muara) per jam untuk nelayan kecil, dihitung dari ramalan gelombang asli, pasang surut, dan fisika gelombang. Bukan status "aman", tapi skala **lebih tenang ↔ lebih berbahaya**. Keputusan tetap di tangan nakhoda, dan peringatan BMKG selalu didahulukan.

Nama alternatif yang sedang dipertimbangkan: **TITEN** (dari *ilmu titen*, "membaca laut seperti nelayan, menghitungnya seperti ilmuwan"). Nama belum diputuskan.

---

## 1. Konteks lomba

| Hal | Isi |
|---|---|
| Lomba | Web Application TECHCOMFEST 2027 |
| Tim | Maksimal 3 orang. Kekuatan tim: fullstack, UI, dan AI/ML |
| Batas submit | 18 Desember 2026, 23.59 WIB. Target internal 17 Desember |
| Feature freeze | 14 Desember 2026 |
| Final | 27 Januari 2027 di Polines, untuk 10 besar |
| Bobot penilaian | Tema/solusi 25% · fungsionalitas/performa 30% · UI/UX 25% · video demo 20% |
| Berkas | Aplikasi ter-hosting, monorepo GitHub, video demo di Google Drive. Tidak boleh memakai template atau website builder |
| Subtema | Belum diputuskan (pilihan lomba: Smart Governance, UMKM/Ekonomi Digital, Digital Wellbeing) |

Standar "wow" yang dipakai tim adalah Pavement-PINN (pemenang negara Intel AI Global Impact Festival 2026). Ciri-cirinya:
- AI yang dibatasi fisika (physics-informed),
- data asli,
- alur tertutup multi-peran,
- halaman bukti berisi metrik terukur,
- angka yang keluar dari mesin perhitungan, bukan dikarang LLM.

## 2. Tujuan

1. Memberi nelayan kecil gambaran per jam tentang seberapa berbahaya ombak di celah keluar-masuk perahu, untuk 72 jam ke depan.
2. Menunjukkan *kenapa* kondisinya begitu: ombak dari mana, periodenya berapa, sedang pasang atau surut. Tujuannya membantu keputusan, bukan menggantikannya.
3. Mempercepat respons kalau ada perahu yang tidak pulang sesuai rencana, lewat ketua kelompok dan Pos SAR.
4. Membuka semua asumsi dan hasil uji di halaman publik **/bukti**, supaya klaim aplikasi bisa dicek juri maupun pengguna.

## 3. Latar belakang

- Ramalan gelombang yang tersedia (BMKG, model global) menggambarkan **laut lepas**. Bahaya terbesar bagi nelayan kecil justru terjadi di **mulut muara**. Di sana ombak meninggi dan pecah karena airnya dangkal, dan tingkat bahayanya berubah setiap jam mengikuti pasang surut.
- Plawangan Puger (Jember) adalah contoh nyata. Celah keluar-masuk perahunya di muara Sungai Besini dulu sekitar 90 m lebar (paper Untirta, data 2019). Citra Sentinel-2 tahun 2026 menunjukkan lebarnya sekitar 120–180 m. Muaranya menghadap barat daya (sekitar 230°), langsung ke arah swell Samudra Hindia.
- Nelayan setempat membaca laut dengan pengalaman (*ilmu titen*). MUARA mencoba menghitung hal yang sama secara fisika dan menampilkannya dengan jujur, lengkap dengan ketidakpastiannya.

## 4. Masalah

1. **Kecelakaan berulang di plawangan.** Paper Untirta mencatat 61 kecelakaan di Puger dari 2014 sampai April 2019: 26 saat berangkat dan 35 saat pulang.
2. **Ramalan "gelombang rendah–sedang" bisa menyesatkan.** Contohnya 25 Sep 2025: di laut lepas ombaknya sekitar 2 m dengan periode sekitar 13 detik. Di celah yang sedang surut, ombak itu meninggi dan pecah.
3. **Tidak ada informasi per jam yang memperhitungkan pasang surut** di titik muara.
4. **Tidak ada sistem sederhana untuk mencatat siapa yang sedang di laut** dan memberi tahu kalau ada yang terlambat pulang.

## 5. Dampak

**Yang dipertaruhkan**
- Sekitar 2.237 perahu di bawah 5 GT tercatat di Puger pada 2019 (paper Untirta).
- Setiap kecelakaan di plawangan berisiko korban jiwa, kehilangan perahu dan alat tangkap, dan hilangnya pendapatan keluarga nelayan.

**Dampak yang diharapkan dari MUARA**
- Nelayan dan ketua kelompok bisa memilih jam keluar-masuk yang relatif lebih tenang, misalnya menunggu pasang.
- Keterlambatan pulang ketahuan lebih cepat, dan ketua bisa langsung menghubungi Pos SAR Jember.
- Pola yang sama bisa diterapkan ke muara lain setelah lomba.

Catatan: dampak ini belum diukur di lapangan. Tim tidak punya akses langsung ke nelayan, jadi yang bisa dibuktikan sebelum lomba adalah konsistensi fisika dan data historis, bukan penurunan kecelakaan.

## 6. Target pengguna

| Persona / role | Kebutuhan | Akses |
|---|---|---|
| **Publik** | Melihat kondisi plawangan, peta, dan bukti | Tanpa login |
| **Nelayan / nakhoda** (persona: Pak Darman) | Tahu jam yang lebih tenang, mencatat rencana melaut, konfirmasi sudah pulang | Nomor HP + PIN, diundang ketua |
| **Ketua kelompok / KUB** (persona: Mas Rizal) | Papan siapa yang sedang di laut, peringatan terlambat pulang, mencatat atas nama anggota | Nomor HP + PIN, dibuat admin |
| **Admin (tim)** | Memantau sumber data, mengatur skenario (berversi dan wajib ada alasan), audit log | Email + password + 2FA |
| **Mitra SAR** (opsional, demo) | Daftar terlambat pulang yang aktif | Akun lihat-saja |

Pemisahan kewenangan:
- Admin tidak bisa mengubah angka keluaran mesin atau catatan melaut.
- Ketua tidak bisa mengubah model.

## 7. Solusi

### Cara kerja mesin

1. **Input:** ramalan gelombang per jam di titik lepas pantai (Open-Meteo, model MFWAM, titik −8,54 / 113,46) dan pasang surut (BIG).
2. **Transformasi gelombang** sepanjang satu garis transek ke mulut muara:
   - relasi dispersi (aproksimasi Hunt),
   - shoaling,
   - refraksi (hukum Snell, kontur dianggap sejajar).
3. **Kriteria pecah:** H ≥ γ(d + η), dengan γ diambil sebagai rentang 0,6–0,9.
4. **Ensemble skenario:** kedalaman celah × γ × debit sungai. Kedalaman celah tidak diketahui pasti, jadi dihitung sebagai rentang.
5. **Keluaran:** tingkat keganasan dibanding pagi-pagi biasa di bulan yang sama (persentil), ditambah rentang tinggi ombak di celah dan jumlah skenario yang memprediksi pecah.
6. **Konteks perahu:** tinggi ombak dibanding lebar kelas perahu, sebagai informasi saja. Kriteria GM **dibuang**, karena paper Untirta tidak memuat data KM atau kurva GZ.
7. **Pengaman BMKG:** kalau Perairan Jember masuk kategori gelombang tinggi atau di atasnya, semua jam ditampilkan "paling berbahaya". Kalau kategorinya sedang, cukup muncul banner. Ketentuan BMKG mewajibkan izin tertulis untuk integrasi ke aplikasi dan melarang scraping. Karena itu, sebelum izin keluar, pengaman ini dinyalakan manual dari halaman Admin dan diberi tautan ke halaman resmi BMKG.
8. **LLM** hanya dipakai untuk membaca foto perahu dan merapikan bahasa penjelasan. Semua angka berasal dari mesin.

### Halaman

Tab bawah di HP: **Kondisi · Peta · Melaut · Bukti · Akun**.

| Halaman | Isi |
|---|---|
| Kondisi | Status sekarang, garis waktu 72 jam (plawangan dan laut lepas), jam yang lebih tenang, banner BMKG |
| Detail jam | Input jam itu, grid skenario, teks penjelasan dari angka mesin, konteks perahu |
| Peta | Peta muara dengan penggeser jam, zona pecah, pita ketidakpastian, garis transek |
| Bukti | Studi kasus, validasi satelit, pasut mesin vs BIG, asumsi, sumber data |
| Melaut | Rencana melaut, tombol "Saya sudah pulang", riwayat |
| Kelompok | Dashboard ketua untuk papan laut, peringatan terlambat, tombol hubungi Pos SAR |
| Admin | Kesehatan data, konfigurasi skenario, akun, audit log |

### Data pengguna yang diambil

**Diambil (seminimal mungkin):**
- nama panggilan, nomor HP, kelompok dan desa,
- data perahu (nama, panjang, lebar),
- rencana melaut: jumlah awak, jam berangkat dan perkiraan pulang, zona tujuan kasar,
- kontak darurat (opsional).

**Sengaja tidak diambil:** NIK, KTP/KK, GPS real-time, nama setiap awak.

Foto perahu dihapus setelah ukurannya dibaca. Catatan melaut dihapus setelah 90 hari. Semua ini mengacu ke UU PDP No. 27/2022.

### Tech stack

- Next.js + Tailwind, dengan peta MapLibre
- API route TypeScript
- Mesin fisika sebagai modul TypeScript dengan unit test numerik
- Supabase (Postgres + Auth), cache per jam, hosting di Vercel
- Skrip Python terpisah untuk eksperimen Sentinel-2

## 8. Data pendukung

### Fakta dari sumber

| Fakta | Sumber |
|---|---|
| 61 kecelakaan di Puger 2014–Apr 2019 (26 berangkat, 35 pulang) | Paper Untirta |
| 2.237 perahu < 5 GT di Puger (2019) | Paper Untirta |
| Celah muara Sungai Besini sekitar 90 m | Paper Untirta |
| Jukung: panjang 12 m, lebar 1,3 m, tinggi 1 m, KG 0,5 m | Paper Untirta |
| Kasus bertanggal: 23 Mar 2021 (4 perahu, 1 terbalik), 1 Agu 2022 (jukung Aliya, ombak ±2 m), 25 Sep 2025 05.52/05.57 WIB (KM Bintang & KM Jara Sanda, ombak dari belakang di pelawangan), 15 Okt 2026 (Tuan Muda, laut lepas Lumajang) | Berita lokal |
| Datum pasut Puger (−8,38 / 113,47): HAT +1,314 · MHWS +0,901 · MSL 0 · MLWS −0,901 · LAT −1,245 m | SRGI BIG |
| Pembanding ambang perahu: Fastnet (ombak pecah ≥35% panjang perahu dari samping); Wolfson Unit (perahu terbuka terbalik di ombak pendek curam 1,1–1,2 m) | Literatur, hanya pembanding |

### Temuan analisis tim (23 November 2026)

Diolah dari Open-Meteo Marine (MFWAM), Okt 2021–Nov 2026:

- **Lokasi titik model penting.** Titik dekat pantai (−8,375) meremehkan ombak. Rata-rata, titik lepas pantai (−8,54) **1,85×** lebih tinggi. Di titik lepas pantai, mediannya 1,8 m, 95% jam di bawah 2,54 m, dan maksimumnya 3,8 m.
- **Validasi awal.** Model di titik lepas pantai memberi **2,04 m** pada 1 Agu 2022, cocok dengan berita yang menyebut ombak sekitar 2 m.
- **Koreksi muka laut.** Muka laut Open-Meteo rata-rata **+0,46 m** di atas MSL BIG. Setelah dikoreksi, sebarannya cocok dengan LAT dan HAT. Semua kedalaman di aplikasi memakai acuan MSL BIG.
- **25 Sep 2025, 05.00 WIB:**
  - ombak lepas pantai 2,0 m, periode 12,85 s, dari 198°,
  - muka laut sekitar −0,95 m (MSL BIG), hampir MLWS,
  - semua 16 skenario memprediksi ombak pecah.
- **Backtest persentil pagi (04.00–07.00):**
  - Hari kecelakaan termasuk 10–30% pagi paling ganas kalau dibandingkan dengan semua pagi (P71–P90, tergantung ukuran yang dipakai).
  - Kalau dibandingkan dengan pagi di bulan yang sama, hari kecelakaan cuma sedikit di atas rata-rata (P62–P70).
  - **Kesimpulan jujur:** model belum bisa diklaim "memprediksi kecelakaan". Juli–Oktober adalah musim swell besar, dan kecelakaan juga bergantung pada berapa banyak perahu yang keluar.
- **Masalah model yang sedang diperbaiki.** Keluaran ya/tidak "pecah" hampir selalu bernilai pecah di jam surut. Karena itu keluaran diganti menjadi tingkat keganasan (persentil). Rentang kedalaman juga dinaikkan ke 2,0–3,5 m thd MSL, dan rentang ini masih perlu diuji.

### Temuan pengumpulan data (24 November 2026)

Semua file ada di folder `TECHCOMFEST\dataset` di laptop tim.

**Data yang terkumpul**

| Data | Isi |
|---|---|
| Gelombang MFWAM, titik −8,54 | Okt 2021–Nov 2026, per jam |
| Gelombang ERA5-Ocean, titik −8,5 / 113,5 | 2016–Nov 2026, per jam. Data sebelum 2016 tidak bisa ditarik |
| ECMWF vs MFWAM, 30 hari | Selisih rata-rata 0,07 m, korelasi 0,97 |
| Prediksi pasut BIG, titik −8,38 / 113,47 | 1 Jan 2016–28 Jan 2027, per jam. Waktu dalam UTC, datum MSL. Mencakup hari final |
| BATNAS v1.6 dan profil transek | 17,1 km dari titik model ke muara. Titik awal sedalam ~880 m (laut dalam). Kedalaman 100 m di 6,8 km, 15 m di 2,3 km, 7 m di 1,1 km dari muara |
| Geometri muara (Sentinel-2) | Tengah celah −8,3874 / 113,4766. Lebar 120–180 m. Muara menghadap ~230° |
| BMKG | Grid InaWaves, prakiraan per jam Perairan Jember (P.O.32) dan Samudra Hindia selatan Jatim (M.12), peringatan gelombang (JSON resmi: dataweb.bmkg.go.id/maritim/fisp/api/warning/warnings.json) |
| Paper Untirta | KG 0,5 m, draft 0,82 m, displasemen 2,41 t (berangkat) / 4,35 t (pulang). Tidak ada KM atau GZ. Jukung berangkat pagi dan pulang sore. Oleng paling besar saat ombak datang dari samping (~65°) |

**Pembanding tiga sumber, 24 Nov 2026 jam 07.00 WIB:**
- ECMWF 1,48 m
- MFWAM 1,34 m
- BMKG InaWaves 1,38 m

Prakiraan BMKG jam 17.00 menunjukkan perbedaan laut lepas dan pantai: Samudra Hindia selatan Jatim 2,1 m, Perairan Jember 1,4 m.

**Koreksi datum.** Muka laut Open-Meteo rata-rata +0,46 m di atas MSL BIG, dengan korelasi 0,92. Kalau dikurangi 0,46 m, sebarannya cocok dengan datum BIG. Prediksi BIG punya titik terendah −1,47 m, lebih rendah dari LAT di popup datum (−1,245 m).

**Backtest per kasus.** Data ERA5 dan pasut BIG. Ukurannya rasio ombak di celah, yaitu nilai tengah dari 16 skenario (kedalaman 2,0–3,5 m × γ 0,6–0,9). Rincian lengkap ada di `kecelakaan_puger.csv`.

| Kasus | Waktu | Gelombang | Pasut BIG | Persentil (semua / bulan sama) |
|---|---|---|---|---|
| 19 Jul 2018 Joko Berek, pulang | 08.15 (pasti) | 3,94 m, Tp 19 s | −0,60 m | P100 / P99,9 |
| 25 Sep 2025 KM Bintang | 05.52 (pasti) | 1,96 m | −1,11 m (surut terendah) | P97,5 / P95,8 |
| 11 Agu 2026, 2 perahu | jendela siang | 3,16 m | −0,93 m | P99,4 / P97,7 |
| 1 Agu 2022 Aliya, berangkat | jendela pagi | 1,98 m | −0,67 m | P78,6 / P68 |
| 16 Agu 2019, 5 perahu saat surut | jendela siang | 1,74 m | −1,05 m | P63,5 / P39 |
| 21 Jul 2018 Anak Manja, berangkat | sore | 2,40 m | −0,10 m | P64 / P35 |
| 5 Mei 2026, berangkat | jendela pagi | 1,68 m | −0,66 m | P47 / P61 |
| 23 Agu 2026, berangkat | jendela pagi | 1,98 m | +0,01 m | P37 / P27 |
| 23 Mar 2021, 4 perahu pulang | jendela sore | 1,82 m | +0,10 m (pasang perbani) | P21 / P36 |

**Kesimpulan jujur dari backtest:**
- Dua kasus yang jamnya pasti (2018 dan 2025) masuk 3% kondisi paling ganas.
- Kecelakaan terjadi dalam **dua pola**:
  1. **Swell ekstrem**, seperti 2018 dan 2026. Ramalan laut lepas sebenarnya sudah menangkap pola ini.
  2. **Ombak biasa bertemu surut di celah**, seperti 2025 dan kemungkinan 2019. Pola ini yang tidak ditangkap ramalan biasa, dan inilah ruang MUARA.
- Empat kasus tidak menonjol di model (2018-07-21, 2021, Mei dan Agu 2026). Penyebabnya bisa model gelombang yang terlalu rendah (untuk kasus 2021, BMKG saat itu menyebut 2–4 m), jam kejadian yang tidak diketahui, atau faktor di luar ombak. Kasus-kasus ini ditampilkan apa adanya di /bukti.
- Dengan 9 kasus, model **tidak** diklaim memprediksi kecelakaan. Yang diklaim adalah bahwa model menangkap kondisi fisik di celah, dan bahwa kasus yang jamnya pasti jatuh di kondisi ekstrem.

**Validasi citra satelit (versi kecil).** Daftar 15 tanggal "ganas" dan 15 tanggal "tenang" menurut model (jam 10.00 WIB, Juni–Oktober 2022–2026) ada di `sentinel_kandidat.csv`. Tim mengukur jarak buih di citra pada tanggal-tanggal itu, lalu hasilnya dibandingkan dengan model.

**Ketentuan pemakaian data** (rincian di `lisensi_data.md`):
- **Open-Meteo:** CC BY 4.0, gratis untuk non-komersial, di bawah 10.000 panggilan per hari.
- **BMKG:** atribusi "Sumber: BMKG". Integrasi ke aplikasi butuh izin tertulis, dan scraping dilarang.
- **BIG:** wajib atribusi. Izin tampil di aplikasi ditanyakan lewat srgi@big.go.id.
- **Sentinel-2:** data terbuka.

### Sumber data aplikasi

| Data | Sumber |
|---|---|
| Gelombang per jam | Open-Meteo Marine (MFWAM, arsip sejak Okt 2021). Pembanding: ECMWF WAM. Cadangan: BMKG InaWaves |
| Gelombang sebelum Okt 2021 | ERA5-Ocean (resolusi 0,5°, keyakinan rendah) |
| Pasang surut | BIG SRGI: datum, konstanta harmonik, prediksi |
| Kedalaman | BATNAS (BIG) untuk lereng di depan muara. Celah dihitung sebagai skenario |
| Geometri muara dan validasi | Citra Sentinel-2 (Copernicus) |
| Peringatan dini | BMKG: JSON peringatan gelombang dan prakiraan Perairan Jember. Otomatis hanya setelah ada izin tertulis |
| Kontak darurat | Pos SAR Jember / Kantor SAR Banyuwangi (Basarnas) |

## 9. Keputusan yang masih terbuka

1. **Cara validasi di /bukti:** rekomendasinya dua-duanya, yaitu kecelakaan ditambah 10–15 citra Sentinel-2. Masih menunggu keputusan tim.
2. **Nama aplikasi:** MUARA atau TITEN.
3. **Subtema lomba** yang dituju.
4. **Izin data:** email ke BIG (srgi@big.go.id) dan surat permohonan izin integrasi ke BMKG (Contact Center 196).
5. **Skripsi ITS** tentang alur Plawangan: perlu diminta lewat form request copy, dan teks alasannya sudah disiapkan. Isinya bisa mempersempit skenario kedalaman celah.

## 10. Dokumen terkait

- Konsep lengkap MUARA: https://claude.ai/artifact/AL3UoCziJgwjRrWnxjyJTf
- Tanggapan atas kritik validitas ilmiah: https://claude.ai/artifact/SuNCyJhEBTxdfpdEdehdTj
- To-do, timeline, dan checklist data (tab To-do, Data, Timeline): https://claude.ai/artifact/XbLuB4bKmQVVD3DZNtFdCp
- Wireframe 8 layar: https://claude.ai/artifact/7SFifYBo8dNgbDUp8WsuAz
