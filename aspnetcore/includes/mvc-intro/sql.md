# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a>ASP.NET Core MVC projesinde SQLite ile çalışma

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` Nesnesini işleme veritabanına bağlanırken ve eşleme görevi `Movie` veritabanı kayıtlarını nesnelere. Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *haline* dosyası:

[!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

[SQLite](https://www.sqlite.org/) Web sitesi durumları:

> SQLite kendi içinde bulunan, yüksek güvenilirlik, katıştırılmış, tam özellikli, ortak etki alanı, bir SQL veritabanı altyapısı ' dir. SQLite dünyanın en fazla kullanılan veritabanı altyapısıdır.

Çok sayıda üçüncü taraf araçları indirebilirsiniz vardır yönetmek ve bir SQLite veritabanı görüntülemek için. Aşağıdaki görüntü arasındadır [SQLite DB tarayıcı](http://sqlitebrowser.org/). Sık kullanılan bir SQLite aracı varsa, bu konuda şeyleri üzerinde bir yorum bırakın.

![SQLite gösteren film db DB tarayıcısı](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Çekirdek veritabanı

Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör. Oluşturulan kod aşağıdakiyle değiştirin:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Olup olmadığını herhangi filmler DB'de, çekirdek Başlatıcı döndürür.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Çekirdek Başlatıcısı ekleme

Çekirdek Başlatıcısı ekleme `Main` yönteminde *Program.cs* dosyası:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

### <a name="test-the-app"></a>Uygulamayı test etme

(Seed yöntemi çalışacak şekilde) DB tüm kayıtları silin. Durdurun ve veritabanını oluşturmak için uygulamayı başlatın.
   
Uygulama hazırlığı yapmış veriler gösterir.

![MVC film uygulaması açık tarayıcı film verileri gösterme](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)
