## Dapetin ip ama internet 

Bikin bridge dulu 
Tambahin dari ether 2-5 sama wlan1, klo ga ada ether 5 sampe ether 4 ae


![image](https://github.com/soulahuden/tkj3/assets/106908185/60255125-25db-40c8-9a3b-2ebd3d6baf80)

![image](https://github.com/soulahuden/tkj3/assets/106908185/38f9854d-dfe7-47b4-9b97-c69feb541629)


Trus masukkin ip address networknya sesuai soal 
Interface nya ke ether2 


![image](https://github.com/soulahuden/tkj3/assets/106908185/7badb96f-c21d-455f-859e-acdf23890ccf)



Buat dhcp client ke **ether1** 


![image](https://github.com/soulahuden/tkj3/assets/106908185/e35c3e5c-a358-4af7-ba83-607ef0b78a55)



Buat DHCP Server ke bridge1, trus next 

![image](https://github.com/soulahuden/tkj3/assets/106908185/89e42ac5-c776-4c01-af7d-33e7c8d28d08)

Trus setting IP Rangenya sesuai soal, disini gua ipnya dari 2 – 5

![image](https://github.com/soulahuden/tkj3/assets/106908185/e403fc09-a2d2-4b83-96dc-14c19d7886bb)

Trus tinggal di nex nex ajah 



Trus bikin firewall, Trus ke **NAT**, trus yang **Out. Interface** nya ke ether1

![image](https://github.com/soulahuden/tkj3/assets/106908185/d09f9e1f-3b1f-4a38-9c37-966739bd0082)

Trus ke Action, pilih actionnya ke **masquerade**, trus apply

![image](https://github.com/soulahuden/tkj3/assets/106908185/33136113-da57-4ec0-b38a-77c9d5bd948f)

Dah dapet internet dah 



## BUAT WIFI 

Ke wireless, Enable-in wlan nya dulu 

![image](https://github.com/soulahuden/tkj3/assets/106908185/c1caf618-d57f-4456-8832-fac2eaf0039a)


Trus ke **Security Profiles**, 
Tambahin security profilenya, Ganti mode nya jadi **dynamic keys**, trus masukin passwordnya sesuai soal
Disini gua passwordnya ueska2023, trus Apply

![image](https://github.com/soulahuden/tkj3/assets/106908185/8318e5f1-a407-49c3-ae3a-6a1ac2df0713)


Trus balik lagi ke **interfaces**, klik **wlan1** nya, Trus ke **wireless**
Ganti **Mode** nya jadi ap bridge, **SSID** nya sesuai soal, trus masukin **Security Profilenya** yg tadi lu buat


![image](https://github.com/soulahuden/tkj3/assets/106908185/b7cded44-c5da-4901-8075-aee509664996)



## BLOCKING SITE


Ke Firewall, trus **Layer7 Protocols**, isi nama sterah, trus website yang mau di block,  trus Apply
Trus ke Filter Rules

![image](https://github.com/soulahuden/tkj3/assets/106908185/6ed2b9ec-2d67-49d7-8100-d9b10281c41e)


Trus ke **Filter Rules, tambahin rulenya, Trus ke **Advanced**
Pilih layer7 protocolnya sama yg tadi udh di buat.
Trus ke action, pilih **drop** trus Apply


![image](https://github.com/soulahuden/tkj3/assets/106908185/4d5fd3ec-9683-4f8f-8bc8-088c08be9340)

![image](https://github.com/soulahuden/tkj3/assets/106908185/72c7df7c-8414-4819-ac04-bd64df5de991)


## LIMIT BANDWITH 


Ke Queues, Tambahin queueueuesnya.
Targetnya ke ip lu, Max limit Uploadnya jadi **512k**(itu Digambar salah) sama Downloadnya jadi **1M**, trus apply 
Bisa juga queuesnya masing-masing ip , jadi buat atu atu, ga kayak Digambar bawah langsung satu network


![image](https://github.com/soulahuden/tkj3/assets/106908185/91ba04ba-2f20-49b4-8c87-003dc98f4eda)



## SETTING JAM AKTIF INTERNET 


Ke **Firewall** trus ke **NAT** yang tadi udh lu buat, trus ke **Extra**, tambahin waktunya kek gambar dibawah

![image](https://github.com/soulahuden/tkj3/assets/106908185/9e597596-c49a-4ed1-a2da-c001c99aedf4)


Cara ngetest waktu aktifnya, Ke System > Clock, trus ganti jamnya jadi jam diluar jam aktif
Tadi kan aktifnya dari jam 07:00 sampe 19:00
Ganti dah, misalnya jadi jam 05:00


![image](https://github.com/soulahuden/tkj3/assets/106908185/c141af59-d212-4d01-bb4e-8190ec44c21a)


