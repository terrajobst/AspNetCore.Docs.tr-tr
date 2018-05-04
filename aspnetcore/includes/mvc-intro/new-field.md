# <a name="adding-a-new-field"></a>Yeni bir alan ekleme

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici için yeni bir alan ekler `Movies` tablo. Biz veritabanını bırakıp biz şema değiştirdiğinizde, yeni bir tane oluşturun (yeni bir alan ekleyin). Biz perserve için herhangi bir üretim veri bulunmadığında bu iş akışı da erken geliştirme çalışır.

Şema değiştirmeniz gerektiğinde, uygulamanın dağıtıldığı ve perserve için gereksinim duyduğunuz verilerine sahip sonra DB bırakılamıyor. Entity Framework [Code First Migrations](/ef/core/get-started/aspnetcore/new-db) şemanızı güncelleştirmek ve verileri kaybetmeden veritabanını geçirme sağlar. Geçişler bir özelliktir popüler birçok geçiş şema işlemleri, SQL Server, ancak SQLlite kullanarak desteklemediğinde şekilde yalnızca çok basitçe geçişler mümkündür. Bkz: [SQLite sınırlamalar](/ef/core/providers/sqlite/limitations) daha fazla bilgi için.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeline derecelendirme özellik ekleme

Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

Yeni bir alan eklediğiniz çünkü `Movie` sınıfı, siz de bu yeni özelliği eklenecek şekilde bağlama beyaz liste güncellemeniz gerekir. İçinde *MoviesController.cs*, güncelleştirme `[Bind]` özniteliği her ikisi için de `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Aynı zamanda görüntülemek için görünümü şablonlarını güncelleştirmek için oluşturmak ve yeni düzenlemek `Rating` tarayıcı görünümünde özelliği.

Düzen */Views/Movies/Index.cshtml* dosya ve ekleme bir `Rating` alan:

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Güncelleştirme */Views/Movies/Create.cshtml* ile bir `Rating` alan.

Uygulama, yeni alanı içerecek şekilde DB güncelleştiriyoruz kadar çalışmaz. Şimdi çalıştırırsanız, aşağıdaki elde edersiniz `SqliteException`:

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

Güncelleştirilmiş film model sınıfı var olan veritabanının film tablonun şeması farklı olduğundan bu hata görüyorsunuz. (Var. hiçbir `Rating` veritabanı tablosundaki sütun.)

Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Veritabanını bırakın ve yeni model sınıfı şemasını temel alan veritabanı otomatik olarak yeniden oluşturma Entity Framework sahip. Bu yaklaşımda, veritabanında var olan veri kaybı — bir üretim veritabanıyla yapamadığı şekilde! Otomatik olarak test verileri olan bir veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir şekilde görülür.

2. El ile modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin. Bu yaklaşımın avantajı, verilerinizi tutmak ' dir. Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.

3. Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.

Bu öğreticide, biz bırakın ve şema değiştiğinde veritabanını yeniden oluşturmanız. Terminal durumundan db bırakmak için aşağıdaki komutu çalıştırın:

`dotnet ef database drop`

Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlayan sınıf. Bir örnek değişiklik aşağıda gösterilen, ancak her biri için bu değişikliği yapmak istersiniz `new Movie`.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Ekleme `Rating` alanı `Edit`, `Details`, ve `Delete` görünümü.

Uygulamayı çalıştırın ve oluşturma/düzenleme/görüntüleme filmler ile yapabilecekleriniz doğrulayın bir `Rating` alan. Şablonlar.
