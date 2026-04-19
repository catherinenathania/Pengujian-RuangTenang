

# Laporan Praktik Analisis Ekosistem Pihak Ketiga — RuangTenang

Dokumentasi ini merangkum hasil audit terhadap penggunaan *library*, *framework*, dan komponen pihak ketiga pada aplikasi RuangTenang guna mengidentifikasi potensi risiko *supply chain attack*.

### Praktik 1: Verifikasi Versi Framework Laravel

**Tujuan:** Memastikan *framework* utama yang digunakan berada pada versi yang didukung dan memiliki pembaruan keamanan terbaru.

  * **Langkah-langkah:**
    1.  Membuka berkas `composer.json` dan `composer.lock` pada direktori proyek.
    2.  Mencari entitas `laravel/framework` untuk melihat detail versi yang terpasang.

<img width="1212" height="333" alt="Screenshot 2026-04-19 161245" src="https://github.com/user-attachments/assets/153a35d7-77de-4c2a-9151-4e8a6b8e1257" />


  * **Hasil Pengamatan:**
      - **Versi Terdeteksi:** Laravel v12.15.0.
      - **Analisis:** Aplikasi menggunakan Laravel versi 12 yang merupakan versi terbaru dengan dukungan keamanan penuh. Penggunaan versi terkini meminimalisir risiko eksploitasi pada celah keamanan yang sudah dipublikasikan (*known vulnerabilities*).

-----

### Praktik 2: Audit Paket Dependency Composer

**Tujuan:** Mengidentifikasi paket tambahan dan memeriksa adanya riwayat kerentanan pada pangkalan data CVE.

  * **Langkah-langkah:**
    1.  Menganalisis berkas `composer.lock` untuk mendata paket pendukung seperti Guzzle dan FakerPHP.
    2.  Melakukan verifikasi versi paket terhadap *National Vulnerability Database* (NVD).

<img width="787" height="297" alt="Screenshot 2026-04-19 161443" src="https://github.com/user-attachments/assets/ac3682ff-df42-4e30-9c6b-5d12649de7c8" />
<img width="782" height="320" alt="Screenshot 2026-04-19 161422" src="https://github.com/user-attachments/assets/8fcd6428-0b79-499c-a2f4-812c57c2b967" />
<img width="736" height="686" alt="Screenshot 2026-04-19 161543" src="https://github.com/user-attachments/assets/714b0f1f-358a-498f-ace4-6f225c18cf94" />


  * **Hasil Pengamatan:**
      - **Guzzle HTTP:** Menggunakan versi 7.9.3 yang stabil untuk menangani permintaan HTTP.
      - **FakerPHP:** Menggunakan versi 1.24 sebagai alat bantu pembuatan data simulasi.
      - **Analisis:** Paket-paket utama yang digunakan merupakan pustaka standar yang sering diperbarui oleh komunitas. Hingga saat ini, tidak ditemukan *Security Advisory* kritis pada versi yang terpasang tersebut.

-----

### Praktik 3: Pemeriksaan Library Frontend

**Tujuan:** Mengevaluasi penggunaan pustaka sisi klien dan potensi risiko pemuatan skrip eksternal.

  * **Langkah-langkah:**
    1.  Memeriksa berkas tampilan pada `resources/views/dashboard.blade.php`.
    2.  Mencari penggunaan *library* seperti jQuery atau Bootstrap serta pemuatan melalui CDN.

<img width="1199" height="186" alt="Screenshot 2026-04-19 161747" src="https://github.com/user-attachments/assets/5d9aa1ba-411a-4d87-8f2d-f04fa7352292" />


  * **Hasil Pengamatan:**
      - **Pencarian Skrip:** Sistem tidak menemukan penggunaan *library* jQuery pada halaman utama.
      - **Interaksi:** Logika interaktif seperti *mood emoji* dibangun menggunakan JavaScript murni (*Vanilla JS*).
      - **Analisis:** Pengurangan ketergantungan pada pustaka pihak ketiga seperti jQuery dapat memperkecil area serangan (*attack surface*). Hal ini juga mengurangi risiko serangan *Content Delivery Network* (CDN) *poisoning*.

-----

### Praktik 4: Audit Konfigurasi Lingkungan (XAMPP/Default)

**Tujuan:** Mengidentifikasi kelemahan pada pengaturan akses basis data yang masih bersifat *default*.

  * **Langkah-langkah:**
    1.  Membuka berkas konfigurasi `.env`.
    2.  Memeriksa variabel `DB_USERNAME` dan `DB_PASSWORD`.

<img width="871" height="157" alt="Screenshot 2026-04-19 161904" src="https://github.com/user-attachments/assets/dddd449c-c98f-4dea-a3e2-7763e63120f1" />


  * **Hasil Pengamatan:**
      - **Kredensial:** Menggunakan *username* `root` dengan kata sandi yang dikosongkan.
      - **Analisis:** Konfigurasi ini merupakan pengaturan standar (*default*) pengembangan pada XAMPP. Penggunaan akun *root* tanpa kata sandi sangat berbahaya jika aplikasi diakses melalui jaringan publik. Perubahan kredensial menjadi kewajiban mutlak sebelum sistem masuk ke tahap produksi.

