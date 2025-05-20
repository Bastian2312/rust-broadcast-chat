## 

![alt text](./images/first.png)
![alt text](./images/second.png)
![alt text](./images/third.png)
![alt text](./images/fourth.png)

Saat menjalankan satu server dan tiga client, pesan yang dikirim dari satu client akan di-broadcast ke semua client yang terhubung. Sebagai contoh, ketika client pertama mengetik "hello 1", pesan tersebut akan diterima oleh server kemudian di-broadcast ke semua client (termasuk client pertama sendiri). Hal yang sama terjadi ketika client kedua mengirim "hello 2" dan client ketiga mengirim "hello 3". Setiap client menerima semua pesan yang dikirim dari client manapun, menunjukkan bahwa komunikasi broadcast berfungsi dengan baik. Server juga menampilkan informasi koneksi dan pesan yang diterima untuk keperluan monitoring.

Aplikasi ini mendemonstrasikan penggunaan asynchronous programming di Rust menggunakan Tokio runtime, pengelolaan multiple connections secara concurrent, dan implementasi pola publish-subscribe menggunakan channel untuk broadcast message.