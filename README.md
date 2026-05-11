<ol>
    <li>How much data your publisher program will send to the message broker in one run?
    <br>
    Setiap kali fungsi p.publish_event dipanggil, program akan membungkus data ke dalam struct UserCreatedEventMessage, melakukan serialisasi data menggunakan format Borsh, dan mengirimkannya ke antrean (queue) yang ada di RabbitMQ. Karena ada 5 baris perintah publish_event yang dijalankan secara berurutan dalam fungsi main, maka total pesan yang masuk ke broker adalah 5.
    </li>
    <br>
    <li>The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?
    <br>
    URL “amqp://guest:guest@localhost:5672” adalah URL koneksi yang digunakan untuk menghubungkan program publisher ke broker RabbitMQ. URL ini terdiri dari beberapa bagian:
    - amqp://: Menunjukkan bahwa protokol yang digunakan adalah AMQP (Advanced Message Queuing Protocol).
    - guest:guest: Menunjukkan credential (username dan password) untuk mengakses broker RabbitMQ.
    - localhost:5672: Menunjukkan alamat host dan port di mana broker RabbitMQ berjalan.
    Karena URL ini sama dengan yang digunakan dalam program subscriber, itu berarti bahwa kedua program (publisher dan subscriber) terhubung ke broker RabbitMQ yang sama. Dengan demikian, pesan yang dikirim oleh publisher akan dapat diterima oleh subscriber yang terhubung ke broker tersebut.
    </li>
    <br>
   

</ol>

![RabbitMQ](assets/images/RabbitMQ.png)

![Publisher](assets/images/cargo_run_publisher.png)

![Subscriber](assets/images/cargo_run_subscriber.png)


Setelah menjalankan `cargo run` pada bagian **publisher**, berikut adalah apa yang terjadi:

1. **Pengiriman Event**: Publisher mengirimkan 5 data (event) berupa `UserCreatedEventMessage` ke Message Broker (RabbitMQ).
2. **Antrean Pesan**: RabbitMQ menampung pesan tersebut dan meneruskannya ke antrean (queue) yang sesuai.
3. **Pemrosesan oleh Subscriber**: Program **subscriber** yang sedang berjalan mendeteksi adanya pesan baru, mengambilnya dari antrean, dan memprosesnya.
4. **Hasil Akhir**: Konsol subscriber menampilkan log pesan yang diterima secara berurutan, lengkap dengan detail `user_id` dan `user_name` yang dikirimkan oleh publisher tadi.

Hal ini membuktikan bahwa sistem komunikasi asinkron antar layanan telah berjalan dengan baik.