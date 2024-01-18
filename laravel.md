<h3>Введение</h3>
HTTP строка -> HTTP запрос (request)
HTTP запрос (request) -> router(классы)
router -> controller
controller ->model
model ->database
model -> view (внешняя обёртка)
view -> router

<span style="color:red;">model</span> - представление 1-го объекта для интерфейса php.

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
        dd($str);  дампим данные, то есть читаем базу
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
