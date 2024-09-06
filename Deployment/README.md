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



#### Sub-subtitle

[Cheatsheet Markdown bisa dilihat disini](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

## To-do (delete later)
- Virtualization
- Nginx
- Firewall
- Security (fail2ban, ufw, hardening ssh, intinya pengamanan dasar vm)
- Deployment DVWA (Live Demo, kalian bisa demoin mau hypervisor atau container)