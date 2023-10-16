# Jarkom-Modul-2-D26-2023

* Fathan Abi Karami (5025211156)
* Alya Putri Salma (5025211174)

# Soal 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 
![](./img/Topologi4.png)

## pengerjaan
buat topologi sesuai gambar

![](./img/Topologi.png)
configure network tiap node

Pandudewanata:
``` txt
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.204.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.204.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.204.3.1
	netmask 255.255.255.0
```
DNSMASTER-Yudhistira:
``` txt
auto eth0
iface eth0 inet static
	address 192.204.1.2
	netmask 255.255.255.0
	gateway 192.204.1.1
```
DNSSLAVE-Werkudara:
``` txt
auto eth0
iface eth0 inet static
	address 192.204.1.3
	netmask 255.255.255.0
	gateway 192.204.1.1
```
Client-Nakula:
``` txt
auto eth0
iface eth0 inet static
	address 192.204.2.2
	netmask 255.255.255.0
	gateway 192.204.2.1
```
Client-Sadewa:
``` txt
auto eth0
iface eth0 inet static
	address 192.204.2.3
	netmask 255.255.255.0
	gateway 192.204.2.1
```
Arjuna:
``` txt
auto eth0
iface eth0 inet static
	address 192.204.3.2
	netmask 255.255.255.0
	gateway 192.204.3.1
```
Abimanyu:
``` txt
auto eth0
iface eth0 inet static
	address 192.204.3.3
	netmask 255.255.255.0
	gateway 192.204.3.1
```
Prabukusuma:
``` txt
auto eth0
iface eth0 inet static
	address 192.204.3.4
	netmask 255.255.255.0
	gateway 192.204.3.1
```
Wisanggeni
``` txt
auto eth0
iface eth0 inet static
	address 192.204.3.5
	netmask 255.255.255.0
	gateway 192.204.3.1
```

pada pandudewanata buat script start.sh untuk konfigurasi 

start.sh:

![](./img/1PanduStart.png)

cari IP DNS dengan menngunakan
``` bash
cat /etc/resolv.conf
```
didapat 
``` txt
nameserver 192.168.122.1
```
pada semua node buat script start.sh berisikan:
![](./img/1NakulaStart.png)

## Testing
lakukan testing dengan melakukan ping google.com
![](./img/1NakulaPing.png)

didapat bahwa dapat terhubung ke internet

# Soal 2 dan 3
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok

## Pengerjaan:
pada node DNSMASTER-Yudhistira modifikasi script start.sh untuk menginsitall bind9

start.sh:

![](./img/2YudhisStart.png)

jalankan script. kemudian buat konfigurasi named.conf.local. copy named.conf.local dari /etc/bind9/named.conf.local ke /root agar konfigurasi tidak hilang. modifikasi named.conf.local di root

![](./img/2namedconflocal.png)

buat script confWeb.sh untuk mengkonfigurasi arjuna dan abimanyu

kemudian pada confWeb.sh tambahkan command untuk men-copy kembali ke /etc/bind.

pada confWeb.sh tambahkan command mkdir untuk membuat direktori arjuna dan abimanyu dan copy /etc/bind/db.local ke kedua direktori dengan nama arjuna.d26.com dan abimanyu.d26.com yang telah dibuat. copy kembali arjuna.d26.com dan abimanyu.d26.com ke /root agar tidak hilang. kemudian modifikasi keduanya yang ada di root, setelah dimodifikasi copy kembali ke /etc/bind/arjuna dan /etc/bind/abimanyu 

arjuna.d26.com:

![](./img/2arjunad26com.png)

abimanyu.d26.com:

![](./img/2abimanyud26com.png)

kemudian copy kembali keduanya ke direktori arjuna dan abimnayu dan restart service bind9

script confWeb.sh:

![](./img/2confWeb1.png)
![](./img/2confWeb2.png)

jalankan confWeb.sh

pada client nakula dan sadewa modifikasi script untuk menambahkan Yudhistira sebagai DNSMASTER
![](./img/2NakulaStart.png)

## Testing
ping arjuna.d26.com

![](./img/2PingArjuna.png)

Ping www.arjuna.d26.com

![](./img/2pingwwwarjuna.png)

ping abimanyu.d26.com

![](./img/2pingabimanyud26com.png)

ping www.abimanyu.d26.com

![](./img/2pingwwwabimanyu.png)

# Soal 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

## Pengerjaan
pada abimanyu.d26.com di /root pada node yudhistira tambahkan 
``` txt
parikesit	IN	A	192.204.3.3	;
```
jalankan kembali confWeb.sh

abimanyu.d26.com:

![](./img/4abimanyud26com.png)

## Testing
ping parikesit.abimanyu.d26.com

![](./img/4pingparikesit.png)

# Soal 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

## Pengerjaan
edit named.conf.local dengan menambahkan:

![](./img/5namedconflocal.png)

pada confWeb.sh tambahkan command untuk men-copy /etc/bind/db.local ke /etc/bind/abimanyu dengan nama 3.204.192.in-addr.arpa, kemudian copy file 3.204.192.in-addr.arpa ke root agar tidak hilang. edit 3.204.192.in-addr.arpa di root menjadi:

![](./img/53204192inaddrarpa.png)

tambahkan comman untuk mencopy 3.204.192.in-addr.arpa dari .root ke /etc/bind/abimanyu

![](./img/5confWeb.png)

jalankan confWeb.sh

## Tesitng
jalankan command
``` txt
host -t PTR 192.204.3.3
```
![](./img/5hostptr19220433.png)

# Soal 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama

## Pengerjaan 
Tambahkan konfigurasi named.conf.local di DNSMASTER-Yudhistira:

![](./img/6yudhisnamedconf1.png)
![](./img/6yudhisnamedconf2.png)
![](./img/6yudhisnamedconf3.png)

jalankan kembali confWeb.sh

pada node DNSSLAVE-Werkudara buat script start.sh untuk instalasi bind9

start.sh

![](./img/6werkustart.png)

jalankan script. kemudian buat konfigurasi named.conf.local. copy named.conf.local dari /etc/bind9/named.conf.local ke /root agar konfigurasi tidak hilang. modifikasi named.conf.local di root

![](./img/6Werkunamedconf1.png)
![](./img/6Werkunamedconf2.png)


buat script confWeb.sh untuk menn-copy named.conf.local dari /root ke /etc/bind/ dan restart bind9

![](./img/6WerkuconfWeb.png)

## Testing
stop service bind9 di yudhistira:
``` txt
service bind9 stop
```
![](./img/6servicebind9stop.png)

pada client edit start.sh untuk tambahkan IP DNSSLAVE-Werkudara ke /etc/resolv.conf

![](./img/6werkustart2.png)

lakukan ping terhadap abimanyu.d26.com

![](./img/6pingabimanyu.png)

# Soal 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

## Pengerjaan
pada abimanyu.d26.com di node DNSMASTER-Yudhistira tambahkan konfigurasi menjadi:

![](./img/7yudhisabimanyu.png)

kemudian edit /etc/bind/named.conf.options

![](./img/7yudhisnamedconfoptions.png)

copy named.conf.options dari /etc/bind ke /root agar tidak hilang.

edit script confWeb.sh untuk men-copy kembali named.conf.options dari /root ke /etc/bind

![](./img/7yudhisconfweb.png)

jalankan confWeb.sh

pada node DNSSLAVE-Werkudara edit /etc/bind/named.conf.options

![](./img/7Werkunamedconfoptions.png)

copy named.conf.options dari /etc/bind ke /root agar tidak hilang.

edit script confWeb.sh untuk men-copy kembali named.conf.options dari /root ke /etc/bind

![](./img/7Werkuconfweb.png)

Edit named.conf.local, tambahkan:

![](./img/7Werkunamedconflocal.png)

pada confWeb.sh tambahkan command mkdir untuk membuat direktori baratayuda dan copy /etc/bind/db.local ke baratayuda dengan nama baratayuda.abimanyu.d26.com. copy kembali baratayuda.abimanyu.d26.com ke /root agar tidak hilang. kemudian modifikasi baratayuda.abimanyu.d26.com yang ada di root, lalu copy kembali ke /etc/bind/baratayuda

baratayuda.abimanyu.d26.com:

![](./img/7werkubaratayuda.png)

restart service bind9

## Testing
ping baratayuda.abimanyu.d26.com

![](./img/7pingbaratayuda.png)

ping www.baratayuda.abimanyu.d26.com

![](./img/7pingwwwbaratayuda.png)

# Soal 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu

## Pengerjaan:
edit baratayuda.abimanyu.d26.com pada node werkudara menjadi:

![](./img/8werkubaratayuda.png)

jalankan confWeb.sh

## Testing
ping rjp.baratayuda.abimanyu.d26.com

![](./img/8pingrjpbaratayuda.png)

ping www.rjp.baratayuda.abimanyu.d26.com

![](./img/8pingwwwrjpbaratayuda.png)













