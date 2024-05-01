Nama: Muhammad Rafi Zia Ulhaq<br>
NPM: 2206814551<br>
Kelas: Pemrograman Lanjut B<br>

# Module 9

### Reflection
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
**Jawab**
a. Unary: Klien mengirimkan satu permintaan ke server dan menunggu sampai server mengembalikan respons tersebut. Unary cocok digunakan ketika klien hanya membutuhkan satu respons dari server seperti mengambil satu item dari database, mengautentikasi pengguna, atau melakukan perhitungan dan mendapatkan hasilnya.<br>
b. Server Streaming: Klien mengirimkan permintaan ke server dan server mengirimkan beberapa respons berkelanjutan kepada klien. Server Streaming cocok digunakan ketika server perlu mengirimkan sejumlah data yang cukup besar kepada klien, seperti *real-time updates* harga pasar saham, *update* cuaca, *news feeds*, atau mengirim file besar dalam beberapa bagian.<br>
c. Bi-directional: Klien dan server dapat mengirim banyak pesan satu sama lain secara terus menerus. Bi-directional cocok digunakan dalam skenario di mana terdapat kebutuhan akan komunikasi interaktif yang berkelanjutan. Misalnya, dalam aplikasi chat, Bi-directional dapat digunakan untuk mengirim dan menerima pesan secara *real-time* antara klien dan server.<br>

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
**Jawab**
Beberapa pertimbangan keamanan saat mengimplementasikan gRPC di Rust yaitu:
* Autentikasi: Memastikan bahwa klien diautentikasi secara sah. Middleware yang dapat digunakan untuk autentikasi antara lain JWT atau OAuth2.
* Otorisasi: Memastikan bahwa klien hanya memiliki akses yang sesuai dengan wewenangnya. 
* Enkripsi data: Melindungi kerahasiaan data yang dikirim antara klien dan server. gRPC secara default menggunakan HTTP/2, yang mendukung enkripsi TLS/SSL untuk melindungi data yang dikirim melalui jaringan.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
**Jawab**
Menurut saya, sinkronisasi data adalah salah satu tantangan atau masalah saat menggunakan bidirectional streaming. Menjaga agar data tetap sinkron antara klien dan server adalah sebuah tantangan khususnya dalam situasi *real-time* seperti aplikasi chat. Misalnya, memastikan bahwa pesan diterima dan ditampilkan dalam urutan yang benar.

4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?
**Jawab**
**Kelebihan**:
* Menyediakan *interface* yang sederhana dan mudah digunakan untuk mengonversi `tokio::sync::mpsc::Receiver` menjadi `Stream`. 
* Fleksibel dalam menghasilkan data streaming.
**Kekurangan**:
* Bukan solusi yang paling efisien dari segi kinerja. Penggunaan MPSC dapat menyebabkan overhead komunikasi tambahan, terutama jika data streaming yang dihasilkan sangat besar.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
**Jawab**
Beberapa praktik yang dapat diterapkan:
* Menyusun kode ke dalam modul-modul yang terpisah sesuai dengan fungsionalitas terkait.
* Memisahkan kode ke dalam fungsi-fungsi yang berfokus pada satu tugas atau tujuan tertentu.
* Menulis unit test untuk setiap komponen penting untuk memastikan bahwa setiap bagian dari kode berfungsi sebagaimana mestinya sehingga dapat mempermudah *maintainability*.

6. In the `MyPaymentService` implementation, what additional steps might be necessary to handle more complex payment processing logic?
**Jawab**
Menambahkan lapisan keamanan untuk melindungi data transaksi seperti penggunaan HTTPS untuk enkripsi data, validasi atas data yang masuk untuk memastikan integritas dan keamanan transaksi. 

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
**Jawab**
gRPC menggunakan Protobuf (Protocol Buffers) sebagai format pertukaran data. Hal ini memfasilitasi interoperabilitas antara aplikasi yang berjalan pada berbagai bahasa pemrograman dan platform, karena Protobuf mendukung banyak bahasa seperti Java, C++, Python, dan Go. gRPC juga dirancang untuk performa tinggi. Dengan menggunakan HTTP/2, gRPC memungkinkan pengiriman pesan secara asinkron. Fitur ini sangat berguna untuk layanan yang memerlukan komunikasi real-time dan dapat menangani beban tinggi dengan lebih efisien daripada protokol berbasis HTTP/1.1.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
**Jawab**
**Kelebihan**:
* Mendukung multiplexing, yang memungkinkan klien dan server untuk saling bertukar beberapa permintaan dan respons melalui satu koneksi TCP tunggal secara bersamaan sehingga dapat mengurangi overhead koneksi dan meningkatkan efisiensi.
* Mendukung header compression yang mengurangi ukuran header permintaan dan respons, sehingga dapat mengurangi penggunaan bandwidth dan meningkatkan kinerja.
* Mendukung server push, yang memungkinkan server untuk secara proaktif mengirimkan data kepada klien yang mungkin diperlukan, sehingga mengurangi latensi dan meningkatkan kecepatan pemuatan halaman.
**Kekurangan**:
* Memiliki spesifikasi yang lebih kompleks dibandingkan dengan HTTP/1.x, yang dapat membuat implementasinya lebih sulit.
* Caching di HTTP/2 menjadi lebih kompleks karena perubahan header dan multiplexing. Hal ini dapat menyebabkan tantangan dalam manajemen cache dan penggunaan memori.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
**Jawab**
Perbedaan antara model *request-response* REST API dan bidirectional streaming gRPC terletak pada sifat komunikasi dan respons *real-time*. REST API beroperasi dalam model *request-response stateless* dan tidak secara alami mendukung komunikasi *real-time*, REST API memerlukan klien untuk membuat permintaan berulang-ulang untuk mendapatkan pembaruan. Sementara itu, gRPC memungkinkan streaming dua arah, memungkinkan klien dan server untuk saling bertukar data secara simultan, yang sangat cocok untuk aplikasi yang memerlukan respons real-time dan komunikasi asinkron, seperti aplikasi chat atau streaming data.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
**Jawab**
Protocol Buffer menggunakan format biner yang lebih ringan dan efisien dibandingkan JSON, yang menghasilkan ukuran payload yang lebih kecil dan mempercepat proses serialisasi dan deserialisasi. Hal ini mengurangi penggunaan bandwidth dan meningkatkan kinerja jaringan, khususnya untuk aplikasi dengan lalu lintas tinggi atau besar. Di sisi lain, JSON memiliki overhead yang lebih besar karena representasi teksnya, yang dapat mempengaruhi kinerja dan skalabilitas aplikasi terutama dalam kasus volume data besar. Protocol Buffer juga cocok untuk aplikasi dengan struktur data yang tetap, Hal ini cocok untuk aplikasi yang memerlukan konsistensi dalam representasi data.