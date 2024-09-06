# Deployment

## Virtualization
Pernahkah kalian melihat sebuah rumah besar yang mewah dengan banyak sekali kamar, nahh sama halnya dengan hal tersebut tanpa virtualisasi, setiap orang membutuhkan rumah sendiri untuk tinggal, sehingga banyak hal hal (tanah, material bangunan, dll.) yang tidak digunakan. Dengan virtualisasi, satu rumah besar dapat dibagi menjadi beberapa kamar, masing-masing dengan pintu dan fasilitas masing masing, sehingga beberapa penghuni dapat tinggal dalam satu rumah besar dengan efisien. Dalam konteks ini, Rumah besar dapat sebagai Hardware atau servernya, Kamar sebagai VM, penghuni sebagai OS atau software yang berada dalam VM, dan Hypervisor dapat sebagai house keeper yang mengatur pembagian dan penggunaan kamar. 

Jadi dapat disimpulkan, virtualisasi adalah teknologi yang memungkinkan pembuatan mesin virtual di dalam satu fisik server. Dengan menggunakan hypervisor, virtualisasi memungkinkan pengelolaan beberapa sistem operasi atau aplikasi yang berjalan secara mandiri. Konsep dasar virtualisasi melibatkan isolasi sumber daya antara mesin virtual, sehingga setiap mesin virtual dapat beroperasi seolah-olah menjadi mesin fisik yang terpisah.

![Virt](./assets/virt.png)

## Contenarization 
Berbeda dengan virtualisasi, kontainerisasi dianalogikan sebagai sebuah kapal kargo besar yang mengangkut berbagai macam barang di dalam sebuah kontainer. Setiap kontainer dapat berisi barang yang berbeda dan memiliki tujuan yang berbeda pula, namun semuanya diangkut menggunakan satu kapal yang sama. Dalam konteks ini, kapal kargo dapat sebagai Kernel sistem operasi yang sama, peti kemas sebagai Kontainer, barang di dalam peti kemas sebagai Aplikasi dan dependensinya, dan pelabuhan sebagai Platform seperti Docker atau Kubernetes yang mengelola kontainer

Jadi dapat disimpulkan, Containerization adalah teknologi yang memungkinkan pengemasan aplikasi dan dependensinya ke dalam sebuah wadah (container) yang dapat dijalankan secara konsisten di berbagai lingkungan komputasi, tanpa perlu mengubah kode atau konfigurasi aplikasi itu sendiri. Container merupakan unit yang portabel, ringan, dan dapat diisolasi, yang mengemas aplikasi, library, dan konfigurasi menjadi satu entitas yang dapat dijalankan di lingkungan yang berbeda, seperti lokal, cloud, atau pusat data.

![](./assets/cont.png)

## Virtualization VS Contenarization
- Virtualisasi menggunakan hypervisor untuk menciptakan mesin virtual yang masing-masing memerlukan sistem operasi lengkap dan isolasi sumber daya seperti CPU, RAM, dan penyimpanan. Sebaliknya, kontainerisasi menggunakan teknologi seperti Docker untuk membuat kontainer yang berbagi sistem operasi host.

- Virtualisasi memungkinkan menjalankan berbagai sistem operasi dan aplikasi secara bersamaan dalam mesin virtual yang terisolasi. Sementara itu, kontainerisasi memungkinkan menjalankan aplikasi yang dibungkus dalam kontainer pada host yang sama, dengan berbagi kernel sistem operasi yang sama.

- Virtualisasi lebih cocok untuk aplikasi yang membutuhkan isolasi penuh, konfigurasi kompleks, dan dukungan untuk berbagai sistem operasi. Di sisi lain, kontainerisasi lebih ideal untuk aplikasi yang ringan, portabel, dan dapat dijalankan di berbagai lingkungan komputasi.

- Proses startup pada virtualisasi memerlukan waktu lebih lama karena melibatkan booting sistem operasi dan konfigurasi tambahan pada setiap mesin virtual. Sementara itu, kontainerisasi memungkinkan proses deploy dan startup yang lebih cepat karena hanya perlu menjalankan kontainer yang sudah dikemas dan siap dijalankan.

![](./assets/vs.png)

## Nginx 
Berdasarkan dengan penjelasan pada web documention dari [NGINX](https://nginx.org/en/), Nginx merupakan sebuah server HTTP dan reverse proxy, server proxy untuk email, serta server proxy generik untuk TCP/UDP, yang awalnya ditulis oleh Igor Sysoev. Nginx juga merupakan salah satu web server yang bersifat open source yang dapat juga digunakan untuk load balancing dan caching. NGINX sangat populer karena kecepatan dan kemampuannya menangani banyak permintaan secara efisien. Berbeda dengan server web biasanya, NGINX dirancang untuk dapat menangani ribuan permintaan dalam waktu yang bersamaan tanpa menguras resources dari server itu sendiri. Berikut adalah default nginx configuration:


#### Default NGINX Configuration
```conf
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ =404;
        }
}
```
- `Listen 80`, menyatakan bahwa nginx server nantinya akan menangkap request melalui port 80 yang merupakan post standar dari http 
- `listen [::]:80` default_server, sama seperti sebelumnya, yang berbeda adalah bagian ini menerima dari ipv6 yang dimana notasinya sama seperti 0.0.0.0
- `root /var/www/html`, menyatakan direktori root tempat file website disimpan. Dalam kasus ini, semua file HTML, CSS, JavaScript, dan resources lainnya diambil dari folder /var/www/html.
-  `index index.html index.htm index.nginx-debian.html`, menentukan Urutan prioritas file pada direktory root, prioritasnya adalah index.html, jika tidak ditemukan maka NGINX mencari index.htm, dan jika itu juga tidak ada, NGINX akan mencoba index.nginx-debian.html
- `server_name _`, Ini berarti server ini menerima permintaan untuk semua nama domain atau subdomain. Hal ini ditunjukkan dengan penggunaan wild card `_`. 
- `try_files $uri $uri/ =404;`, pada bagian ini terdapat sebuah direktif yang berguna memberi tahu NGINX untuk mencoba beberapa file atau direktori yang diminta oleh user, jika gagal akan mengeluarkan respons 404. 


### Apa saja hal lain yang dapat dilakukan oleh NGINX?

- Reverse Proxy: NGINX bertindak sebagai jembatan penghubung antara user dan server backend (server tempat aplikasi atau website sebenarnya berjalan). Misalnya, jika ada banyak permintaan dari user, NGINX akan melakukan forwarding / meneruskan permintaan tersebut ke server lain, sehingga server utama tidak kelebihan beban.

![](./assets/nginx.png)

##### Simple Reverse Proxy Configuration
```conf
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend_server;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
- Load Balancer: Jika sebuah aplikasi atau website di-hosting pada beberapa server, NGINX bisa mendistribusikan permintaan dari user ke server-server tersebut secara merata. Ini memastikan bahwa semua server bekerja sama untuk menangani lalu lintas, sehingga website tetap cepat dan tidak down.

##### Simple Load Balancer Configuration
```conf
http {
    upstream backend_servers {
        server backend1.example.com;
        server backend2.example.com;
        server backend3.example.com;
    }

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://backend_servers;
        }
    }
}

```


- Caching: NGINX juga bisa menyimpan halaman website sementara (cache), sehingga ketika user yang sama mengunjungi kembali website, halaman dapat dimuat lebih cepat tanpa perlu mengambilnya lagi dari server backend.

##### Simple Caching Configuration
```conf
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m max_size=10g inactive=60m use_temp_path=off;

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_cache my_cache;
        proxy_pass http://backend_server;
        proxy_cache_valid 200 302 10m;
        proxy_cache_valid 404 1m;
        add_header X-Cache-Status $upstream_cache_status;
    }
}

```



#### Sub-subtitle

[Cheatsheet Markdown bisa dilihat disini](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

## To-do (delete later)
- Virtualization
- Nginx
- Firewall
- Security (fail2ban, ufw, hardening ssh, intinya pengamanan dasar vm)
- Deployment DVWA (Live Demo, kalian bisa demoin mau hypervisor atau container)