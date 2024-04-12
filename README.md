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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
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
    -   [v] Commit: `Implement list_messages function in Notification service.`
    -   [V] Commit: `Implement list function in Notification controller.`
    -   [V] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?<br/>

Dalam tutorial ini, penggunaan RwLock<> untuk mensinkronisasi penggunaan Vec dari notifikasi sangat penting karena memungkinkan akses konkuren yang lebih efisien ke data. Berikut penjelasan mengapa RwLock<> diperlukan dan alasan memilihnya dibandingkan Mutex<>:

Mengapa RwLock<> Diperlukan?
RwLock<> (Read-Write Lock) memungkinkan banyak pembaca mengakses data secara simultan, selama tidak ada penulis yang sedang menulis. Jika penulis ingin mengakses data, ia harus menunggu sampai tidak ada pembaca atau penulis lain yang aktif. Ini ideal untuk kasus di mana operasi baca jauh lebih sering terjadi daripada operasi tulis, seperti dalam kasus pengelolaan notifikasi di mana data mungkin sering dibaca (misalnya, untuk menampilkan notifikasi) tetapi tidak terlalu sering diubah atau ditambahkan.

Mengapa Tidak Menggunakan Mutex<>?
Mutex<> (Mutual Exclusion) memberikan akses eksklusif ke data bagi thread yang memegang kunci. Ini berarti jika satu thread sedang mengakses data, thread lain harus menunggu sampai thread pertama selesai, tanpa memandang apakah operasi tersebut adalah baca atau tulis. Ini bisa mengakibatkan kebuntuan atau pengurangan kinerja dalam skenario di mana banyak thread hanya ingin membaca data dan tidak ada konflik penulisan.

Perbandingan:
Efisiensi: RwLock<> lebih efisien dalam skenario dengan operasi baca yang dominan karena memungkinkan akses baca bersamaan. Mutex<>, di sisi lain, membatasi akses ke satu thread pada satu waktu, yang dapat menyebabkan pemborosan waktu menunggu yang tidak perlu dalam kasus baca-banyak.

Penggunaan Kasus: RwLock<> ideal untuk struktur data yang jarang diubah tetapi sering dibaca. Mutex<> lebih cocok untuk situasi di mana akses eksklusif ke data penting atau di mana operasi baca dan tulis terjadi dengan frekuensi yang hampir sama.

Dalam konteks tutorial untuk sistem notifikasi, di mana kemungkinan banyak thread akan sering membaca informasi notifikasi tanpa sering memodifikasinya, penggunaan RwLock<> memberikan keseimbangan yang baik antara keselamatan konkurensi dan kinerja, menjadikannya pilihan yang lebih baik dibandingkan Mutex<>.

2. In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?<br/>
##### Keamanan Memori

Rust sangat berfokus pada keamanan memori dan memastikan bahwa akses ke memori dilakukan dengan cara yang aman untuk mencegah _data races_, _dangling pointers_, dan masalah lain yang sering terjadi dalam pemrograman sistem. Mutasi global atau variabel statis secara langsung bisa menyebabkan kondisi balapan (_race conditions_) jika diakses oleh beberapa _thread_ secara bersamaan tanpa sinkronisasi yang tepat.

##### Keselamatan Konkurensi

Untuk menjamin keselamatan konkurensi, Rust membatasi cara variabel statis dimutasi. Rust mengharuskan semua akses ke variabel statis untuk dianggap tidak aman (`unsafe`) kecuali jika menggunakan tipe yang memastikan keselamatan konkurensi, seperti `Mutex` atau `RwLock`. Hal ini memaksa pengembang untuk secara eksplisit memikirkan tentang sinkronisasi dan keselamatan konkurensi saat mengakses dan memodifikasi data secara global.

##### Penggunaan `lazy_static`

Karena Rust tidak mengizinkan inisialisasi variabel statis yang tidak konstan pada waktu kompilasi, `lazy_static` digunakan untuk mendefinisikan variabel yang memerlukan inisialisasi dinamis atau yang tidak bisa ditentukan nilai awalnya pada waktu kompilasi. `lazy_static` menjamin bahwa inisialisasi dilakukan secara aman pada saat runtime saat variabel tersebut pertama kali diakses.

Dibandingkan dengan Java, di mana mutasi variabel statis lebih longgar dan tidak memerlukan pertimbangan ekstra tentang keselamatan konkurensi pada level bahasa, Rust mengambil pendekatan yang lebih konservatif untuk memastikan bahwa kode yang ditulis adalah aman secara konkurensi dan bebas dari kesalahan umum pemrograman sistem. Ini adalah bagian dari upaya Rust untuk menawarkan jaminan keamanan memori yang lebih kuat sambil tetap memungkinkan performa yang tinggi.

#### Reflection Subscriber-2
1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.<br/>

Dari kode yang disediakan dalam src/lib.rs, ada beberapa poin pembelajaran penting yang dapat diambil, terutama mengenai bagaimana Rust mengelola konfigurasi, variabel statis, dan penanganan kesalahan:

- Penggunaan lazy_static untuk Inisialisasi Variabel Statis:
Rust memerlukan bahwa variabel statis harus memiliki nilai yang diketahui pada waktu kompilasi kecuali jika menggunakan crate seperti lazy_static. Ini memungkinkan inisialisasi variabel statis yang membutuhkan pemrosesan atau tidak bisa ditentukan secara langsung pada waktu kompilasi, seperti klien HTTP REQWEST_CLIENT dan konfigurasi aplikasi APP_CONFIG.

- Pengelolaan Konfigurasi dengan dotenvy dan Figment:
Crate dotenvy digunakan untuk memuat variabel lingkungan dari file .env, memudahkan pengelolaan konfigurasi yang sensitif atau spesifik lingkungan tanpa harus mengkodkannya langsung dalam kode sumber.
Figment dari Rocket digunakan untuk menggabungkan berbagai sumber konfigurasi, seperti nilai default dan variabel lingkungan, yang memungkinkan fleksibilitas tinggi dalam pengelolaan konfigurasi aplikasi.

- Penggunaan Getters dari Crate getset:
Menggunakan macro #[getset(get = "pub with_prefix")] memudahkan pembuatan method getter untuk field-field dalam struct AppConfig, meningkatkan keamanan dan enkapsulasi data.

- Penanganan Kesalahan dengan Custom Error Types:
Definisi type Result<T, E = Error> menunjukkan customisasi dari type Result yang sering digunakan dalam Rust untuk penanganan kesalahan. Ini memudahkan penggunaan type error khusus aplikasi ini.
Error di sini didefinisikan sebagai Custom<Json<ErrorResponse>>, memberikan cara standarisasi untuk merespons kesalahan dalam format JSON yang konsisten di seluruh aplikasi.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?<br/>

Observer pattern memang dirancang untuk memudahkan proses penambahan atau pengurangan "subscribers" (penerima) dalam sistem, tanpa memerlukan perubahan pada "publisher" (pengirim). Ini adalah salah satu kelebihan utama dari pola desain ini, yang membuatnya sangat cocok untuk sistem di mana jumlah penerima bisa berubah-ubah seiring waktu, seperti dalam sistem notifikasi.

- Mempermudah Penambahan Subscriber
Dalam konteks sistem notifikasi yang kita bangun, penambahan subscriber baru sangat mudah. Setiap instance dari "Receiver" hanya perlu mendaftarkan diri menggunakan metode subscribe dan secara otomatis ditambahkan ke dalam daftar subscriber. Mereka akan langsung mulai menerima pembaruan dari "Main app" (publisher) tanpa memerlukan perubahan pada sisi publisher.

- Spawning Lebih dari Satu Instance dari Main App
Mengenai pertanyaan apakah akan tetap mudah menambahkan lebih dari satu instance dari "Main app", hal ini bergantung pada bagaimana sistem dirancang:

Jika semua instance dari "Main app" menggunakan mekanisme pusat untuk mengelola subscriber, seperti database terpusat atau layanan manajemen subscriber, maka penambahan instance "Main app" bisa dilakukan dengan relatif mudah. Semua instance bisa berbagi informasi tentang subscriber yang terdaftar dan berkoordinasi untuk mengirim notifikasi.

Namun, jika tiap instance dari "Main app" mengelola subscriber-nya sendiri secara independen, maka akan lebih rumit untuk memastikan bahwa semua subscriber menerima notifikasi yang konsisten. Hal ini bisa memerlukan implementasi mekanisme sinkronisasi atau replikasi informasi subscriber antar instance.

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).<br/>

Mengintegrasikan Postman untuk membuat tes atau meningkatkan dokumentasi koleksi bisa sangat membantu. Meskipun saya tidak melakukan ini secara langsung, saya bisa menjelaskan bagaimana fitur-fitur tersebut bermanfaat:

- Membuat Tes di Postman
Membuat tes di Postman memungkinkan kita untuk secara otomatis memverifikasi respon dari API sesuai dengan ekspektasi. Hal ini termasuk memeriksa status kode, format respon, dan keberadaan nilai tertentu dalam respon. Dalam konteks proyek kelompok atau tutorial, ini berarti kita bisa dengan cepat memastikan bahwa perubahan yang kita buat tidak memecah fungsi yang ada dan bahwa API berperilaku seperti yang diharapkan.

Keuntungan: Pengujian otomatis ini sangat berguna untuk memvalidasi kontrak API secara konsisten, yang membantu dalam pengembangan yang berkelanjutan dan penerapan CI/CD.

- Meningkatkan Dokumentasi di Postman
Dokumentasi yang baik adalah kunci untuk API yang bisa digunakan dan dipelihara dengan baik. Postman memungkinkan eksport dokumentasi koleksi yang bisa diakses oleh pengembang frontend, tester, atau pihak ketiga yang menggunakan API tersebut.

Keuntungan: Dokumentasi yang terjaga dan mudah diakses mempercepat proses integrasi dan meminimalisir kesalahpahaman tentang cara menggunakan API. Ini sangat penting dalam proyek kelompok di mana komunikasi yang efektif antar tim bisa sangat meningkatkan efisiensi pengembangan.