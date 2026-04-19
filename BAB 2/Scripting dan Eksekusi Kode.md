
###  Praktik 1: Pengujian Cross-Site Scripting (XSS)
**Tujuan:** Memastikan aplikasi aman dari injeksi script berbahaya pada input pengguna.

* **Langkah-langkah:**
    1.  Membuka form pendaftaran, buku harian, dan edit profil.
    2.  Memasukkan *payload* XSS: `<script>alert('XSS-Protected')</script>` dan `<img src=x onerror=alert('XSS')>`.
    3.  Menyimpan data dan melihat hasilnya pada halaman tampilan.
<img width="1919" height="962" alt="Screenshot 2026-04-19 084314" src="https://github.com/user-attachments/assets/fd33a6d7-90f9-4f58-a4a9-3346f134969a" />

* **Hasil Pengamatan:**
    - Script tidak berjalan (tidak muncul jendela alert).
    - Input tampil sebagai teks biasa (plain text).
    - **Analisis:** Email harus menggunakan simbol @.

---

###  Praktik 2: Verifikasi Middleware Autentikasi
**Tujuan:** Memastikan halaman sensitif (Dashboard/Diary) tidak dapat diakses tanpa login.

* **Langkah-langkah:**
    1.  Membuka browser dalam mode *Incognito* (tanpa session).
    2.  Mengakses langsung URL: `http://127.0.0.1:8000/dashboard`.
<img width="1919" height="962" alt="image" src="https://github.com/user-attachments/assets/753ecf8f-e638-43e0-a0ee-0c4cfbefd7c6" />

* **Hasil Pengamatan:**
    - Sistem secara otomatis mengarahkan (*redirect*) pengguna kembali ke halaman `/login`.
    - **Analisis:** Rute (Route) telah dilindungi dengan middleware `'auth'` di file `web.php`, sehingga akses ilegal berhasil dicegah.

---

###  Praktik 3: Audit Proteksi CSRF
**Tujuan:** Memastikan setiap pengiriman data (POST) terlindungi dari serangan *Cross-Site Request Forgery*.

* **Langkah-langkah:**
    1.  Melakukan inspeksi kode pada file `resources/views/login.blade.php` dan `resources/views/daftar.blade.php`.
    2.  Mengecek keberadaan direktif `@csrf`.
<img width="705" height="417" alt="image" src="https://github.com/user-attachments/assets/e3dcffa7-d910-4737-804c-42e683636d48" />
<img width="762" height="355" alt="image" src="https://github.com/user-attachments/assets/c219de98-94b4-467d-b8f1-daf45c280428" />

* **Hasil Pengamatan:**
    - Semua elemen `<form>` memiliki `@csrf` yang menghasilkan input hidden berisi token keamanan unik.
    - **Analisis:** Tanpa token ini, Laravel akan menghasilkan error `419 Page Expired`, yang berarti aplikasi menolak permintaan eksternal yang mencurigakan.

---

###  Praktik 4: Review Validasi Controller
**Tujuan:** Memastikan data yang masuk ke server telah difilter dan sesuai format yang diizinkan.

* **Langkah-langkah:**
    1.  Membuka file `app/Http/Controllers/AuthController.php`.
    2.  Mencari fungsi `$request->validate([...])`.
<img width="1467" height="447" alt="image" src="https://github.com/user-attachments/assets/4c878eb2-60a9-4249-abdf-ec958d26fb54" />

* **Hasil Pengamatan:**
    - Fitur login dan register telah menggunakan validasi server-side untuk mengecek tipe data, panjang karakter (min/max), dan keunikan email (`unique:users`).
    - **Analisis:** Validasi ini mencegah pengiriman data kosong atau format yang sengaja dirusak untuk mengacaukan database.

---


