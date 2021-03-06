# Artisan CLI

- [Giriş](#introduction)
- [Kullanım](#usage)

<a name="introduction"></a>
## Giriş

Artisan, Laravel içerisinde gelen CLI'ın (Command-line Interface) adıdır. Artisan size uygulamanızı geliştirirken birçok yardımcı komut sağlar. Artisan, güçlü Symfony Console bileşeni üzerinden geliştirilmiştir.

<a name="usage"></a>
## Kullanım

Tüm Artisan komutlarını görmek için `list` komutunu kullanabilirsiniz:

** Tüm Kullanılabilir Komutları Listelemek**

	php artisan list

Tüm komutların özel bir "yardım" ekranı vardır ve komut hakkındaki argüman sırası ile ayarlar gibi bilgilerin açıklanmasını sağlar. Bir yardım ekranını görüntülemek için komut adından önce `help` yazın:

** Bir Komut için Yardım Ekranını Görmek**

	php artisan help migrate

Bir komut kullanırken kullanılacak olan Ortam Ayarları'nı `--env` komutuyla belirleyebilirsiniz:

** Ortam Ayarlarını Belirlemek**

	php artisan migrate --env=local

Ayrıca şu an kullanmakta olduğunuz Laravel'in sürümünü de `--version` seçeneğini kullanarak Artisan üzerinden görebilirsiniz:

** Laravel'in Sürümünü Görmek**

	php artisan --version