# :globe_with_meridians: Praktikum 3 : Integrasi MongoDB dan Express
Dibawah ini merupakan langkah pengerjaan praktikum serta hasil screenshot pengerjaan Praktikum Modul 3 saya mengenai CRUD MongoDB.

## :scroll: Dasar Teori
* ### Express
    Express.js adalah framework web app untuk Node.js yang ditulis dengan bahasa pemrograman JavaScript. Framework open source ini dibuat oleh TJ Holowaychuk pada tahun 2010 lalu. <br/>
    Express.js adalah framework back end. Artinya, ia bertanggung jawab untuk mengatur fungsionalitas website, seperti pengelolaan routing dan session, permintaan HTTP, penanganan error, serta pertukaran data di server.
* ### Mongoose 
    Mongoose adalah pustaka berbasis Node.js yang digunakan untuk pemodelan data pada MongoDB. Mongoose menyediakan feature diantaranya, model data application berbasis Schema. Dan juga termasuk built-in type casting, validation, query building, business logic hooks dan masih banyak lagi yang menjadi ke andalan mongoose.
* ### Async Await 
    Async sendiri merupakan sebuah fungsi yang mengembalikan sebuah Promise. Await sendiri merupakan fungsi yang hanya berjalan di dalam Async. Await bertujuan untuk menunda jalannya Async hingga proses dari Await tersebut berhasil dieksekusi.
* ### Model
    Model merupakan bagian yang bertugas untuk menyiapkan, mengatur, memanipulasi, dan mengorganisasikan data yang ada di database.

* ### Controller
    Controller merupakan bagian yang menjadi tempat berkumpulnya logika pemrograman yang digunakan untuk memisahkan organisasi data pada database. Dalam beberapa kasus, controller menjadi penghubung antara model dan view pada arsitektur MVC

* ### Route
    Router mengatur pintu masuk yang berupa request pada aplikasi, mereka memilah dan mengolah request url untuk kemudian diproses sesuai dengan tujuan akhir url tersebut. Bisa jadi url tersebut berfungsi untuk mengambil data kemudian menampilkannya, menghapus data, menampilkan form, sampai mengolah session.

## :footprints: Langkah Percobaan
* ### Percobaan instalasi NodeJS
    1. Menjalankan command ```node -v``` untuk memeriksa apakah NodeJS sudah terinstall. <br/>
    ![Memeriksa NodeJS](../assets/praktikum3/1-Memeriksa%20NodeJS.png)

* ### Inisiasi project Express dan pemasangan package
    1. Membuat folder dengan nama express-mongodb dan masuk ke dalam folder tersebut lalu buka melalui text editor masing-masing.
    2. Melakukan npm init untuk mengenerate file package.json dengan menggunakan command ```npm init -y```. <br/>
    ![Melakukan npm init](../assets/praktikum3/2-Npm%20init.png)
    3. Melakukan instalasi express, mongoose, dan dotenv dengan menggunakan command ```npm i express mongoose dotenv```. <br/>
    ![Instalasi express, mongoose, dan dotenv](../assets/praktikum3/3-Instalasi%20express,%20mongoose%20dan%20dotenv.png)

* ### Koneksi Express ke MongoDB
    1. Membuat file index.js pada root folder dan masukkan kode di bawah ini <br/>
        ```
        require('dotenv').config();
        const express = require('express');
        const mongoose = require('mongoose');

        const app = express();

        app.use(express.json());

        app.get('/', (req, res) => {
            res.status(200).json({
                message: '<nama>,<nim>'
            })
        })

        const PORT = 8000;
        app.listen(PORT, () => {
            console.log(`Running on port ${PORT}`);
        })
        ``` 
        ![Membuat file index.js](../assets/praktikum3/4-Membuat%20index.js.png)
        Setelah itu coba jalankan aplikasi dengan command ```node index.js```
        ![Menjalankan Node](../assets/praktikum3/5-Mencoba%20node.png)

    2. Melakukan pembuatan file .env dan masukkan baris berikut
        ```
        PORT=5000
        ```
        Setelah itu ubahlah kode pada listening port menjadi berikut dan coba jalankan aplikasi kembali
        ```
        ...

        const PORT = process.env.PORT || 8000;
        app.listen(PORT, () => {
            console.log(`Running on port ${PORT}`);
        })
        ```
        ![Membuat env](../assets/praktikum3/6-Membuat%20env.png)
    3. Copy connection string yang terdapat pada compas atau atlas dan paste kan pada .env seperti berikut
        ```
        MONGO_URI=<Connection string masing-masing>
        ```
        ![Connection String pada .env](../assets/praktikum3/7-Connection%20string%20env.png)
    4. Menambahkan baris kode berikut pada file index.js
        ```
        require('dotenv').config();
        const express = require('express');
        const mongoose = require('mongoose');

        mongoose.connect(process.env.MONGO_URI);
        const db = mongoose.connection;

        db.on('error', (error) => {
            console.log(error);
        });

        db.once('connected', () => {
            console.log('Mongo connected');
        })
        ...
        ```
        ![Menambahkan baris kode pada file index.js](../assets/praktikum3/8-Menambahkan%20kode%20di%20index%20.png)
        Setelah itu coba jalankan aplikasi kembali <br/>
        ![Coba jalankan node kembali](../assets/praktikum3/9-Mencoba%20node%20lagi.png)

* ### Pembuatan Routing
    1. Lakukan pembuatan direktori routes di tingkat yang sama dengan index.js
    2. Buatlah file book.route.js di dalamnya
    3. Tambahkan baris kode berikut untuk fungsi getAllBooks
        ```
        const router = require('express').Router();
        
        router.get('/', function getAllBooks(req, res) {
            res.status(200).json({
                message: 'mendapatkan semua buku'
            })
        })

        module.exports = router;
        ```
    4. Lakukan hal yang sama untuk getOneBook, createBook, updateBook, dan deleteBook
        ```
        const router = require('express').Router();
        
        ...
        
        router.get('/:id', function getOneBook(req, res) {
            const id = req.params.id;
            res.status(200).json({
                message: 'mendapatkan satu buku',
                id,
            })
        })

        router.post('/', function createBook(req, res) {
            res.status(200).json({
                message: 'membuat buku baru'
            })
        })

        router.put('/:id', function updateBook(req, res) {
            const id = req.params.id;
            res.status(200).json({
                message: 'memperbaharui satu buku',
                id,
            })
        })

        router.delete('/:id', function deleteBook(req, res) {
            const id = req.params.id;
            res.status(200).json({
                message: 'menghapus satu buku',
                id,
            })
        })

        module.exports = router;
        ```
    5. Lakukan import book.route.js pada file index.js dan tambahkan baris kode berikut
        ```
        require('dotenv').config();
        const express = require('express');
        const mongoose = require('mongoose');
        
        const bookRoutes = require('./routes/book.route'); //
        
        ...

        app.get('/', (req, res) => {
            res.status(200).json({
                message: '<nama>,<nim>'
            })
        })
        
        app.use('/books', bookRoutes); //
        
        const PORT = process.env.PORT || 8000;
        app.listen(PORT, () => {
            console.log(`Running on port ${PORT}`);
        })
        ```
    6. Uji salah satu endpoint dengan Postman 

* ### Pembuatan Controller
    1. Lakukan pembuatan direktori controllers di tingkat yang sama dengan index.js
    2. Buatlah file book.controller.js di dalamnya
    3. Salin baris kode dari routes untuk fungsi getAllBooks
        ```
        function getAllBooks(req, res) {
            res.status(200).json({
                message: 'mendapatkan semua buku'
            })
        };

        module.exports = {
            getAllBooks,
        }
        ```  
    4. Lakukan hal yang sama untuk getOneBook, createBook, updateBook, dan deleteBook
        ```
        ...
        
        function getOneBook(req, res) {
            const id = req.params.id;
            res.status(200).json({
                message: 'mendapatkan satu buku',
                id,
            })
        }

        function createBook(req, res) {
            res.status(200).json({
                message: 'membuat buku baru'
            })
        }

        function updateBook(req, res) {
            const id = req.params.id;
            res.status(200).json({
                message: 'memperbaharui satu buku',
                id,
            })
        }

        function deleteBook(req, res) {
            const id = req.params.id;
            res.status(200).json({
                message: 'menghapus satu buku',
                id,
            })
        }

        module.exports = {
            getAllBooks,
            getOneBook, //
            createBook, //
            updateBook, //
            deleteBook //
        }
        ```
    5. Lakukan import book.controller.js pada file book.route.js
        ```
        const router = require('express').Router();
        const book = require('../controllers/book.controller'); //
        
        ...
        
        module.exports = router;
        ```
    6. Lakukan perubahan pada fungsi agar dapat memanggil fungsi dari book.controller.js
        ```
        const router = require('express').Router();
        const book = require('../controllers/book.controller');
        
        router.get('/', book.getAllBooks);

        router.get('/:id', book.getOneBook);
     
        router.post('/', book.createBook);
     
        router.put('/:id', book.updateBook);
     
        router.delete('/:id', book.deleteBook);
     
        module.exports = router;
        ```
    7. Lakukan pengujian kembali, pastikan response tetap sama

* ### Pembuatan Model
    Berikut adalah gambaran bentuk data dari modul sebelumnya <br/>
    |  |  |
    | ----------- | ----------- |
    | title | string |
    | author | string |
    | year | number |
    | pages | number |
    | summary | string |
    | publisher | string |

    1. Lakukan pembuatan direktori models di tingkat yang sama dengan index.js
    2. Buatlah file book.model.js di dalamnya
    3. Tambahkan baris kode berikut sesuai dengan tabel di atas
        ```
        const mongoose = require('mongoose');
        
        const bookSchema = new mongoose.Schema({
            title: {
                type: String
            },
            author: {
                type: String
            },
            year: {
                type: Number
            },
            pages: {
                type: Number
            },
            summary: {
                type: String
            },
            publisher: {
             type: String
            }
        })
        
        module.exports = mongoose.model('book', bookSchema);
        ```

* ### Operasi CRUD
    1. Hapus semua data pada collection books
   
    

