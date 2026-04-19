

# Laporan Praktik Analisis Keamanan Perimeter (WAF & Throttle) — RuangTenang

Dokumentasi ini merangkum hasil pengujian pada lapisan pertahanan perimeter aplikasi RuangTenang, mencakup mekanisme *Rate Limiting* dan enkripsi jalur komunikasi.

### Praktik 1: Identifikasi Throttle Middleware pada Autentikasi

**Tujuan:** Memastikan aplikasi memiliki mekanisme pertahanan terhadap serangan *brute force* pada halaman login.

  * **Langkah-langkah:**
    1.  Membuka berkas rute aplikasi pada `routes/web.php`.
    2.  Melakukan pencarian terhadap *middleware* `'throttle'` pada rute metode POST untuk login.
    3.  Menganalisis batasan jumlah percobaan masuk yang diizinkan oleh sistem.

<img width="1202" height="673" alt="Screenshot 2026-04-19 164040" src="https://github.com/user-attachments/assets/9aa574a2-85dc-4f58-9a4b-e8eb7f7957ff" />


  * **Hasil Pengamatan:**
      - **Status:** *Middleware* `'throttle'` tidak ditemukan pada rute login maupun daftar.
      - **Konfigurasi Rute:** Sistem hanya menggunakan *middleware* `'guest'` untuk menyaring pengguna yang belum terautentikasi.
      - **Analisis:** Ketiadaan batasan frekuensi percobaan login (*rate limiting*) membuat aplikasi rentan terhadap serangan *brute force*. Tanpa fitur ini, penyerang dapat melakukan percobaan kata sandi secara otomatis dalam jumlah besar tanpa hambatan dari sisi server.

-----

### Praktik 2: Verifikasi Protokol Komunikasi (HTTP vs HTTPS)

**Tujuan:** Memeriksa keamanan jalur komunikasi data antara peramban pengguna dan server aplikasi.

  * **Langkah-langkah:**
    1.  Mengakses aplikasi melalui alamat `http://127.0.0.1:8000`.
    2.  Memperhatikan bilah alamat (*address bar*) pada peramban untuk melihat skema protokol dan indikator keamanan.

<img width="1919" height="960" alt="Screenshot 2026-04-19 164515" src="https://github.com/user-attachments/assets/9f1dc89b-7b76-45dc-b14d-1ca5ac16132b" />


  * **Hasil Pengamatan:**
      - **Protokol:** Masih menggunakan protokol HTTP (Tanpa Enkripsi).
      - **Indikator:** Peramban menampilkan peringatan bahwa koneksi ke situs ini tidak aman.
      - **Analisis:** Penggunaan protokol HTTP pada aplikasi yang menyimpan data sensitif sangat berisiko terhadap serangan *Man-in-the-Middle* (MitM). Data yang dikirimkan oleh pengguna, termasuk catatan buku harian pribadi, dapat disadap dengan mudah karena tidak melalui proses enkripsi SSL/TLS.

