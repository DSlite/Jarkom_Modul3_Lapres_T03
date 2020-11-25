# Lapres Modul 2 Jarkom 2020 - T03

**Oleh:**
  * I Made Dindra Setyadharma (05311840000008)
  * Rindi Kartika Sari (05311840000013)

---

## Soal 1

Setting UML agar memiliki topologi seperti berikut

![image](https://user-images.githubusercontent.com/17781660/100103797-54f63a00-2ea0-11eb-8fcc-51d1aeb64a3b.png)

**Solusi**\
Pertama kita setting `topologi.sh` pada virtual machine agar menambahkan beberapa UML baru dan mengatur seluruh network interface agar sesuai dengan gambar topologi seperti berikut.

![image](https://user-images.githubusercontent.com/17781660/100225245-e1643380-2f58-11eb-9f85-4c704956b5a7.png)

lalu pada `bye.sh` juga disetting dan menambahkan UML yang baru.

![image](https://user-images.githubusercontent.com/17781660/100225322-ffca2f00-2f58-11eb-821f-65de89675bbb.png)

Untuk Router **SURABAYA** disetting dulu `net.ipv4.ip_forward` pada `/etc/sysctl.conf` lalu diaktifkan dengan command `sysctl -p`

![image](https://user-images.githubusercontent.com/17781660/100225520-4d469c00-2f59-11eb-937b-7f9836ff4092.png)

Masing-masing UML akan disetting network interfacenya.

**SURABAYA**

![image](https://user-images.githubusercontent.com/17781660/100225663-85e67580-2f59-11eb-8a37-cbe2cc28dd5f.png)
![image](https://user-images.githubusercontent.com/17781660/100225713-9ac30900-2f59-11eb-82d8-93052bc1a31d.png)

**MALANG**

![image](https://user-images.githubusercontent.com/17781660/100225815-c34b0300-2f59-11eb-8fda-340c8bbbf9b7.png)

**TUBAN**

![image](https://user-images.githubusercontent.com/17781660/100225883-d8279680-2f59-11eb-9911-3b59f7089653.png)

**MOJOKERTO**

![image](https://user-images.githubusercontent.com/17781660/100225927-ebd2fd00-2f59-11eb-928d-540e04a5cdd7.png)

**SIDOARJO, GRESIK, BANYUWANGI, MADIUN**

![image](https://user-images.githubusercontent.com/17781660/100226021-0e651600-2f5a-11eb-8916-1f5fd5c1c760.png)

Lalu masing-masing UML akan di `service networking restart`. Untuk UML Client, command tersebut dijalankan setelah setting DHCP server (**TUBAN**) selesai.

## Soal 2

Setting Router **SURABAYA** agar menjadi DHCP relay antar DHCP server dan client.

**Solusi**\
Pada router, kita install DHCP relay dengan command `apt-get install isc-dhcp-relay`. Lalu kita buka `/etc/default/isc-dhcp-relay` dan mengedit konfigurasi seperti berikut.

![image](https://user-images.githubusercontent.com/17781660/100226513-d3afad80-2f5a-11eb-8356-23f5b7744337.png)

`SERVERS=` Kita isi dengan alamat IP DHCP server (**TUBAN**), dan `INTERFACES=` diisi dengan `eth1 eth2 eth3`, karena pada DHCP relay (**SURABAYA**) akan meneruskan DHCP request dari network interface **eth1** dan **eth2**, lalu meneruskannya ke DHCP server melalui **eth3**. Setelah itu service dapat direstart dengan command `service isc-dhcp-relay restart`

## Soal 3

Setting DHCP server (**TUBAN**) agar subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.

**Solusi**\
Pertama download DHCP server menggunakan command `apt-get install isc-dhcp-server`. lalu setting `INTERFACES` yang digunakan oleh **TUBAN** pada file `/etc/default/isc-dhcp-server`

![image](https://user-images.githubusercontent.com/17781660/100227289-df4fa400-2f5b-11eb-8f76-bfaae6d4ec3d.png)

Agar DHCP Server dapat berjalan dengan lancar, perlu deklarasi subnet yang terkoneksi pada **TUBAN** (subnet dari eth0 **TUBAN**) pada `/etc/dhcp/dhcpd.conf`. Untuk subnet 2 ini hanya harus dideklarasikan, tetapi tidak harus memiliki settingan dhcp.

![image](https://user-images.githubusercontent.com/17781660/100226863-4882e780-2f5b-11eb-8666-2ee56e862d65.png)

Lalu settingan subnet 1 sebagai berikut.

![image](https://user-images.githubusercontent.com/17781660/100227380-07d79e00-2f5c-11eb-84f3-5eb90879aecd.png)

## Soal 4

Setting DHCP server (**TUBAN**) agar subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.

**Solusi**\
Sama dengan soal sebelumnya, settingan subnet 3 sebagai berikut.

![image](https://user-images.githubusercontent.com/17781660/100227436-1e7df500-2f5c-11eb-887f-01b74820260d.png)

## Soal 5

Setting DHCP server (**TUBAN**) agar client mendapatkan DNS **MALANG** dan DNS 202.46.129.2 dari DHCP

**Solusi**\
Untuk settingan DNS harus menggunakan `,` antar DNS tidak seperti `range`.

![image](https://user-images.githubusercontent.com/17781660/100227535-42413b00-2f5c-11eb-8a07-201f9f286cc0.png)

## Soal 6

Setting DHCP server (**TUBAN**) agar subnet 1 mendapatkan peminjaman alamat IP selama 5 menit dan subnet 3 mendapatkan peminjaman alamat IP selama 10 menit.

**Solusi**\
Untuk soal ini `default-lease-time diedit dengan satuan detik.

![image](https://user-images.githubusercontent.com/17781660/100227632-6270fa00-2f5c-11eb-8442-2ed6f466ea03.png)

## Soal 7

Setting agar akses Proxy server (**MOJOKERTO**) memerlukan autentikasi dengan user: `userta_t03` dan password: `inipassw0rdta_t03`

**Solusi**\
Pertama kita download dulu squid dengan command `apt-get install squid` dan juga apache2-utils dengan command `apt-get install apache2-utils`. Lalu dibuat konfigurasi username dan password dengan command `htpasswd -c /etc/squid/passwd userta_t03`. Lalu nanti akan diminta password dan diinputkan dengan `inipassw0rdta_t03`.

Selanjutnya kita setting `/etc/squid/squid.conf` agar memiliki konfigurasi sebagai berikut

![image](https://user-images.githubusercontent.com/17781660/100228579-d95ac280-2f5d-11eb-92b7-13a00a739aa2.png)

Ketika kita mencoba koneksi proxy, maka akan muncul prompt untuk login.

![image](https://user-images.githubusercontent.com/17781660/100228390-913ba000-2f5d-11eb-843e-2fe701e21982.png)

## Soal 8

Setting agar Proxy server (**MOJOKERTO**) hanya dapat diakses pada hari Selasa-Rabu pukul 13.00-18.00.

**Solusi**\
Pertama kita edit `/etc/squid/acl.conf` agar menjadi sebagai berikut.

![image](https://user-images.githubusercontent.com/17781660/100228701-0c04bb00-2f5e-11eb-8e0f-bd4cf1214a9c.png)

Lalu pada `/etc/squid/squid.conf`, konfigurasi diedit menjadi berikut.

![image](https://user-images.githubusercontent.com/17781660/100228789-2e96d400-2f5e-11eb-9547-893790a6bc81.png)

## Soal 9

Setting agar Proxy server (**MOJOKERTO**) hanya dapat diakses pada hari Selasa-Kamis pukul 21.00-09.00 keesokan harinya.

**Solusi**\
Sama seperti soal berikutnya, file `/etc/squid/acl.conf` hanya perlu diedit menjadi seperti berikut.

![image](https://user-images.githubusercontent.com/17781660/100228882-55eda100-2f5e-11eb-9090-9a3994aaf91d.png)

## Soal 10

Setting Proxy server (**MOJOKERTO**) agar ketika mengakses **google.com**, maka akan diredirect menuju **monta.if.its.ac.id**.

**Solusi**\
Disini menggunakan program `squidguard` untuk melakukan redirectingnya. Pertama install squidguard dengan command `apt-get install squidguard`. lalu kita edit konfigurasi `/etc/squidguard/squidGuard.conf` agar menjadi sebagai berikut.

![image](https://user-images.githubusercontent.com/17781660/100229139-b8df3800-2f5e-11eb-9cff-49613f3d8495.png)

Konfigurasi tersebut akan meredirect seluruh domain dan url yang terdapat pada `domainlist` dan `urllist` ke `monta.if.its.ac.id`. Lalu jalankan command `mkdir /var/lib/squidguard/db/deny`. dan konfigurasikan file `/var/lib/squidguard/db/deny/domains` seperti berikut.

![image](https://user-images.githubusercontent.com/17781660/100229388-20958300-2f5f-11eb-83a3-9c411675e6dd.png)

Dan konfigurasikan file `/var/lib/squidguard/db/deny/urls` seperti berikut.

![image](https://user-images.githubusercontent.com/17781660/100229428-2d19db80-2f5f-11eb-8497-51c2f9a97dcd.png)

Setelah itu jalankan command `squidGuard -C all` untuk mengcompile konfigurasi squidguard. Lalu jalankan command `chown -R proxy. /var/lib/squidguard /var/log/squidguard` agar squidguard dapat mengedit directory tersebut. Lalu terakhir pada `/etc/squid/squid.conf` ditambahkan line berikut.

![image](https://user-images.githubusercontent.com/17781660/100229673-926dcc80-2f5f-11eb-873b-9e2502c77e19.png)

*Note: ketika membuka google.com harus protokol http karena squid dan squidguard tidak dapat redirect dari protokol https*

## Soal 11

Setting Proxy server (**MOJOKERTO**) agar mengubah error page default squid menjadi halaman yang disediakan

**Solusi**\
Setelah site yang telah disediakan didownload, pertama kita akan menjalankan command `mkdir /usr/share/squid/errors/error403`. lalu file yang telah didownload di `mv` ke directory tadi. Setelah itu konfigurasi squid di edit agar menambahkan `error_directory` dan `deny_info` sebagai berikut.

![image](https://user-images.githubusercontent.com/17781660/100230082-2fc90080-2f60-11eb-8758-2996f65c046a.png)

Maka ketika terjadi error akan muncul sebagai berikut.

![image](https://user-images.githubusercontent.com/17781660/100230330-90f0d400-2f60-11eb-9579-9c4c2bc968b4.png)

## Soal 12

Setting DNS server (**MALANG**) agar mengakses Proxy server (**MOJOKERTO**) dapat melalui domain **janganlupa-ta.t03.pw** dengan port **8080**.

**Solusi**\
Pada **MALANG**, download terlebih dahulu bind9 dengan command `apt-get install bind9`. Lalu pada `/etc/bind/named.conf.options` kita akan setting forwarders sebagai berikut.

![image](https://user-images.githubusercontent.com/17781660/100230786-1b393800-2f61-11eb-89bc-3d3465a57d82.png)

Lalu setting zone pada `/etc/bind/named.conf.local` untuk domain name dan zone reverse DNSnya.

![image](https://user-images.githubusercontent.com/17781660/100230902-3e63e780-2f61-11eb-87fd-eac1ede31877.png)

Lalu setting DNS record pada `/etc/bind/jarkom/janganlupa-ta.t03.pw` dan `/etc/bind/jarkom/73.151.10.in-addr.arpa`

![image](https://user-images.githubusercontent.com/17781660/100231031-6c492c00-2f61-11eb-81c9-a3e9887878ac.png)
![image](https://user-images.githubusercontent.com/17781660/100231265-bf22e380-2f61-11eb-8f25-c07b8d9107cc.png)

Setelah itu proxy server dapat diakses menggunakan domain `janganlupa-ta.t03.pw`, untuk port sendiri sudah disetting pada squid di **MOJOKERTO**.

![image](https://user-images.githubusercontent.com/17781660/100231379-e4afed00-2f61-11eb-8d2d-5fd6ece737d8.png)

