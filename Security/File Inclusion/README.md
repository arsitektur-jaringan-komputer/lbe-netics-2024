# File Inclusion
## Definisi

File Inclusion atau yang sering disebut sebagai Local File Inclusion adalah jenis serangan yang mengeksploitasi aplikasi untuk menyertakan file lokal dari sistem server. File yang disertakan mungkin file sensitif seperti file konfigurasi, file log, atau file sistem lain yang seharusnya tidak diakses pengguna.

![LFI](https://miro.medium.com/v2/resize:fit:644/1*UPMlwBWgKMSUzSvY5mt5uw.png)

## Vulnerable Code

```php
<?php
if (isset($_GET['page'])) {
    $page = $_GET['page'];
    include($page);
} else {
    echo "Page not found.";
}
?>
```

Karena tidak adanya sanitasi terhadap input yang diberikan, maka kode tersebut rentan terhadap `Local File Inclusion`.

## Contoh Serangan

```
# Request
http://localhost/vulnerabilities/fi/?page=file2.php

# Response
"I needed a password eight characters long so I picked Snow White and the Seven Dwarves." ~ Nick Helm

# Request
http://localhost/vulnerabilities/fi/?page=/etc/passwd

# Response
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/bin/false
mysql:x:101:101:MySQL Server,,,:/nonexistent:/bin/false
```

## Secure Code

```php
<?php
$whitelist = ['home.php', 'about.php', 'contact.php'];

if (isset($_GET['page']) && in_array($_GET['page'], $whitelist)) {
    include($_GET['page']);
} else {
    echo "Page not found.";
}
?>
```

Kode diatas menerapkan whitelist, sehingga melakukan request file diluar dari whitelist mendapatkan response `Page not found`.

## Mitigasi

- `Validasi input`: Pastikan hanya file yang diizinkan yang bisa di-include, misalnya hanya file dari direktori tertentu atau dengan ekstensi tertentu (seperti .php).
- `Gunakan whitelist`: Daripada mengandalkan input langsung dari pengguna, gunakan daftar file yang diizinkan untuk dimuat.

## Referensi

1. https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion
2. https://portswigger.net/web-security/file-path-traversal