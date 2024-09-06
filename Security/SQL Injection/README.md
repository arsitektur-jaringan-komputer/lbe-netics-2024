# SQL Injection
## Definisi

SQL Injection merupakan serangan server side yang menggunakan database sebagai utilisasi dari serangannya. Serangan ini biasa terjadi karena kurang baiknya implementasi sanitasi user input. Penyerang menyisipkan bahasa SQL untuk melancarkan serangannya yang dapat berdampak pada: file inclusion, RCE, auth bypass, information disclosure, dan lain-lain.

![SQL](https://techterms.com/img/xl/sql_injection_1567.png)

## Tipe SQL Injection

1. `Union-based SQL Injection`: Penyerang menggunakan perintah UNION untuk menggabungkan hasil kueri yang sah dengan kueri berbahaya, lalu mendapatkan data dari tabel yang berbeda.
2. `Boolean-based Blind SQL Injection`: Penyerang mengajukan kueri yang menyebabkan respons aplikasi berubah berdasarkan kondisi benar atau salah.
3. `Time-based Blind SQL Injection`: Penyerang menggunakan fungsi yang menyebabkan penundaan waktu pada respons server (seperti SLEEP()), memungkinkan penyerang menyimpulkan hasil kueri SQL berdasarkan durasi respons.

![Type-SQL](https://media.geeksforgeeks.org/wp-content/uploads/20220716180638/types.PNG)

## Vulnerable Code

```php
<?php
$conn = mysqli_connect("localhost", "root", "", "testdb");

$username = $_POST['username'];
$password = $_POST['password'];

$sql = "SELECT * FROM users WHERE username = '$username' AND password = '$password'";

$result = mysqli_query($conn, $sql);

if (mysqli_num_rows($result) > 0) {
    echo "Login berhasil!";
} else {
    echo "Login gagal!";
}

mysqli_close($conn);
?>
```

Dapat dilihat tidak ada sanitasi pada form input username dan password, sehingga penyerang dapat memanipulasi input.

Username/Password: `' OR 1=1-- '`

Sehingga query SQL yang dihasilkan menjadi seperti ini.
```sql
SELECT * FROM users WHERE username = '' OR 1=1-- '' AND password = '$password';
```

Yang dimana query tersebut sebagai kondisi `always true` tanpa adanya kredensial yang valid.

## Secure Code

```php
<?php
$conn = mysqli_connect("localhost", "root", "", "testdb");

$username = $_POST['username'];
$password = $_POST['password'];

$sql = "SELECT * FROM users WHERE username = ? AND password = ?";

$stmt = mysqli_prepare($conn, $sql);

mysqli_stmt_bind_param($stmt, "ss", $username, $password);

mysqli_stmt_execute($stmt);

$result = mysqli_stmt_get_result($stmt);

if (mysqli_num_rows($result) > 0) {
    echo "Login berhasil!";
} else {
    echo "Login gagal!";
}

mysqli_stmt_close($stmt);
mysqli_close($conn);
?>
```

Pada kode diatas menggunakan `prepare statement`, yang dimana input dari user akan divalidasi dan dicheck, sehingga saat penyerang menginputkan karakter SQL berbahaya akan dianggap sebagai data dan bukan bagian dari query perintah SQL.

## Referensi

1. https://portswigger.net/web-security/sql-injection
2. https://owasp.org/www-community/attacks/SQL_Injection