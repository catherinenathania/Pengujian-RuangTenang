
# Laporan Praktik Analisis Secure Coding — RuangTenang

Dokumentasi ini merangkum hasil pengujian pada lapisan kode aplikasi (*Source Code Layer*) untuk memastikan penerapan prinsip *Secure Coding* pada fungsi-fungsi kritis.

### Praktik 1: Verifikasi Hashing Password

**Tujuan:** Memastikan kredensial pengguna tidak disimpan dalam bentuk teks biasa (*plaintext*) di basis data.

  * **Langkah-langkah:** Memeriksa logika autentikasi pada `app/Http/Controllers/AuthController.php`.
  * **Hasil Pengamatan:**
      - **Status:** **Terimplementasi.**
      - **Analisis:** Kode menunjukkan penggunaan `Hash::check` untuk memvalidasi kata sandi yang dimasukkan pengguna dengan data di basis data. Hal ini mengonfirmasi bahwa aplikasi menyimpan kata sandi dalam bentuk *hash* yang aman (menggunakan algoritma Bcrypt sesuai standar Laravel).

<img width="1214" height="315" alt="Screenshot 2026-04-19 170347" src="https://github.com/user-attachments/assets/2d6bcea3-4fb6-455c-96b1-cf22d82e22b5" />


-----

### Praktik 2: Verifikasi Session Regeneration

**Tujuan:** Mencegah serangan *Session Fixation* dengan memperbarui ID sesi setelah pengguna berhasil login.

  * **Langkah-langkah:** Mencari fungsi `regenerate()` pada blok kode `Auth::attempt` di `AuthController.php`.
  * **Hasil Pengamatan:**
      - **Status:** **Tidak Terimplementasi.**
      - **Analisis:** Setelah proses `Auth::attempt` berhasil, kode langsung mengarahkan pengguna ke halaman `/dashboard` tanpa memanggil `$request->session()->regenerate()`. Hal ini menimbulkan risiko keamanan di mana penyerang dapat membajak sesi pengguna jika ID sesi tetap sama sebelum dan sesudah login.

<img width="1344" height="310" alt="Screenshot 2026-04-19 170441" src="https://github.com/user-attachments/assets/51471e3c-c575-46a0-b9ae-b0344158b272" />


-----

### Praktik 3: Proteksi Cross-Site Request Forgery (CSRF)

**Tujuan:** Memastikan setiap permintaan POST dari formulir memiliki token keamanan untuk mencegah serangan CSRF.

  * **Langkah-langkah:** Memeriksa keberadaan direktif `@csrf` pada file Blade formulir.
  * **Hasil Pengamatan:**
      - **Formulir Daftar:** **Terimplementasi.** Terdapat direktif `@csrf` di dalam tag `<form>`.
      - **Formulir Login:** **Terimplementasi.** Terdapat direktif `@csrf` sebelum elemen input email.
      - **Analisis:** Laravel secara otomatis akan menolak permintaan POST yang tidak menyertakan token ini, sehingga melindungi pengguna dari eksekusi perintah yang tidak disengaja.

<img width="1174" height="301" alt="Screenshot 2026-04-19 170529" src="https://github.com/user-attachments/assets/4fbc7a4c-4b02-41c0-9105-6d6e5266ea7d" />
<img width="1151" height="293" alt="Screenshot 2026-04-19 170548" src="https://github.com/user-attachments/assets/e58df617-d66c-4cfe-a635-80d9e259a118" />


-----

### Praktik 4: Validasi Input Server-Side

**Tujuan:** Mencegah data berbahaya atau tidak sesuai masuk ke dalam sistem melalui pemfilteran di sisi server.

  * **Langkah-langkah:** Memeriksa penggunaan metode `$request->validate()` pada fungsi login di `AuthController.php`.
  * **Hasil Pengamatan:**
      - **Status:** **Terimplementasi.**
      - **Analisis:** Aplikasi melakukan validasi terhadap input `email` (wajib dan format email) serta `password` (wajib dan minimal 6 karakter). Validasi ini sangat penting karena validasi sisi klien (HTML5/JS) mudah dimanipulasi oleh penyerang menggunakan alat seperti Burp Suite atau Postman.

<img width="1188" height="292" alt="Screenshot 2026-04-19 170655" src="https://github.com/user-attachments/assets/91a35800-2f11-4ec4-bb32-5e7d05b6427d" />


