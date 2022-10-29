# Laporan Praktikum JARKOM 2 Kelompok B07 #

| Nama                      | NRP           |
| ------------------------- | ------------- |
| Danial Farros Maulana     | 5025201004    |
| Rendi Dwi Francisko       | 5025201056    |
| Ahmad Ibnu Malik Rahman   | 5025201232    |



## Script .bashrc
### Wise ###
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
mkdir /etc/bind/wise
```

### Berlint ###
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
mkdir /etc/bind/operation
```
### Eden ###
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get install nano -y
apt-get install bind9 -y
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0
service apache2 start
apt-get install ca-certificates openssl -y
apt-get install unzip -y
apt-get install git -y
git clone https://github.com/Rendyfranzz/resourcejarkom2.git
unzip -o /resourcejarkom2/\*.zip -d /Resourcegan
```

### SSS ###
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils -y
apt-get install lynx -y
echo nameserver 192.176.2.2 > /etc/resolv.conf
echo nameserver 192.176.3.2 >> /etc/resolv.conf
```
### Garden ###
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils -y
apt-get install lynx -y
echo nameserver 192.176.2.2 > /etc/resolv.conf
echo nameserver 192.176.3.2 >> /etc/resolv.conf
```
## Nomer 1 ##
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet.
**Jawab :**
![Logo Nomer 1](/image/nomer1.png)
## Nomer 2 ##
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise 
**Jawab :**
Wise
lakukan ```nano /etc/bind/named.conf.local``` untuk mengedit
![Logo Nomer 1](/image/nomer2.1.png)
## Nomer 3 ##
Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden
## Nomer 4 ##
Buat juga reverse domain untuk domain utama
## Nomer 5 ##
Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama
## Nomer 6 ##
Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation 
## Nomer 7 ##
Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden
## Nomer 8 ##
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.wise.yyy.com. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com
## Nomer 9 ##
Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home
## Nomer 10 ##
Setelah itu, pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com
## Nomer 11 ##
Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja
## Nomer 12 ##
Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache
## Nomer 13 ##
Loid juga meminta Franky untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.eden.wise.yyy.com/public/js menjadi www.eden.wise.yyy.com/js
## Nomer 14 ##
Loid meminta agar www.strix.operation.wise.yyy.com hanya bisa diakses dengan port 15000 dan port 15500

**Jawab :**

pada eden buat config baru dengan ```nano /etc/apache2/sites-available/strix.operation.wise.b07.com.conf ```
![Logo Nomer 14.1](/image/nomer14.1.png)
kemudian aktifkat dengan ```a2ensite strix.operation.wise.b07.com```
kemudian edit juga pada ```/etc/apache2/ports.conf ``` seperti berikut
![Logo Nomer 14.1](/image/nomer14.2.png)

## Nomer 15 ##
dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy
## Nomer 16 ##
dan setiap kali mengakses IP Eden akan dialihkan secara otomatis ke www.wise.yyy.com
## Nomer 17 ##
Karena website www.eden.wise.yyy.com semakin banyak pengunjung dan banyak modifikasi sehingga banyak gambar-gambar yang random, maka Loid ingin mengubah request gambar yang memiliki substring “eden” akan diarahkan menuju eden.png. Bantulah Agent Twilight dan Organisasi WISE menjaga perdamaian!
