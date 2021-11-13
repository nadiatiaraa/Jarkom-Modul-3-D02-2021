# Jarkom-Modul-3-D02-2021

Anggota :
- Sabrina Lydia Simanjuntak (05111940000107)
- Zulfayanti Sofia Solichin (05111940000189)
- Nadia Tiara Febriana (05111940000217)

# Soal
Luffy yang sudah menjadi Raja Bajak Laut ingin mengembangkan daerah kekuasaannya dengan membuat peta seperti berikut:

<img width="501" alt="Screen Shot 2021-11-12 at 17 20 19" src="https://user-images.githubusercontent.com/72669398/141451163-e456c534-c25e-4144-a8d6-4fdb1501fc98.png">

# Soal No 1
Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server

Pembuatan topologi dimana node EniesLobby digunakan sebagai DNS Server dengan menginstall `apt-get install bind9`, Jipangu sebagai DHCP Server dengan menginstall `apt-get install isc-dhcp-server`, dan Water7 sebagai proxy Server dengan menginstall `apt-get install squid`.

<img width="495" alt="modul3_no1" src="https://user-images.githubusercontent.com/72669398/141601807-45a7015e-444c-40b7-b581-5ae192fe0f5f.png">

# Soal No 2
Foosha sebagai DHCP Relay

Membuat node Foosha sebagai DHCP relay dengan menginstall `apt-get install isc-dhcp-relay`

<img width="495" alt="modul3_no2a" src="https://user-images.githubusercontent.com/72669398/141601811-211b1042-954c-4b69-acc7-c30cdf4a4325.png">

Setelah menginstall DHCP relay, pada node Jipangu masukkan `INTERFACES="eth0"` pada file `/etc/default/isc-dhcp-server` untuk mendapatkan layanan dari DHCP Server.

<img width="500" alt="modul3_no2b" src="https://user-images.githubusercontent.com/72669398/141601813-6b5e466b-3a0a-40fe-a6e6-a147abaead3f.png">

# Soal No 3, 4, 5, dan 6
Ada beberapa kriteria yang ingin dibuat oleh Luffy dan Zoro, yaitu:
Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari ``[prefix IP].1.20 - [prefix IP].1.99`` dan ``[prefix IP].1.150 - [prefix IP].1.169``(3).

Client yang melalui Switch3 mendapatkan range IP dari ``[prefix IP].3.30 - [prefix IP].3.50`` (4)

Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut. (5)

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.(6)

Melakukan konfigurasi pada DHCP Server dimana untuk Client yang melalui Switch1 mendapatkan range IP ``10.22.1.20 - 10.22.1.99`` dan ``10.22.1.150 - 10.22.1.169``. Sedangkan untuk Client yang melalui Switch3 menggunakan range IP ``10.22.3.30 - 10.22.3.50``. Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut. Durasi peminjaman alamat IP pada Client yang terhubung dengan Switch1 adalah 6 menit (360 detik), sedangkan durasi peminjaman Client yang terhubung dengan Switch2 adalah 12 menit (720 detik) dengan waktu maksimal alokasi peminjaman alamat IP adalah 120 menit (7200 detik). 

Untuk itu, maka ubah script file ``/etc/dhcp/dhcpd.conf`` pada node Jipangu menjadi sebagai berikut

```
subnet 10.22.1.0 netmask 255.255.255.0 {
    range 10.22.1.20 10.22.1.99;
    range 10.22.1.150 10.22.1.169;
    option routers 10.22.1.1;
    option broadcast-address 10.22.1.255;
    option domain-name-servers 10.22.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}

subnet 10.22.2.0 netmask 255.255.255.0 {}

subnet 10.22.3.0 netmask 255.255.255.0 {
    range 10.22.3.30 10.22.3.50;
    option routers 10.22.3.1;
    option broadcast-address 10.22.3.255;
    option domain-name-servers 10.22.2.2;
    default-lease-time 720;
    max-lease-time 7200;
}
```
Lalu lakukan restart DHCP server pada node Jipangu dengan perintah service ``isc-dhcp-server restart`` dan start DHCP relay pada node Foosha dengan perintah ``/etc/init.d/isc-dhcp-relay start``.

Kemudian ubah script ``/etc/network/interfaces`` pada node-node Client menjadi sebagai berikut

```
auto eth0
iface eth0 inet dhcp
```

Lalu, restart node Client.
Setelah itu, lakukan pengecekan IP setiap Client dengan menjalankan ip a, dan didapatkan hasil sebagai berikut

Client Loguetown

<img width="491" alt="modul3_no3456-a" src="https://user-images.githubusercontent.com/72669398/141601834-c5497052-f558-4a5d-8597-1260c57243a3.png">

Client Alabasta

<img width="489" alt="modul3_no3456-b" src="https://user-images.githubusercontent.com/72669398/141601845-4e0fd541-7f5a-4b97-ae81-b6517eaeeb68.png">

Client TottoLand

<img width="495" alt="modul3_no3456-c" src="https://user-images.githubusercontent.com/72669398/141601849-b6a64260-5e0f-407c-bd00-7d7c4538824e.png">

Client Skypie

<img width="496" alt="modul3_no3456-d" src="https://user-images.githubusercontent.com/72669398/141601855-94b9dcf3-f96c-40b0-8639-917614dd7cd0.png">

Untuk pengecekan client dapat terhubung dengan internet melalui DNS dari EniesLobby dapat menggunakan perintah ``cat /etc/resolv.conf``

<img width="331" alt="modul3_no3456-e" src="https://user-images.githubusercontent.com/72669398/141601856-e1fefccf-0325-4616-8428-32254ca8666f.png">
	
# Soal No 7
Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP ``[prefix IP].3.69 (7)``. 

Membuat Client Skypie memiliki alamat IP yang tetap dengan ``IP 10.22.3.69`` dengan cara menambahkan script file ``/etc/dhcp/dhcpd.conf`` pada node Jipangu sebagai berikut

```
host Skypie {
    hardware ethernet 42:dd:82:05:8f:ae; //hwaddress Skypie
    fixed-address 10.22.3.69;
}
```

Lalu restart DHCP server pada node Jipangu

Konfigurasi DHCP pada node Skypie adalah dengan menambahkan konfigurasi 

```
hwaddress ether 42:dd:82:05:8f:ae	//hwaddress Skypie
```

Pada network interface yang dapat diakses pada ``/etc/network/interfaces``.
Kemudian restart node Skypie.
Setelah itu dilakukan pengecekan dengan menjalankan ip a, dapat dilihat bahwa ip yang dipinjam adalah ``10.22.3.69``.

<img width="491" alt="modul3_no7" src="https://user-images.githubusercontent.com/72669398/141602061-8228de2c-46bc-41e4-b397-61d1c0aeb6c7.png">

# Soal No 8, 9, dan 10
Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000 (8). 

Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy (9). 

Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00) (10).

Membuat Client Loguetown dapat diakses dengan nama ``jualbelikapal.d02.com`` dengan ``port 5000``.
Website dibuat hanya dapat diakses oleh 2 USER yaitu dengan ``username luffybelikapald02`` dengan ``password luffy_d02`` dan ``username zorobelikapald02`` dengan ``password zoro_d02`` dengan menggunakan autentikasi user proxy dengan enkripsi MD5.
Website juga hanya dapat diakses pada hari Senin-Kamis pukul 07.00 - 11.00 dan hari  Selasa - Jumat pukul 17.00 - 03.00 keesokan harinya (sampai sabtu 03.00)

Oleh karena itu pada ``file /etc/squid/acl.conf`` pada node Water7 masukkan script sebagai berikut

```
acl AVAILABLE_WORKING time MTWH 07:00-11:00
acl AVAILABLE_WORKING time TWHF 17:00-24:00
acl AVAILABLE_WORKING time WHFA 00:00-03:00
```

Dan juga script berikut pad file ``/etc/squid/squid.conf``

```
include /etc/squid/acl.conf

http_port 5000
visible_hostname jualbelikapal.d02.com

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS AVAILABLE_WORKING
http_access deny all

```

Kemudian restart Squid pada node Water7.

Kemudian untuk membuat user luffybelikapald02 dengan password luffy_d02 dapat dijalankan

```
htpasswd -c -m /etc/squid/passwd luffybelikapald02
```

<img width="499" alt="modul3_no8910-a" src="https://user-images.githubusercontent.com/72669398/141642910-06fe0764-2bab-4722-b7d5-92e98bd9a22f.png">

Dan untuk membuat user zorobelikapald02 dengan password zoro_d02 dapat dijalankan

```
htpasswd -m /etc/squid/passwd zorobelikapald02
```

<img width="501" alt="modul3_no8910-b" src="https://user-images.githubusercontent.com/72669398/141642915-08757d3a-ac14-4b89-a119-bdff298adf42.png">

Buat folder ``/etc/bind/proxy/jualbelikapal.d02.com`` pada node EniesLobby dengan script sebagai berikut 

```
$TTL    604800
@       IN      SOA     jualbelikapal.d02.com. root.jualbelikapal.d02.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      jualbelikapal.d02.com.
@       IN      A       10.22.2.3       ;IP Water7

```

Lalu, buat zone pada folder ``/etc/bind/named.conf.local`` dengan script sebagai berikut

```
zone "jualbelikapal.d02.com" {
        type master;
        file "/etc/bind/proxy/jualbelikapal.d02.com";
};
```

Lalu restart DNS pada node EniesLobby

Setelah itu, aktifkan Proxy pada Client Loguetown dengan menjalankan

```
export http_proxy="http://jualbelikapal.d02.com:5000"
```

Kemudian periksa waktu uji coba dengan menjalankan date, lalu jalankan ``lynx its.ac.id`` untuk mencoba mengakses web, kemudian masukkan username dan password untuk login. Jika waktu uji coba merupakan waktu saat website dapat diakses maka user akan masuk ke website, namu jika waktu uji coba merupakan waktu saat website tidak dapat diakses maka akan terdapat tulisan Access Denied.


<img width="262" alt="modul3_no8910-c" src="https://user-images.githubusercontent.com/72669398/141642917-1a459635-6522-476c-8d7b-a2c097a3626a.png">


<img width="496" alt="modul3_no8910-d" src="https://user-images.githubusercontent.com/72669398/141642919-966cdf65-4001-4d81-8a56-1e93d8c623cd.png">


<img width="286" alt="modul3_no8910-e" src="https://user-images.githubusercontent.com/72669398/141642921-259a02f2-57f5-4d65-8bf3-fa8cc9395193.png">


<img width="492" alt="modul3_no8910-f" src="https://user-images.githubusercontent.com/72669398/141642922-c41dd826-1f4c-40a1-9c8b-7a0b20dd1d75.png">


# Soal No 11
Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie (11).

# Soal No 12
Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps (12). 

# Soal No 13
Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya (13).

# Kendala Pengerjaan
1. Pada saat ingin mengakses web jualbelikapal.d02.com, website terblokir (muncul internet positif)
2. Pada saat uji coba nomor 8, saat proxy yang dinyalakan menggunakan domain jualbelikapal.d02.com, website tidak dapat diakses
3. Pada saat menguji coba nomor 10, dimana uji coba dilakukan saat website seharusnya tidak dapat diakses, tetapi website dapat diakses
