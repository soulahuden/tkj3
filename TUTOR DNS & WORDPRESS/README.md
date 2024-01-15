Repo debian 11 
```
deb http://deb.debian.org/debian bullseye main contrib non-free
deb http://deb.debian.org/debian bullseye-updates main contrib non-free
deb http://deb.debian.org/debian bullseye-backports main contrib non-free
deb http://security.debian.org/debian-security/ bullseye-security main contrib non-free
```

Install dns dan apache2 :
```
apt update
apt install bind9 apache2
```

Buat 2 konfigurasi file, misal db.usk dan db.absen :
```
cd /etc/bind
cp db.local db.usk
cp db.local db.absen
```

Buat file reverse :
```
cp db.127 db.192

dan copy file named: 

cp named.conf.defaul-zones named.conf.local
```

Ubah konfigurasi db.usk seperti dibawah :

```
nano db.usk

# UBAH localhost MENJADI DOMAIN ANDA 
# UBAH IP MENJADI IP DEBIAN ANDA 
# DAN DI PALING BAWAH TAMBAHKAN SUBDOMAIN WWW DAN IP DEBIAN SEPERTI DIBAWAH

;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     usk13662.net. root.usk13662.net. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      usk13662.net.
@       IN      A       192.168.0.194
www     IN      A       192.168.0.194

```

Ubah konfigurasi db.absen seperti dibawah : 
```
nano db.absen 

# UBAH localhost MENJADI DOMAIN ABSEN ANDA
# UBAH IP MENJADI IP DEBIAN 
# DAN DI PALING BAWAH TAMBAHKAN SUBDOMAIN WWW DAN IP DEBIAN SEPERTI DIBAWAH

;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     absen35.my.id. root.absen35.my.id. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      absen35.my.id.
@       IN      A       192.168.0.194
www     IN      A       192.168.0.194
```

Ubah konfigurasi db.192 seperti dibawah :
```
nano db.192

# UBAH localhost MENJADI DOMAIN USK ANDA 
# TAMBAHKAN IP REVERSE DIPALING BAWAH

;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     usk13662.net. root.usk13662.net. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      usk13662.net.
194     IN      PTR     usk13662.net.
```

Ubah konfigurasi **named.conf.local** seperti dibawah :
```
nano named.conf.local

# HAPUS SEMUA ZONE DAN SISAKAN SEPERTI DIBAWAH 
# DAN TAMBAHKAN ZONE absen

zone "usk13662.net" {
        type master;
        file "/etc/bind/db.usk";
};

zone "absen35.my.id" {
        type master;
        file "/etc/bind/db.absen";
};

zone "192.in-addr.arpa" {
        type master;
        file "/etc/bind/db.192";
};

```

Ubah konfigruasi **named.conf.options** seperti dibawah : 
```
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

        forwarders {
                192.168.0.194;
                8.8.8.8;
        };

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        dnssec-validation auto;

        listen-on-v6 { any; };
};
```

Restart bind9 dan ubah resolve : 
```
systemctl restart bind9
nano /etc/resolv.conf

ubah resolve menjadi ip debian anda 

nameserver 192.168.0.194
```

Test ping ke domain *www.absen35.my.id* dan *www.usk13662.net*
```
ping www.absen35.my.id

PING www.absen35.my.id (192.168.0.194) 56(84) bytes of data.
64 bytes from 192.168.0.194 (192.168.0.194): icmp_seq=1 ttl=64 time=0.022 ms
64 bytes from 192.168.0.194 (192.168.0.194): icmp_seq=2 ttl=64 time=0.101 ms
64 bytes from 192.168.0.194 (192.168.0.194): icmp_seq=3 ttl=64 time=0.049 ms

ping www.usk13662.net

PING www.usk13662.net (192.168.0.194) 56(84) bytes of data.
64 bytes from 192.168.0.194 (192.168.0.194): icmp_seq=1 ttl=64 time=0.034 ms
64 bytes from 192.168.0.194 (192.168.0.194): icmp_seq=2 ttl=64 time=0.047 ms
64 bytes from 192.168.0.194 (192.168.0.194): icmp_seq=3 ttl=64 time=0.100 ms
```

# Install Wordpress

Install database untuk wordpress pelengkap lainnya : 
```
apt install mariadb-server php php-mysql wget 
```

Pindah ke direktori **/var/www/html** dan download wordpress :
```
cd /var/www/html
wget wordpress.org/latest.tar.gz
tar xzfv latest.tar.gz
```

Buat direktori baru dan copy **latest.tar.gz** kedalam direktori baru:
```
mkdir wordpress2
cp latest.tar.gz wordpress2/
```

extract wordpress : 
```
tar xzfv latest.tar.gz
cd wordpress2 
tar xzfv latest.tar.gz
```

Lalu ganti akses di kedua lokasi wordpress : 
```
chmod 777 /var/www/html/wordpress
chmod 777 /var/www/html/wordpress2/wordpress/
```

## Setting Apache2

Setting apache2 dan buat 2 konfigurasi untuk masing-masing dns :
```
cd /etc/apache2/sites-available
cp 000-default.conf usk.conf
cp 000-default.conf absen.conf
```

Ubah konfigurasi di kedua file seperti dibawah ini : 
```
nano usk.conf 

<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        ServerName www.usk13662.net

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html/wordpress
```

```
nano absen.conf

<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        ServerName www.absen35.my.id

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html/wordpress2/wordpress
```

Enable kedua file dengan command : 
```
a2ensite usk.conf
a2ensite absen35.conf
systemctl restart apache2
```

## Setting Database MySQL

Secure install mariadb dan ubah password terserah klean : 
```
mysql_secure_installation
```

Buat database untuk domain usk dan absen :
```
mysql -u root 

> create database usk;
> create database absen;
> show databases;
```

Lalu ke Pc Host/Windows kalian dan ubah DNS Server nya menjadi DNS Server IP Linux kalian

Lalu buka domain kalian di web browser 

<img width="678" alt="image" src="https://github.com/soulahuden/xtkj3/assets/106908185/ff3521fd-c905-42f8-95ac-0f43f601b8b8">

Lalu isi database name dengan database yang kalian buat sesuai namanya. Masukkan username root dan password yang sudah kalian buat, lalu Submit. 
<img width="669" alt="image" src="https://github.com/soulahuden/xtkj3/assets/106908185/f61a6b9f-ca67-434c-8f0a-c680726d7357">


Setelah itu isi dibawah ini semua seterah kalian .
<img width="598" alt="image" src="https://github.com/soulahuden/xtkj3/assets/106908185/853322aa-afe3-4b28-a486-c823c9be0248">

Wordpress Berhasil terinstall dengan DNS
<img width="855" alt="image" src="https://github.com/soulahuden/xtkj3/assets/106908185/2ec1e363-e50e-4f39-b661-49063af407ac">

Lakukan hal yang sama di domain kalian yang satu lagi.
