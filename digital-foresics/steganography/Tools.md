# Daftar Tools Steganografi Populer untuk CTF

Berikut adalah daftar tools steganografi yang sering digunakan dalam kompetisi CTF, beserta format file yang didukung dan keterangan singkatnya. Tools ini dapat membantu dalam proses ekstraksi maupun penyisipan pesan tersembunyi.

| Nama Tool         | File yang Didukung      | Keterangan Singkat                                                                                       |
| ----------------- | ----------------------- | -------------------------------------------------------------------------------------------------------- |
| **steghide**      | JPG, JPEG, BMP, WAV, AU | Menyisipkan dan mengekstrak data tersembunyi di file gambar/audio. Sering digunakan, mendukung password. |
| **zsteg**         | PNG                     | Deteksi dan ekstraksi data tersembunyi di file PNG, khususnya LSB dan chunk PNG.                         |
| **stegsolve**     | PNG, JPG, BMP, GIF      | Viewer GUI untuk analisis gambar, termasuk filter warna, LSB, dan channel analysis.                      |
| **binwalk**       | Semua file biner        | Ekstraksi file tersembunyi, signature, dan data terenkapsulasi di file biner/gambar.                     |
| **exiftool**      | JPG, PNG, GIF, dll      | Analisis metadata file, sering digunakan untuk mencari pesan tersembunyi di metadata.                    |
| **stegseek**      | JPG, JPEG               | Brute-force password steghide pada file JPG/JPEG. Sangat cepat untuk wordlist besar.                     |
| **outguess**      | JPG, PPM                | Menyisipkan dan mengekstrak data di gambar, mirip steghide.                                              |
| **wavsteg**       | WAV                     | Menyisipkan dan mengekstrak data di file audio WAV.                                                      |
| **stegcracker**   | JPG, JPEG               | Brute-force password steghide, berbasis Python.                                                          |
| **stegano**       | PNG, JPG, GIF, BMP      | Library Python untuk menyisipkan dan mengekstrak pesan di gambar.                                        |
| **openstego**     | PNG, BMP, GIF, JPG      | Menyisipkan pesan dan watermark di gambar, GUI dan CLI.                                                  |
| **pngcheck**      | PNG                     | Mengecek struktur dan chunk PNG, kadang menemukan data tersembunyi.                                      |
| **gifshuffle**    | GIF                     | Menyisipkan pesan di file GIF dengan mengacak frame.                                                     |
| **jsteg**         | JPG, JPEG               | Menyisipkan pesan di gambar JPEG dengan teknik LSB.                                                      |
| **stegdetect**    | JPG, JPEG               | Deteksi otomatis steganografi di file JPEG.                                                              |
| **audio-steg**    | WAV, MP3                | Tools untuk analisis dan ekstraksi steganografi di audio.                                                |
| **bpcs-stego**    | BMP, PNG                | Menyisipkan pesan dengan metode BPCS (Bit-Plane Complexity Segmentation).                                |
| **snow**          | TXT                     | Menyisipkan pesan tersembunyi di spasi/whitespace file teks.                                             |
| **f5**            | JPG, JPEG               | Menyisipkan pesan di JPEG dengan algoritma F5.                                                           |
| **lsb-steg**      | PNG, BMP, WAV           | Menyisipkan dan mengekstrak pesan dengan teknik LSB.                                                     |
| **stegpy**        | PNG, BMP, GIF           | Library Python untuk steganografi gambar.                                                                |
| **camouflage**    | ZIP, EXE, dll           | Menyembunyikan file dalam file lain (misal: .jpg.exe).                                                   |
| **stegbreak**     | JPG, JPEG               | Brute-force password pada file steghide/outguess/jphide.                                                 |
| **jphide/jpseek** | JPG, JPEG               | Menyisipkan dan mengekstrak pesan di JPEG.                                                               |

> **Tips Penggunaan Tools:**
>
> - Cobalah tools di atas satu per satu jika tidak tahu metode embed yang digunakan.
> - Perhatikan juga metadata, struktur file, dan lakukan analisis manual jika perlu.
> - Beberapa tools membutuhkan dependensi khusus (misal: Java untuk stegsolve).
> - Selalu gunakan wordlist yang relevan saat melakukan brute
