## install arch Linux

### hal-hal yang harus disediakan sebelum menginstall
1. laptop
2. penyimpanan internal yang cukup 50gb maksimal
3. flasdisk
4. file arch-linux iso https://mirror.citrahost.com/archlinux/iso/2025.12.01/

## Install e-prints di docker
langkah pertama buka terminal lalu ketik

```sql
sudo pacman -Syu docker
```
untuk memperbaharui sistem arch-linux sekaligus menginstall docker untuk e-prints

lalu buka https://github.com/DTLudlow/eprints-3.4.4-docker

Dan clone repository tersebut dengan mengklik tombol hijau yang bertuliskan "code" lalu copy yang (HTTPS) 

<img src="" alt="github" width="500" height="600"> 
 
setelah itu masuk kedalam terminal lalu ketik

```sql
git clone https://github.com/DTLudlow/eprints-3.4.4-docker.git
```

lalu masuk kedalam folder e-prints menggunakan

```sql
cd /home/eprints
```

selanjutnya membuat container docker dengan mengetik

```sql
docker compose up-build -d
```

Proses ini akan memakan waktu agak lama karena docker sedang membuat volume dan mengambil semua yang dibutuhkan EPrints untuk berjalan.

Untuk memastikan EPrints telah dikonfigurasi dengan benar, pastikan prompt terminal ada di folder eprints dengan cara buka kontainer docker di terminal lalu masukkan

```sql
docker compose exec eprintshttpd sh
```

lalu jalankan

```sql
chown -R c eprints.eprints /usr/share/eprints/
```

untuk memastikan bahwa pengguna eprints memiliki semua file di direktori eprints.

lalu beralih ke pengguna eprints dengan
```sql
su eprints
```

setelah itu Jalankan
```sql
/usr/share/eprints/bin/generate_st atic pub --prune
```

untuk memastikan tidak ada tautan yang rusak.

lalu jalankan

```sql
/usr/share/eprints/bin/epadmin update pub
```

untuk memastikan pengguna admin ditambahkan ke database.

selanjutnya jalankan

```sql
/usr/share/eprints/bin/epadmin reload pub
```

untuk memuat ulang konfigurasi arsip

lalu ketik

```sql
exit
```

untuk beralih ke pengguna root.

dan jalankan

```sql
httpd -k restart
```

untuk memulai ulang server web

## memulai sesi

Periksa apakah Docker berjalan dengan baik menggunakan

```sql
service docker status
```

lalu navigasi ke folder tempat eprints diinstal

```sql
cd /eprints
```

setelah itu jalankan eprint dengan mengetik

```sql
docker compose up -d
```

lalu buka browser dan buka http://localhost untuk melihat antarmuka front-end.

## pengguna bawaan

untuk masuk atau login ke admin gunakan pengguna bawaan yaitu:

user: admin

password: admin123

## mengakhiri sesi

Tutup koneksi ke kontainer Docker dengan mengetik
```sql
exit
```
dua kali (pertama untuk keluar dari pengguna eprints dan kedua untuk keluar dari pengguna root)

lalu keluar dari aplikasi EPrints dengan

```sql
docker compose down
```
