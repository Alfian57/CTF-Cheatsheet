# 📖 Penjelasan Google Dorking

## Apa itu Google Dorking?

Google Dorking adalah teknik memanfaatkan fitur pencarian lanjutan Google dengan menggunakan operator khusus (disebut "dorks") untuk menemukan informasi yang tidak mudah ditemukan melalui pencarian biasa. Teknik ini sering digunakan dalam dunia keamanan siber, CTF (Capture The Flag), OSINT (Open Source Intelligence), dan ethical hacking.

## Bagaimana Cara Kerja Google Dorking?

Google mengindeks hampir seluruh konten publik di internet, termasuk file, direktori, konfigurasi, dan halaman yang kadang tidak sengaja terekspos. Dengan operator pencarian tertentu, kita bisa memfilter hasil pencarian untuk menemukan:

- File sensitif (password, konfigurasi, backup)
- Halaman login/admin tersembunyi
- Informasi pribadi (email, username)
- Error atau pesan debug aplikasi
- Kamera atau perangkat IoT yang terbuka

## Operator Dasar Google Dorking

- **"kata kunci"**: Mencari frasa persis.
- **-kata**: Mengecualikan kata tertentu.
- **OR / |**: Salah satu dari beberapa kata kunci.
- **\***: Wildcard, menggantikan satu kata.

## Operator Khusus Google Dorking

- **site:** Membatasi pencarian pada domain tertentu.
- **inurl:** Mencari kata di URL.
- **intitle:** Mencari kata di judul halaman.
- **intext:** Mencari kata di isi halaman.
- **filetype:**/ **ext:** Mencari file dengan ekstensi tertentu.
- **cache:** Melihat versi cache Google.
- **related:** Mencari situs serupa.
- **info:** Informasi singkat tentang domain.

## Etika & Legalitas

Google Dorking sangat powerful, namun harus digunakan secara etis dan legal. Jangan gunakan untuk mengakses atau mengeksploitasi data tanpa izin. Gunakan hanya untuk pembelajaran, CTF, atau pengujian keamanan yang sah.
