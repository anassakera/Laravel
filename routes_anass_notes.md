

مني تبغي تصاوب مشروع جديد بقاعدة بيانات محددة
قم بتنفيذ الأمر التالي:

```PowerShell
ANASS(config)$ composer global update
```

```PowerShell
ANASS(config)$ composer create-project laravel/laravel:^11.0 ofppt

OR

ANASS(config)$ laravel new ofppt --breeze
```

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=ofpptdb
DB_USERNAME=root
DB_PASSWORD=
```

```PowerShell
ANASS(config)$ php artisan migrate
```




باش أنك تقدر تشوف التطبيق ديالك ضروري أنك تكون مخدم
```PowerShell
ANASS(config)$ php artisan serve
```

# Routes :
ماهما إلا طريقة كانعلمو بيها لارافيل كيفاش يدير يجاوب 
مجموعة ديال العناوين
مثلا في حالة كتبت في المتصفح 
```PowerShell
http://127.0.0.1:8080/Hello
```
مغاديش يعطيني حتي شي Response ببساطة حينت مكيعرفش هد Route

الطريقة باش كانضيف Route جديد مع الوظائف ديالو هي
أنني كانمشي لعند المسار

```PowerShell
ofppt/routes/web.php
```
أونضيف المسار ديال بهد الشكل
```php
Route::get('/anass', function () {
    return 'welcome ANASS CHAKLOUL!';
});
```

# Views:
في حالة بغيت نرجع صفحة صاوبة 

الحالة الأولى:
خاصني نصوبها في المسار التالي :
```PowerShell
ofppt\resources\views\anass.blade.php
```
الحالة الثانية:
أو أنها في حالة كانت داخل مجلد
```PowerShell
ofppt\resources\views\pages\anass.blade.php
```

أو نضيفها بهذ الشكل بإستخدام view
```php
Route::get('/chakloul', function () {
    #الحالة الأولى
    return view('anass');
    #الحالة الثانية
    return view('pages/anass');
});
```

# Controller:

+ الكونترولر هو للي كايستقبل http request
بالإضافة إلى معالجتها Prossessing ديالها
حينت كايتواصل مع الViews أو ال Modles


كانلقاو الكونترولرز ديالنا في المسار التالي
```PowerShell
ofppt\app\Http\Controllers
```
بغيتي تعرف كيفاش كايخدم أي أمر تبعو بـ --help
```PowerShell
ANASS(config)$ php artisan make:controller --help
```
في حالة بغينا نصوبو كونترولر جديد كايخصنا نديرو الأمر التالي
```PowerShell
ANASS(config)$ php artisan make:controller AnassController
```
+ كانديرو فيه Logic ديال التطبيق ديالنا

<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
class AnassController extends Controller
{
    public function index(){
        $var1 = 'salam anass';
        return view('pages/anass' , compact('var1'));
    }
}

إلى صاوبنا الكونترولر ديالنا بقياناش غادين نحتاجو ال 
الكلوجر في الملف ديال web.php
نحذفوه بهد الشكل

Route::get('/chakloul', );

في حالة صاوبت كونترولر جديد خاصني نستوردرو في ملف web.php

use App\Http\Controllers;

أو نقول ليه أش من كونترولر غادي يستعمل أو أش من ميثود غادي 
يستعمل في هد الكونترولر بهد الشكل

Route::get('/chakloul', [AnassController::class, 'index']);






