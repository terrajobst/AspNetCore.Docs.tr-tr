---
ms.openlocfilehash: 088f347cee73e745882455e6882d24a1e949f095
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265068"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin

<!-- This include not used by windows version -->

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici için yeni bir alan ekleyeceksiniz `Movies` tablo. Biz veritabanını bırakıp şema değiştirdiğimizde yeni bir tane oluşturun (yeni bir alan ekleyin). Biz korumak için herhangi bir üretim veri olmadığında bu iş akışı geliştirme erken iyi çalışır.

Şema değiştirmeniz gerektiğinde korumak için ihtiyacınız olan verilere sahip olduğunuz ve uygulamanız dağıtıldıktan sonra veritabanı bırakılamıyor. Entity Framework [Code First Migrations](/ef/core/get-started/aspnetcore/new-db) şemanızı güncelleştirin ve veri kaybı olmadan veritabanını geçirme olanak tanır. Geçişleri bir özelliktir popüler birçok geçiş şema işlemleri SQL Server, ancak SQLite kullanarak desteklemediğinde bu nedenle yalnızca çok basit geçişleri mümkündür. Bkz: [SQLite sınırlamaları](/ef/core/providers/sqlite/limitations) daha fazla bilgi için.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeli derecelendirme özellik ekleme

Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

Yeni bir alan eklediğiniz çünkü `Movie` sınıfı da gereken bağlama beyaz liste bu yeni özellik dahil edilecek şekilde güncelleştirin. İçinde *MoviesController.cs*, güncelleştirme `[Bind]` özniteliği hem de `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Ayrıca görüntülemek için görüntüleme şablonları güncelleştirilecek oluşturabilecek ve yeni `Rating` Tarayıcı Görünümü özelliği.

Düzen */Views/Movies/Index.cshtml* dosya ve ekleme bir `Rating` alan:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Güncelleştirme */Views/Movies/Create.cshtml* ile bir `Rating` alan.

Uygulama, yeni alan eklemek için bir veritabanı güncelleştiriyoruz kadar çalışmaz. Şimdi çalıştırırsanız, aşağıdaki elde edecekleriniz `SqliteException`:

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

Güncelleştirilmiş film modeli sınıfı var olan veritabanının film tablonun şeması farklı olduğu için bu hatayı görüyorsunuz. (Yok hiçbir `Rating` veritabanı tablosundaki sütun.)

Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Veritabanını bırakın ve yeni model sınıfı şemasını temel alan veritabanı otomatik olarak yeniden oluşturma Entity Framework sahip. Bu yaklaşımda, veritabanında var olan veri kaybı — üretim veritabanıyla yapamadığı şekilde! Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır.

2. El ile model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin. Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır. Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.

3. Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.

Bu öğreticide, biz bırakın ve şema değiştiğinde veritabanını yeniden oluşturmanız. Bir terminal db bırakmak için aşağıdaki komutu çalıştırın:

`dotnet ef database drop`

Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını. Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Ekleme `Rating` alanı `Edit`, `Details`, ve `Delete` görünümü.

Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan. şablonları.
