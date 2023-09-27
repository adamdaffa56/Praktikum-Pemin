# :globe_with_meridians: Praktikum 4 : Basic Routing & Migration
Dibawah ini merupakan langkah pengerjaan praktikum serta hasil screenshot pengerjaan Praktikum Modul 4 saya mengenai Basic Routing & Migration.

## :footprints: Langkah Percobaan
* ### GET<br/>
    Mengunjungi file web.php pada folder routes. Kemudian tambahkan baris ini pada akhir file <br/>
    ```
    ...

    $router->get('/get', function () {
        return 'GET';
    });
    ```
    ![Menambahkan baris kode](../assets/praktikum4/1.%20Menambahkan%20baris%20web.php.png)
    Setelah itu mencoba menjalankan aplikasi dengan command,
    ```
    php -S localhost:8000 -t public
    ```
    ![Mencoba menjalankan aplikasi](../assets/praktikum4/2.%20coba%20menjalankan.png)

* ### POST, PUT, PATCH, DELETE, dan OPTIONS<br/>
    Sama halnya saat menambahkan method GET, kita dapat menambahkan methode POST, PUT, PATCH, DELETE, dan OPTIONS pada file web.php dengan code seperti ini,
    ```
    ...

    $router->post('/post', function () {
        return 'POST';
    });
    $router->put('/put', function () {
        return 'PUT';
    });
    $router->patch('/patch', function () {
        return 'PATCH';
    });
    $router->delete('/delete', function () {
        return 'DELETE';
    });
    $router->options('/options', function () {
        return 'OPTIONS';
    });
    ```
    ![Menambahkan baris kode](../assets/praktikum4/3.%20Menambahkan%20kode.png)
    1. Kita dapat menginstall ekstensi dengan membuka panel extensions lalu mencari thunder client <br/>
    ![Install ekstensi thunder client](../assets/praktikum4/4.%20Install%20ekstensi%20thunder%20client.png)
    2. Setelah menginstall Thunder Client, kita akan melihat logo seperti petir pada activity bar kita (sebelah kiri).<br/>
    3. Kita dapat membuat request dengan menekan "New Request" pada ekstensi<br/>
    ![Menekan New Request](../assets/praktikum4/5.%20Menekan%20logo%20dan%20new%20Request.png)
    4. Setelah itu kita dapat memasukkan method dan url yang dituju<br/>
    5. Akses url yang baru saja ditambahkan pada aplikasi dengan methodnya<br/>
    ![Akses url yang baru ditambahkan dengan method](../assets/praktikum4/6.%20Akses%20method.png)

* ### Migrasi Database<br/>
    1. Sebelum melakukan migrasi database pastikan server database aktif kemudian pastikan sudah membuat database dengan nama `lumenapi`<br/>
    ![Membuat database](../assets/praktikum4/7.%20Membuat%20database.png)
    2. Kemudian ubah konfigurasi database pada file .env menjadi seperti ini
        ```
        DB_CONNECTION=mysql
        DB_HOST=127.0.0.1
        DB_PORT=3306
        DB_DATABASE=lumenapi
        DB_USERNAME=root
        DB_PASSWORD=<<password masing-masing>>
        ```
        ![Merubah konfigurasi .env](../assets/praktikum4/8.Merubah%20konfigurasi%20.env.png)
    3. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan beberapa library bawaan dari lumen dengan membuka file app.php pada folder bootstrap dan mengubah baris ini,
        ```
        //$app->withFacades();
        //$app->withEloquent();
        ```
        Menjadi,

        ```
        $app->withFacades();
        $app->withEloquent();
        ```
        ![Menghidupkan library bawaan](../assets/praktikum4/9.%20Menghidupkan%20library%20bawaan.png)
    4. Setelah itu jalankan command berikut untuk membuat file migration,
        ```
        php artisan make:migration create_users_table 
        php artisan make:migration create_products_table
        ```
        ![Membuat file migration](../assets/praktikum4/10.%20Membuat%20file%20migration.png)
    5. Ubah fungsi up pada file migrasi `create_users_table`
        ```
        # sebelumnya
        ...
        public function up()
        {
            Schema::create('users', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });
        }
        ...
        # diubah menjadi
        ...
        public function up()
        {
            Schema::create('users', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
                $table->string('name');
                $table->string('email');
                $table->string('password');
            });
        }
        ...
        ```
        ![Mengubah fungsi up pada file create_users_table](../assets/praktikum4/11.%20merubah%20create%20user%20table.png)
    6. Ubah fungsi up pada file migrasi `create_products_table`
        ```
        # sebelumnya
        ...
        public function up()
        {
            Schema::create('products', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });
        }
        ...
        # diubah menjadi
        ...
        public function up()
        {
            Schema::create('products', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
                $table->string('name');
                $table->integer('category_id');
                $table->string('slug');
                $table->integer('price');
                $table->integer('weight');
                $table->text('description');
            });
        }
        ...
        ```
        ![Mengubah fungsi up pada file create_product_table](../assets/praktikum4/12.%20merubah%20create%20product.png)
    7. Kemudian jalankan command,
        ```
        php artisan migrate
        ```
        ![Menjalankan artisan migrate](../assets/praktikum4/13.%20php%20artisan%20migrate.png)