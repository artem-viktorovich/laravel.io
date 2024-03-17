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

<h2>Модель. Метод удаления данных (delete) и soft delete</h2>
Для этого создаётся метод delete

Создаём route

Route::get('/posts/delete', [PostController::class, 'delete'] );

В контроллере прописываем класс delete и проверку выполнения
```
public function delete()  
{  
    $post = Post::find(5);  
    $post->delete();  
    dd('delete');  
}
```

Такой метод в реальной жизни не используется. Так как при повторном запросе будет ошибка удаления не существующего айдишника.
Поэтому используется контролируемого удаления - <strong>soft delete (мягкое удаление)</strong>

В таблице создаём колонку с пунктом:
```
$table->softDeletes();
```

Находим модель, прописываем в ней трейд (некая реализация логики)

```
use SoftDeletes;
```

Для изменения, нужно обновить миграции, тем самым данные в таблице удаляться

```
php artisan migrate:fresh
```

После обновления и добавления постов, добавляется колонка <strong>delete_at</strong>
При удалении данных, появляется в ячейке delete_at дата удаления поста
Для восстановления данных, нужно прописать в классе  delete такую запись
```
public function delete()  
{  
    $post = Post::withTrashed() -> find(2);
    $post->restore();  
    dd('delete');  
}
```

После обновления удалённый пост восстанавливается и дата удаляется.

<h2>Комбинированные  методы создания и обновления данных</h2>
Бывают ситуации, когда из базы требуется достать запись, но её нет, соответственно её нужно создать, для этого используют <strong>firstOrCreate</strong>. Иногда это используется для проверки дубликатов.
Аналогичный запрос <strong>updateOrCreate</strong> - обновление постов, записей и при этом совпадение по атрибутам.

Создаём route 

```
Route::get('/posts/firstOrCreate', [PostController::class, 'firstOrCreate'] );
```

Прописываем в контроллере новый класс для добавления поста

```
public function firstOrCreate()  
{  
    $anotherPost = [  
        'title' => 'Create title phpstorm',  
        'content' => 'Create interesting content',  
        'image' => 'Create.jpg',  
        'likes' => '132',  
        'is_published' => 1,  
    ];  
    $post = Post::firstOrCreate([  
        'title' => 'Create title phpstorm',  если находится пост с таким title,то он его отдаёт, если нет, он создаёт пост со всем содержащим в title ниже
    ],[  
        'title' => 'Create title phpstorm',  
        'content' => 'Create interesting content',  
        'image' => 'Create.jpg',  
        'likes' => '132',  
        'is_published' => 1,  
    ]);  
    dump($post->content);  
    dd('firstOrCreate');  
}
```

 При проверке title, если его содержимое совпадает с содержимым в таблице, в браузере будет ответ - <span style="color:red;">"interesting content" // app\Http\Controllers\PostController.php:82</span>
 Если нет такого поста с таким содержимым, в браузере появится ответ - <span style="color:red;">"Create interesting content" // app\Http\Controllers\PostController.php:82</span>
 Аналогично срабатывает updateOrCreate, при проверке, проверяется title? если он есть, можно обновить данные
```
public function updateOrCreate()  
{  
    $anotherPost = [  
        'title' => 'update title phpstorm',  
        'content' => 'update interesting content',  
        'image' => 'Create.jpg',  
        'likes' => '132',  
        'is_published' => 1,  
    ];  
    $post = Post::updateOrCreate([  
        'title' => 'update title phpstorm',  
    ],[  
        'title' => 'update title phpstorm2',  
        'content' => 'Create interesting content',  
        'image' => 'Create.jpg',  
        'likes' => '132',  
        'is_published' => 1,  
    ]);  
    dump($post->content);  
    dd('updateOrCreate');  
}
```

<h2>  Миграции. Редактирование миграций </h2>

Для изменения миграций создаётся другая миграция.

Основные действия:
<ul>
<li>удалить атрибут</li>
<li>добавить атрибут</li>
<li>изменить атрибут</li>
</ul>

Выполним добавление колонки через миграцию

<span style='color: red;'>php artisan make:migration add_column_description_to_posts_table</span> - данная миграция с аббревиатурой <strong>to_posts_table</strong> говорит о том, что идёт привязка к таблице posts, и её редактирование

принцип изменения таблицы, описывается в таблице, и обязательно нужно внести откат миграции

Прописываем добавление колонки в таблице

```
public function up(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->text('description')->nullable();  
    });  
}
```

Для отката, также пропишем удаление изменений
```
public function down(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->dropColumn('description');  
    });  
}
```

Далее выполняем накат миграции <span style="color:red;">php artisan migrate</span>

Проверяем откат миграции <span style="color:red;">php artisan migrate:rollback</span>

Так как добавляемая колонка прописывается последней нужно прописать место создания колонки

```
public function up(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->text('description')->nullable()->after('content');  
    });  
}
```

Для удаления конкретной колонки пропишем команду - <span style="color:red;">php artisan make:migration delete_column_description_to_posts_table</span>

Миграция создана. В созданной миграции нужно прописать удаление колонки
```
public function up(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->dropColumn('description');  
    });  
}
```

Для отката действия прописываем создание колонки

```
public function down(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->text('description')->nullable()->after('content');  
    });  
}
```

Далее идёт редактирование атрибутов

Создаем миграцию с привязкой к таблице - <span style="color:red;">php artisan make:migration edit_column_content_to_posts_table</span>

В созданной миграции, прописываем правило изменения для указанных атрибутов
```
public function up(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->renameColumn('content', 'post_content');  
    });  
}
```

Аналогично делаем для отката

```
public function down(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->renameColumn('post_content', 'content');  
    });  
}
```

Изменим атрибут у <strong>post_content</strong> - <span style="color:red;"> php artisan make:migration change_column_post_content_to_string_to_posts_table</span>
В созданной миграции прописываем правило для изменения атрибута

```
public function up(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->string('post_content')->change();  
    });  
}
```

Откат действий

```
public function down(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->text('post_content')->change();  
    });  
}
```

Для удаления таблицы создаём миграцию <strong>php artisan make:migration delete_table_from_db</strong>


```
public function up(): void  
{  
    Schema::table('posts', function (Blueprint $table) {  
        $table->text('description')->nullable()->after('content');  
    });  
}
```

Для отката прописывается создание таблицы

```
public function down(): void  
{  
    Schema::create('posts', function (Blueprint $table) {  
    $table->id();  
    $table->string('title');  
    $table->string('post_content');  
    $table->string('image')->nullable();  
    $table->unsignedBigInteger('likes')->nullable();  
    $table->boolean('is_published')->default(1);  
    $table->timestamps();  
    $table->softDeletes();  
}); 
}
```

<h2>View</h2>
Сбросим таблицы удалив ранее созданные миграции и перезагрузить бд -  php artisan migrate:Fresh

<strong>View</strong> - некий шаблон, в который помещается шаблон данных

Выведем наши посты

```
public function index()  
{  
    $posts = Post::all();  
    dd($posts);  
  
}
```

Будем работать с файлом, который находится - resources/views/welcome.blade.php

Создадим в этой директории файл - posts.blade.php
Чтобы вызвать этот файл, нужно обратиться через route  index
Пропишем с помощью зарезервированного метода вызов <strong>view</strong>
Для просмотра аргументов, в скобках нажимаем CTRL + P

Вызовем файл posts.blade.php

```
public function index()  
{  
    $posts = Post::all();  
    return view('posts');  
  
}
```

как итог, в браузере появится нужным нам файл с фразой - this is blade file

Создадим шаблон, для дальнейшей работы в файле html:5 TAB
В этом файле можно использовать обычные теги, для вёрстки, но и также, можно связать базу данных с постами и выводить их пользователю.

пропишем вывод наших постов через цикл

```
@foreach($posts as $post)  - инструмент вывода информации
    <div>{{$post->title}}</div>  - показ нужной нам информации в браузере
@endforeach
```
Данная конструкция не сработает, так как выводится ошибка - <strong>Undefined variable $posts</strong>

Для этого в контроллере нужно вывести эту переменную для возврата

```
public function index()  
{  
    $posts = Post::all();  
    return view('posts', compact('posts'));  
  
}
```

В браузер выводится title, который прописан в таблице БД


Пропишем все теги для работы и вывода базы данных

```
@foreach($posts as $post)  
    <div>{{$post->title}}</div>   
    <div>{{$post->content}}</div>   
    <div>{{$post->likes}}</div>  
@endforeach
```

Во view можно создавать систему шаблонов
До пустим будет несколько страниц
Для этого создаётся система шаблонизации, чтобы повторяющие части кода не прописывать по новой
Для этого создадим папку views/layouts/main.blade.php

Далее нужно laravel объяснить, что создаваемый шаблон можно пере использовать на других страницах.

Этот способ даст возможность подгрузки других страниц, в зависимости от того, какую выбираем

Для этого создаём тег в других страницах

```
@section('content')  тег для переиспользования контента на других страницах
    <div>  
        this is about page  
    </div>  
@endsection
```

Для того, чтобы этот тег подгружался, в создаваемом шаблоне прописываем тег <strong>@yield</strong>

Прописываем тег, с обращением в шаблон -  @yield('content')

```
<!doctype html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport"  
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">  
    <meta http-equiv="X-UA-Compatible" content="ie=edge">  
    <title>Document</title>  
</head>  
<body>  
<div>  
    @yield('content')  
</div>  
</body>  
</html>
```

В странице, которую нужно экспортировать на шаблон, нужно  указать extends, для связки с шаблоном
	
Пример:
	В файле posts.blade.php пишем связку
```
@extends('layouts.main')  
@section('content')  
    <div>  
        this is about page  
    </div>  
@endsection
```

Для того, чтобы страницы работали, нужно создать Route, под каждую страницу и контроллеры. В файле web.php прописываем роуты


В созданных контроллерах прописываем обращение к постам

```
<?php  
  
namespace App\Http\Controllers;  
  
use App\Models\Post;  
use Illuminate\Http\Request;  
  
class AboutController extends Controller  
{  
    public function index()  
    {  
        return view('about');  
  
    }  
}
```
По аналогии прописывается contact и main

В файле main.blade.php создадим навигацию.
```
<div>  
    <nav>        
    <ul>            
	    <li><a href="{{route('about.index')}}">About</a></li>  
		<li><a href="{{route('main.index')}}">Main</a></li>  
		<li><a href="{{route('contact.index')}}">Contact</a></li>  
		<li><a href="{{route('post.index')}}">Posts</a></li>
    </ul>    
    </nav>
    </div>
```

Для использования ссылок, в роутах, нужно задать имя этим ссылкам

```
  
Route::get('/posts', [PostController::class, 'index'] )->name('post.index');  
Route::get('/about', [AboutController::class, 'index'] )->name('about.index');  
Route::get('/contact', [ContactController::class, 'index'] )->name('contact.index');  
Route::get('/main', [MainController::class, 'index'] )->name('main.index');
```

<h2>bootstrap в ларе</h2>
<h3>Подключение в проекту</h3>
Bootstrap подключается через интерфейс лары
<strong>composer require laravel/ui</strong> - команда установки ui компонентов
После установки, прописываем команду <strong>php artisan</artisan>, вывод кода

```
 ui
  ui:auth                 Scaffold basic login and registration views and routes
  ui:controllers          Scaffold the authentication controllers
```

Фрагмент кода, который говорит об установке компонентов

Подключаем - <strong>php artisan ui bootstrap</strong>
Появится ошибка и команды, которые помогут установить доп пакеты npm

```
   WARN  Please run [npm install && npm run dev] to compile your fresh scaffolding.
```

Так как OPEN SERVER не видит этого пакета, делаем установку с помощью php storm

после команды php run dev , выводится информация
```

> dev
> vite


  VITE v4.5.2  ready in 244 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help

  LARAVEL v10.44.0  plugin v0.7.8

  ➜  APP_URL: http://localhost


```
Говорит о том, что компилятор запущен, для выгрузки проекта, в конечный результат, нужно запустить команду <strong>npm run build</strong>

<strong>vite.config.js</strong> - настройка компиляции кода

В директории public, появится папка build с компилированными стилями

<h3>подключаем bootstrap</h3>
Переходим в директорию <strong>views/layouts/main.blade.php</strong> и подключаем стили

Используем хелпер, для подключения стилей

```
<link rel="stylesheet" href="{{asset('build/assets/app-1bd03d06.css')}}">
```

Добавим пару классов

```
<div class="container">  
    <div class="row">  
        <nav>            
        <ul>                
		        <li><a href="{{route('about.index')}}">About</a></li>  
                <li><a href="{{route('main.index')}}">Main</a></li>  
                <li><a href="{{route('contact.index')}}">Contact</a></li>  
                <li><a href="{{route('post.index')}}">Posts</a></li>  
            </ul>        
        </nav>    
        </div>    
        @yield('content')  
</div>
```

и перейдя на сайт, видно изменение стилей и добавится контейнер

<h2>CRUD через интерфейс, Имена роутов, контроллеров по конвенции Laravel</h2>
CRUD реализует связку того, что приходит с фронтенда, идёт обработка и получать нужный нам результат

В CRUD существует 7 шаблонов

<a href="https://laravel.su/docs/10.x/controllers#deistviia-vypolniaemye-resursnymi-kontrollerami" target="_blank">Основная информация</a> 

Route Name -  описывается основной нейминг роутов

Так как нужно соблюдать конвенцию, нужно переименовать blade файлы, дабы привести к нужному виду. Так как работаем с постами, нужно подстроить под посты.

Создаём директорию post перемещаем файл posts.blade.php, переименовываем в  index.blade.php и редактируем контроллер

```
public function index()  
{  
    $posts = Post::all();  
    return view('post.index', compact('posts'));  
  
}
```

index.blade.php пропишем код, для вывода title

```
@extends('layouts.main')  
@section('content')  
    <div>  
        @foreach($posts as $post)  
            <div>  
                {{$post->id}}. {{$post->title}}  айдишник и заголовок
            </div>  
        @endforeach  
    </div>  
@endsection
```

Далее нужно создать интерфейс для поста, вторая строка таблицы

В контроллере PostController редактируем <strong>create</strong>

```
public function create()  
{  
    return view('post.create');  
}
```

файл <strong>create.blade.php</strong> - создаём интерфейс

```
<div>  
    <form action="{{route('post.store')}}" method="post">
				    связь добавления поста с оправкой в бд   
    <div class="mb-3">  
            <label for="title" class="form-label">Title</label>  
            <input type="text" class="form-control" id="title" placeholder="Title">  
        </div>        
        <div class="mb-3">  
            <label for="content" class="form-label">Content</label>  
            <textarea type="text" class="form-control" id="content" placeholder="Content"></textarea>  
        </div>        
        <div class="mb-3">  
            <label for="image" class="form-label">Content</label>  
            <input type="text" class="form-control" id="image" placeholder="image">  
        </div>        
        <button type="submit" class="btn btn-primary">Создать пост</button>  
    </form></div>
```
Переходим по адресу роута posts/create
Появляется форма, для работы добавления поста

Теперь нужно создать роут, куда будут приходить эти данные store, 3я строка

Создаём роут в web.php

```
Route::post('/posts', [PostController::class, 'store'] )->name('post.store');
															отправка данных
```

В контроллере создаём метод store

```
public function store()  
{  
    dd('store');  
}
```


При отправке поста, возникнет ошибка 419, это говорит о том, что при использовании кроме алгоритмов get, нужно использовать <span style="font-size: 30px;">CRSF токен </span>
Добавляется он в форме
Редактируем, добавляя name, с ключами полей
```
<form action="{{route('post.store')}}" method="post">  
    @csrf  - наш токен
    <div class="mb-3">  
        <label for="title" class="form-label">Title</label>  
        <input type="text" name="title" class="form-control" id="title" placeholder="Title">  
    </div>    <div class="mb-3">  
        <label for="content" class="form-label">Content</label>  
        <textarea type="text" name="content" class="form-control" id="content" placeholder="Content"></textarea>  
    </div>    <div class="mb-3">  
        <label for="image" class="form-label">Content</label>  
        <input type="text" name="image" class="form-control" id="image" placeholder="image">  
    </div>    <button type="submit" class="btn btn-primary">Создать пост</button>  
</form>
```

Теперь при добавлении поста, роут отрабатывает корректно.
Далее нужно получить данные, которые вводятся в поля

```
public function store()  
{  
    $data = request()->validate([  
        'title' => 'string',    \
        'content' => 'string',   } контроль заполнения полей
        'image' => 'string',    /
    ]);  
    dd($data); 
}
```

Информация о <a href="https://laravel.su/docs/8.x/validation#dostupnye-pravila-validacii" target="_blank"> валидации </a>

Теперь сделаем создание поста в store

```
public function store()  
{  
    $data = request()->validate([  
        'title' => 'string',  
        'content' => 'string',  
        'image' => 'string',  
    ]);  
    Post::create($data);  
    return redirect()->route('post.index');  - возврат на страницу опубликованных постов
}
```

Для создания конкретной записи, нужно создать роут

```
Route::get('/posts/{post}', [PostController::class, 'show'] )->name('post.show');
```

Теперь создадим action в контроллере

```
public function show($id)  
{  
    $post = Post::find($id);  
  
    dd($id);  
}
```

Данная запись, показывает, что можно прописать любой ключ в айдишник, так как прописано правило <strong>/posts/{post}</strong>

Пропишем вывод title через прописанный айдишник

```
public function show($id)  
{  
    $post = Post::find($id);  
  
    dd($post->title);  
}
```

Если вписать айдишник, которого в таблице нет, то вылезет ошибка - <strong>Attempt to read property "title" on null</strong>

Поэтому используется метод - <strong>findOrFail</strong>

```
public function show($id)  
{  
    $post = Post::findOrFail($id);  
  
    dd($post->title);  
}
```

Если искуемый айдишник не будет найден,. то вернёт ошибку 404

Данную запись можно упростить - 

```
public function show(Post $post)  
{  
    dd($post->title);  
}
```
Для возврата поста, создадим конструкцию в новом файле в директории - views/post/show.blade.php

```
@extends('layouts.main')  
@section('content')  
            <div> {{$post->id}}. {{$post->title}}  
            </div>  
            <div>  {{$post->content}}</div>  
  
@endsection
```

В контроллере, пропишем action

```
public function show(Post $post)  
{  
    return view('post.show', compact('post'));  
}
```

При выборе айдишника, получим title и контент поста

Теперь сделаем возможность переходить на пост и смотреть его содержимое

```
@extends('layouts.main')  
@section('content')  
    <div>  
        @foreach($posts as $post)  
            <div><a href="{{route('post.show', $post->id)}}">{{$post->id}}. {{$post->title}}</a></div>  
        @endforeach  
    </div>  
@endsection
```

Теперь в show сделаем кнопку для возврата на предыдущую страницу

```
@extends('layouts.main')  
@section('content')  
            <div>{{$post->id}}. {{$post->title}}</div>  
            <div>{{$post->content}}</div>  
            <div><a href="{{route('post.index')}}">Назад</a></div>  
@endsection
```

Теперь в index.blade.php сделаем кнопку для перехода на страницу создания поста

```
@extends('layouts.main')  
@section('content')  
    <div>  
        <a href="{{route('post.create')}}">Создать пост</a>  - добавленная функция
    </div>    <div>        @foreach($posts as $post)  
            <div><a href="{{route('post.show', $post->id)}}">{{$post->id}}. {{$post->title}}</a></div>  
        @endforeach  
    </div>  
@endsection
```


Создадим route - edit , для редактирования поста

```
Route::get('/posts/{post}/edit', [PostController::class, 
                   {айдишник поста}/edit
                   'edit'] )->name('post.edit');
```

Обратимся к action

```
public function edit(Post $post)  
{  
   return view('post.edit', compact('post'));
}
```

Создаём файл edit.blade.php, поместив туда форму
Редактируем данные приёма и способ метод отправки

```
@extends('layouts.main')  
@section('content')  
    <div>  
        <form action="{{route('post.update', $post->id)}}" 
                                    используемый роут
                                    method="post">  
            @csrf  
            @method('patch')  - метод отправки по конвенции ларавел
            <div class="mb-3">  
                <label for="title" class="form-label">Title</label>  
                <input type="text" name="title" class="form-control" id="title" placeholder="Title">  
            </div>            <div class="mb-3">  
                <label for="content" class="form-label">Content</label>  
                <textarea type="text" name="content" class="form-control" id="content" placeholder="Content"></textarea>  
            </div>            <div class="mb-3">  
                <label for="image" class="form-label">Content</label>  
                <input type="text" name="image" class="form-control" id="image" placeholder="image">  
            </div>            <button  type="submit" class="btn btn-primary">Создать пост</button>  
        </form>    </div>@endsection
```

Создадим route под patch

```
Route::patch('/posts/{post}', [PostController::class, 'update'] )->name('post.update');
```

создаём action в контроллере

```
public function update(Post $post)  
{  
    $data = request()->validate([  
        'title' => 'string',  
        'content' => 'string',  
        'image' => 'string',  
    ]);  получаем наши данные
  
    $post->update($data);  отправляются данные на изменение
    return redirect()->route('post.show', $post->id); 
    вывод измененных данны 
}
```

<strong>В БД также обновлен пост</strong>

Но нет ссылки, перейдём в show и создадим 

```
@extends('layouts.main')  
@section('content')  
            <div>{{$post->id}}. {{$post->title}}</div>  
            <div>{{$post->content}}</div>  
            <div>                <a class="btn btn-primary mt-3 mb-3" href="{{route('post.edit', $post->id)}}">Редактировать</a> - созданная ссылка 
            </div>            <div><a class="btn btn-primary" href="{{route('post.index')}}">Назад</a></div>  
@endsection
```

Но при нажатии на редактирование, поля пустые

Поэтому нужно прописать значение <strong>value</strong>

```
@extends('layouts.main')  
@section('content')  
    <div>  
        <form action="{{route('post.update', $post->id)}}" method="post">  
            @csrf  
            @method('patch')  
            <div class="mb-3">  
                <label for="title" class="form-label">Title</label>  
                <input type="text" name="title" class="form-control" id="title" placeholder="Title" value="{{$post->title}}">  
            </div>            <div class="mb-3">  
                <label for="content" class="form-label">Content</label>  
                <textarea type="text" name="content" class="form-control" id="content" placeholder="Content" >{{$post->content}}</textarea>  
            </div>            <div class="mb-3">  
                <label for="image" class="form-label">Content</label>  
                <input type="text" name="image" class="form-control" id="image" placeholder="image" value="{{$post->image}}">  
            </div>            <button  type="submit" class="btn btn-primary">Редактировать пост</button>  
        </form>    </div>@endsection
```

Но при удалении какой-либо ячейки, и нажатии на обновление поста, валидатор не пропускает и удалённое значение восстанавливается. Это своего рода защита от базы, чтобы не было ошибок

Теперь реализация удаления поста - DELETE

Создаём роут

```
Route::delete('/posts/{post}', [PostController::class, 'destroy'] )->name('post.destroy');
```

Прописываем action в контроллере

```
public function destroy(Post $post)  
{  
    $post->delete();  
    return redirect()->route('post.index');  
}
```

В файле show добавим удаление

```
@extends('layouts.main')  
@section('content')  
            <div>{{$post->id}}. {{$post->title}}</div>  
            <div>{{$post->content}}</div>  
            <div>                <a class="btn btn-primary mt-3 mb-3" href="{{route('post.edit', $post->id)}}">Редактировать</a>  
            </div>            <div>                <form class="btn btn-primary mb-3" action="{{route('post.delete', $post->id)}}" method="post">  
                    @csrf  
                    @method('delete')  
                    <input type="submit" value="Удалить">  
                </form>            </div>            <div><a class="btn btn-primary" href="{{route('post.index')}}">Назад</a></div>  
@endsection
```
Для удаления нужно прописать форму,  c action в котором помещается route с информацией о посте

Посты удалены, в таблице при удалении появилась информация в deleted__at

<h2>Отношение один ко многим</h2>
Отношение между моделями в базе

Существуют несколько вариантов отношений моделей в базе. Так как в постах и базе могут быть категории, то между ними нужно сделать связь.

Для создания отношение, нужно создать категории

```
php artisan make:model Category -m
```

создаётся таблица категорий и модель Category

Создадим одну колонку 
```
public function up(): void  
{  
    Schema::create('categories', function (Blueprint $table) {  
        $table->id();  
        $table->string('title');  - созданная колонка
        $table->timestamps();  
    });  
}
```

У одной категории может быть много постов, также у одного поста, может быть одна категория

В таблице постов добавим значения

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
        $table->softDeletes();  
  
        $table->unsignedBigInteger('category_id')->nullable(); - не будет отрицательных значений, положительные удваиваются
        $table->index($table->index('category_id', 'post_category_idx'); - связка с категорией и именем таблицы
        $table->foreign('category_id', 'post_category_fk')->on('categories')->references('id');
        связка категорий с именем таблицы и присвоение айдишника
    });  
}
```

Выполним миграцию <strong>php artisan migrate</strong>

В БД создаётся таблица <strong>categories</strong>

Перезапишем миграции <strong>php artisan migrate:fresh
</strong>
Если возникнет ошибка в posts, но удаляем Category и таблицу миграции. Отдельно создаём миграцию постов, затем отдельно миграцию категорий

Создадим категорию в БД

categories пропишем title cats, dogs
posts - пропишем 

	cats title cats content  fhfhf.pnd 12 1 ссылка автоматически выбирает айдишник категории

перейдём в контроллер и поработаем с <strong>index</strong>

Выведем категории

```
public function index()  
{  
    $posts = Post::all();  
    $categories = Category::all();  
  
    dd($categories);  
  
}
```

в браузере появятся категории

Это даёт возможность брать посты, которые принадлежат определённой категории

Выведем нужную категорию

```
public function index()  
{  
    $posts = Post::all();  
    $categories = Category::find(1);  
    dd($categories->title);  

}
```


Выведем посты, которые принадлежат нужной категории

```
public function index()  
{  
    $category = Category::find(1);  
  
    $posts = Post::where('category_id', $category->id)->get();  
    dd($posts);  
}
```

В браузере выводится результат запроса
```
Illuminate\Database\Eloquent\Collection {#1303 ▼ // app\Http\Controllers\PostController.php:16
  #items: array:2 [▼
    0 => App\Models\Post {#1304 ▶}
    1 => App\Models\Post {#1305 ▶}
  ]
  #escapeWhenCastingToString: false
}
```

Чтобы ускорить запрос, будем работать в модели Category

```
class Category extends Model  
{  
    use HasFactory;  
  
    public function posts()  
    {  
        return $this->hasMany(Post::class, 'category_id', 'id'); 
									    =category_id в таблице 
    }  
}
```

hasMany - определяем отношение данного объекта с другим объектом (посты)

Упростим запрос в контроллере

```
public function index()  
{  
    $category = Category::find(1);  
    dd($category->posts);  
}
```

Браузер выводит тот же результат

Также сделаем с постами
Добавим запись в модель поста

```
class Post extends Model  
{  
    use HasFactory;  
    use SoftDeletes;  
    protected $table = 'posts';  
    protected  $guarded = false;  
  
    public function category()  - связь с категорией
    {  
        return $this->belongsTo(Category::class, 'category_id', 
                      ^обратная связка
                      'id');  
    }  
}
```

Пропишем вызов категорий

```
public function index()  
{  
    $category = Category::find(1);  
    $post = Post::find(3);  - смотреть айдишник в БД
    dd($post->category);  
}
```

<h2>Отношения многие ко многим</h2>

У каждого поста, может быть много тегов, как и у тега, может быть много постов
Создадим миграцию для тегов - <strong>php artisan make:model Tag -m</strong>

В миграции создадим title

```
public function up(): void  
{  
    Schema::create('tags', function (Blueprint $table) {  
        $table->id();  
        $table->string('title');  
        $table->timestamps();  
    });  
}
```

Чтобы связать одну таблицу с другой, создадим ещё одну промежуточную таблицу
Для связки используем метод связки через модель - <strong>php artisan make:model PostTag -m</strong>

В таблице пропишем связку

```
public function up(): void  
{  
    Schema::create('post_tags', function (Blueprint $table) {  
        $table->id();  
  
        $table->unsignedBigInteger('post_id');  
        $table->unsignedBigInteger('tag_id');  
  
        $table->index('post_id', 'post_tag_post_idx');  
        $table->index('tag_id', 'post_tag_tag_idx');  
  
        $table->foreign('post_id', 'post_tag_post_fk')->on('posts')->references('id');  
        $table->foreign('tag_id', 'post_tag_tag_fk')->on('tags')->references('id');  
  
        $table->timestamps();  
    });  
}
```

Делаем миграцию php artisan migrate
Таблица в БД - post_tags

Добавим теги (tags) , holidays, travel, pizza

Добавим в постах строку holidays title, holidays content, 1.png, 30, 1, категория dogs

Таблица post_tags

```
		id post_id tag_id
		1 1 1
		2 1 3
		3 2 2
		4 2 1
		5 3 1
		6 3 1
		7 3 1
```

Поработаем с постами через контроллер

```
public function index()  
{  
    $post = Post::find(1);  
    dd($post->tags);  
}
```

При таком запросе результат - null

Создадим связь в модели поста

<strong>belongsToMany()</strong> - двусторонняя связь постов с тегами

```
class Post extends Model  
{  
    use HasFactory;  
    use SoftDeletes;  
    protected $table = 'posts';  
    protected  $guarded = false;  
  
    public function category()  
    {  
        return $this->belongsTo(Category::class, 'category_id', 'id');  
    }  
    public function tags()  
    {  
        return $this->belongsToMany(Tag::class, 'post_tags', 'post_id', 'tag_id');  связь отношение ко многим(такой способ можно копипастить, ГЛАВНОЕ ПОДСТАВЛЯТЬ НУЖНЫЕ ЗНАЧЕНИЯ)
    }  
}
```

Пропишем для Tag в контроллере

```
public function index()  
{  
    $post = Post::find(1);  
    $tag = Tag::find(1);  
    dd($tag->posts);  
}
```

В модели Tag пропишем отношение тегов к постам

```
public function posts()  
{  
    return $this->belongsToMany(Post::class, 'post_tags', 'tag_id', 'post_id');  
}
```

<h2>CRUD через интерфейс - модифицируем интерфейс</h2>

Вернём функцию создания постов

```
public function index()  
{  
    $posts = Post::all();  
    return view('post.index', compact('posts'));  
}
```

Добавим выпадающий список с элементов бутстрапа

```
<select class="form-select form-select-sm mb-3" aria-label=".form-select-sm">  
    <option selected>Категория</option>  
    <option value="1">1</option>  
  
</select>

```


create.blade.php

```
@extends('layouts.main')  
@section('content')  
    <div>  
        <form action="{{route('post.store')}}" method="post">  
            @csrf  
            <div class="mb-3">  
                <label for="title" class="form-label">Title</label>  
                <input type="text" name="title" class="form-control" id="title" placeholder="Title">  
            </div>            <div class="mb-3">  
                <label for="content" class="form-label">Content</label>  
                <textarea type="text" name="content" class="form-control" id="content" placeholder="Content"></textarea>  
            </div>            <div class="mb-3">  
                <label for="image" class="form-label">Content</label>  
                <input type="text" name="image" class="form-control" id="image" placeholder="image">  
            </div>            <select class="form-select form-select-sm mb-3" aria-label=".form-select-sm">  
    <option selected>Категория</option>  
    <option value="1">1</option>  
  
</select>           <button  type="submit" class="btn btn-primary">Создать пост</button>  
        </form>    </div>@endsection
```

Нужные списки не нужны, теперь нужно добавить функционал, то есть связать с интерфейсом выбор категорий

В контроллере пропишем вывод в create

```
public function create()  
{  
    $categories = Category::all();  
    return view('post.create', compact('categories'));  
}
```

Добавим вывод категорий в код

```
@extends('layouts.main')  
@section('content')  
    <div>  
        <form action="{{route('post.store')}}" method="post">  
            @csrf  
            <div class="mb-3">  
                <label for="title" class="form-label">Title</label>  
                <input type="text" name="title" class="form-control" id="title" placeholder="Title">  
            </div>            <div class="mb-3">  
                <label for="content" class="form-label">Content</label>  
                <textarea type="text" name="content" class="form-control" id="content" placeholder="Content"></textarea>  
            </div>            <div class="mb-3">  
                <label for="image" class="form-label">Content</label>  
                <input type="text" name="image" class="form-control" id="image" placeholder="image">  
            </div>            <select class="form-select form-select-sm mb-3" aria-label=".form-select-sm" name="category_id">  
                @foreach($categories as $category)  - перебор категорий
                    <option value="{{$category->id}}">{{$category->title}}</option>  - вывод категорий
                @endforeach  
            </select>  
            <button  type="submit" class="btn btn-primary">Создать пост</button>  
        </form>    </div>@endsection
```

Правим store

```
public function store()  
{  
    $data = request()->validate([  
        'title' => 'string',  
        'content' => 'string',  
        'image' => 'string',  
        'category_id' => '', - добавленный нейминг
  
    ]);  
    Post::create($data);  
    return redirect()->route('post.index');  
}
```

Добавим категории в update (edit.blade.php)

```
@extends('layouts.main')  
@section('content')  
    <div>  
        <form action="{{route('post.update', $post->id)}}" method="post">  
            @csrf  
            @method('patch')  
            <div class="mb-3">  
                <label for="title" class="form-label">Title</label>  
                <input type="text" name="title" class="form-control" id="title" placeholder="Title" value="{{$post->title}}">  
            </div>            <div class="mb-3">  
                <label for="content" class="form-label">Content</label>  
                <textarea type="text" name="content" class="form-control" id="content" placeholder="Content" >{{$post->content}}</textarea>  
            </div>            <div class="mb-3">  
                <label for="image" class="form-label">Content</label>  
                <input type="text" name="image" class="form-control" id="image" placeholder="image" value="{{$post->image}}">  
            </div>  
            <select class="form-select form-select-sm mb-3" aria-label=".form-select-sm" name="category_id">  
                @foreach($categories as $category)  
                    <option value="{{$category->id}}">{{$category->title}}</option>  
                @endforeach  
            </select>  
  
            <button  type="submit" class="btn btn-primary">Редактировать пост</button>  
        </form>    </div>@endsection
```

Теперь нужно в контроллер добавить вывод категорий

```
public function edit(Post $post)  
{  
    $categories = Category::all();  
    return view('post.edit', compact('post', 'categories'));  
}


public function update(Post $post)  
{  
    $data = request()->validate([  
        'title' => 'string',  
        'content' => 'string',  
        'image' => 'string',  
        'category_id' => '',  
    ]);  
  
    $post->update($data);  
    return redirect()->route('post.show', $post->id);  
}
```

 Но есть ошибка, нужно исправить ошибку, чтобы при выборе поста с категорией, было отображение именно выбранной категории. <strong>Для этого нужно сравнить айдишник с постом, который приходит с базы</strong>

```
<select class="form-select form-select-sm mb-3" aria-label=".form-select-sm" name="category_id">  
  
    @foreach($categories as $category)  
        <option  
            {{$category->id === $post->category->id ? 'selected' : ''}}  - сравнение
            value="{{$category->id}}">{{$category->title}}</option>  
    @endforeach  
  
</select>
```

Теперь научимся добавлять теги

Добавим элемент мультивыбора списка в create.blade.php
```
<div class="form-group">  
    <label for="tags">Tags</label>  
    <select class="form-select" multiple aria-label="пример множественного выбора" id="tags">  
        <option value="1">1</option>  
    </select></div>
```

multiple - отвечает за эту функцию

в постконтроллере нужно задать прокидывание тегов

```
public function create()  
{  
    $categories = Category::all();  
    $tags = Tag::all();  - вывод всех тегов
    return view('post.create', compact('categories','tags')); 

}
tags - опрокидывание
```


Прописываем вывод тегов в списке

```
<div class="form-group mb-3">  
    <label for="tags">Tags</label>  
    <select class="form-select" multiple aria-label="пример множественного выбора" id="tags" name="tags">  
        @foreach($tags as $tag)  
        <option value="{{$tag->id}}">{{$tag->title}}</option>  
        @endforeach  
    </select>  
</div>
```

PostController добавляем в store теги

```
public function store()  
{  
    $data = request()->validate([  
        'title' => 'string',  
        'content' => 'string',  
        'image' => 'string',  
        'category_id' => '',  
        'tags' => '',  
  
    ]);  
    Post::create($data);  
    return redirect()->route('post.index');  
}
```

Результат вывода

```
array:5 [▼ // app\Http\Controllers\PostController.php:37
  "title" => "fsefsefse"
  "content" => "Cerfsr"
  "image" => "image.png"
  "category_id" => "2"
  "tags" => "3" - неверный вывод
]
```

Для исправления этой ошибки, добавим [ ] 

```
<select class="form-select" multiple aria-label="пример множественного выбора" id="tags" name="tags[]"> - даём понять, что нам нужен массив
```

Теперь при выборе нужных тегов, будет выводится массив с указанными айдишниками и их наименования

```
array:5 [▼ // app\Http\Controllers\PostController.php:37
  "title" => "efwe"
  "content" => "werwe"
  "image" => "image4.jpeg"
  "category_id" => "1"
  "tags" => array:2 [▼
    0 => "2"
    1 => "3"
  ]
]
```

Но так как, массив создаст проблемы в создании постов, нужно с помощью unset очищать html

```
public function store()  
{  
    $data = request()->validate([  
        'title' => 'string',  
        'content' => 'string',  
        'image' => 'string',  
        'category_id' => '',  
        'tags' => '',  
  
    ]);  
    $tags = $data['data'];  
    unset($data['tags']);  
    Post::create($data);  
    return redirect()->route('post.index');  
}

Эта запись рахделяет посты и теги, но теперь нужно сделать привязку, чтобы теги выводились вместе с постом
```

Для начала разрешим PоstTag добавление и удаление записей

```
class PostTag extends Model  
{  
    use HasFactory;  
  
    protected $guarded = false;  -  добавленная строка
}
```

Теперь можно напрямую обращаться к модели <strong>PоstTag</strong>


```
public function store()  
{  
    $data = request()->validate([  
        'title' => 'string',  
        'content' => 'string',  
        'image' => 'string',  
        'category_id' => '',  
        'tags' => '',  
  
    ]);  
    $tags = $data['tags'];  
    unset($data['tags']);  
  
    $post = Post::create($data);  
    foreach ($tags as $tag) {  
        PostTag::firstOrCreate([  
            'tag_id'=>$tag,  
            'post_id'=> $post->id,  
        ]);  
    }  
  
    return redirect()->route('post.index');  
}
!!!При таком методе, может происходить дублирвоание в базе, в дальнейшем, эта ошибка будет исправлена
```

Переделаем в более профессиональный способ c помощью метода <strong>attach</strong>

```
public function store()  
{  
    $data = request()->validate([  
        'title' => 'string',  
        'content' => 'string',  
        'image' => 'string',  
        'category_id' => '',  
        'tags' => '',  
  
    ]);  
    $tags = $data['tags'];  
    unset($data['tags']);  
  
    $post = Post::create($data);  
  
    $post->tags()->attach($tags);  
  
    return redirect()->route('post.index');  
}
```

Перенесём теги в edit в PostController

```
public function edit(Post $post)  
{  
    $categories = Category::all();  
    $tags = Tag::all();  
    return view('post.edit', compact('post', 'categories', 'tags'));  
}
```

В файле edit.blade.php внесём теги

```
<div class="form-group mb-3">  
    <label for="tags">Tags</label>  
    <select class="form-select" multiple aria-label="multiple" id="tags" name="tags[]">  
        @foreach($tags as $tag)  
            <option  
                @foreach($post->tags as $postTag)  
                    {{$tag->id === $postTag->id ? 'selected' : ''}}  
                @endforeach  
                value="{{$tag->id}}">{{$tag->title}}</option>  
        @endforeach  
    </select>  
</div>
```

Для доступа в update сделаем изменения для тегов

```
public function update(Post $post)  
{  
    $data = request()->validate([  
        'title' => 'string',  
        'content' => 'string',  
        'image' => 'string',  
        'category_id' => '',  
        'tags' => '',  
    ]);  
    $tags = $data['tags'];  
    unset($data['tags']);  
  
    $post->update($data);  
    $post->tags()->sync($tags); - всё удаляет до момента привязки и обновляет данные  
  
  
    return redirect()->route('post.show', $post->id);  
}
```