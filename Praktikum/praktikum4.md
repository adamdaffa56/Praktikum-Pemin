# :globe_with_meridians: Praktikum 4 : Basic Routing & Migration
Dibawah ini merupakan langkah pengerjaan praktikum serta hasil screenshot pengerjaan Praktikum Modul 2 saya mengenai Basic Routing & Migration.

## :footprints: Langkah Percobaan
1. GET<br/>
Mengunjungi file web.php pada folder routes. Kemudian tambahkan baris ini pada akhir file <br/>
    ```
    ...

    $router->get('/get', function () {
        return 'GET';
    });
    ```
    Setelah itu coba jalankan aplikasi dengan command,
    ```
    php -S localhost:8000 -t public
    ```
    ![]()

2. POST, PUT, PATCH, DELETE, dan OPTIONS<br/>
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
    ![]()
        a. Kita dapat menginstall ekstensi dengan membuka panel extensions lalu mencari thunder client
    ![]()
    b. Setelah menginstall Thunder Client, kita akan melihat logo seperti petir pada activity bar kita (sebelah kiri).
    c. Kita dapat membuat request dengan menekan "New Request" pada ekstensi
    d. Setelah itu kita dapat memasukkan method dan url yang dituju
    e. Akses url yang baru saja ditambahkan pada aplikasi dengan methodnya

3. Migrasi Database<br/>
    a. Sebelum melakukan migrasi database pastikan server database aktif kemudian pastikan sudah membuat database dengan nama `lumenapi`
    b. Kemudian ubah konfigurasi database pada file .env menjadi seperti ini
    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=lumenapi
    DB_USERNAME=root
    DB_PASSWORD=<<password masing-masing>>
    ```
    c. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan beberapa library bawaan dari lumen dengan membuka file app.php pada folder bootstrap dan mengubah baris ini,
    ```
    //$app->withFacades();
    //$app->withEloquent();
    ```
    Menjadi,
    ```
    $app->withFacades();
    $app->withEloquent();
    ```
    d. Setelah itu jalankan command berikut untuk membuat file migration,
    ```
    php artisan make:migration create_users_table # membuat migrasi untuk tabel users
    php artisan make:migration create_products_table # membuat migrasi untuk tabel products
    ```
    e. Ubah fungsi up pada file migrasi `create_users_table`
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
    f. Ubah fungsi up pada file migrasi `create_products_table`
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
    g. Kemudian jalankan command,
    ```
    php artisan migrate
    ```