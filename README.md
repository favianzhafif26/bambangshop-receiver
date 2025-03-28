# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [V] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [V] Commit: `Create Notification model struct.` 
    -   [V] Commit: `Create SubscriberRequest model struct.`
    -   [V] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [V] Commit: `Implement add function in Notification repository.`
    -   [V] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [V] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [V] Commit: `Create Notification service struct skeleton.`
    -   [V] Commit: `Implement subscribe function in Notification service.`
    -   [V] Commit: `Implement subscribe function in Notification controller.`
    -   [V] Commit: `Implement unsubscribe function in Notification service.`
    -   [V] Commit: `Implement unsubscribe function in Notification controller.`
    -   [V] Commit: `Implement receive_notification function in Notification service.`
    -   [V] Commit: `Implement receive function in Notification controller.`
    -   [V] Commit: `Implement list_messages function in Notification service.`
    -   [V] Commit: `Implement list function in Notification controller.`
    -   [V] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. Penggunaan `RwLock` dalam tutorial ini sangat penting karena struktur data `Vec` diakses oleh banyak thread secara bersamaan, baik untuk membaca maupun menulis. Dengan `RwLock`, beberapa thread dapat membaca data secara paralel tanpa saling mengganggu, selama tidak ada thread yang sedang melakukan penulisan. Pendekatan ini sangat efisien dalam kasus ini, karena sebagian besar operasi, seperti menampilkan daftar notifikasi, hanya memerlukan akses baca.  

Di sisi lain, `Mutex` hanya mengizinkan satu thread untuk mengakses data pada satu waktu, baik untuk membaca maupun menulis. Jika menggunakan `Mutex`, bahkan operasi membaca sederhana pun harus menunggu giliran, yang dapat memperlambat sistem secara signifikan, terutama saat banyak pembacaan terjadi secara bersamaan.

2. Rust tidak mengizinkan mutasi langsung terhadap variabel `static` seperti yang dilakukan di Java, karena hal ini dapat menyebabkan race condition dan akses data yang tidak aman secara bawaan. Untuk memastikan keamanan dan konsistensi data dalam lingkungan multithreaded, Rust mewajibkan penggunaan mekanisme sinkronisasi seperti `RwLock` atau `Mutex`. Selain itu, variabel global yang dapat diubah harus didefinisikan menggunakan bantuan `crate` seperti `lazy_static`, sehingga akses terhadap data tetap terkontrol dan aman.

#### Reflection Subscriber-2

1. Ya, saya sudah mengeksplorasi `src/main.rs`. File ini bertanggung jawab untuk menjalankan aplikasi menggunakan framework `Rocket`. Di dalamnya, environment variable dimuat menggunakan `dotenv()`, dan aplikasi dikonfigurasi serta dibangun dengan `rocket::build()`, yang mencakup pengaturan tambahan seperti `HTTP client` dan `route handler`.  

Selain itu, fungsi `route_stage()` dari modul controller digunakan untuk mendaftarkan semua endpoint yang akan dilayani. File ini juga mendeklarasikan modul-modul penting seperti controller, service, repository, dan model, yang membentuk struktur arsitektur aplikasi secara keseluruhan.

2. Observer pattern sangat bermanfaat dalam menambahkan lebih banyak subscriber, karena setiap subscriber hanya perlu melakukan proses subscribe ke `Main app` tanpa harus mengubah kode di sisi publisher. Dengan mekanisme ini, notifikasi akan secara otomatis dikirim ke semua subscriber yang sudah terdaftar, menjadikan sistem lebih fleksibel dan scalable. Subscriber baru dapat ditambahkan kapan saja tanpa memengaruhi komponen lain dalam sistem, sehingga memungkinkan ekspansi yang lebih mudah.  

Namun, jika ingin menambahkan lebih dari satu instance `Main app (publisher)`, kompleksitas sistem akan meningkat. Dalam sistem dengan banyak publisher, tidak ada jaminan bahwa semua subscriber terhubung ke publisher yang sesuai, karena setiap publisher hanya mengetahui subscriber yang mendaftar langsung kepadanya. Oleh karena itu, pendekatan tambahan diperlukan untuk memastikan bahwa semua notifikasi tersampaikan dengan benar ke subscriber yang sesuai, misalnya dengan menerapkan mekanisme `event broker` atau `message queue` untuk mengelola distribusi notifikasi secara lebih terorganisir.

3. Saya telah menambahkan dokumentasi pada koleksi Postman, khususnya di bagian deskripsi setiap request. Fitur ini sangat berguna dalam proyek kelompok, karena membantu semua anggota tim memahami bagaimana setiap API bekerja tanpa harus membaca kode secara langsung. Dengan dokumentasi yang jelas, kolaborasi menjadi lebih efektif, mengurangi miskomunikasi, dan mempercepat proses pengembangan serta debugging.