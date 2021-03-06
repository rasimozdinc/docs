# Temel Veritabanı Kullanımı

- [Yapılandırma](#configuration)
- [Oku / Yaz Bağlantıları](#read-write-connections)
- [Sorguları Çalıştırma](#running-queries)
- [Veritabanı İşlemleri](#database-transactions)
- [Bağlantılara Erişme](#accessing-connections)
- [Sorgu Günlükleme](#query-logging)

<a name="configuration"></a>
## Yapılandırma

Laravel, veritabanı bağlantısını ve sorguları çalıştırmayı fazlasıyla kolay kılar. Veritabanı yapılandırma ayarları `app/config/database.php` dosyasında bulunmaktadır. Bu dosyada hem tüm veritabanı bağlantılarını tanımlayabilir, hem de hangi bağlantının varsayılan olarak kullanılacağını seçebilirsiniz. Bu dosyada, desteklenen veritabanı sistemlerinin tümü için örnekler verilmiştir.

Laravel tarafından desteklenen veritabanı sistemleri: MySQL, Postgres, SQLite, ve SQL Server.

<a name="read-write-connections"></a>
## Oku / Yaz Bağlantıları

Bazen SELECT sorguları için bir bağlantı ve diğer sorgular için başka bir bağlantı kullanmak isteyebilirsiniz. Laravel bunu inanılmaz bir şekilde kolaylaştırır ve düz sorgu, sorgu oluşturucu (query builder) veya Eloquent ORM kullansanız bile hepsi için doğru bağlantıyı kullanır.

Oku / yaz bağlantılarının nasıl yapılandırıldığını görmek için bu örneği inceleyebilirsiniz:

	'mysql' => array(
		'read' => array(
			'host' => '192.168.1.1',
		),
		'write' => array(
			'host' => '196.168.1.2'
		),
		'driver'    => 'mysql',
		'database'  => 'database',
		'username'  => 'root',
		'password'  => '',
		'charset'   => 'utf8',
		'collation' => 'utf8_unicode_ci',
		'prefix'    => '',
	),

Yapılandırma dizisine eklenen iki anahtara dikkat edin: `read` ve `write`. İkisi de sadece `host` anahtarını barındıran bir dizi. Geri kalan tüm veritabanı seçenekleri hem `read` hem `write` için geçerli oluyor. Yani `read` ve `write` dizilerine sadece farklı olan değerleri girmeniz yeterli oluyor. Bu durumda `read` bağlantısı kullanılırken `192.168.1.1` adresine, `write` bağlantısı kullanılırken `192.168.1.2` adresine bağlanılıyor. Kullanıcı adı ve şifre gibi diğer tüm bilgiler `read` ve `write` anahtarlarının dışında kalan genel anahtarlardan alınıyor.

<a name="running-queries"></a>
## Sorguları Çalıştırma

Veritabanı bağlantılarını bir kere yapılandırdıktan sonra `DB` sınıfını kullanarak sorgularını çalıştırabilirsiniz.

#### Kayıt Çekme (Select)

	$sonuclar = DB::select('select * from uyeler where id = ?', array(1));

`select` metodu sonuçları her zaman `dizi` tipinde döndürür.

#### Yeni Kayıt Ekleme (Insert)

	DB::insert('insert into uyeler (id, isim) values (?, ?)', array(1, 'Emre'));

#### Kayıt Güncelleme (Update)

	DB::update('update uyeler set oy = 100 where isim = ?', array('Hakan'));

#### Kayıt Silme (Delete)

	DB::delete('delete from uyeler');

> **Not:** `update` ve `delete` sorguları, bu işlemlerden etkilenen satır sayısını döndürür.

#### Genel Bir Sorgu Çalıştırma

	DB::statement('drop table uyeler');

`DB::listen` metodunu kullanarak sorgu olaylarını dinleyebilirsiniz:

#### Sorgu Olaylarını Dinleme

	DB::listen(function($sql, $bindings, $time)
	{
		//
	});

<a name="database-transactions"></a>
## Veritabanı İşlemleri

Bir veritabanı işleminde, birden fazla işlemi birden gerçekleştirmek için, 'transaction' metodunu kullanabilirsiniz:

	DB::transaction(function()
	{
		DB::table('uyeler')->update(array('votes' => 1));

		DB::table('posts')->delete();
	});

> **Not:** `transaction` metoduna girilen anonim fonksiyonunda oluşan herhangi bir exception, transaction işleminin otomatik olarak geri sarılmasına (rollback edilmesine) sebep olur.

Transaction'ı elle başlatmanız gerekirse:

	DB::beginTransaction();

Transaction'ı geri sarmanız gerekirse:

	DB::rollback();

Transaction'ı tamamlamak için:

	DB::commit();

<a name="accessing-connections"></a>
## Bağlantılara Erişme

Birden fazla bağlantı kullandığınız durumlarda, bu bağlantılara `DB::connection` metodu aracılığı ile ulaşabilirsiniz.

	$uyeler = DB::connection('foo')->select(...);

Ayrıca temel PDO örneğine de ulaşabilirsiniz:

	$pdo = DB::connection()->getPdo();

Bazen veritabanına tekrar bağlanmaya ihtiyacınız olabilir.

	DB::reconnect('foo');

Veritabanıyla, PDO nesnesinin `max_connections` limiti aşıldığı için bağlantıyı koparmanız gerekirse `disconnect` metodunu kullanabilirsiniz:

	DB::disconnect('foo');

<a name="query-logging"></a>
## Sorgu Günlükleme

Varsayılan olarak, Laravel güncel istek için çalıştırılabilecek tüm sorgular için bellekte bir günlük tutar. Bununla birlikte, bu bazı durumlarda, örneğin çok sayıda satır eklerken, uygulamanın aşırı bellek kullanmasına neden olabilir. Günlüğü devre dışı bırakmak için `disableQueryLog` metodunu kullanabilirsiniz.

	DB::connection()->disableQueryLog();

Çalıştırılan sorguların listesini bir dizi olarak almak için `getQueryLog` metodunu kullanabilirsiniz:

	$queries = DB::getQueryLog();