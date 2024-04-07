في الملف auth.php الموجود في المسار التالي:

```
ofppt1\config\auth.php
```

الإشكال في حالة أنك تريد تسجيل الدخول كمسؤول أو مستخدم يمكنك ذلك من خلال
إنشاء خانة إسمها Role و تحدد الصلاحيات ولكنهذه الطريقة فاشلة في حالة كان
لدي مايفوق 300 مستخدم السيرفر سيتأخر في الرد لأنه يبحث عن الإستعلام في قائمة
طويلة لذلك سوف أقوم بإنشاء جداول بال guards :

```php
    'guards' => [
        
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],

        //في حالة أنني قلت super_admin غادي يمشى يشوف جدول super_admins
        'super_admin' => [
            'driver' => 'session',
            'provider' => 'super_admins',
        ],

        //في حالة أنني قلت admin غادي يمشى يشوف جدول admins
        'admin' => [
            'driver' => 'session',
            'provider' => 'admins',
        ],
        
        //في حالة أنني قلت user غادي يمشى يشوف جدول users
        'user' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
    ],
```

# OTP project
- [x] 1- Install Laravel & Laravel Breeze OTP Project
- [x] 2- Add Two Columns In Table Users OTP Project
- [x] 3- Make Function Generate Code OTP Project
- [x] 4- Insert Code In Database OTP Project
- [ ] 5- Make Controller & Middleware OTP Project
- [ ] 6- Verify Code OTP Project
- [ ] 7- Configuration Account In Mailtrap OTP Project
- [ ] 8- Send Notification Code In Mail OTP Project
- [ ] 9- send notification code in mobil
>Link of Course: (https://www.youtube.com/playlist?list=PLftLUHfDSiZ72wM7RhZmPpJdHJ89FjkQB)
## create project Laravel ✅
```
composer create-project laravel/laravel OTP 
```

## install laravel breeze & DebugBar ✅
```
composer require laravel/breeze --dev
php artisan breeze:install blade
```
install DebugBar
> composer require barryvdh/laravel-debugbar --dev
## create database ✅
```SQL
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=OTP
DB_USERNAME=root
DB_PASSWORD=
```
```
php artisan migrate
```

##  add tow columns in table users ✅
in this path 
```
ofppt1\database\migrations\0001_01_01_000000_create_users_table.php
```

add
```php
$table->string('code')->nullable();
$table->dateTime('expire_at')->nullable();
```

in this path 
```
ofppt1\app\Models\User.php
```

add
```php
protected $fillable = [
    'name',
    'email',
    'password',
    'type',
    'expire_at'
];
```

clear my database
```
php artisan mi:f
```

## make function generate code ✅
in this path 
```
ofppt1\app\Models\User.php
```
add
```php
public function generateCode(){
    // Disable automatic timestamp updating for this operation
    $this->timestamps = false;

    // Generate a random code between 1000 and 9999
    $this->code = rand(1000,9999);

    // Set the expiration time for the code to 15 minutes from the current time
    $this->expire_at = now()->addMinutes(15); 

    // Save the changes to the model
    $this->save();
}
```
in this path 
```
ofppt1\app\Http\Requests\Auth\LoginRequest.php
```
add
```php
58  $user = User::where('email', $this->input('email'))->first();
59  $user->generateCode();
60  RateLimiter::clear($this->throttleKey());
61  }
```
clear my database
```
php artisan mi:f
```

## make controller TowFactorController 👇
create TowFactorController 
```
php artisan make:controller TowFactorController -r
```
create TowFactor
```
php artisan make:middleware TowFactor
```
in this path 
```
ofppt1\bootstrap\app.php
```
add
```php
14 ->withMiddleware(function (Middleware $middleware) {
15    $middleware->alias([
16        'tow_factor' => \App\Http\Middleware\TowFactor::class,
17    ]);
18 }
19 )
```
in this path 
```
ofppt1\routes\web.php
```
add
```php
10 Route::get('/dashboard', function () {
11    return view('dashboard');
12    // قبل مايوصل شي واحد ل Dashboar فراه حتي يدوز على tow_factor عاد باش يوصل ل Dashboard
13 })->middleware(['auth', 'verified', 'tow_factor'])->name('dashboard');
```


## create account in mailtrap ⛔

## php artisan make:notification TowFactorCode ⛔







