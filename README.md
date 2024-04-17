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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. Penggunaan RwLock<> dibanding Mutex<> disini lebih optimal karena dapat memungkinkan akses bersama untuk Read dan akses secara eksklusif untuk Write. Ini cocok untuk sistem notifikasi yang sudah ada karena akan ada banyak thread yang Read notifikasi secara bersamaan tanpa ada write, namun hanya ada satu thread yang Write notifikasi. Mutex<> disini tidak digunakan karena Mutex<> memungkinkan hanya satu thread untuk memiliki akses variabel dalam satu waktu. Untuk sistem notifikasi kita, penggunaan Mutex<> tidak akan memungkinkan karena pada proses ini akan ada banyak thread yang perlu Read suatu notifikasi.
2. Karena Rust memiliki aturan yang cukup ketat dalam mengelola variabel statis. Apabila variabel statis bersifat mutable, maka variabel tersebut dapat memunculkan masalah-masalah terkait keamanan dan juga berpotensi merusak thread safety. Oleh karena itu, digunakanlah lazy_static yang juga berfungsi untuk membuat variabelnya menjadi Singleton, hanya ada satu di programnya.

#### Reflection Subscriber-2
1. Saya belum sempat terlalu banyak melakukan eksplorasi. Kalau boleh jujur, alasannya adalah karena kurangnya waktu akibat mengerjakan tutorial ini terlalu dekat dengan deadline. Akan tetapi, saya tertarik untuk melakukan eksplorasi karena menurut saya konsep Observer pattern ini berharga untuk dipelajari lebih lanjut dan suatu saat bisa saja _come in handy_ apabila ada proyek-proyek yang bisa mengimplementasikan fitur ini.
2. Pada kasus BambangShop ini, Observer pattern bisa mempermudah penambahan observer baru, baik jenis yang sama maupun jenis yang berbeda karena program sudah ditulis dengan menerapkan prinsip Open-Closed. Hanya saja untuk jenis observer tambahan, mungkin akan diperlukan adanya trait untuk lebih mengaplikasikan prinsip Open-Closed. Dengan Observer pattern pada kasus ini, setiap subscriber saling terpisahkan satu sama lain dan tidak bergantung pada implementasi di source, dan perubahan state subscribe atau tidak juga bisa dilakukan tanpa perlu mengubah kode. Pada kasus lebih dari satu instance dari Main app, penambahan subscriber juga masih akan memungkinkan, yang penting penambahan subscriber dilakukan dengan mengarahkan ke API yang tepat.
3. Saya sudah mencoba membuat beberapa test tambahan pada Postman collection yang telah diberikan. Hal ini membantu saya untuk melakukan eksplorasi dan lebih melihat behavior sistem secara menyeluruh untuk memastikan API yang saya kembangkan sudah berfungsi dengan baik dan semua kasus yang mungkin terjadi juga sudah di-handle. Proses ini dapat membantu saya membiasakan diri untuk melakukan tes pada Postman untuk menguji API yang telah dibuat dan interaksinya antar service.
