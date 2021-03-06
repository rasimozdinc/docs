# Yardımcı (Helper) Fonksiyonları

- [Arrayler (Diziler)](#arrays)
- [Dosya Yolları](#paths)
- [Yazı İşlemleri](#strings)
- [URL İşlemleri](#urls)
- [Diğer](#miscellaneous)

<a name="arrays"></a>
## Arrayler (Diziler)

### array_add

`array_add` fonksiyonu, verilen anahtar / değer çiftini, eğer daha önce eklenmemişse array'e eklemeye yarar.

	$array = array('foo' => 'bar');

	$array = array_add($array, 'key', 'value');

### array_divide

`array_divide` fonksiyonu, birincisi anahtarlar, ikincisi değerler olacak şekilde iki farklı array döndürür.

	$array = array('foo' => 'bar');

	list($keys, $values) = array_divide($array);

### array_dot

`array_dot` fonksiyonu, çok boyutlu bir array'i derinlikleri 'nokta (dot)' notasyonunu sağlayacak şekilde 1 boyutlu array'e çevirir.

	$array = array('foo' => array('bar' => 'baz'));

	$array = array_dot($array);

	// array('foo.bar' => 'baz');

### array_except

`array_except` fonksiyonu, verilen anahtar / değer çiftini array'den siler.

	$array = array_except($array, array('keys', 'to', 'remove'));

### array_fetch

`array_fetch` metodu seçilen bir iç elemanı içeren düz bir dizi döndürür.

	$array = array(
		array('developer' => array('name' => 'Taylor')),
		array('developer' => array('name' => 'Dayle')),
	);

	$array = array_fetch($array, 'developer.name');

	// array('Taylor', 'Dayle');

### array_first

`array_first` fonksiyonu, verilen doğruluk testine uyan ilk array elemanını döndürür.

	$array = array(100, 200, 300);

	$value = array_first($array, function($key, $value)
	{
		return $value >= 150;
	});

Ayrıca varsayılan bir değer, üçüncü eleman olarak verilebilir.

	$value = array_first($array, $callback, $default);

### array_last

The `array_last` method returns the last element of an array passing a given truth test.

	$array = array(350, 400, 500, 300, 200, 100);

	$value = array_last($array, function($key, $value)
	{
		return $value > 350;
	});

	// 500

A default value may also be passed as the third parameter:

	$value = array_last($array, $callback, $default);

### array_flatten

`array_flatten` metodu çok boyutlu bir diziyi tek düzey halinde düzleştirir.

	$array = array('name' => 'Joe', 'languages' => array('PHP', 'Ruby'));

	$array = array_flatten($array);

	// array('Joe', 'PHP', 'Ruby');

### array_forget

`array_forget` metodu "dot" notasyonu kullanarak, derin bir iç içe diziden belirli bir anahtar / değer çiftini kaldıracaktır.

	$array = array('names' => array('joe' => array('programmer')));

	$array = array_forget($array, 'names.joe');

### array_get

`array_get` metodu nokta notasyonu kullanarak derin bir iç içe diziden belirli bir değeri döndürür.

	$array = array('names' => array('joe' => array('programmer')));

	$value = array_get($array, 'names.joe');

> **Note:** Want something like `array_get` but for objects instead? Use `object_get`.

### array_only

`array_only` fonksiyonu, array'den sadece verilen anahtar / değer çiftlerini döndürür.

	$array = array('name' => 'Joe', 'age' => 27, 'votes' => 1);

	$array = array_only($array, array('name', 'votes'));

### array_pluck

`array_pluck` metodu verilen bir anahtar / değer çiftleri listesini diziden koparacaktır.

	$array = array(array('name' => 'Taylor'), array('name' => 'Dayle'));

	$array = array_pluck($array, 'name');

	// array('Taylor', 'Dayle');

### array_pull

`array_pull` metodu diziden belirli bir anahtar / değer çifti döndürecek, aynı zamanda bu çifti diziden çıkartacaktır.

	$array = array('name' => 'Taylor', 'age' => 27);

	$name = array_pull($array, 'name');

### array_set

`array_set` metodu nokta notasyonu kullanarak, derin bir iç içe dizide bir değer ayarlar.

	$array = array('names' => array('programmer' => 'Joe'));

	array_set($array, 'names.editor', 'Taylor');

### array_sort

The `array_sort` method sorts the array by the results of the given Closure.

	$array = array(
		array('name' => 'Jill'),
		array('name' => 'Barry'),
	);

	$array = array_values(array_sort($array, function($value)
	{
		return $value['name'];
	}));

### array_where

Filter the array using the given Closure.

	$array = array(100, '200', 300, '400', 500);

	$array = array_where($array, function($key, $value)
	{
		return is_string($value);
	});

	// Array ( [1] => 200 [3] => 400 )

### head

Dizideki ilk elemanı döndürür. PHP 5.3.x'deki metod zincirleme işine yarar.

	$first = head($this->returnsArray('foo'));

### last

Dizideki son elemanı döndürür. Metod zincirlemesinde işe yarar.

	$last = last($this->returnsArray('foo'));

<a name="paths"></a>
## Dosya Yolları

### app_path

`app` dizininin tam dosya yolunu getirir.

	$path = app_path();

### base_path

Uygulamanın ana dizininin tam dosya yolunu getirir.

### public_path

`public` dizininin tam dosya yolunu getirir.

### storage_path

`app/storage` dizininin tam dosya yolunu getirir.

<a name="strings"></a>
## Yazı İşlemleri

### camel_case

Yazıyı `camelCase` olacak şekilde düzenler.

	$camel = camsel_case('foo_bar');

	// fooBar

### class_basename

Girilen class'ın namespace'ler olmadan sadece adını dondürür.

	$class = class_basename('Foo\Bar\Baz');

	// Baz

### e

Girilen yazıya UTF-8 desteğiyle `htmlentities` fonksiyonunu uygular.

	$entities = e('<html>foo</html>');

### ends_with

Girilen yazının verilen değerle bitip bitmediğine karar verir.

	$value = ends_with('This is my name', 'name');

### snake_case

Yazıyı `snake_case` olacak şekilde düzenler.

	$snake = snake_case('fooBar');

	// foo_bar

### str_limit

Limit the number of characters in a string.

	str_limit($value, $limit = 100, $end = '...')

Example:

	$value = str_limit('The PHP framework for web artisans.', 7);

	// The PHP...

### starts_with

Girilen yazının verilen değerle başlayıp başlamadığına karar verir.

	$value = starts_with('This is my name', 'This');

### str_contains

Girilen yazının içinde verilen değerin olup olmadığına karar verir.

	$value = str_contains('This is my name', 'my');

### str_finish

Girilen yazının sonuna verilen değeri ekler. Verilen değerden oluşan ekstraları yok eder.

	$string = str_finish('this/string', '/');

	// this/string/

### str_is

Girilen yazıyla verilen değerin eşleşip eşleşmediğine karar verir. Yıldız işareti (*) genel arama karakteri olarak kullanılabilir.

	$value = str_is('foo*', 'foobar');

### str_plural

Girilen kelimeyi çoğul hale getirir (Sadece ingilizce için geçerli).

	$plural = str_plural('car');

### str_random

Girilen değer kadar uzunlukta rastgele karakterlerden oluşan bir yazı üretir.

	$string = str_random(40);

### str_singular

Girilen kelimeyi tekil hale getirir (Sadece ingilizce için geçerli).

	$singular = str_singular('cars');

### studly_case

Girilen yazıyı `StudlyCase` olacak şekilde düzenler.

	$value = studly_case('foo_bar');

	// FooBar

### trans

Girilen dil satırını çevirir. `Lang::get` fonksiyonunun kısayolu.

	$value = trans('validation.required'):

### trans_choice

Girilen dil satırını çekimli çevirir. `Lang::choice` fonksiyonunun kısayolu.

	$value = trans_choice('foo.bar', $count);

<a name="urls"></a>
## URL İşlemleri

### action

Belirli bir denetçi eylemi için bir URL üretir.

	$url = action('HomeController@getIndex', $params);

### route

Verilen isimli rota için URL oluştur.

	$url = route('routeName', $params);

### asset

Bir varlık için bir URL üretir.

	$url = asset('img/photo.jpg');

### link_to

Girilen URL'e gerekli HTML linkini oluşturur.

	echo link_to('foo/bar', $title, $attributes = array(), $secure = null);

### link_to_asset

Verilen varlık için bir HTML bağlantısı üretir.

	echo link_to_asset('foo/bar.zip', $title, $attributes = array(), $secure = null);

### link_to_route

Girilen rota için gerekli HTML linkini oluşturur.

	echo link_to_route('route.name', $title, $parameters = array(), $attributes = array());

### link_to_action

Verilen bir denetçi eylemi için bir HTML linki oluşturur.

	echo link_to_action('HomeController@getIndex', $title, $parameters = array(), $attributes = array());

### secure_asset

Girilen eleman için gerekli HTML linkini HTTPS kullanarak oluşturur.

	echo secure_asset('foo/bar.zip', $title, $attributes = array());

### secure_url

Girilen URL'e gerekli HTML linkini HTTPS kullanarak oluşturur.

	echo secure_url('foo/bar', $parameters = array());

### url

Verilen bir dosya yolu için tam kalifiye bir URL üretir.

	echo url('foo/bar', $parameters = array(), $secure = null);

<a name="miscellaneous"></a>
## Diğer

### csrf_token

CSRF token'inin güncel değerini döndürür.

	$token = csrf_token();

### dd

Girilen veriyi ekrana basar ve uygulamayı durdurur.

	dd($value);

### value

Eğer girilen değer anonim bir fonksiyonsa, değer olarak anonim fonksiyonun döndürdüğü değer döndürür. Eğer değilse direk değeri döndürür.

	$value = value(function() { return 'bar'; });

### with

Girilen objeyi döndürür. PHP 5.3.x kullanımında metod zincirleme işlemi için çok yararlı.

	$value = with(new Foo)->doWork();