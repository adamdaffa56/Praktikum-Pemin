# :globe_with_meridians: Praktikum 5 : Dynamic Route & Middleware
Dibawah ini merupakan langkah pengerjaan praktikum serta hasil screenshot pengerjaan Praktikum Modul 5 saya mengenai Dynamic Route & Middleware.

## :footprints: Langkah Percobaan
### 1. Dynamic Route<br/>
    Dynamic route adalah route yang dapat berubah-ubah, contohnya pada saat kita membuka suatu halaman web, kadang kita melihat `/users/1` atau `/users/2` , hal ini yang dinamakan dynamic routes. <br/>
    
    Untuk menambahkan dynamic routes pada aplikasi lumen kita, kita dapat menggunakan syntax berikut,
    ```
    $router->get('/user/{id}', function ($id) {
        return 'User Id = ' . $id;
    });
    ```
    ![Dynamic Route Id](../assets/praktikum5/1.%20Dynamic%20Route%20id.png)

    Saat menambahkan parameter pada routes, kita tidak terbatas pada 1 variable saja, namun kita dapat menambahkan sebanyak yang diperlukan seperti kode berikut,
    ```
    $router->get('/post/{postId}/comments/{commentId}', function ($postId, $commentId) {
        return 'Post ID = ' . $postId . ' Comments ID = ' . $commentId;
    });
    ```
    ![Dynamic Route variabel post dan comment](../assets/praktikum5/2.%20Dynamic%20Route%20banyak%20variable.png)
    Pada dynamic routes kita juga bisa menambahkan optional routes, yang mana optional routes tidak mengharuskan kita untuk memberi variable pada endpoint kita, namun saat kita memanggil endpoint, dapat menggunakan parameter variable ataupun tidak, seperti pada kode dibawah ini,
    ```
    $router->get('/users[/{userId}]', function ($userId = null) {
        return $userId === null ? 'Data semua users' : 'Data user dengan id ' . $userId;
    });
    ```
    ![Optional Route](../assets/praktikum5/3.%20Optional%20Route.png)


### 2. Aliases Route
    Aliases Route digunakan untuk memberi nama pada route yang telah kita buat, hal ini dapat membantu kita, saat kita ingin memanggil route tersebut pada aplikasi kita. Berikut syntax untuk menambahkan aliases route
    ```
    $router->get('/auth/login', ['as' => 'route.auth.login', function (...) {...}])

    ...

    $router->get('/profile', function (Request $request) {
        if ($request->isLoggedIn) {
            return redirect()->route('route.auth.login');
        }
    });
    ```


