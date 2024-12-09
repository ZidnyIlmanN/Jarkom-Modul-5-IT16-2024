# Jarkom-Modul-5-IT16-2024

| Nama          | NRP          |
| ------------- | ------------ |
| Zidny Ilman Nafi'an | 5027221072 |
| MUHAMMAD DZAKWAN | 5027201065 |

## Topologi
![A1](https://github.com/user-attachments/assets/4cdaed88-2e8f-44cc-a042-5356c9e0908b)

## Pembagian Rute Subnet
| Nama Subnet | Rute | Jumlah IP | Netmask |
|-------------|------|------------|---------|
| A1          | NewEridu > LuminaSquare | 2   | /30     |
| A2          | LuminaSquare > Ofc.Mewmew > HIA > Ofc.Mewmew > BalletTwins | 3   | /29     |
| A3          | LuminaSquare > PubSec > Jane(200-Host) > PubSec > Policeboo(30-Host) | 231 | /24     |
| A4          | BalletTwins > Victoria > Lycaon(20-Host) > Victoria > Ellen(100-Host) | 121 | /25     |
| A5          | NewEridu > SixStreet | 2   | /30     |
| A6          | SixStreet > RandomPlay > Fairy > HDD | 3   | /29     |
| A7          | SixStreet > Metro > OuterRing > Metro > ScootOutpost | 3   | /29     |
| A8          | ScootOutpost > HollowZero | 2   | /30     |
| A9          | OuterRing > SoC > Burnice(5-Host) > SoC > Caesar(50-Host) | 56  | /26     |
| **Total**   |                          | **423** | **/23** |

## Pembagian IP VLSM
| Subnet | Network ID        | Netmask          | Broadcast       | Range IP                     |
|--------|-------------------|------------------|-----------------|------------------------------|
| A3     | 192.237.0.0       | 255.255.255.0    | 192.237.0.255   | 192.237.0.1 - 192.237.0.254 |
| A4     | 192.237.1.0       | 255.255.255.128  | 192.237.1.127   | 192.237.1.1 - 192.237.1.126 |
| A9     | 192.237.1.128     | 255.255.255.192  | 192.237.1.191   | 192.237.1.129 - 192.237.1.190 |
| A2     | 192.237.1.192     | 255.255.255.248  | 192.237.1.199   | 192.237.1.193 - 192.237.1.198 |
| A6     | 192.237.1.200     | 255.255.255.248  | 192.237.1.207   | 192.237.1.201 - 192.237.1.206 |
| A7     | 192.237.1.208     | 255.255.255.248  | 192.237.1.215   | 192.237.1.209 - 192.237.1.214 |
| A1     | 192.237.1.216     | 255.255.255.252  | 192.237.1.219   | 192.237.1.217 - 192.237.1.218 |
| A5     | 192.237.1.220     | 255.255.255.252  | 192.237.1.223   | 192.237.1.221 - 192.237.1.222 |
| A8     | 192.237.1.224     | 255.255.255.252  | 192.237.1.227   | 192.237.1.225 - 192.237.1.226 |

## Konfigurasi
- NewEridu
```
auth eth0
iface eth0 inet dhcp

#A5
auto eth1
iface eth1 inet static
	address 192.237.1.221
	netmask 255.255.255.252

#A1
auto eth2
iface eth2 inet static
	address 192.237.1.217
	netmask 255.255.255.252

#Routing
#A6
post-up route add -net 192.237.1.200 netmask 255.255.255.248 gw 192.237.1.222

#A7
post-up route add -net 192.237.1.208 netmask 255.255.255.248 gw 192.237.1.222

#A8
post-up route add -net 192.237.1.224 netmask 255.255.255.252 gw 192.237.1.222

#A9
post-up route add -net 192.237.1.128 netmask 255.255.255.192 gw 192.237.1.222

#A2
post-up route add -net 192.237.1.192 netmask 255.255.255.248 gw 192.237.1.218

#A3
post-up route add -net 192.237.0.0 netmask 255.255.255.0 gw 192.237.1.218

#A4
post-up route add -net 192.237.1.0 netmask 255.255.255.128 gw 192.237.1.218
```

- SixStreet
```
auto lo
iface lo inet loopback

#A5
auto eth0
iface eth0 inet static
    address 192.237.1.222
    netmask 255.255.255.252
    gateway 192.237.1.221

#A6
auto eth2
iface eth2 inet static
    address 192.237.1.201
    netmask 255.255.255.248

#A7
auto eth1
iface eth1 inet static
    address 192.237.1.209
    netmask 255.255.255.252

#Routing
#A8
post-up route add -net 192.237.1.224 netmask 255.255.255.252 gw 192.237.1.210

#A9
post-up route add -net 192.237.1.128 netmask 255.255.255.192 gw 192.237.1.211

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- HDD
```
#A6
auto eth0
iface eth0 inet static
    address 192.237.1.202
    netmask 255.255.255.248
    gateway 192.237.1.201

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Fairy
```
#A6
auto eth0
iface eth0 inet static
    address 192.237.1.203
    netmask 255.255.255.248
    gateway 192.237.1.201

#Routing
#A9
post-up route add -net 192.237.1.128 netmask 255.255.255.192 gw 192.237.1.201

#A3
post-up route add -net 192.237.0.0 netmask 255.255.255.0 gw 192.237.1.201

#A4
post-up route add -net 192.237.1.0 netmask 255.255.255.128 gw 192.237.1.201

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- ScootOutpost
```
auto lo
iface lo inet loopback

#A7
auto eth0
iface eth0 inet static
    address 192.237.1.210
    netmask 255.255.255.248
    gateway 192.237.1.209

#A8
auto eth1
iface eth1 inet static
    address 192.237.1.225
    netmask 255.255.255.252

#Routing
#A5
post-up route add -net 192.237.1.220 netmask 255.255.255.252 gw 192.237.1.209

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- HollowZero
```
#A8
auto eth0
iface eth0 inet static
    address 192.237.1.226
    netmask 255.255.255.252
    gateway 192.237.1.225

#Routing
post-up route add -net 192.237.1.220 netmask 255.255.255.252 gw 192.237.1.209

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- OuterRing
```
auto lo
iface lo inet loopback

#A7
auto eth0
iface eth0 inet static
    address 192.237.1.211
    netmask 255.255.255.248
    gateway 192.237.1.209

#A9
auto eth1
iface eth1 inet static
    address 192.237.1.129
    netmask 255.255.255.192

#Routing
#A5
post-up route add -net 192.237.1.220 netmask 255.255.255.252 gw 192.237.1.209

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Caesar (50 Host)
```
#A9
auto eth0
iface eth0 inet static
    address 192.237.1.130
    netmask 255.255.255.192
    gateway 192.237.1.129
```

- Burnice (5 Host)
```
#A9
auto eth0
iface eth0 inet static
    address 192.237.1.131
    netmask 255.255.255.192
    gateway 192.237.1.129
```

- LuminaSquare
```
auto lo
iface lo inet loopback

#A1
auto eth0
iface eth0 inet static
    address 192.237.1.218
    netmask 255.255.255.252
    gateway 192.237.1.217

#A3
auto eth1
iface eth1 inet static
    address 192.237.0.1
    netmask 255.255.255.0

#A2
auto eth2
iface eth2 inet static
    address 192.237.1.193
    netmask 255.255.255.248

#Routing
#A4
post-up route add -net 192.237.1.0 netmask 255.255.255.128 gw 192.237.1.195

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
  
- Jane (200 Host)
```
#A3
auto eth0
iface eth0 inet static
    address 192.237.0.2
    netmask 255.255.255.0
    gateway 192.237.0.1
```

- Policeboo (30 Host)
```
#A3
auto eth0
iface eth0 inet static
    address 192.237.0.3
    netmask 255.255.255.0
    gateway 192.237.0.1
```

- HIA
```
#A2
auto eth0
iface eth0 inet static
    address 192.237.1.194
    netmask 255.255.255.248
    gateway 192.237.1.193

#Routing
#A1
post-up route add -net 192.237.1.216 netmask 255.255.255.252 gw 192.237.1.193

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- BalletTwins
```
auto lo
iface lo inet loopback

#A2
auto eth0
iface eth0 inet static
    address 192.237.1.195
    netmask 255.255.255.248
    gateway 192.237.1.193

#A4
auto eth1
iface eth1 inet static
    address 192.237.1.1
    netmask 255.255.255.128

#Routing
#A1
post-up route add -net 192.237.1.216 netmask 255.255.255.128 gw 192.237.1.193

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Lycaon (20 Host)
```
#A4
auto eth0
iface eth0 inet static
    address 192.237.1.2
    netmask 255.255.255.128
    gateway 192.237.1.1
```

- Ellen (100 Host)
```
#A4
auto eth0
iface eth0 inet static
    address 192.237.1.3
    netmask 255.255.255.128
    gateway 192.237.1.1
```
## Misi 1 Nomor 4
**Testing web server HollowZero dan HIA**
![image](https://github.com/user-attachments/assets/36779bd4-a3b5-4546-b3b9-4b7ffa2d596b)
![image](https://github.com/user-attachments/assets/37ea6c27-fe05-4d4e-acfc-1e6b55ffacfd)

**Testing IP Client mendapatkan IP dari DHCP server**
![image](https://github.com/user-attachments/assets/85bf7fcb-fc5c-4257-9415-4d309ae507fa)
![image](https://github.com/user-attachments/assets/24e5bdf6-0e25-4dd5-803b-a7274ef4576d)
![image](https://github.com/user-attachments/assets/e1f941ee-71a4-4942-ab38-a5e4da35bc46)
![image](https://github.com/user-attachments/assets/768aa94d-afaa-4952-a2fa-261abb05ce9c)
![image](https://github.com/user-attachments/assets/c707b073-810f-42a1-a90d-f0ed5ab64ef6)
![image](https://github.com/user-attachments/assets/2b471053-941e-4e4e-9761-0ebd278c4803)

## Misi 2 Nomor 1
Jalankan skrip berikut di node Fairy
```
#/bin/bash

export DEBIAN_FRONTEND=noninteractive
apt update
apt install isc-dhcp-server netcat -y

service isc-dhcp-server start

cp ~/dhcpd.conf /etc/dhcp/dhcpd.conf
echo INTERFACESv4=\"eth0\" >/etc/default/isc-dhcp-server

service rsyslog start

service isc-dhcp-server restart
```

Buatlah file bernama dhcpd.conf yang berisikan config subnet dan netmask berikut:
<details>
  <summary>Click to expand</summary>
  
```
#A9
subnet 192.237.1.128 netmask 255.255.255.192 {
  range 192.237.1.130 192.237.1.189;
  option routers 192.237.1.129;
  option broadcast-address 192.237.1.190;
  option domain-name-servers 192.237.1.202;
  default-lease-time 600;
  max-lease-time 7200;
}

#A4
subnet 192.237.1.0 netmask 255.255.255.128 {
  range 192.237.1.2 192.237.1.21;
  range 192.237.1.22 192.237.1.125;
  option routers 192.237.1.1;
  option broadcast-address 192.237.1.126;
  option domain-name-servers 192.237.1.202;
  default-lease-time 600;
  max-lease-time 7200;
}

#A3
subnet 192.237.0.0 netmask 255.255.255.0 {
  range 192.237.0.2 192.237.0.253;
  range 192.237.0.2 192.237.0.253;
  option routers 192.237.0.1;
  option broadcast-address 192.237.1.254;
  option domain-name-servers 192.237.1.202;
  default-lease-time 600;
  max-lease-time 7200;
}

#A6
subnet 192.237.1.200 netmask 255.255.255.248 {}
```
</details>

## Misi 2 Nomor 2
Dalam node Fairy, jalankan command berikut
```
iptables -P INPUT DROP
iptables -A OUTPUT -p icmp --icmp-type echo-request -j ACCEPT
iptables -A INPUT -p icmp --icmp-type echo-reply -j ACCEPT
iptables -A INPUT -p icmp --icmp-type echo-request -j REJECT --reject-with icmp-host-unreachable
```
Dengan ini, semua node yang melakukan ping ke Fairy akan tertulis Host Unreachable dan Fairy bisa melakukan ping ke node manapun

**Hasil**

Ping dari node HIA <br>
![image](https://github.com/user-attachments/assets/967d8895-54b1-4daf-997b-9b690c2855f9)

Ping dari node Caesar <br>
![image](https://github.com/user-attachments/assets/49caf2de-6ead-4395-b254-083e1bc2d96a)

Ping Fairy ke node HIA <br>
![image](https://github.com/user-attachments/assets/e5b2f31f-e3f1-4398-966b-27f6a48b8173)

Ping Fairy ke node Caesar <br>
![image](https://github.com/user-attachments/assets/5ebe3116-ad90-45e0-8005-5c0af532bc60)

## Misi 2 Nomor 3
Dalam node HDD, jalankan command berikut
```
iptables -A INPUT -p tcp -s 192.237.1.203 --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp --dport 8080 -j REJECT
```

**Hasil**
Koneksi dari Fairy ke HDD

![image](https://github.com/user-attachments/assets/43fe40b2-3dd2-4a3e-8de1-3c6a091f4772)
![image](https://github.com/user-attachments/assets/34a33b03-df6a-4d40-88a1-d64d20fea021)

Ping dari node lain ke HDD

![image](https://github.com/user-attachments/assets/6bf1a4c3-719f-4395-a9b1-2edf0063931e)

## Misi 2 Nomor 4
Dalam node HollowZero, jalankan command berikut
```
iptables -A INPUT -p tcp -s 192.237.1.131 --dport 80 -m time --timestart 00:00 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -p tcp -s 192.237.1.130 --dport 80 -m time --timestart 00:00 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -p tcp -s 192.237.0.2 --dport 80 -m time --timestart 00:00 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -p tcp -s 192.237.0.3 --dport 80 -m time --timestart 00:00 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j REJECT
```

Ini akan memblokir semua node yang mencoba untuk melakukan ping ke HollowZero dan akan mengizinkan client dari SoC dan PubSec saja (mulai Senin hingga Jumat)

**Hasil**

![image](https://github.com/user-attachments/assets/3a5f283d-5331-44d5-a828-ff841204219f)
![image](https://github.com/user-attachments/assets/563ebcd5-4a18-433e-b95f-4a16138ade58)

Ping dari client yang tidak diizinkan
![image](https://github.com/user-attachments/assets/125dc1a2-ca04-4144-8e69-fcd6d1f9c6f8)

Apabila client yang diizinkan mengakses di waktu yang tidak ditentukan

![image](https://github.com/user-attachments/assets/713d8dfe-a992-43b3-a440-d84933470a8b)

## Misi 2 Nomor 5
Dalam node HIA, jalankan command berikut
```
iptables -A INPUT -p tcp -s <IP Ellen> --dport 80 -m time --timestart 08:00 --timestop 21:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat,Sun -j ACCEPT
iptables -A INPUT -p tcp -s <IP Lycaon> --dport 80 -m time --timestart 08:00 --timestop 21:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat,Sun -j ACCEPT
iptables -A INPUT -p tcp -s <IP Jane> --dport 80 -m time --timestart 03:00 --timestop 23:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat,Sun -j ACCEPT
iptables -A INPUT -p tcp -s <IP Policeboo> --dport 80 -m time --timestart 03:00 --timestop 23:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat,Sun -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j REJECT
```

**Hasil**

![image](https://github.com/user-attachments/assets/a31a84ca-5000-4358-99d6-3d9672791081)
![image](https://github.com/user-attachments/assets/e7660b02-44a7-44c5-9380-d65f20eb1592)

Jika diakses tidak pada waktunya <br>
![image](https://github.com/user-attachments/assets/2a912b65-ebff-41fc-9f65-a0201ce60f35)
![image](https://github.com/user-attachments/assets/76f9f6ec-d386-40d8-944f-ae6643750f13)

Jika diakses oleh client lain <br>
![image](https://github.com/user-attachments/assets/5cb0cd3e-1ee2-449a-81ae-1c7fa16a175b)

## Misi 2 Nomor 6
Dalam node HIA, jalankan command berikut
```
iptables -N PORTSCAN
iptables -A INPUT -p tcp --dport 1:100 -m state --state NEW -m recent --set --name portscan
iptables -A INPUT -p tcp --dport 1:100 -m state --state NEW -m recent --update --seconds 10 --hitcount 25 --name portscan -j PORTSCAN
iptables -A PORTSCAN -m recent --set --name blacklist
iptables -A PORTSCAN -j DROP
iptables -A INPUT -m recent --name blacklist --rcheck -j REJECT
iptables -A OUTPUT -m recent --name blacklist --rcheck -j REJECT
iptables -A PORTSCAN -j LOG --log-prefix='PORT SCAN DETECTED' --log-level 4
```

**Hasil**

Jalankan `nmap -p 1-100 192.237.1.194` pada node Jane dan seharusnya Jane tidak bisa melakukan ping, nc, dan curl ke HIA

![image](https://github.com/user-attachments/assets/a8e01153-9e40-48d8-974d-c5619a0e96dd)
![image](https://github.com/user-attachments/assets/ea5011d7-fd24-47f6-8acf-dfb29679dbb1)

Client tidak diblokir jika dalam rentang waktu 10 detik hanya melakukan scanning kurang dari 25 port
![image](https://github.com/user-attachments/assets/f8027dfa-b254-4fd8-a8df-b6305f20971b)

## Misi 2 Nomor 7
Dalam node HollowZero, jalankan command berikut
```
iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -m recent --set
iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -m recent --update --seconds 1 --hitcount 3 -j REJECT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

**Hasil**

Lakukan pengetesan dengan paralel curl seperti `parallel curl -s http://IP-HollowZero ::: IP-Caesar IP-Burnice IP-Jane IP-Policeboo`
![image](https://github.com/user-attachments/assets/a0ea5b33-d79c-421e-9d94-32b580a18300)

Output hanya muncul 2 kali, artinya iptables berjalan karena 1 IP hanya memperbolehkan 2 koneksi saja

## Misi 2 Nomor 8
Dalam node Burnice, jalankan command berikut
```
iptables -t nat -A PREROUTING -p tcp -j DNAT --to-destination <IP HollowZero> --dport 8080
iptables -A FORWARD -p tcp -d <IP HollowZero> -j ACCEPT
```

**Hasil**

![image](https://github.com/user-attachments/assets/d0ad89e0-cd88-4934-9dda-e41e536fee17)
![image](https://github.com/user-attachments/assets/0108210d-692d-4c4f-841f-6f86971cb4f6)
![image](https://github.com/user-attachments/assets/746f0a02-e861-4c79-9f27-dd11437c2534)

## Misi 3
Dalam node Burnice, jalankan command berikut
```
iptables --policy INPUT DROP
iptables --policy OUTPUT DROP
iptables --policy FORWARD DROP
```

**Hasil**

![image](https://github.com/user-attachments/assets/bec8f9a8-a8da-45de-b566-653edb038d77)
![image](https://github.com/user-attachments/assets/d2768e5c-081a-4f25-84c3-b9273df1a2c2)
![image](https://github.com/user-attachments/assets/c1ad4fd0-b82b-48e0-b619-c06dd515535e)
![image](https://github.com/user-attachments/assets/8a35d4dd-08cf-4bf3-8a33-7feec3c16136)

Node Burnice tidak dapat melakukan ping dan di ping, serta tidak bisa menerima nc dari node lain




















