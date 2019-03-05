<h1 align="center"><img src="https://upload.wikimedia.org/wikipedia/en/0/0f/MediaWiki_logo_reworked_embroidery.jpg"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) | [Maintenance](#maintenance) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:

# Sekilas Tentang
[`^ kembali ke atas ^`](#)

**MediaWiki** adalah perangkat lunak berbasis server yang dilisensikan di bawah **GNU General Public License** (GPL). **MediaWiki** dirancang untuk berjalan pada *server farm* yang besar dan mendapat jutaan kunjungan per hari. **MediaWiki** merupakan perangkat lunak implementasi dari *wiki* yang sangat andal, terskalakan dan banyak fitur. **MediaWiki** menggunakan **PHP** untuk memproses dan menampilkan data yang tersimpan dalam ***database mysql***.

# Instalasi
[`^ kembali ke atas ^`](#)
Ikuti petunjuk berikut untuk menginstall MediaWiki:

### Step 1: Install Apache2

**MediaWiki** membutuhkan webserver untuk berfungsi dan webserver yang umum digunakan adalah Apache2. Untuk menginstall Apache2 di ubuntu jalankan command berikut:

```
sudo apt-get install apache2
```

Setelah menginstall Apache2, jalankan command berikut untuk menonaktifkan *directory lsting.*

```
sudo sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf
```

Berikutnya jalankan perintah berikut agar apache2 selalu aktif saat server menyala.

```
sudo systemctl stop apache2.service
sudo systemctl start apache2.service
sudo systemctl enable apache2.service
```

### Step 2: Install MariaDB

**MediaWiki** juga membutuhkan database untuk menjalankan fungsinya dan kali ini kita akan menggunakan MariaDB. Untuk meningstall MariaDB jalankan command berikut:

```
sudo apt-get install mariadb-server mariadb-client
```

Berikutnya jalankan perintah berikut agar MariaDB selalu aktif saat server menyala.

```
sudo systemctl stop mariadb.service
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service
````

Setelah itu untuk keamanan jalankan perintah berikut.

```
sudo mysql_secure_installation
```

Dan jawab pertanyaan berikut seperti contoh dibawah ini.

```
Enter current password for root (enter for none): tekan tombol Enter
Set root password? [Y/n]: Y
New password: Enter password
Re-enter new password: Repeat password
Remove anonymous users? [Y/n]: Y
Disallow root login remotely? [Y/n]: Y
Remove test database and access to it? [Y/n]:  Y
Reload privilege tables now? [Y/n]:  Y
```

Dan jalankan ulang MariaDB serve.

```
sudo systemctl restart mariadb.service
```

### Step 3: Install PHP dan Modul yang Diperlukan

**MediaWiki** juga membutuhkan fungsi PHP, Untuk menginstall PHP jalankan command berikut.

```
sudo apt-get install php php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-mcrypt php-ldap php-zip php-curl
```

### Step 4: Membuat MediaWiki Database

Setelah menginstall semua yang dibutuhkan, selanjutnya mulai untuk mengkonfigurasi server. Pertama jalankan perintah berikut untuk masuk ke database server.

```
sudo mysql -u root -p
```

Selanjutnya buat database dengan nama mediawiki.

```
CREATE DATABASE mediawiki;
```

Buat user untuk database dengan nama mediawikiuser dengan password baru.

```
CREATE USER 'mediawikiuser'@'localhost' IDENTIFIED BY 'new_password_here';
```

Dan berikan full akses untuk user.

```
GRANT ALL ON mediawiki.* TO 'mediawikiuser'@'localhost' IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;
```

Terakhir simpan dan exit.

```
FLUSH PRIVILEGES;
EXIT;
```

### Step 5: Download MediaWiki

Berikutnya, jalankan command berikut untuk mendownload dengan command.

```
cd /tmp && wget https://releases.wikimedia.org/mediawiki/1.29/mediawiki-1.29.0.tar.gz
```

Dan jalankan command untuk mengekstrak ke folder Apache2.

```
sudo tar -zxvf mediawiki*.tar.gz
sudo mkdir -p /var/www/html/mediawiki
sudo mv mediawiki-1.29.0/* /var/www/html/mediawiki
```

Mengganti dan memodifikasi *directory permission.*

```
sudo chown -R www-data:wwrw-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```

### Step 6: Configure Apache2
Terakhir, konfigurasi Apache2 site *configuration* file untuk **MediaWiki**. File ini akan mengontrol user yang mengakses konten di **MediaWiki**. Jalankan perintah dibawah untuk membuat fail konfigurasi bernama **mediawiki.conf**.

```
sudo nano /etc/apache2/sites-available/mediawiki.conf
```

Selanjutnya salin kalimat ini dan simpan ke **mediawiki.conf**, untuk nama domain dan lokasi dokumen dapat disesuaikan.

```
<VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /var/www/html/mediawiki/
     ServerName example.com
     ServerAlias www.example.com

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

Simpan dan exit.

### Step 7: Aktifkan MediaWiki

Setelah selesai mengkonfigurasi VirtualHost, aktifkan dengan menjalankan perintah berikut.

```
sudo a2ensite mediawiki.conf
```

### Step 8: Restart Apache2

Untuk menjalankan semua fungsi yang sudah dibuat, *restart* **Apache2** dengan menjalankan perintah berikut.

```
sudo systemctl restart apache2.service
```

Selanjutnya ketikan alamat domain yang sudah dibuat atau IP address dan akan muncul tampilan ***MediaWiki** site setup wizard* seperti gambar dibawah. 



# Konfigurasi
[`^ kembali ke atas ^`](#)

Halaman Utama.

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%201.PNG)

Klik set up the wiki first untuk melakukan konfigurasi awal.

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%202.PNG)

Pilih bahasa yang akan digunakan untuk aplikasi.

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%203.PNG)

Klik lanjut untuk menyetujui persyaratan**MediaWiki**.

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%204.PNG)

Pilih jenis database yang digunakan (aplikasi kami menggunakan database mysql). Lalu gunakan localhost untuk inang basis data (karena aplikasi berjalan di server lokal).

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%205.PNG)

Isi identifikasi wiki dan akun pengguna basis data sesuai proses instalasi.

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%206.PNG)

Isi nama wiki beserta nama proyek.

<![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%207.PNG)

Isi akun pengurus (akun yang akan digunakan untuk login mediawiki).

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%208.PNG)

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%209.PNG)

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%209.PNG)

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%2010.PNG)

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%2011.PNG)

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%2012.PNG)

![alt text](https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%2013.PNG)

Lengkapi sesuai keperluan.



# Maintenance
[`^ kembali ke atas ^`](#)

# Otomatisasi
[`^ kembali ke atas ^`](#)

# Cara Pemakaian
[`^ kembali ke atas ^`](#)

# Pembahasan
[`^ kembali ke atas ^`](#)

# Referensi
[`^ kembali ke atas ^`](#)
