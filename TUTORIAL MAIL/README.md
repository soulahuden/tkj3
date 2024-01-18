
kek biasa langkah awal : 

```
#apt update 
#apt install postfix dovecot-imapd
```

Pilih Internet Site
![image](https://github.com/soulahuden/tkj3/assets/106908185/8c5fbba9-ac2b-4440-a30e-82e1b20265d2)

Tulis domain absen kalian, tidak usah pakai subdomain.
![image](https://github.com/soulahuden/tkj3/assets/106908185/15397862-3b7f-4e74-b3af-0ec37f94a437)




Edit file **main.cf** : 
```
#cd /etc/postfix
#nano main.cf
```

trus tambahin "home_mailbox = Maildir/" kek dibawah : 

![image](https://github.com/soulahuden/tkj3/assets/106908185/cc48932b-09e3-4d63-8ccd-1a97c1d0e7bd)


buat mail directory di directory /etc/skel
```
#maildirmake.dovecot /etc/skel/Maildir
```

abis itu konfigurasi ulang :
```
#dpkg-reconfigure postfix
```

yang ini kosongin 
<img width="816" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/cab4a495-62b3-4734-aa51-2d45fb1c5ff4">



Masukkan localhost dan domain kalian
<img width="758" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/848fd5de-119c-4bcf-98d6-674d089402cb">


Pilih "Yes"
<img width="806" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/208170da-84ad-4d78-b0b0-b5f77eb9e77a">


Masukkan ip linux kalian dan 0.0.0.0/0
<img width="812" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/d353d1a0-37fd-4783-b675-c9f203184fa8">


Ini OK aja 
<img width="813" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/e6e4dd2a-d942-4605-b7dc-aed8f24c6545">


Ini OK aja 


<img width="623" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/c3d1232f-87ca-45f8-8d93-23edabbc02c7">


Pilih ipv4 
<img width="809" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/b916de19-d056-4e36-8286-441413d1703b">


Restart postfix 
```
#systemctl restart postfix
```

## Konfigurasi Dovecot 

konfigurasi 
```
#nano /etc/dovecot/dovecot.conf
```

trus ubah kek dibawah,pagernya apus, sisain bintang aje : 

<img width="202" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/fb6e2af8-bddc-4aec-8819-5e2e42727f51">



Edit file konfigurasi /etc/dovecot/conf.d/10-auth.conf : 
```
#nano /etc/dovecot/conf.d/10-auth.conf
```

trus cari "disable_plaintext_auth" trus ganti dah kek di bawah 

apus pagernye trus ganti yes jadi no


<img width="298" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/479feb3c-bcf7-4586-b9ed-93f43e8cc23d">




Edit file konfigurasi /etc/dovecot.conf.d/10-mail.conf :
```
#nano /etc/dovecot/conf.d/10-mail.conf
```



trus cari mail_location trus pagernya apuss kek dibawah 

<img width="551" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/05ea834a-053f-4f0f-8f23-7c092bf35768">

trus bawahan dikit ada "mail_location = mbox:~/mail:INBOX=/var/mail/%u"  nah itu lu pagerin ae, kek gambar dibawah :


<img width="382" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/2eca2ff2-68da-4f2f-8eed-3169b9ab40ef">

Restart dovecot :
```
#systemctl restart dovecot
```

## Menambahkan User Mail

Tambahin user, passwodnya sesuai soal
```
#adduser billing
#adduser ceo
```

Restart postfix dan dovecot :
```
#systemctl restart postfix dovecot
```

## Konfigurasi Roundcube

Install roundcube dan mariadb-server (kalo bloman) : 
```
#apt install roundcube mariadb-server
```

Pilih "Yes" Biar kebuat databasenya otomatis
<img width="825" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/606b8871-ab89-4aa2-9e5c-9e82a053f99e">


Trus masukin passwordnya, biar gampang gua "123" ae 

<img width="818" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/eb0b89b4-0d1e-40ae-bf57-a60b5fd2a4a3">

edit file config.inc.php 
```
#nano /etc/roundcube/config.inc.php
```

ubah default host dengan domain kalian 

<img width="366" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/fe3fbdd0-59a8-4c6f-ba78-f335720b4f57">

ganti smpt server dengan domain kalian 


<img width="303" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/cab7af9d-67aa-4001-8575-c129847f2ec5">


Ubah portnya menjadi 25 


<img width="211" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/0504334f-dd78-403a-bade-c6b85c023884">


trus kosongkan user dan passnya 

<img width="259" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/5af5cb26-fdb2-4087-aef0-57580ba71434">

Konfigurasi ulang roundcube :
```
#dpkg-reconfigure roundcube-core
```

ini kosongin ae
![image](https://github.com/soulahuden/tkj3/assets/106908185/b458e171-7b2e-4c8c-a5ec-b828c9c85dc3)

trus pilih yang "en_US" (males ss)


trus pilih no, gausah install database ulang : 

<img width="817" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/755e3e72-9e1b-4f14-bc00-73f8b5640dd7">

Pilih apache2 aja 


<img width="406" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/77ca2b0b-63d9-48f6-be27-d5210b9dd83e">

Restart.


<img width="739" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/faa9adad-0de2-40f8-ab5e-938ee7d38dad">


trus pilih "keep the local blalblaasdfjae" 

<img width="708" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/908a5868-765c-4033-99d9-568296d7ee76">


## Setting Apache2

Edit file apache2.conf
```
#nano /etc/apache2/apache2.conf
```

Trus tambahin ini dipaling bawah 

<img width="376" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/78ff2955-d30b-4086-8c0b-941cab122a7b">

Trus kita buat konfigurasi buat mail di sites-available 
```
#cd /etc/apache/sites-available
#cp 000-default.conf mail.conf
#nano mail.conf
```


<img width="368" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/cf990236-f456-413b-a178-30ce2525e6f6">



aktifkan file mail.conf dan restart apache2 : 
```
#a2ensite mail.conf
#systemctl restart apache2
```

Oh iya jan lupa buat DNS buat mail nya :

```
#cd /etc/bind
#nano db.absen
#nano db.192
```

db.absen

<img width="452" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/5bba5b24-5370-4df4-bc4e-f936124c3785">


db.192

<img width="356" alt="image" src="https://github.com/soulahuden/tkj3/assets/106908185/f33e2f7f-687f-422e-9944-45e05574f7ef">



