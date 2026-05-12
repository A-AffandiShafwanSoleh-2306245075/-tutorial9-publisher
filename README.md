# Publisher - Event Driven Architecture

## Understanding Publisher and Message Broker

### a. How much data will the publisher send in one run?

Publisher akan mengirimkan 5 event dalam satu kali run. Setiap event berisi
data berupa objek UserCreatedEventMessage yang memiliki dua field yaitu
user_id (berupa angka 1-5) dan user_name (berupa string nama yang diawali
dengan NPM). Jadi total ada 5 pesan yang dikirimkan ke message broker
RabbitMQ dalam satu kali eksekusi program publisher. Setiap pesan
diproses secara terpisah oleh subscriber yang sedang mendengarkan queue
"user_created". Data yang dikirim bersifat serial menggunakan format borsh
sehingga dapat diterima dan di-deserialize oleh subscriber dengan benar.

### b. The URL amqp://guest:guest@localhost:5672 is the same as subscriber, what does it mean?

URL yang sama berarti publisher dan subscriber terhubung ke message broker
(RabbitMQ) yang sama, yaitu yang berjalan di localhost port 5672. Publisher
mengirimkan event ke RabbitMQ di alamat tersebut, dan subscriber
mendengarkan/mengkonsumsi event dari RabbitMQ di alamat yang sama.
Inilah cara keduanya berkomunikasi secara tidak langsung, yaitu melalui
perantara message broker yang sama tanpa harus saling mengenal satu sama
lain. Konsep ini disebut decoupling, dimana publisher tidak perlu tahu
siapa yang akan memproses pesannya, dan subscriber tidak perlu tahu siapa
yang mengirim pesan. Hal ini membuat sistem lebih fleksibel dan mudah
dikembangkan karena kita bisa menambah subscriber baru tanpa mengubah
kode publisher sama sekali.

## Running RabbitMQ as Message Broker

RabbitMQ berhasil dijalankan menggunakan Docker dengan perintah:
`docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management`

RabbitMQ berfungsi sebagai message broker yang menerima pesan dari publisher
dan meneruskannya ke subscriber. Port 5672 digunakan untuk koneksi AMQP
(protokol komunikasi antar aplikasi), sedangkan port 15672 digunakan untuk
mengakses dashboard management RabbitMQ melalui browser. Dashboard ini
memungkinkan kita memantau jumlah pesan yang antri, kecepatan pengiriman
pesan, dan jumlah koneksi yang aktif secara real-time.

![RabbitMQ Dashboard](assets/Screenshot%202026-05-11%20110904.png)

## Sending and Processing Event

Ketika publisher dijalankan (`cargo run`), publisher mengirimkan 5 event
sekaligus ke message broker RabbitMQ melalui queue bernama "user_created".
Event-event tersebut kemudian dikonsumsi dan diproses oleh subscriber yang
sedang berjalan dan mendengarkan queue yang sama. Subscriber mencetak setiap
pesan yang diterima ke console, menampilkan user_id dan user_name dari setiap
event. Proses ini menunjukkan bagaimana event-driven architecture bekerja,
dimana producer dan consumer tidak langsung berkomunikasi melainkan melalui
perantara message broker. Hal ini membuat sistem lebih responsif karena
publisher tidak perlu menunggu subscriber selesai memproses pesan sebelum
mengirim pesan berikutnya.

![Sending Event](assets/Screenshot%202026-05-11%20122448.png)

## Monitoring Chart Based on Publisher

Setiap kali publisher dijalankan, terjadi spike (lonjakan) pada chart
message rates di RabbitMQ. Hal ini terjadi karena publisher mengirimkan
5 pesan sekaligus ke message broker dalam waktu yang sangat singkat,
sehingga terjadi peningkatan aktivitas pengiriman pesan yang terlihat
sebagai spike pada grafik. Setelah publisher selesai mengirim pesan,
grafik kembali turun karena tidak ada pesan baru yang dikirim. Spike
ini menunjukkan bahwa RabbitMQ berhasil mendeteksi dan mencatat aktivitas
pengiriman pesan secara real-time. Semakin sering publisher dijalankan
dalam waktu berdekatan, semakin banyak spike yang terlihat pada grafik,
yang mencerminkan tingginya throughput pesan dalam sistem.

![Monitoring Chart](assets/Screenshot%202026-05-11%20122938.png)