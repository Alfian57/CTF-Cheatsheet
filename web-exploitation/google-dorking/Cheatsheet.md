# 📌 Cheatsheet Google Dorking untuk CTF

---

## 1. Penggunaan Operator Dasar

| Operator       | Fungsi                | Contoh            |
| -------------- | --------------------- | ----------------- |
| `"kata kunci"` | Frasa persis          | `"admin login"`   |
| `-kata`        | Kecualikan kata       | `admin -login`    |
| `*`            | Wildcard (kata bebas) | `"admin * login"` |
| `OR` / `\|`    | Salah satu keyword    | `admin OR user`   |

---

## 2. Penggunaan Operator Khusus

| Operator           | Fungsi                        | Contoh                |
| ------------------ | ----------------------------- | --------------------- |
| `site:`            | Domain tertentu               | `site:example.com`    |
| `inurl:`           | Keyword di URL                | `inurl:admin`         |
| `intitle:`         | Keyword di judul halaman      | `intitle:"index of"`  |
| `intext:`          | Keyword di isi halaman        | `intext:password`     |
| `filetype:`/`ext:` | File dengan ekstensi tertentu | `filetype:pdf`        |
| `cache:`           | Melihat cache Google          | `cache:example.com`   |
| `related:`         | Website mirip domain          | `related:example.com` |
| `info:`            | Info singkat domain           | `info:example.com`    |

---

## 3. Contoh Dorking untuk Berbagai Kebutuhan

### a. Mencari Informasi Sensitif

- `filetype:txt password`
- `filetype:log password`
- `filetype:xls site:example.com`
- `intitle:"index of" "parent directory"`
- `"login" inurl:admin`
- `site:example.com inurl:login`

### b. File & Directory Listing

- `intitle:"index of /"`
- `intitle:"index of /" + passwd`
- `intitle:"index of /admin"`
- `intitle:"index of /backup"`

### c. Halaman Login & Admin Panel

- `inurl:admin login`
- `inurl:admin intitle:login`
- `inurl:login site:example.com`
- `"admin" "login" "username" "password"`

### d. Database & Config Files

- `filetype:sql site:example.com`
- `filetype:db site:example.com`
- `filetype:cfg site:example.com`
- `filetype:env site:example.com`

### e. Kamera / IoT

- `inurl:"/view.shtml"`
- `inurl:"/viewerframe?mode="`
- `intitle:"webcamXP 5"`
- `inurl:":8080"`

### f. Email & Usernames

- `"@gmail.com" site:example.com`
- `"@yahoo.com" site:example.com`
- `filetype:xls email`

### g. Vulnerable Pages

- `inurl:"php?id="`
- `inurl:".php?id="` (SQLi)
- `"Warning: mysql_fetch"` (error database)
- `"Warning: include("` (path disclosure)

---

## 4. Kombinasi Dorking untuk CTF

- `site:example.com filetype:pdf "confidential"`
- `site:example.com intitle:"index of" backup`
- `site:gov inurl:admin`

---

## 5. Tips Praktis

- **Bypass limit:** Gabungkan beberapa operator, gunakan wildcard, atau variasikan kata kunci.
- **Google Advanced Search:** Mempercepat pencarian dengan filter visual.
- **Wayback Machine:** Temukan versi lama halaman dengan `site:example.com`.
- **Gunakan tab incognito:** Untuk hasil pencarian yang lebih netral.
- **Eksperimen dengan sinonim:** Misal, `login`, `signin`, `masuk`, dsb.

---

> **Catatan:** Selalu gunakan Google Dorking secara etis dan hanya untuk tujuan legal seperti CTF, pembelajaran, atau pengujian keamanan yang diizinkan.
