<h1 align="center"><img src="https://upload.wikimedia.org/wikipedia/en/0/0f/MediaWiki_logo_reworked_embroidery.jpg"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) | [Maintenance](#maintenance) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:

# Sekilas Tentang
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%201.PNG">
[`^ kembali ke atas ^`](#)

**MediaWiki** adalah perangkat lunak berbasis server yang dilisensikan di bawah **GNU General Public License** (GPL). **MediaWiki** dirancang untuk berjalan pada *server farm* yang besar dan mendapat jutaan kunjungan per hari. **MediaWiki** merupakan perangkat lunak implementasi dari *wiki* yang sangat andal, terskalakan dan banyak fitur. **MediaWiki** menggunakan **PHP** untuk memproses dan menampilkan data yang tersimpan dalam ***database mysql***.

# Instalasi
[`^ kembali ke atas ^`](#)

Ikuti petunjuk berikut untuk menginstall MediaWiki

## Step 1: Install Apache2

**MediaWiki** membutuhkan webserver untuk berfungsi dan webserver yang umum digunakan adalah Apache2. Untuk menginstall Apache2 di ubuntu jalankan command berikut:

```bash
sudo apt-get install apache2
```

setelah menginstall Apache2, jalankan command berikut untuk menonaktifkan *directory lsting.*

```bash
sudo sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf
```
berikutnya jalankan perintah berikut agar apache2 selalu aktif saat server menyala

```bash
sudo systemctl stop apache2.service
sudo systemctl start apache2.service
sudo systemctl enable apache2.service
```
## Step 2: Install MariaDB

**MediaWiki** juga membutuhkan database untuk menjalankan fungsinya dan kali ini kita akan menggunakan MariaDB. Untuk meningstall MariaDB jalankan command berikut:

```bash
sudo apt-get install mariadb-server mariadb-client
```

berikutnya jalankan perintah berikut agar MariaDB selalu aktif saat server menyala
```bash
sudo systemctl stop mariadb.service
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service
````
setelah itu untuk keamanan jalankan perintah berikut
```bash
sudo mysql_secure_installation
```
dan jawab pertanyaan berikut seperti contoh dibawah ini

Enter current password for root (enter for none): tekan tombol Enter
Set root password? [Y/n]: Y
New password: Enter password
Re-enter new password: Repeat password
Remove anonymous users? [Y/n]: Y
Disallow root login remotely? [Y/n]: Y
Remove test database and access to it? [Y/n]:  Y
Reload privilege tables now? [Y/n]:  Y

dan jalankan ulang MariaDB service
```bash
sudo systemctl restart mariadb.service
```
## Step 3: Install PHP dan Modul yang Diperlukan

MediaWiki juga membutuhkan fungsi PHP, Untuk menginstall PHP jalankan command berikut
```bash
sudo apt-get install php php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-mcrypt php-ldap php-zip php-curl
```
## Step 4: Membuat MediaWiki Database

Setelah menginstall semua yang dibutuhkan, selanjutnya mulai untuk mengkonfigurasi server. Pertama jalankan perintah berikut untuk masuk ke database server
```bash
sudo mysql -u root -p
```

Selanjutnya buat database dengan nama mediawiki
```bash
CREATE DATABASE mediawiki;
```
Buat user untuk database dengan nama mediawikiuser dengan password baru

```bash
CREATE USER 'mediawikiuser'@'localhost' IDENTIFIED BY 'new_password_here';
```
Dan berikan full akses untuk user 
```bash
GRANT ALL ON mediawiki.* TO 'mediawikiuser'@'localhost' IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;
```
Terakhir simpan dan exit
```bash
FLUSH PRIVILEGES;
EXIT;
```
## Step 5: Download MediaWiki
Berikutnya, jalankan command berikut untuk mendownload dengan command
```bash
cd /tmp && wget https://releases.wikimedia.org/mediawiki/1.29/mediawiki-1.29.0.tar.gz
```
Dan jalankan command untuk mengekstrak ke folder Apache2
```bash2
sudo tar -zxvf mediawiki*.tar.gz
sudo mkdir -p /var/www/html/mediawiki
sudo mv mediawiki-1.29.0/* /var/www/html/mediawiki
```
Mengganti dan memodifikasi *directory permission.*
```bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```
## Step 6: Configure Apache2
Terakhir, konfigurasi Apache2 site configuration file untuk MediaWiki. File ini akan mengontrol user yang mengakses konten di MediaWiki. Jalankan perintah dibawah untuk membuat fail konfigurasi bernama mediawiki.conf
```bash
sudo nano /etc/apache2/sites-available/mediawiki.conf
```
Selanjutnya salin kalimat ini dan simpan ke mediawiki.conf, untuk nama domain dan lokasi dokumen dapat disesuaikan
```bash
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
 
## Step 7: Aktifkan MediaWiki
Setelah selesai mengkonfigurasi VirtualHost, aktifkan dengan menjalankan perintah berikut
```bash
sudo a2ensite mediawiki.conf
```
## Step 8: Restart Apache2
Untuk menjalankan semua fungsi yang sudah dibuat, restart Apache2 dengan menjalankan perintah berikut
```bash
sudo systemctl restart apache2.service
```
Selanjutnya ketikan alamat domain yang sudah dibuat atau IP address dan akan muncul tampilan MediaWiki site setup wizard seperti gambar dibawah. 

# Konfigurasi
[`^ kembali ke atas ^`](#)

Halaman Utama
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%201.PNG">
Klik set up the wiki first untuk melakukan konfigurasi awal
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%202.PNG">
Pilih bahasa yang akan digunakan untuk aplikasi
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%203.PNG">
Klik lanjut untuk menyetujui persyaratan MediaWiki
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%204.PNG">
Pilih jenis database yang digunakan (aplikasi kami menggunakan database mysql). Lalu gunakan localhost untuk inang basis data (karena aplikasi berjalan di server lokal)
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%205.PNG">
Isi identifikasi wiki dan akun pengguna basis data sesuai proses instalasi
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%206.PNG">
Isi nama wiki beserta nama proyek
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%207.PNG">
Isi akun pengurus (akun yang akan digunakan untuk login mediawiki)
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%208.PNG">
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%209.PNG">
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%209.PNG">
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%2010.PNG">
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%2011.PNG">
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%2012.PNG">
<img src="https://github.com/miqbals1649/mediaWiki/blob/master/Komdat/MW%2013.PNG">
Lengkapi sesuai keperluan


# Maintenance
[`^ kembali ke atas ^`](#)

# Otomatisasi
[`^ kembali ke atas ^`](#)

# Cara Pemakaian
**Penggunaan Dasar**
Untuk mengedit halaman, cukup klik tautan edit yang muncul di setiap halaman. Gunakan skin Vector default, ini dalam bentuk tab di bagian atas halaman. Sebuah formulir akan muncul, berisi markup yang ada. Setelah selesai melakukan modifikasi, klik tombol Simpan untuk melakukan perubahan Anda.

**Membuat Laman Baru**
Ada beberapa cara untuk membuat halaman baru:
Buat tautan ke halaman di halaman lain, lalu klik tautan merah yang muncul
Telusuri ke lokasi laman yang dituju, mis. http://www.example.com/index.php?title=New_page dan klik tautan "**Edit**", "**Create**" atau "**Create Source**".
Pada beberapa wiki, pencarian halaman yang gagal akan berisi tautan yang memungkinkan Anda untuk mengedit halaman itu.

**Bagaimana cara menghapus suatu versi lama suatu laman?**
Versi lama data halaman disimpan dalam database dan dapat diakses melalui fitur history. Ini berguna untuk meninjau perubahan dan memperbaiki atau mengembalikan yang tidak diinginkan, tetapi dalam beberapa kasus, administrator mungkin ingin membuat informasi ini tidak tersedia, karena alasan hukum, atau untuk mengurangi ukuran database.
Administrator dapat menghapus revisi halaman yang lama dengan menghapus halaman tersebut, dan kemudian membatalkan penghapusan revisi secara selektif.
Ekstensi Pengawasan (juga dikenal sebagai HideRevision) dapat digunakan untuk memindahkan revisi berbahaya dari riwayat halaman pada versi MediaWiki yang lebih lama (<1,16).
Untuk MediaWikis yang lebih baru (1,14+), Anda dapat mengaktifkan fitur RevisionDelete inti yang memungkinkan pengguna yang memiliki hak istimewa untuk menghapus satu revisi dari histori halaman.
Skrip pemeliharaan maintenance / deleteOldRevisions.php dapat menghapus secara massal semua revisi halaman lama dan catatan teks yang terkait.




[`^ kembali ke atas ^`](#)

# Pembahasan
Sebuah wiki adalah sebuah jenis situs web yang kontennya dapat disunting dari peramban web, dan situs web yang tetap menyimpan versi terdahulu dari setiap halaman yang dapat disunting. Wiki seringkali, tetapi tidak selalu, dapat disunting oleh setiap pengunjung situs.

MediaWiki adalah perangkat lunak berbasis server yang dilisensikan di bawah GNU General Public License (GPL). MediaWiki dirancang untuk berjalan pada server farm yang besar dan mendapat jutaan kunjungan per hari. MediaWiki merupakan perangkat lunak implementasi dari wiki yang sangat andal, terskalakan dan banyak fitur. MediaWiki menggunakan PHP untuk memproses dan menampilkan data yang tersimpan dalam database mysql.

Kelebihan dari MediaWiki adalah:
Pemeliharaan dan dukungan jangka panjang: Pengembangan MediaWiki didukung oleh organisasi dengan anggaran tahunan lebih dari $ 27 juta. Orang tidak perlu khawatir bahwa di masa mendatang basis kode akan sepenuhnya ditinggalkan oleh pengembangnya dan menjadi tidak terawat.
Kesesuaian untuk wiki besar, sangat aktif: MediaWiki digunakan oleh Wikipedia bahasa Inggris, wiki terbesar di dunia, dengan lebih dari 4 juta halaman, 600 juta suntingan sejak awal proyek, [1] dan 470 juta pengunjung unik setiap bulan. [2 ] MediaWiki telah dirancang dengan skalabilitas dalam pikiran untuk penggunaan tinggi, situs profil tinggi yang rentan terhadap vandalisme, spam, dan serangan lainnya.
Banyak konten untuk dipinjam: Apa pun jenis artikel atau templat (mis. Infobox) yang biasa Anda lihat di Wikipedia dan menurut Anda akan berguna di situs Anda, Anda dapat mengimpor. Seringkali tempat awal untuk wiki baru adalah meminjam konten dari Wikipedia (tunduk pada lisensi CC BY-SA). Jika Anda menggunakan perangkat lunak wiki selain MediaWiki, konten Wikipedia mungkin harus diformat ulang atau Anda harus mulai dari awal.

Kelemahan MediaWiki:
Kompleksitas: MediaWiki rumit untuk diatur dan dikelola. Anda harus menginstal dan mengkonfigurasi MediaWiki (termasuk menginstal dan mengkonfigurasi ekstensi apa pun yang Anda inginkan), yang bisa menjadi tugas utama, terutama untuk yang belum berpengalaman. Mungkin ada cukup banyak konten yang perlu Anda impor dari wiki lain atau desain sendiri, seperti konten MediaWiki Anda: namespaces (mis. MediaWiki: Common.css dan MediaWiki: Common.js), dan sebagainya. Setidaknya dua kali setahun, versi MediaWiki baru keluar. Jika Anda ingin terus meminjam konten baru dari Wikipedia, Anda kadang-kadang harus meningkatkan MediaWiki dan ekstensinya dan / atau menginstal ekstensi baru yang telah diperkenalkan Wikimedia. Menginstal, mengonfigurasi, dan memutakhirkan MediaWiki dan ekstensinya tidak selalu mudah dengan satu klik.
Kualitas dokumentasi yang tidak konsisten: Dokumentasi MediaWiki masih dalam proses yang tergantung terutama pada upaya sukarela; itu bisa dibilang belum dalam kondisi sangat baik.


Ada beberapa website serupa seperti Wikipedia, Wikimedia. Berikut adalah beberapa perbedaannya:
Wikimedia adalah nama kolektif untuk sekelompok proyek yang saling terkait, termasuk Wikipedia, Wiktionary, Wikisource, Wikibooks, dan lainnya, yang ditujukan untuk menggunakan daya kolaboratif Internet, dan konsep wiki untuk membuat dan berbagi segala jenis pengetahuan bebas.
Wikipedia adalah sebuah proyek Wikimedia yang mendunia, bebas ensiklopedia daring multibahasa. Wikipedia adalah yang tertua, terbesar, dari proyek Wikimedia, mendahului Yayasan Wikimedia itu sendiri. Wikipedia seringkali dideskripsikan sebagai sebuah wiki, tetapi faktanya Wikipedia adalah kumpulan dari lebih dari 200 wiki, satu untuk setiap bahasa, yang semuanya dijalankan di perangkat lunak MediaWiki.
MediaWiki adalah sebuah mesin wiki, yang dikembangkan untuk dan digunakan oleh Wikipedia dan proyek Wikimedia lainnya. MediaWiki tersedia secara bebas untuk digunakan (dan dikembangkan) oleh orang lain, dan salinannya telah digunakan pada berbagai jenis proyek dan organisasi di seluruh dunia.

[`^ kembali ke atas ^`](#)

# Referensi
[`^ kembali ke atas ^`](#)



