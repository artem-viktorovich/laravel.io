<h3>Введение</h3>
HTTP строка -> HTTP запрос (request)
HTTP запрос (request) -> router(классы)
router -> controller
controller ->model
model ->database
model -> view (внешняя обёртка)
view -> router

<span style="color: red;">composer dump-autoload</span>  - проверка загрузки и работоспособности файлов

<span style="color:red;">model</span> - представление 1-го объекта для интерфейса php.

composer create-project laravel/laravel example_app
<h3>Первый route в laravel</h3>
Первый request 
Основной старт в папке routes->web.php

Проверка работы route
```
Route::get('/', function () {  
    return 'Запуск!';  
});
```

Route без опрокидывания:

```
Route::get('/my_page', function (){  
    return 'page';  
});
```

<h3>Первый controller в laravel</h3>
<span style="color: red;">php artisan make:controller ВерблюжьяНотация</span> - команда создания контроллера - php artisan make:controller PostController

Далее создаётся файл в адресе app/Http/Controllerss\Postcontroller

В файле Postcontroller, в классе прописываем return

```
class PostController extends Controller  
{  
    public function index()  
    {  
        return 'page';  
    }  
}
```

index - обращение к нужному нам роутеру и т.д.

В route пишем обращение к контроллеру: 

```
Route::get('/posts', [PostController::class, 'index']);
```

<h3>Первая миграция в laravel</h3>
<span style="color:red;">php artisan migrate</span> - команда миграций
<h3>создаём модель с миграцией</h3>
```
php artisan make:model имя модели -m
```
<span style="color:red;">php artisan make:model Post -m</span> - команда миграций
В папке Models, создаётся модель Post
В пути database/migrations, создаётся миграция связанная с моделью
```
NFO  Model [app/Models/Post.php] created successfully.  

   INFO  Migration [database/migrations/2024_01_18_115154_create_posts_table.php] created successfully.  
```

В таблице (create_posts_table.php) в функции up прописываем свою таблице с нужными нам значениями
(create-создать)

Создаём таблицу
string - ограниченное количество вводимых символов
text - прописываем текст в неограниченном количестве
unsignedBigInteger - большое число положительных значений

Описывание таблицы
```
public function up(): void  
{  
    Schema::create('posts', function (Blueprint $table) {  
        $table->id();  
        $table->string('title');  
        $table->text('content');  
        $table->string('image')->nullable();  
        $table->unsignedBigInteger('likes')->nullable();  
        $table->boolean('is_published')->default(1);  
        $table->timestamps();  
    });  
}
```

делаем миграцию - <span style="color:red;">php artisan migrate</span>

Результат миграции
```
 INFO  Running migrations.  

  2024_01_18_115154_create_posts_table .................................................................................................. 106ms DONE

```

<h2>Модель в Laravel</h2>
Подробнее <a href="https://href.kz/blog/laravel/sozdanie-modeli-i-migracii-v-laravel-8-x">информация</a>

<h2>Модель. Метод чтения данных из базы</h2>
Хелперы
```
class PostController extends Controller  
{  
    public function index()  
    {  
        $str = 'string';  
        var_dump($str);  дампим данные, то есть читаем базу
    }  
}
```

Результат - string(6) "string"
При демонстрации данных и остановке выполнения кода, использeется зарезервированный метод dd()

код обретают след вид

```
class PostController extends Controller  
{  
    public function index()  
    {  
        $str = 'string';  
        dd($str);  дампим данные, то есть читаем базу и прекращается          выполнение
    }  
}
```

Если использовать чистый <span style="color:red;">dump</span>, функция не прекращает свою работу

Создаём первый пост в таблице

Далее вытягиваем данные с таблицы

```
public function index()  
{  
    $post = Post::find(1); - вытягиваем айдишник
    dump($post);  
}

```

Если вылазит ошибка, ставим путь к модели - use App\Models\Post; или Alt + Enter
Переменные называем как угодно

Результат вызова

```
App\Models\Post {#632 ▼ // app/Http/Controllers/PostController.php:13
  #connection: "mysql"
  #table: "posts"
  #primaryKey: "id"
  #keyType: "int"
  +incrementing: true
  #with: []
  #withCount: []
  +preventsLazyLoading: false
  #perPage: 15
  +exists: true
  +wasRecentlyCreated: false
  #escapeWhenCastingToString: false
  #attributes: array:8 [▶]
  #original: array:8 [▶]
  #changes: []
  #casts: []
  #classCastCache: []
  #attributeCastCache: []
  #dateFormat: null
  #appends: []
  #dispatchesEvents: []
  #observables: []
  #relations: []
  #touches: []
  +timestamps: true
  +usesUniqueIds: false
  #hidden: []
  #visible: []
  #fillable: []
  #guarded: array:1 [▶]
}
```
Таким образом дампится объект

Все свойства, указанные в объекте, используются с родительского класса Model

Также можем создать свои свойства

Пропишем его через public, тем самым задав ему уровень доступа

```
class Post extends Model  
{  
    use HasFactory;  
  
    public $someProperty;  
}
```

Результат

```
App\Models\Post {#632 ▼ // app/Http/Controllers/PostController.php:13
  #connection: "mysql"
  #table: "posts"
  #primaryKey: "id"
  #keyType: "int"
  +incrementing: true
  #with: []
  #withCount: []
  +preventsLazyLoading: false
  #perPage: 15
  +exists: true
  +wasRecentlyCreated: false
  #escapeWhenCastingToString: false
  #attributes: array:8 [▶]
  #original: array:8 [▶]
  #changes: []
  #casts: []
  #classCastCache: []
  #attributeCastCache: []
  #dateFormat: null
  #appends: []
  #dispatchesEvents: []
  #observables: []
  #relations: []
  #touches: []
  +timestamps: true
  +usesUniqueIds: false
  #hidden: []
  #visible: []
  #fillable: []
  #guarded: array:1 [▶]
  +someProperty: null - наше свойство
}
```

Чтобы обращаться к конкретному атрибуту, нужно вывести с помощью записи 
```
public function index()  
{  
    $post = Post::find(1);  
    dd($post->title);  
}
```

Результат - "first_title" // app/Http/Controllers/PostController.php:13

Расшифровка аттрибутов

```
App\Models\Post {#632 ▼ // app/Http/Controllers/PostController.php:13
  #connection: "mysql" - информация, об использовании вида базы данных
  #table: "posts" - имя таблицы
  #primaryKey: "id" - указан айдишник
  #keyType: "int" - тип
  +incrementing: true - айдишник автоинкрементируется
  #with: []
  #withCount: []
  +preventsLazyLoading: false
  #perPage: 15
  +exists: true
  +wasRecentlyCreated: false
  #escapeWhenCastingToString: false
  #attributes: array:8 [▶]
  #original: array:8 [▶]
  #changes: []
  #casts: []
  #classCastCache: []
  #attributeCastCache: []
  #dateFormat: null
  #appends: []
  #dispatchesEvents: []
  #observables: []
  #relations: []
  #touches: []
  +timestamps: true
  +usesUniqueIds: false
  #hidden: []
  #visible: []
  #fillable: []
  #guarded: array:1 [▶]
  +someProperty: null - кастомное свойство
}
```

Не лишним будет указать имя таблицы

```
class Post extends Model  
{  
    use HasFactory;  
    protected $table = 'posts';  
}
```

<h2>Модель. Реализация методов у модели</h2>

Подробная <a href="https://href.kz/blog/laravel/sozdanie-modeli-i-migracii-v-laravel-8-x">информация</a>

<h2>Модель. Методы чтения данных</h2>
Метод <span style="color:red;">find(1)</span>  он ищет айдишник, который указан в скобках, и выводит нашу строку

Для просмотра всех данных, делаем запись метода:
```
public function index()  
{  
    $posts = Post::all();  
    dd($posts);  
}
```

Браузер выведет массив с данными:

```
Illuminate\Database\Eloquent\Collection {#304 ▼ // app/Http/Controllers/PostController.php:13
  #items: array:2 [▼
    0 => App\Models\Post {#306 ▼
      #connection: "mysql"
      #table: "posts"
      #primaryKey: "id"
      #keyType: "int"
      +incrementing: true
      #with: []
      #withCount: []
      +preventsLazyLoading: false
      #perPage: 15
      +exists: true
      +wasRecentlyCreated: false
      #escapeWhenCastingToString: false
      #attributes: array:8 [▶]
      #original: array:8 [▶]
      #changes: []
      #casts: []
      #classCastCache: []
      #attributeCastCache: []
      #dateFormat: null
      #appends: []
      #dispatchesEvents: []
      #observables: []
      #relations: []
      #touches: []
      +timestamps: true
      +usesUniqueIds: false
      #hidden: []
      #visible: []
      #fillable: []
      #guarded: array:1 [▶]
    }
    1 => App\Models\Post {#307 ▼
      #connection: "mysql"
      #table: "posts"
      #primaryKey: "id"
      #keyType: "int"
      +incrementing: true
      #with: []
      #withCount: []
      +preventsLazyLoading: false
      #perPage: 15
      +exists: true
      +wasRecentlyCreated: false
      #escapeWhenCastingToString: false
      #attributes: array:8 [▶]
      #original: array:8 [▶]
      #changes: []
      #casts: []
      #classCastCache: []
      #attributeCastCache: []
      #dateFormat: null
      #appends: []
      #dispatchesEvents: []
      #observables: []
      #relations: []
      #touches: []
      +timestamps: true
      +usesUniqueIds: false
      #hidden: []
      #visible: []
      #fillable: []
      #guarded: array:1 [▶]
    }
  ]
  #escapeWhenCastingToString: false
}
```

Illuminate\Database\Eloquent\Collection - аналог массивов в ларавел
Если пройтись по всем массивам, посмотреть заголовки, по пишем
```
public function index()  
{  
    $posts = Post::all(); \\ выводим все посты
    foreach ($posts as $post){ \\ перебираем посты
        dump($post->title); \\выводим заголовки 
    }  
    dd($posts);  
}
```

Результат:
```
"first_title" // app/Http/Controllers/PostController.php:14

"2_post" // app/Http/Controllers/PostController.php:14
```

Для выбора is_published=1, запишем метод get()
При использовании get(), всегда попадёт коллекция в $posts.

```
public function index()  
{  
   $posts = Post::where('is_published', 1)->get();  
    foreach ($posts as $post){  
        dump($post->title);  
    }  
    dd('end');  
}
```

При этом методе ничего не меняется

Выведем первый пост. Для этого используется метод first()

```
public function index()  
{  
   $post = Post::where('is_published', 1)->first();  
    dump($post->title);  
    dd($post);  
}
```

<h2>Модель. Метод добавления данных в базу (create)</h2>
Для добавление данных в базу, создадим route

```
Route::get('/posts/create', [PostController::class, 'create'] );
```

Перейдём в PostController и создадим метод с атрибутами, которые находятся в таблице базы

```
class PostController extends Controller  
{  
    public function index()  
    {  
        $posts = Post::all();  
        foreach ($posts as $post) {  
            dump($post->title);  
        }  
  
    }  
  
    public function create()  
    {  
        $postsArr = [  
            [  
                'title' => 'title phpstorm',  
                'content' => 'interesting content',  
                'image' => 'image.jpg',  
                'likes' => '11',  
                'is_published' => 1,  
  
            ], [  
                'title' => 'another title phpstorm',  
                'content' => 'another interesting content',  
                'image' => 'another.jpg',  
                'likes' => '14',  
                'is_published' => 1,  
  
            ],  
        ];  
    }  
}
```

Чтоб создать пост, нужно обратиться к модели Post

```
class PostController extends Controller  
{  
    public function index()  
    {  
        $posts = Post::all();  
        foreach ($posts as $post) {  
            dump($post->title);  
        }  
  
    }  
  
    public function create()  
    {  
        $postsArr = [  
            [  
                'title' => 'title phpstorm',  
                'content' => 'interesting content',  
                'image' => 'image.jpg',  
                'likes' => '11',  
                'is_published' => 1,  
  
            ], [  
                'title' => 'another title phpstorm',  
                'content' => 'another interesting content',  
                'image' => 'another.jpg',  
                'likes' => '14',  
                'is_published' => 1,  
  
            ],  
        ];  
        
        
        Post::create([   \\указывает куда добавлять пост
            'title' => 'another title phpstorm',  
            'content' => 'another interesting content',  
            'image' => 'another.jpg',  
            'likes' => '14',  
            'is_published' => 1,  
        ]); 

		 dd('created'); -  показывает работу добавления поста в базу
    }  
}
```

Перейдём к посту - http://project/posts/create, далее будет ошибка

Add [title] to fillable property to allow mass assignment on [App\Models\Post]. - это значит, что в ларавел стоит защита от добавления постов
Для исправления ошибки, нужно перейти в модель, прописать разрешение <span style="color: red;">protected $guarded = []; или protected $guarded = false;  
</span> или <span style="color: red;">protected $ fillable= [ ' ' ];</span>, но во втором случае нужно писать атрибуты с таблицы 

 В браузере высветится <strong> "creared" // app\Http\Controllers\PostController.php:46,</strong> подтверждая выполнение запроса

<strong>Работа с массивом</strong>
 Для добавления массива, нужно пройтись циклом <strong>foreach</strong>
 
 Пропишем цикл и проверку на выведение массива
```
public function create()  
{  
    $postsArr = [  
        [  
            'title' => 'title phpstorm',  
            'content' => 'interesting content',  
            'image' => 'image.jpg',  
            'likes' => '11',  
            'is_published' => 1,  
  
        ], [  
            'title' => 'another title phpstorm',  
            'content' => 'another interesting content',  
            'image' => 'another.jpg',  
            'likes' => '14',  
            'is_published' => 1,  
  
        ],  
    ];  
    foreach ($postsArr as $item){  
        dd($item);  
        Post::create();  
    }  
  
  
  
    dd('created');  
}
```

Данная запись говорит о том, что массив добавлен

```
array:5 [▼ // app\Http\Controllers\PostController.php:39
  "title" => "title phpstorm"
  "content" => "interesting content"
  "image" => "image.jpg"
  "likes" => "11"
  "is_published" => 1
]
```

```
public function create()  
{  
    $postsArr = [  
        [  
            'title' => 'title phpstorm',  
            'content' => 'interesting content',  
            'image' => 'image.jpg',  
            'likes' => '11',  
            'is_published' => 1,  
  
        ], [  
            'title' => 'another title phpstorm',  
            'content' => 'another interesting content',  
            'image' => 'another.jpg',  
            'likes' => '14',  
            'is_published' => 1,  
  
        ],  
    ];  
    foreach ($postsArr as $item){  
        Post::create($item);  
    }  
  
  
  
    dd('created');  
}
```

Результат Created, посты добавлены в таблицу

<h2>Модель. Методы обновления данных (update)</h2>

Для обновления данных в базу, создадим route

```
Route::get('/posts/update', [PostController::class, 'update'] );
```

Для обновления данных, нужно по id вытаскивать нужным нам пост

пропишем в контроллере

```
public function update()  
{  
    $post = Post::find(6);  - айдишник поста
    dd($post->title); - заголовок поста
}
```

Создадим объект вызывая метод <strong>update</strong>

```
public function update()  
{  
    $post = Post::find(6);  
    $post ->update([  
        'title' => 'updated title phpstorm',  
        'content' => 'updated interesting content',  
        'image' => 'updated.jpg',  
        'likes' => '13',  
        'is_published' => 0,  
    ]);  
    dd('update');  - проверка выполнения операции
}
```

По ключу меняем нужные значения тем самым обновляя данные в таблице