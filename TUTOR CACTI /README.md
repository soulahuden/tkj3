Langkah awal (tidak usah jika sudah)
```
#apt update
#apt upgrade
#apt install mariadb-server
#mysql_secure_installation
```

Install cacti : 
```
#apt install cacti
```

Pilih "apache2" dan pilih "Yes" : 
![image](https://github.com/soulahuden/tkj3/assets/106908185/6376ed54-dd43-41a9-a631-763765d78a82)
![image](https://github.com/soulahuden/tkj3/assets/106908185/7a36fcf9-ea4e-4e77-97a4-f6a19dee981a)

Lalu masukkan password untuk cacti, disini saya isi "123" 
![image](https://github.com/soulahuden/tkj3/assets/106908185/aab3d566-88ac-4d46-ad72-ba36ccecfb05)


Install SNMP Daemon, SNMP Client, dan SNMP Dev : 
```
#apt install snmpd snmp libsnmp-dev
```

Buat file backup dengan command dibawah : 
```
#cd /etc/snmp
#cp snmpd.conf snmpd.conf-original
```

Edit file **snmpd.conf** seperti dibawah : 
```
#nano /etc/snmp/snmpd.conf
```
<img width="298" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/0ae7d48c-5408-424c-9da4-6e07038f358f">

Lalu ke paling bawah lagi dan ubah seperti dibawah sesuai nama dan ip elu : 
![image](https://github.com/soulahuden/tkj3/assets/106908185/717cc751-f972-49c2-a9bd-4aa1223574cf)


Install SNMP rrdtool nya dengan command : 
```
#apt install snmp-mibs-downloader rrdtool

NOTE : JIKA COMMAND DIATAS TIDAK BISA, PASTIKAN MENGGUNAKAN / UBAH REPO MENJADI SEPERTI DIBAWAH :

deb http://deb.debian.org/debian bullseye main contrib non-free
deb http://deb.debian.org/debian bullseye-updates main contrib non-free
deb http://deb.debian.org/debian bullseye-backports main contrib non-free
deb http://security.debian.org/debian-security/ bullseye-security main contrib non-free

SETELAH SUDAH DI UBAH JALANKAN COMMAND apt update .
```

Lalu restart snmp : 
```
#systemctl restart snmpd
```

## Membuat DNS untuk Cacti 

```
#cd /etc/bind
#nano db.usk

LALU TAMBAHKAN INI DIPALING BAWAH FILE : 

cacti   IN      A       192.168.1.34
```

Buat juga di db.192 : 

```
#nano db.192

TAMBAHKAN INI DIPALING BAWAH FILE :

34      IN      PTR     cacti.usk13662.net.
^                               ^ 
(ip terakhir anda)          (subdomain)

```

Ke folder /etc/apache2/sites-available lalu buat file configurasi untuk cacti : 
```
#cd /etc/apache2/sites-available
#cp 000-default.conf cacti.conf
```

Ubah konfigurasi **cacti.conf** :
```
#nano cacti.conf
```

Ubah konfigurasi seperti dibawah : 
![image](https://github.com/soulahuden/tkj3/assets/106908185/abff601b-d937-4a9f-bf86-c10ed956c60e)


Lalu edit file /usr/share/cacti/site/include/config.php :
```
#nano /usr/share/cacti/site/include/config.php
```

Lalu ubah konfigurasi seperti dibawah : 


![image](https://github.com/soulahuden/tkj3/assets/106908185/4e7a967a-fb9d-4498-a0fc-e5beb9d504a3)


Setelah itu enable **cacti.conf** nya dan service lainya :
```
#a2ensite cacti.conf
#systemctl restart apache2
#systemctl restart bind9
```

## Monitoring Server 

Buka domain cacti yang telah klean buat, disini domain saya adalah cacti.usk13662.net

Lalu kalian Create "New Device".\
Ubah "Description"nya terserah klean. \
Ubah "hostname" nya menjadi ip linux kalian. \ 

Ubah "SNMP Community String" menjadi nama kalian yang kalian isi di **/etc/snmp/snmpd.conf**. \ 

Lalu Create! 
![image](https://github.com/soulahuden/tkj3/assets/106908185/b5f50187-2e9a-4e95-afac-3b1c9839e8cc)


Jika seperti ini berarti sudah berhasil terbuat 
![image](https://github.com/soulahuden/tkj3/assets/106908185/cdca04a2-3f52-459b-9b24-036720aa48a8)


Setelah itu kalian ke paling bawah, tambahkan "SNMP - Interface Statistics" seperti dibawah, lalu Add : 
![image](https://github.com/soulahuden/tkj3/assets/106908185/4c45d51a-e3e9-4fec-b184-533725c548e1)



Lalu verify all di keduanya : 
![image](https://github.com/soulahuden/tkj3/assets/106908185/133a1064-3da9-474f-9dc1-63ca6d625e8e)

Setelah itu kalian ke Device lagi, trus centang device yang tdi kalian sudah buat, trus pilih "Place on a Tree (Default Tree)" trus Go 
![image](https://github.com/soulahuden/tkj3/assets/106908185/1779b738-112d-4728-bd7f-09ef575bb3fd)


Kalau sudah "Up", kalian ke "New Graphs" trus pilih device kalian, kek dibawah 
<img width="634" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/5b53a869-eaea-417b-b974-ac6bf6307754">


trus centang, trus pilih "In/Out Bytes" trus create 
![image](https://github.com/soulahuden/tkj3/assets/106908185/e7ede6ad-9df0-4ead-b406-f6f1e2ac5f8f)



Setelah itu kalian ke **Graphs** 

<img width="919" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/82412acc-c0a9-4ab8-8b90-5f84bb0fe003">

dah kelar.


