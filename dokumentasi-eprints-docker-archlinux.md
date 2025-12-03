## install arch Linux

### hal-hal yang harus disediakan sebelum menginstall
1. laptop
2. penyimpanan internal yang cukup 50gb maksimal
3. flasdisk
4. file arch-linux iso https://mirror.citrahost.com/archlinux/iso/2025.12.01/
5. rufus https://rufus.ie/en/#google_vignette
   cara install dan menggunakannya ada di webnya

### langkah-langkah
1. jika anda menggunakan windows, buat partisi untuk arch linux dengan menekan shortcut windows + x
2. lalu cari disk management
3. setelah itu klik kanan pada disk yang masih memiliki ruang untuk dijadikan partisi linux minimal 20GB
4. lalu pilih srink volume dan atur menjadi 20GB atau 50GB tergantung sisa penyimpanan disk anda
5. setelah itu sambungkan flasdisk ke laptop
6. lalu buat boot table archlinux.iso kedalam flashdisk dengan menggunakan rufus (perlu diperhatikan jika patition scheme itu harus di singkronisasi dengan device kalian bisa MBR atau GPT) <img src="/img/" alt="github" width="500" height="600">
7. setelah itu restart laptop atau pc anda dan langsung masuk ke boot menu dengan menekan
   - Lenovo = F12
   - Acer   = F12
   - Asus   = F2
   - MSI    = F11
   (merek lain bisa cari di browser)
   saat laptop atau pc anda mati langsung spam tombol untuk ke boot menu sebelum logo merek pada layar anda muncul
8.   lalu pilih USB Disk yang sudah dijadikan boot table archlinux
9. setelah itu buka link ini https://youtu.be/g0u_hSxcUVA?si=1q8uiW-p7aC7Lhvi dan ikuti video tutorialnya dari menit 2.30 agar anda paham cara menginstall archlinux.

## Install e-prints di docker
1. langkah pertama buka terminal lalu ketik

```sql
sudo pacman -Syu docker
```
untuk memperbaharui sistem arch-linux sekaligus menginstall docker untuk e-prints

2. lalu buka https://github.com/DTLudlow/eprints-3.4.4-docker

Dan clone repository tersebut dengan mengklik tombol hijau yang bertuliskan "code" lalu copy yang (HTTPS) 

<img src="/img/Screenshot_20251203_180750.png" alt="github" width="500" height="600"> 
 
3. setelah itu masuk kedalam terminal lalu ketik

```sql
git clone https://github.com/DTLudlow/eprints-3.4.4-docker.git
```

4. lalu masuk kedalam folder e-prints menggunakan

```sql
cd /home/eprints
```

5. selanjutnya membuat container docker dengan mengetik

```sql
docker compose up-build -d
```

Proses ini akan memakan waktu agak lama karena docker sedang membuat volume dan mengambil semua yang dibutuhkan EPrints untuk berjalan.

6. Untuk memastikan EPrints telah dikonfigurasi dengan benar, pastikan prompt terminal ada di folder eprints dengan cara buka kontainer docker di terminal lalu masukkan

```sql
docker compose exec eprintshttpd sh
```

7. lalu jalankan

```sql
chown -R c eprints.eprints /usr/share/eprints/
```

untuk memastikan bahwa pengguna eprints memiliki semua file di direktori eprints.

8. lalu beralih ke pengguna eprints dengan
```sql
su eprints
```

9. setelah itu Jalankan
```sql
/usr/share/eprints/bin/generate_st atic pub --prune
```

untuk memastikan tidak ada tautan yang rusak.

10. lalu jalankan

```sql
/usr/share/eprints/bin/epadmin update pub
```

untuk memastikan pengguna admin ditambahkan ke database.

11. selanjutnya jalankan

```sql
/usr/share/eprints/bin/epadmin reload pub
```

untuk memuat ulang konfigurasi arsip

12. lalu ketik

```sql
exit
```

untuk beralih ke pengguna root.

13. jalankan

```sql
httpd -k restart
```

untuk memulai ulang server web

## memulai sesi

1. Periksa apakah Docker berjalan dengan baik menggunakan

```sql
service docker status
```

2. lalu navigasi ke folder tempat eprints diinstal

```sql
cd /eprints
```

3. setelah itu jalankan eprint dengan mengetik

```sql
docker compose up -d
```

4. lalu buka browser dan buka http://localhost untuk melihat antarmuka front-end.

## pengguna bawaan

untuk masuk atau login ke admin gunakan pengguna bawaan yaitu:

user: admin

password: admin123

## mengakhiri sesi

1. Tutup koneksi ke kontainer Docker dengan mengetik
```sql
exit
```
dua kali (pertama untuk keluar dari pengguna eprints dan kedua untuk keluar dari pengguna root)

2. lalu keluar dari aplikasi EPrints dengan

```sql
docker compose down
```
