# Steganografi: Penjelasan dan Konsep Dasar

Steganografi adalah seni dan ilmu menyembunyikan pesan, file, atau data di dalam objek lain sehingga keberadaan pesan tersebut tidak terdeteksi oleh orang lain. Berbeda dengan kriptografi yang menyembunyikan isi pesan, steganografi menyembunyikan eksistensi pesan itu sendiri.

Pada kompetisi CTF (Capture The Flag), steganografi sering digunakan untuk menyembunyikan flag atau petunjuk di dalam file gambar, audio, video, atau bahkan dokumen teks. Teknik yang umum digunakan antara lain:

- **Least Significant Bit (LSB):** Menyisipkan data pada bit paling tidak signifikan di file gambar/audio.
- **Manipulasi Metadata:** Menyembunyikan pesan di metadata file (EXIF, ID3, dsb).
- **Chunk/Segment Manipulation:** Menyisipkan data di bagian struktur file yang jarang diperiksa.
- **Whitespace/Text Steganography:** Menyembunyikan pesan di spasi, tab, atau karakter tak terlihat pada file teks.
- **Watermarking:** Menyisipkan pesan sebagai watermark digital.

> **Tips Umum Analisis Steganografi:**
>
> - Selalu cek metadata file.
> - Bandingkan hash file asli dan file yang dicurigai.
> - Analisis struktur file dengan tools hex editor.
> - Coba buka file dengan berbagai aplikasi viewer/editor.
> - Gunakan tools steganografi secara berurutan jika tidak tahu teknik yang dipakai.
