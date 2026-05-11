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