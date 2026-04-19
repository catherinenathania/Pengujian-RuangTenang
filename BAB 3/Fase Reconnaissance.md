
# Laporan Praktik Reconnaissance — RuangTenang

Dokumentasi ini merangkum hasil pengujian pada tahap *reconnaissance* untuk mengidentifikasi teknologi dan celah konfigurasi pada aplikasi RuangTenang.

### Praktik 1: Fingerprinting Teknologi

**Tujuan:** Mengidentifikasi tumpukan teknologi (*technology stack*) yang digunakan oleh aplikasi.

  * **Langkah-langkah:**
    1.  Mengaktifkan ekstensi browser Wappalyzer pada peramban.
    2.  Mengakses alamat aplikasi lokal `http://127.0.0.1:8000`.
    3.  Menganalisis hasil deteksi pada panel Wappalyzer.
       
<img width="1919" height="953" alt="Screenshot 2026-04-19 154830" src="https://github.com/user-attachments/assets/bb33c39f-b3b4-4c84-925d-b11199376f01" />


  * **Hasil Pengamatan:**
      - **Framework:** Terdeteksi menggunakan Laravel.
      - **Bahasa Pemrograman:** PHP versi 8.2.12.
      - **Server:** Apache (melalui tumpukan XAMPP).
      - **Analisis:** Informasi versi perangkat lunak yang terpapar secara spesifik dapat mempermudah penyerang dalam mencari celah eksploitasi yang sesuai dengan versi tersebut.

-----

### Praktik 2: Pengujian Akses Direktori Sensitif

**Tujuan:** Memastikan berkas konfigurasi dan direktori internal tidak dapat diakses melalui publik.

  * **Langkah-langkah:**
    1.  Melakukan percobaan akses langsung pada URL sensitif: `/.env`, `/storage`, `/vendor`, dan `/phpmyadmin`.
    2.  Mengamati respon kode status HTTP yang diberikan oleh server.


<img width="1919" height="943" alt="Screenshot 2026-04-19 154939" src="https://github.com/user-attachments/assets/7c94dca0-4707-4324-9b7c-4d4a139042e8" />
<img width="1919" height="698" alt="Screenshot 2026-04-19 155026" src="https://github.com/user-attachments/assets/77fb1789-e927-48ed-8eaa-b63c178369c1" />


  * **Hasil Pengamatan:**
      - **Akses `/.env`:** Menghasilkan respon *404 Not Found*.
      - **Akses `/phpmyadmin`:** Antarmuka pengelolaan basis data terbuka secara langsung.
      - **Analisis:** Berkas konfigurasi `.env` berhasil terlindungi oleh struktur folder publik Laravel. Namun, akses terbuka ke *phpMyAdmin* menunjukkan kelemahan pada konfigurasi keamanan layanan server lokal.

-----

### Praktik 3: Audit Konfigurasi APP\_DEBUG

**Tujuan:** Memeriksa status mode perbaikan (*debug mode*) pada berkas konfigurasi utama.

  * **Langkah-langkah:**
    1.  Membuka berkas `.env` pada direktori *root* aplikasi.
    2.  Memeriksa nilai pada variabel `APP_DEBUG`.

<img width="1026" height="427" alt="Screenshot 2026-04-19 155405" src="https://github.com/user-attachments/assets/ddab4ca9-e7d3-4418-8665-3af818526dd5" />


  * **Hasil Pengamatan:**
      - **Status:** `APP_DEBUG=true`.
      - **Analisis:** Mode *debug* yang aktif dalam lingkungan non-pengembangan sangat berbahaya karena akan menampilkan informasi teknis detail (seperti kueri SQL dan variabel sistem) saat terjadi kesalahan. Hal ini merupakan temuan keamanan serius yang harus segera diubah menjadi `false` sebelum rilis produksi.

-----

