# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Pada kasus BambangShop, sebuah trait untuk Observer tidak terlalu diperlukan karena tidak ada variasi perilaku yang diharapkan dari observer. Sebagai alternatif, sebuah struct Model yang mencakup data dan perilaku yang diperlukan sudah cukup memadai.
2. Agar keunikan ID atau URL dari customer terjaga, maka menggunakan Vec (list) sudah cukup. Namun, DashMap (map/dictionary) lebih disarankan karena keunggulan dalam kecepatan akses dan manipulasi data, serta kemudahan dalam pengecekan keunikan ID atau URL.
3. DashMap yang digunakan untuk variabel statis SUBSCRIBERS adalah pilihan yang tepat karena memungkinkan akses ke data yang sama dari mana pun dalam aplikasi dan menjaga keamanan konkurensi. Implementasi Singleton pattern mungkin memungkinkan, tetapi tidak mutlak diperlukan karena DashMap sudah menyediakan fungsionalitas yang diperlukan untuk pengelolaan keamanan konkurensi. Jadi, DashMap adalah pilihan yang tepat untuk SUBSCRIBERS dalam BambangShop.

#### Reflection Publisher-2

1. Pada prinsip Single Responsibility Principle menekankan kebutuhan akan class yang fokus pada satu tanggung jawab. Oleh karena itu, pemisahan "Service" dan "Repository" dari Model diperlukan agar Model hanya fokus pada struktur data. Ini memastikan Model hanya bertanggung jawab atas representasi data, sesuai dengan prinsip SRP.
2. Jika hanya menggunakan Model tanpa memisahkan tanggung jawabnya, akan mengakibatkan model harus menangani representasi data dan business logic. Hal ini dapat meningkatkan kompleksitas kode, sulit dalam perawatan, dan membuat interaksi antar-model menjadi rumit dan terikat. Misalnya, jika sebuah Product harus memberi tahu Subscriber tentang perubahan, maka akan terjadi interaksi langsung antara keduanya tanpa pemisahan yang jelas.
3. Postman sangat berguna dalam pengujian program dengan kemampuannya untuk mengirim respons ke endpoint API. Fitur pengiriman permintaan HTTP, pengujian otomatis, dan pengelolaan environment variables sangat membantu dalam memverifikasi perilaku endpoint API, menguji berbagai kondisi, dan memudahkan penyesuaian antar lingkungan seperti development, staging, dan production saat menguji API.


#### Reflection Publisher-3

1. Pada tutorial ini, kita menggunakan variasi model Push dari Observer Pattern. Hal ini terlihat dari  request HTTP POST untuk mengirimkan notifikasi kepada pelanggan yang telah berlangganan. Notifikasi ini dipicu oleh create, promote, atau delete produk.
2. Apabila kita menggunakan variasi Pull dari Observer Pattern, Salah satu kelebihannya adalah mengurangi penggunaan sumber daya jaringan dan komputasi karena pelanggan hanya mengambil data saat diperlukan. Mereka juga memiliki kendali penuh atas timing pengambilan data. Namun, kekurangannya adalah pelanggan harus secara aktif meminta pembaruan, yang dapat menyebabkan keterlambatan dalam mendapatkan informasi terbaru. Implementasi Model Pull juga membutuhkan penambahan logika di sisi pelanggan untuk mengelola permintaan dan pembaruan data.
3. Apabila kita memilih untuk tidak menggunakan multi-threading dalam proses notifikasi, proses akan dilakukan secara berurutan dengan setiap notifikasi diproses satu per satu. Ini berarti kita harus menunggu hingga notifikasi sebelumnya selesai sebelum memproses yang berikutnya. Jika ada banyak notifikasi yang harus dikirim, ini dapat mengakibatkan keterlambatan dan delay dalam respons. Penggunaan multi-threading dapat mempercepat proses notifikasi dengan memprosesnya secara paralel, meningkatkan responsivitas aplikasi, dan lebih efisien ketika ingin mengirim banyak notifikasi.
