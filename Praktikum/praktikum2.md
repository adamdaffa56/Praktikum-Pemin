# :globe_with_meridians: Praktikum 2 : CRUD MongoDB
Dibawah ini merupakan langkah pengerjaan praktikum serta hasil screenshot pengerjaan Praktikum Modul 2 saya mengenai CRUD MongoDB.

## :scroll: Dasar Teori
* ### MongoDB Compass
    MongoDB Compass adalah tool berbasis Graphical User Interface (GUI) yang digunakan untuk berinteraksi dengan MongoDB yang terpasang secara on-premise dan MongoDB Atlas yang berbasis cloud. Tool ini dapat melakukan aktivitas dasar seperti CREATE, READ, UPDATE, dan DELETE (CRUD) tanpa berhadapan dengan baris perintah (command line)
* ### MongoDB Shell 
    Walaupun dapat melakukan operasi seperti MongoDB Compass, interaksi yang dilakukan MongoDB Shell berbasis Command Line Interface (CLI) sehingga diperlukan baris perintah untuk melakukan aktivitas dasar. MongoDB Shell dapat diakses langsung dari MongoDB Compass atau menggunakan mongosh pada Command Prompt

## :footprints: Langkah Percobaan
* ### MongoDB Compass
    1. Melakukan koneksi ke MongoDB menggunakan connection string, disini saya melakukan secara local menggunakan ``` mongodb://localhost:27017 ```. <br/>
    ![Koneksi MongoDB secara local](../assets/praktikum2/A_1_Koneksi%20MongoDB.png)
    2. Membuat database dengan melakukan klik “Create Database”. <br/>
    ![Create Database "bookstore"](../assets/praktikum2/A_2_Membuat%20Database.png)
    3. Melakukan insert buku pertama dengan melakukan klik “Add Data”, pilih “Insert Document”, isi dengan data yang diinginkan dan klik “Insert”. <br/>
    ![Insert data buku pertama](../assets/praktikum2/A_3_Insert%20Buku%20Pertama.png)
    4. Melakukan insert buku kedua dengan cara yang sama. <br/>
    ![Insert data buku kedua](../assets/praktikum2/A_4_Insert%20Buku%20Kedua.png)
    5. Melakukan pencarian buku dengan author “Osamu Dazai” dengan mengisi filter yang diinginkan dan klik “Find”. <br/>
    ![Pencarian buku dengan author "Osamu Dazai"](../assets/praktikum2/A_5_Pencarian%20Buku%20Osamu%20Dazai.png)
    6. Melakukan perubahan summary pada buku “No Longer Human” menjadi “Buku yang bagus (<NAMA>,<NIM>) dengan melakukan klik “Edit Document” (berlambang pensil), mengisi nilai summary yang baru, dan melakukan klik “Update”. <br/>
    ![Melakukan perubahan summary pada buku "No Longer Human"](../assets/praktikum2/A_6_Perubahan%20Summary%20No%20Longer%20Human.png)
    7. Menghapus buku “I Am a Cat” dengan melakukan klik “Remove Document” (berlambang tong sampah) dan melakukan klik “Delete”. <br/>
    ![Menghapus buku "I Am a Cat"](../assets/praktikum2/A_7_Delete%20I%20Am%20a%20Cat.png)

* ### MongoDB Shell
    1. Melakukan koneksi ke MongoDB Server dengan menjalankan command ```mongosh```.
    2. Mencoba melihat list database yang ada di server dengan menjalankan command ```show dbs```. Serta mencoba menjalankan beberapa command seperti gambar dibawah.
    3. Melakukan insert buku “Overlord I” dengan menggunakan command ```db.books.insertOne(<data kalian>)```.
    4. Melakukan insert buku “The Setting Sun” dan “Hujan” dengan insert many dengan menggunakan command ```db.books.insertMany(<data kalian>)```.
    5. Melakukan pencarian buku dengan menggunakan command ```db.books.find()``` untuk melakukan pencarian semua buku.
    6. Menampilkan seluruh buku dengan author “Osamu Dazai” dengan mengisi argument pada find() dengan menggunakan command ```db.books.find({<filter yang ingin diisi>})```.
    7. Merubah summary pada buku “Hujan” menjadi “Buku yang bagus (<NAMA>,<NIM>) dengan mengunakan command ```db.books.updateOne({<filter>}, {$set: {<data yang akan di update>}})```.
    8. Merubah publisher menjadi “Yen Press” pada semua buku “Osamu Dazai” dengan menggunakan command ```db.books.updateMany({<filter>}, {$set: {<data yang akan di update>}})```.
    9. Melakukan penghapusan pada buku “Overlord I” dengan menggunakan command ```db.books.deleteOne({<argument>})```.
    10. Melakukan penghapusan pada semua buku “Osamu Dazai dengan menggunakan command ```db.books.deleteMany({<argument>})```.

