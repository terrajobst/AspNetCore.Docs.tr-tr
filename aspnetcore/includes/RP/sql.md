# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a>ASP.NET core'da SQLite ile çalışma Razor sayfaları uygulama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

`MovieContext` Nesne veritabanına bağlanma ve eşleme görevi işleme `Movie` veritabanı kayıtlarını nesneleri. Veritabanı bağlamı kayıtlı [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında `ConfigureServices` yönteminde *Startup.cs* dosyası:

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

Kullanma hakkında daha fazla bilgi için `DbContext` DI ile bkz [kullanarak DbContext DI ile](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).

## <a name="sqlite"></a>SQLite

[SQLite](https://www.sqlite.org/) Web sitesi durumları:

> SQLite müstakil, yüksek güvenilirlik, katıştırılmış, tam özellikli, ortak etki alanı, bir SQL veritabanı altyapısı ' dir. SQLite, dünyanın en çok kullanılan veritabanı altyapısıdır.

Birçok üçüncü taraf araçları indirebileceğiniz yönetmek ve bir SQLite veritabanı görüntülemek için. Aşağıdaki görüntüde dandır [DB tarayıcı sqlite](http://sqlitebrowser.org/). Bir sık kullanılan SQLite aracınız varsa, bu konuda şeyleri üzerinde bir yorum yazın.

![SQLite gösteren film db için DB tarayıcı](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Veritabanının çekirdeğini oluşturma

Adlı yeni bir sınıf oluşturun `SeedData` içinde *modelleri* klasör. Oluşturulan kodu aşağıdakiyle değiştirin:

[!code-csharp[](code/Models/SeedData.cs)]

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

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a>Uygulamayı test etme

Bu nedenle (seed yöntemi çalıştırılır) veritabanındaki tüm kayıtları silin. Veritabanının çekirdeğini oluşturma için app durdurup yeniden açın.

Uygulama, çekirdeği oluşturulmuş veri gösterir.
