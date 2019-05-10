# <a name="work-with-sqlite-in-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulaması içindeki SQLite ile çalışma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri. Veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:

[!code-csharp[](~/tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

[SQLite](https://www.sqlite.org/) Web sitesi durumları:

> SQLite müstakil, yüksek güvenilirlik, katıştırılmış, tam özellikli, ortak etki alanı, bir SQL veritabanı altyapısı ' dir. SQLite, dünyanın en çok kullanılan veritabanı altyapısıdır.

Birçok üçüncü taraf araçları indirebileceğiniz yönetmek ve bir SQLite veritabanı görüntülemek için. Aşağıdaki görüntüde dandır [DB tarayıcı sqlite](http://sqlitebrowser.org/). Bir sık kullanılan SQLite aracınız varsa, bu konuda şeyleri üzerinde bir yorum yazın.

![SQLite gösteren film db için DB tarayıcı](~/tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör. Oluşturulan kodu aşağıdakiyle değiştirin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Varsa tüm film DB'de, çekirdek Başlatıcı döndürür.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a>Çekirdek Başlatıcı Ekle

İçin çekirdek Başlatıcı Ekle `Main` yönteminde *Program.cs* dosyası:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

::: moniker-end

### <a name="test-the-app"></a>Uygulamayı test etme

Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin. Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.
   
Uygulama, çekirdeği oluşturulmuş veri gösterir.

![MVC film uygulaması açık tarayıcı film verileri gösterme](~/tutorials/first-mvc-app/working-with-sql/_static/m55.png)
