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

a. Konfigurasi ```/etc/bind/wise/wise.B07.com``` di Wise
```
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.b07.com. root.wise.b07.com. (
                        2022102401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      wise.b07.com.
@       IN      A       192.176.2.2
www     IN      CNAME   wise.b07.com.
eden    IN      A       192.176.3.3
www.eden     IN      CNAME   eden.wise.b07.com.
' > /etc/bind/wise/wise.b07.com
```
b. Kemudian restart ```service bind9 restart``` di Wise dan lakukan ```ping www.eden.wise.B07.com``` di client
## Nomer 4 ##
Buat juga reverse domain untuk domain utama

a. Konfigurasi ```/etc/bind/wise/2.176.192.in-addr.arpa``` di Wise
```
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.b07.com. root.wise.b07.com. (
                        2022102401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
2.176.192.in-addr.arpa. IN      NS      wise.b07.com.
2                       IN      PTR     wise.b07.com.
' > /etc/bind/wise/2.176.192.in-addr.arpa
```
b. Konfigurasi ```/etc/bind/named.conf.local``` di Wise
```
zone "2.176.192.in-addr.arpa" {
    type master;
    file "/etc/bind/wise/2.176.192.in-addr.arpa";
};
' > /etc/bind/named.conf.local
```
c. Kemudian restart ```service bind9 restart``` di Wise dan lakukan ```host -t PTR 192.176.3.2``` di client
## Nomer 5 ##
Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama

a. Konfigurasi ```/etc/bind/named.conf.local``` di Berlint
```
echo '
zone "wise.b07.com" {
    type slave;
    masters { 192.176.2.2; }; // Masukan IP Wise tanpa tanda petik
    file "/var/lib/bind/wise.b07.com";
}; ' > /etc/bind/named.conf.local
```
b. Kemudian restart ```service bind9 restart``` di Berlint dan stop ```service bind9 stop``` di Wise. Lakukan ```ping www.wise.B07.com``` di client
## Nomer 6 ##
Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation 

a. Konfigurasi ```/etc/bind/wise/wise.B07.com``` di Wise
```
echo '
ns1       IN      A       192.176.3.2
operation       IN      NS       ns1
' >> /etc/bind/wise/wise.b07.com
```
b. Konfigurasi ```/etc/bind/named.conf.options``` di Wise
```
echo '
options {
        directory "/var/cache/bind";
        // forwarders {
        //      0.0.0.0;
        // };
        //dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options
```
c. Konfigurasi ```/etc/bind/operation/operation.wise.B07.com``` di Berlint
```
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     operation.wise.b07.com. root.operation.wise.b07.com. (
                        2022102401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      operation.wise.b07.com.
@       IN      A       192.176.3.3
www     IN      CNAME   operation.wise.b07.com.
strix    IN      A       192.176.3.3
www.strix     IN      CNAME   strix.operation.wise.b07.com.
' > /etc/bind/operation/operation.wise.b07.com
```
d. Konfigurasi ```/etc/bind/named.conf.options``` di Berlint
```
echo '
options {
        directory "/var/cache/bind";
        // forwarders {
        //      0.0.0.0;
        // };
        //dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options
```
e. Kemudian restart ```service bind9 restart``` di Wise dan Berlint. Lakukan ```ping www.wise.B07.com``` di client
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
![Logo Nomer 14.2](/image/14.2.png)

dan jangan lupa untuk copy resourcenya ``` cp -r /Resourcegan/strix.operation.wise/. /var/www/strix.operation.wise.b07.com/ ```
Kemudian tes dengan ``` lynx strix.operation.wise.b07.com:15000``` atau 15500

![Logo Nomer 14.3](/image/14.3.png)

## Nomer 15 ##
dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy

**Jawab :**

bikin password di Eden buat apache2 ```htpasswd -c -b /etc/apache2/.htpasswd Twilight opStrix```
dan tambahkan 
```
<Directory \"/var/www/strix.operation.wise.b07.com\">
                AuthType Basic
                AuthName \"Restricted Content\"
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
        </Directory>
```
pada setiap host
lalu test dengan ```lynx strix.operation.wise.b07.com:15000```
maka akan disuruh memasukan username dan password
![Logo Nomer 15.1](/image/15.1.png)
dan hasilnya seperti ini
![Logo Nomer 15.2](/image/15.2.png)
## Nomer 16 ##
dan setiap kali mengakses IP Eden akan dialihkan secara otomatis ke www.wise.yyy.com

**Jawab :**
```
        RewriteEngine On
        RewriteCond %{HTTP_HOST} ^192\.176\.3\.3$
        RewriteRule /.* http://wise.b07.com/ [R]
```
pada  /etc/apache2/sites-available/000-default.conf

lalu tes menggunakan Ip Eden lynx 192.176.3.3
Maka hasilnya 
![Logo Nomer 16](/image/16.png)

## Nomer 17 ##
Karena website www.eden.wise.yyy.com semakin banyak pengunjung dan banyak modifikasi sehingga banyak gambar-gambar yang random, maka Loid ingin mengubah request gambar yang memiliki substring “eden” akan diarahkan menuju eden.png. Bantulah Agent Twilight dan Organisasi WISE menjaga perdamaian!

**Jawab :**

pada eden nano /var/www/eden.wise.b07.com/.htaccess dan tambahkan
```
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_URI} !/public/images/eden.png
RewriteRule /.* http://eden.wise.b07.com/public/images/eden.png [L]
```
lalu ganti config pada /etc/apache2/sites-available/eden.wise.b07.com.conf dengan menambah AllowOverride All
lalu jangan lupa a2enmod rewrite dan juga jangan lupa restart apache
![Logo Nomer 17.1](/image/17.1.png)
lalu cek dengan ```lynx www.eden.wise.b07.com/asedenash``` maka hasilnya
![Logo Nomer 17.2](/image/17.2.png)

