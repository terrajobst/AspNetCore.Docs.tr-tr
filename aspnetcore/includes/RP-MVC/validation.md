
## <a name="add-validation-rules-to-the-movie-model"></a>Film modeli için doğrulama kuralları ekleme

Açık *Movie.cs* dosya. DataAnnotations ad alanı, sınıf ya da özellik bildirimli olarak uygulanan yerleşik doğrulama öznitelikleri kümesi sağlar. DataAnnotations gibi biçimlendirme öznitelikleri de içeren `DataType` biçimlendirmesinde yardımcı olabilecek ve tüm doğrulama sağlaması gerekmez.

Güncelleştirme `Movie` yerleşik yararlanmak için sınıf `Required`, `StringLength`, `RegularExpression`, ve `Range` doğrulama öznitelikleri.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie22/Models/MovieDateRatingDA.cs?name=snippet1)]

Doğrulama özniteliklerinin uygulandığı model özellikleri uygulamak istediğiniz davranışı belirtin:

* `Required` Ve `MinimumLength` öznitelikleri belirtmek için bir özellik değeri; olmalıdır, ancak hiçbir şey bir kullanıcı bu doğrulamayı gerçekleştirmek için boşluk girişini engeller.
* `RegularExpression` Özniteliği hangi karakter olabilir sınırlamak için kullanılan giriş. Yukarıdaki kod, "Tarzı":

  * Yalnızca harf kullanmanız gerekir.
  * İlk harfi büyük olması gerekir. Boşluk, rakam ve özel karakterlere izin verilmez.

* `RegularExpression` "Değerlendirme":

  * İlk karakter bir büyük harf olmasını gerektirir.
  * Özel karakterler ve sonraki boşluklar numaraları sağlar. "PG-13'ü" bir derecelendirme için geçerlidir, ancak "Tarzı" için başarısız olur.

* `Range` Öznitelik değerine belirtilen bir aralıktaki kısıtlar.
* `StringLength` Özniteliği bir dize özelliğini en fazla uzunluğu ve isteğe bağlı olarak, minimum uzunluk ayarlamanızı sağlar.
* Değer türleri (gibi `decimal`, `int`, `float`, `DateTime`) kendiliğinden gereklidir ve gerekmeyen `[Required]` özniteliği.

Doğrulama kuralları otomatik olarak ASP.NET Core tarafından zorlanan sahip uygulamanızı daha sağlam hale getirmeye yardımcı olur. Ayrıca, bir şey doğrulamak ve yanlışlıkla veritabanına bozuk veri unutursanız olamaz sağlar.
