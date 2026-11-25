# README — UTS Data Acquisition & Management

## 1. Identitas Mahasiswa

| Keterangan | Informasi |
|---|---|
| Nama | ISI NAMA MAHASISWA |
| NIM | ISI NIM |
| Program Studi | Informatika |
| Institusi | Institut Bisnis dan Teknologi Indonesia (INSTIKI) |
| Mata Kuliah | Data Acquisition & Management |
| Topik Proyek | Sebaran Daya Tarik Wisata di Provinsi Bali |

## 2. Tujuan Proyek

Proyek ini bertujuan mengumpulkan, memeriksa, mengolah, dan menggabungkan data daya tarik wisata di Provinsi Bali menggunakan tiga metode akuisisi: pengunduhan dataset terbuka, pengambilan data melalui API, dan web scraping. Data dinormalisasi dan diintegrasikan untuk menghasilkan data yang dapat diperiksa dan dianalisis.

## 3. Sumber Data

| No. | Sumber | URL | Metode |
|---|---|---|---|
| 1 | data.go.id — Sebaran Daya Tarik Wisata di Bali | https://data.go.id/dataset/dataset/sebaran-daya-tarik-wisata-di-bali | Unduh resource dataset |
| 2 | Wikidata Query Service | https://query.wikidata.org/sparql | SPARQL API |
| 3 | Wikipedia — Kategori Tempat wisata di Bali | https://id.wikipedia.org/wiki/Kategori:Tempat_wisata_di_Bali | Web scraping |

Referensi teknis:
- CKAN package_show: https://docs.ckan.org/en/2.9/api/
- Wikidata Query Service: https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service
- Wikipedia robots.txt: https://id.wikipedia.org/robots.txt

## 4. Metode Akuisisi dan Pengolahan Data

### A. Pengunduhan Dataset Terbuka
Dataset diperoleh dari data.go.id. Notebook menggunakan CKAN `package_show` untuk menemukan resource berdasarkan Dataset ID. File disimpan di `data/raw/`, kemudian dibaca menggunakan Pandas. Pemeriksaan meliputi jumlah baris/kolom, tipe data, nilai kosong, duplikasi, ukuran file, dan estimasi storage.

### B. Akuisisi Data Menggunakan API
Data tambahan diperoleh dari Wikidata Query Service melalui SPARQL. Query mengambil objek wisata yang memiliki koordinat dalam batas geografis Bali. Respons JSON disimpan sebagai data mentah, lalu dinormalisasi menjadi tabel. Status HTTP diperiksa dan request menggunakan timeout, User-Agent, jeda, serta retry.

### C. Web Scraping
Scraping dilakukan pada kategori Wikipedia “Tempat wisata di Bali”. Sebelum scraping, notebook memeriksa `robots.txt`. HTML asli disimpan di `data/raw/`, kemudian BeautifulSoup mengekstrak daftar artikel anggota kategori ke CSV.

### D. Integrasi Data
Dataset data.go.id dan hasil scraping Wikipedia digabungkan. Nama objek dinormalisasi menjadi huruf kecil, aksen/tanda baca dihapus, dan spasi dirapikan. Penggabungan menggunakan `pandas.merge()` dengan `how="inner"` dan `validate="many_to_one"`. Duplikasi diperiksa sebelum hasil disimpan ke CSV. Wikidata menjadi data pendukung terpisah karena pencocokan entitas lintas sumber perlu verifikasi tambahan.

## 5. Hasil Pengumpulan Data

Jumlah baris dan kolom final dicatat otomatis oleh notebook setelah proses akuisisi berhasil. Tidak ada angka hasil yang direkayasa. Hasil aktual tersedia pada tabel ringkasan di bagian akhir notebook.

## 6. Dokumentasi dan Metadata

- `data/raw/`: file dataset asli, JSON API, robots.txt, dan HTML asli.
- `data/processed/`: tabel hasil normalisasi API, scraping, dan integrasi.
- `docs/provenance.json`: URL, waktu akses, lisensi, ukuran file, dan MD5.
- `docs/data_ethics_checklist.csv`: checklist etika 9 butir.
- `notebooks/uts_dam_wisata_bali.ipynb`: seluruh kode dan penjelasan pengerjaan.

## 7. Keterbatasan Data

1. Dataset pemerintah yang tersedia menggambarkan cakupan dan tahun sebagaimana metadata sumber, bukan kondisi terkini secara otomatis.
2. Wikidata dan Wikipedia bersifat kolaboratif sehingga kelengkapan dan konsistensi nama tempat dapat berbeda.
3. Tidak semua objek wisata pasti muncul pada ketiga sumber.
4. Pencocokan nama dapat gagal jika nama berbeda, atau berisiko menyamakan dua objek yang namanya mirip.
5. Halaman data.go.id menyatakan dataset terbuka, tetapi lisensi spesifik tidak terlihat pada metadata yang diperiksa; lisensi tidak diasumsikan.
6. API Wikidata adalah data tambahan, bukan pengganti dataset resmi pemerintah.

## 8. Atribusi dan Lisensi

### A. data.go.id
- Sumber: Pemerintah Provinsi Bali melalui data.go.id.
- Status: Terbuka.
- Lisensi spesifik: tidak tampak pada halaman sumber yang diperiksa; tidak diasumsikan.

### B. Wikidata
- Sumber: Wikidata Query Service.
- Lisensi data terstruktur: CC0.
- Atribusi: Wikidata contributors.

### C. Wikipedia
- Sumber: Wikipedia bahasa Indonesia.
- Lisensi: CC BY-SA; atribusi dan ketentuan share-alike harus dipertahankan.
- Atribusi: Wikipedia contributors, kategori “Tempat wisata di Bali”.

Lisensi dan ketentuan penggunaan harus diperiksa kembali saat tanggal pengumpulan.
