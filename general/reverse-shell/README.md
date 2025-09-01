# Shell, Reverse Shell, dan Bind Shell

## Apa itu Shell?

Shell adalah antarmuka yang memungkinkan pengguna untuk berinteraksi dengan sistem operasi, biasanya melalui baris perintah (command line). Dalam konteks keamanan, shell sering digunakan untuk mengontrol sistem target setelah berhasil dieksploitasi.

## Apa itu Reverse Shell?

Reverse shell adalah teknik di mana target (korban) membuka koneksi keluar ke attacker, lalu attacker mendapatkan akses shell ke sistem target. Reverse shell sering digunakan untuk melewati firewall atau NAT, karena koneksi dimulai dari dalam jaringan korban.

**Ilustrasi Reverse Shell:**

```
[Attacker] <--- [Firewall/NAT] <--- [Target]
      ^                             |
      |---------(Reverse)-----------|
```

## Apa itu Bind Shell?

Bind shell adalah teknik di mana target membuka port dan menunggu koneksi masuk dari attacker. Attacker kemudian menghubungkan ke port tersebut untuk mendapatkan akses shell.

**Ilustrasi Bind Shell:**

```
[Attacker] -----> [Firewall/NAT] -----> [Target (Bind Port)]
```

## Perbedaan Reverse Shell dan Bind Shell

- **Reverse Shell:** Target menghubungi attacker.
- **Bind Shell:** Attacker menghubungi target.
- **Reverse Shell** lebih sering digunakan untuk melewati firewall.

---

Lanjutkan ke [Cheatsheet.md](./Cheatsheet.md) untuk payload dan teknik eksploitasi.
