# File Upload
## Definisi

File Upload merupakan celah kerentanan yang dimana web server memperbolehkan user untuk mengunggah file tanpa adanya validasi seperti nama file, tipe file, konten file, dan ukuran file.

File Upload terjadi karena pada web server tidak adanya pembatasan file yang akan diupload oleh user, disisi lain pihak developer merasa fiturnya aman yang sebenarnya dapat mudah untuk dilakukan bypass.

![file-upload](https://www.cobalt.io/hs-fs/hubfs/file-upload-vulnerabilities-example.png)

## Vulnerable Code

```php
<?php

if (isset($_FILES['file'])) {
    $target_dir = "uploads/";
    $target_file = $target_dir . basename($_FILES["file"]["name"]);

    if (move_uploaded_file($_FILES["file"]["tmp_name"], $target_file)) {
        echo "The file ". basename($_FILES["file"]["name"]). " has been uploaded.";
    } else {
        echo "Sorry, there was an error uploading your file.";
    }
}
?>
```

Dapat dilihat bahwa kode diatas tidak ada sebuah validasi terhadap jenis file dan ukuran file yang mungkin berpotensi sebagai serangan DoS (Denial of Service).

![chain-attack](https://pbs.twimg.com/media/F-FZQHabYAAqUhW?format=jpg&name=large)

## Contoh Serangan

- Menggunakan double extension.

```file.jpg.php```

- Menggunakan null byte.

```file.php%00.gif```

- Manipulasi konten dengan GIF89a;

```POST /images/upload/ HTTP/1.1
Host: target.com
...

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="setya404.php"
Content-Type: image/gif

GIF89a; <?php system("id") ?>
```

## Secure Code

```php
<?php
if (isset($_FILES['file'])) {
    $allowed_types = array('jpg', 'jpeg', 'png');
    $allowed_mime_types = array('image/jpeg', 'image/png');MIME type
    $max_size = 2 * 1024 * 1024;
    $upload_dir = "uploads/";

    $file_name = basename($_FILES["file"]["name"]);
    $file_size = $_FILES["file"]["size"];
    $file_tmp = $_FILES["file"]["tmp_name"];
    $file_ext = strtolower(pathinfo($file_name, PATHINFO_EXTENSION));
    $file_mime = mime_content_type($file_tmp);

    if (!in_array($file_ext, $allowed_types)) {
        echo "File extension is not allowed.";
        exit();
    }

    if (!in_array($file_mime, $allowed_mime_types)) {
        echo "File MIME type is not allowed.";
        exit();
    }

    if ($file_size > $max_size) {
        echo "File size exceeds the limit.";
        exit();
    }

    if (in_array($file_ext, array('jpg', 'jpeg', 'png', 'gif'))) {
        $image_info = getimagesize($file_tmp);
        if ($image_info === false) {
            echo "File header is invalid or not a real image.";
            exit();
        }
    }

    $new_file_name = uniqid() . "." . $file_ext;

    if (move_uploaded_file($file_tmp, $upload_dir . $new_file_name)) {
        echo "File uploaded successfully!";
    } else {
        echo "Error uploading file.";
    }
}
?>

```

Penjelasan kode diatas sebagai berikut:
- `Validasi tipe file`: Hanya memperbolehkan file dengan ekstensi tertentu.
- `Validasi ukuran file`: Membatasi ukuran file yang dapat diunggah.
- `Validasi tipe MIME`: Lapisan keamanan ekstra karena penyerang bisa saja mengubah ekstensi file.
- `Validasi header file gambar`: Digunakan untuk memeriksa header file gambar dan memastikan bahwa file tersebut adalah gambar asli dan bukan file berbahaya dengan ekstensi gambar yang dimodifikasi.

## Referensi
1. https://portswigger.net/web-security/file-upload
2. https://cyberw1ng.medium.com/understanding-file-upload-vulnerabilities-in-web-app-penetration-testing-6de583fba63f
3. https://github.com/daffainfo/AllAboutBugBounty/blob/master/Arbitrary%20File%20Upload.md