# :globe_with_meridians: Praktikum 3 : Integrasi MongoDB dan Express
Dibawah ini merupakan langkah pengerjaan praktikum serta hasil screenshot pengerjaan Praktikum Modul 3 saya mengenai Integrasi MongoDB dan Express.

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
    1. Melakukan pembuatan direktori routes di tingkat yang sama dengan index.js
    2. Membuat file book.route.js di dalamnya
    3. Menambahkan baris kode berikut untuk fungsi getAllBooks
        ```
        const router = require('express').Router();
        
        router.get('/', function getAllBooks(req, res) {
            res.status(200).json({
                message: 'mendapatkan semua buku'
            })
        })

        module.exports = router;
        ```
        ![Membuat book.route.js dan memasukkan baris kode](../assets/praktikum3/10-Membuat%20file%20book.route.png)
    4. Melakukan hal yang sama untuk getOneBook, createBook, updateBook, dan deleteBook
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
        ![Melakukan hal yang sama untuk getOneBook, createBook, updateBook, dan deleteBook](../assets/praktikum3/11.-Lakukan%20hal%20yang%20sama%20getOneBook%20.....png)
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
        ![Import book.route.js](../assets/praktikum3/12-Import%20book.route%20pada%20index.png)
    6. Uji salah satu endpoint dengan Postman <br/>
        ![Uji di postman](../assets/praktikum3/13-Uji%20menggunakan%20postman.png)

* ### Pembuatan Controller
    1. Melakukan pembuatan direktori controllers di tingkat yang sama dengan index.js 
    2. Membuat file book.controller.js di dalamnya
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
        ![Membuat controller](../assets/praktikum3/14-Membuat%20book.controller.png)
    4. Melakukan hal yang sama untuk getOneBook, createBook, updateBook, dan deleteBook
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
        ![Menambahkan kode diatas](../assets/praktikum3/15.-Melakukan%20hal%20yang%20sama%20getOneBook.png)
    5. Melakukan import book.controller.js pada file book.route.js
        ```
        const router = require('express').Router();
        const book = require('../controllers/book.controller'); //
        
        ...
        
        module.exports = router;
        ```
        ![Import book.controller.js](../assets/praktikum3/16-Import%20book.controller%20ke%20book.route.png)
    6. Melakukan perubahan pada fungsi agar dapat memanggil fungsi dari book.controller.js
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
        ![Merubah fungsi](../assets/praktikum3/17-Merubah%20fungsi.png)
    7. Melakukan pengujian kembali, memastikan response tetap sama.<br/>
        ![Melakukan pengujian kembali](../assets/praktikum3/18-Pengujian%20kembali%20postman.png)

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
        ![Membuat model](../assets/praktikum3/19-Membuat%20model.png)

* ### Operasi CRUD
    1. Hapus semua data pada collection books. <br/>
        ![Menghapus data collection books](../assets/praktikum3/A-1-Menghapus%20data%20collection%20books.png)
    2. Melakukan import book.model.js pada file book.controller.js
        ```
        const Book = require('../models/book.model');
        
        ...
        ```
        ![Melakukan import book.model.js](../assets/praktikum3/A-2-Melakukan%20import%20book.model.png)
    3. Melakukan perubahan pada fungsi createBook
        ```
        const Book = require('../models/book.model');
        
        ...
        
        async function createBook(req, res) {
            const book = new Book({
                title: req.body.title,
                author: req.body.author,
                year: req.body.year,
                pages: req.body.pages,
                summary: req.body.summary,
                publisher: req.body.publisher,
            })

            try {
                const savedBook = await book.save();
                res.status(200).json({
                    message: 'membuat buku baru',
                    book: savedBook,
                })
            } catch (error) {
                res.status(500).json({
                    message: 'kesalahan pada server',
                    error: error.message,
                })
            }
        }
        ...
        ```
        ![Melakukan perubahan pada fungsi createBook](../assets/praktikum3/A-3-Melakukan%20perubahan%20createBook.png)
    4. Membuat dua buah buku dengan data di bawah ini dengan Postman
        ```
        {
            "title": "Dilan 1990",
            "author": "Pidi Baiq",
            "year": 2014,
            "pages": 332,
            "summary": "Mirea, anata wa utsukushī",
            "publisher": "Pastel Books"
        }
        ```
        ```
        {
            "title": "Dilan 1991",
            "author": "Pidi Baiq",
            "year": 2015,
            "pages": 344,
            "summary": "Watashi ga kare o aishite iru to ittara",
            "publisher": "Pastel Books"
        }
        ```
        ![Membuat buku 1990](../assets/praktikum3/A-4-Membuat%20buku%201990.png)
        ![Membuat buku 1991](../assets/praktikum3/A-5-Membuat%20buku%201991.png)
    5. Melakukan perubahan pada fungsi getAllBooks
        ```
        const Book = require('../models/book.model');
        
        async function getAllBooks(req, res) {
            try {
                const books = await Book.find();
                res.status(200).json({
                    message: 'mendapatkan semua buku',
                    books,
                })
            } catch (error) {
                res.status(500).json({
                    message: 'kesalahan pada server',
                    error: error.message,
                })
            }
        }
        ...
        ```
        ![Melakukan perubahan pada fungsi getAllBooks](../assets/praktikum3/A-6-Melakukan%20perubahan%20getAllBook.png)
    6. Melakukan perubahan pada fungsi getOneBook
        ```
        const Book = require('../models/book.model');
        
        ...
        
        async function getOneBook(req, res) {
            const id = req.params.id;
            try {
                const book = await Book.findById(id);
                res.status(200).json({
                    message: 'mendapatkan satu buku',
                    book,
                })
            } catch (error) {
                res.status(500).json({
                    message: 'kesalahan pada server',
                    error: error.message,
                })
            }
        }

        ...
        ```
        ![Melakukan perubahan pada fungsi getOneBook](../assets/praktikum3/A-7-Melakukan%20perubahan%20getOneBook.png)
    7. Menampilkan semua buku dengan Postman. <br/>
        ![Menampilkan semua buku dengan Postman.](../assets/praktikum3/A-8-Menampilkan%20semua%20buku.png)
    8. Menampilkan buku Dilan 1990 dengan Postman. <br/>
        ![Menampilkan buku Dilan 1990 dengan Postman.](../assets/praktikum3/A-9-Menampilkan%20dilan%201990.png)
    9. Melakukan perubahan pada fungsi updateBook
        ```
        const Book = require('../models/book.model');
        
        ...
        
        async function updateBook(req, res) {
            const id = req.params.id;
            try {
                const book = await Book.findByIdAndUpdate(
                    id, req.body, { new: true }
                )
                res.status(200).json({
                    message: 'memperbaharui satu buku',
                    book,
                })
            } catch (error) {
                res.status(500).json({
                    message: 'kesalahan pada server',
                    error: error.message,
                })
            }
        }

        ...
        ```
        ![Melakukan perubahan pada fungsi updateBook](../assets/praktikum3/A-10-Melakukan%20perubahan%20updateBook.png)
    10. Mengubah judul buku Dilan 1991 menjadi “<NAMA PANGGILAN> 1991” dengan Postman. <br/>
        ![Mengubah judul buku Dilan 1991 menjadi “<NAMA PANGGILAN> 1991”](../assets/praktikum3/A-11-Ubah%20judul.png)
    11. Melakukan perubahan pada fungsi deleteBook
        ```
        const Book = require('../models/book.model');
        
        ...
        
        async function deleteBook(req, res) {
            const id = req.params.id;
            try {
                const book = await Book.findByIdAndDelete(id);
                res.status(200).json({
                    message: 'menghapus satu buku',
                    book,
                })
            } catch (error) {
                res.status(500).json({
                    message: 'kesalahan pada server',
                    error: error.message,
                })
            }
        }
        ...
        ```
        ![Melakukan perubahan pada fungsi deleteBook](../assets/praktikum3/A-12-Melakukan%20perubahan%20pada%20deleteBook.png)
    12. Menghapus buku Dilan 1990 dengan Postman. <br/>
        ![Menghapus buku Dilan 1990 dengan Postman.](../assets/praktikum3/A-13-Menghapus%20dilan%201990.png)
   
    

