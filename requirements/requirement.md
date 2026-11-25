# REQUIREMENTS.md
# Mini CBT – Aplikasi Ujian Online Sederhana

## 1. Gambaran Aplikasi

Mini CBT adalah aplikasi ujian online sederhana yang dibuat untuk membantu guru menyelenggarakan ujian bagi siswa.

Aplikasi ini memungkinkan guru menyiapkan soal melalui Google Sheets, kemudian siswa mengerjakan ujian melalui browser HP atau komputer.

Aplikasi memiliki dua jenis soal:
- **Pilihan Ganda:** dinilai otomatis oleh sistem.
- **Esai:** dinilai menggunakan bantuan AI dan hasilnya dapat diperiksa kembali oleh guru.

## 2. Teknologi yang Digunakan

Aplikasi dibuat menggunakan:
- **Google Sheets:** menyimpan data ujian, soal, dan hasil pengerjaan siswa.
- **Google Apps Script:** menghubungkan aplikasi dengan Google Sheets dan menjalankan proses aplikasi.
- **HTML, CSS, dan JavaScript:** membuat tampilan aplikasi.
- **Gemini API:** membantu memeriksa jawaban esai siswa.

Tidak perlu menggunakan database atau layanan hosting tambahan.

## 3. Pengguna Aplikasi

Ada dua jenis pengguna:

**A. Guru**
- Membuat dan mengatur ujian melalui Google Sheets.
- Menambahkan soal pilihan ganda dan esai.
- Melihat hasil ujian siswa.
- Memeriksa hasil penilaian esai dari AI.

**B. Siswa**
- Masuk menggunakan kode ujian, nama, dan NIS.
- Melihat soal ujian.
- Memilih jawaban atau mengetik jawaban esai.
- Mengirim jawaban dan melihat konfirmasi bahwa ujian telah selesai.

Siswa tidak perlu membuat akun atau login menggunakan Google.

## 4. Fitur Utama

### A. Halaman Masuk Ujian

Siswa harus mengisi:
- Kode ujian.
- Nama siswa.
- NIS (Nomor Induk Siswa).

Sistem memeriksa apakah kode ujian benar dan ujian sedang aktif sebelum siswa dapat masuk.

### B. Halaman Pengerjaan Ujian

Setelah berhasil masuk, siswa dapat:
- Melihat judul dan petunjuk ujian.
- Melihat daftar soal.
- Menjawab soal pilihan ganda.
- Menulis jawaban esai.
- Melihat sisa waktu ujian.

Tampilan harus sederhana, mudah digunakan, dan nyaman di HP maupun komputer.

### C. Timer Ujian

- Durasi ujian ditentukan oleh guru.
- Waktu mulai dan batas akhir ujian dicatat oleh sistem.
- Timer tetap akurat meskipun halaman dimuat ulang.
- Ketika waktu habis, jawaban yang tersimpan otomatis dikirim.
- Siswa tidak boleh memperpanjang waktu dengan mengubah jam perangkat.

### D. Penyimpanan Jawaban

- Jawaban siswa disimpan sementara di browser agar tidak mudah hilang ketika halaman tidak sengaja dimuat ulang.
- Jawaban dikirim ke server ketika siswa menekan tombol Kirim atau waktu ujian habis.
- Sistem memberikan konfirmasi setelah jawaban berhasil dikirim.
- Satu siswa hanya memiliki satu pengerjaan untuk satu ujian. Jika membuka kembali ujian yang masih berlangsung, siswa melanjutkan pengerjaan sebelumnya.

### E. Penilaian Pilihan Ganda

- Sistem memeriksa jawaban berdasarkan kunci jawaban yang disimpan di Google Sheets.
- Nilai dihitung secara otomatis.
- Kunci jawaban tidak boleh terlihat oleh siswa.

### F. Penilaian Esai dengan AI

- Jawaban esai dikirim ke Gemini API untuk dinilai.
- AI menggunakan jawaban acuan dan rubrik penilaian yang dibuat guru.
- AI memberikan nilai dan umpan balik untuk setiap jawaban.
- Guru tetap dapat memeriksa dan mengoreksi hasil penilaian AI.
- Jika AI gagal memberikan penilaian, jawaban tidak boleh otomatis dianggap salah atau mendapat nilai nol. Jawaban harus ditandai untuk diperiksa guru.
- Di google sheet terdapat menu untuk menilai esai dengan AI untuk baris yang dicentang sebagai alternatif jika AI sedang sibuk dan gagal menilai saat siswa mengirim jawaban.

### G. Hasil Ujian

Guru dapat melihat hasil pengerjaan siswa melalui Google Sheets, meliputi:
- Nama dan NIS siswa.
- Waktu mulai dan waktu pengiriman.
- Nilai pilihan ganda.
- Nilai esai.
- Nilai akhir.
- Status penilaian dan umpan balik AI.

Nilai akhir yang melibatkan penilaian esai perlu ditinjau guru sebelum dianggap sebagai nilai final.

## 5. Struktur Google Sheets

Aplikasi menggunakan satu file Google Sheets sebagai database, yang terdiri dari lima sheet (tab).

Struktur setiap sheet mengacu pada lima file CSV yang tersedia di dalam folder `requirements`.

### File acuan struktur database

| File CSV | Nama Sheet di Google Sheets | Fungsi |
|---|---|---|
| `database_mini_cbt_Pengaturan.csv` | Pengaturan | Menyimpan konfigurasi dasar aplikasi |
| `database_mini_cbt_Ujian.csv` | Ujian | Menyimpan informasi dan pengaturan ujian |
| `database_mini_cbt_Soal.csv` | Soal | Menyimpan daftar soal, pilihan jawaban, kunci jawaban, dan rubrik penilaian |
| `database_mini_cbt_Pengerjaan.csv` | Pengerjaan | Menyimpan identitas siswa, jawaban, waktu pengerjaan, dan hasil penilaian |
| `database_mini_cbt_Log.csv` | Log | Mencatat aktivitas dan kesalahan sistem |

### Lokasi file dalam project

Pastikan struktur folder project seperti berikut:

```text
project-mini-cbt/
│
├── backend/
│   ├── Code.gs
│
├── frontend-siswa/
│   ├── Index.html
│   ├── css dan javascript
│
├── requirements/
│   ├── requirement.md
│   ├── database_mini_cbt_Pengaturan.csv
│   ├── database_mini_cbt_Ujian.csv
│   ├── database_mini_cbt_Soal.csv
│   ├── database_mini_cbt_Pengerjaan.csv
│   └── database_mini_cbt_Log.csv
│

```

### Aturan penggunaan file CSV

1. Kelima file CSV merupakan **acuan utama struktur database** aplikasi.
2. Gunakan nama kolom dan urutan kolom sesuai file CSV masing-masing.
3. Setiap file CSV merepresentasikan satu sheet dalam Google Sheets.
4. Jangan mengubah nama kolom atau menghapus kolom tanpa memperbarui kode aplikasi.
5. Gunakan struktur tersebut secara konsisten ketika membaca, menyimpan, dan memperbarui data.
6. Jika ada kebutuhan menambah kolom, pastikan perubahan diterapkan pada file CSV dan kode aplikasi.

**Catatan:** Sebelum membuat kode aplikasi, baca seluruh file CSV di folder `requirements` untuk memahami struktur data, nama kolom, dan hubungan antar-sheet. Jangan membuat struktur database sendiri atau mengasumsikan nama kolom yang tidak tercantum dalam file acuan.
Pastikan NIS Siswa yang diinput ke dalam database google sheet berupa format teks, bukan angka.


## 6. Aturan Penting

1. Semua data ujian dan jawaban disimpan di Google Sheets.
2. Kunci jawaban, jawaban acuan, dan rubrik penilaian tidak boleh dikirim ke halaman siswa.
3. Perhitungan nilai dilakukan oleh sistem, bukan oleh browser siswa.
4. Satu siswa hanya dapat mengerjakan satu kali untuk setiap ujian.
5. Sistem harus mencegah pengiriman jawaban berulang yang menyebabkan data atau nilai menjadi ganda.
6. Jika koneksi bermasalah, aplikasi harus menampilkan pesan yang jelas dan memberikan kesempatan untuk mencoba mengirim kembali.
7. API Key Gemini disimpan secara aman di pengaturan Google Apps Script, bukan di kode yang dapat dilihat siswa.
8. Data pribadi siswa tidak perlu dikirim ke Gemini. AI hanya membutuhkan soal, jawaban siswa, jawaban acuan, dan rubrik penilaian.

## 7. Tampilan Aplikasi

Gunakan desain yang:
- Sederhana dan modern.
- Menggunakan bahasa Indonesia.
- Mudah dipahami guru dan siswa.
- Responsif untuk HP dan komputer.
- Memiliki tombol dan tulisan yang jelas.
- Menampilkan indikator loading ketika sedang memproses data.
- Menampilkan pesan jika terjadi kesalahan.

Tidak perlu membuat dashboard guru yang kompleks. Pengelolaan data cukup dilakukan melalui Google Sheets.

## 8. Batasan Project

Aplikasi ini merupakan MVP (versi awal) untuk kebutuhan satu guru dan satu kelas, dengan target pengujian sekitar 30–40 siswa.

Belum perlu membuat:
- Login menggunakan akun Google.
- Banyak sekolah atau banyak guru.
- Sistem pengawasan ujian (proctoring).
- Import soal dari Word atau PDF.
- Dukungan ujian offline sepenuhnya.
- Integrasi WhatsApp atau email.
- Dashboard analitik yang kompleks.

Aplikasi perlu diuji dengan beberapa siswa yang mengerjakan secara bersamaan sebelum digunakan dalam ujian sebenarnya.

## 9. Kriteria Keberhasilan

Project dianggap berhasil apabila:

- Guru dapat membuat ujian dan soal melalui Google Sheets.
- Siswa dapat masuk menggunakan kode ujian, nama, dan NIS.
- Siswa dapat mengerjakan soal pilihan ganda dan esai.
- Timer berjalan sesuai durasi ujian.
- Jawaban dapat disimpan dan dikirim.
- Nilai pilihan ganda dihitung otomatis.
- AI dapat membantu menilai jawaban esai.
- Guru dapat melihat dan memeriksa hasil ujian.
- Aplikasi dapat digunakan melalui HP dan komputer.
- Aplikasi berhasil dipublikasikan sebagai Google Apps Script Web App.

---

**Prinsip pengembangan:** Buat aplikasi sesederhana mungkin, pastikan fitur utama berjalan dengan baik, dan utamakan kemudahan penggunaan oleh guru dan siswa.


## 10. File pendukung
- URL Lokasi Google Sheets: url-google-sheet
- API Key Gemini: your-key