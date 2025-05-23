## Original Code of Broadcast Chat.

![alt text](./images/first.png)
![alt text](./images/second.png)
![alt text](./images/third.png)
![alt text](./images/fourth.png)

Saat menjalankan satu server dan tiga client, pesan yang dikirim dari satu client akan di-broadcast ke semua client yang terhubung. Sebagai contoh, ketika client pertama mengetik "hello 1", pesan tersebut akan diterima oleh server kemudian di-broadcast ke semua client (termasuk client pertama sendiri). Hal yang sama terjadi ketika client kedua mengirim "hello 2" dan client ketiga mengirim "hello 3". Setiap client menerima semua pesan yang dikirim dari client manapun, menunjukkan bahwa komunikasi broadcast berfungsi dengan baik. Server juga menampilkan informasi koneksi dan pesan yang diterima untuk keperluan monitoring.

Aplikasi ini mendemonstrasikan penggunaan asynchronous programming di Rust menggunakan Tokio runtime, pengelolaan multiple connections secara concurrent, dan implementasi pola publish-subscribe menggunakan channel untuk broadcast message.

## Modifying the Websocket Port 

![alt text](./images/fifth.png)
![alt text](./images/sixth.png)
![alt text](./images/seventh.png)
![alt text](./images/eighth.png)

Untuk mengubah port yang digunakan pada aplikasi chat ini, diperlukan modifikasi pada dua file yang berbeda karena komunikasi terjadi antara dua sisi: server dan client. Pada file `server.rs`, port diubah pada baris kode `TcpListener::bind("127.0.0.1:8080").await?;`. Sedangkan pada sisi client, modifikasi dilakukan pada file `client.rs` di baris `ClientBuilder::from_uri(Uri::from_static("ws://127.0.0.1:8080"))`. Kedua file tersebut menggunakan protokol WebSocket yang sama, ditunjukkan dengan penggunaan URI dengan skema "ws://" pada client dan implementasi WebSocket server di file server.rs melalui `tokio_websockets` crate. WebSocket protocol memungkinkan komunikasi dua arah (bidirectional) secara real-time antara server dan client, yang merupakan teknologi ideal untuk aplikasi chat.

## Small changes, add IP and Port

![alt text](./images/ninth.png)
![alt text](./images/tenth.png)
![alt text](./images/eleventh.png)
![alt text](./images/twelveth.png)

Saya melakukan perubahan pada *server side* karena server sudah melacak pesan mana yang sesuai dengan alamat klien tertentu. Ketika klien pertama kali terhubung, server membuat sebuah *thread* asinkronus baru yang bertugas menangani *message streams* dari klien tersebut. Setiap koneksi klien mendapatkan *thread* dan *handler* terpisah dengan informasi alamat socket yang disimpan dalam parameter `addr`.

Dengan memodifikasi baris `bcast_tx.send(text.into())?;` menjadi `bcast_tx.send(format!("{addr:?} {text:?}"))?;`, saya memanfaatkan parameter `addr` yang sudah tersedia untuk menyisipkan informasi alamat pengirim pada pesan yang di-broadcast. Hal ini terlihat jelas pada gambar, dimana setiap pesan yang diterima klien menampilkan alamat IP dan port pengirim.