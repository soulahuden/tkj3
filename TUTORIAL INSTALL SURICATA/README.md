Install Suricata :

```
sudo apt update
sudo apt-get install software-properties-common
sudo apt install suricata jq
```

Untuk ngecek versi suricata :
```
sudo suricata --build-info
sudo systemctl status suricata
```

Konfigurasi suricata di /etc/suricata/suricata.yaml, ubah menjadi seperti dibawah : 
```
nano /etc/suricata/suricata.yaml
lalu CTRL + W dan ketik "af-packet" supaya lebih cepat mencarinya
```

dan ubah konfigurasinya seperti dibawah :

```
af-packet:
    - interface: (ubah jadi interface anda, misal : enp0s3)
      cluster-id: 99
      cluster-type: cluster_flow
      defrag: yes
      use-mmap: yes
      tpacket-v3: yes
```

Buat rule DDOS : 

```
nano /etc/suricata/rules/(nama file bebas)
misal 
nano /etc/suricata/rules/ddos.rules

lalu isi rule dibawah : 





alert tcp any any -> $HOME_NET 80 (msg: "Possible DDoS attack"; flags: S; flow: stateless; threshold: type both, track by_dst, count 200, seconds 1; sid:1000001; rev:1;)
```

lalu masukkan file **ddos.rules** nya kedalam konfigurasi suricata.yaml : 
```
rule-files: 
- suricata.rules   
- ddos.rules
```

restart suricata 
```
systemctl restart suricata 
suricata-update
```


Install aplikasi **ethtool** dan matikan limit interface 
```
sudo apt install ethtool

/usr/sbin/ethtool -K enp0s3 gro off lro off
```

Matikan semua process ID (PID) suricata dan jalankan kembali 
```
rm /var/run/suricata.pid 
suricata -D -c /etc/suricata/suricata.yaml -i enp0s3
```

Monitor dengan 
```
suricata --list-runmodes
```

# Testing DDOS 

Install hping3 untuk melakukan ddos 
```
sudo apt install hping3
hping3 -S --flood -V -p [port] [ip]
misal : 
hping3 -S --flood -V -p 80 192.168.10.249
```

Liat log serangan : 
```
sudo tail -f /var/log/suricata/fast.log
```


