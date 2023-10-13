# :globe_with_meridians: Praktikum 6 : Model Controller & Request Response
Dibawah ini merupakan langkah pengerjaan praktikum serta hasil screenshot pengerjaan Praktikum Modul 6 saya mengenai Model Controller & Request Response.

## :scroll: Dasar Teori
* ### Model
    Model merupakan bagian yang bertugas untuk menyiapkan, mengatur, memanipulasi, dan mengorganisasikan data yang ada di database. Model merepresentasikan kolom apa saja yang ada pada databas, termasuk relasi dan primary key dapat didefinisikan di dalam model. Dengan menggunakan perintah Artisan, pembuatan model pada Laravel dapat dilakukan dengan satu perintah menggunakan
    ```
    php artisan make:model nama_model
    ```
    Namun karena perintah Artisan yang terbatas pada Lumen, pembuatan model harusdilakukan secara manual.

* ### Controller
    Controller merupakan bagian yang menjadi tempat berkumpulnya logika pemrograman yang digunakan untuk memisahkan organisasi data pada database. Dalam beberapa kasus, controller menjadi penghubung antara model dan view pada arsitektur MVC

* ### Request Handler
    Request handler adalah fungsi yang digunakan untuk berinteraksi dengan request yang datang. Request handler dapat digunakan untuk melihat apa saja yang dikirimkan oleh user seperti parameter, query, dan body.

* ### Response Handler
    Response handler adalah fungsi yang digunakan untuk membentuk output yang diharapkan kepada user dan beberapa properti selain data seperti status code dan header.
    
## :footprints: Langkah Percobaan
* ### Model 
    1. Pastikan terdapat tabel users yang dibuat menggunakan migration pada bab sebelumnya. Berikut informasi kolom yang harus ada <br/>

        |  |  
        | ----------- | 
        | id |
        | createdAt | 
        | updatedAt | 
        | name | 
        | email | 
        | password |

        ![Memastikan structure tabel user](../assets/praktikum6/1.%20Memastikan%20tabel%20user.png)

    2. Bersihkan isi User.php yang ada sebelumnya dan isi dengan baris kode berikut
        ```
        <?php

        namespace App\Models;
        
        use Illuminate\Database\Eloquent\Model;
        
        class User extends Model
        {
            /**
            * The attributes that are mass assignable.
            *
            * @var array
            */
            protected $fillable = [
            'name', 'email', 'password'
            ];
            /**
            * The attributes excluded from the model's JSON form.
            *
            * @var array
            */
            protected $hidden = [];
        }
        ```
        ![Mengubah isi User.php](../assets/praktikum6/2.%20Mengubah%20isi%20user.php.png)

* ### Controller
    1. Buatlah salinan ExampleController.php pada folder app/Http/Controllers dengan nama HomeController.php dan buatlah fungsi index() yang berisi
        ```
        <?php
        
        namespace App\Http\Controllers;
        
        class HomeController extends Controller
        {
            /**
            * Create a new controller instance.
            *
            * @return void
            */
            public function __construct()
            {
                //
            }

            // Pembuatan fungsi index() //
            public function index()
            {
                return 'Hello, from lumen!';
            }
            // Pembuatan fungsi index() //
            
            //
        }
        ```
        ![Membuat HomeController.php](../assets/praktikum6/3.%20Membuat%20HomeController.png)
        
    2. Ubah route / pada file routes/web.php menjadi seperti ini
        ```
        # Sebelum,
        $router->get('/', function () use ($router) {
            return $router->app->version();
        });

        # Setelah,
        $router->get('/', ['uses' => 'HomeController@index']);
        ```
        ![Mengubah route](../assets/praktikum6/4.%20Mengubah%20route.png)

    3. Jalankan aplikasi
        ![Menjalankan aplikasi](../assets/praktikum6/5.%20Menjalankan%20aplikasi.png)

* ### Request Handler
    1. Lakukan import library Request dengan menambahkan baris berikut di bagian atas file
        ```
        <?php

        namespace App\Http\Controllers;
        
        // Import Library Request
        use Illuminate\Http\Request;
        ```
        ![import library request](../assets/praktikum6/6.%20Import%20library%20request.png)

    2. Ubah fungsi index menjadi
        ```
        <?php
        
        namespace App\Http\Controllers;
        
        use Illuminate\Http\Request;
        
        class HomeController extends Controller
        {
            /**
            * Create a new controller instance.
            *
            * @return void
            */
            public function __construct()
            {
                //
            }
            
            // Perubahan fungsi index
            public function index (Request $request)
            {
            return 'Hello, from lumen! We got your request from endpoint: ' . $request->path();
            }
            // Perubahan fungsi index
            
            //
        }
        ```
        ![Mengubah fungsi index](../assets/praktikum6/7.%20Mengubah%20fungsi%20index.png)

    3. Jalankan aplikasi
        ![Menjalankan aplikasi](../assets/praktikum6/8.%20Menjalankan%20aplikasi.png)

* ### Response Handler
    1. Lakukan import library Response dengan menambahkan baris berikut di bagian atas file
        ```
        <?php

        namespace App\Http\Controllers;
        use Illuminate\Http\Request;
        use Illuminate\Http\Response; // import library Response
        ```
        ![Import library response](../assets/praktikum6/9.%20Import%20library%20response.png)

    2. Buatlah fungsi hello() yang berisi
        ```
        <?php
        
        namespace App\Http\Controllers;
        
        use Illuminate\Http\Request;
        use Illuminate\Http\Response;
        
        class HomeController extends Controller
        {
            /**
            * Create a new controller instance.
            *
            * @return void
            */
            public function __construct()
            {
                //
            }
            
            public function index (Request $request)
            {
            return 'Hello, from lumen! We got your request from endpoint: ' . $request->path();
            }
            
            //
            
            // Pembuatan fungsi hello
            public function hello()
            {
                $data['status'] = 'Success';
                $data['message'] = 'Hello, from lumen!';
                return (new Response($data, 201))
                    ->header('Content-Type', 'application/json');
            }
        }
        ```
        ![Menambahkan fungsi hello()](../assets/praktikum6/10.%20Menambahkan%20fungsi%20hello().png)

    3. Tambahkan route /hello pada file routes/web.php
        ```
        <?php
        
        $router->get('/', ['uses' => 'HomeController@index']);
        $router->get('/hello', ['uses' => 'HomeController@hello']); // route hello
        ```
        ![Menambahkan route hello](../assets/praktikum6/11.%20Menambahkan%20route%20hello.png)
    4. Jalankan aplikasi pada route /hello
        ![Menjalankan aplikasi](../assets/praktikum6/12.%20Menjalankan%20aplikasi.png)

* ### Penerapan
    1. Lakukan import model User dengan menambahkan baris berikut di bagian atas file
        ```
        <?php
        
        namespace App\Http\Controllers;
        
        use App\Models\User; // import model User
        use Illuminate\Http\Request;
        use Illuminate\Http\Response;
        ```
        ![Import model User](../assets/praktikum6/13.%20Import%20User.png)

    2. Tambahkan ketiga fungsi berikut di HomeController.php
        ```
        <?php

        namespace App\Http\Controllers;

        use App\Models\User; // import model User
        use Illuminate\Http\Request;
        use Illuminate\Http\Response;

        class HomeController extends Controller
        {
            ...
            // Tiga Fungsi
            public function defaultUser()
            {
                $user = User::create([
                    'name' => 'Nahida',
                    'email' => 'nahida@akademiya.ac.id',
                    'password' => 'smol'
                ]);

                return response()->json([
                    'status' => 'Success',
                    'message' => 'default user created',
                    'data' => [
                        'user' => $user,
                    ]
                ],200);
            }

            public function createUser(Request $request)
            {
                $name = $request->name;
                $email = $request->email;
                $password = $request->password;

                $user = User::create([
                    'name' => $name,
                    'email' => $email,
                    'password' => $password
                ]);

                return response()->json([
                    'status' => 'Success',
                    'message' => 'new user created',
                    'data' => [
                        'user' => $user,
                    ]
                ],200);
            }

            public function getUsers()
            {
                $users = User::all();
            
                return response()->json([
                    'status' => 'Success',
                    'message' => 'all users grabbed',
                    'data' => [
                        'users' => $users,
                    ]
                ],200);
            }
            // Tiga Fungsi
        }
        ```
        ![Menambahkan 3 fungsi](../assets/praktikum6/14.%20Menambahkan%203%20fungsi.png)

    3. Tambahkan ketiga route pada file routes/web.php menggunakan group route
        ```
        $router->get('/', ['uses' => 'HomeController@index']);
        $router->get('/hello', ['uses' => 'HomeController@hello']);

        // Tiga Route
        $router->group(['prefix' => 'users'], function () use ($router) {
            $router->post('/default', ['uses' => 'HomeController@defaultUser']);
            $router->post('/new', ['uses' => 'HomeController@createUser']);
            $router->get('/all', ['uses' => 'HomeController@getUsers']);
        });
        ```
        ![Menambahkan tiga route](../assets/praktikum6/15.%20Menambahkan%20tiga%20route.png)

    4. Jalankan aplikasi pada route /users/default menggunakan Postman
        ![Menjalankan aplikasi pada route /users/default](../assets/praktikum6/16.%20Menjalankan%20route%20user%20default.png)
    5. Jalankan aplikasi pada route /users/new dengan mengisi body sebagai berikut
    
        |  |  |
        | ----------- | ----------- |
        | name | Cyno | 
        | email | cyno@akademiya.ac.id |
        | password | mahamatra |

        ![Menjalankan aplikasi pada route users/new](../assets/praktikum6/17.%20Menjalankan%20aplikasi%20route%20users%20new.png)
    
    6. Jalankan aplikasi pada route /users/all
        ![Menjalankan aplikasi pada route users/all](../assets/praktikum6/18.%20Menjalankan%20aplikasi%20route%20users%20all.png)
