# Publisher - Event Driven Architecture

## Understanding Publisher and Message Broker

### a. How much data will the publisher send in one run?

Publisher akan mengirimkan 5 event dalam satu kali run. Setiap event berisi
data berupa objek UserCreatedEventMessage yang memiliki dua field yaitu
user_id (berupa angka 1-5) dan user_name (berupa string nama). Jadi total
ada 5 pesan yang dikirimkan ke message broker RabbitMQ dalam satu kali
eksekusi program publisher.

### b. The URL amqp://guest:guest@localhost:5672 is the same as subscriber, what does it mean?

URL yang sama berarti publisher dan subscriber terhubung ke message broker
(RabbitMQ) yang sama. Publisher mengirimkan event ke RabbitMQ di alamat
tersebut, dan subscriber mendengarkan/mengkonsumsi event dari RabbitMQ di
alamat yang sama. Inilah cara keduanya berkomunikasi secara tidak langsung,
yaitu melalui perantara message broker yang sama.


## Running RabbitMQ as Message Broker

RabbitMQ berhasil dijalankan menggunakan Docker dengan perintah:
`docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management`

![RabbitMQ Dashboard](assets/Screenshot%202026-05-11%20110904.png)


## Sending and Processing Event

Ketika publisher dijalankan (`cargo run`), publisher mengirimkan 5 event 
ke message broker RabbitMQ. Event-event tersebut kemudian dikonsumsi dan 
diproses oleh subscriber. Subscriber mencetak setiap pesan yang diterima 
ke console seperti terlihat pada screenshot berikut.

![Sending Event](assets/Screenshot%202026-05-11%20122448.png)