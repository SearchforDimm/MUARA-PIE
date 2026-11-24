# Metode ukur buih dari citra Sentinel-2 (validasi MUARA)

Dikerjakan 24 November 2026 dari screenshot Copernicus Browser (Sentinel-2 L2A, True color, zoom 16, pusat peta -8.38782, 113.47386).

## Yang diukur
1. **Lebar zona pecah (m):** dari garis pantai di sebelah barat jetty, lurus ke laut (tegak lurus pantai), sampai tepi luar pita buih yang masih menyambung. Diukur di tiga titik: sekitar 80, 220, dan 350 m dari jetty (T1, T2, T3). Yang dipakai adalah rata-ratanya.
2. **Buih di celah:** bagian garis mulut celah (dari ujung jetty ke tepi timur) yang tertutup buih. Di atas 60% ditulis "ya", 20–60% "sebagian", di bawah 20% "tidak".

## Aturan yang sama untuk semua citra
- Skala: 1,575 m per piksel layar (zoom 16, Windows 150%), dicek dengan bar skala 200 m.
- Piksel dihitung buih kalau kecerahannya lebih dari kecerahan laut lepas + 40 dan warnanya tidak mencolok (selisih kanal warna di bawah 100).
- Buih boleh mulai sampai 150 m dari pantai. Celah tanpa buih sampai 24 m masih dihitung menyambung.
- Posisi mulut celah dan titik transek sama untuk semua tanggal. Jetty-nya kelihatan di posisi yang sama dari 2022 sampai 2026.
- Citra yang celah atau garis ukurnya kena awan tidak dipakai.

## Kenapa bukan "jarak buih lurus dari mulut celah"
Rencana awalnya mengukur jarak buih lurus ke laut dari mulut celah. Ternyata hasilnya 0 m di semua tanggal, termasuk hari ganas, karena ujung jetty sudah berada di tepi luar zona pecah. Karena itu ukurannya diganti dengan lebar zona pecah di pantai sebelah jetty. Penggantian ini dilakukan setelah data dilihat, dan perlu disebut terus terang di laporan.

## Model pembanding
Rasio celah dan persentil dihitung pada jam 10.00 WIB (Sentinel lewat sekitar 09.50 WIB) dari ERA5 dan pasut BIG. Persentil dibandingkan dengan semua jam 10.00 WIB bulan Juni–Oktober tahun 2016–2026.

## Jadwal satelit
Citra di lokasi ini muncul tiap 5 hari, yaitu di tanggal yang selisihnya dengan 13 Agustus 2022 habis dibagi 5. Pola ini disimpulkan dari tanggal-tanggal citra yang sudah dicek. Satu pengecualian: 14 Agustus 2022.
